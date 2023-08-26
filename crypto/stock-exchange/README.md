# Stock Exchange

## Firestore Collections

* Stocks
> this will contain details of all stocks that should be listed

## Stock Object

```
{
  "name": <stock_name>,
  "price": <listed_price>,
  "quantity": <available_quantity>
}
```

## Trading Request Object

```
{
  "tradeType": "buy/sell",
  "stockName": <name_of_the_stock>,
  "quantity": <number_of_quantities>,
  "listedPrice": <listed_stock_price>,
  "totalPrice": <total_purchase_price>
}
```

## Trading Response Object

```
{
  "userBalance": <balance left with user after successful trade>
}
```

## **Requests**

|               Route                |    Description    |
| :--------------------------------: | :---------------: |
|      [GET /stocks](#get-stocks)      | Returns all stocks to be listed |
|     [POST /stocks](#post-stocks)     | Creates new stock  |
|     [PATCH /trade/:username](#patch-tradeusername)     | New trading request  |

## **GET /stocks**

Returns all the stocks to be listed

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
  message: 'Stocks returned successfully!'
  stocks: [
           {<stock_object>},
           {<stock_object>}
         ]
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **POST /stocks**

- **Params**  
  None
- **Query**  
  None
- **Headers**  
  Content-Type: application/json
- **Body** `{ <stock_object> }`
- **Success Response:**
- **Code:** 200
  - **Content:**

```
{
  message: 'Stock created successfully!'
  stock: {<stock_object>}
  id: <newly created stock id>
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **PATCH /trade/:username**

- **Params**  
  _Required:_ `username=[string]`

- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Body** `{ <trading_request_object> }`
- **Success Response:**
- **Code:** 200
  - **Content:** `{<trading_response_object>}`
- **Error Response:**
  - **Code:** 403
    - **Content:** `{ 'statusCode': 403, 'error': 'Forbidden', 'message': 'Trading failed due to insufficient funds'}`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`
