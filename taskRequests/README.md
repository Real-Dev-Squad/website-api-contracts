# Task Request

## **Requests**

|                       Route                       |                   Description                    |
| :-----------------------------------------------: | :----------------------------------------------: |
|      [GET /taskRequests](#get-taskRequests)       |            Returns all task Reqeusts             |
|   [POST /addOrUpdate](#addOrUpdate-taskRequest)   | Creates a task Request or updates the requestors |
| [PATCH /taskRequest/approve](#patch-taskRequests) |          Approves task request to user           |

## **GET /taskRequests**

Returns all tasks to super user.

- **Params**  
  None
- **Query**  
  None
- **Body**  
  None
- **Headers**  
  None
- **Cookie**  
  rds-session: `<JWT>`
- Success Response
  - Code: 200
    Content:
    ```js
    {
      message: "Task requests returned successfully",
      taskRequests: [
        {<task request object>}
      ],
    }
    ```
- Error Response
  - Code: 401
    Content: `{'statusCode': 401, 'error': 'Unauthorized User' }`
  - Code: 500
    Content: `{'statusCode: 500, 'error': 'Internal Server Error'}`

## **POST /addOrUpdate**

Creates a task request.

- **Params**  
  None
- **Query**  
  None
- **Headers**  
  `Content-Type: 'application/json'`
- **Body**
  `{taskId, userId}`
- **Cookie**  
  rds-session: `<JWT>`
- Success Response
  - Code: 201  
    Content:
    ```js
    {
      message: "Task request successfully created",
      taskRequest: {<task request object>}
    }
    ```
  - Code: 200  
    Content:
    ```js
    {
      message: "Task request successfully updated",
      requestors: [<requestors>]
    }
    ```
- Error Response
  - Code: 400
    - If taskId is not provided  
      Content: `{statusCode: 400, error: 'Bad Request', 'message': 'taskId is not provided'}`
    - If userId is not provided  
      Content: `{statusCode: 400, error: 'Bad Request', 'message': 'userId is not provided'}`
  - Code: 401
    Content: `{statusCode: 500, 'error': "Unauthorized User"}`
  - Code: 409
    - If user is already a requestor  
      Content:
      ```js
      {
        'statusCode': 409,
        'error': 'Conflict',
        'message': 'User is already requesting for the task'
      }
      ```
    - If user does not exist
      Content:
      ```js
      {
        'statusCode': 409,
        'error': 'Conflict',
        'message': 'User does not exist'
      }
      ```
    - If user status does not exist
      Content:
      ```js
      {
        'statusCode': 409,
        'error': 'Conflict',
        'message': 'User Status does not exist'
      }
      ```
    - If task doesn't exist
      Content:
      ```js
      {
        'statusCode': 409,
        'error': 'Conflict',
        'message': "Task does not exist"
      }
      ```
  - Code: 500
    Content: `{statusCode: 500, 'error': "Internal server error"}`

## PATCH /taskRequest/approve

Changes status of task request

- **Params**  
  Task Request id
- **Query**  
  None
- **Headers**  
  `Content-Type: 'application/json'`
- **Body**  
  `{taskRequestId, userId}`
- **Success**
  - Code: 200
    Content: `{'message': 'Task successfully assigned to user <username>', 'taskRequest': <task request object>}`
- **Error Response**
  - Code: 400  
    Content: `{'statusCode': 401, 'message': 'Invalid request body'}`
  - Code: 401
    Content: `{'statusCode': 401, 'error': "Unauthorized User"}`
  - Code: 409
    - If user doesn't exist
      ```js
      {
       'statusCode': 409,
       'error': 'Conflict',
       'message': "User does not exist"
      }
      ```
    - If user is OOO
      ```js
      {
       'statusCode': 409,
       'error': 'Conflict',
       'message': "User is currently OOO"
      }
      ```
    - If user is active on another task
      ```js
      {
       'statusCode': 409,
       'error': 'Conflict',
       'message': "User is currently active on another task"
      }
      ```
