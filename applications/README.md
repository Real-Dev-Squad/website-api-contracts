# application

## application object

```{
  userId: string,
  biodata: {
    firstName: string,
    lastName: string,
  },
  location: {
    city: string,
    state: string,
    country: string,
  },
  professional: {
    institution: string,
    skills: string,
  },
  intro: {
    introduction: string,
    funFact: string,
    forFun: string,
    whyRds: string,
    numberOfHours: string,
  },
  foundFrom: string,
  status: string || null,
  discordLink: string || null
}
```

## **Requests**

|                         Route                          |             Description              |
| :----------------------------------------------------: | :----------------------------------: |
|                [GET /applications](#get-applications)                |   Returns all the applications in the system   |
|           [GET /applications/:userId](#get-applicationsuserid)            | Returns the logged in user's details |
|  [POST /applications](#post-applications)   |    Returns user with given userId    |
|       [PATCH /applications/:userId](#patch-applicationsuserid)       |   Returns user with given username   |
|        |

## **GET /applications/**

Return all the applications which are not accepted or rejected, this API will only be accessible to super_user

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
    `{ 'message': 'applications returned successfully!', 'applications': <application_object> }`
- **Error Response:**
  - **Code:** 401
    - **Content:**
      `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 500
    - **Content:**
      `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **POST /applications/**

Adds an application for joining RDS, a user can only add one application, until the previous application is either rejected or accepted by super_user

- **Params**  
  None
- **Query** 
  None
- **Body** `{ <application_object> }`
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 201
    - **Content:**
    `{ 'message': 'application added successfully!', 'application': <application_object> }`
- **Error Response:**
  - **Code:** 401
    - **Content:**
      `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 409
    - **Content:**
      `{ 'statusCode': 409, 'error': 'Conflict', 'message': 'User application is already present' }`
  - **Code:** 500
    - **Content:**
      `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`


## **GET /applications/:userId**

Return the application of a particular user, the super can access the application of any user, but any other can only access their application

- **Params**  
  _Required:_ `userId=[string]`
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
    `{ 'message': 'User application returned successfully!', 'application': <application_object> }`
- **Error Response:**
  - **Code:** 401
    - **Content:**
      `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 404
    - **Content:**
      `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'Application doesn't exist' }`
  - **Code:** 500
    - **Content:**
      `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **PATCH /applications/:userId**

This will update a particular application, this API will only be accessible to super_user

- **Params**  
  _Required:_ `userId=[string]`
- **Query** 
  _optional:_ `generate_discord_link=[boolean]`
- **Body**
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 204
    - **Content:**
    `{ 'message': 'Application updated successfully!' }`
- **Error Response:**
  - **Code:** 401
    - **Content:**
      `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 404
    - **Content:**
      `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'Application doesn't exist' }`
  - **Code:** 500
    - **Content:**
      `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`