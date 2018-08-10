title: Revo API

language_tabs:
   - shell

toc_footers:
   - <a href='#'>Sign Up for a Developer Key</a>
   - <a href='https://github.com/lavkumarv'>Documentation Powered by lav</a>

<!-- includes:
   - errors
 -->
search: true



# Introduction

test

Revo API for merchant APP.

**Version:** 2.0.0.beta

# Authentication

|apiKey|*API Key*|
|---|---|

# /SESSIONS
## ***POST***

**Summary:** Creates agent session.

**Description:** Returns token for next requests.

### HTTP Request
`***POST*** /sessions`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| user | body | User credentials for auth | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | OK |
| 401 | Invalid credetials |
| 422 | Unprocessible entity |

## ***DELETE***

**Summary:** Destroy agent session

**Description:** Remove current agent token

### HTTP Request
`***DELETE*** /sessions`

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful operation |
| 401 | Unauthorized |

# /PASSWORDS
## ***POST***

**Summary:** Send confirmation code

**Description:** Send confirmation code to agent phone

### HTTP Request
`***POST*** /passwords`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| user | body | User with mobile phone | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful operation |
| 422 | Unprocessible entity |

## ***PUT***

**Summary:** Set new agent pin

**Description:** Check confirmation code, set new pin and return api_key

### HTTP Request
`***PUT*** /passwords`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| user | body | User login, confirmation_code, new pin and pin confirmation | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful operation |
| 422 | Unprocessible entity |

# /AGENT
## ***GET***

**Summary:** Get agent info

**Description:** Returns basic agent information

### HTTP Request
`***GET*** /agent`

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful operation |
| 401 | Unauthorized |

# /STORES/{ID}/REPORTS
## ***GET***

**Summary:** Get agent reports

**Description:** Returns agent sales report by period for store

### HTTP Request
`***GET*** /stores/{id}/reports`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| id | path | Agent store ID | Yes | string |
| from | query |  | Yes | string (%d-%m-%Y) |
| to | query |  | Yes | string (%d-%m-%Y) |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful operation |
| 401 | Unauthorized |

# /STORES/{ID}
## ***GET***

**Summary:** Get store information

**Description:** Returns store information

### HTTP Request
`***GET*** /stores/{id}`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| id | path | Agent store ID | Yes | string |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful operation |
| 401 | Unauthorized |

# /LOAN_REQUESTS
## ***POST***

**Summary:** Create loan request

**Description:** Create loan request record and return token

### HTTP Request
`***POST*** /loan_requests`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| loan_request | body |  | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful operation |
| 400 | bad request |
| 422 | Unprocessible entity |

# /LOAN_REQUESTS/{TOKEN}
## ***GET***

**Summary:** Get tariff information

**Description:** Get information about tariff and terms

### HTTP Request
`***GET*** /loan_requests/{token}`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | path | Loan request unique token | Yes | string |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful operation |
| 422 | unproccesable entity |

## ***PUT***

**Summary:** Update loan_request

**Description:** Update phone or sum for loan_request

### HTTP Request
`***PUT*** /loan_requests/{token}`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | path | Loan request unique token | Yes | string |
| loan_request | body | Loan request parameters | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful operation |
| 422 | unprocessable entity |

# /LOAN_REQUESTS/{TOKEN}/CLIENT
## ***GET***

**Summary:** Client information

**Description:** Check client for loan_request

### HTTP Request
`***GET*** /loan_requests/{token}/client`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | path | Unique loan_request token | Yes | string |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful operation |
| 422 | unprocessable entity |

## ***POST***

**Summary:** Create client

**Description:** Create client for loan_request

### HTTP Request
`***POST*** /loan_requests/{token}/client`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | path |  | Yes | string |
| client | body | Client form parameters | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful operation |
| 422 | unprocessable entity |

# /LOAN_REQUESTS/{TOKEN}/CONFIRMATION
## ***POST***

**Summary:** Confirm client

**Description:** Confirm client by code

### HTTP Request
`***POST*** /loan_requests/{token}/confirmation`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | path |  | Yes | string |
|  | body |  | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful operation |
| 422 | unprocessable entity |

# /LOAN_REQUESTS/{TOKEN}/CLIENT/CONFIRMATION
## ***POST***

**Summary:** Send confirmation code

**Description:** Send confirmation code to client

### HTTP Request
`***POST*** /loan_requests/{token}/client/confirmation`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | path | Unique loan_request token | Yes | string |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful operation |
| 422 | unprocessable entity |

# /LOAN_REQUESTS/{TOKEN}/LOAN
## ***POST***

**Summary:** Create approved loan

**Description:** Create LoanApplication with `approved` status

### HTTP Request
`***POST*** /loan_requests/{token}/loan`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | path |  | Yes | string |
|  | body |  | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful operation |

# /LOAN_REQUESTS/{TOKEN}/LOAN/FINALIZATION
## ***POST***

**Summary:** Finalize loan

**Description:** Finalize LoanApplication

### HTTP Request
`***POST*** /loan_requests/{token}/loan/finalization`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | path |  | Yes | string |
| loan | body | Loan form parameters | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful operation |
| 422 | unprocessable entity |

# /LOAN_REQUESTS/{TOKEN}/DOCUMENTS/{KIND}
## ***GET***

**Summary:** Get documents

**Description:** Load loan documents by kind

### HTTP Request
`***GET*** /loan_requests/{token}/documents/{kind}`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| token | path |  | Yes | string |
| kind | path |  | Yes | string |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful operation |

<!-- Converted with the swagger-to-slate https://github.com/lavkumarv/swagger-to-slate -->
