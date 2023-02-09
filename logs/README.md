# Logs

## Log object

```json
{
  "type": "string",
  "meta": "object",
  "body": "object",
  "timestamp": "object"
}
```

## **Requests**

|         Route          |        Description        |
| :--------------------: | :-----------------------: |
| [GET /logs](#get-logs) | Returns logs of all types |

---

## **GET /logs**

**Note :**

- This API can only be accessed by super user

Returns all logs according to the provided queries and path/named-route param.

- **Param**  
  None
- **Query**  
  type=<LOG_TYPE>
- **Body**  
  None
- **Headers**  
  None
- **Cookie**  
  rds-session: `<JWT>`

- **Success Response:**

  - **Code:** 200
    - **Content:**
    ```json
    {
      "message": "Logs returned successfully!",
      "logs": ["<LOG_OBJECT>", "<LOG_OBJECT>...."]
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
