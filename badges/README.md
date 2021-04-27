# Badges

## Badge object

```
{
  'id': string,
  'title': string,
  'description': string,
  'imgUrl': string,
  'users': [
    rdsUserId,
  ],
}
```

## **Requests**

|              Route               |           Description            |
|:--------------------------------:|:--------------------------------:|
|    [GET /badges](#get-badges)    | Returns all badges in the system |
|    [GET /badges/:username](#get-badgesusername)    | Returns the badges of the individual member |


## **GET /badges**

Returns all badges in the system.

- **Params**  
  None
- **Query**  
  None
- **Body**  
  None
- **Headers**  
  None
- **Cookie**  
  None
- **Success Response:**
- **Code:** 200
  - **Content:**

```
{
  message: 'Badges returned successfully!'
  badges: [
    {<badge_object>}
  ]
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'Something went wrong. Please contact admin' }`
