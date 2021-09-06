# Goals

## Goal object

```
{
  "title": "Title of the goal",
  "status": "active | assigned | unAssigned | inProgress | completed",
  "createdBy":"<userId>",
  "participants": [
    <user_id>
    <user_id>
    <user_id>
  ],
  "completionAward": { dinero: 1500, neelam: 1 },
  "lossRate": { dinero: 100 }  // if member fails to complete goal within time
}
```

## **Requests**

|               Route                |    Description    |
| :--------------------------------: | :---------------: |
|      [GET /goals](#get-goals)      | Returns all goals |
| [GET /goals/:username](#get-goalsusername) |  Returns all goals of the user |
|     [POST /goals](#post-goals)     | Creates new goal  |
| [PATCH /goals/:id](#patch-goalsid) |   Updates goals   |

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


## **GET /goals/:username**

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
  message: 'Goals returned successfully!'
  goals: [
           {<goal_object>},
           {<goal_object>}
         ]
}
```

- **Error Response:**
  - **Code:** 404
    - **Content:** `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'User doesn't exist' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`


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
  message: 'Goal created successfully!'
  goal: {<goal_object>}
  id: <newly created goal id>
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