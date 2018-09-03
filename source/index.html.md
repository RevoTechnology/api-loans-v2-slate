---
title: Revo API Loans Documentation

language_tabs: # must be one of https://git.io/vQNgJ
  - ruby--tab: Ruby
  - java: Java

toc_footers:
- <a href='https://revo.ru/'>Revo.ru</a>
<!-- - <a href='https://revo.ru/API/en'>API Documentation in English</a> -->

<!-- includes:
- business -->

search: true
---

# Введение

API Loans реализовано на протоколе HTTPS на основе JSON запросов.

# Авторизация

## Базовые URL адреса

```javascript
BASE_URL = "https://r.revoplus.ru/"
BASE_URL = "https://demo.revoplus.ru/"
```

1. Для взаимодействия с сервисами Рево используются 2 базовых адреса:
 * https://r.revoplus.ru/ - адрес `production` сервиса.
 * https://demo.revoup.ru/ - адрес `demo` сервиса.
2. `BASE_URL` - переменная обозначающая базовый адрес.

<aside class="notice">
Подключение должно производиться только по протоколу HTTPS  - при попытке подключения по HTTP будет возникать 404 ошибка.
</aside>

## Параметры авторизации

Для авторизации обращения к методам API используется `authentication_token`, который генерируется путём создания сессии с помощью метода <a href="#post-sessions">sessions</a>.
`login` и `password` необходимые для этого метода создаются на стороне Рево.

# Методы API

## POST sessions

```ruby
POST BASE_URL/api/loans/v1/sessions
```

Метод для создания сессии пользователя (консультанта). Возвращает `authentication_token`, который необходим для авторизации обращения к другим методам API.

###Parameters

> Пример запроса в формате json

```jsonnet
{
  "user":
  {
    "login": "qwerty",
    "password": "123456"
  }
}
```

| | | |
-:|-:|:-|:-
 |**user**<br> <font color="#939da3">object</font> | <td colspan="2"> Объект, содержащий информацию об авторизуемом пользователе (консультанте).
  <td colspan="2" style="text-align:right">**login**<br> <font color="#939da3">string</font> | | Логин пользователя.
  <td colspan="2" style="text-align:right">**password**<br> <font color="#939da3">sring</font> | | Пароль пользователя.

###Response Parameters

> Пример ответа при успешной аутентификации (code 200).

```jsonnet
{
  "user":
  {
    "authentication_token": "api_key_123456"
  }
}
```

> Пример ответа при неуспешной аутентификации (code 422).

```jsonnet
{
  "errors":
  {
    "login": ["неверный логин или пароль"]
  }
}
```

| | | |
-:|-:|:-|:-
 |**authentication_token**<br> <font color="#939da3">string</font> | <td colspan="2"> Токен для последующего обращения к методам API.
 |**errors**<br> <font color="#939da3">object</font> | <td colspan="2"> Объект, содержащий информацию об ошибках.

## DELETE sessions

  ```ruby
  DELETE BASE_URL/api/loans/v1/sessions
  ```

  Метод для закрытия сессии пользователя (консультанта).

###Response Parameters

  | | | |
  -:|-:|:-|:-
   |**Code 200** | <td colspan="2"> Операция успешна.
   |**Code 401** | <td colspan="2"> Сессия не обнаружена.

<!-- ##POST passwords

 ```ruby
 POST BASE_URL/passwords
 ```

 Метод для отправки кодя подтверждения пользователю (консультанту).

###Parameters
 > Пример запроса в формате json

 ```jsonnet
 {
   "user":
   {
     "login": "qwerty"
   }
 }
 ```

 | | | |
 -:|-:|:-|:-
  |**user**<br> <font color="#939da3">object</font> | <td colspan="2"> Объект, содержащий информацию об авторизуемом пользователе (консультанте).
   <td colspan="2" style="text-align:right">**login**<br> <font color="#939da3">string</font> | | Логин пользователя.

###Response Parameters

   > Пример ответа при неуспешной аутентификации (code 422).

   ```jsonnet
   {
     "errors":
     {
       "login": ["логин не найден"]
     }
   }
   ```

   | | | |
   -:|-:|:-|:-
    |**authentication_token**<br> <font color="#939da3">string</font> | <td colspan="2"> Токен для последующего обращения к методам API.
    |**errors**<br> <font color="#939da3">object</font> | <td colspan="2"> Объект, содержащий информацию об ошибках.
    <td colspan="2" style="text-align:right">**login**<br> <font color="#939da3">string</font> | | Логин пользователя ???

