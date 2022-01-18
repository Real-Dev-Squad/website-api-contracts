# Feature Flags

# Feature Flag object

```
{
    'name': string,
    'id': string
    'title':string,
    'created_at': timestamp,
    'updated_at': timestamp,
    'config': {'enabled': boolean},
    'owner': string
    'launched_at': timestamp
}
```

## **Requests**

|                 Route                 |             Description                  |
|:-------------------------------------:|:----------------------------------------:|
| [GET /featureflags](#get-featureflags)  |   Returns all feature flags in system  |
| [POST /featureflags](#post-featureflags)   |   Creates a feature flag            |
| [PATCH /featureflags/:id](#patch-featureflagsId)   | Updates data of featureflag |
| [DELETE /featureflags/:id](#delete-featureflagsId) |  Deletes the feature flag   |

## **GET /featureflags**

 Returns all feature flags in the system 

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
  message: 'Feature Flags returned successfully!'
  featureFlags: [
    {
      'name': string,
      'title':string,
      'created_at': timestamp,
      'updated_at': timestamp,
      'config': {'enabled': boolean},
      'owner': 
      {
        'username': string,
        'img': string
      },
      'launched_at': timestamp
    }
  ]
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **POST /featureflags** 

Creates a feature flag 

- **Params**  
  None
- **Query**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Body** `
{ 
   _Required:_ 'feature_name'= <unique feature name>,
   _Required:_ 'feature_title'= <feature title>,
   _Required:_ 'config'= <Object with one boolean value which specify feature is enabled or disabled>,             
 }`
- **Success Response:**
- **Code:** 200
  - **Content:**

```
{
  message: 'Feature Flag added successfully!'
  data: {
      'name': string,
      'id': string
      'title':string,
      'created_at': timestamp,
      'updated_at': timestamp,
      'config': {'enabled': boolean},
      'owner': string,
    }
}
```

- **Error Response:**
  - **Code:** 400
    - **Content:** `{ 'statusCode': 400, 'error': 'Bad Request ', 'message': 'Missing required fields' }`
  - **Code:** 403
    - **Content:** `{ 'statusCode': 403, 'error': 'Forbidden', 'message': 'Unauthorized User' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`
    

## **PATCH /featureflags/:id**

Updates data of the feature flag 

- **Params**  
  _Required:_ `id=[string]`
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Body** `{ <featureFlag_object> }`
- **Success Response:**
- **Code:** 204

  - **Content:** `<No Content>`

- **Error Response:**
  - **Code:** 403
    - **Content:** `{ 'statusCode': 403, 'error': 'Forbidden', 'message': 'Unauthorized User' }`
  - **Code:** 404
    - **Content** `{ 'statusCode': 404, 'error': 'Not found', 'message': 'No featureFlag found' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **DELETE /featureflag/:id**

 Deletes the feature flag 

- **Params**  
  _Required:_ `id=[string]`
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Success Response:**
- **Code:** 200
  - **Content:**
```
{
  message: 'Feature Flag deleted successfully!'
}
```
- **Error Response:**
  - **Code:** 403
    - **Content:** `{ 'statusCode': 403, 'error': 'Forbidden', 'message': 'Unauthorized User' }`
  - **Code:** 404
    - **Content** `{ 'statusCode': 404, 'error': 'Not found', 'message': 'No featureFlag found' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`
