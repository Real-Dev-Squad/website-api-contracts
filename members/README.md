# Members

## Member object

```
{
  'id': string,
  'first_name': string,
  'last_name': string,
  'yoe': number,
  'company': string,,
  'designation': string,
  'img': string,
  'github_id': string,
  'linkedin_id': string,
  'twitter_id': string,
  'instagram_id': string,
  'site': string,
  'github_display_name': string,
  'isMember': boolean,
  'tokens': {}
}
```
## Recruiter Object

```
{
    'company': string,
    'first_name': string,
    'last_name': string,
    'designation': string,
    'reason': string,
    'email': string,
    'currency': string,
    'package': number
}
```

## **Requests**

|               Route                |           Description           |
| :--------------------------------: | :-----------------------------: |
|      [GET /members](#get-members)      | Returns all members in the system |
|      [GET /members/idle](#get-inactive/idle-members)      | Returns all inactive/idle members in the system |
|[POST /members/intro](#post-members/intro)|Post request for members profile introduction|
----
## **GET /members**

Returns all members in the system.

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
  message: 'Members returned successfully!'
  members: [
           {<member_object>}
         ]
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'Something went wrong. Please contact admin' }`

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

```
{
  message: 'Idle members returned successfully!'
  members: [
           <username>
         ]
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'Something went wrong. Please contact admin' }`

## POST /members/intro
Creates new request for member introduction 

- **Params**\
    None
- **Query**\
    None
- **Body**\
    `{recruiter_object}`
- **Headers**\
    None
- **Cookie**\
    None
- **Success Response:**
    - **Code:** 200
        - **Content:** 
        ```
        {
            message: 'Request Submission Successful!!'
            id: <new unique submission id>
        }
        ```

- **Error Response:**
    - **Code:** 500
        - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`
