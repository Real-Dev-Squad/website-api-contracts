# Cache

## **Requests**

|           Route            |                       Description                       |
| :------------------------: | :-----------------------------------------------------: |
|  [GET /cache](#get-cache)  | Returns the cloudflare cache meta data of last 24 hours |
| [POST /cache](#post-cache) |          Purges the cloudflare cache of a page          |

---

**NOTE**

- There is a limitation in the **POST** API, that Superuser cannot clear their
  cache, if they have already cleared the other users cache for more than 3
  times in last 24 hours.

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

## **POST /cache**

Purges the cloudflare cache of a page

- **Params**  
  None
- **Query**  
  None
- **Body**  
  user: "<USERNAME>" // optional - only super user has authority to pass this
  JSON
- **Headers**  
  X-Auth-Key: `<CLOUDFLARE_X_AUTH_KEY>` 
  X-Auth-Email: `<CLOUDFLARE_X_AUTH_EMAIL>`
- **Cookie**  
  rds-session: `<JWT>`

- **Data** 
  files: `<FILES>`
- **API Used** 
  CLOUDFLARE_PURGE_CACHE_API: `https://api.cloudflare.com/client/v4/zones/${CLOUDFLARE_ZONE_ID}/purge_cache`

- **Success Responses:**

  - **Code:** 200
    - **Content:**
    ```json
    {
      "message": "Cache purged successfully",
      "result": {
        "id": "ba637cab83d148e6935cbba0b197d495"
      },
      "success": true,
      "errors": [],
      "messages": []
    }
    ```
  - **Code:** 200
    - **Content:**
    ```json
    {
      "message": "Maximum Limit Reached for Purging Cache. Please try again after some time"
    }
    ```

- **Error Responses:**
  - **Code:** 400
    - **Content:**
    ```json
    {
      "statusCode": 400,
      "error": "Bad Request",
      "message": "Bad Request"
    }
    ```
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
