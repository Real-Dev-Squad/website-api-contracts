# Badges

## Badge object

```
{
  'id': string,
  'name': string,
  'description': string,
  'imageUrl': string,
  'createdBy': string,
  'createdAt': { 'date': string, 'time': string }
}
```

## **Requests**

|              Route                                                      |           Description                   |
|:-----------------------------------------------------------------------:|:---------------------------------------:|
|    [GET /badges](#get-badges)                                           |   Returns all badges in the system      |
|    [GET /badges/:username](#get-badgesusername)                         |   Returns badges assigned to the user   |
|    [POST /badges](#post-badges)                                         |   Creates a new badge                   |
|    [POST /badges/assign/:username](#post-badgesassignusername)          |   Assigns a badge to the user           |
|    [DELETE /badges/unassign/:username](#delete-badgesunassignusername)  |   Unassigns a badge from user badges    |


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


## **GET /badges/:username**

Returns badges assigned to the user

- **Params**  
  Required: `username=[string]`
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
  - **Content:** `{ 'message': 'Badges returned succesfully', 'badges': Array<badge_object> }` 
- **Error Response:**
  - **Code:** 400
    - **Content:** `{ 'statusCode': 400, 'error': 'Bad Request', 'message': 'Failed to get user badges.' }`


## **POST /badges**

Creates a new badge

- **Params**  
  None
- **Query**  
  None
- **Body**  
  Required: `file=[File Object]`
  <br>
  Required: `name=[string]`
  <br>
  Required: `createdBy=[string]`
  <br>
  Optional: `description=[string]` (`description` default value is empty string(''))
- **Headers**  
  Content-Type: multipart/form-data
- **Cookie**  
  rds-session: `JWT`
- **Success Response:**
  - **Code:** 200
    - **Content:** `{ 'message': 'Badge created succesfully', 'badge': {<badge_object>} }` 
- **Error Response:**
  - **Code:** 400 
    - **Content:** `{ 'statusCode': 400, 'error': 'Bad Request', 'message': 'API payload failed validation, <validation_fail_message>' }`
  - **Code:** 401 
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated user' }`
  - **Code:** 422 
    - **Content:** `{ 'statusCode': 422, 'error': 'Unprocessable Entry', 'message': 'Only one file allowed' }`
 

## **POST /badges/assign/:username**

Assigns a badge to the user

- **Params**  
  Required: `username=[string]` 
- **Query**  
  None
- **Body**  
  Required: `badgeIds=Array<string>` (`badgeIds` allows unique and non-empty ids)
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `JWT`
- **Success Response:**
  - **Code:** 200
    - **Content:** `{ 'message': 'Badges assigned succesfully' }` 
- **Error Response:**
  - **Code:** 400 
    - **Content:** `{ 'statusCode': 400, 'error': 'Bad Request', 'message': 'API payload failed validation, <validation_fail_message>' }`


## **DELETE /badges/unassign/:username**

Unassigns a badge from user badges

- **Params**  
  Required: `username=[string]` 
- **Query**  
  None
- **Body**  
  Required: `badgeIds=Array<string>` (`badgeIds` allows unique and non-empty ids)
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `JWT`
- **Success Response:**
  - **Code:** 200
    - **Content:** `{ 'message': 'Badges unassigned succesfully' }` 
- **Error Response:**
  - **Code:** 400 
    - **Content:** `{ 'statusCode': 400, 'error': 'Bad Request', 'message': 'API payload failed validation, <validation_fail_message>' }`


