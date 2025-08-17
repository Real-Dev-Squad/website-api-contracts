# Wallets

## Wallet object

```
{
  userId: <userId>,
  isActive: bool,
  currencies: {
    neelam: number,
    dinero: number
  }
}
```

## **Requests**

|                     Route                     |                  Description                   |
| :-------------------------------------------: | :--------------------------------------------: |
|          [GET /wallet](#get-wallet)           |        Returns  the user wallet details        |
| [GET /wallet/:username](#get-walletusername) | Returns the user wallet details for a username |


## **GET /wallet**

Returns  the user wallet details

- **Params**  
  None
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
  message: 'Wallets returned successfully!'
  wallet: {
    id:<walletId>,
    userId: <userId>,
    isActive: bool,
    currencies: {
      neelam: number,
      dinero: number
    }
  }
}
```

- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **GET wallet/username**

Returns the user wallet details for a username.

- **Params**  
  None
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
    - **Content:** `{ <wallet_object> }`
- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 403
    - **Content:** `{ 'statusCode': 403, 'error': 'Forbidden', 'message': 'You are not authorized for this action'}`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`
