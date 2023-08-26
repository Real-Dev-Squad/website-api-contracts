# Auctions

## Firestore collections:
- `auctions`
- `biddingLog`

## Auctions Metadata object

```
{
  'id': <auction-id>,
  'seller': <userId>,
  'item_type': <example: neelam>,
  'quantity': <example: 10>,
  'highest_bidder': <userId>,
  'highest_bid': <example: 200 dinero | default: initial_price>,
  'start_time': <unix-timestamp>,
  'end_time': <unix-timestamp>,
  'bidders': [<userId>, <userId>]
}
```

## Auction Details Object

```
{
  'id': <auction-id>,
  'seller': <userId>,
  'item_type': <example: neelam>,
  'quantity': <example: 10>,
  'initial_price': <example: 100 dinero>,
  'highest_bidder': <userId>,
  'highest_bid': <example: 200 dinero | default: initial_price>,
  'start_time': <unix-timestamp>,
  'end_time': <unix-timestamp>,
  'bidders_and_bids': [
    {
      'bidder': <userId>,
      'bid': <number>,
      'timestamp': <unix-timestamp>
    }
  ],
}
```


## **Requests**

|                 Route                    |             Description              |
|:----------------------------------------:|:------------------------------------ |
|       [GET /auctions](#get-auctions)     | Returns all available auctions metadata  |
|       [GET /auctions/:id](#get-auctionsid)     | Returns detailed auction data for one auction  |
|      [POST /auctions](#post-auctions)    | Creates a new auction                |
| [POST /bid/:id](#post-bidid) | Place new bid               |


## **GET /auctions**

Returns active (ongoing) auctions

- **Params**  
  None
- **Query**  
  None
- **Body**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  None
- **Success Response:**
- **Code:** 200
  - **Content:**

```
{
  message: 'Auctions returned successfully!'
  auctions: [
              {Auctions Metadata object},
              {Auctions Metadata object},
              {Auctions Metadata object}              
            ]
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`


## **GET /auctions/:id**

Get detailed information for an auction

- **Params**  
    _Required:_ `id=[string]`
- **Query**  
  None
- **Body**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  None
- **Success Response:**
- **Code:** 200
  - **Content:** `{Auction Details Object}`

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **POST /auctions**

Creates a new auction

- **Params**  
  None
- **Query**  
  None
- **Body** 
  ```
  {
    'item_type': <example: neelam>,
    'quantity': <example: 10>,
    'initial_price': <example: 100>,
    'end_time': <unix_timestamp>
  }
  ```
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 201
    - **Content:** `{
  message: 'Auction created successfully!'}`
- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **POST /bid/:id**

Place a new bid when auctionId is given

- **Params**  
  _Required:_ `id=[string]`
- **Query**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Body** 
  `{
    bid: <number>
  }`
- **Success Response:**
  - **Code:** 201
    - **Content:** `{ 'message': 'Successfully placed bid!'}`
- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'User cannot be authenticated' }`
  - **Code:** 403
    - **Content:** 
    ```
    {
      'statusCode': 403,
      'error': 'Forbidden',
      'message': 
        'Your bid was not higher than current one!' /
        'You do not have sufficient money' /
        'You do not have a wallet!'
    }
    ```
  - **Code:** 404
    - **Content:** `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'Auction doesn't exist' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`
