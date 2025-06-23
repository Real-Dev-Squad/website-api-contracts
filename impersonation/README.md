# Impersonation API Contract

The Impersonation API provides endpoints for creating, fetching, and updating impersonation requests, along with APIs to start or stop impersonation sessions. This contract details the available routes, request and response structures, query parameters, headers, cookies, and error handling.

|                         Route                                  |               Description                 |
| :------------------------------------------------------------: | :---------------------------------------: |
| [POST /impersonation/requests](#post-impersonationrequests)    |    Create a new impersonation request     |
| [PATCH /impersonation/requests/:id](#patch-impersonationrequestsid)      |  Updates the status of an impersonation request.    |

## **POST /impersonation/requests**

Creates a new impersonation request.

- **Description:** Allows a super-user to create a new impersonation request with the provided details.

- **URL:** `https://api.realdevsquad.com/impersonation/requests`

- **Query Parameters:**

  - `dev`: Required boolean feature flag to create impersonation requests.

- **Method:** POST

- **Headers:**

  - Content-Type: application/json

- **Cookie:**

  - rds-session: `<JWT>`

- **Request Body:**

  - Body Parameters:

    - `impersonatedUserId`: **Required.** String. The userId of the user to be impersonated.
    - `reason`: **Required.** String. The reason for requesting impersonation.

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
- The status is set to "PENDING" by default.

## **PATCH /impersonation/requests/:id**

- **Description:**  
  This endpoint updates the status of an impersonation request.

- **URL:** `https://api.realdevsquad.com/impersonation/requests/:id`

- **Method:** PATCH

- **Path Parameters:**
  - `id`: The unique identifier of the request to be updated.

- **Query Parameters:**

  - `dev`: Required boolean feature flag to update request status.

- **Headers:**
  - Content-Type: application/json

- **Cookie:**
  - rds-session: `<JWT>`

- **Request Body:**

  - Body Parameters:

    - `status`: **Required.** String. Allowed values: `"APPROVED"` or `"REJECTED"`. The new status for the impersonation request.
    - `message`: **Optional.** String. An optional message or reason for approval or rejection.

    - Example Request Body:

      ```json
      {
        "status": "APPROVED",
        "message": "Approved by the user"
      }
      ```

- **Success Response of Impersonation Request:**

  - **Code:** 200
  - **Content:**
    ```json
    {
      "message": "Request approved/rejected successfully",
      "data": {
        "id": "string",
        "lastModifiedBy": "string",
        "message": "string",
        "status": "string"
      }
    }
    ```

- **Error Responses of Impersonation Request:**
  - **Code:** 400
    - **Content:** `{ "statusCode": 400, "error": "Bad Request", "message": "Invalid request type" }`
  - **Code:** 403
    - **Content:** `{ "statusCode": 403, "error": "Forbidden", "message": "You are not allowed for this Operation at the moment" }`
  - **Code:** 401
    - **Content:** `{ "statusCode": 401, "error": "Unauthorized", "message": "Unauthenticated User" }`
  - **Code:** 404
    - **Content:** `{ "statusCode": 404, "error": "Not Found", "message": "Request does not exist" }`
  - **Code:** 500
    - **Content:** `{ "statusCode": 500, "error": "Internal Server Error", "message": "An internal server error occurred" }`

#### Authentication and Authorization

- Authentication is required for accessing this endpoint.