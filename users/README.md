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
  'github_user_id': string,
  'linkedin_id': string,
  'twitter_id': string,
  'instagram_id': string,
  'website': string,
  'github_display_name': string,
  'isMember': boolean,
  'userType': string,
  'discordId': string,
  'tokens': {},
  'badges': []
  'disabled_roles':[]
}
```

**Note:**: Only the GET `users/self` route will return `phone` and `email` if
`private` query is passed as true. This way we are not exposing users' phone
numbers and email addresses to everyone. Users can only see their own phone
number and email address.

## **Requests**

|                         Route                          |               Description                |
| :----------------------------------------------------: | :--------------------------------------: |
|                [GET /users](#get-users)                |     Returns all users in the system      |
|           [GET /users/self](#get-usersSelf)            |   Returns the logged in user's details   |
|  [GET /users/userId/:userId](#get-usersuseriduserid)   |      Returns user with given userId      |
|       [GET /users/:username](#get-usersusername)       |     Returns user with given username     |
|    [GET /users/:userId/badges](#get-usersidbadges)     |   Returns badges assigned to the user    |
|         [GET /users/search](#get-users-search)         | Returns users based on specified filters |
|               [POST /users](#post-users)               |            Creates a new User            |
|         [PATCH /users/self](#patch-usersself)          |         Updates data of the User         |
| [PATCH /users/:id/temporary/data](#patch-usersidroles) |            Updates user roles            |
|              [PATCH /users](#patch-users)              |     Archive users if not in discord      |

## **GET /users**

Returns all users in the system.

- **Params**
  None
- **Query**
  - Optional: `profile=["true"]` (if profile is set to true, it will return currently logged-in users data.)
  - Optional: `size=[integer]` (`size` is the number of users requested per page, value ranges between 1-100, and the default value is 100)
  - Optional: `page=[integer]` (`page` can either be 0 or a positive number, and the default value is 0)
  - Optional: `search=[string]` (`search` is a string value for username prefix)
  - Optional: `next=[string]` (`next` is the id of the DB document to get the next batch/page of results after that document.)
  - Optional: `prev=[string]` (`prev` is the id of the DB document to get the previous batch/page of results before that document.)
  - Optional: `query=[string]` (`query` can be used to filter and/or sort users based on their PR and Issue status within a given date range. [Learn more](https://github.com/Real-Dev-Squad/website-backend/wiki/Filter-and-sort-users-based-on-PRs-and-Issues) )
  - Optional: `departed=[boolean]` ( if departed is set to true with dev feature flag as true, it will return all the users who have departed the discord server with pending tasks assigned to them. )
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
      "message": "Users returned successfully!",
      "users": [
        { <user_object> }
      ],
      "links": {
        "next": '/users?next={<DB document id>}&size={number}&search={string}',
        "prev": '/users?prev={<DB document id>}&size={number}&search={string}'
      }
    }
    ```

  **If `/users?profile=true`**

  - **Code:** 200

    - **Content:**

    ```
    {
      <user_object>
    }
    ```

  **If `/users?departed=true&dev=true`**

  - **Code:** 204 (for `departed=true` when no abandoned tasks exist)

    - **Content:**
      `{}`

- **Error Response:**
  - **Code:** 401
    - **Content:**
      `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 404 (for `departed=true` without `dev=true`)
    - **Content:**
      `{ 'message': 'Route not found' }

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

## **GET /users/search**

Returns users based on the specified filters.

- **Params:**
  None

- **Query Parameters:**

  - Optional: `levelId=[string]` (Specifies the level ID)
  - Optional: `levelName=[string]` (Specifies the level name)
  - Optional: `levelValue=[number]` (Specifies the level value)
  - Optional: `tagId=[string]` (Specifies the tag ID)
  - Optional: `state=[string]` (Specifies the user state. Possible values: "ACTIVE", "OOO", "IDLE", "ONBOARDING", "ONBOARDING31DAYS". This parameter can be repeated for multiple states.)
  - Optional: `role=[string]` (Specifies the user role, valid values are "MEMBER", "INDISCORD", "ARCHIVED")
  - Optional: `verified=[string]` (Specifies if the user is verified. Possible values: "true", "false")
  - Optional: `time=[string]` (Specifies the time filter, e.g., "31d")

- **Body:**
  None

- **Headers:**
  Content-Type: application/json
  rds-session: `<JWT>`

- **Success Response:**

  - **Code:** 200

    - **Content:**

    ```
    {
      "message": "Users found successfully!",
      "users": [
        { <user_object> }
      ],
      "links": {
        "next": "/users/search?next={<DB document id>}&page={number}&size={number}&dev={boolean}&state=ACTIVE&state=OOO&state=IDLE&state=ONBOARDING&time=31d",
        "prev": "/users/search?prev={<DB document id>}&page={number}&size={number}&dev={boolean}&state=ACTIVE&state=OOO&state=IDLE&state=ONBOARDING&time=31d"
      },
      "count": Number
    }
    ```

- **Error Response:**

  - **Code:** 400

    - **Content:**
      `{ 'statusCode': 400, 'error': 'Bad Request', 'message': 'Filter for item not provided' }`

  - **Code:** 401

    - **Content:**
      `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`

  - **Code:** 503
    - **Content:**
      `{ 'statusCode': 503, 'error': 'Service Unavailable', 'message': 'Something went wrong, please contact admin' }`

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

## **PATCH /users/self(To Be Deprecated)**

> **⚠️ Deprecation Notice**
>
> This endpoint is scheduled for deprecation. A new endpoint will be announced in the future to replace this functionality.
> Please prepare to update your integrations accordingly.

Updates data of the User. Doesn't update if user is `(in_discord && !userDetailsIncomplete)`, Except for `disabled_roles` property.

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
  - **Code:** 200
    - **Content** `{ message: "Privilege modified successfully!", disabled_roles: string[] }`
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
      `{ 'statusCode': 403, 'error': 'Forbidden', 'message': 'Developers can only update disabled_roles. Use profile service for updating other attributes.'}`
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
  - Optional: `debug=[boolean]`(`debug` when set to true returns additional result in response to help debugging)
- **Headers**
  Content-Type: application/json
- **Cookie**
  rds-session: `<SUPERUSER JWT>`
- **Body**

  ```json
  {
    "action": "nonVerifiedDiscordUsers | archiveUsers"
  }
  ```

- **Success Response:**
  - **Code:** 200
    - **Content:**

```json
{
  "message": "Successfully updated users archived role to true if in_discord role is false | Couldn't find any users currently inactive in Discord but not archived.",
  "data": {
    "totalUsers": "number",
    "totalUsersArchived": "number",
    "totalOperationsFailed": "number"
  }
}
```

**Addition info if debug query is set to true**

```json
{
  "message": "Successfully updated users archived role to true if in_discord role is false | Couldn't find any users currently inactive in Discord but not archived.",
  "data": {
    "totalUsers": "number",
    "totalUsersArchived": "number",
    "totalOperationsFailed": "number",
    "updatedUserDetails": "array",
    "failedUserDetails": "array"
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

- **Code:** 400

  - **Content:**

```json
{
  "statusCode": 400,
  "error": "Bad Request",
  "message": "Invalid payload"
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
