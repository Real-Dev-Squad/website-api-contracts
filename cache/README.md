# Cache

## **Requests**

|          Route           |                       Description                       |
| :----------------------: | :-----------------------------------------------------: |
| [GET /cache](#get-cache) | Returns the cloudflare cache meta data of last 24 hours |

---

## **GET /cache**

Returns the cache meta data and its count of last 24 hours.

- **Params**  
  None
- **Query**  
  None
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
      "message": "Cache data fetched successfully",
      "count": "number",
      "timeLastCleared": "<ISO 8601 timestamp>"
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
