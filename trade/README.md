# TRADE

| Method | Route                                           | Description                                                    |
| ------ | ----------------------------------------------- | -------------------------------------------------------------- |
| POST    | [/trade/stock/new/:userId](#post---trade)                | It will post and execute your required trade based on your userID and the data body you pass.    |

## Stocks Object
```
    {
        "message": "Congrats, Stock Trade done successfully!! 🎉🎉🎉🎊🎊🎊",
        "userBalance": number
    }
```

## **POST /trade/stock/new/:userId**

Returns congrats message and user-balance if trade is successfull.

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
        "message": "Congrats, Stock Trade done successfully!! 🎉🎉🎉🎊🎊🎊",
        "userBalance": number
    }
    ```


- **Error Response:**
  - **Code:** 403
    - **Content:**
      ```
      { 
        "statusCode": 403,
        "error": "Forbidden",
        "message": "Trade was not successful due to insufficient funds" 
      }
      ```
    - **Content:**
      ```
      {
        "statusCode": 403,
        "error": "Forbidden",
        "message": "Trade was not successful because you do not have enough quantity"
      }
      ```
  - **Code:** 500
    - **Content:**
      ```
      { 
        'statusCode': 500, 
        'error': 'Internal Server Error', 
        'message': 'An internal server error occurred' 
      }
      ```
