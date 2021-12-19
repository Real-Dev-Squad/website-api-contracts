# Story

## Story object

```json
{
    "title": "String",
    "description": "String",
    "status": "Enum",
    "tasks": [
        "<TaskId>",
        "<TaskId>"
    ],
    "featureOwner": "<UserId>",
    "backendEngineer": "<UserId>",
    "frontendEngineer": "<UserId>",
    "StartedOn": "Epoch",
    "EndsOn": "Epoch"
}
```

## **Requests**

|                    Route                    |          Description          |
| :-----------------------------------------: | :---------------------------: |
|          [GET /story](#get-story)           |       Returns all stories     |
|         [POST /story](#post-story)          |       Creates new story       |
|     [PATCH /story/:id](#patch-storyid)      |         Updates story         |
<!-- |      [GET /story/self](#get-storyself)      |  Returns all story of a user  | -->
<!-- | [GET /story/:username](#get-storyusername)  | Returns all story of the user | -->
<!-- | [PATCH /story/self/:id](#patch-storyselfid) |      Changes in own story      | -->

## **GET /story**

Returns all stories

- **Params**  
  None
- **Query**  
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
    "message": "Stories returned successfully!",
    "stories": [
            "<story_object>",
            "<story_object>"
            ]
    }
    ```

- **Error Response:**
  - **Code:** 500
    - **Content:**
        ```json
        {
        "statusCode": 500,
        "error": "Internal Server Error",
        "message": "An internal server error occurred"
        }
        ```

<!-- ## **GET /tasks/self**

Returns all the completed tasks of user if query `completed=true` is passed, else returns all the active and blocked tasks of the user.

- **Params**  
  None
- **Query**  
  completed=[boolean]
- **Body**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 200
    - **Content:**

```
[
  {<task_object>},
  {<task_object>},
  {<task_object>},
  {<task_object>},
  {<task_object>}
]
```

- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 404
    - **Content:** `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'User doesn't exist' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }` -->

<!-- ## **GET /tasks/:username**

Returns all tasks of the requested user.

- **Params**  
  _Required:_ `username=[string]`
- **Query**  
  None
- **Body**  
  None
- **Headers**  
  Content-Type: application/json
- **Success Response:**
  - **Code:** 200
    - **Content:**

```
{
  message: 'Tasks returned successfully!'
  tasks: [
           {<task_object>},
           {<task_object>}
         ]
}
```

- **Error Response:**
  - **Code:** 404
    - **Content:** `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'User doesn't exist' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }` -->

## **POST /story**

- **Params**  
  None
- **Query**  
  None
- **Headers**  
  Content-Type: application/json
- **Body** `{ <story_object> }`
- **Success Response:**
- **Code:** 200
  - **Content:**
    ```json
    {
    "message": "Story created successfully!",
    "story": "<story_object>",
    "id": "<newly created story id>"
    }
    ```

- **Error Response:**
  - **Code:** 500
    - **Content:**
        ```json
        {
        "statusCode": 500,
        "error": "Internal Server Error",
        "message": "An internal server error occurred"
        }
        ```

## **PATCH /story/:id**

- **Params**  
  _Required:_ `id=[string]`

- **Headers**  
  Content-Type: application/json
- **Body** `{ <story_object> }`
- **Success Response:**
- **Code:** 204
  - **Content:** `<No Content>`

- **Error Response:**
  - **Code** 404
    - **Content:**
        ```json
        {
        "statusCode": 404,
        "error": "Not found",
        "message": "No stories found"
        }
        ```
  - **Code:** 500
    - **Content:**
        ```json
        {
        "statusCode": 500,
        "error": "Internal Server Error",
        "message": "An internal server error occurred"
        }
        ```

<!-- ## **PATCH /tasks/self/:id**

- **Params**  
  _Required:_ `id=[string]`

- **Headers**  
  Content-Type: application/json

- **Body**

  ```
  {
    status: <new-status>
    percentCompleted: <number>
  }
  ```

- **Cookie**  
  rds-session: `<JWT>`

- **Success Response:**

  - **Code**: 200
    - **Content:** `{'message': 'Task updated successfully!'}`

- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'User can not be authenticated' }`
  - **Code:** 403
    - **Content:** `{ 'statusCode': 403, 'error': 'Forbidden', 'message':'This task is not assigned to you' }`
  - **Code:** 404
    - **Content:** `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'Task doesn't exist' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }` -->
