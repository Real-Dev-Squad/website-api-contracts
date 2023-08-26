# Badges

## Badge object

```
{
  'id': string,
  'name': string,
  'description': string,
  'imageUrl': string,
  'createdBy': string,
  'createdAt': { 'date': string, 'time': string },
  'discordId': string
}
```

## **Requests**

|                     Route                     |            Description             |
| :-------------------------------------------: | :--------------------------------: |
|          [GET /badges](#get-badges)           |  Returns all badges in the system  |
|         [POST /badges](#post-badges)          |        Creates a new badge         |
|   [POST /badges/assign](#post-badgesassign)   |     Assigns badges to the user     |
| [DELETE /badges/remove](#delete-badgesremove) | Remove assigned badges from a user |

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
    - **Content:**
      `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'Something went wrong. Please contact admin' }`

## **POST /badges**

Creates a new badge

- **Params**  
  None
- **Query**  
  None
- **Body**  
  Required: `file=[File Object]` <br> Required: `name=[string]` <br> Required:
  `createdBy=[string]` <br> Optional: `description=[string]` (`description`
  default value is empty string(''))
- **Headers**  
  Content-Type: multipart/form-data
- **Cookie**  
  rds-session: `JWT`
- **Authorize Roles:** SUPERUSER
- **Success Response:**
  - **Code:** 200
    - **Content:**
      `{ 'message': 'Badge created succesfully', 'badge': {<badge_object>} }`
- **Error Response:**
  - **Code:** 400
    - **Content:**
      `{ 'statusCode': 400, 'error': 'Bad Request', 'message': 'API payload failed validation, <validation_fail_message>' }`
  - **Code:** 401
    - **Content:**
      `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated user' }`
  - **Code:** 422
    - **Content:**
      `{ 'statusCode': 422, 'error': 'Unprocessable Entry', 'message': 'Only one file allowed' }`

## **POST /badges/assign**

Assigns a badge to the user

- **Params**  
  None
- **Query**  
  None
- **Body**  
  Required: `badgeIds=Array<string>` (`badgeIds` allows unique and non-empty
  ids) <br> Required: `userId=[string]`
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `JWT`
- **Authorize Roles:** SUPERUSER
- **Success Response:**
  - **Code:** 200
    - **Content:** `{ 'message': 'Badges assigned succesfully' }`
- **Error Response:**
  - **Code:** 400
    - **Content:**
      `{ 'statusCode': 400, 'error': 'Bad Request', 'message': 'API payload failed validation, <validation_fail_message>' }`

## **DELETE /badges/remove**

Remove assigned badges from a user

- **Params** None
- **Query**  
  None
- **Body**  
  Required: `badgeIds=Array<string>` (`badgeIds` allows unique and non-empty
  ids) <br> Required: `userId=[string]`
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `JWT`
- **Authorize Roles:** SUPERUSER
- **Success Response:**
  - **Code:** 200
    - **Content:** `{ 'message': 'Badges removed succesfully' }`
- **Error Response:**
  - **Code:** 400
    - **Content:**
      `{ 'statusCode': 400, 'error': 'Bad Request', 'message': 'API payload failed validation, <validation_fail_message>' }`
