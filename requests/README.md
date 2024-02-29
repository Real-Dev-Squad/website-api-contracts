# Requests API Contract

The Requests API provides endpoints for creating, fetching, and updating requests. The API contract details the available routes, request and response structures, query parameters, headers, cookies, and error handling.

| Route                                | Description                                                       |
| ------------------------------------ | ----------------------------------------------------------------- |
| [GET /requests](#get-requests)       | Returns a list of requests with pagination and filtering options. |
| [POST /requests](#post-requests)     | Creates a new request.                                            |
| [PUT /requests/:id](#put-requestsid) | Updates an existing request.                                      |

### **GET /requests**

Returns a list of requests with pagination and filtering options.

- **Description:** Fetches a list of requests, optionally filtered by various parameters.

- **URL:** `https://api.realdevsquad.com/requests?dev=true`

- **Method:** GET

- **Query Parameters:**

  - `dev`: Required boolean to fetch requests.
  - `page`: Optional integer to specify the page number. Default is 1.
  - `size`: Optional integer to specify the number of requests per page. Default is 5.
  - `requestedBy`: Optional string to filter requests by the requester's username.
  - `type`: Optional string to filter requests by type (e.g., OOO).
  - `state`: Optional string to filter requests by state (e.g., APPROVED, REJECTED).

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
          "createdAt": "number",
          "requestedBy": "string",
          "from": "number",
          "until": "number",
          "type": "string",
          "message": "string",
          "reason": "string",
          "lastModifiedBy": "string",
          "state": "string",
          "updatedAt": "number"
        },
        {
          "id": "string",
          "createdAt": "number",
          "requestedBy": "string",
          "from": "number",
          "until": "number",
          "type": "string",
          "message": "string",
          "reason": "string",
          "lastModifiedBy": "string",
          "state": "string",
          "updatedAt": "number"
        }
        // Additional request objects
      ],
      "next": "string",
      "prev": "string"
    }
    ```

- **Error Responses:**

  - **Code:** 500
    - **Content:** `{ "statusCode": 500, "error": "Internal Server Error", "message": "An internal server error occurred" }`

- **Not Found Response:**

  - **Code:** 204
    - **Content:** No content

- **Notes:**
  - Pagination is applied using the `page` and `size` parameters.
  - Filtering options are available using parameters like `requestedBy`,`type`, and `state`.
  - The response includes a list of request objects with their respective properties.

### Authentication and Authorization:

- Authentication is required for accessing this endpoint.

### Additional Notes:

- The provided response includes details of each request, such as ID, timestamps, requester, type, message, state, etc.
- Pagination functionality is implemented using `next` and `prev` parameters in the response.
- Error handling is provided for internal server errors (status code 500).

This API contract details the GET method for fetching requests, including all available query parameters, response structure, potential error responses, authentication, authorization requirements, and additional notes.

### **POST /requests**

Creates a new request.

- **Description:** Creates a new request with the provided details.

- **URL:** `https://api.realdevsquad.com/requests?dev=true`

- **Query Parameters:**

  - `dev`: Required boolean to create requests in developer mode.

- **Method:** POST

- **Headers:**

  - Content-Type: application/json

- **Cookie:**

  - rds-session: `<JWT>`

- **Request Body:**

- Body Parameters:

  - `type`: Required string to specify the type of request (e.g., OOO).
  - `from`: Required number to specify the start timestamp of the request.
  - `until`: Required number to specify the end timestamp of the request.
  - `message`: Required string to specify the message for the request.
  - `state`: Required string to specify the state of the request (e.g., PENDING).

  - Example:
    ```json
    {
      "type": "OOO",
      "from": "<timestamp>",
      "until": "<timestamp>",
      "message": "string",
      "state": "PENDING"
    }
    ```

- **Success Response:**

  - **Code:** 200
  - **Content:**
    ```json
    {
      "message": "Request created successfully",
      "data": {
        "id": "string",
        "createdAt": "number",
        "updatedAt": "number",
        "requestedBy": "string",
        "type": "string",
        "from": "number",
        "until": "number",
        "message": "string",
        "state": "string"
      }
    }
    ```

- **Error Responses:**
  - **Code:** 400
    - **Content:** `{ "statusCode": 400, "error": "Bad Request", "message": "Request already exists. Please wait for approval or rejection" }`
  - **Code:** 500
    - **Content:** `{ "statusCode": 500, "error": "Internal Server Error", "message": "An internal server error occurred" }`

### Authentication and Authorization:

- Authentication is required for creating a new request.

### Additional Notes:

- The request body should contain the necessary details for creating a new request, including type, timestamps for from and until, message, and state.
- The state is set to "PENDING" by default.
- Error handling is provided for cases where the request already exists and for internal server errors.

### **PUT /requests/:id**

Updates an existing request with the provided details.

- **Description:** Updates an existing request with the provided details.

- **URL:** `https://api.realdevsquad.com/requests/:id`

- **Method:** PUT

- **Path Parameters:**

  - `id`: The unique identifier of the request to be updated.

- **Query Parameters:**

  - `dev`: Required boolean to update requests in developer mode.

- **Headers:**
  - Content-Type: application/json
- **Cookie:**

  - rds-session: `<JWT>`

- **Request Body:**

- Body Parameters:

  - `type`: Required string to specify the type of request (e.g., OOO).
  - `reason`: Optional string to specify the reason for the request.
  - `state`: Required string to specify the state of the request (e.g., APPROVED or REJECTED).

  - Example:
    ```json
    {
      "type": "OOO",
      "reason": "string",
      "state": "APPROVED"
    }
    ```

- **Success Response:**

  - **Code:** 200
  - **Content:**
    ```json
    {
      "message": "Request approved successfully",
      "data": {
        "id": "string",
        "updatedAt": "number",
        "lastModifiedBy": "string",
        "type": "string",
        "reason": "string",
        "state": "APPROVED"
      }
    }
    ```

- **Error Responses:**
  - **Code:** 400
    - **Content:** `{ "statusCode": 400, "error": "Bad Request", "message": "Request already approved" }`
  - **Code:** 500
    - **Content:** `{ "statusCode": 500, "error": "Internal Server Error", "message": "An internal server error occurred" }`

### Authentication and Authorization:

- Authentication is required for accessing this endpoint.

### Additional Notes:

- The request body should contain the necessary details for updating an existing request, including type, reason, and state.
- Error handling is provided for cases where the request is already approved and for internal server errors.

## Conclusion

The Requests API contract provides detailed information about the available routes, request and response structures, query parameters, headers, cookies, error handling, and authentication and authorization requirements. It also includes additional notes for each endpoint to provide further context and guidance. This contract serves as a comprehensive reference for developers who need to interact with the Requests API.
