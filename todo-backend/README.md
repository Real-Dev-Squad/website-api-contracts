# Todo Backend

#### Task Object:

```
{
    id: <ObjectId>
    taskId: <string>
    title: <string>
    description: <string> | null
    priority: LOW | MEDIUM | HIGH
    status: TODO | IN_PROGRESS | DONE
    assignee: {
        id: <string>
        name: <string>
    } | null
    labels: [
        {
            name: <string>
            color: <string>
            createdAt: <datetime> | null
            updatedAt: <datetime> | null
            createdBy: {
                id: <string>
                name: <string>
            } | null
            updatedBy: {
                id: <string>
                name: <string>
            } | null
        }
    ]
    isAcknowledged: <boolean>
    isDeleted: <boolean>
    deferredDetails: {
        deferredAt: <datetime> | null
        deferredTill: <datetime> | null
        deferredBy: <string> | null
    }
    dueDate: <datetime>
    startDate: <datetime> | null
    createdAt: <datetime>
    updatedAt: <datetime> | null
    createdBy: <string>
    updatedBy: <string> | null
}
```

#### DeferredDetails Object:

```
{
    deferredDate: <datetime> | null
    deferredTill: <datetime> | null
    deferredBy: <string> | null
}
```

## **Requests**

|                         Route                          |            Description             |
| :----------------------------------------------------: | :--------------------------------: |
|             [GET /v1/tasks](#get-v1tasks)              |  Return all tasks with pagination  |
|            [POST /v1/tasks](#post-v1tasks)             |          Creates new task          |
|    [PATCH /v1/tasks/{taskId}](#patch-v1taskstaskid) | Partially updates an existing task |
|         [GET /v1/health](#get-v1health)             |       Health check endpoint        |
|      [GET /v1/tasks/{taskId}](#get-v1taskstaskid)      | Retrieves a single task by its ID  |
| [DELETE /v1/tasks/{taskId}](#delete-v1taskstaskid) |      Deletes a specific task      |

## **GET /v1/tasks**

Return all tasks with pagination support

- **Params**  
  None
- **Query**

  - Optional: `page=[integer]` (Page number for pagination, default: 1)
  - Optional: `limit=[integer]` (Number of items per page, default: from Settings)

- **Success Response:**

  - **Code:** 200
  - **Content:**
    ```json
    {
      "statusCode": 200,
      "sucessMessage": "Tasks retrieved successfully",
      "data": [
        {
          "id": "<string>",
          "taskId": "<string>",
          "title": "<string>",
          "description": "<string> | null",
          "priority": "LOW | MEDIUM | HIGH",
          "status": "TODO | IN_PROGRESS | DONE",
          "assignee": {
            "id": "<string>",
            "name": "<string>"
          } | null,
          "isAcknowledged": "<boolean>",
          "labels": [
            {
              "name": "<string>",
              "color": "<string>",
              "createdAt": "<datetime> | null",
              "updatedAt": "<datetime> | null",
              "createdBy": {
                "id": "<string>",
                "name": "<string>"
              } | null,
              "updatedBy": {
                "id": "<string>",
                "name": "<string>"
              } | null
            }
          ],
          "startDate": "<datetime> | null",
          "dueDate": "<datetime> | null",
          "createdAt": "<datetime>",
          "updatedAt": "<datetime> | null",
          "createdBy": {
            "id": "<string>",
            "name": "<string>"
          },
          "updatedBy": {
            "id": "<string>",
            "name": "<string>"
          } | null
        }
      ],
      "links": {
        "next": "<string> | null",
        "prev": "<string> | null"
      },
      "count":"number"
    }
    ```

- **Error Response:**

  - **Code:** 400
  - **Content:**

    ```json
    {
      "status": "validation_failed",
      "statusCode": 400,
      "errorMessage": "Validation Error",
      "errors": [
        {
          "field": "<string>",
          "message": "<string>"
        }
      <!-- Example {
          "field": "page",
        "message": "page must be greater than or equal to 1"
      } -->
      ]
    }
    ```

  - **Code:** 500
  - **Content:**

    ```json
    {
      "statusCode": 500,
      "errorMessage": "An unexpected error occurred",
      "errors": [
        {
          "detail": "Internal server error"
        }
      ]
    }
    ```

## **POST /v1/tasks**

Creates a new task

- **Params**  
  None
- **Body**

  ```json
  {
    "title": "<string>",
    "description": "<string> | null",
    "priority": "LOW | MEDIUM | HIGH",
    "status": "TODO | IN_PROGRESS | DONE",
    "assignee": "<string> | null",
    "labels": ["<ObjectId>"],
    "dueDate": "<datetime>"
  }
  ```

- **Success Response:**

  - **Code:** 201
  - **Content:**
    ```json
    {
      "statusCode": 201,
      "sucessMessage": "Task created successfully",
      "data": {
          "id": "<string>",
          "taskId": "<string>", // "TASK-1001"
          "title": "<string>",
          "description": "<string> | null",
          "priority": "LOW | MEDIUM | HIGH",
          "status": "TODO | IN_PROGRESS | DONE",
          "assignee": {
            "id": "<string>",
            "name": "<string>"
          } | null,
          "isAcknowledged": "<boolean>",
          "labels": [
            {
              "name": "<string>",
              "color": "<string>",
              "createdAt": "<datetime>",
              "updatedAt": null,
              "createdBy": {
                "id": "<string>",
                "name": "<string>"
              },
              "updatedBy": null
            }
          ],
          "startDate": null,
          "dueDate": "<datetime> | null",
          "createdAt": "<datetime>",
          "updatedAt": null,
          "createdBy": {
            "id": "<string>",
            "name": "<string>"
          },
          "updatedBy": null
        }
    }
    ```

- **Error Response:**

  - **Code:** 400
  - **Content:**

    ```json
    {
      "status": "validation_failed"
      "statusCode": 400,
      "errorMessage": "Validation Error",
      "errors": [
        {
          "field": "<string>",
          "message": "<string>"
        }
      ]
    }
    ```

    <!-- Example {
          "field": "title",
          "message": "This field is required"
        } -->

  - **Code:** 500
  - **Content:**
    ```json
    {
      "status": "internal_server_error",
      "statusCode": 500,
      "errorMessage": "An unexpected error occurred",
      "errors": [
        {
          "detail": "Internal server error"
        }
      ]
    }
    ```

## **PATCH /v1/tasks/{taskId}**

Partially updates an existing task by its ID. The `{taskId}` in the path refers to the MongoDB ObjectId of the task.

- **Params**
  - `taskId=[string]` (Path parameter: The MongoDB ObjectId of the task to update)
- **Body**
  _(Provide any subset of the fields below to update. Fields not provided will remain unchanged.)_

  ```json
  {
    "title": "<string>",
    "description": "<string> | null",
    "priority": "LOW | MEDIUM | HIGH",
    "status": "TODO | IN_PROGRESS | DONE | DEFERRED",
    "assignee": "<string (User ID)> | null",
    "labels": ["<ObjectId (Label ID)>"],
    "isAcknowledged": "<boolean>",
    "deferredDetails": {
        "deferredAt": "<datetime> | null",
        "deferredTill": "<datetime> | null"
    } | null,
    "dueAt": "<datetime> | null",
    "startedAt": "<datetime> | null"
  }
  ```

- **Success Response:**

  - **Code:** 200
  - **Content:**
    ```json
    {
      "id": "672f7c5b775ee9f4471ff1dd",
      "displayId": "#2",
      "title": "Refactor Optimize the TaskDetails Component",
      "description": "Refactor Optimize the TaskDetails Component and break it into multiple sub-component",
      "priority": "MEDIUM",
      "status": "IN_PROGRESS",
      "assignee": {
        "id": "qMbT6M2GB65W7UHgJS4g",
        "name": "SYSTEM"
      },
      "isAcknowledged": false,
      "labels": [
        {
          "name": "Label 1",
          "color": "#fa1e4e",
          "createdAt": "2024-11-08T10:14:35",
          "updatedAt": "2025-05-30T14:00:45.458000",
          "createdBy": {
            "id": "qMbT6M2GB65W7UHgJS4g",
            "name": "SYSTEM"
          },
          "updatedBy": null
        },
        {
          "name": "Label 2",
          "color": "#ea1e4e",
          "createdAt": "2024-11-08T10:14:35",
          "updatedAt": "2025-05-30T14:00:45.466000",
          "createdBy": {
            "id": "qMbT6M2GB65W7UHgJS4g",
            "name": "SYSTEM"
          },
          "updatedBy": null
        }
      ],
      "dueAt": "2025-12-08T10:14:35",
      "createdAt": "2024-11-08T10:14:35",
      "updatedAt": "2025-05-31T19:14:55.080000",
      "createdBy": {
        "id": "qMbT6M2GB65W7UHgJS4g",
        "name": "SYSTEM"
      },
      "updatedBy": {
        "id": "system_patch_user",
        "name": "SYSTEM"
      },
      "startedAt": null
    }
    ```

- **Error Response:**

  - **Code:** 400 (Invalid Task ID Format)
  - **Content:**

    ```json
    {
      "statusCode": 400,
      "message": "Please enter a valid Task ID format.",
      "errors": [
        {
          "source": {
            "path": "task_id"
          },
          "title": "Validation Error",
          "detail": "Please enter a valid Task ID format."
        }
      ]
    }
    ```

  - **Code:** 400 (Validation Error in Request Body)
  - **Content:**

    ```json
    {
      "status": "validation_failed",
      "statusCode": 400,
      "errorMessage": "Validation Error",
      "errors": [
        {
          "field": "<field_name>",
          "message": "<error_message>"
        }
      ]
    }
    ```

    <!-- Example:
    {
      "status": "validation_failed",
      "statusCode": 400,
      "errorMessage": "Validation Error",
      "errors": [
        {
          "field": "priority",
          "message": "Priority must be one of LOW, MEDIUM, HIGH."
        }
      ]
    }
    -->

  - **Code:** 404 (Task Not Found)
  - **Content:**

    ```json
    {
      "statusCode": 404,
      "message": "Task with ID 672f7c5b775ee9f4471ff1dc not found.",
      "errors": [
        {
          "source": {
            "path": "task_id"
          },
          "title": "Resource Not Found",
          "detail": "Task with ID 672f7c5b775ee9f4471ff1dc not found."
        }
      ]
    }
    ```

  - **Code:** 500 (Internal Server Error)
  - **Content:**
    ```json
    {
      "status": "internal_server_error",
      "statusCode": 500,
      "errorMessage": "An unexpected error occurred",
      "errors": [
        {
          "detail": "Internal server error"
        }
      ]
    }
    ```

## **DELETE /v1/tasks/{taskId}**

Deletes a task with the given `task_id`.

- **Params**

  - `task_id`: The unique identifier of the task (as a path parameter)

- **Success Response:**

  - **Code:** 204
  - **Content:** None

- **Error Responses:**

  - **Code:** 400

    - **Reason:** Invalid `task_id` format
    - **Content:**
      ```json
      {
        "statusCode": 400,
        "message": "Please enter a valid Task ID format.",
        "errors": [
          {
            "source": {
              "path": "task_id"
            },
            "title": "Validation Error",
            "detail": "Please enter a valid Task ID format."
          }
        ]
      }
      ```

  - **Code:** 404

    - **Reason:** Task with the given `task_id` was not found
    - **Content:**
      ```json
      {
        "statusCode": 404,
        "message": "Task with ID <task_id> not found.",
        "errors": [
          {
            "source": {
              "path": "task_id"
            },
            "title": "Resource Not Found",
            "detail": "Task with ID <task_id> not found."
          }
        ]
      }
      ```

  - **Code:** 500
    - **Reason:** Internal server error
    - **Content:**
      ```json
      {
        "statusCode": 500,
        "message": "An unexpected error occurred",
        "errors": [
          {
            "detail": "Internal server error"
          }
        ]
      }
      ```

## **GET /v1/health**

Health check endpoint

- **Params**  
  None
- **Query**  
  None

- **Success Response:**

  - **Code:** 200
  - **Content:**
    ```json
    {
      "status": "healthy"
    }
    ```

- **Error Response:**
  - **Code:** 500
  - **Content:**
    ```json
    {
      "status": "unhealthy"
    }
    ```

## **GET /v1/tasks/{taskId}**

Retrieves a single task by its ID.

- **Params**
  - `taskId=[string]` (Path parameter: The MongoDB ObjectId of the task to retrieve)
- **Query**
  None

- **Success Response:**

  - **Code:** 200
  - **Content:**
    ```json
    {
      "statusCode": 200,
      "data": {
        "id": "<ObjectId (string)>",
        "displayId": "<string>",
        "title": "<string>",
        "description": "<string> | null",
        "priority": "LOW | MEDIUM | HIGH",
        "status": "TODO | IN_PROGRESS | DONE | DEFERRED",
        "assignee": {
          "id": "<string>",
          "name": "<string>"
        } | null,
        "isAcknowledged": "<boolean>",
        "labels": [
          {
            "name": "<string>",
            "color": "<string>",
            "createdAt": "<datetime> | null",
            "updatedAt": "<datetime> | null",
            "createdBy": { "id": "<string>", "name": "<string>" } | null,
            "updatedBy": { "id": "<string>", "name": "<string>" } | null
          }
        ],
        "startedAt": "<datetime> | null",
        "dueAt": "<datetime> | null",
        "createdAt": "<datetime>",
        "updatedAt": "<datetime> | null",
        "createdBy": {
          "id": "<string>",
          "name": "<string>"
        },
        "updatedBy": {
          "id": "<string>",
          "name": "<string>"
        } | null
      }
    }
    ```

- **Error Response:**

  - **Code:** 400 (Invalid Task ID Format)
  - **Content:**

    ```json
    {
      "statusCode": 400,
      "message": "Please enter a valid Task ID format.",
      "errors": [
        {
          "source": {
            "path": "task_id"
          },
          "title": "Validation Error",
          "detail": "Please enter a valid Task ID format."
        }
      ]
    }
    ```

  - **Code:** 404 (Task Not Found)
  - **Content:**

    ```json
    {
      "statusCode": 404,
      "message": "Task with ID <taskId> not found.",
      "errors": [
        {
          "source": {
            "path": "task_id"
          },
          "title": "Resource Not Found",
          "detail": "Task with ID <taskId> not found."
        }
      ]
    }
    ```

  - **Code:** 500 (Internal Server Error)
  - **Content:**
    ```json
    {
      "status": "internal_server_error",
      "statusCode": 500,
      "errorMessage": "An unexpected error occurred",
      "errors": [
        {
          "detail": "Internal server error"
        }
      ]
    }
    ```

## **Error Codes**

| Status Code |          Description           |
| :---------: | :----------------------------: |
|     200     |            Success             |
|     201     |            Created             |
|     400     | Bad Request (Validation Error) |
|     404     |           Not Found            |
|     500     |     Internal Server Error      |
