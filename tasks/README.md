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

## Response

```
[
  {<task_object>},
  {<task_object>}
]

```

## **POST /tasks**

## Body

```
{<task_object>}
```

## Response

Return the same response from [GET /tasks](#get-tasks)

## **PATCH /tasks/:id**

## Body

```
{<task_object>}
```