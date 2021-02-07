# Notifications

## Notification Object

```
{
  id: string,
  username: string,
  message: string,
  createOn: unix-timestamp,
  url: string,
  type: string
}
```


## **Requests**

|               Route                |           Description           |
| :--------------------------------: | :-----------------------------: |
|      [GET /notifications](#get-notifications)      | Returns notifications of the logged in user |

## **GET /notifications**

Returns all notifications for a logged in user

- **Params**  
  None
- **Query**  
  page=[integer], n=[integer]
- **Body**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Success Response:**
- **Code:** 200
  - **Content:**

```
{
  message: 'Notifications returned successfully!',
  notifications: [
    <notification-object>,
    <notification-object>,
    <notification-object>,
    <notification-object>,
    <notification-object>
  ]
}
```

- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 503
    - **Content:** `{ 'statusCode': 503, 'error': 'Service Unavailable', 'message': 'Something went wrong please contact admin' }`
