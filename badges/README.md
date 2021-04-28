# Badges

## Badge object

```
{
  'id': string,
  'title': string,
  'description': string,
  'imgUrl': string,
  'users': [
    rdsUserId,
  ],
}
```

## **Requests**

|             Route              |           Description            |
| :----------------------------: | :------------------------------: |
|   [GET /badges](#get-badges)   | Returns all badges in the system |
|   [PUT /badges](#put-badges)   |       Creates a new badge        |
| [PATCH /badges](#patch-badges) |    Updates data of the Badge     |

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

## **PUT /Badges**

Creates a new badge.

- **Params**  
  _Required:_ `username=[string]`
- **Query**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  None
- **Success Response:**
  - **Code:** 200
    - **Content:** `{ <badge_object> }`
- **Error Response:**
  - **Code:** 204
    - **Content:** `{ "statusCode": 204), "error": "Conflict", "message": "Empty badge data object provided" }`
  - **Code:** 401
    - **Content:** `{ "statusCode": 204), "error": "Conflict", "message": "User is not a member" }`

## **PATCH /Badges**

Updates data of the Badge.

- **Params**  
  _Required:_ `id=[string]`
  _Required:_ `username=[string]`
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
  - **Code:** 204
    - **Content:** `{ "statusCode": 204), "error": "Conflict", "message": "Empty badge data object provided" }`
  - **Code:** 401
    - **Content:** `{ "statusCode": 204), "error": "Conflict", "message": "User is not a member" }`
