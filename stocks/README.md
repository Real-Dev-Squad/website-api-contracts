# STOCKS

| Method | Route                                           | Description                                                    |
| ------ | ----------------------------------------------- | -------------------------------------------------------------- |
| GET    | [/stocks/:userId](#get---stocks)                | It will fetch you all the user stocks based on your own id.    |

## Stocks Object
```
    {
        "message": "User stocks returned successfully!",
        "userStocks": [
            {
            "id": string,
            "userId": string,
            "stockId": string,
            "stockName": string,
            "quantity": number,
            "orderValue": number,
            "initialStockValue": number
            }
        ]
    }
```

## **GET /stocks/:userId**

Returns all the stocks of the user.

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
        "message": "User stocks returned successfully!",
        "userStocks": [
            {
            "id": string,
            "userId": string,
            "stockId": string,
            "stockName": string,
            "quantity": number,
            "orderValue": number,
            "initialStockValue": number
            }
        ]
    }
    ```

When No Stock is found.

  - **Code:** 200
    - **Content:**

    ```
    { 
      message: "No stocks found", 
      userStocks: [] 
    }
    ```

- **Error Response:**
  - **Code:** 403
    - **Content:**
      ```
      { 
        'statusCode': 403,
        'error': 'Unauthorized',
        'message': 'Unauthorized access' 
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
