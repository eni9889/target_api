## Ремаркетинговые списки пользователей


### Удаление загруженного списка пользователей
`DELETE /api/v1/remarketing_users_list/{users_list_id}.json`

Метод позволяет удалить загруженный список. Это возможно только в
том случае, если список не используется ни в одной аудитории. В случае
успеха метод возвращает пустой ответ с кодом 204.


### Получение списка загруженных списков пользователей
`GET /api/v1/remarketing_users_lists.json`

Метод позволяет получить список загруженных списков пользователей, которые
могут быть использованы в качестве источника данных для ремаркетинга, в
виде объектов RemarketingUsersListForm.


### Загрузка списка пользователей
`POST /api/v1/remarketing_users_lists.json`

Метод позволяет загрузить список пользователей в качестве источника данных
для ремаркетинга. В случае успешной загрузки возвращается объект типа
RemarketingUsersListForm.

Передаваемые параметры `name` и `type`:

* name - название списка
* type - тип списка. Возможные варианты:
    * ok - id пользователей одноклассников,

Формат файла:

* Первая строка - урл приложения
* Остальные строки id пользователя сложенное по модулю 2 (xor) с ключом
  приложения.

Алгоритм подписи запроса на загрузку файла немного отличается от
стандартного. В запросе необходимо передать HTTP-заголовок `X-Trg-Chksum`,
который содержит хэш-сумму от загружаемого файла, полученную по
алгоритму `MD5`. Кроме того, при создании подписи вместо тела POST-запроса
нужно использовать эту же хэш-сумму.

В отличие от других POST-запросов к API, имеющих тип `application/json`,
запрос на загрузку должен иметь тип `multipart/form-data`.
Сам файл для загрузки нужно передавать как часть multipart-запроса
с именем `file`. Название списка `name` и тип файла `type` нужно передавать
также в multipart-виде.
