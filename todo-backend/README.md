# Todo Backend

#### Task Object:

```
{
    id: <ObjectId>
    displayId: <string>
    title: <string>
    description: <string> | null
    priority: LOW | MEDIUM | HIGH
    status: TODO | IN_PROGRESS | DONE
    assignee: <string> | null
    labels: [<ObjectId>]
    isAcknowledged: <boolean>
    isDeleted: <boolean>
    deferredDetails: {
        deferredAt: <datetime> | null
        deferredTill: <datetime> | null
        deferredBy: <string> | null
    }
    dueAt: <datetime>
    startedAt: <datetime> | null
    createdAt: <datetime>
    updatedAt: <datetime> | null
    createdBy: <string>
    updatedBy: <string> | null
}
```

#### DeferredDetails Object:

```
{
    deferredAt: <datetime> | null
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

  - Optional: `page=[integer]` (Page number for pagination, defaults to 1)
  - Optional: `limit=[integer]` (Number of items per page)

- **Success Response:**

  - **Code:** 200
  - **Content:**
    ```json
    {
      "tasks": [
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
      ],
      "links": {
        "next": "<string> | null",
        "prev": "<string> | null"
      }
    }
    ```

- **Error Response:**
  - **Code:** 400
  - **Content:**
    ```json
    {
      "status": "validation_failed",
      "statusCode": 400,
      "message": "Validation Error",
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
    "dueAt": "<datetime>"
  }
  ```

- **Success Response:**

  - **Code:** 201
  - **Content:**
    ```json
    {
      "task": {
          "id": "<string>",
          "displayId": "<string>",
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
      },
    ```

- **Error Response:**
  - **Code:** 400
  - **Content:**
    ```json
    {
      "status": "validation_failed"
      "statusCode": 400,
      "message": "Validation Error",
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
      "status": "internal_server_error"
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

## **Error Codes**

| Status Code |          Description           |
| :---------: | :----------------------------: |
|     200     |            Success             |
|     201     |            Created             |
|     400     | Bad Request (Validation Error) |
|     404     |           Not Found            |
|     500     |     Internal Server Error      |
