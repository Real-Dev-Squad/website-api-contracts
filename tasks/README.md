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
  "createdAt": "<epoch> timestamp",
  "updatedAt": "<epoch> timestamp",
  "status": "AVAILABLE | ASSIGNED | IN_PROGRESS | BLOCKED | SMOKE_TESTING | COMPLETED | NEEDS_REVIEW | IN_REVIEW | APPROVED | MERGED | SANITY_CHECK | REGRESSION_CHECK | RELEASED | VERIFIED",
  "createdBy":"<userId>",
  "assignee":<userId> (in case of individual tasks)",
  "percentCompleted": 0,
  "dependsOn": [
    // optional
    <task_id>,
    <task_id>
  ],
  "level": 1 | 2 | 3 | 4 | 5 // optional - can be added only if category exists, 
  "category": <string> // optional (example: "FRONTEND" | "BACKEND"),
  "participants": [
    // for group tasks
    // optional
    <user_id>
    <user_id>
  ],
  "completionAward": { dinero: 1500, neelam: 1 },
  "lossRate": { dinero: 100 }  // Loss per day of overshoot after deadline,
  'isNoteworthy': true,
  'github': {
    // optional
    'issue': {
      'status': <string>,
      'assignee': <string>, // optional
      'id': <number>,
      'closedAt': <string>, // optional
      'assigneeRdsInfo': {
        // optional
        firstName: <string>,
        lastName: <string>,
        username: <string>
      }
    }
  }
}
```

## **Requests**

|               Route                |    Description    |
| :--------------------------------: | :---------------: |
|      [GET /tasks](#get-tasks)      | Returns all tasks |
|      [GET /tasks/self](#get-tasksself)      | Returns all tasks of a user |
|     [POST /tasks](#post-tasks)     | Creates new task  |
| [PATCH /tasks/:id](#patch-tasksid) |   Updates tasks   |
| [GET /tasks/:id/details](#get-tasksiddetails)         | Get details of a particular task|
| [GET /tasks/:username](#get-tasksusername) |  Returns all tasks of the user |
| [PATCH /tasks/self/:id](#patch-tasksselfid) |  Changes in own task  |
| [GET /tasks/users/discord](#get-tasksusersdiscord) |  Returns discord details of users with active tasks |

## **GET /tasks**

Returns all the tasks

- **Params**  
  None
- **Query**  
  - Optional: `dev=[boolean]` (`dev` is passed to get all tasks in the developer mode with features that are flagged)
  - Optional: `status=[string]` (`status` is a case insenstive string with one of the following values [AVAILABLE, ASSIGNED, COMPLETED, IN_PROGRESS, BLOCKED, SMOKE_TESTING, NEEDS_REVIEW, IN_REVIEW, APPROVED, MERGED, SANITY_CHECK, REGRESSION_CHECK, RELEASED, VERIFIED, DONE, UNASSIGNED] which represents the status of the task)
  - Optional: `assignee=[string]` (`assignee` can be assignee username in case of single assignee or multiple comma separated values in case of multiple assignee)
  - Optional: `title=[string]` (`title` can be case sensitive, whole or starting portion of task title)
  - Optional: `page=[integer]` (`page` can either be 0 or a positive integer. Default value is 0)
  - Optional: `size=[integer]` (`size` is the number of tasks requested per page. Range of value is 1-100. Default value is 5)
  - Optional: `next=[string]` (`next` is id of the document to get next page of results from that document)
  - Optional: `prev=[string]` (`prev` is id of the document to get prev page of results from that document)
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

## **GET /tasks/:id/details**

Returns details of a particular task

- **Params**  
  _Required:_ `id=[string]`
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
  "message":"task returned successfully",
  "taskData": [
    {<task_object>}
  ]
}
```

- **Error Response:**
  - **Code:** 404
    - **Content:** `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'Task not found' }`
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
  userStatus?: {
    status : "String",
    message : "String"
    data: {
      "previousStatus"?: "String",
      "currentStatus"?: "String",
      "futureStatus"?: "String"
    }
  }
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

## **PATCH /tasks/self/:id**

- **Params**  
  _Required:_ `id=[string]`

- **Headers**  
  Content-Type: application/json

- **Body**  
  ```
  { 
    status: <new-status> 
    percentCompleted: <number>
  }
  ```

- **Cookie**  
  rds-session: `<JWT>`

- **Success Response:**
  - **Code**: 200
```
{
  message: 'Task updated successfully!'
  userStatus?: {
    status : "String",
    message : "String"
    data: {
      "previousStatus"?: "String",
      "currentStatus"?: "String",
      "futureStatus"?: "String"
    }
  }
}
```
- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'User can not be authenticated' }`
  - **Code:** 403
    - **Content:** `{ 'statusCode': 403, 'error': 'Forbidden', 'message':'This task is not assigned to you' }`
  - **Code:** 404
    - **Content:** `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'Task doesn't exist' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **GET /tasks/users/discord**

Returns details of a particular task
- **Params**  
  None
- **Query**  
  - Optional: `q=[string]` (`q` can have the following values)
      - Optional: `status=[string]` (`status` is a case senstive string with one of the following values [missed-updates] )
      - Optional: `date-count=[integer]` (`date-count` is the number of days where the users have not provided an update ont their task default :[3] )
      - Optional: `date=[timestamp]` (`date` is the timestamp of the date that needs to be excluded from the date-count. No default. eg [1702122435405] )
      - Optional: `weekday=[string]` (`weekday` the weekday that needs to be excluded while calculating the missed updates eg: [sun,mon,tue,wed,thu,fri,sat])
  - Optional: `size=[integer]` (`size` is the number of tasks that are processed to find the users. Range of value is 1-1000.)
  - Optional: `cursor=[string]` (`cursor` is id of the document to get next page of results from that document)
  eg: `?size=10&cursor=1xh3Bsd32&q=status:missed-updates -date-count:3 -date:1702122435405 -weekday:sun -weekday:sat`
- **Body**  
  None
- **Headers**  
  Content-Type: application/json
- **Success Response:**
  - **Code:** 200
    - **Content:**
```
{
  usersToAddRole: Array : `List of users who missed update`
  tasks: number : `Total tasks processed`
  missedUpdatesTasks: number : `Tasks with missed updates`
 }
```

- **Error Response:**
  - **Code:** 400
    - **Content:** `{ 'statusCode': 400, 'error': 'Bad request' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`
