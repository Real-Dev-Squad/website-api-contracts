# Members

## Member object

```
{
  'id': string,
  'incompleteUserDetails': boolean,
  'username': string,
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

## **Requests**

|               Route                |           Description           |
| :--------------------------------: | :-----------------------------: |
|      [GET /members](#get-members)      | Returns all members in the system |
|      [GET /members/idle](#get-inactive/idle-members)      | Returns all inactive/idle members in the system |


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
