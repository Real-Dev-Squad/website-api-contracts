
## **Requests**

|                         Route                          |             Description              |
| :----------------------------------------------------: | :----------------------------------: |
|                [POST /requests](#post-requests)                |   Creates a new Out of Office (OOO) request    |

## **POST /requests**

Creates a new request.

- **Middleware:**
  - `authenticate`: Middleware to authenticate user
  - `createRequestsMiddleware`: Middleware to validate create request data

- **Input:**
  - **Headers:**
    - `cookie`: User authentication token
  - **Body:**
    - `type`: Type of request
    - `userData`: User data
    - **Query Parameters:**
      - `dev`: Flag to enable developer mode

- **Success Response:**
  - **Code:** 201
  - **Content:**
    ```json
    {
        "message": "OOO status requested successfully"
    }
    ```

- **Error Responses:**
  - **Code:** 401
    - **Content:**
      ```json
      {
          "statusCode": 401,
          "error": "Unauthorized User"
      }
      ```
  - **Code:** 400
    - **Content:**
      ```json
      {
          "statusCode": 400,
          "error": "Bad Request",
          "message": "Please use feature flag to make this request"
      }
      ```
  - **Code:** 500
    - **Content:**
      ```json
      {
          "statusCode": 500,
          "error": "Internal Server Error",
          "message": "An internal server error occurred"
      }
      ```