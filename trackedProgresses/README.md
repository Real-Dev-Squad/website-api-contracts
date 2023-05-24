# API Contracts for trackedProgresses Collection

## Requests

|                      Route                       |                   Description                    |
| :----------------------------------------------: | :----------------------------------------------: |
|          [POST /monitor](#post-monitor)          |             Creates a new document.              |
|           [GET /monitor](#get-monitor)           | Retrieves the document based on path parameters. |
| [PATCH /monitor/:type/:id](#patch-monitortypeid) |          Updates an existing document.           |

## POST /monitor

    Creates a new document.

- **Params**  
  None

- **Query**
  None

- **Body**

  - Attributes:
    - type (required, string): Specifies the type of the progresses entry (e.g., "task", "user").
    - taskId (optional, string): Task ID associated with the progresses entry (applicable for type task).
    - userId (optional, string): Task ID associated with the progresses entry (applicable for type user).
    - monitored (required, boolean): Whether the progresses entry is currently monitored
    - frequency (optional, positive integer): The frequency of the progresses entry (default is 1 if not specified for task, for user its always 1).

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
          "monitored": "boolean",
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
        "message": "Resource is already being monitored."
      }
      ```

  - **Code:** 500

    - **Content:**
      ```json
      {
        "message": "The server has encountered an unexpected error. Please contact the administrator for more information."
      }
      ```

- **Example for POST request:**<br/>

  POST /monitor<br/>
  Content-Type: application/json<br/>
  Request-Body:<br/>

  ```json
  {
    "type": "user",
    "userId": "SooJK37gzjIZfFNH0tlL",
    "monitored": true
  }
  ```

  Response :<br/>
  Status 201<br/>
  Content-Type: application/json<br/>

  ```json
  {
    "message": "Document created successfully.",
    "data": {
      "id": "en7nlNnpfqoqodcmtMeZ",
      "type": "user",
      "userId": "SooJK37gzjIZfFNH0tlL",
      "frequency": 1,
      "monitored": true,
      "createdAt": "2023-05-16T14:35:00Z",
      "updatedAt": "2023-05-16T14:35:00Z"
    }
  }
  ```

  POST /monitor<br/>
  Content-Type: application/json<br/>
  Request-Body:<br/>

  ```json
  {
    "type": "task",
    "taskId": "SooJK37gzjIZfFNH0tlL",
    "monitored": true,
    "frequency": 2
  }
  ```

  Response :<br/>
  Status 201<br/>
  Content-Type: application/json<br/>

  ```json
  {
    "message": "Document created successfully.",
    "data": {
      "id": "en7nlNnpfqoqodcmtMeZ",
      "type": "task",
      "taskId": "SooJK37gzjIZfFNH0tlL",
      "frequency": 2,
      "monitored": true,
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

  For document already created<br/>
  Status 409 Conflict

  ```json
  {
    "message": "Resource is already being monitored."
  }
  ```

  For Internal Server Error on the server side<br/>
  Status 500 Internal Server Error

  ```json
  {
    "message": "The server has encountered an unexpected error. Please contact the administrator for more information."
  }
  ```

## GET /monitor

Retrieves the document based on the path parameters.

- **Params**<br/>
  None

- **Query**<br/>

  - type: The type of the document (string)
  - monitored: If the user/task is being tracked (boolean)
  - userId: The ID of the user
  - taskId: The ID of the task

- **Body**  
  None

- **Headers**  
  None

- **Cookie**  
  None

- **Success Response:**

  - **Code:** 200

    - **Content:**
      For multiple task/user

    ```json
    {
      "message": "string",
      "data": [
        {
          "id": "string",
          "type": "string",
          "userId": "string",
          "taskId": "string",
          "monitored": "boolean",
          "frequency": "positive integer",
          "createdAt": "Timestamp ISO 8601 format",
          "updatedAt": "Timestamp ISO 8601 format"
        }
      ]
    }
    ```

    For single task/user

    ```json
    {
      "message": "string",
      "data": {
        "id": "string",
        "type": "string",
        "userId": "string",
        "taskId": "string",
        "monitored": "boolean",
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
        "message": "No records found."
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

  GET /monitor?userId=SooJK37gzjIZfFNH0tlL<br/>
  Status: 200 OK

  ```json
  {
    "message": "monitored document retrieved successfully.",
    "data": {
      "id": "eBe01VS3oYI2HJuWdRuG",
      "type": "user",
      "userId": "SooJK37gzjIZfFNH0tlL",
      "frequency": 1,
      "monitored": true,
      "createdAt": "2023-05-16T14:35:00Z",
      "updatedAt": "2023-05-16T14:35:00Z"
    }
  }
  ```

  GET /monitor?type=task<br/>
  Status: 200 OK

  ```json
  {
    "message": "Resource retrieved successfully.",
    "data": [
      {
        "id": "7GW0AoUagE662drETrmM",
        "createdAt": "2023-05-23T16:35:33.588Z",
        "type": "task",
        "taskId": "ViMfxn7kDi5eRZD15p57",
        "monitored": false,
        "frequency": 2,
        "updatedAt": "2023-05-23T16:38:56.458Z"
      },
      {
        "id": "eKwSATvQyO7fTs3xhEoK",
        "createdAt": "2023-05-23T03:37:20.627Z",
        "type": "task",
        "taskId": "y7pFElvT5gBo40cD9NxY",
        "frequency": 5,
        "monitored": true,
        "updatedAt": "2023-05-23T16:38:08.200Z"
      }
    ]
  }
  ```

  GET /monitor?type=event<br/>
  Status: 400 Bad Request

  ```json
  {
    "message": "Type field is restricted to either 'user' or 'task'."
  }
  ```

  GET /monitor?userId=SooSk37gzjIZfFNH0tlL<br/>
  Status: 404 Not Found

  ```json
  {
    "message": "No progress records found."
  }
  ```

  GET /monitor?userId=invalidUser<br/>
  Status: 404 Not Found

  ```json
  {
    "message": "User invalidUser does not exist."
  }
  ```

  GET /monitor?userId=hPz5hfWBd9oSwMljGk1s<br/>
  Status: 500 Internal Server Error

  ```json
  {
    "message": "The server has encountered an unexpected error. Please contact the administrator for more information."
  }
  ```

## PATCH /monitor/:type/:id

    Updates an existing entry.

- **Params**

  - type (required, string): Specifies the type of the progresses entry (e.g., "task", "user").
  - id (required, string): The ID of the task or user associated with the progresses entry. i.e taskId or userId

- **Query**<br/>
  None

- **Body**

  - Attributes:
    - monitored (optional,boolean): Whether the progresses entry is currently monitored
    - frequency (optional, positive integer): The frequency of the progresses entry (only applicable for task).

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
          "monitored": "boolean",
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

- **Example for POST request:**<br/>

  PATCH /monitor/user/SooJK37gzjIZfFNH0tlL<br/>
  Content-Type: application/json<br/>
  Request-Body:<br/>

  ```json
  {
    "monitored": false
  }
  ```

  Response :<br/>
  Status 200<br/>
  Content-Type: application/json<br/>

  ```json
  {
    "message": "Document updated successfully.",
    "data": {
      "id": "en7nlNnpfqoqodcmtMeZ",
      "type": "user",
      "userId": "SooJK37gzjIZfFNH0tlL",
      "frequency": 1,
      "monitored": false,
      "createdAt": "2023-05-16T14:35:00Z",
      "updatedAt": "2023-05-16T14:35:00Z"
    }
  }
  ```

  POST /monitor/task/SooJK37gzjIZfFNH0tlL<br/>
  Content-Type: application/json<br/>
  Request-Body:<br/>

  ```json
  {
    "monitored": false
  }
  ```

  Response :<br/>
  Status 201<br/>
  Content-Type: application/json<br/>

  ```json
  {
    "message": "Document created successfully.",
    "data": {
      "id": "en7nlNnpfqoqodcmtMeZ",
      "type": "task",
      "taskId": "SooJK37gzjIZfFNH0tlL",
      "frequency": 2,
      "monitored": false,
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

  For Internal Server Error on the server side<br/>
  Status 500 Internal Server Error

  ```json
  {
    "message": "The server has encountered an unexpected error. Please contact the administrator for more information."
  }
  ```
