# Subscription API

## Endpoints

| Route | Method | Description |
|-------|--------|-------------|
| [/subscription](#post-subscription) | POST | Subscribe a user |
| [/subscription](#patch-subscription) | PATCH | Unsubscribe a user |
| [/subscription/notify](#get-subscriptionnotify) | GET | Send a test email |

## POST /subscription

Subscribe a user to notifications.

- **Authentication**: Required
- **Authorization**: Any authenticated user

### Request

- **Body**:
  - `email` (string, required): User's email address
  - `phoneNumber` (string, optional): User's phone number

### Response

- **Success Response**:
  - **Code**: 201
  - **Content**:
    ```json
    {
      "message": "user subscribed successfully"
    }
    ```

- **Error Response**:
  - **Code**: 500
  - **Content**: 
    ```json
    {
      "statusCode": 500,
      "error": "Internal Server Error",
      "message": "An internal server error occurred"
    }
    ```

## PATCH /subscription

Unsubscribe a user from notifications.

- **Authentication**: Required
- **Authorization**: Any authenticated user

### Request

- **Body**: None required

### Response

- **Success Response**:
  - **Code**: 200
  - **Content**:
    ```json
    {
      "message": "user unsubscribed successfully"
    }
    ```

- **Error Response**:
  - **Code**: 500
  - **Content**: 
    ```json
    {
      "statusCode": 500,
      "error": "Internal Server Error",
      "message": "An internal server error occurred"
    }
    ```

## GET /subscription/notify

Send a test email. This endpoint is for development and testing purposes only.

- **Authentication**: Required
- **Authorization**: SUPERUSER role only

### Request

- **Body**: None required

### Response

- **Success Response**:
  - **Code**: 200
  - **Content**:
    ```json
    {
      "message": "Email sent",
      "info": {
        // Email sending information
      }
    }
    ```

- **Error Response**:
  - **Code**: 500
  - **Content**: 
    ```json
    {
      "message": "Failed to send email",
      "error": {
        // Error details
      }
    }
    ```

## Notes

1. All routes require authentication.
2. The `/subscription/notify` route is restricted to users with the SUPERUSER role.
3. Error handling for authentication and authorization failures is not explicitly defined in the provided code, but would typically result in 401 (Unauthorized) or 403 (Forbidden) responses.
4. The email sending functionality uses nodemailer and requires proper configuration of SMTP credentials.
5. The API uses Express.js and includes middleware for authentication and role-based authorization.


Common error codes:
- 400: Bad Request (e.g., invalid input)
- 401: Unauthorized (authentication required)
- 403: Forbidden (insufficient permissions)
- 500: Internal Server Error

For detailed error messages and handling, please refer to the API implementation.