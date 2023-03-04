# Task Request

## Task Request Object

```

```

## **Requests**

|               Route                             |    Description    |
| :-------------------------------------------:   | :---------------: |
| [GET /taskRequests](#get-taskRequests)          | Returns all tasks |

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
      ```
      {
        message: "Task requests returned successfully",
        taskRequests: [
          {<task request object>}
        ],
      }
      ```
- Error Response
  - Code: 401
    Content: `{'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthorized User' }`
