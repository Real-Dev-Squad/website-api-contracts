# Users

## Contributions Object

```
{
  prList: [{
    'prTitle': <title of pull request>,
    'prUrl': <url of pull request>,
    'state': <open or closed>,
    'prCreatedAt': <unix_timestamp>,
    'prUpdatedAt': <unix_timestamp>,
    'prRaisedBy': <username of the assignee>
  }],
  taskDetails: {
    'title': <task tile>,
    'purpose': <why this task is needed>,
    'endsOn': <unix_timestamp>,
    'startedOn': <unix_timestamp>,
    'deployedOn': <unix_timestamp>,
    'participants': [
      {
        'first_name': <first name of user>,
        'last_name': <last name of user>,
        'img': <image url of user>
      },
      {
        'first_name': <first name of user>,
        'last_name': <last name of user>,
        'img': <image url of user>
      }
    ],
    'featureUrl': <url of the feature task>,
    'isNoteworthy': true
  }
}
```

## **Requests**

|                           Route                            |        Description         |
| :--------------------------------------------------------: | :------------------------: |
| [GET /contributions/:username](#get-contributionsusername) | Returns user contributions |

## **GET /contributions/:username**

Returns the specified user.

- **Params**  
  _Required:_ `username=[string]`
- **Body**  
  None
- **Headers**  
  Content-Type: application/json
- **Success Response:**
  - **Code:** 200
    - **Content:**

```
{
  message: 'Contributions returned successfully!'
  contributions: {
    noteWorthy: [
      <contribution_object>,
      <contribution_object>
    ],
    all: [
      <contribution_object>,
      <contribution_object>
    ]
  }
}
```

- **Error Response:**
  - **Code:** 404
    - **Content:** `{ error: 'Not Found', message: 'User doesn't exist' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`
