# API CONTRACTS

| Method | Route      | Description                                                    |
| ------ | ---------- | -------------------------------------------------------------- |
| POST   | /questions | it will be used for host to ask the question.                  |
| GET    | /questions | it will be for users to get the question.                      |
| POST   | /answers   | it will be to submit questions by the users                    |
| GET    | /answers   | it will be to get the answer                                   |
| PATCH  | /answers   | for updating the answers(will primarily be using for approval) |

### POST - /questions

It will be used for host to ask the question.

- **Params:**
  - None
- **Query**
  - None
- **Body**
  - **`question=[STRING]`** required - (The question text.)
  - **`createdBy=[STRING]`** required (The ID of the user who asked the question.)
  - **`eventId=[STRING]`** required (The ID of the session where the question was asked.)
  - **`maxWords=[NUMBER || null]`** optional
- **Headers**
  - Authorization: Bearer <management_token>
- **Cookie**
  - `rds-session`
- **Middlewares**
  - `authorizeRoles([SUPERUSER, MEMBER])`
- **Success Response:**
  - **Status Code:** 200 OK
  - **Content:**
  ```json
  {
    "message": "Question created successfully",
    "data": {
      "id": "<STRING>",
      "question": "<STRING>",
      "created_by": "<STRING>",
      "event_id": "EVENT_ID",
      "word_limit": "<NUMBER>",
      "timestamps": {
        "created_at": "<FIREBASE_TIMESTAMP>",
        "updated_at": "<FIREBASE_TIMESTAMP>"
      }
    }
  }
  ```
- **Error Response:**
  - **Code:** 401 UNAUTHORIZED
    - **Content:**
    ```json
    {
      "statusCode": 401,
      "error": "Unauthorized",
      "message": "You are not authorized for this action."
    }
    ```
  - **Code:** 500 INTERNAL_SERVER_ERROR
    - **Content:**
    ```json
    {
      "statusCode": 500,
      "error": "Internal Server Error",
      "message": "An internal server error occurred"
    }
    ```
