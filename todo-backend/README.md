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

|                                  Route                                   |            Description             |
| :----------------------------------------------------------------------: | :--------------------------------: |
|                      [GET /v1/tasks](#get-v1tasks)                       |  Return all tasks with pagination  |
|                     [POST /v1/tasks](#post-v1tasks)                      |          Creates new task          |
|             [PATCH /v1/tasks/{taskId}](#patch-v1taskstaskid)             | Partially updates an existing task |
| [PATCH /v1/tasks/{taskId}?action=defer](#patch-v1taskstaskidactiondefer) |   Defers a task to a future date   |
|                     [GET /v1/health](#get-v1health)                      |       Health check endpoint        |
|               [GET /v1/tasks/{taskId}](#get-v1taskstaskid)               | Retrieves a single task by its ID  |
|            [DELETE /v1/tasks/{taskId}](#delete-v1taskstaskid)            |      Deletes a specific task       |
|                     [GET /v1/labels](#get-v1labels)                      | Return all labels with pagination  |

## **GET /v1/tasks**

Return all tasks with pagination and sorting support

- **Params**  
  None
- **Query**

  - Optional: `page=[integer]` (Page number for pagination, default: 1)
  - Optional: `limit=[integer]` (Number of items per page, default: from Settings)

- Optional: `sort_by=[string]` (Field to sort by: "createdAt", "dueAt", "priority", "assignee.name"; default: "createdAt")

- Optional: `order=[string]` (Sort order: "asc", "desc", default varies by field: createdAt: "desc", dueAt: "asc", priority: "desc", assignee: "asc")

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
      "id": "<string>",
      "displayId": "<string>",
      "title": "<string>",
      "description": "<string>",
      "priority": "LOW | MEDIUM | HIGH",
      "status": "TODO | IN_PROGRESS | DONE ",
      "assignee": {
        "id": "<string>",
        "name": "<string>"
      },
      "isAcknowledged": "<boolean>",
      "labels": [
        {
          "name": "<string>",
          "color": "<string>",
          "createdAt": "<datetime>",
          "updatedAt": "<datetime>| null",
          "createdBy": {
            "id": "<string>",
            "name": "<string>"
          },
          "updatedBy": {
            "id": "<string>",
            "name": "<string>"
          }
        },
        {
          "name": "<string>",
          "color": "<string>",
          "createdAt": "<datetime>",
          "updatedAt": "<datetime>| null",
          "createdBy": {
            "id": "<string>",
            "name": "<string>"
          },
          "updatedBy": {
            "id": "<string>",
            "name": "<string>"
          }
        }
      ],
      "dueAt": "<datetime>| null",
      "createdAt": "<datetime>",
      "updatedAt": "<datetime>",
      "createdBy": {
        "id": "<string>",
        "name": "<string>"
      },
      "updatedBy": {
        "id": "<string>",
        "name": "<string>"
      },
      "startedAt": "<datetime>| null"
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

  - **Code:** 404 (Task Not Found)
  - **Content:**

    ```json
    {
      "statusCode": 404,
      "message": "Task with ID taskId not found.",
      "errors": [
        {
          "source": {
            "path": "task_id"
          },
          "title": "Resource Not Found",
          "detail": "Task with ID taskId not found."
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

## **PATCH /v1/tasks/{taskId}?action=defer**

Defers a task to a future date. This is an action performed on the Task resource.

- **Params**
  - `taskId=[string]` (Path parameter: The MongoDB ObjectId of the task to defer)
- **Query**

  - `action=defer` (Required action parameter)

- **Body**

  ```json
  {
    "deferredTill": "<datetime>"
  }
  ```

- **Success Response:**

  - **Code:** 200
  - **Content:** The full updated task object with status as `DEFERRED` and `deferredDetails` populated.
    ```json
    {
      "id": "<string>",
      "displayId": "<string>",
      "title": "<string>",
      "description": "<string> | null",
      "priority": "LOW | MEDIUM | HIGH",
      "status": "TODO | IN_PROGRESS | DONE",
      "assignee": {
        "id": "<string>",
        "name": "<string>"
      },
      "isAcknowledged": "<boolean>",
      "labels": [
        {
          "name": "<string>",
          "color": "<string>",
          "createdAt": "<datetime>",
          "updatedAt": "<datetime>| null",
          "createdBy": {
            "id": "<string>",
            "name": "<string>"
          },
          "updatedBy": {
            "id": "<string>",
            "name": "<string>"
          }
        }
      ],
      "deferredDetails": {
        "deferredAt": "<datetime>",
        "deferredTill": "<datetime>",
        "deferredBy": {
          "id": "<string>",
          "name": "<string>"
        }
      },
      "dueAt": "<datetime>",
      "createdAt": "<datetime>",
      "updatedAt": "<datetime>",
      "createdBy": {
        "id": "<string>",
        "name": "<string>"
      },
      "updatedBy": {
        "id": "<string>",
        "name": "<string>"
      },
      "startedAt": "<datetime> | null"
    }
    ```

- **Error Response:**

  - **Code:** 400 (Validation Error in Request Body)
  - **Content:**

    ```json
    {
      "status": "validation_failed",
      "statusCode": 400,
      "errorMessage": "Validation Error",
      "errors": [
        {
          "field": "deferredTill",
          "message": "Datetime has wrong format. Use one of these formats instead: YYYY-MM-DDThh:mm[:ss[.uuuuuu]][+HH:MM|-HH:MM|Z]."
        }
      ]
    }
    ```

  - **Code:** 404 (Task Not Found)
  - **Content:**

    ```json
    {
      "statusCode": 404,
      "message": "Task with ID taskId not found.",
      "errors": [
        {
          "source": {
            "path": "task_id"
          },
          "title": "Resource Not Found",
          "detail": "Task with ID taskId not found."
        }
      ]
    }
    ```

  - **Code:** 409 (State Conflict)
  - **Content:**

    ```json
    {
      "statusCode": 409,
      "message": "Cannot defer a task that is already done.",
      "errors": [
        {
          "source": {
            "path": "task_id"
          },
          "title": "State Conflict",
          "detail": "Cannot defer a task that is already done."
        }
      ]
    }
    ```

  - **Code:** 422 (Unprocessable Entity)
  - **Content:**

    ```json
    {
      "status": "unprocessable_entity",
      "statusCode": 422,
      "message": "Cannot defer a task less than 20 days before the due date.",
      "errors": [
        {
          "source": {
            "parameter": "deferredTill"
          },
          "title": "Validation Error",
          "detail": "Cannot defer a task less than 20 days before the due date."
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

## **GET /v1/labels**

Return all labels with pagination support

- **Params**  
  None
- **Query**

  - Optional: `page=[integer]` (Page number for pagination, default: 1)
  - Optional: `limit=[integer]` (Number of items per page, default: 10)
  - Optional: `search=[string]` (Search query string; default: "")

- **Success Response:**

  - **Code:** 200
  - **Content:**
    ```json
    {
      "links": {
        "next": "<string> | null",
        "prev": "<string> | null"
      },
      "labels": [
        {
          "id": "<string>",
          "name": "<string>",
          "color": "<string>"
        }
      ],
      "total": "number",
      "page": "number",
      "limit": "number"
    }
    ```

- **Error Response:**

  - **Code:** 400
  - **Content:**

    ```json
    {
      "statusCode": 400,
      "message": "<string>",
      "errors": [
        {
          "source": {
            "parameter": "<string>"
          },
          "detail": "<string>"
        }
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

## **Error Codes**

| Status Code |          Description           |
| :---------: | :----------------------------: |
|     200     |            Success             |
|     201     |            Created             |
|     400     | Bad Request (Validation Error) |
|     404     |           Not Found            |
|     500     |     Internal Server Error      |
|     422     |      Unprocessable Entity      |
