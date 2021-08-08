# Currency Exchange

## Firestore Collections

- Users
  > this will contain details about the accepted currencies at crypto-website
- Transactions
  > this will contain details about the accepted currencies at crypto-website
- Banks
  > this will contain details about the accepted currencies at crypto-website
- Wallets
  > this will contain details about the accepted currencies at crypto-website

## Currency Exchange Object

```
  <currency_name>: {
    <currency_name>: <exchange_value>
    <currency_name>: <exchange_value>
  }
```

## Bank Object
```
{
    "bankId": <bank_id>,
    "bankName": <bank_name>
},
```

## **Requests**

|               Route                    |                      Description                        |
| :------------------------------------: | :-----------------------------------------------------: |
| [GET /rates](#get-rates)               | Get exchange rates                                      |
| [POST /rates](#post-rates)             | Post exhcange rate                                      |
| [GET /banks](#get-banks)               | Get all bank names                                      |
| [GET /bank/:bankId](#get-bank-details) | Get bank details for bankId                             |
| [PATCH /](#patch-currency-exchange)    | Update the user's wallet and bank currency availability |

## **GET /rates**

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
  currency: {
    <currency_exchange_object>,
    <currency_exchange_object>,
    ...
  }
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **POST /rate**

Adds the currency exchange value. Only Ankush is authorised to make use this request.

- **Params**  
  None
- **Query**  
  None
- **Headers**
- **Body**
```
{
    src: <currency_name> (string),
    target: <currency_name> (string),
    quantity: <currency_quantity> (quantity),
}
```
- **Success Response:**
- **Code:** 200
- **Content:**

```
{
  message: 'success'
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **GET /banks**

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
  
  message: 'Bank returned successfully!',
  banks: [<Bank_Object>, <Bank_Object>, ...]
    
}
```

## **GET /:bankId**

Returns available currency for a particular `bankId`

- **Params**  
  _Required:_ `bankId=[string]`
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
  "message": "currency returned successfully!",
  "currency": {
      <currency_name>: <available_quantity>,
      <currency_name>: <available_quantity>,
      ...
  }
}
```



## **PATCH /**

- **Params**
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Body**
```
{
    src: <currency_name> (string),
    target: <currency_name> (string),
    quantity: <currency_quantity> (quantity),
    bankId: <bank_id> (string),
}
```
- **Success Response:**
- **Code:** 200
  - **Content:**
```
{
  "status": "success",
  "message": "Transaction Successful"
}
```
- **Error Response:**
  - **Code:** 403
    - **Content:** `{ 'statusCode': 403, 'error': 'Forbidden', 'message': 'Trading failed due to insufficient funds'}`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`
