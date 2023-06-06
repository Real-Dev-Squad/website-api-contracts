# Collection - QrCodeAuth

## API Endpoints

|                                       Route                                        |                                         Description                                          |
| :--------------------------------------------------------------------------------: | :------------------------------------------------------------------------------------------: |
|                      [POST /qr-code-auth](#post-qr-code-auth)                      |                            Creates a new authentication document.                            |
|                       [GET /qr-code-auth](#get-qr-code-auth)                       |           Retrieves all the authentication document belonging to a specified user            |
| [PATCH /qr-code-auth](#patch-qr-code-authuser_iduser_idis_authorizedis_authorized) | Updates the is_authorized field of an existing qr-code-auth document for the specified user. |

## POST /qr-code-auth

    Creates a new qr-code-auth document.

- **Params**  
  None
- **Query**
  None
- **Body**
  - Attributes
    - **device_info** (required, string): Specifies the device information associated with the authentication authentication.
    - **is_authorized** (required, boolean): Indicates whether the authentication document is authorized or not.
    - **access_token** (required, string): Specifies the access token associated with the authentication document.
- **Headers**
  - Content-Type: application/json
- **Cookie**
  - rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 201
    - **Content:**
      ```json
      {
        "message": "String",
        "data": {
          "user_id": "String",
          "device_info": "String",
          "is_authorized": "Boolean",
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
  POST /qr-code-auth<br/>
  Content-Type: application/json<br/>
  Request-Body:<br/>

  ```json
  {
    "device_info": "t5k77PHnuDSrgEzvMJAj",
    "is_authorized": true,
    "access_token": "NLFSj7Kz30oHgolfIZtJ"
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
      "is_authorized": true,
      "access_token": "NLFSj7Kz30oHgolfIZtJ"
    },
    "message": "authentication document created successfully."
  }
  ```

## GET /qr-code-auth

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
        "is_authorized": "Boolean",
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
  GET /qr-code-auth?user_id=SooJK37gzjIZfFNH0tlL<br/>
  Status: 200 OK<br/>
  ```json
  {
    "message": "Authentication document retrieved successfully.",
    "data": {
      "user_id": "SooJK37gzjIZfFNH0tlL",
      "device_info": "t5k77PHnuDSrgEzvMJAj",
      "is_authorized": true,
      "access_token": "NLFSj7Kz30oHgolfIZtJ"
    }
  }
  ```
  GET /qr-code-auth?user_id=invalidUserId<br/>
  Status: 404 Not Found<br/>
  ```json
  {
    "message": "No Authentication authentication found."
  }
  ```
  GET /qr-code-auth?taskId=GTB4UUtlKwGemRN2lwBp11<br/>
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

## PATCH /qr-code-auth/is_authorized/{is_authorized}

Updates the is_authorized field of an existing qr-code-auth document for the specified user.

- **Params**
  - **is_authorized** (required, boolean): Specifies whether the authentication document is authorized or not.
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
          "is_authorized": "Boolean",
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
  PATCH /qr-code-auth/is_authorized/true<br/>
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
      "is_authorized": true,
      "access_token": "NLFSj7Kz30oHgolfIZtJ"
    },
    "message": "Authentication document for user SooJK37gzjIZfFNH0tlL updated successfully."
  }
  ```
