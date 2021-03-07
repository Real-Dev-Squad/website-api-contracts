# Members

## Member object

Same as the [user](https://github.com/Real-Dev-Squad/website-api-contracts/tree/main/users#user-object) object

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
