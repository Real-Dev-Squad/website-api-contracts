# Progress

## Requests

|              Route               |                         Description                          |
| :------------------------------: | :----------------------------------------------------------: |
| [POST /progress](#post-progress) |                Creates a new progress entry.                 |
|  [GET /progress](#get-progress)  | Retrieves progress entries based on the provided parameters. |

## POST /progress

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

  - **Code:** 401
    - **Content:**
      ```json
      {
        "statusCode": 401,
        "error": "Unauthorized",
        "message": "Unauthenticated User."
      }
      ```
  - **Code:** 400

    - **Content:**
      ```json
      {
        "message": "Bad Request"
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
  POST /progress
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
  Status 400

  ```json
  {
    "message": "User Progress for the day has already been created"
  }
  ```

## GET /progress

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
  GET /progress?type=user
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
  GET /progress?userId=NLFSj7Kz30oHgolfIZtJ
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
  GET /progress?taskId=d757s743d3c54bdf61a6
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
