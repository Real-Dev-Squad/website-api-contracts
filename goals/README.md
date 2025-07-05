# Goals

## Goal object

```
{
  "title": "Title of the goal",
  "status": "active | pending | completed",
  "assignedTo": [
    <member_id>,
    <member_id>,
  ],
  "assignedBy": "<admin>",
  "assignedOn": "<epoch>",
  "completionAward": "100 dineros",
}
```

## **Requests**

|               Route                |    Description    |
| :--------------------------------: | :---------------: |
|      [GET /goals](#get-goals)      | Returns all goals |
|      [GET /goals/:id](#get-goalsid)      | Returns goal with given id |
|     [POST /goals](#post-goals)     | Creates new goal  |
| [PATCH /goals/:id](#patch-goalsid) |   Updates goals   |
| [GET /goals/username/:username](#get-goalsusernameusername) |  Returns all goals of the user |

## **GET /goals**

Returns all the goals

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
  message: 'Goals returned successfully!'
  goals: [
           {<goal_object>},
           {<goal_object>}
         ]
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`


## **GET /goals/:id**

Returns the specified goal.

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
  - **Content:** `{ 'message': 'Goal with <id> returned successfully!', 'user': <user_object> }`
- **Error Response:**
  - **Code:** 404
    - **Content:** `{ error: 'Not Found', message: 'Goal with <id> not found' }`
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
    

## **POST /goals**

Creates new goals

- **Params**  
  None
- **Query**  
  None
- **Headers**  
  Content-Type: application/json
- **Body**
  `{ <goal_object> }`
- **Success Response:**
  - **Code:** 200
    - **Content:**

```
{
  goal: {<goal_object>},
  message: 'Goal created successfully!'
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **PATCH /goals/:id**

Updates an existing goal

- **Params**  
  _Required:_ `id=[string]`

- **Headers**  
  Content-Type: application/json
- **Body**
  `{ <goal_object> }`
- **Success Response:**
  - **Code:** 204
    - **Content:** `<No Content>`

- **Error Response:**
  - **Code** 404
    - **Content** `{ 'statusCode': 404, 'error': 'Not found', 'message': 'No goals found' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **GET /goals/username/:username**

Returns all goals of the requested user.

- **Params**  
  _Required:_ `username=[string]`
- **Query**  
  None
- **Body**  
  None
- **Headers**  
  Content-Type: application/json
- **Success Response:**
  - **Code:** 200
    - **Content:**
```
{
  goals: [
           {<goal_object_containing_username>},
           {<goal_object_containing_username>}
         ],
  message: 'Goals returned successfully!'
}
```

- **Error Response:**
  - **Code:** 404
    - **Content:** `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'User doesn't exist' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`