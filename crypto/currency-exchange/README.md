# Currency Exchange

## Firestore Collections

- Currency
  > this will contain details about the accepted currencies at crypto-website

## Currency Object

```
{
    "id": <currency_id>,
    "name": <currency_name>,
    "quantity": <available_quantity>
}
```

## Currency Exchange Object

```
{
    from: {
        "id": <currency_id>,
        "name": <currency_name>
    },
    to: {
        "id": <currency_id>,
        "name": <currency_name>
    },
    value: <exchange_value>
}
```

## Exchange Request Object

```
{
  from: <src_currency_id>,
  to: <trgt_currency_id>,
  quantity: <selected_quantity>
}
```

## **Requests**

|                         Route                         |                                Description                                 |
| :---------------------------------------------------: | :------------------------------------------------------------------------: |
|      [GET /currency-list](#get-currency-listing)      |     Returns all accepted currency with their availability in the bank      |
| [GET /currency-exchange-rate](#get-currency-exchange) | Returns currency with their current exchange value with the other currency |
|           [PATCH /convert/](#patch-convert)           |           Update the user wallet and bank currency availability            |

## **GET /currenct-list**

Returns all accepted currency with their availability in the bank

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
  status: 'success'
  currency: [
           <currency_object>,
           <currency_object>
         ]
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **GET /currency-exchange**

- **Params**  
  None
- **Query**  
  None
- **Headers**
- **Body**
- **Success Response:**
- **Code:** 200
  - **Content:**

```
{
  message: 'success'
  currency: [
        {
          <currency_exchange_object>,
          <currency_exchange_object>,
          <currency_exchange_object>
        }
    ]
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **PATCH /convert/:uid**

- **Params**  
  _Required:_ `uid=[string]`

- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Body** `{ <trading_request_object> }`
- **Success Response:**
- **Code:** 200
  - **Content:**

```
        {
        status: 'success'
        }
```

- **Error Response:**
  - **Code:** 403
    - **Content:** `{ 'statusCode': 403, 'error': 'Forbidden', 'message': 'Trading failed due to insufficient funds'}`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`
