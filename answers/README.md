# ANSWERS

| Method | Route                        | Description                                                    |
| ------ | ---------------------------- | -------------------------------------------------------------- |
| POST   | [/answers](#post---answers)  | it will be to submit answer by the peers                       |
| PATCH  | [/answers](#patch---answers) | for updating the answers(will primarily be using for approval) |
| GET    | [/answers](#get---answers)   | it will be to get the answers                                  |

### POST - /answers

It will be used for peers to answer questions.

- **Params:**
  - None
- **Query**
  - None
- **Body**
  - **`answer=[STRING]`** required - (The answer text)
  - **`answeredBy=[STRING]`** required (The ID of the peer who answered the question.)
  - **`eventId=[STRING]`** required (The ID of the event where the question was asked.)
  - **`questionId=[STRING]`** required (The ID of the question for which the answer is.)
- **Headers**
  - Authorization: Bearer <management_token>
- **Cookie**
  - `none`
- **Middlewares**
  - `none`
- **Success Response:**
  - **Status Code:** 201 CREATED
  - **Content:**
  ```json
  {
    "message": "Answer submitted successfully",
    "data": {
      "id": "<STRING>",
      "answer": "<STRING>",
      "is_approved": "<BOOLEAN>", //so that HOST | MODERATOR can filter out the answers which has to be shown in word cloud
      "approved_by": "<RDS_USER_ID>", //can be of HOST | MODERATOR
      "question_id": "<STRING>", //FOREIGN KEY POINTING TO QUESTION COLLECTION
      "answered_by": "<STRING>", //ID OF THE RESPONDER OF THE QUESTION(FROM PEERS COLLECTION)
      "event_id": "<STRING>", //ID OF THE EVENT IN WHICH ANSWER IS GIVEN
      "timestamps": {
        "created_at": "<FIREBASE_TIMESTAMP>",
        "updated_at": "<FIREBASE_TIMESTAMP>"
      }
    }
  }
  ```
- **Error Response:**
  - **Code:** 500 INTERNAL_SERVER_ERROR
    - **Content:**
    ```json
    {
      "statusCode": 500,
      "error": "Internal Server Error",
      "message": "An internal server error occurred"
    }
    ```

### PATCH - /answers

It will be used to update the answers.

- **Params:**
  - None
- **Query**
  - None
- **Body**
  - **`isApproved=[BOOLEAN]`** optional
- **Headers**
  - Authorization: Bearer <management_token>
- **Cookie**
  - `rds-session`
- **Middlewares**
  - `authorizeRoles([SUPERUSER, MEMBER])`
- **Success Response:**
  - **Status Code:** 204 No content
  - **Content: n/a**
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

<aside>
📌 `approvedBy` will be taken from `req.userData` in backend
</aside>

### GET - /answers

It will be used to get all the answers in realtime.

- **Params:**
  - None
- **Query**
  - `isApproved` - `<BOOLEAN>`
  - `eventId` - `<STRING>`
  - `questionId` - `<STRING>`
- **Body**
  - `none`
- **Headers**
  - Authorization: Bearer <management_token>
  - Content-Type: 'text/event-stream'
  - Connection: 'keep-alive'
  - Cache-Control: 'no-cache'
- **Cookie**
  - `none`
- **Middlewares**
  - `none`
- **Success Response:**
  - **Status Code:** 200 OK
  - **Content:**
  ```json
  {
    "message": "Answers returned successfully",
    "data": [
      {
        "id": "<STRING>",
        "answer": "<STRING>",
        "is_approved": "<BOOLEAN>", //so that HOST | MODERATOR can filter out the answers which has to be shown in word cloud
        "approved_by": "<RDS_USER_ID>", //can be of HOST | MODERATOR
        "question_id": "<STRING>", //FOREIGN KEY POINTING TO QUESTION COLLECTION
        "event_id": "<STRING>", //ID OF THE EVENT IN WHICH ANSWER IS GIVEN
        "answered_by": "<STRING>", //ID OF THE RESPONDER OF THE QUESTION(FROM PEERS COLLECTION)
        "created_at": "<FIREBASE_TIMESTAMP>",
        "updated_at": "<FIREBASE_TIMESTAMP>"
      },
      {
        "id": "<STRING>",
        "answer": "<STRING>",
        "is_approved": "<BOOLEAN>", //so that HOST | MODERATOR can filter out the answers which has to be shown in word cloud
        "approved_by": "<RDS_USER_ID>", //can be of HOST | MODERATOR
        "question_id": "<STRING>", //FOREIGN KEY POINTING TO QUESTION COLLECTION
        "event_id": "<STRING>", //ID OF THE EVENT IN WHICH ANSWER IS GIVEN
        "answered_by": "<STRING>", //ID OF THE RESPONDER OF THE QUESTION(FROM PEERS COLLECTION)
        "created_at": "<FIREBASE_TIMESTAMP>",
        "updated_at": "<FIREBASE_TIMESTAMP>"
      }
    ]
  }
  ```
- **Error Response:**
  - **Code:** 500 INTERNAL_SERVER_ERROR
    - **Content:**
    ```json
    {
      "statusCode": 500,
      "error": "Internal Server Error",
      "message": "An internal server error occurred"
    }
    ```
