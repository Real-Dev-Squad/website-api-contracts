# API Contracts for trackedProgresses Collection

## Requests

|                           Route                            |                            Description                             |
| :--------------------------------------------------------: | :----------------------------------------------------------------: |
|     [POST /trackedProgresses](#post-trackedprogresses)     |             Creates a new trackedProgresses document.              |
| [GET /trackedProgresses](#get-trackedprogressestypetypeid) | Retrieves the trackedProgresses document based on path parameters. |

## POST /trackedProgresses

    Creates a new trackedProgresses entry.

- **Params**  
  None
- **Query**
  None
- **Body**  
  Attributes:
  -type (required, string): Specifies the type of the trackedProgresses entry (e.g., "task", "user").
  -taskId (optional, string): Task ID associated with the trackedProgresses entry (applicable for type task).
  -userId (optional, string): Task ID associated with the trackedProgresses entry (applicable for type user).
  -currentlyTracked (required, boolean): Whether the trackedProgresses entry is currently tracked
  -frequency (optional, positive integer): The frequency of the trackedProgresses entry (default is 1 if not specified for task, for user its always 1).
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 201
    - **Content:**
      ```json
      {
        "message": "string",
        "data": {
          "id": "string",
          "type": "string",
          "userId": "string (for type user)",
          "taskId": "string (for type task)",
          "currentlyTracked": "boolean",
          "frequency": "positive integer",
          "createdAt": "ISO 8601 format Timestamp",
          "updatedAt": "ISO 8601 format Timestamp"
        }
      }
      ```
- **Error Response:**

  - **Code:** 400

    - **Content:**
      ```json
      {
        "message": "Bad Request."
      }
      ```

  - **Code:** 401

    - **Content:**
      ```json
      {
        "message": "Unauthorized User."
      }
      ```

  - **Code:** 403

    - **Content:**
      ```json
      {
        "message": "You do not have permission to access this resource."
      }
      ```

  - **Code:** 404

    - **Content:**
      ```json
      {
        "message": "User/Task id <:id> does not exist."
      }
      ```

  - **Code:** 409

    - **Content:**
      ```json
      {
        "message": "User/task <:id> is already being tracked."
      }
      ```

  - **Code:** 500

    - **Content:**
      ```json
      {
        "message": "The server has encountered an unexpected error. Please contact the administrator for more information."
      }
      ```

- **Example for trackedProgresses POST request:**<br/>

  POST /trackedProgresses<br/>
  Content-Type: application/json<br/>
  Request-Body:<br/>

  ```json
  {
    "type": "user",
    "userId": "SooJK37gzjIZfFNH0tlL",
    "currentlyTracked": true
  }
  ```

  Response :<br/>
  Status 201<br/>
  Content-Type: application/json<br/>

  ```json
  {
    "message": "User trackedProgresses document created successfully.",
    "data": {
      "id": "en7nlNnpfqoqodcmtMeZ",
      "type": "user",
      "userId": "SooJK37gzjIZfFNH0tlL",
      "frequency": 1,
      "currentlyTracked": true,
      "createdAt": "2023-05-16T14:35:00Z",
      "updatedAt": "2023-05-16T14:35:00Z"
    }
  }
  ```

  POST /trackedProgresses<br/>
  Content-Type: application/json<br/>
  Request-Body:<br/>

  ```json
  {
    "type": "task",
    "taskId": "SooJK37gzjIZfFNH0tlL",
    "currentlyTracked": true,
    "frequency": 2
  }
  ```

  Response :<br/>
  Status 201<br/>
  Content-Type: application/json<br/>

  ```json
  {
    "message": "User trackedProgresses document created successfully.",
    "data": {
      "id": "en7nlNnpfqoqodcmtMeZ",
      "type": "task",
      "taskId": "SooJK37gzjIZfFNH0tlL",
      "frequency": 2,
      "currentlyTracked": true,
      "createdAt": "2023-05-16T14:35:00Z",
      "updatedAt": "2023-05-16T14:35:00Z"
    }
  }
  ```

  For Bad Request from the client<br/>
  Status 400 Bad Request

  ```json
  {
    "message": "Bad Request."
  }
  ```

  For Unauthenticated Request from the client<br/>
  Status 401 Unauthorized

  ```json
  {
    "message": "Unauthorized User."
  }
  ```

  For someone who doesn't have superuser permissions<br/>
  Status 403 Forbidden

  ```json
  {
    "message": "You do not have permission to access this resource."
  }
  ```

  For taskId that doesn't exist<br/>
  Status 404 Not Found

  ```json
  {
    "message": "Task 4ERr8WrizICemnQnMF0U1511 does not exist."
  }
  ```

  For trackedProgresses document already created<br/>
  Status 409 Conflict

  ```json
  {
    "message": "User 4ERr8WrizICemnQnMF0U1511 is already being tracked."
  }
  ```

  For Internal Server Error on the server side<br/>
  Status 500 Internal Server Error

  ```json
  {
    "message": "The server has encountered an unexpected error. Please contact the administrator for more information."
  }
  ```

## GET /trackedProgresses/:type/:typeId

Retrieves the trackedProgress document based on the path parameters.

- **Params**<br/>

  - type: The type of the trackedProgress document (string)
  - typeId: The Id of the type tracked (string)

- **Query**<br/>
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

    ```json
    {
      "message": "string",
      "data": {
        "id": "string",
        "type": "string",
        "userId": "string",
        "taskId": "string",
        "currentlyTracked": "boolean",
        "frequency": "positive integer",
        "createdAt": "Timestamp ISO 8601 format",
        "updatedAt": "Timestamp ISO 8601 format"
      }
    }
    ```

- **Error Response:**

  - **Code:** 400

    - **Content:**
      ```json
      {
        "message": "Bad Request"
      }
      ```

  - **Code:** 404
    - **Content:**
      ```json
      {
        "message": "User / Task with id <:id> does not exist."
      },
      {
        "message": "No trackedProgress records found."
      }
      ```
  - **Code:** 500
    - **Content:**
      ```json
      {
        "message": "The server has encountered an unexpected error. Please contact the administrator for more information."
      }
      ```

- **Example:**

  GET /trackedProgresses/user/SooJK37gzjIZfFNH0tlL<br/>
  Status: 200 OK

  ```json
  {
    "message": "trackedProgress document retrieved successfully.",
    "data": {
      "id": "eBe01VS3oYI2HJuWdRuG",
      "type": "user",
      "userId": "SooJK37gzjIZfFNH0tlL",
      "frequency": 1,
      "currentlyTracked": true,
      "createdAt": "2023-05-16T14:35:00Z",
      "updatedAt": "2023-05-16T14:35:00Z"
    }
  }
  ```

  GET /trackedProgresses/event/hPz5hfWBd9oSwMljGk1s<br/>
  Status: 400 Bad Request

  ```json
  {
    "message": "type can either be user or task"
  }
  ```

  GET /trackedProgresses/user/SooSk37gzjIZfFNH0tlL<br/>
  Status: 404 Not Found

  ```json
  {
    "message": "No progress records found."
  }
  ```

  GET /trackedProgresses/user/invalidUser<br/>
  Status: 404 Not Found

  ```json
  {
    "message": "User invalidUser does not exist."
  }
  ```

  GET /trackedProgresses/user/hPz5hfWBd9oSwMljGk1s<br/>
  Status: 500 Internal Server Error

  ```json
  {
    "message": "The server has encountered an unexpected error. Please contact the administrator for more information."
  }
  ```

## PATCH /trackedProgresses/:type/:id

    Updates an existing trackedProgresses entry.

- **Params**

  - type (required, string): Specifies the type of the trackedProgresses entry (e.g., "task", "user").
  - id (required, string): The ID of the task or user associated with the trackedProgresses entry. i.e taskId or userId

- **Query**<br/>
  None

- **Body**

  - Attributes:
    - currentlyTracked (optional,boolean): Whether the trackedProgresses entry is currently tracked
    - frequency (optional, positive integer): The frequency of the trackedProgresses entry (only applicable for task).

- **Headers**  
  Content-Type: application/json

- **Cookie**  
  rds-session: `<JWT>`

- **Success Response:**

  - **Code:** 200
    - **Content:**
      ```json
      {
        "message": "string",
        "data": {
          "id": "string",
          "type": "string",
          "userId": "string (for type user)",
          "taskId": "string (for type task)",
          "currentlyTracked": "boolean",
          "frequency": "positive integer",
          "createdAt": "ISO 8601 format Timestamp",
          "updatedAt": "ISO 8601 format Timestamp"
        }
      }
      ```

- **Error Response:**

  - **Code:** 400

    - **Content:**
      ```json
      {
        "message": "Bad Request."
      }
      ```

  - **Code:** 401

    - **Content:**
      ```json
      {
        "message": "Unauthorized User."
      }
      ```

  - **Code:** 403

    - **Content:**
      ```json
      {
        "message": "You do not have permission to access this resource."
      }
      ```

  - **Code:** 404

    - **Content:**
      ```json
      {
        "message": "Resource not found."
      }
      ```

  - **Code:** 500

    - **Content:**
      ```json
      {
        "message": "The server has encountered an unexpected error. Please contact the administrator for more information."
      }
      ```

- **Example for trackedProgresses POST request:**<br/>

  PATCH /trackedProgresses/user/SooJK37gzjIZfFNH0tlL<br/>
  Content-Type: application/json<br/>
  Request-Body:<br/>

  ```json
  {
    "currentlyTracked": false
  }
  ```

  Response :<br/>
  Status 200<br/>
  Content-Type: application/json<br/>

  ```json
  {
    "message": "User trackedProgresses document updated successfully.",
    "data": {
      "id": "en7nlNnpfqoqodcmtMeZ",
      "type": "user",
      "userId": "SooJK37gzjIZfFNH0tlL",
      "frequency": 1,
      "currentlyTracked": false,
      "createdAt": "2023-05-16T14:35:00Z",
      "updatedAt": "2023-05-16T14:35:00Z"
    }
  }
  ```

  POST /trackedProgresses/task/SooJK37gzjIZfFNH0tlL<br/>
  Content-Type: application/json<br/>
  Request-Body:<br/>

  ```json
  {
    "currentlyTracked": false
  }
  ```

  Response :<br/>
  Status 201<br/>
  Content-Type: application/json<br/>

  ```json
  {
    "message": "User trackedProgresses document created successfully.",
    "data": {
      "id": "en7nlNnpfqoqodcmtMeZ",
      "type": "task",
      "taskId": "SooJK37gzjIZfFNH0tlL",
      "frequency": 2,
      "currentlyTracked": false,
      "createdAt": "2023-05-16T14:35:00Z",
      "updatedAt": "2023-05-16T14:35:00Z"
    }
  }
  ```

  For Bad Request from the client<br/>
  Status 400 Bad Request

  ```json
  {
    "message": "Bad Request."
  }
  ```

  For Unauthenticated Request from the client<br/>
  Status 401 Unauthorized

  ```json
  {
    "message": "Unauthorized User."
  }
  ```

  For someone who doesn't have superuser permissions<br/>
  Status 403 Forbidden

  ```json
  {
    "message": "You do not have permission to access this resource."
  }
  ```

  For taskId that doesn't exist<br/>
  Status 404 Not Found

  ```json
  {
    "message": "Task 4ERr8WrizICemnQnMF0U1511 does not exist."
  }
  ```

  For trackedProgresses document already created<br/>
  Status 409 Conflict

  ```json
  {
    "message": "User 4ERr8WrizICemnQnMF0U1511 is already being tracked."
  }
  ```

  For Internal Server Error on the server side<br/>
  Status 500 Internal Server Error

  ```json
  {
    "message": "The server has encountered an unexpected error. Please contact the administrator for more information."
  }
  ```
