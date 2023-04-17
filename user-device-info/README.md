# User Device Info

## User Device Info object

```json
{
  "userId": "String",
  "deviceType": "String"
}
```

## **Requests**

|                    Route                     |          Description           |
| :------------------------------------------: | :----------------------------: |
| [POST /userDeviceInfo](#post-userDeviceInfo) | Stores user id and device type |

## **POST /userDeviceInfo**

Stores user id and device type

- **Params**  
  None
- **Query**  
  None
- **Headers**  
  Content-Type: application/json
- **Body** `{ <userDeviceInfo_object> }`
- **Success Response:**
- **Code:** 200

  - **Content:**
    ```json
    {
      "message": "User Device Info stored successfully!",
      "userDeviceInfo": "<userDeviceInfo_object>"
    }
    ```

- **Error Response:**
  - **Code:** 500
    - **Content:**
      ```json
      {
        "statusCode": 500,
        "error": "Internal Server Error",
        "message": "An internal server error occurred"
      }
      ```
