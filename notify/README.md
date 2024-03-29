# Collection - no collection for now

## API Endpoints

|                     Route                     |                  Description                  |
| :-------------------------------------------: | :-------------------------------------------: |
| [POST /v1/notifications](#post-notifications) | send notification to your specified fcm token |
|                                               |

## POST /v1/notifications

    Send notification to your specified device.

- **Params**  
  None
- **Query**
  None
- **Body**

  - Attributes
    - **title** (required, string): notification title
    - **body** (required, string): notification message
    - **userId** (optional, string): user who is assigning
    - **groupRoleId** (optional, string) : group to whom the notification will be broadcasted

- **Headers**
  - Content-Type: application/json
- **Cookie**
  None
- **Success Response:**
  - **Code:** 200
    - **Content:**
      ```json
      {
        "status": 200,
        "message": "User notified successfully"
      }
      ```
- **Error Response:**

  - **Code:** 400

    - **Content:**

      ```json
      {
        "statusCode": 400,
        "error": "Bad Request",
        "message": "\"value\" must contain at least one of [userId, groupRoleId]"
      }
      ```

      - **Code:** 401

    - **Content:**
      ```json
      {
        "statusCode": 406,
        "error": "Bad Request",
        "message": "Message length exceeds"
      }
      ```

  - **Code:** 500
    - **Content:**
      ```json
      {
        "statusCode": 500,
        "message": "The server has encountered an unexpected error. Please contact the administrator for more information."
      }
      ```

- **Example for device fcm-token document creation request:**
  POST /v1/notifications<br/>
  Content-Type: application/json<br/>
  Request-Body:<br/>

  ```json
  {
    "title": "testing",
    "body": "Helloo world",
    "userId": "feJ49TjqHVG4O0luUDlz"
  }
  ```

  Response :
  Status 200<br/>
  Content-Type: application/json<br/>

  ```json
  {
    "status": 200,
    "message": "User notified successfully"
  }
  ```

  ```json
  {
    "title": "testing",
    "body": "Helloo world",
    "groupRoleId": "1147354535342383104"
  }
  ```

  Response :
  Status 200<br/>
  Content-Type: application/json<br/>

  ```json
  {
    "status": 200,
    "message": "User notified successfully"
  }
  ```
