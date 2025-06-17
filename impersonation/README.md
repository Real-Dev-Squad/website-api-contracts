# Impersonation API Contract

The Impersonation API provides endpoints for creating, fetching, and updating impersonation requests, along with APIs to start or stop impersonation sessions. This contract details the available routes, request and response structures, query parameters, headers, cookies, and error handling.

|                         Route                                  |               Description                 |
| :------------------------------------------------------------: | :---------------------------------------: |
| [POST /impersonation/requests](#post-impersonationrequests)    |    Create a new impersonation request     |

## **POST /impersonation/requests**

Creates a new request.

- **Description:** Allows a super-user to create a new impersonation request with the provided details.

- **URL:** `https://api.realdevsquad.com/impersonation/requests?dev=true`

- **Query Parameters:**

  - `dev`: Required Boolean flag to enable developer mode. If not provided, developer mode is disabled by default and the request will be processed in production mode.

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
    { "statusCode": 403, "error": "Forbidden", "message": "Request already exists, please wait for approval or rejection." }
    ```

    ```json
    { "statusCode": 403, "error": "Forbidden", "message": "Please complete impersonation before creating a new request." }
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