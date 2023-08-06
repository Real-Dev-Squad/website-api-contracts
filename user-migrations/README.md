# Collection - Users

## API Endpoints

|                                           Route                                           |                           Description                            |
| :---------------------------------------------------------------------------------------: | :--------------------------------------------------------------: |
| [PATCH /migrations/addDefaultColorProperty](#patch-migrations-add_default_color_property) | Adds a colors property all to users, without the colors property |

## PATCH /migrations/addDefaultColorProperty

Adds a colors field to every exisiting user.

- **Params**
  None
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
            "usersDetails": {
             "totalUsersFetched": "Number",
            "totalUsersUpdated": "Number",
            "totalUsersUnaffected": "Number"
            },
            "message": "String",
          }
            ```json

    ```

    ```

- **Error Response:**

  - **Code:** 401

    - **Content:**
      ```json
      {
        "statusCode": 401,
        "error": "Unauthorized",
        "message": "Unauthenticated User."
      }
      ```

  - **Code:** 500
    - **Content:**
      ```json
      {
        "message": "The server has encountered an unexpected error. Please contact the administrator for more information."
      }
      ```

- **Example for updating an existing user documents:**
  PATCH /migrations/addDefaultColorProperty<br/>
  Content-Type: application/json<br/>
  Request-Body:<br/>
  None
  Response :
  Status 200 OK <br/>
  Content-Type: application/json<br/>

  ```json
  {
     {
    "usersDetails": {
        "totalUsersFetched": 5,
        "totalUsersUpdated": 2,
        "totalUsersUnaffected": 3
    },
    "message": "User colors updated successfully!"
  }
  }
  ```
