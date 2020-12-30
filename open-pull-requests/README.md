# Open Pull Requests

## Open Pull Request Object

```
{
    'title': <title of PR>,
    'url': <url of PR>,
    'state': <open or closed>,
    'createdAt': <time of PR created>,
    'updatedAt': <last event: commit or review or closing of PR>,
    'username': <username who requested PR>
    'labels': <labels array>,
    'assignees': <username of the assignee>
}
```

## **Requests**

|               Route                |           Description           |
| :--------------------------------: | :-----------------------------: |
|      [GET /pullrequests/open](#get-pullrequestsopen)      | Returns 10 latest open PRs in RDS |


## **GET /pullrequests/open**

Returns 10 latest open PRs in Real-Dev-Squad organisation

- **Success Response:**
- **Code:** 200
  - **Content:**

```
{
  message: 'Open PRs'
  pullRequests: [
           {<Pull Request Object>},
           {<Pull Request Object>},
           {<Pull Request Object>},
           {<Pull Request Object>},
           {<Pull Request Object>}
         ]
}
```

- **Success Response:**
- **No Open PRs found**
- **Code:** 200
  - **Content:**

```
{
  message: 'No pull requests found!'
  pullRequests: []
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'Something went wrong. Please contact admin' }`
