# Members

## Member object

Same as the [user](https://github.com/Real-Dev-Squad/website-api-contracts/tree/main/users#user-object) object
## Recruiter Object

```json
{   
    "company": "string",
    "first_name": "string",
    "last_name": "string",
    "designation": "string",
    "reason": "string",
    "email": "string",
    "currency": "string",
    "package": "number"
}
```

## **Requests**

|               Route                |           Description           |
| :--------------------------------: | :-----------------------------: |
|      [GET /members](#get-members)      | Returns all members in the system |
|      [GET /members/idle](#get-inactive/idle-members)      | Returns all inactive/idle members in the system |
|[POST /members/intro/:username](#post-members/intro/:username)|Post request for members profile introduction|
----
## **GET /members**

Returns all members in the system.

- **Params**  
  None
- **Query**  
  type=[ all | new | blocked ]
- **Body**  
  None
- **Headers**  
  None
- **Cookie**  
  None
- **Success Response:**
- **Code:** 200
  - **Content:**
    ```json
    {
        "message": "Members returned successfully!",
        "members": [
            "<member_object>"
        ]
    }
    ```

- **Error Response:**
  - **Code:** 500
    - **Content:** 
      ```json
      {
          "statusCode": 500,
          "error": "Internal Server Error",
          "message": "Something went wrong. Please contact admin"
      }
      ```

## **GET /members/idle**

Returns all inactive/idle members in the system.

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
    ```json
    {
        "message": "Idle members returned successfully!",
        "members": [
            "<username>"
        ]
    }
    ```

- **Error Response:**
  - **Code:** 500
    - **Content:** 
      ```json
        {
            "statusCode": 500,
            "error": "Internal Server Error",
            "message": "Something went wrong. Please contact admin"
        }
      ```

## POST /members/intro/:username
Creates new request for member introduction 

- **Params**\
    _Required_: `username=[string]`
- **Query**\
    None
- **Body**\
    `{<recruiter_object>}`
- **Headers**\
    None
- **Cookie**\
    None
- **Success Response:**
    - **Code:** 200
        - **Content:** 
          ```json
          {
              "message": "Request Submission Successful!!",
              "id": "<new unique submission id>"
          }
          ```

- **Error Response:**
    - **Code:** 500
        - **Content:** 
          ```json
          {
              "statusCode": 500,
              "error": "Internal Server Error",
              "message": "An internal server error occurred"
          }
          ```

## **GET /members/intro/**

Returns all requests for member introduction by recruiter in the system.

- **Params**  
  None
- **Query**  
  type=[ all | new | blocked ]
- **Body**  
  None
- **Headers**  
  None
- **Cookie**  
  None
- **Success Response:**
- **Code:** 200
  - **Content:**
    ```json
    {
        "message": "Recruiters returned successfully!",
        "recruiters": [
            "<recruiter_object>",
            "<recruiter_object>"
        ]
    }
    ```

- **Error Response:**
  - **Code:** 500
    - **Content:** 
      ```json
      {
          "statusCode": 500,
          "error": "Internal Server Error",
          "message": "Something went wrong. Please contact admin"
      }
      ```