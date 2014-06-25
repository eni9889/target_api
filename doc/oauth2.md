## Реализация протокола OAuth2 в API Таргет@Mail.Ru

Для аутентификации и авторизации в API Таргет@Mail.Ru, начиная со второй версии,
используется протокол OAuth2. Основным его преимуществом над используемым в
первой версии способом аутентификации является то, что он по факту совмещает
аутентификацию и авторизацию, включая возможность выполнения операций одним
пользователем от имени другого (в нашем случае агентства от имени клиента и
платформы от имени прямого рекламодателя).

Именно поэтому мы реализовали две схемы (flow) из спецификации OAuth2:
[Client Credentials Grant](http://tools.ietf.org/html/draft-ietf-oauth-v2-31#section-4.4)
и [Authorization Code Grant](http://tools.ietf.org/html/draft-ietf-oauth-v2-31#section-4.1).

Client Credentials Grant используется для работы с API от своего имени напрямую.
В этом случае регистрируемый OAuth-клиент имеет тип «USER» (пользователь).

Authorization Code Grant используется для получения доступа к API от имени
других пользователей. В этом случае регистрируемый OAuth-клиент имеет тип «APP»
(приложение).

Регистрация клиентов для работы с API через OAuth2 пока осуществляется в ручном
режиме по заявке в саппорт из интерфейса сервиса.

### Client Credentials Grant

Эта схема протокола OAuth2 позволяет пользователю взаимодействовать с API напрямую.
Для пользователя создаётся клиент с параметрами `client_id` и `client_secret`,
пользуясь которыми он сам получает access-токен для аутентификации в API.

Чтобы получить access-токен, нужно послать запрос вида:

    POST /api/v2/oauth2/token.json HTTP/1.1
    Host: target.mail.ru
    Content-Type: application/x-www-form-urlencoded

    grant_type=client_credentials&client_id={client_id}&client_secret={client_secret}

В случае успешного выполнения ответ будет выглядеть следующим образом:

    HTTP/1.1 200 OK
    Content-Type: application/json; charset=UTF-8

    {
      "access_token": "{access_token}",
      "token_type": "bearer",
      "scope": "{scope}",
      "expires_in": "86400",
      "refresh_token": "{refresh_token}"
    }

Далее, полученный access-токен можно использовать при отправке запросов к API Target@Mail.Ru:

    GET /api/v2/campaigns.json HTTP/1.1
    Host: target.mail.ru
    Authorization: Bearer {access_token}

### Authorization Code Grant

Эта схема протокола OAuth2 позволяет дать возможность одним пользователям
действовать от имени других. Клиент, создаваемый для работы по этой схеме,
помимо параметров client_id и client_secret должен указать адрес redirect_uri,
на который сервис будет перенаправлять пользователей после разрешения (или
не разрешения) ими доступа от своего имени.

При это алгоритм получения доступа выглядит следующим образом:

Клиент перенаправляет пользователя на специальную страницу
https://target.mail.ru/oauth2/authorize, указав параметры `response_type`
(со значением `code`), `state` (сгенерированный на стороне клиента токен,
используется для предотвращения CSRF), свой `client_id` и список прав
доступа `scope`:

    GET /oauth2/authorize?response_type=code&client_id={client_id}&state={state}&scope={scopes} HTTP/1.1
    Host: target.mail.ru

Пользователь на странице соглашается дать доступ и сервис перенаправляет
его по адресу, заданному параметром redirect_uri при регистрации клиента,
передавая параметры `code` (специальный токен со сроком жизни в один час) и
`state` (переданный в первоначальном запросе):

    GET {redirect_uri:path}?code={code}&state={state} HTTP/1.1
    Host: <redirect_uri:host>

Получив параметр `code`, клиент может запросить `access_token` для дальнейшей
работы с API от имени пользователя. Для этого нужно послать запрос на
`/api/v2/oauth2/token.json`, передав параметры `grant_type` (со значением
`authorization_code`), `code` (полученный при обратном редиректе на
`redirect_uri` токен):

    POST /api/v2/oauth2/token.json HTTP/1.1
    Host: target.mail.ru
    Content-Type: application/x-www-form-urlencoded

    grant_type=authorization_code&code={code}&client_id={client_id}

Ответ:

    HTTP/1.1 200 OK
    Content-Type: application/json; charset=UTF-8

    {
      "access_token": "{access_token}",
      "token_type": "bearer",
      "scope": "{scope}",
      "expires_in": "86400",
      "refresh_token": "{refresh_token}"
    }

Полученный `access_token` используется для аутентификации запросов, посылаемых
в API от имени пользователя:

    GET /api/v2/campaigns.json HTTP/1.1
    Host: target.mail.ru
    Authorization: Bearer {access_token}


#### Scopes — права доступа

Права доступа указываются через запятую в параметре scope запроса доступа у
пользователя в схеме Authorization Code Grant. В зависимости от типа
пользователя запрашиваемые права доступа делятся на две группы.
Первая — для обычного пользователя-рекламодателя:

* `read_ads` — чтение статистики и РК;
* `read_payments` — чтение денежных транзакций и баланса;
* `create_ads` — создание и редактирование настроек РК, баннеров, аудиторий
    (ставки, статус, таргетинги и т.п.).

Для пользователей-агентств и пользователей-представительств:

*  `create_clients` — создание новых клиентов;
*  `read_clients` — просмотр клиентов и операции от их имени;
*  `create_agency_payments` — переводы средств на счёта клиентов и обратно.

### Истечение срока действия access-токена

Каждый полученный access-токен является действительным в течение суток.
На это указывает свойство `expires_in` в ответе на запрос access-токена.
Там же указывается `refresh_token` — специальный токен для получения нового
access-токена. За это отвечает схема
[Refreshing an Access Token](http://tools.ietf.org/html/draft-ietf-oauth-v2-31#section-6)
в протоколе OAuth2.

Запрос на обновление access-токена:

    POST /api/v2/oauth2/token.json HTTP/1.1
    Host: target.mail.ru
    Content-Type: application/x-www-form-urlencoded

    grant_type=refresh_token&refresh_token={refresh_token}&client_id={client_id}&client_secret={client_secret}

Пример ответа:

    HTTP/1.1 200 OK
    Content-Type: application/json; charset=UTF-8

    {
      "access_token": "{new_access_token}",
      "token_type": "bearer",
      "scope": "{scope}",
      "expires_in": "86400",
      "refresh_token": "{new_refresh_token}"
    }

### Agency Client Credentials Grant

Эта схема протокола OAuth2 не является стандартной. Она была реализована для
того, чтобы дать возможность агентствам создавать access-токены для своих
клиентов напрямую (без подтверждения от клиента). Схема очень похожа на
стандартную Client Credentials Grant за исключением того, что в запросе
нужно передавать дополнительный параметр agency_client_name (имя пользователя
клиента):

    POST /api/v2/oauth2/token.json HTTP/1.1
    Host: target.mail.ru
    Content-Type: application/x-www-form-urlencoded

    grant_type=agency_client_credentials&client_id={client_id}&client_secret={client_secret}&agency_client_name={client_username}