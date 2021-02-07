# Badges

## Badge object

```
{
  'id': string,
  'title': string,
  'description': string,
  'imgUrl': string,
  'badges': [
    rdsUserId,
  ],
}
```

## **Requests**

|           Route            |           Description            |
|:--------------------------:|:--------------------------------:|
| [GET /badges](#get-badges) | Returns all badges in the system |
| [POST /badges](#post-badges) | Creates a new badge |
| [Patch /badges](#patch-badgesid) | Updates data of the badge |

## **GET /badges**

Returns all badges in the system.

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
  message: 'Badges returned successfully!'
  badges: [
          {<badge_object>}
        ]
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'Something went wrong. Please contact admin' }`

## **POST /badges**

Creates a new Badge.

- **Params**  
  None
- **Query**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Body** `{ <badge_object> }`
- **Success Response:**
  - **Code:** 200
    - **Content:** `{ <badge_object> }`
- **Error Response:**
  - **Code:** 409
    - **Content:** `{ "statusCode": 409, "error": "Conflict", "message": "Badge already exists" }`
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated badge' }`

## **PATCH /badges/:id**

Updates data of the badge.

- **Params**  
  None
- **Query**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Body** `{ <badge_object> }`
- **Success Response:**
  - **Code:** 204
    - **Content:** `{ 'message': 'Badge updated successfully!'}`
- **Error Response:**
  - **Code:** 404
    - **Content:** `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'Badge not found' }`
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated Badge' }`
  - **Code:** 503
    - **Content:** `{ 'statusCode': 503, 'error': 'Service Unavailable', 'message': 'Something went wrong please contact admin' }`

