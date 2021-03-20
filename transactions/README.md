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
|      [GET /fetchLatest/:userId/:noOfRecords](#fetch-latestTransactions)      | Returns noOfRecords latest Transactions in the system  for userId provided|
----
## **GET /latestTransactions**

Returns N number of transactions for userId provided.

- **Params**  
  userId: userUId of loggedin user.
  noOfRecords: No of transactions want to fetch.
- **Query**  
  uiser=[string] noOfRecords=[integer]
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
