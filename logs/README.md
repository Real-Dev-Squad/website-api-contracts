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

|            Route             |        Description        |
| :--------------------------: | :-----------------------: |
| [GET /logs/:type](#get-logs) | Returns logs of all types |

---

## **GET /logs/:type**

**Note :**

- This API can only be accessed by super user

Returns all logs according to the provided queries and path/named-route param.

- **Param**  
  type:

  - `CLOUDFLARE_CACHE_PURGED`
  - `PROFILE_DIFF_APPROVED`
  - `PROFILE_DIFF_REJECTED`
  - `extensionRequests`
  - `task`
  - `archive-details`

- **Query**

  - Optional: `userId=[string]` for type = `archiveDetails`

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
      "message": "You are not authorized for this action."
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
    - **Code:** 503
    - **Content:**
    ```json
    {
      "statusCode": 503,
      "error": "Internal Server Error",
      "message": "Something went wrong. Please contact admin"
    }
    ```
