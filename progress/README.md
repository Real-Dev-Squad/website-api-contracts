# Progress

## Requests

|                     Route                     |                                          Description                                          |
| :-------------------------------------------: | :-------------------------------------------------------------------------------------------: |
|     [POST /progresses](#post-progresses)      |                                 Creates a new progress entry.                                 |
|      [GET /progresses](#get-progresses)       |                 Retrieves progress entries based on the provided parameters.                  |
| [GET /progresses/range](#get-progressesrange) | Retrieves the progress records for a particular user or task within the specified date range. |

## POST /progresses

    Creates a new progress entry.

- **Params**  
  None
- **Query**
  None
- **Body**  
  Attributes:
  -type (required, string): Specifies the type of the progress entry (e.g., "task", "user").
  -taskId (optional, string): Task ID associated with the progress entry (applicable for task progress only).
  -completed (required, string): Completed portion of the task or progress.
  -planned (required, string): Planned portion of the task or progress.
  -blockers (required, string): Blockers for the progress entry.
  Note : Since this is a authenticated route we don't need to pass userId in the request body.
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 201
    - **Content:**
      ```json
      {
        "message": "String",
        "data": {
          "type": "String",
          "completed": "String",
          "planned": "String",
          "blockers": "String",
          "userId": "String",
          "taskId": "String (for task type)",
          "createdAt": "Timestamp",
          "date": "Timestamp",
          "id": "String"
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

  - **Code:** 401
    - **Content:**
      ```json
      {
        "statusCode": 401,
        "error": "Unauthorized",
        "message": "Unauthenticated User."
      }
      ```
  - **Code:** 404
    - **Content:**
      ```json
      {
        "message": "Task with id <:taskId> does not exist."
      }
      ```
  - **Code:** 409
    - **Content:**
      ```json
      {
        "message": "User Progress for the day has already been created"
      }
      ```
  - **Code:** 500
    - **Content:**
      ```json
      {
        "message": "Internal Server Error"
      }
      ```

- **Example for user progress creation request:**
  POST /progresses
  Content-Type: application/json
  Request-Body:

  ```json
  {
    "type": "user",
    "completed": "Monitoring for issues and performance",
    "planned": "Plan for future enhancements",
    "blockers": "None"
  }
  ```

  Response :
  Status 201
  Content-Type: application/json

  ```json
  {
    "data": {
      "type": "user",
      "completed": "Monitoring for issues and performance",
      "planned": "Plan for future enhancements",
      "blockers": "None",
      "userId": "SooJK37gzjIZfFNH0tlL",
      "createdAt": 1683688367555,
      "date": 1683676800000,
      "id": "t5k77PHnuDSrgEzvMJAj"
    },
    "message": "User Progress document created successfully."
  }
  ```

  for progress document already created
  Status 409 Conflict

  ```json
  {
    "message": "User Progress for the day has already been created"
  }
  ```

  for progress taskId that doesn't exist
  Status 404 Not Found

  ```json
  {
    "message": "Task with id 4ERr8WrizICemnQnMF0U1511 does not exist."
  }
  ```

## GET /progresses

Retrieves progress entries based on the provided parameters.

- **Params**
  None
- **Query**
  Note : Only one query parameter is allowed per request

  - type : Specifies the type of progress to filter (e.g., "task", "user").
  - userId : Specifies the ID of the User whose progress we are interested in
  - taskId : Specifies the ID of the Task whose progress we are interested in

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
      "message": "String",
      "count": "Number",
      "data": [
        {
          "type": "String",
          "completed": "String",
          "planned": "String",
          "blockers": "String",
          "userId": "String",
          "taskId": "String (for task type)",
          "createdAt": "Timestamp",
          "date": "Timestamp",
          "id": "String"
        },
        {
          "type": "String",
          "completed": "String",
          "planned": "String",
          "blockers": "String",
          "userId": "String",
          "taskId": "String (for task type)",
          "createdAt": "Timestamp",
          "date": "Timestamp",
          "id": "String"
        }
      ]
    }
    ```

- **Example:**
  GET /progresses?type=user
  Status: 200 OK
  ```json
  {
    "message": "Progress document retrieved successfully.",
    "count": 2,
    "data": [
      {
        "date": 1683590400000,
        "createdAt": 1683650576402,
        "blockers": "None",
        "completed": "Monitoring for issues and performance",
        "planned": "Plan for future enhancements",
        "type": "user",
        "userId": "NLFSj7Kz30oHgolfIZtJ"
      },
      {
        "createdAt": 1683650564713,
        "blockers": "None",
        "completed": "Implemented error handling for API endpoints",
        "planned": "Add unit tests for backend",
        "type": "user",
        "userId": "SooJK37gzjIZfFNH0tlL",
        "date": 1683417600000
      }
    ]
  }
  ```
  GET /progresses?userId=NLFSj7Kz30oHgolfIZtJ
  Status: 200 OK
  ```json
  {
    "message": "Progress document retrieved successfully.",
    "count": 1,
    "data": [
      {
        "date": 1683590400000,
        "createdAt": 1683650576402,
        "blockers": "None",
        "completed": "Monitoring for issues and performance",
        "planned": "Plan for future enhancements",
        "type": "user",
        "userId": "NLFSj7Kz30oHgolfIZtJ"
      }
    ]
  }
  ```
  GET /progresses?taskId=d757s743d3c54bdf61a6
  Status: 200 OK
  ```json
  {
    "message": "Progress document retrieved successfully.",
    "count": 1,
    "data": [
      {
        "date": 1683676800000,
        "createdAt": 1683690250824,
        "blockers": "None",
        "completed": "Monitoring for issues and performance",
        "planned": "Plan for future enhancements",
        "type": "task",
        "userId": "SooJK37gzjIZfFNH0tlL",
        "taskId": "d757s743d3c54bdf61a6"
      }
    ]
  }
  ```
  GET /?userId=invalidUserId
  Status: 404 Not Found
  ```json
  {
    "message": "No progress records found."
  }
  ```
  GET /?taskId=invalidTaskId
  Status: 404 Not Found
  ```json
  {
    "message": "No progress records found."
  }
  ```

## GET /progresses/range

Retrieves the progress records for a particular user or task within the specified date range, from start to end date.

- **Params**
  None
- **Query**
  Note : Only one in userId or taskId parameter is allowed per request

  - userId : Specifies the ID of the User whose progress we are interested in
  - taskId : Specifies the ID of the Task whose progress we are interested in
  - startDate : Specifies the start date of the date range in ISO 8601 format (e.g. "2023-05-15"). Only progress data on or after this date will be included in the results.
  - endDate : Specifies the end date of the date range in ISO 8601 format (e.g. "2023-05-24"). Only progress data on or before this date will be included in the results.
    Note: The date should be in the ISO 8601 format i.e YYYY-MM-DD

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
      "message": "Progress document retrieved successfully.",
      "data": {
        "startDate": "YYYY-MM-DD",
        "endDate": "YYYY-MM-DD",
        "progressRecords": {
          "YYYY-MM-DD": "boolean",
          "YYYY-MM-DD": "boolean",
          "YYYY-MM-DD": "boolean"
        }
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
        "message": "No progress records found."
      }
      ```
  - **Code:** 500
    - **Content:**
      ```json
      {
        "message": "Internal Server Error"
      }
      ```

- **Example:**
  GET /progresses/range?userId=SooJK37gzjIZfFNH0tlL&startDate=2023-05-01&endDate=2023-05-10
  Status: 200 OK
  Note: The date passe in the query params is of ISO 8601 format i.e YYYY-MM-DD
  ```json
  {
    "message": "Progress document retrieved successfully.",
    "data": {
      "startDate": "2023-05-01",
      "endDate": "2023-05-10",
      "progressRecords": {
        "2023-05-01": true,
        "2023-05-02": true,
        "2023-05-03": true,
        "2023-05-04": true,
        "2023-05-05": true,
        "2023-05-06": true,
        "2023-05-07": true,
        "2023-05-08": false,
        "2023-05-09": true,
        "2023-05-10": true
      }
    }
  }
  ```
  GET /?userId=GTB4UUtlKwGemRN2lwBp11
  Status: 400 Bad Request
  ```json
  {
    "statusCode": 400,
    "error": "Bad Request",
    "message": "Start date and End date is mandatory."
  }
  ```
  GET /?userId=invalidUserId&startDate=2023-05-01&endDate=2023-05-15
  Status: 404 Not Found
  ```json
  {
    "message": "User with id invalidUserId does not exist."
  }
  ```
  GET /?userId=GTB4UUtlKwGemRN2lwBp11&startDate=2023-05-01&endDate=2023-05-15
  Status: 404 Not Found
  ```json
  {
    "message": "No progress records found."
  }
  ```
  GET /?userId=GTB4UUtlKwGemRN2lwBp11&startDate=2023-05-01&endDate=2023-05-15
  Status: 500 Internal Server Error
  ```json
  {
    "message": "Internal Server Error."
  }
  ```
