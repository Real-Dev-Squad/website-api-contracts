# applicants

## applicants object

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
  reason: string || null
}
```

## **Requests**

|                         Route                          |             Description              |
| :----------------------------------------------------: | :----------------------------------: |
|                [GET /applications](#get-applications)                |   Returns all the applications in the system   |
|  [POST /applications](#post-applications)   |    Add application of a user    |
|       [PATCH /applications/:applicationId](#patch-applicationsapplicationid)       |   Updates application of a user   |
|        |

## **GET /applications/**

Return all the applications which are not accepted or rejected, this API will only be accessible to super_user or user who have filled the application if the userId passed in same as the userId of the user

- **Params**  
  None
- **Query** 
    - Optional: `userId=[string]`: if userId is provided it return application of one user, if the user is not super user then the userId passed here must be the userId of that user only otherwise it will give 403
- **Body**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 200
    - **Content:**
    The following response will be returned if userId is not provided in the query param 
    ```
      { 
        message: 'applications returned successfully! || 'applications returned successfully'',
        applications: [<application_object>, <application_object>]
      }
    ```
    The following response will be returned if userId is provided in the query param 
    ```
      { 
        message: 'applications returned successfully! || 'application returned successfully'',
        application: <application_object>
      }
    ```
- **Error Response:**
  - **Code:** 401
    - **Content:**
      ```
        { 
          statusCode: 401,
          error:'Unauthorized',
          message: 'Unauthenticated User'
        }
      ```
  - **Code:** 500
    - **Content:**
      ```
        { 
          statusCode: 500,
          error: 'Internal Server Error', 
          message: 'An internal server error occurred' 
        }
      ```

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
    ```
      { 
        message: 'User application added.',
      }
    ```
- **Error Response:**
  - **Code:** 401
    - **Content:**
      ```
        { 
          statusCode: 401,
          error: 'Unauthorized',
          message: 'Unauthenticated User' 
        }
      ```
  - **Code:** 409
    - **Content:**
      ```
        { 
          statusCode: 409, 
          error: 'Conflict',
          message: 'Previous application is still under process' 
        }
      ```
  - **Code:** 500
    - **Content:**
      ```
        { 
          statusCode: 500,
          error: 'Internal Server Error',
          message: 'An internal server error occurred' 
        }
      ```



## **PATCH /applications/:applicationId**

This will update a particular application, this API will only be accessible to super_user

- **Params**  
  _Required:_ `applicationId=[string]`
- **Query** 
- **Body**
  _optional_: `feedback=[string]`
  _optional_: `status=[string]`
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 201
    - **Content:**
    ```
      { 
        message: 'Application updated successfully!' 
      }
    ```
- **Error Response:**
  - **Code:** 401
    - **Content:**
      ```
        { 
          statusCode: 401,
          error: 'Unauthorized',
          message: 'Unauthenticated User' 
        }
      ```
  - **Code:** 404
    - **Content:**
      ```
        {
          statusCode: 404,
          error: 'Not Found',
          message: 'Application doesn't exist' 
        }
      ```
  - **Code:** 500
    - **Content:**
      ```
        { 
          statusCode': 500,
          error: 'Internal Server Error',
          message: 'An internal server error occurred' }
      ```