## PUT passwords

  ```ruby
  PUT BASE_URL/passwords
  ```
  Метод для проверки кода подтверждения, установки нового ПИН-а и получения `api_key`.

### Parameters

  > Пример запроса в формате json

  ```jsonnet
  {
    "user":
    {
      "login": "string",
      "confirmation_code": "string",
      "password": "string",
      "password_confirmation": "string"
    }
  }
  ```

  | | | |
  -:|-:|:-|:-
   |**user**<br> <font color="#939da3">object</font> | <td colspan="2"> Объект, содержащий информацию об авторизуемом пользователе (консультанте).
    <td colspan="2" style="text-align:right">**login**<br> <font color="#939da3">string</font> | | Логин пользователя.
    <td colspan="2" style="text-align:right">**confirmation_code**<br> <font color="#939da3">string</font> | | Код подтверждения.
    <td colspan="2" style="text-align:right">**password**<br> <font color="#939da3">sring</font> | | Пароль пользователя.
    <td colspan="2" style="text-align:right">**password_confirmation**<br> <font color="#939da3">string</font> | | Пароль подтверждения.

###Response Parameters

  > Пример ответа при успешной аутентификации (code 200).

  ```jsonnet
  {
    "user":
    {
      "authentication_token": "string"
    }
  }
  ```

  > Пример ответа при неуспешной аутентификации (code 422).

  ```jsonnet
  {
    "errors":
    {
      "confirmation_code": ["неверный"],
      "password": ["должен быть равен 4 символам"]
    }
  }
  ```

  | | | |
  -:|-:|:-|:-
  <td colspan="2" style="text-align:right">**confirmation_code**<br> <font color="#939da3">string</font> | | Код подтверждения.
  <td colspan="2" style="text-align:right">**password**<br> <font color="#939da3">sring</font> | | Пароль пользователя.
 -->

## POST loan_requests

```ruby
POST BASE_URL/api/loans/v1/loan_requests
```

Метод для создания завки на займ. Возвращает `{token}` заявки.

###Parameters

> Пример запроса в формате json

```jsonnet
{
  "loan_request":
  {
    "store_id": "1234",
    "order_id": "R2424"
  }
}
```

| | | |
-:|-:|:-|:-
 |**loan_request**<br> <font color="#939da3">object</font> | <td colspan="2"> Объект, содержащий информацию о займе.
  <td colspan="2" style="text-align:right">**store_id**<br> <font color="#939da3">string</font> | | Идентификатор торговой точки. Создается на стороне Рево.
  <td colspan="2" style="text-align:right">**order_id**<br> <font color="#939da3">string</font> | | Идентификатор заявки по системе Рево.

###Response Parameters

> Пример ответа при успешной аутентификации (code 200).

```jsonnet
{
  "loan_request":
  {
    "token": "b4e9d33f6021ad31a1b494ffb7be29069d580dfc"
  }
}
```

> Пример ответа при неуспешной аутентификации (code 422).

```jsonnet
{
  "errors":
  {
    "store_id": ["не найден"]
  }
}
```

| | | |
-:|-:|:-|:-
 |**loan_request**<br> <font color="#939da3">object</font> | <td colspan="2"> Объект, содержащий информацию о займе.
 <td colspan="2" style="text-align:right">**token**<br> <font color="#939da3">string</font> | | Токен заявки.
 |**errors**<br> <font color="#939da3">object</font> | <td colspan="2"> Объект, содержащий информацию об ошибках.

## GET loan_requests/{token}

```ruby
GET BASE_URL/api/loans/v1/loan_requests/{token}?amount=AMOUNT
```

Метод для получения доступных графиков платежей для заявки. Для расчёта графика платежей для суммы покупок `AMOUNT` используется переменная `amount`. Если она не указана, значение суммы покупки берётся из заявки (проставляется методом <a href="#put-loan_requests-token">loan_request</a>).

Если в заявке не указан номер телефона, то расчёт графика платежей производится как для нового клиента.

###Response Parameters

> Пример ответа при успешной аутентификации (code 200).

