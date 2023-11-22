# QUESTIONS - API CONTRACTS

| Method | Route                            | Description                                   |
| ------ | -------------------------------- | --------------------------------------------- |
| POST   | [/questions](#post---questions)  | it will be used for host to ask the question. |
| GET    | [ /questions ](#get---questions) | it will be for users to get the question.     |

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
  - **Status Code:** 201 CREATED
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

### GET - /questions

It will be used to get the questions in realtime.

- **Params:**
  - None
- **Query**
  - `eventId` - `<STRING>`
  - `isNew` - `<BOOLEAN>`
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
  "data": 	{
  		"id": "<STRING>",
  		"question": "<STRING>",
  		"event_id": "EVENT_ID",
  		"is_new": "<BOOLEAN>",//will help us in filtering which question to broadcast/send to connected clients
  		"max_words": "<NUMBER>" //number of words answer can have for this question
  		"created_at": "<FIREBASE_TIMESTAMP>",
  		"updated_at": "<FIREBASE_TIMESTAMP>",
  		"created_by": "STRING" //ID OF THE CREATOR OF THE QUESTION(FROM RDS USER COLLECTION)
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
