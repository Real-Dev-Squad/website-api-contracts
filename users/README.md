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

**Note:**: Only the GET `users/self` route will return `phone` and `email` if
`private` query is passed as true. This way we are not exposing users' phone
numbers and email addresses to everyone. Users can only see their own phone
number and email address.

## **Requests**

|                         Route                          |             Description              |
| :----------------------------------------------------: | :----------------------------------: |
|                [GET /users](#get-users)                |   Returns all users in the system    |
|           [GET /users/self](#get-usersSelf)            | Returns the logged in user's details |
|  [GET /users/userId/:userId](#get-usersuseriduserid)   |    Returns user with given userId    |
|       [GET /users/:username](#get-usersusername)       |   Returns user with given username   |
|    [GET /users/:userId/badges](#get-usersidbadges)     | Returns badges assigned to the user  |
|               [POST /users](#post-users)               |          Creates a new User          |
|         [PATCH /users/self](#patch-usersself)          |       Updates data of the User       |
| [PATCH /users/:id/temporary/data](#patch-usersidroles) |          Updates user roles          |
|              [PATCH /users](#patch-users)              |   Archive users if not in discord    |

## **GET /users**

Returns all users in the system.

- **Params**  
  None
- **Query**
  - Optional: `size=[integer]` (`size` is number of users requested per page,
    value ranges in between 1-100, and default value is 100)
  - Optional: `page=[integer]`
    (`page` can either be 0 or positive-number, and default value is 0)
  - Optional: `search=[string]` (`search` is a string value for username prefix)
  - Optional: `next=[string]` (`next` is id of the DB document to get next batch/page of results after that document.)
  - Optional: `prev=[string]` (`prev` is id of the DB document to get previous batch/page of results before that document.)
  - Optional: `query=[string]` ( `query` can be used to filter and/or sort users based on their PR and Issue status within a given date range. [Learn more](https://github.com/Real-Dev-Squad/website-backend/wiki/Filter-and-sort-users-based-on-PRs-and-Issues) )
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
  links: {
    next: '/users?next={<DB document id>}&size={number}&search={string}',
    prev: '/users?prev={<DB document id>}&size={number}&search={string}'
  }
}
```

- **Error Response:**
  - **Code:** 401
    - **Content:**
      `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`

## **GET /users/self**

Returns the details of logged in user.

- **Params**  
  None
- **Query** private=[boolean] private=[boolean]
- **Body**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 200
    - **Content:** `{ <user_object> }`
      > **Note**: The user object will include `phone` and `email` only when the
      > query `private` is passed as `true` for this route. No other route will
      > return `phone` and `email`.
- **Error Response:**
  - **Code:** 401
    - **Content:**
      `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 404
    - **Content:**
      `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'User doesn't exist' }`
  - **Code:** 500
    - **Content:**
      `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **GET /users/userId/:userId**

Returns the specified user.

- **Params**  
  _Required:_ `userId=[string]`
- **Body**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  None
- **Success Response:**
- **Code:** 200
  - **Content:**
    `{ 'message': 'User returned successfully!', 'user': <user_object> }`
- **Error Response:**
  - **Code:** 404
    - **Content:** `{ error: 'Not Found', message: 'User doesn't exist' }`
  - **Code:** 500
    - **Content:**
      `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'Error while formatting phone and email' }`
  - **Code:** 503
    - **Content:**
      `{ 'statusCode': 503, 'error': 'Service Unavailable', 'message': 'Something went wrong please contact admin' }`

## **GET /users/:username**

Returns the specified user.

- **Params**  
  _Required:_ `username=[string]`
- **Body**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  None
- **Success Response:**
- **Code:** 200
  - **Content:**
    `{ 'message': 'User returned successfully!', 'user': <user_object> }`
- **Error Response:**
  - **Code:** 404
    - **Content:** `{ error: 'Not Found', message: 'User doesn't exist' }`
  - **Code:** 503
    - **Content:**
      `{ 'statusCode': 503, 'error': 'Service Unavailable', 'message': 'Something went wrong please contact admin"' }`

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
    - **Content:**
      `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 500
    - **Content:**
      `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **GET /users/:id/badges**

Returns badges assigned to the user

- **Params**  
  Required: `id=[string]`
- **Query**  
  None
- **Body**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  None
- **Success Response:**
- **Code:** 200
  - **Content:**
    `{ 'message': 'Badges returned succesfully', 'badges': Array<badge_object> }`
- **Error Response:**
  - **Code:** 400
    - **Content:**
      `{ 'statusCode': 400, 'error': 'Bad Request', 'message': 'Failed to get user badges.' }`

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
    - **Content:**
      `{ "statusCode": 409, "error": "Conflict", "message": "User already exists" }`
  - **Code:** 401
    - **Content:**
      `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`

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
    - **Content:**
      `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'User not found' }`
  - **Code:** 401
    - **Content:**
      `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 403
    - **Content:**
      `{ 'statusCode': 403, 'error': 'Forbidden', 'message': 'Cannot update username again'}`
  - **Code:** 503
    - **Content:**
      `{ 'statusCode': 503, 'error': 'Service Unavailable', 'message': 'Something went wrong please contact admin' }`

## **PATCH /users/:id/temporary/data**

Updates roles for the User.

- **Params**  
  _Required:_ `userId=[string]`
- **Query**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Body**
  `{
  member?: <boolean>
  archived?: <boolean>
}`
- **Success Response:**
  - **Code:** 200
    - **Content:** `{ 'message': 'role updated successfully!'}`
- **Error Response:**
  - **Code:** 409
    - **Content:**
      `{ 'statusCode': 409, 'error': 'Not Found', 'message': 'role already exist!' }`
  - **Code:** 404
    - **Content:**
      `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'User not found' }`
  - **Code:** 400
    - **Content:**
      `{ 'statusCode': 400, 'error': 'Invalid Request', 'message': 'Invalid role' }`
  - **Code:** 401
    - **Content:**
      `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 500
    - **Content:**
      `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## PATCH /users

Archive users if not in Discord.

- **Params**  
  None
- **Query**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<SUPERUSER JWT>`
- **Body**  
  None
- **Success Response:**

  - **Code:** 200
    - **Content:**
    ```json
    {
      "message": "Successfully updated users archived role to true if in_discord role is false |
                  Couldn't find any users currently inactive in Discord but not archived.",
      "data": {
        "totalUsers": "number",
        "totalUsersArchived": "number",
        "totalOperationsFailed": "number"
      }
    }
    ```

- **Error Response:**

  - **Code:** 401
    - **Content:**
    ```json
    {
      "statusCode": 401,
      "error": "Unauthorized",
      "message": "Unauthenticated User"
    }
    ```
  - **Code:** 500
    - **Content:**
    ```json
    {
      "statusCode": 500,
      "error": "Internal Server Error",
      "message": "An internal server error occurred"
    }
    ```
