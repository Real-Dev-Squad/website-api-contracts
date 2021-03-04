# Auctions

## Auction object

```
{
  'id': <auction-id>,
  'seller': <userId>,
  'item_type': <example: neelam>,
  'initial_price': <example: 100 dinero>,
  'bidders_and_bids': [{
    bidder: <userId>,
    last_bid: <number>,
    timestamp: <unix-timestamp>
  }],
  'highest_bidder': <userId>,
  'highest_bid': <example: 200 dinero>,
  'start_time': <unix-timestamp>,
  'duration': <unix-timestamp>,
  'status': <active or expired>
}
```

## **Requests**

|                 Route                    |             Description              |
|:----------------------------------------:|:------------------------------------ |
|       [GET /auctions](#get-auctions)     | Returns all available auctions data  |
|      [POST /auctions](#post-auctions)    | Creates a new auction                |
| [PATCH /auctions/:id](#patch-auctionsid) | Update the bid by user               |


## **GET /auctions**

Returns only **active** auctions data by default, unless the query `include=expired` is set.

- **Params**  
  None
- **Query**  
  `include=expired`
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
  message: 'Auctions data returned successfully!'
  auctions: [
              {<auction_object>},
              {<auction_object>},
              {<auction_object>}
            ]
}
```

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
  `{ <auction_object> }`
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 201
    - **Content:** `{
  message: 'New auction created successfully!'}`
- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **PATCH /auctions/:id**

Update user's previous bid.

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
    my_latest_bid: number
  }`
- **Success Response:**
  - **Code:** 204
    - **Content:** `{ 'message': 'Auction data updated successfully!'}`
- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 403
    - **Content:** `{ 'statusCode': 403, 'error': 'Forbidden', 'message': 'Cannot update expired auction data'}`
  - **Code:** 404
    - **Content:** `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'Auction not found' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`
