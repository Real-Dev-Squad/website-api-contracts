# Pull Requests

## Pull Request Object

```
{
    'title': <title of PR>,
    'username': <author of the pull request>,
    'url': <url of PR>,
    'state': <open or closed>,
    'createdAt': <time of PR created>,
    'updatedAt': <last event: commit or review or closing of PR>,
    'repository': <name of the repository in which PR is requested>,
    'labels': <labels array>,
    'assignees': <username of the assignee>
}
```

## **Requests**

|               Route                |           Description           |
| :--------------------------------: | :-----------------------------: |
|      [GET /pullrequests/user/:username](#get-pullrequestsuserusername)      | Returns latest PRs by the user in RDS |
|      [GET /pullrequests/open](#get-pullrequestsopen)      | Returns n latest open PRs in RDS |
|      [GET /pullrequests/stale](#get-pullrequestsstale)       | Returns n stale PRs in RDS |


## **GET /pullrequests/user/:username**

Returns latest pull requests by an user in Real-Dev-Squad organisation

- **Params**  
  `username`

- **Success Response:**
- **Code:** 200
  - **Content:**

```
{
  message: 'Pull requests returned successfully!'
  pullRequests: [
           {<Pull Request Object>},
           {<Pull Request Object>},
           {<Pull Request Object>},
           {<Pull Request Object>},
           {<Pull Request Object>}
         ]
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'Something went wrong. Please contact admin' }`

## **GET /pullrequests/open**

Returns n latest open PRs in Real-Dev-Squad organisation

- **Params**  
  None
- **Query**  
  page=[integer], n=[integer]
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

## **GET /pullrequests/stale**

Returns n stale pull requests in Real-Dev-Squad organisation

- **Params**  
  None
- **Query**  
  page=[integer], n=[integer]
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

