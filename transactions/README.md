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
|      [GET /fetchLatest/:username?noOfRecords](#fetch-latestTransactions)      | Returns noOfRecords latest Transactions in the system  for username provided|
----
## **GET /latestTransactions**

Returns N number of transactions for username provided. First fetch the userid from users collection and get userId of loggedin user.
Fetch transactions using userId.

- **Params**  
  username: username of loggedin user.
  noOfRecords: No of transactions want to fetch. Default value is 10 if not provided in request URL
- **Query**  
  username=[string] noOfRecords=[integer]
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
   "responseCode": 200,
   "data": [
           {transactions-object}
         ]
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'Something went wrong. Please contact admin' }`
