# Stock Exchange

## Firestore Collections

* Stocks
> this will contain details of all stocks that should be listed

## Request Object

```
{
"trade_type": "buy/sell",
"stock_name": <name_of_the_stock>,
"quantity": <number_of_quantities>,
"listed_price": <listed_stock_price>,
"total_price": <total_purchase_price>
}
```

## Response Object

```
{
  "user_balance": <balance left with user after successful trade>
}
```

## **Requests**

|               Route                |    Description    |
| :--------------------------------: | :---------------: |
|     [PATCH /trade/:username](#patch-tradeusername)     | New trading request  |


## **PATCH /trade/:username**

- **Params**  
  _Required:_ `username=[string]`

- **Headers**  
  Content-Type: application/json
- **Body** `{ <request_object> }`
- **Success Response:**
- **Code:** 200
  - **Content:** `{<response_object>}`
- **Error Response:**
  - **Code:** 403
    - **Content:** `{ 'statusCode': 403, 'error': 'Forbidden', 'message': 'Trading failed due to insufficient funds'}`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`
