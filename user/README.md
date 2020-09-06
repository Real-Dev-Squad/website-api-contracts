# Users

## User object

```
{
  'id': string,
  'first_name': string,
  'last_name': string,
  'yoe': number,
  'company': string,,
  'designation': string,
  'img': string,
  'github_id': string,
  'linkedin_id': string,
  'twitter_id': string,
  'instagram_id': string,
  'site': string,
  'github_display_name': string,
  'isMember': boolean
}
```

## **Requests**

|               Route                |           Description           |
| :--------------------------------: | :-----------------------------: |
|      [GET /users](#get-users)      | Returns all users in the system |
|   [GET /users/:id](#get-usersid)   |   Returns user with given id    |
|     [POST /users](#post-users)     |       Creates a new User        |
| [PATCH /users/:id](#patch-usersid) |     Updates data of a User      |

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
  - **Code:** 404
    - **Content:** `{ error: 'Not Found', message: 'No users available' }`
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`

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
  - **Code:** 400
    - **Content:** `{ error: 'Bad Request', message: 'User already exists' }`
      OR
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`

## **PATCH /users/:id**

Updates data of a User.

- **Params**  
  _Required:_ `id=[string]`
- **Query**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Body** `{ <user_object> }`
- **Success Response:**
  - **Code:** 200
    - **Content:** `{ 'message': 'User updated successfully!' }`
- **Error Response:**
  - **Code:** 404
    - **Content:** `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'User not found' }`
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