```jsonnet
{
  "loan_request":
  [
    {
      "term_id": 2,
      "term": 3,
      "monthly_payment": 2945,
      "total_of_payments": 8833,
      "sum_with_discount": 6399,
      "total_overpayment": 2434,
      "sms_info": 59.0,
      "schedule":
      [
        {
          "date": "10-09-2018",
          "amount": 2945
        },
        {
          "date": "09-10-2018",
          "amount": 2945
        },
        {
          "date": "09-11-2018",
          "amount": 2943
        }
      ]
    }
  ]
}
```

| | | | | |
-:|-:|-:|:-|:-|:-
 | **loan_request**<br> <font color="#939da3">object</font> | | <td colspan="3"> Объект, содержащий информацию о расчёте доступных продуктов.
 <td colspan="2" style="text-align:right">**term_id**<br> <font color="#939da3">integer</font> | | <td colspan="2" style="text-align:left"> Номер продукта.
 <td colspan="2" style="text-align:right">**term**<br> <font color="#939da3">integer</font> | | <td colspan="2" style="text-align:left"> Срок займа в месяцах.
 <td colspan="2" style="text-align:right">**monthly_payment**<br> <font color="#939da3">float</font> | | <td colspan="2" style="text-align:left"> Величина ежемесячного платежа с учётом переплаты.
 <td colspan="2" style="text-align:right">**total_of_payments**<br> <font color="#939da3">float</font> | | <td colspan="2" style="text-align:left"> Общая сумма к погашению займа (с учётом переплат).
 <td colspan="2" style="text-align:right">**sum_with_discount**<br> <font color="#939da3">float</font> | | <td colspan="2" style="text-align:left"> Стоимость товаров в рублях с копейками с учётом комиссии Рево (скидки, предоставляемой партнёром).
 <td colspan="2" style="text-align:right">**total_overpayment**<br> <font color="#939da3">float</font> | | <td colspan="2" style="text-align:left"> Общая сумма переплаты за весь период займа.
 <td colspan="2" style="text-align:right">**sms_info**<br> <font color="#939da3">float</font> | | <td colspan="2" style="text-align:left"> Стоимость услуги смс-информирования.
 <td colspan="2" style="text-align:right">**schedule**<br> <font color="#939da3">object</font> | | <td colspan="2" style="text-align:left"> Объект, содержащий информацию о графике платежей.
 <td colspan="3" style="text-align:right">**date**<br> <font color="#939da3">string</font> | | | Дата платежа в формате `dd-mm-yyyy`.
 <td colspan="3" style="text-align:right">**amount**<br> <font color="#939da3">float</font> | | | Величина платежа клиента в месяц в рублях с копейками. Последний платёж может отличаться от предыдущих.

###Response Parameters

> Пример ответа при неуспешной аутентификации (code 422).

```ruby
{
  "errors":
  {
    "amount": ["не может быть пустым"],
    "mobile_phone": ["не может быть пустым"]
  }
}
```

| | | |
-:|-:|:-|:-
 |**errors**<br> <font color="#939da3">object</font> | <td colspan="2"> Объект, содержащий информацию об ошибках.

## PUT loan_requests/{token}

```jsonnet  
PUT BASE_URL/api/loans/v1/loan_requests
```

Метод для обновления информации по займу, например суммы или мобильного телефона.

###Parameters

> Пример запроса в формате json

```jsonnet
{
  "loan_request":
  {
    "mobile_phone": "79998887776",
    "amount": 1399.00
  }
}
```

| | | |
-:|-:|:-|:-
 |**loan_request**<br> <font color="#939da3">object</font> | <td colspan="2"> Объект, содержащий информацию о заявке.
  <td colspan="2" style="text-align:right">**mobile_phone**<br> <font color="#939da3">string</font> | | Мобильный телефон клиента из 11 цифр (с кодом страны).
  <td colspan="2" style="text-align:right">**amount**<br> <font color="#939da3">float</font> | | Стоимость товаров в рублях с копейками с учётом комиссии Рево (скидки, предоставляемой партнёром).

###Response Parameters

> Пример ответа при неуспешной аутентификации (code 422).

```jsonnet
{
  "errors": {
    "phone_number": ["имеет неверный формат"],
    "amount": ["не может быть больше 100000 рублей"]
  }
}
```

| | | |
-:|-:|:-|:-
 |**errors**<br> <font color="#939da3">object</font> | <td colspan="2"> Объект, содержащий информацию об ошибках.

