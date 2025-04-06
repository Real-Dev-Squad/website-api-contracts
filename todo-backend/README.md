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

|             Route             |           Description            |
| :---------------------------: | :------------------------------: |
|  [GET /v1/tasks](#get-tasks)  | Return all tasks with pagination |
| [POST /v1/tasks](#post-tasks) |         Creates new task         |
| [GET /v1/health](#get-health) |      Health check endpoint       |

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
        },
        <!-- Example {
          "field": "title",
          "message": "This field is required"
        } -->
      ]
    }
    ```
  - **Code:** 500
  - **Content:**
    ```json
    {
      "status": "internal_server_error"
      "statusCode": 500,
      "errorMessage": "An unexpected error occurred",
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

## **Error Codes**

| Status Code |          Description           |
| :---------: | :----------------------------: |
|     200     |            Success             |
|     201     |            Created             |
|     400     | Bad Request (Validation Error) |
|     404     |           Not Found            |
|     500     |     Internal Server Error      |
