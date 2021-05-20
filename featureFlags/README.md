# FeatureFlags

# FeatureFlag object

```
{
    'name': string,
    'id': string,
    'created_at': timestamp,
    'updated_at': timestamp,
    'config': {'enabled': boolean},
    'owner': string,
    'launched_at': timestamp
}
```

## **Requests**

|                 Route                 |             Description                        |
|:-------------------------------------:|:----------------------------------------------:|
|  [GET /featureFlags](#get-featureFlags)        |   Returns all feature flags in system |
|  [POST /featureFlags](#post-featureFlags)      |   Creates a feature flag              |
|  [PATCH /featureFlags/:id](#patch-featureFlagsId)   | Updates data of the feature flag |
|  [DELETE /featureFlags/:id](#delete-featureFlagsId) |   Deletes the feature flag       |



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
  -**Content:**

```
{
  message: 'FeatureFlags returned successfully!'
  featureFlags: [
           {<featureFlag_object>},
           {<featureFlag_object}
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
- **Body** `
{ 
  'feature_name': <feature name>,
  'feature_owner':<feature owner name>,
  'is_enabled':<feature is enabled or disabled>,
              
 }`
- **Success Response:**
- **Code:** 200
  - **Content:**

```
{
  message: 'FeatureFlag created successfully!'
  featureFlag: {<featureFlag_object>}
  id: <newly created featureFlag id>
}
```

- **Error Response:**
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`


# **PATCH /featureFlags/:id**

Updates data of the feature flag 

- **Params**  
  _Required:_ `id=[string]`

- **Headers**  
  Content-Type: application/json
- **Body** `{ <featureFlag_object> }`
- **Success Response:**
- **Code:** 204

  - **Content:** `<No Content>`

- **Error Response:**
  - **Code** 404
    - **Content** `{ 'statusCode': 404, 'error': 'Not found', 'message': 'No featureFlags found' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`



# **DELETE /featureFlag/:id**

 Deletes the feature flag 

 **Params**  
  _Required:_ `id=[string]`

- **Headers**  
  Content-Type: application/json
- **Success Response:**
- **Code:** 200

  - **Content:**

```
{
  message: 'FeatureFlag deleted successfully!'
}
```

- **Error Response:**
  - **Code** 404
    - **Content** `{ 'statusCode': 404, 'error': 'Not found', 'message': 'No featureFlags found' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`






