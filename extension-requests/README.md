# Extension Requests

## Extension Request Object

```
{
    "taskId": "Id of the task for which extension is requested",
    "title": "Title of the task",
    "assignee": "<userId>",
    oldEndsOn: "<epoch>",
    newEndsOn: "<epoch>",
    reason: "string" (Reasons for asking extension),
    status : `PENDING | APPROVED | DENIED`
}
```

## **Requests**

|               Route                |    Description    |
| :--------------------------------: | :---------------: |
|      [GET /extension-requests](#get-extension-requests)      | Return all extension requests |
|      [GET /extension-requests/:id](#get-extension-requests-id)      | Return extension request with id |
|      [GET /extension-requests/self](#get-extension-requests-self)      | Return all extension requests of a user |
|     [POST /extension-requests](#post-extension-requests)     | Creates new extension request  |
|     [PATCH /extension-requests/:id](#patch-extension-requestsid) |   Updates extension request   |
|     [PATCH /extension-requests/:id/status](#patch-extension-requests-id-status) |   APPROVE or DENY an extension request   |

## **GET /extension-requests**

Return all the extension-requests

- **Params**  
  None
- **Query**  
  `status=APPROVED | DENIED | PENDING, taskId=<task_object_id>, assignee=userId`
- **Body**  
  None
- **Headers**  
  None
- **Cookie**  
  rds-session: `<JWT SUPERUSER | APPOWNER >`
- **Success Response:**
- **Code:** 200
  - **Content:**

```
{
  message: 'Extension Requests returned successfully!'
  allExtensionRequest: [
           {<extension_request_object>},
           {<extension_request_object>}
         ]
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **GET /extension-requests/:id**

Return extension-request with `id`

- **Params**  
  id=<extension_request_object_id>
- **Query**  
  None
- **Body**  
  None
- **Headers**  
  None
- **Cookie**  
  rds-session: `<JWT SUPERUSER | APPOWNER >`
- **Success Response:**
- **Code:** 200
  - **Content:**

```
{
  message: 'Extension Requests returned successfully!'
  extensionRequest: {<extension_request_object>}
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **GET /extension-requests/self**

Returns all the extension requests of a user for a task if query `taskId=<taskId>` is passed, else returns all the extension requests of the user.

- **Params**  
  None
- **Query**  
  `status=APPROVED | DENIED | PENDING, taskId=<task_object_id>`
- **Body**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 200
    - **Content:**
```
{
  message: 'Extension Requests returned successfully!'
  allExtensionRequests: [{<extension_request_object>}]
}
```

- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 404
    - **Content:** `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'User doesn't exist' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`


## **POST /extension-requests**

- **Params**  
  None
- **Query**  
  None
- **Headers**  
  Content-Type: application/json
- **Body** 
- **Cookie**  
  rds-session: `<JWT>`
- **Success Response:**
- **Code:** 200
  - **Content:**

```
{
  message: 'Extension Request created successfully!'
  extensionRequest: {<extension_request_object>}
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`
    
## **PATCH /extension-requests/:id**

- **Params**  
  _Required:_ `id=[string]`

- **Headers**  
  Content-Type: application/json
- **Body** `{ <extension_request_object> }`
- **Success Response:**
- **Code:** 204

  - **Content:** `<No Content>`

- **Error Response:**
  - **Code** 404
    - **Content** `{ 'statusCode': 404, 'error': 'Not found', 'message': 'No extension requests found' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **PATCH /extension-requests/:id/status**

- **Params**  
  _Required:_ `id=[string]`

- **Headers**  
  Content-Type: application/json
- **Body** `{ status:APPROVED | DENIED | PENDING }`
- **Success Response:**
- **Code:** 204

  - **Content:**
```
{
  message: 'Extension Request _status_ successfully!'
  extensionLog: {<extension_request_log_object>}
}
```

- **Error Response:**
  - **Code** 404
    - **Content** `{ 'statusCode': 404, 'error': 'Not found', 'message': 'No extension requests found' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`
