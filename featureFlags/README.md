# FeatureFlags

# FeatureFlag object

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
| [GET /featureFlags](#get-featureFlags)  |   Returns all feature flags in system  |
| [POST /featureFlags](#post-featureFlags)   |   Creates a feature flag            |
| [PATCH /featureFlags/:id](#patch-featureFlagsId)   | Updates data of featureflag |
| [DELETE /featureFlags/:id](#delete-featureFlagsId) |  Deletes the feature flag   |

## **GET /featureFlags**

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
  message: 'FeatureFlags returned successfully!'
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

## **POST /featureFlags** 

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
  message: 'FeatureFlag added successfully!'
  featureFlag: {
      'name': string,
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
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`
    

# **PATCH /featureFlags/:id**

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
  - **Content:** 
 ```
{
  message: 'FeatureFlag updated successfully!'
}
```
- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 404
    - **Content** `{ 'statusCode': 404, 'error': 'Not found', 'message': 'No featureFlags found' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

# **DELETE /featureFlag/:id**

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
  message: 'FeatureFlag deleted successfully!'
}
```
- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 404
    - **Content** `{ 'statusCode': 404, 'error': 'Not found', 'message': 'No featureFlags found' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`
