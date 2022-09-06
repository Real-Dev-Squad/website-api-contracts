# Cache

## **Requests**

|              Route              |                       Description                       |
| :-----------------------------: | :-----------------------------------------------------: |
| [GET /cache/member](#get-cache) | Returns the cloudflare cache meta data of last 24 hours |

---

## **GET /cache/member**

Returns the cache logs and its count of last 24 hours.

- **Params**  
  None
- **Query**  
  pastHour=24
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
      "id": "string",
      "message": "Purged cache metadata returned successfully!",
      "count": "number",
      "timestamp": "number"
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