## GET loan_requests/{token}/client

```ruby
GET BASE_URL/api/loans/v1/loan_requests/{token}/client
```

Метод для получения информации о клиенте. С помощью данного метода можно отличать повторных клиентов от новых: если в ответе с кодом 200 пришел пустой json `{}`, то клиент является новым; во всех остальных случаях - повторным.

###Response Parameters

> Пример ответа при успешной аутентификации (code 200).

```jsonnet
{
  "client":
  {
    "id": 1,
    "mobile_phone": "79998887776",
    "first_name": "Иван",
    "middle_name": "Иванович",
    "last_name": "Иванов",
    "birth_date": "01-01-1990",
    "email": "user@example.com",
    "area": "Москва",
    "settlement": "Москва",
    "street": "Новая",
    "house": "123",
    "building": "123",
    "apartment": "123",
    "postal_code": "12345",
    "credit_limit": 33000.00,
    "missing_documents": ["name"],
    "id_documents":
    [
      {
        "series": "7711",
        "number": "123456",
        "expiry_date": "02-03-2025",
        "issue_date": "02-03-2025",
        "issuing_authority": "МВД России по г.Москва",
        "issuing_authority_code": "770001"
      }
    ]
  }
}
```

| | | | | |
-:|-:|-:|:-|:-|:-
 | **client**<br> <font color="#939da3">object</font> | | <td colspan="3"> Объект, содержащий информацию о клиенте.
 <td colspan="2" style="text-align:right">**id**<br> <font color="#939da3">integer</font> | | <td colspan="2" style="text-align:left"> Уникальный идентификатор клиента.
 <td colspan="2" style="text-align:right">**mobile_phone**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Мобильный телефон клиента из 11 цифр (с кодом страны).
 <td colspan="2" style="text-align:right">**first_name**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Имя клиента.
 <td colspan="2" style="text-align:right">**middle_name**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Отчество клиента.
 <td colspan="2" style="text-align:right">**last_name**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Фамилия клиента.
 <td colspan="2" style="text-align:right">**birth_date**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Дата рождения клиента в формате `dd-mm-yyyy`.
 <td colspan="2" style="text-align:right">**email**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Email клиента.
 <td colspan="2" style="text-align:right">**area**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Область по месту регистрации клиента.
 <td colspan="2" style="text-align:right">**settlement**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Город по месту регистрации клиента.
 <td colspan="2" style="text-align:right">**street**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Улица по месту регистрации клиента.
 <td colspan="2" style="text-align:right">**house**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Номер дома по месту регистрации клиента.
 <td colspan="2" style="text-align:right">**building**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Строение по месту регистрации клиента.
 <td colspan="2" style="text-align:right">**apartment**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Номер квартиры по месту регистрации клиента.
 <td colspan="2" style="text-align:right">**postal_code**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Почтовый индекс по месту регистрации клиента.
 <td colspan="2" style="text-align:right">**credit_limit**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Лимит клиента в руб с копейками.
 <td colspan="2" style="text-align:right">**missing_documents**<br> <font color="#939da3">array</font> | | <td colspan="2" style="text-align:left"> Список документов, которых не хватает у клиента.
 <td colspan="2" style="text-align:right">**id_documents**<br> <font color="#939da3">object</font> | | <td colspan="2" style="text-align:left"> Объект, содержащий информацию о документах клиента.
 <td colspan="3" style="text-align:right">**series**<br> <font color="#939da3">string</font> | | | Серия паспорта клиента.
 <td colspan="3" style="text-align:right">**number**<br> <font color="#939da3">string</font> | | | Номер паспорта клиента.
 <td colspan="3" style="text-align:right">**expiry_date**<br> <font color="#939da3">string</font> | | | Дата окончания действия паспорта в формате `dd-mm-yyyy`.
 <td colspan="3" style="text-align:right">**issue_date**<br> <font color="#939da3">string</font> | | | Дата выдачи паспорта в формате `dd-mm-yyyy`.
 <td colspan="3" style="text-align:right">**issuing_authority**<br> <font color="#939da3">string</font> | | | Орган, выдавший паспорт.
 <td colspan="3" style="text-align:right">**issuing_authority_code**<br> <font color="#939da3">string</font> | | | Код подразделения, выдавшего паспорт.

