# Skills

## Skill object

```
{
  'name': string,
  'awarded on': string,
  'awarded by': string,
  'why it was awarded': string,
  'users': [
    rdsUserId,
  ],
}
```

## **Requests**

|              Route               |           Description            |
|:--------------------------------:|:--------------------------------:|
|    [GET /skills](#get-skills)    | Returns all skills in the system |

## **GET /skills**

Returns all skills in the system.

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
  message: 'Skills returned successfully!'
  skills: [
    {<skill_object>}
  ]
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'Something went wrong. Please contact admin' }`
