# Collection - QrCodeAuth

## API Endpoints

|                                       Route                                        |                                         Description                                          |
| :--------------------------------------------------------------------------------: | :------------------------------------------------------------------------------------------: |
|                      [POST /auth/qr-code-auth](#post-authqr-code-auth)                      |                            Creates a new authentication document.                            |
|                       [GET /auth/qr-code-auth](#get-authqr-code-auth)                       |           Retrieves all the authentication document belonging to a specified user            |
| [PATCH /auth/qr-code-auth](#patch-authqr-code-authauthorization_statusauthorization_status) | Updates the authorization_status field of an existing qr-code-auth document for the specified user. |

## POST /auth/qr-code-auth

    Creates a new qr-code-auth document.

- **Params**  
  None
- **Query**
  None
- **Body**
  - Attributes
    - **user_id** (required, string): Specifies the user id for the document.
    - **device_info** (required, string): Specifies the device information associated with the authentication.
- **Headers**
  - Content-Type: application/json
- **Cookie**
  None
- **Success Response:**
  - **Code:** 201
    - **Content:**
      ```json
      {
        "message": "String",
        "data": {
          "user_id": "String",
          "device_info": "String",
          "authorization_status": "String",
          "access_token": "String"
        }
      }
      ```
- **Error Response:**

  - **Code:** 400

    - **Content:**
      ```json
      {
        "message": "Bad Request"
      }
      ```

  - **Code:** 409

    - **Content:**
      ```json
      {
        "message": "The authentication document has already been created"
      }
      ```

  - **Code:** 500
    - **Content:**
      ```json
      {
        "message": "The server has encountered an unexpected error. Please contact the administrator for more information."
      }
      ```

- **Example for user authentication document creation request:**
  POST /auth/qr-code-auth<br/>
  Content-Type: application/json<br/>
  Request-Body:<br/>

  ```json
  {
    "device_info": "t5k77PHnuDSrgEzvMJAj",
    "user_id": "NLFSj7Kz30oHgolfIZtJ"
  }
  ```

  Response :
  Status 201<br/>
  Content-Type: application/json<br/>

  ```json
  {
    "data": {
      "user_id": "SooJK37gzjIZfFNH0tlL",
      "device_info": "t5k77PHnuDSrgEzvMJAj",
      "authorization_status": "NOT_INIT",
      "access_token": ""
    },
    "message": "authentication document created successfully."
  }
  ```

## GET /auth/qr-code-auth

Retrieves THE authentication document.

- **Params**
  None
- **Query**

  - user_id : Specifies the ID of the User whose authentication document will be retrieved

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
      "message": "String",
      "data": {
        "user_id": "String",
        "device_info": "String",
        "authorization_status": "String",
        "access_token": "String"
      }
    }
    ```

- **Error Response:**

  - **Code:** 400

    - **Content:**
      ```json
      {
        "message": "Bad Request"
      }
      ```

  - **Code:** 404
    - **Content:**
      ```json
      {
        "message": "User with id <:id> does not exist."
      }
      ```
  - **Code:** 500
    - **Content:**
      ```json
      {
        "message": "The server has encountered an unexpected error. Please contact the administrator for more information."
      }
      ```

- **Example:**
  GET /auth/qr-code-auth?user_id=SooJK37gzjIZfFNH0tlL<br/>
  Status: 200 OK<br/>
  ```json
  {
    "message": "Authentication document retrieved successfully.",
    "data": {
      "user_id": "SooJK37gzjIZfFNH0tlL",
      "device_info": "t5k77PHnuDSrgEzvMJAj",
      "authorization_status": "NOT_INIT",
      "access_token": ""
    }
  }
  ```
  GET /auth/qr-code-auth?user_id=invalidUserId<br/>
  Status: 404 Not Found<br/>
  ```json
  {
    "message": "No Authentication authentication found."
  }
  ```
  GET /auth/qr-code-auth?taskId=GTB4UUtlKwGemRN2lwBp11<br/>
  Status: 400 Bad Request
  ```json
  {
    "statusCode": 400,
    "error": "Bad Request",
    "message": "invalid query parameters passed"
  }
  ```
  GET /?user_id=GTB4UUtlKwGemRN2lwBp11
  Status: 500 Internal Server Error
  ```json
  {
    "message": "The server has encountered an unexpected error. Please contact the administrator for more information."
  }
  ```

Sure, here's the updated API contract for the PATCH call:

## PATCH /auth/qr-code-auth/authorization_status/{authorization_status}

Updates the authorization_status field of an existing qr-code-auth document for the specified user.

- **Params**
  - **authorization_status** (required, string): Specifies the authorization status of the user. The possible values are "NOT_INIT" , "AUTHORIZED" , "REJECTED". The default value will be "NOT_INIT"
- **Query**
  None
- **Headers**
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 200
    - **Content:**
      ```json
      {
        "message": "String",
        "data": {
          "user_id": "String",
          "device_info": "String",
          "authorization_status": "String",
          "access_token": "String"
        }
      }
      ```
- **Error Response:**

  - **Code:** 400

    - **Content:**
      ```json
      {
        "message": "Bad Request"
      }
      ```

  - **Code:** 401
    - **Content:**
      ```json
      {
        "statusCode": 401,
        "error": "Unauthorized",
        "message": "Unauthenticated User."
      }
      ```
  - **Code:** 404
    - **Content:**
      ```json
      {
        "message": "Document not found"
      }
      ```
  - **Code:** 500
    - **Content:**
      ```json
      {
        "message": "The server has encountered an unexpected error. Please contact the administrator for more information."
      }
      ```

- **Example for updating an existing authentication document:**
  PATCH /auth/qr-code-auth/authorization_status/AUTHORIZED<br/>
  Content-Type: application/json<br/>
  Request-Body:<br/>
  None
  Response :
  Status 200 OK <br/>
  Content-Type: application/json<br/>

  ```json
  {
    "data": {
      "user_id": "SooJK37gzjIZfFNH0tlL",
      "device_info": "t5k77PHnuDSrgEzvMJAj",
      "authorization_status": "AUTHORIZED",
      "access_token": "NLFSj7Kz30oHgolfIZtJ"
    },
    "message": "Authentication document for user SooJK37gzjIZfFNH0tlL updated successfully."
  }
  ```
