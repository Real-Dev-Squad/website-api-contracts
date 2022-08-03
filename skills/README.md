# Skills

## Skills Object

```
{
    "name": <name of skill>,
    "awarded_on": <when skill was added>,
    "awarded_by": <who added it>,
    "awarded_for": <reason for adding it> 
}
```

## **Requests**

|                     Route                      |         Description          |
| :--------------------------------------------: | :--------------------------: |
|           [GET /skills](#get-skills)           | Returns skills in collection |
| [POST /skills/:username](#post-skillsusername) |  Adds skills to collection   |

## **GET /skills**

Returns all skills
- **Params**  
  None
- **Query**  
  None
- **Body**  
  None
- **Headers**  
  None
- **Success Response:**
  - **Code:** 200
    - **Content:**
```
{
  message: 'Skills returned successfully!'
  tasks: [
           {<skill_object>},
           {<skill_object>}
         ]
}
```

## **POST /skills/:username**

Adds skill to user object
- **Params**  
  _Required:_ `username=[string]`
- **Query**  
  None
- **Body**  
  { <skill_object> }
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 200
    - **Content:**
```
{
    message: "Added data successfully!"
    content: { <skill_object> }
}
```
- **Error Response:**
  - **Code:** 401
    - **Content:** ` { "statusCode": 401, "error": "Unauthorized", "message": "Unauthenticated User" } `
  - **Code:** 404
    - **Content:** ` { "statusCode": 404, "error": "Not Found", "message": "User doesn't exist" } `
  - **Code:** 500
    - **Content:** ` { "statusCode": 500, "error": "Internal Server Error", "message": "An internal server error occurred" } `