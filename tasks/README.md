# Tasks

## Task object

```
{
  "title": "Title of the task",
  "purpose": "<why is this task needed>, optional",
  "featureURL": "<live url of the feature>, optional",
  "type":"feature | group",
  "links": [
    // optional
    <link1>,
    <link2>
  ],
  "endsOn":"<epoch>",
  "startedOn":"<epoch>",
  "status": "active | assigned | blocked | completed | unAssigned",
  "createdBy":"<userId>",
  "assignee":<userId> (in case of individual tasks)",
  "percentCompleted": 0,
  "dependsOn": [
    // optional
    <task_id>,
    <task_id>
  ],
  "participants": [
    // for group tasks
    // optional
    <user_id>
    <user_id>
  ],
  "completionAward": { dinero: 1500, neelam: 1 },
  "lossRate": { dinero: 100 }  // Loss per day of overshoot after deadline,
  'isNoteworthy': true
}
```

## **Requests**

|               Route                |    Description    |
| :--------------------------------: | :---------------: |
|      [GET /tasks](#get-tasks)      | Returns all tasks |
|      [GET /tasks/self](#get-tasksself)      | Returns all tasks of a user |
|     [POST /tasks](#post-tasks)     | Creates new task  |
| [PATCH /tasks/:id](#patch-tasksid) |   Updates tasks   |
| [GET /tasks/:username](#get-tasksusername) |  Returns all tasks of the user |

## **GET /tasks**

Returns all the tasks

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
  message: 'Tasks returned successfully!'
  tasks: [
           {<task_object>},
           {<task_object>}
         ]
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`


## **GET /tasks/self**

Returns all the completed tasks of user if query `completed=true` is passed, else returns all the active and blocked tasks of the user.

- **Params**  
  None
- **Query**  
  completed=[boolean]
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
[
  {<task_object>},
  {<task_object>},
  {<task_object>},
  {<task_object>},
  {<task_object>}
]
```

- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 404
    - **Content:** `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'User doesn't exist' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **GET /tasks/:username**

Returns all tasks of the requested user.

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
  message: 'Tasks returned successfully!'
  tasks: [
           {<task_object>},
           {<task_object>}
         ]
}
```

- **Error Response:**
  - **Code:** 404
    - **Content:** `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'User doesn't exist' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`


## **POST /tasks**

- **Params**  
  None
- **Query**  
  None
- **Headers**  
  Content-Type: application/json
- **Body** `{ <task_object> }`
- **Success Response:**
- **Code:** 200
  - **Content:**

```
{
  message: 'Task created successfully!'
  task: {<task_object>}
  id: <newly created task id>
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **PATCH /tasks/:id**

- **Params**  
  _Required:_ `id=[string]`

- **Headers**  
  Content-Type: application/json
- **Body** `{ <task_object> }`
- **Success Response:**
- **Code:** 204

  - **Content:** `<No Content>`

- **Error Response:**
  - **Code** 404
    - **Content** `{ 'statusCode': 404, 'error': 'Not found', 'message': 'No tasks found' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`
