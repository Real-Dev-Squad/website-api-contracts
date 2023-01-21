# Learn

## User object

```
user {
    liked: PostId[]
    bookmarks: PostId[]
}
```

## Label object

```
label: {
    labelName: string,
    labelPostCount: number
}
```

## Post object

```
{
    postId: string
    userId: string
    title: string
    body/description: string
    link: string
    publishedDate: unix timestamp
    likeCount: number
    bookmarkCount: number
    type: video|article
    filters: array of string
    likedBy: UserId[]
    bookmarkedBy: UserId[]
}

```

## Feed object

```
{
  "feeds": [
    // optional, can be empty
    <Post>,
    <Post>
  ]
  ,
  "isLast": boolean<true|false>,
}
```

Anonymous (Not LoggedIn) - View
LoggedIn, but not RDS Member - View, Like, and Bookmark
LoggedIn, and RDS Member - All of the above and Add Post

## **Requests**

|                         Route                         |         Description          |
| :---------------------------------------------------: | :--------------------------: |
|              [POST /posts](#post-posts)               |          Save post           |
|              [GET /labels](#get-labels)               |          get labels          |
|             [POST /labels](#post-labels)              |         post labels          |
|               [GET /posts](#get-posts)                |    get posts - paginated     |
|           [GET /posts/{postId}](#get-post)            |           get post           |
|             [GET /posts/self](#get-posts)             |           get post           |
|           [PATCH /post/{postId}](#patch-post)           |          patch post          |
|         [GET /user/bookmarks](#get-bookmarks)         | get posts bookmarked by user |
|      [GET /post/{postId}/likes](#get-post-like)       | get users who liked the post |
| [PATCH /user/{postId}/{interactionType}](#patch-post) |    patch user interaction    |


## **GET /feeds**

Returns all the feeds

- **Params**  
  None
- **Query**  
  size=[integer], page=[integer]
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
  message: 'Feed returned successfully!'
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
  size=[integer], page=[integer]
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
- **Cookie**
  rds-session: `<JWT>`
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
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 409
    - **Content:** `{ "statusCode": 409, "error": "Conflict", "message": "User already exists" }`
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
- **Query**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Body** `{ <feed_object> }`
- **Success Response:**
- **Code:** 204
  - **Content:** `{ 'message': 'User updated successfully!'}`
- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 403
    - **Content:** `{ 'statusCode': 403, 'error': 'Forbidden', 'message': 'Cannot update username again'}`
  - **Code** 404
    - **Content:** `{ 'statusCode': 404, 'error': 'Not found', 'message': 'No bookmark found' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`
  - **Code:** 503
    - **Content:** `{ 'statusCode': 503, 'error': 'Service Unavailable', 'message': 'Something went wrong please contact admin' }`


---