> Пример ответа при неуспешной аутентификации (code 422).

```jsonnet
{
  "errors":
  {
    "amount": ["не может быть пустым"],
    "mobile_phone": ["не может быть пустым"]
  }
}
```

| | | |
-:|-:|:-|:-
 |**errors**<br> <font color="#939da3">object</font> | <td colspan="2"> Объект, содержащий информацию об ошибках.

## POST loan_requests/{token}/client

```ruby
GET BASE_URL/api/loans/v1/loan_requests/{token}/client
```

Метод для создания клиента с персональными данными.

### Parameters

> Пример ответа при успешной аутентификации (code 200).

```jsonnet
{
  "client":
  {
    "id": 1,
    "mobile_phone": "79998887776",
    "first_name": "Иван",
    "middle_name": "Иванович",
    "last_name": "Иванов",
    "birth_date": "01-01-1990",
    "email": "user@example.com",
    "pesel": "81010200131",
    "area": "Москва",
    "settlement": "Москва",
    "street": "Новая",
    "house": "123",
    "building": "123",
    "apartment": "123",
    "postal_code": "12345",
    "id_documents":
    [
      {
        "series": "7711",
        "number": "123456",
        "expiry_date": "02-03-2025",
        "issue_date": "02-03-2025",
        "issuing_authority": "МВД России по г.Москва",
        "issuing_authority_code": "770001"
      }
    ]
  }
}
```

| | | | | |
-:|-:|-:|:-|:-|:-
 | **client**<br> <font color="#939da3">object</font> | | <td colspan="3"> Объект, содержащий информацию о клиенте.
 <td colspan="2" style="text-align:right">**id**<br> <font color="#939da3">integer</font> | | <td colspan="2" style="text-align:left"> Уникальный идентификатор клиента.
 <td colspan="2" style="text-align:right">**mobile_phone**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Мобильный телефон клиента из 11 цифр (с кодом страны).
 <td colspan="2" style="text-align:right">**first_name**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Имя клиента.
 <td colspan="2" style="text-align:right">**middle_name**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Отчество клиента.
 <td colspan="2" style="text-align:right">**last_name**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Фамилия клиента.
 <td colspan="2" style="text-align:right">**birth_date**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Дата рождения клиента в формате `dd-mm-yyyy`.
 <td colspan="2" style="text-align:right">**email**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Email клиента.
 <td colspan="2" style="text-align:right">**area**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Область по месту регистрации клиента.
 <td colspan="2" style="text-align:right">**settlement**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Город по месту регистрации клиента.
 <td colspan="2" style="text-align:right">**street**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Улица по месту регистрации клиента.
 <td colspan="2" style="text-align:right">**house**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Номер дома по месту регистрации клиента.
 <td colspan="2" style="text-align:right">**building**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Строение по месту регистрации клиента.
 <td colspan="2" style="text-align:right">**apartment**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Номер квартиры по месту регистрации клиента.
 <td colspan="2" style="text-align:right">**postal_code**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Почтовый индекс по месту регистрации клиента.
 <td colspan="2" style="text-align:right">**id_documents**<br> <font color="#939da3">object</font> | | <td colspan="2" style="text-align:left"> Объект, содержащий информацию о документах клиента.
 <td colspan="3" style="text-align:right">**series**<br> <font color="#939da3">string</font> | | | Серия паспорта клиента.
 <td colspan="3" style="text-align:right">**number**<br> <font color="#939da3">string</font> | | | Номер паспорта клиента.
 <td colspan="3" style="text-align:right">**expiry_date**<br> <font color="#939da3">string, *optional*</font> | | | Дата окончания действия паспорта в формате `dd-mm-yyyy`.
 <td colspan="3" style="text-align:right">**issue_date**<br> <font color="#939da3">string, *optional*</font> | | | Дата выдачи паспорта в формате `dd-mm-yyyy`.
 <td colspan="3" style="text-align:right">**issuing_authority**<br> <font color="#939da3">string, *optional*</font> | | | Орган, выдавший паспорт.
 <td colspan="3" style="text-align:right">**issuing_authority_code**<br> <font color="#939da3">string, *optional*</font> | | | Код подразделения, выдавшего паспорт.

