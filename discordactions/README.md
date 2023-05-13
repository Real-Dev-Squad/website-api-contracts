# Discord actions


## **Requests**

|                         Route                          |          Description          |
|:------------------------------------------------------:|:-----------------------------:|
|           [GET /groups](#get-groups)           |    Returns all group roles     |
|          [POST /groups](#post-groups)          |     Creates new group role     |
| [POST /roles](#post-roles) | Adds a group role to a user |

## **GET /groups**

> returns all group roles

## Response

```json
{
    "message":"Roles fetched successfully!",
    "groups":[
        {
            "id":"BcfrtGo9hAhVxcDZ5ADQ",
            "date":{
                "_seconds":1683970114,
                "_nanoseconds":286000000
            },
            "createdBy":"k15z2SLFe1U2J3gshXUG",
            "rolename":"group-peer-programming",
            "roleid":"1106875628000653332"
        },
        {
            "id":"ELqELhWrdoSX1AusBwPw",
            "date":{
                "_seconds":1683917265,
                "_nanoseconds":874000000
            },
            "createdBy":"1uilsoHQZ1TzcYyy25BA",
            "rolename":"group-live-site",
            "roleid":"1106653965799657533"
        },
    ]
}

```
- **Error Response:**
  - **Code:** 500
    - **Content:** 
    ```json
    {
      "statusCode": 400,
      "error": "INTERNAL SERVER ERROR",
      "message": "Internal server error"
    }
    ```
  - **Code:** 404
    - **Content:** 
    ```json
    {
      "statusCode": 404,
      "error": "NOT FOUND",
      "message": "Not found"
    }
    ```

## **POST /group**
> creates a new group role
## Body

```json
{
    "rolename": "some-demo"
}
```

## Response


- **Error Response:**
  - **Code:** 400
    - **Content:**
    ```json
    {
      "statusCode": 400,
      "error": "BAD REQUEST",
      "message": "Role already exists"
    }
    ```

## **POST /roles**

> This request allows users to add group-roles to themselves

## Body

Request

```json
    {
        "userid": "<unique discord id of the rds member>",
        "roleid": "<role id of the group>"
    }
```

Response

```json
{
     "message": "Role added successfully!"
}
```
- **Error Response:**
  - **Code:** 400
    - **Content:** 
    ```json
    {
      "statusCode": 400,
      "error": "BAD REQUEST",
      "message": "Role already exists!"
    }
    ```
