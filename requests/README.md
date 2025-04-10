# Requests API Contract

The Requests API provides endpoints for creating, fetching, and updating requests. The API contract details the available routes, request and response structures, query parameters, headers, cookies, and error handling.

| Route                                | Description                                                       |
| ------------------------------------ | ----------------------------------------------------------------- |
| [GET /requests](#get-requests)       | Returns a list of requests with pagination and filtering options. |
| [POST /requests](#post-requests)     | Creates a new request.                                            |
| [PUT /requests/:id](#put-requestsid) | Updates an existing request.                                      |
| [PATCH /requests/:id](#patch-requestsid) | Updates an existing request before approval or rejection. Also acknowledges (approve or reject) existing request.                                     |

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

#### Authentication and Authorization:

- Authentication is required for accessing this endpoint.

#### Additional Notes:

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

  - `type`: Required string to specify the type of request (e.g., OOO,EXTENSION,TASK).
  - `from`: Required number to specify the start timestamp of the request.
  - `until`: Required number to specify the end timestamp of the request.
  - `message`: Required string to specify the message for the request.
  - `state`: Required string to specify the state of the request (e.g., PENDING).

  - Example OOO Request:
    ```json
    {
      "type": "OOO",
      "from": "<timestamp>",
      "until": "<timestamp>",
      "reason": "<Request Reason>"
    }
    ```
  - Example EXTENSION Request:
    ```json
    {
      "type": "EXTENSION",
      "taskId": "<Task ID>",
      "title": "<Extension Title>",
      "oldEndsOn": "<timestamp>",
      "newEndsOn": "<timestamp>",
      "message": "<Extension Message>",
      "state": "PENDING"
    }
    ```
  - Example TASK Request:
    ```json
    {
      "type": "TASK",
      "externalIssueUrl": "<GitHub Issue API URL>",
      "externalIssueHtmlUrl": "<GitHub Issue HTML URL>",
      "requestType": "CREATION | ASSIGNMENT",
      "proposedStartDate": "<timestamp>",
      "proposedDeadline": "<timestamp>",
      "description": "<Request Description>",
      "markdownEnabled": "false | true",
      "userId": "<RDS User ID>",
      "state": "PENDING"
    }
    ```
  - Example Onboarding Extension Request:
    ```json
    {
      "type": "ONBOARDING",
      "numberOfDays": "<number>",
      "userId": "<RDS Discord Id>",
      "reason": "<Request Reason>"
    }
    ```

- **Success Response of OOO Request:**

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
        "userId": "string",
        "type": "string",
        "from": "number",
        "until": "number",
        "reason": "string",
        "comment": "string",
        "status": "string",
        "lastModifiedBy": "string"
      }
    }
    ```

- **Error Responses of OOO Request:**
  - **Code:** 403
    - **Content:** `{ "statusCode": 403, "error": "Forbidden", "message": "Your status is already OOO. Please cancel OOO to raise new one" }`
  - **Code:** 404
    - **Content:** `{ "statusCode": 404, "error": "Not Found", "message": "User status not found" }`
  - **Code:** 409
    - **Content:** `{ "statusCode": 409, "error": "Conflict", "message": "Request already exists please wait for approval or rejection" }`
  - **Code:** 500
    - **Content:** `{ "statusCode": 500, "error": "Internal Server Error", "message": "An internal server error occurred" }`

- **Success Response of Onboarding Extension Request:**

  - **Code:** 201
  - **Content:**
    ```json
    {
      "message": "Onboarding extension request created successfully",
      "data": {
        "id": "string",
        "createdAt": "number",
        "updatedAt": "number",
        "requestedBy": "<username>",
        "type": "string",
        "state": "string",
        "userId": "string",
        "requestNumber": "number",
        "reason": "<request-reason>",
        "newEndsOn": "number",
        "oldEndsOn": "number",
      }
    }
    ```

- **Error Responses of Onboarding Extension Request:**
  - **Code:** 409
    - **Content:** `{ "statusCode": 409, "error": "Conflict", "message": "Request already exists please wait for approval or rejection" }`
  - **Code:** 500
    - **Content:** `{ "statusCode": 500, "error": "Internal Server Error", "message": "An internal server error occurred"" }`
  - **Code:** 404
    - **Content:** `{ "statusCode": 404, "error": "Not Found", "message": "User not found" }`
  - **Code:** 403
    - **Content:** `{ "statusCode": 403, "error": "Forbidden", "message": "Only super user and onboarding user are  authorized to create an onboarding extension request" }`


#### Authentication and Authorization:

- Authentication is required for creating a new request.

#### Additional Notes:

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

- **Success Response of Onboarding Extension Request:**

  - **Code:** 200
  - **Content:**
    ```json
    {
      "message": "Request approved successfully | Request rejected successfully",
      "data": {
        "id": "string",
        "updatedAt": "number",
        "type": "string",
        "state": "string",
        "lastModifiedBy": "string",
        "message": "string"
      }
    }
    ```

- **Error Responses of Onboarding Extension Request:**
  - **Code:** 400
    - **Content:** `{ "statusCode": 400, "error": "Bad Request", "message": "Request already rejected" }`
  - **Code:** 400
    - **Content:** `{ "statusCode": 400, "error": "Bad Request", "message": "Request already approved" }`
  - **Code:** 404
    - **Content:** `{ "statusCode": 404, "error": "Not Found", "message": "Request does not exist" }`
  - **Code:** 500
    - **Content:** `{ "statusCode": 500, "error": "Internal Server Error", "message": "An internal server error occurred" }`
  - **Code:** 401
    - **Content:** `{ "statusCode": 401, "error": "Unauthorized", "message": "Unauthenticated User" }`
  - **Code:** 401
    - **Content:** `{ "statusCode": 401, "error": "Unauthorized", "message": "You are not authorized for this action." }`

#### Authentication and Authorization:

- Authentication is required for accessing this endpoint.

#### Additional Notes:

- The request body should contain the necessary details for updating an existing request, including type, reason, and state.
- Error handling is provided for cases where the request is already approved and for internal server errors.

### **PATCH /requests/:id**

This endpoint serves dual purposes based on the request type:
- Updates an existing request before approval or rejection with the provided details. Also acknowledges (approve or reject) existing request with the provided details.

- **Description:** Updates an existing request before approval or rejection with the provided details. Also acknowledges (approve or reject) existing request with the provided details.

- **URL:** `https://api.realdevsquad.com/requests/:id`

- **Method:** PATCH

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

   - **Example of OOO Request:**
    ```json
    {
      "type": "OOO",
      "status": "string", // status must be APPROVED or REJECTED
      "comment": "string" // optional
    }
    ```

- **Success Response of OOO Request:**

  - **Code:** 200
  - **Content:**
    ```json
    {
      "message": "Request approved/rejected successfully"
    }
    ```

- **Error Responses of OOO Request:**
  - **Code:** 400
    - **Content:** `{ "statusCode": 400, "error": "Bad Request", "message": "Invalid request type" }`
  - **Code:** 400
    - **Content:** `{ "statusCode": 400, "error": "Bad Request", "message": "Request already approved" }`
  - **Code:** 400
    - **Content:** `{ "statusCode": 400, "error": "Bad Request", "message": "Request already rejected" }`
  - **Code:** 401
    - **Content:** `{ "statusCode": 401, "error": "Unauthorized", "message": "Unauthenticated User" }`
  - **Code:** 401
    - **Content:** `{ "statusCode": 401, "error": "Unauthorized", "message": "Only super users are allowed to acknowledge OOO requests" }`
  - **Code:** 404
    - **Content:** `{ "statusCode": 404, "error": "Not Found", "message": "Request does not exist" }`
  - **Code:** 500
    - **Content:** `{ "statusCode": 500, "error": "Internal Server Error", "message": "An internal server error occurred" }`

   - **Example of Onboarding Extension Request:**
    ```json
    {
      "type": "ONBOARDING",
      "reason": "string", // optional
      "newEndsOn": "number"
    }
    ```
- **Success Response of Onboarding Extension Request:**

  - **Code:** 200
  - **Content:**
    ```json
    {
      "message": "Request updated successfully",
      "data": {
        "id": "string",
        "newEndsOn": "number"
        "updatedAt": "number",
        "type": "ONBOARDING",
        "lastModifiedBy": "string",
        "reason": "string"
      }
    }
    ```

- **Error Responses of Onboarding Extension Request:**
  - **Code:** 400
    - **Content:** `{ "statusCode": 400, "error": "Bad Request", "message": "Invalid type" }`
  - **Code:** 400
    - **Content:** `{ "statusCode": 400, "error": "Bad Request", "message": "Invalid request" }`
  - **Code:** 400
    - **Content:** `{ "statusCode": 400, "error": "Bad Request", "message": "Invalid request type" }`
  - **Code:** 400
    - **Content:** `{ "statusCode": 400, "error": "Bad Request", "message": "Only pending extension request can be updated" }`
  - **Code:** 400
    - **Content:** `{ "statusCode": 400, "error": "Bad Request", "message": "New deadline of the request must be greater than old deadline" }`
  - **Code:** 403
    - **Content:** `{ "statusCode": 403, "error": "Forbidden", "message": "Unauthorized to update request" }`
  - **Code:** 404
    - **Content:** `{ "statusCode": 404, "error": "Not Found", "message": "Request does not exist" }`
  - **Code:** 500
    - **Content:** `{ "statusCode": 500, "error": "Internal Server Error", "message": "An internal server error occurred" }`
  - **Code:** 401
    - **Content:** `{ "statusCode": 401, "error": "Unauthorized", "message": "Unauthenticated User" }`

#### Authentication and Authorization:

- Authentication is required for accessing this endpoint.

## Conclusion

The Requests API contract provides detailed information about the available routes, request and response structures, query parameters, headers, cookies, error handling, and authentication and authorization requirements. It also includes additional notes for each endpoint to provide further context and guidance. This contract serves as a comprehensive reference for developers who need to interact with the Requests API.