###Response Parameters

 > Пример ответа при успешной аутентификации (code 200).

 ```jsonnet
 {
   "client":
   {
     "id": 1,
     "mobile_phone": "79998887776",
     "first_name": "Иван",
     "middle_name": "Иванович",
     "last_name": "Иванов",
     "birth_date": "01-01-1990",
     "email": "user@example.com",
     "pesel": "81010200131",
     "area": "Москва",
     "settlement": "Москва",
     "street": "Новая",
     "house": "123",
     "building": "123",
     "apartment": "123",
     "postal_code": "12345",
     "credit_limit": "33000.00",
     "id_documents":
     [
       {
         "series": "7711",
         "number": "123456",
         "expiry_date": "02-03-2025",
         "issue_date": "02-03-2025",
         "issuing_authority": "МВД России по г.Москва",
         "issuing_authority_code": "770001"
       }
     ]
   }
 }
 ```

 > Пример ответа при неуспешной аутентификации (code 422).

 ```jsonnet
 {
   "errors":
   {
     "amount": ["не может быть пустым"],
     "mobile_phone": ["не может быть пустым"]
   }
 }
 ```

 | | | | | |
 -:|-:|-:|:-|:-|:-
  | **client**<br> <font color="#939da3">object</font> | | <td colspan="3"> Объект, содержащий информацию о клиенте.
  <td colspan="2" style="text-align:right">**id**<br> <font color="#939da3">integer</font> | | <td colspan="2" style="text-align:left"> Уникальный идентификатор клиента.
  <td colspan="2" style="text-align:right">**mobile_phone**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Мобильный телефон клиента из 11 цифр (с кодом страны).
  <td colspan="2" style="text-align:right">**first_name**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Имя клиента.
  <td colspan="2" style="text-align:right">**middle_name**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Отчество клиента.
  <td colspan="2" style="text-align:right">**last_name**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Фамилия клиента.
  <td colspan="2" style="text-align:right">**birth_date**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Дата рождения клиента в формате `dd-mm-yyyy`.
  <td colspan="2" style="text-align:right">**email**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Email клиента.
  <td colspan="2" style="text-align:right">**area**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Область по месту регистрации клиента.
  <td colspan="2" style="text-align:right">**settlement**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Город по месту регистрации клиента.
  <td colspan="2" style="text-align:right">**street**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Улица по месту регистрации клиента.
  <td colspan="2" style="text-align:right">**house**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Номер дома по месту регистрации клиента.
  <td colspan="2" style="text-align:right">**building**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Строение по месту регистрации клиента.
  <td colspan="2" style="text-align:right">**apartment**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Номер квартиры по месту регистрации клиента.
  <td colspan="2" style="text-align:right">**postal_code**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Почтовый индекс по месту регистрации клиента.
  <td colspan="2" style="text-align:right">**credit_limit**<br> <font color="#939da3">string</font> | | <td colspan="2" style="text-align:left"> Лимит клиента в руб с копейками.
  <td colspan="2" style="text-align:right">**id_documents**<br> <font color="#939da3">object</font> | | <td colspan="2" style="text-align:left"> Объект, содержащий информацию о документах клиента.
  <td colspan="3" style="text-align:right">**series**<br> <font color="#939da3">string</font> | | | Серия паспорта клиента.
  <td colspan="3" style="text-align:right">**number**<br> <font color="#939da3">string</font> | | | Номер паспорта клиента.
  <td colspan="3" style="text-align:right">**expiry_date**<br> <font color="#939da3">string</font> | | | Дата окончания действия паспорта в формате `dd-mm-yyyy`.
  <td colspan="3" style="text-align:right">**issue_date**<br> <font color="#939da3">string</font> | | | Дата выдачи паспорта в формате `dd-mm-yyyy`.
  <td colspan="3" style="text-align:right">**issuing_authority**<br> <font color="#939da3">string</font> | | | Орган, выдавший паспорт.
  <td colspan="3" style="text-align:right">**issuing_authority_code**<br> <font color="#939da3">string</font> | | | Код подразделения, выдавшего паспорт.
  | **errors**<br> <font color="#939da3">object</font> | | <td colspan="3"> Объект, содержащий информацию об ошибках.

## PATCH loan_requests/{token}/client

```ruby
PATCH  BASE_URL/api/loans/v1/loan_requests/{token}/client
```

Метод для загрузки фотографий клиента. Используется multipart/form-data.

###Parameters

