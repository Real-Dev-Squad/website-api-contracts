# ARTS

| Method | Route                                           | Description                                                    |
| ------ | ----------------------------------------------- | -------------------------------------------------------------- |
| GET    | [/arts/:userId](#get---arts)                    | It will fetch you the user arts based on id.                   |

## Art Object
```
    {
      "id": string,
      "css": string,
      "price": number,
      "title": string,
      "userId": string
    }
```

## **GET /arts/:userId**

Returns all the art of the user.

- **Params**
  _Required:_ `userId=[string]`
- **Query**
  None
- **Body**
  None
- **Headers**
  Content-Type: application/json
- **Cookie**
  rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 200
    - **Content:**

    ```

    {
        "message": "User Arts of userId <userId> returned successfully",
        "arts": [
            {
            "id": string,
            "css": string,
            "price": number,
            "title": string,
            "userId": string
            }
        {
    }
    ```

- **Error Response:**
  - **Code:** 401
    - **Content:**
      `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 404
    - **Content:**
      `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'User doesn't exist' }`
  - **Code:** 500
    - **Content:**
      `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`
      