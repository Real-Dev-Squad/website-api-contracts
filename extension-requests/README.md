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
|      [GET /extension-requests](#get-extension-requests)      | Returns all extension requests |
|     [POST /extension-requests](#post-tasks)     | Creates new extension request  |

## **GET /extension-requests**

Returns all the extension-requests

- **Params**  
  None
- **Query**  
  None
- **Body**  
  None
- **Headers**  
  None
- **Cookie**  
  None
- **Success Response:**
- **Code:** 200
  - **Content:**

```
{
  message: 'Extension Requests returned successfully!'
  extension_requests: [
           {<extension_request_object>},
           {<extension_request_object>}
         ]
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **POST /extension-requests**

- **Params**  
  None
- **Query**  
  None
- **Headers**  
  Content-Type: application/json
- **Body** `{ <extension_request_object> }`
- **Success Response:**
- **Code:** 200
  - **Content:**

```
{
  message: 'Extension Request created successfully!'
  task: {<extension_request_object>}
  id: <newly created extension request id>
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`
