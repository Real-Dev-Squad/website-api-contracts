# Impersonation API Contract

The Impersonation API provides endpoints for creating, fetching, and updating impersonation requests, along with APIs to start or stop impersonation sessions. This contract details the available routes, request and response structures, query parameters, headers, cookies, and error handling.

|                         Route                                  |               Description                 |
| :------------------------------------------------------------: | :---------------------------------------: |
| [POST /impersonation/requests](#post-impersonationrequests)    |    Create a new impersonation request     |
| [GET /impersonation/requests](#get-impersonationrequests)      |   Returns a list of impersonation requests with pagination and filtering options.    |

## **POST /impersonation/requests**

Creates a new impersonation request.

- **Description:** Allows a super-user to create a new impersonation request with the provided details.

- **URL:** `https://api.realdevsquad.com/impersonation/requests`

- **Query Parameters:**

  - `dev`: Required Boolean flag to enable developer mode. If not provided, developer mode is disabled by default and the request will be processed in production mode.

- **Method:** POST

- **Headers:**

  - Content-Type: application/json

- **Cookie:**

  - rds-session: `<JWT>`

- **Request Body:**

  - Body Parameters:

    - `impersonatedUserId`: Required string to specify userId of the user to be impersonated.
    - `reason`: Required string to specify reason for requesting impersonation.

    - Example Request Body:

      ```json
      {
        "impersonatedUserId": "<id of user to impersonate>",
        "reason": "<reason for impersonation>"
      }
      ```

- **Success Response of Impersonation Request:**

  - **Code:** 201

    ```json
    {
      "message": "Request created successfully",
      "data": {
        "id": "string",
        "createdAt": "Timestamp",
        "updatedAt": "Timestamp",
        "status": "string",
        "userId": "string",
        "impersonatedUserId": "string",
        "isImpersonationFinished": "boolean",
        "createdBy": "string",
        "createdFor": "string",
        "reason": "string"
      }
    }
    ```

- **Error Responses of Impersonation Request:**

  - **Code:** 403

    ```json
    { "statusCode": 403, "error": "Forbidden", "message": "You are not allowed for this Operation at the moment" }
    ```

  - **Code:** 500

    ```json
    { "statusCode": 500, "error": "Internal Server Error", "message": "An internal server error occurred." }
    ```

  - **Code:** 404

    ```json
    { "statusCode": 404, "error": "Not Found", "message": "User not found." }
    ```

  - **Code:** 401

    ```json
    { "statusCode": 401, "error": "Unauthorized", "message": "You are not authorized for this action." }
    ```

#### Authentication and Authorization

- Authentication is required to create a new request.
- Only super-users can create new impersonation requests.

#### Additional Notes

- The request body must contain the necessary details for creating a new request, which are the ID of the user to be impersonated and the reason for impersonation.
- The status is set to "PENDING" by default.# Impersonation API Contract


### **GET /impersonation/requests**

Returns a list of impersonation requests with pagination and filtering options.

- **Description:** Fetches a list of impersonation requests, optionally filtered by various parameters.

- **URL:** `https://api.realdevsquad.com/impersonation/requests`

- **Method:** GET

- **Query Parameters:**

  - `dev`: Required boolean to fetch requests.
  - `id`: Optional string to fetch the request by its id.
  - `page`: Optional integer to specify the page number.
  - `size`: Optional integer to specify the number of requests per page. Default is 5.
  - `createdBy`: Optional string to filter requests by username of super-user who created the request.
  - `createdFor`: Optional string to filter requests by username of user for whom the request is created.
  - `status`: Optional string to filter requests by status (e.g., APPROVED, REJECTED, PENDING).
  - `prev`: Optional string to filter requests by the next cursor, used in cursor based pagination.
  - `next`: Optional string to filter requests by the prev curosr, used in cursor based pagination.

- **Headers:**

  - Content-Type: application/json

- **Cookie:**

  - rds-session: `<JWT>`

- **Success Response:**

  - **Code:** 200
  - **Content:**
    ```json
    {
      "message": "Request fetched successfully",
      "data": [
        {
        "id": "string",
        "createdAt": "Timestamp",
        "updatedAt": "Timestamp",
        "status": "string",
        "userId": "string",
        "impersonatedUserId": "string",
        "isImpersonationFinished": "boolean",
        "createdBy": "string",
        "createdFor": "string",
        "startedAt": "Timestamp",
        "endedAt": "Timestamp",
        "reason": "string"
       },
       {
        "id": "string",
        "createdAt": "Timestamp",
        "updatedAt": "Timestamp",
        "status": "string",
        "userId": "string",
        "impersonatedUserId": "string",
        "isImpersonationFinished": "boolean",
        "createdBy": "string",
        "createdFor": "string",
        "startedAt": "Timestamp",
        "endedAt": "Timestamp",
        "reason": "string"
      }
      ],
      "next": "string",
      "prev": "string",
      "nextPage": "string",
      "count": "number"
    }
    ```

- **Error Responses:**

  - **Code:** 500
    - **Content:** `{ "statusCode": 500, "error": "Internal Server Error", "message": "An internal server error occurred" }`

- **Not Found Response:**

  - **Code:** 204
    - **Content:** No content


#### Authentication and Authorization:

- Authentication is required for accessing this endpoint.

#### Additional Notes:

- The provided response includes details of each request, such as ID, timestamps, username, userId, reson, status, etc.
- Pagination functionality is implemented using `next` and `prev` parameters in the response.
- Filtering options are available using parameters like `createdBy`,`createdFor`,`status`,etc.
- The response includes a list of request objects with their respective properties.
- Error handling is provided for internal server errors (status code 500).

This API contract details the GET method for fetching impersonation requests, including all available query parameters, response structure, potential error responses, authentication, authorization requirements, and additional notes.
