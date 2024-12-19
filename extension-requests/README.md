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
|     [GET /extension-requests/user/:userId](#get-extension-requests-userId) |   Return extension request with userId   |
|     [POST /extension-requests](#post-extension-requests)     | Creates new extension request  |
|     [PATCH /extension-requests/:id](#patch-extension-requestsid) |   Updates extension request   |
|     [PATCH /extension-requests/:id/status](#patch-extension-requests-id-status) |   APPROVE or DENY an extension request   |

## **GET /extension-requests**

Return all the extension-requests

- **Params**  
  None
- **Query**  
  - Optional: `dev=[boolean]` (`dev` is passed to get all tasks in the developer mode with features that are flagged)
  - Optional: `q=[string]` (`q` can have the following values)
      - Optional: `status=[string]` (`status` is a case senstive string with one of the following values [APPROVED,PENDING, DENIED] )
      - Optional: `taskId=[string]` (`taskId` is the taskId related to the extension request)
      - Optional: `assignee=[string]` (`assignee` is the userId of the assignee )
  - Optional: `size=[integer]` (`size` is the number of tasks requested per page. Range of value is 1-100.)
  - Optional: `cursor=[string]` (`cursor` is id of the document to get next page of results from that document)
  - Optional: `order=[string]` (`order` sorts the response based on created_at time and can have following values [asc,desc])

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
  next: <string>
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


## **GET /extension-requests/user/:userId**

Returns all the extension requests of a authenticated user for a task by userId if query `taskId=<taskId>` is passed, else returns all the extension requests of the user.

- **Params**  
    userId=``<userId>``
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
  - **Code:** 403
    - **Content:** `{ 'statusCode': 403, 'error': 'Forbidden', 'message': 'Forbidden access' }`
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
- **Cookie**  
  rds-session: `<JWT SUPERUSER | APPOWNER >`

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
- **Cookie**  
  rds-session: `<JWT SUPERUSER | APPOWNER >`

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
