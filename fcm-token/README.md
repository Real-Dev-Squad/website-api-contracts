# Collection - fcmTokens

## API Endpoints

|                Route                |                    Description                     |
| :---------------------------------: | :------------------------------------------------: |
| [POST /v1/fcm-tokens](#post-notify) | Saves FCM token to uniquely identified the device. |
|                                     |

## POST /v1/fcm-tokens

    Creates a new fcm-token document.

- **Params**  
  None
- **Query**
  None
- **Body**

  - Attributes
    - **fcmToken** (required, string): Specifies the fcm token for the document which will uniquely identified the device.

- **Headers**
  - Content-Type: application/json
- **Cookie**
  None
- **Success Response:**

  - **Code:** 201

    - **Content:**

      ```json
      {
        "status": 201,
        "message": "Device registered successfully"
      }
      ```

- **Error Response:**

  - **Code:** 400

    - **Content:**

      ```json
      {
        "statusCode": 400,
        "error": "Bad Request",
        "message": "\"fcmToken\" is required"
      }
      ```

  - **Code:** 409

    - **Content:**

      ```json
      {
        "status": 409,
        "message": "Device Already Registered"
      }
      ```

  - **Code:** 500

    - **Content:**

      ```json
      {
        "message": "The server has encountered an unexpected error. Please contact the administrator for more information."
      }
      ```

- **Example for device fcm-token document creation request:**
  POST /v1/fcm-tokens<br/>
  Content-Type: application/json<br/>
  Request-Body:<br/>

  ```json
  {
    "fcmToken": "jkshdsadlksajdl"
  }
  ```

  Response :
  Status 201<br/>
  Content-Type: application/json<br/>

  ```json
  {
    "status": 201,
    "message": "Device registered successfully"
  }
  ```
