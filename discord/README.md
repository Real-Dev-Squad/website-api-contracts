# Discord

## **Requests**

|             Route              |                      Description                      |
| :----------------------------: | :---------------------------------------------------: |
| [POST /discord](#post-discord) | Creates the data of discord bot and user data linking |

---

## **POST /discord**

Creates the data of discord bot and user data linking

- **Params**  
  None
- **Query**  
  None
- **Body**

  ```json
  {
    "token": "<ENCRYPTED_TOKEN> (String)",
    "discordId": "<DISCORD_USER_ID> (String)",
    "timestamp": "<CURRENT_TIME_TIMESTAMP> (Number)",
    "expiry": "<EXPIRY_TIME_TIMESTAMP> (Number)",
    "linkStatus": "<LINK_STATUS> (Boolean)"
  }
  ```

- **Headers**  
  Authorization: Bearer `<JWT>`

- **Success Responses:**

  - **Code:** 201
    - **Content:**
    ```json
    {
      "message": "Added discord data successfully"
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
      "message": "Unauthorized Bot"
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
