# QrCodeAuth

## Requests

1. POST 

`/qr-code-auth?user-id=${user-id}&device-info={deviceInfo}`

Description: Authenticates the user by verifying the user ID and device information.

- **Params**
  None
- **Query**
  None

- **Body**  
 ```
 {
  "user_id": "<user-id>",
  "device_info": "<device-info>"
 }
```
- **Headers**  
  None
- **Cookie**  
  None

#### Success Response:

```
{
    "statusCode": 201,
    "userDeviceInfoData: {
        "user_id": "9",
        "device_info": "window"
    },
    "message": "User Device Info added successfully!"
}
```
#### Error Responses
```
{
    "statusCode": 404,
    "message": "Api not found."
}
```

```
{
    "statusCode": 500,
    "message": "Internal server error."
}
```
```
{
    "statusCode": 400,
    "message": "Bad Request"
}
```









