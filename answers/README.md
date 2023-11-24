# ANSWERS

| Method | Route                        | Description                                                    |
| ------ | ---------------------------- | -------------------------------------------------------------- |
| POST   | [/answers](#post---answers)  | it will be to submit answer by the peers                       |
| PATCH  | [/answers](#patch---answers) | for updating the answers(will primarily be using for approval) |
| GET    | [/answers](#get---answers)   | it will be to get the answers                                  |

### Status description

`PENDING` - peer has submitted the answer but haven't got any review on the answer yet.

`APPROVED` - if host/moderator has approved the answer

`REJECTED` - if host/moderator has rejected the answer

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
  - `n/a`
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
      "status": "<STRING>", //(can be PENDING, APPROVED,REJECTED) //so that HOST | MODERATOR can filter out the answers which has to be shown in word cloud
      "approved_by": "<RDS_USER_ID> || null", //can be of HOST | MODERATOR
      "rejected_by": "<RDS_USER_ID> || null", //can be of HOST | MODERATOR
      "question_id": "<STRING>", //FOREIGN KEY POINTING TO QUESTION COLLECTION
      "answered_by": "<STRING>", //ID OF THE RESPONDER OF THE QUESTION(FROM PEERS COLLECTION)
      "event_id": "<STRING>", //ID OF THE EVENT IN WHICH ANSWER IS GIVEN
      "created_at": "<FIREBASE_TIMESTAMP>",
      "updated_at": "<FIREBASE_TIMESTAMP>"
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

### PATCH - /answers/:answerId

It will be used to update the answers.

- **Params:**
  - None
- **Query**
  - None
- **Body**
  - **`status=[STRING]`** optional
- **Headers**
  - `n/a`
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
📌 `approvedBy || rejectedBy` will be taken from `req.userData` in backend
</aside>

### GET - /answers

It will be used to get all the answers in realtime.

- **Params:**
  - None
- **Query**
  - `status` - `<STRING>`
  - `eventId` - `<STRING>`
  - `questionId` - `<STRING>`
- **Body**
  - `none`
- **Headers**
  - `n/a`
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
        "status": "<STRING>", //(can be PENDING, APPROVED,REJECTED) //so that HOST | MODERATOR can filter out the answers which has to be shown in word cloud
        "approved_by": "<RDS_USER_ID> || null", //can be of HOST | MODERATOR
        "rejected_by": "<RDS_USER_ID> || null", //can be of HOST | MODERATOR
        "question_id": "<STRING>", //FOREIGN KEY POINTING TO QUESTION COLLECTION
        "answered_by": "<STRING>", //ID OF THE RESPONDER OF THE QUESTION(FROM PEERS COLLECTION)
        "event_id": "<STRING>", //ID OF THE EVENT IN WHICH ANSWER IS GIVEN
        "created_at": "<FIREBASE_TIMESTAMP>",
        "updated_at": "<FIREBASE_TIMESTAMP>"
      },
      {
        "id": "<STRING>",
        "answer": "<STRING>",
        "status": "<STRING>", //(can be PENDING, APPROVED,REJECTED) //so that HOST | MODERATOR can filter out the answers which has to be shown in word cloud
        "approved_by": "<RDS_USER_ID> || null", //can be of HOST | MODERATOR
        "rejected_by": "<RDS_USER_ID> || null", //can be of HOST | MODERATOR
        "question_id": "<STRING>", //FOREIGN KEY POINTING TO QUESTION COLLECTION
        "answered_by": "<STRING>", //ID OF THE RESPONDER OF THE QUESTION(FROM PEERS COLLECTION)
        "event_id": "<STRING>", //ID OF THE EVENT IN WHICH ANSWER IS GIVEN
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
