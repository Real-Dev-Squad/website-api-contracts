# Learn

## Feed object

```
{
  "title": "Title of <video|article>",
  "type": "video|article",
  "link": "<url>",
  "author": "Name of the author",
  "publishedDate": timestamp,
  "likes": number,
  "bookmarkedBy": "<userId>",
  "bookmarkedDate": timestamp,
}

```

## Feeds object

```
{
  "feeds": [
    // optional, can be empty
    <feed1>,
    <feed2>
  ]
  ,
  "isLast": boolean<yes|no>,
}
```

## **Requests**

|               Route                |    Description    |
| :--------------------------------: | :---------------: |
| [GET /feeds](#get-feeds)           | Returns all feeds |
| [GET /feeds/self](#get-feedsself)      | Returns all feeds of logged in user |
| [POST /bookmark](#post-bookmark)   | Creates new feed aka bookmark  |
| [GET /bookmark/:id](#get-bookmarkid) |  Returns feed with specific id |
| [PATCH /bookmark/:id](#patch-bookmarkid) |   Updates feed with specific id   |


## **GET /feeds**

Returns all the feeds

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
  message: 'Feeds returned successfully!'
  feeds: [
           {<feed_object>},
           {<feed_object>}
         ]
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **GET /feeds/self**

Returns all the feeds of a logged in user

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
  message: 'Feeds returned successfully!'
  feeds: [
           {<feed_object>},
           {<feed_object>}
         ],
  isLast: <true|false>
}
```

- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 404
    - **Content:** `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'User doesn't exist' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **POST /bookmark**

- **Params**  
  None
- **Query**  
  None
- **Headers**  
  Content-Type: application/json
- **Body** `{ <feed_object> }`
- **Success Response:**
- **Code:** 200
  - **Content:**

```
{
  message: 'Bookmark created successfully!'
  task: {<feed_object>}
  id: <newly created feed id>
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **GET /bookmark/:id**

Returns feed of the requested id.

- **Params**  
  _Required:_ `id=<ID>`
- **Query**  
  None
- **Body**  
  None
- **Headers**  
  Content-Type: application/json
- **Success Response:**
  - **Code:** 200
    - **Content:**
```
{
  message: 'Feed returned successfully!'
  {<feed_object>}
}
```

- **Error Response:**
  - **Code:** 404
    - **Content:** `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'Bookmark doesn't exist' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`


## **PATCH /bookmark/:id**

- **Params**  
  _Required:_ `id=<ID>`

- **Headers**  
  Content-Type: application/json
- **Body** `{ <feed_object> }`
- **Success Response:**
- **Code:** 204

  - **Content:** `<No Content>`

- **Error Response:**
  - **Code** 404
    - **Content** `{ 'statusCode': 404, 'error': 'Not found', 'message': 'No bookmark found' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`