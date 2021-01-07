# Tasks

## Task object

```
{
  "type":"Dev | Group",
  "links": [
    <link1>,
    <link2>
  ],
  "endsOn":"<end date>",
  "startedOn":"<start date>",
  "status": "Active",
  "ownerId":"<app owner user id>",
  "percentCompleted":10%,
  "dependsOn": [
    <tasksList>
  ],
  "participants": [list of users participating in this tasks. For 'group' type only],
  "completionAward": { gold: 3, bronze: 300 },
  "lossRate": { gold: 1 }  // Loss per day of overshoot after deadline
}
```

## **Requests**

|               Route                |    Description    |
| :--------------------------------: | :---------------: |
|      [GET /tasks](#get-tasks)      | Returns all tasks |
|     [POST /tasks](#post-tasks)     | Creates new task  |
| [PATCH /tasks/:id](#patch-tasksid) |   Updates tasks   |

## **GET /tasks**

> returns all the tasks

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
  - **Code:** 503
    - **Content:** `{ 'statusCode': 503, 'error': 'Service Unavailable', 'message': 'Something went wrong please contact admin' }`

## **POST /tasks**

- **Headers**  
  Content-Type: application/json
- **Body** `{ <task_object> }`
- **Success Response:**
- **Code:** 200
  - **Content:** `{ 'message': 'Task created successfully!', 'tasks': [{<task_object>},{<task_object>},] }`

Return the same response from [GET /tasks](#get-tasks)

- **Error Response:**
  - **Code:** 503
    - **Content:** `{ 'statusCode': 503, 'error': 'Service Unavailable', 'message': 'Something went wrong please contact admin' }`

## **PATCH /tasks/:id**

- **Params**  
  _Required:_ `id=[string]`

- **Headers**  
  Content-Type: application/json
- **Body** `{ <task_object> }`
- **Success Response:**
- **Code:** 200

  - **Content:** `{ 'message': 'Task updated successfully!' }`

- **Error Response:**
  - **Code:** 503
    - **Content:** `{ 'statusCode': 503, 'error': 'Service Unavailable', 'message': 'Something went wrong please contact admin' }`