> Пример запроса

```jsonnet
{
  "client":
  {
    "documents":
    [
      "kind": File.open("name.jpg")
    ]
  }
}
```

 | |
-:|:-
 **[name]**<br> <font color="#939da3">file</font> | Фото первого разворота паспорта.
 **[living_addr]**<br> <font color="#939da3">file</font> | Фото паспорта со страницей регистрации.
 **[client_with_passport]**<br> <font color="#939da3">file</font> | Фото клиента с паспортом.
 **[previous_passport]**<br> <font color="#939da3">file</font> | Фото предыдущего имени клиента.
 **[issued_by]**<br> <font color="#939da3">file</font> | Фото страницы "кем выдан" паспорт.
 **[first_two_pages]**<br> <font color="#939da3">file</font> | Фото первых двух страниц паспорта.

###Response Parameters

> Пример ответа при неуспешной аутентификации (code 422).

```jsonnet
{
  "errors":
   {
    "kind": ["неверное значение типа документа"],
    "client": ["не удалось найти клиента"]
  }
}
```

| | | |
-:|-:|:-|:-
  |**errors**<br> <font color="#939da3">object</font> | <td colspan="2"> Объект, содержащий информацию об ошибках.
 <td colspan="2" style="text-align:right">**kind**<br> <font color="#939da3">string</font> | | Тип загружаемого документа.

## POST loan_requests/{token}/confirmation

```ruby
POST  BASE_URL/api/loans/v1/loan_requests/{token}/confirmation
```

Метод для подтверждения согласия с передачей персональных данных и верификации номера телефона клиента с помощью смс кода. Используется для существующих клиентов. Для новых клиентов код передаётся при создании клиента с помощью метода <a href="#post-loan_requests-token-client">loan_requests/{token}/client</a>.

###Parameters

> Пример запроса в формате json

```jsonnet
{
  "code": "1234"
}
```

 | |
-:|:-
 **code**<br> <font color="#939da3">int</font> | Четырёхзначный код подтверждения.

###Response Parameters

> Пример ответа при успешной аутентификации (code 200).

```jsonnet
{
  "client":
   {
    "first_name": "Иван",
    "middle_name": "Иванович",
    "last_name": "Иванов",
    "decision": "approved",
    "credit_limit": 1255.2
  }
}
```

> Пример ответа при неуспешной аутентификации (code 422).

```jsonnet
{
  "errors":
  {
    "code": ["имеет неверное значение"]
  }
}
```

| | | |
-:|-:|:-|:-
 | **client**<br> <font color="#939da3">object</font> | <td colspan="2"> Объект, содержащий информацию о клиенте.
 <td colspan="2" style="text-align:right">**first_name**<br> <font color="#939da3">integer</font> | | Имя клиента.
 <td colspan="2" style="text-align:right">**middle_name**<br> <font color="#939da3">integer</font> | | Отчество клиента.
 <td colspan="2" style="text-align:right">**last_name**<br> <font color="#939da3">integer</font> | | Фамилия клиента.
 <td colspan="2" style="text-align:right">**decision**<br> <font color="#939da3">integer</font> | | Решение по лимиту. Возможные значения: `approved` - лимит одобрен; `declined` - лимит не одобрен.
 <td colspan="2" style="text-align:right">**credit_limit**<br> <font color="#939da3">integer</font> | | Величина доступного клиенту лимита в рублях с копейками.
  |**errors**<br> <font color="#939da3">object</font> | <td colspan="2"> Объект, содержащий информацию об ошибках.

## POST loan_requests/{token}/client/confirmation

```ruby
POST  BASE_URL/api/loans/v1/loan_requests/{token}/confirmation
```

Метод для отправки кода подтверждения клиенту по смс. Также используется для повторной отправки смс кода. Смс код используется как для верификации номера телефона (и согласия клиента с передачей персональных данных), так и для подтверждения займа при финализации.

###Response Parameters

> Пример ответа при неуспешной аутентификации (code 422).

```jsonnet
{
  "errors":
  {
    "mobile_phone": ["не может быть пустым"],
    "amount": ["не может быть пустым"]
  }
}
```

| | | |
-:|-:|:-|:-
  |**errors**<br> <font color="#939da3">object</font> | <td colspan="2"> Объект, содержащий информацию об ошибках.

## POST loan_requests/{token}/loan

