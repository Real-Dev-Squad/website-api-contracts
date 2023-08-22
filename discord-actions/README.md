# Groups

## Group Object 

```
 {
      "id": "string",
      "date": {
        "_seconds": number,
        "_nanoseconds": number
      },
      "rolename": "string",
      "roleid": "string",
      "createdBy": "string",
      "count": number,
      "firstName": "string", // Group creator firstName
      "lastName": "string",  // Group creator lastName
      "image": "string"      // Group creator image 
      "isMember": boolean,   // Is logged in user is member of the the group
}

```


## Requests

|               Route                |    Description    |
|------------------------------------|-------------------|
|      [GET /groups](#get-tasks)     | Returns all the groups |


## GET /groups

Return all the groups present in RDS

- **Query**
    - Optional: `dev=boolean` (If set to `true` it will send some extra data like firstName, lastName, isMember, image)  
     

- **Success Response without dev query**
    - Status Code: 200
    - Content
        ```
        { "message": "Roles fetched successfully!",
            "groups": [
                {
                "id": "string",
                "date": {
                    "_seconds": number,
                    "_nanoseconds": number
                },
                "rolename": "string",
                "roleid": "string",
                "createdBy": "string"
                }
            ]
        } 
        ```

- **Success Response with dev query**
    - Status Code: 200
    - Content
        ```
        {
            "message": "Roles fetched successfully!",
            "groups": [
                {
                "id": "string",
                "date": {
                    "_seconds": number,
                    "_nanoseconds": number
                },
                "rolename": "string",
                "roleid": "string",
                "createdBy": "string",
                "count": number,
                "firstName": "string",
                "lastName": "string",
                "isMember": boolean,
                "image": "string"
                }
            ]
        }
        ```
- **Error Response**
    - Internal Server Error
        - Status Code: 500
        - Content: 
            ```
            { 
                'statusCode': 500, 
                'error': 'Internal Server Error', 
                'message': 'An internal server error occurred' 
            }
            ```
    - If not logged in
        - Status Code: 401
        - Content:
            ```
            {
                "statusCode": 401,
                "error": "Unauthorized",
                "message": "Unauthenticated User"
            }
            ```
    - If not verified or an archived user
        - Status Code: 403
        - Content: 
            ```
            {
                "statusCode": 403,
                "error": "Forbidden",
                "message": "You are restricted from performing this action"
            }
            ```

