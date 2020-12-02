# Pull Requests

## Pull Request Object

```
{
    'title': <title of PR>,
    'url': <url of PR>,
    'state': <open or closed>,
    'createdAt': <time of PR created>,
    'updatedAt': <last event: commit or review or closing of PR>,
    'readyForReview': <true or false>,
    'labels': <labels array>,
    'assignees': <username of the assignee>
}
```

## **Requests**

|               Route                |           Description           |
| :--------------------------------: | :-----------------------------: |
|      [GET /pullrequests/:id](#get-pullrequestsid)      | Returns latest PRs by the user in RDS |


## **GET /pullrequests/:id**

Returns latest pull requests by an user in Real-Dev-Squad organisation

- **Params**  
  `id`

- **Success Response:**
- **Code:** 200
  - **Content:**

```
[
    {<Pull Request Object>},
    {<Pull Request Object>},
    {<Pull Request Object>},
    {<Pull Request Object>},
    {<Pull Request Object>}
]
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'Something went wrong. Please contact admin' }`