```ruby
POST  BASE_URL/api/loans/v1/loan_requests/{token}/loan
```

Метод для создания займа.

### Parameters

> Пример запроса в формате json

```jsonnet
{
  "term_id": 2,
  "term": 3
}
```

 | |
-:|:-
**term_id**<br> <font color="#939da3">integer</font> | Номер продукта.
**term**<br> <font color="#939da3">integer</font> | Срок займа в месяцах.

### Response Parameters

> Пример ответа при неуспешной аутентификации (code 422).

```jsonnet
{
  "errors": {
    "amount": ["может иметь значение меньшее или равное 3000"],
    "base": ["К сожалению, ваша заявка отклонена"]
  }
}
```

| | | |
-:|-:|:-|:-
  |**errors**<br> <font color="#939da3">object</font> | <td colspan="2"> Объект, содержащий информацию об ошибках.
 <td colspan="2" style="text-align:right">**base**<br> <font color="#939da3">string</font> | | Информация по заявке.



## POST loan_requests/{token}/loan/finalization

```ruby
POST BASE_URL/api/loans/v1/loan_requests/{token}/loan/finalization
```

Метод для финализации заявки. В качестве смс кода может использоваться тот же код, что и для верификации номера телефона (и согласия клиента с передачей персональных данных).

###Parameters

> Пример запроса в формате json

```jsonnet
{
  "loan":
  {
    "agree_sms_info": true,
    "agree_processing": true,
    "confirmation_code": "1111"
  }
}
```

| | | |
-:|-:|:-|:-
 |**loan**<br> <font color="#939da3">object</font> | <td colspan="2"> Объект, содержащий информацию о займе.
  <td colspan="2" style="text-align:right">**agree_sms_info**<br> <font color="#939da3">boolean</font> | | Согласие клиента с подключением платного смс-информирования.
  <td colspan="2" style="text-align:right">**agree_processing**<br> <font color="#939da3">boolean</font> | | Согласие клиента с оформлением договора.
  <td colspan="2" style="text-align:right">**confirmation_code**<br> <font color="#939da3">string</font> | | Четырёхзначный код подтверждения.

###Response Parameters

> Пример ответа при неуспешном запросе (code 422).

```jsonnet
{
  "errors":
  {
    "agree_processing": ["нужно подтвердить"],
    "confirmation_code": ["имеет неверное значение"],
    "loan_application": ["для финализации займ должен быть подтвержден"]
  }
}
```

| | | |
-:|-:|:-|:-
 |**errors**<br> <font color="#939da3">object</font> | <td colspan="2"> Объект, содержащий информацию об ошибках.
 <td colspan="2" style="text-align:right">**loan_application**<br> <font color="#939da3">string</font> | |

## GET loan_requests/{token}/documents/{kind}.{format}

```ruby
GET BASE_URL/api/loans/v1/loan_requests/{token}/documents/{kind}.{pdf|html}
```

Метод для генерации юридических документов для клиента. Возвращает документ в формате pdf или html (по умолчанию).

### Kind values
 | |
-:|:-
**[asp]**<br> <font color="#939da3">file</font> | Согласие использование аналога собственноручной подписи (АСП), т.е. согласие использовать смс код вместо ручной подписи.
**[offer]**<br> <font color="#939da3">file</font> | Договор оферты.
**[agreement]**<br> <font color="#939da3">file</font> | Согласие на обработку персональных данных.
**[third_parties]**<br> <font color="#939da3">file</font> | Согласие на взаимодействие с третьими лицами, направленное на возврат просроченной задолженности.

# Коды ошибок

Код | Комментарии
-:|:-
**200** | Всё ок.
**400** | Некорректный формат json запроса.
**422** | В запросе содержится ошибка. Описание конкретных полей, где содержатся ошибки, находятся в массиве `errors`.

# What's new

23.08.2018
1. Added `missing_documents` to `GET client`. Represent client photo files that are missing for client.
2. Added `term_id` to `GET loan_request` and `POST loan_request/loan` to specify exact loan products.
3. Added `agree_sms_info` flag to `POST loan/finalization`. Used to specify if client wants to use sms-information feature.
4. Added `sms_info` to `GET loan_requests/{token}` to specify sms-information price for product calculation.
5. Added `store_id` to `POST loan_requests` to specify store order for further matching.
