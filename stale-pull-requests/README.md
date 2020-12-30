# Stale Pull Request

## Stale Pull Request Object

```
{
    'title': <title of PR>,
    'url': <url of PR>,
    'state': <open or closed>,
    'createdAt': <time of PR created>,
    'updatedAt': <last event: commit or review or closing of PR>,
    'username': <username who requested PR>,
    'labels': <labels array>,
    'assignees': <username of the assignee>
}
```

## **Requests**

|               Route                |           Description           |
| :--------------------------------: | :-----------------------------: |
|      [GET /pullrequests/stale](#get-pullrequestsstale)       | Returns stale PRs in RDS |


## **GET /pullrequests/stale**

Returns stale pull requests in Real-Dev-Squad organisation

- **Success Response:**
- **Code:** 200
  - **Content:**

```
{
  message: 'Stale PRs'
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
- **No Stale PRs found:**
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
