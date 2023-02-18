# External Accounts

## **Requests**

|                       Route                        |                         Description                         |
| :------------------------------------------------: | :---------------------------------------------------------: |
| [POST /external-accounts](#post-external-accounts) | Creates the data of external accounts and user data linking |
|  [GET /external-accounts](#get-external-accounts)  |         Fetches the accountId of a external account         |

---

## **POST /external-accounts**

Creates the data of external accounts and user data linking

- **Params**  
  None
- **Query**  
  None
- **Body** Possible values for `type`: [discord]

  ```json
  {
    "type": "<ACCOUNT_TYPE> (String)",
    "token": "<ENCRYPTED_TOKEN> (String)",
    "accountId": "<ACCOUNT_ID> (String)",
    "timestamp": "<CURRENT_TIME_TIMESTAMP> (Timestamp)",
    "expiry": "<EXPIRY_TIME_TIMESTAMP> (Timestamp)"
  }
  ```

- **Headers**  
  Authorization: Bearer `<JWT>`

- **Success Responses:**

  - **Code:** 201
    - **Content:**
    ```json
    {
      "accountId": "Added account data successfully"
    }
    ```

- **Error Responses:**
  - **Code:** 400
    - **Content:**
    ```json
    {
      "statusCode": 400,
      "error": "Bad Request",
      "message": "Invalid Request"
    }
    ```
  - **Code:** 401
    - **Content:**
    ```json
    {
      "statusCode": 401,
      "error": "Unauthorized",
      "message": "Unauthorized Account"
    }
    ```
    - **Code:** 500
    - **Content:**
    ```json
    {
      "statusCode": 500,
      "error": "Internal Server Error",
      "message": "Something went wrong. Please contact admin"
    }
    ```

## **GET /external-accounts**

Fetches the accountId of a external account

- **Params**  
  :token : `<EXTERNAL_ACCOUNT_TOKEN> (String)`
- **Query**  
  type: `<ACCOUNT_TYPE> (String)`
- **Headers**  
  None
- **Cookie**  
  rds-session: `<JWT>`

- **Success Responses:**

  - **Code:** 200
    - **Content:**
    ```json
    {
      "accountId": "<ACCOUNT_ID> (String)"
    }
    ```

- **Error Responses:**
  - **Code:** 401
    - **Content:**
    ```json
    {
      "statusCode": 401,
      "error": "Unauthorized",
      "message": "Unauthenticated User"
    }
    ```
    - **Code:** 500
    - **Content:**
    ```json
    {
      "statusCode": 500,
      "error": "Internal Server Error",
      "message": "Something went wrong. Please contact admin"
    }
    ```
