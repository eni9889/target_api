## Интересы пользователей


### Получение дерева интересов
`GET /api/v1/interests.json`

Метод позволяет получить интересы пользователей в виде дерева.

#### Пример

HTTP-запрос:

    GET /api/v1/interests.json HTTP/1.1
    Host: target-sandbox.my.com
    Accept-Encoding: gzip, deflate, compress
    Authorization: Bearer Bh8kQmBUwgGDLuprqZhfMMm..7JrLbTAEFbEv74TydrC18

Curl-запрос:

    curl \
    -H 'Authorization: Bearer Bh8kQmBUwgGDLuprqZhfMMm..7JrLbTAEFbEv74TydrC18' \
    'https://target-sandbox.my.com/api/v1/interests.json'

Пример ответа:

    [
       {
          "children" : [
             {
                "name" : "Климатическая техника",
                "id" : 67
             },
             {
                "name" : "Техника для дома",
                "id" : 70
             }
          ],
          "name" : "Бытовая техника",
          "id" : 66
       },
       {
          "children" : [
             {
                "name" : "Дачная жизнь",
                "id" : 72
             },
             {
                "id" : 80,
                "name" : "Товары для дома и дачи"
             }
          ],
          "name" : "Для дома и дачи",
          "id" : 71
       },
       {
          "children" : [
             {
                "id" : 82,
                "name" : "Аквариум"
             },
             {
                "id" : 83,
                "name" : "Ветеринарные клиники"
             },
             {
                "name" : "Собаки",
                "id" : 89
             }
          ],
          "name" : "Домашние животные",
          "id" : 81
       }
    ]

