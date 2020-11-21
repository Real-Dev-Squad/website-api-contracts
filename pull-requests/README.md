# Pull Requests

## Pull Request Info Object

```
{
    'title': <title of PR>,
    'url': <url of PR>,
    'state': <open or closed>,
    'created_at': <time of PR created>,
    'updated_at': <last event: commit or review or closing of PR>,
    'draft': <ready for review or changes requested>,
    'labels': <labels array>,
    'assignee': <username of the assignee>
}
```

## **Requests**

|               Route                |           Description           |
| :--------------------------------: | :-----------------------------: |
|      [GET /pulls/:id](#get-pullsid)      | Returns latest 5 PRs by the user in RDS |


## **GET /pulls/:id**

Returns latest 5 pull requests by an user in Real-Dev-Squad organisation

- **Params**  
  `:id`

- **Success Response:**
- **Code:** 200
  - **Content:**

```
{
  message: 'Pull requests returned successfully!'
  pulls: [
           {<Pull Request Info Object>}
         ]
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'Something went wrong. Please contact admin' }`
