# Users Status

## Users Status object

```
{
  "userId": "String",
  "currentStatus":  {
      "state": "OOO | IDLE | ACTIVE",
      "updatedAt": "TimeStamp",
      "from": "TimeStamp ",
      "until": "TimeStamp",
      "message": "String"
    },
  "futureStatus":  {
      "state": "OOO | IDLE | ACTIVE",
      "updatedAt": "TimeStamp",
      "from": "TimeStamp ",
      "until": "TimeStamp",
      "message": "String"
    },
  "monthlyHours": {
      "committed": "Integer",
      "updatedAt": "TimeStamp"
    }
}
```

**Note:** The futureStatus can only be updated internally. It is used to store future status. and when the right times arrive it updates the current status with the future staus.A user can only send current Status in patch request.

## **Requests**

|                          Route                          |                           Description                           |
| :-----------------------------------------------------: | :-------------------------------------------------------------: |
|          [GET /users/status](#get-usersstatus)          |     Returns the Users Status of all the Users in the system     |
|     [GET /users/status/self](#get-usersstatusself)      |     Returns the Users Status details of the logged in User      |
|    [GET /users/status/:id](#patch-usersstatususerid)    |         Returns the User Status data with the given id          |
|   [PATCH /users/status/self](#patch-usersstatusself)    | Creates or Updates a new User Status data of the logged in User |
| [PATCH /users/status/:userId](#patch-usersstatususerid) |   Creates or Updates a new User Status data with the given id   |
|     [DELETE /users/self](#delete-usersstatususerid)     |   Deletes the User Status data of the User with the given id    |
|  [PATCH /users/status/batch](#patch-usersstatusbatch)   |      Batch Update the UserIds of the passed users to Idle       |

## **GET /users/status**

Returns the users status of all the users in the system.

- **Params**  
  None
- **Query**

  - state=[OOO | IDLE | ACTIVE]
  - batch = boolean

- **Body**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 200 - **Content:** `{
  message: 'All User Status found successfully.'
  users: [
          {<user_status_object>}
        ]
}`
- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **GET /users/status/self**

Returns the Users Status details of the logged in User.

- **Params**  
  None
- **Query**
  None
- **Body**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 200
    - **Content:** `{ 'id':'documentId' ,'userId':'userId', data:<user_status_object>,"message": User Status found successfully. }`
- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 404
    - **Content:** `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'User Status couldn't be found.' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'The User Status could not be found as an internal server error occurred.' }`

## **GET /users/status/:userId**

Returns the User Status data with the given User Id.

- **Params**  
  _Required:_ `userId=[string]`
- **Body**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Success Response:**
- **Code:** 200
  - **Content:** `{ 'id':'documentId' ,'userId':'userId', data:<user_status_object>,"message": User Status found successfully. }`
- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 404
    - **Content:** `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'User Status couldn't be found.' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'The User Status could not be found as an internal server error occurred.' }`

## **PATCH /users/status/self**

Creates or Updates a new User Status data of the logged in User.

- **Params**  
  None
- **Query**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Body**<br/>

  - Attributes:
    - **currentStatus** (optional, object):
      - **state** (required, string): Specifies the user's state, which must be one of the following values: `OOO`, or `ONBOARDING`.
      - **from** (required, number): Specifies the timestamp from which the status is valid.
      - **until** (optional, number): Specifies the timestamp until which the status is valid, if the state is `OOO`.required for OOO.
      - **message** (optional, string): Specifies a message associated with the status, if the state is `OOO`.
    - **monthlyHours** (optional, object):
      - **committed** (required, number): Specifies the number of hours committed by the user for the current month.
    - **cancelOOO** (optional, boolean): If set to `true`, the API will cancel the user's OOO status and update it to either `IDLE` or `ACTIVE`

- **Success Response:**
  - **Code:** 201|200
    - **Content:** `{'id':'documentId' ,'userId':'userId','data': <user_object>,"message": "User Status created successfully. | User Status updated successfully. " }`
- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **PATCH /users/status/:userId**

Updates data of the User.

- **Params**  
  _Required:_ `userId=[string]`
- **Query**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Body**

  - Attributes:
    - **currentStatus** (optional, object):
      - **state** (required, string): Specifies the user's state, which must be one of the following values: `OOO`, or `ONBOARDING`.
      - **from** (required, number): Specifies the timestamp from which the status is valid.
      - **until** (optional, number): Specifies the timestamp until which the status is valid, if the state is `OOO`.required for OOO.
      - **message** (optional, string): Specifies a message associated with the status, if the state is `OOO`.
    - **monthlyHours** (optional, object):
      - **committed** (required, number): Specifies the number of hours committed by the user for the current month.
    - **cancelOOO** (optional, boolean): If set to `true`, the API will cancel the user's OOO status and update it to either `IDLE` or `ACTIVE`

- **Success Response:**
  - **Code:** 201|200
    - **Content:** `{'id':'documentId' ,'userId':'userId','data': <user_object>,"message": "User Status created successfully. | User Status updated successfully. " }`
- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' | 'You are not authorized to perform this action.'  }`
  - **Code:** 404
    - **Content:** `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'The User doesn't exist.' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **PATCH /users/status/batch**

Batch Updates the UserIds of the passed user list to Idle if the user is not OOO or Onboarding. If the User is OOO then their future status is updated. Please note that this is a restricted route intended for super users only.

- **Params**
  None
- **Query**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Body**

  - Attributes:
    - users (required, array): An array of objects, each representing a user with the following properties:
      - userId (required, string): Specifies the unique identifier for the user.
      - expectedState (required, string): Specifies the user's state, which must be one of the following values: IDLE or ACTIVE.
  - Example:

```
  {
      "users": [
          {
              "userId": "4kAkRv9TBlOfR6WEUhoQ",
              "expectedState": "IDLE"
          },
          {
              "userId": "SooJK37gzjIZfFNH0tlL",
              "expectedState": "ACTIVE"
          }
      ]
  }
```

- **Success Response:**

  - **Code**
    - 200</br>
  - **Content**</br>

```
    {
      "message": "String",
      "data": {
        "totalUsers": "Number",
        "totalUnprocessedUsers": "Number",
        "totalOnboardingUsersAltered": "Number",
        "totalOnboardingUsersUnAltered": "Number",
        "totalActiveUsersAltered": "Number",
        "totalActiveUsersUnAltered": "Number",
        "totalIdleUsersAltered": "Number",
        "totalIdleUsersUnAltered": "Number"
      }
    }
```

- **Example**

```
  {
    "message": "users status updated successfully.",
    "data": {
        "totalUsers": 7,
        "totalUnprocessedUsers": 1,
        "totalOnboardingUsersAltered": 1,
        "totalOnboardingUsersUnAltered": 1,
        "totalActiveUsersAltered": 1,
        "totalActiveUsersUnAltered": 1,
        "totalIdleUsersAltered": 1,
        "totalIdleUsersUnAltered": 1
    }
}
```

- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'The server has encountered an unexpected error. Please contact the administrator for more information.' }`

## **DELETE /users/status/:userId**

Deletes the User Status data of the User with the given id.

- **Params**
  _Required:_ `userId=[string]`
- **Query**
  None
- **Headers**
  Content-Type: application/json
- **Cookie**
  rds-session: `<JWT>`
- **Body**
  None
- **Success Response:**
  - **Code:** 201|200
    - **Content:** `{'id':'documentId' ,'userId':'userId',"message": "User Status deleted successfully." }`
- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 404
    - **Content:** `{ 'statusCode': 404, 'id': null ,'userId':'userId',"message": "User Status to delete not found." }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

```

```
