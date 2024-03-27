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
| [GET /logs/:type](#get-logstype) | Returns logs of the specific types |
| [GET /logs](#get-logs) | Returns all the logs |

---
**Note :**

- These API can only be accessed by super user
  
## **GET /logs/:type**

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

  - Optional: `userId=[string]` for type = `archive-details`
  
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
## **GET /logs**

Returns all the logs present in the collection.

**Query**

  `dev=[boolean]`
  
  Optional: `type=`
  - `CLOUDFLARE_CACHE_PURGED`
  - `PROFILE_DIFF_APPROVED`
  - `PROFILE_DIFF_REJECTED`
  - `extensionRequests`
  - `task`
  - `task-requests`
  - `REQUEST_CREATED`
  - `REQUEST-APPROVED`
  - `REQUEST-REJECTED`

- Optional: `format=feed` (returns all the logs in flattend or formatted way)
- Optional: `page=[integer]` (page can either be 0 or a positive integer. Default value is 0)
- Optional: `size=[integer]` (size is the number of logs requested per page. Default value is 5)
- Optional: `next=[string]` (next is the id of the document to get next set of documents of results from that document)
- Optional: `prev=[string]` (prev is the id of the document to get prev set of documents of results before that document)

- **Body**  
  None
- **Headers**  
  None

- **Cookie**
rds-session: <JWT SUPERUSER>

- **Success Response:**

  - **Code:** 200
    - **Content:**
    ```json
    {
      "message": "All Logs fetched successfully",
      "data": ["<LOG_OBJECT>", "<LOG_OBJECT>...."],
      "next": "/logs?dev=true&size=5&next=<document-id>",
      "prev": null
    }
    ```
- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 204
    - **Content:** `{ 'statusCode': 204, 'error': 'Not Found', 'message': 'Logs not found' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`
