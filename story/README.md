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