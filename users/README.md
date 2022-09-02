# Users

## User object

```
{
  'id': string,
  'username': string,
  'first_name': string,
  'last_name': string,
  'email': string,
  'phone': number,
  'yoe': number,
  'company': string,
  'designation': string,
  'img': string,
  'github_id': string,
  'linkedin_id': string,
  'twitter_id': string,
  'instagram_id': string,
  'website': string,
  'github_display_name': string,
  'isMember': boolean,
  'userType': string,
  'tokens': {},
  'badges': []
}
```
**Note:**: Only the GET `users/self` route will return `phone` and `email` if `private` query is passed as true. This way we are not exposing users' phone numbers and email addresses to everyone. Users can only see their own phone number and email address.


## **Requests**

|                   Route                   |             Description              |
| :---------------------------------------: | :----------------------------------: |
|         [GET /users](#get-users)          |   Returns all users in the system    |
|     [GET /users/self](#get-usersSelf)     | Returns the logged in user's details |
|      [GET /users/:id](#get-usersid)       |      Returns user with given id      |
|        [POST /users](#post-users)         |          Creates a new User          |
|   [PATCH /users/self](#patch-usersself)   |       Updates data of the User       |
| [POST /users/picture](#post-userspicture) |     Uploads user profile picture     |


## **GET /users**

Returns all users in the system.

- **Params**  
  None
- **Query**  
  size=[integer], page=[integer]
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
  message: 'Users returned successfully!'
  users: [
           {<user_object>}
         ]
}
```

- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`

## **GET /users/self**

Returns the details of logged in user.

- **Params**  
  None
- **Query**
  private=[boolean]    
- **Body**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 200
    - **Content:** `{ <user_object> }`
    > **Note**: The user object will include `phone` and `email` only when the query `private` is passed as `true` for this route. No other route will return `phone` and `email`.
- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 404
    - **Content:** `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'User doesn't exist' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **GET /users/:id**

Returns the specified user.

- **Params**  
  _Required:_ `id=[string]`
- **Body**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Success Response:**
- **Code:** 200
  - **Content:** `{ 'message': 'User returned successfully!', 'user': <user_object> }`
- **Error Response:**
  - **Code:** 404
    - **Content:** `{ error: 'Not Found', message: 'User doesn't exist' }`
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`

## **GET /users/isUsernameAvailable/:username**

Returns the availability of username.

- **Params**  
  _Required:_ `username=[string]`
- **Body**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 200
    - **Content:** `{ 'isUsernameAvailable': <boolean> }`
- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`


## **POST /users**

Creates a new User.

- **Params**  
  None
- **Query**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Body** `{ <user_object> }`
- **Success Response:**
  - **Code:** 200
    - **Content:** `{ <user_object> }`
- **Error Response:**
  - **Code:** 409
    - **Content:** `{ "statusCode": 409, "error": "Conflict", "message": "User already exists" }`
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`

## **PATCH /users/self**

Updates data of the User.

- **Params**  
  None
- **Query**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Body** `{ <user_object> }`
- **Success Response:**
  - **Code:** 204
    - **Content:** `{ 'message': 'User updated successfully!'}`
- **Error Response:**
  - **Code:** 404
    - **Content:** `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'User not found' }`
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 403
    - **Content:** `{ 'statusCode': 403, 'error': 'Forbidden', 'message': 'Cannot update username again'}`
  - **Code:** 503
    - **Content:** `{ 'statusCode': 503, 'error': 'Service Unavailable', 'message': 'Something went wrong please contact admin' }`
  

## **POST /users/picture**

Upload user proflie picture.

- **Params**  
  - **in**: formData
  - **name**: profile
  - **type**: file
  - **description**: Profile picture to upload
- **Query**  
  None
- **Headers**  
  Content-Type: multipart/form-data
- **Cookie**  
  rds-session: `<JWT>`
- **Body** `{ 'image':{'publicId':'image publicId','url':'imageURl'} }`
- **Success Response:**
  - **Code:** 200
    - **Content:** `{ 'message': 'Profile picture uploaded successfully', 'image':{'publicId':'image publicId','url':'imageURl'} }`
- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 404
    - **Content:** `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'User not found' }`
  - **Code:** 413
    - **Content:** `{ 'statusCode': 413, 'error': 'File too large', 'message': 'File too large' }`
  - **Code:** 415
    - **Content:** `{ 'statusCode': 415, 'error': 'Unsupported media type', 'message': 'Invalid content-type header: ${contentType}, expected: application/json or multipart/form-data' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

  **Note**
  - Consumes only [multipart/form-data](https://developer.mozilla.org/en-US/docs/Web/API/FormData)
  - Accepts only image/png, image/jpeg
  - Image should be attached to Formfiled with name *'profile'*
  - Max image size allowed is **2MB**
