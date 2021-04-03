# Transactions

## Transaction object

```
{   
    typeOfCurrency: string,
    typeOfTransaction: string,
    userID: string,
    amount: number,
    dateTime: unix-timestamp
}
```

## **Requests**

|               Route                |           Description           |
| :--------------------------------: | :-----------------------------: |
|      [GET /transactions/fetch/:username](#fetch)      | Returns N Transactions in order specified for username provided|
----
## **GET /transactions/fetch/:username**

Returns N number of transactions for username provided. 
First fetch the userid from users collection using username, fetch all the transactions for userId and then sorts in ASC or DESC and return N transactions.
For fetching 5 latest transaction for username kratika, URL will be
/transactions/fetch/kratika?n=5&o=DESC

- **Params**
  username: username of loggedin user.
- **Query**
  n: No of transactions to fetch. Default value is 10 if not provided in request URL
  o: Order of records according to date of transaction, default is DESC
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
   "message": "Transactions returned successfully!",
   "data": [
           {transactions-object},
           {transactions-object},
           {transactions-object},
           {transactions-object},
         ]
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'Something went wrong. Please contact admin' }`
  - **Code:** 404
    - **Content:** `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'User does not exist!' }`
  - **Code:** 404
    - **Content:** `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'No transactions exist!' }`
