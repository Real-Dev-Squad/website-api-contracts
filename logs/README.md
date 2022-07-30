# Logs

## Log object

```
{
    type: string,
    meta: object,
    body: object
    timestamp: object
}
```

## **Requests**

|               Route               |                      Description                       |
| :-------------------------------: | :----------------------------------------------------: |
| [GET /logs/cache](#get-logscache) | Returns the cache logs and its count of last 24 hours  |
| [GET /logs/:type](#get-logstype)  | Returns all logs according to the provided type of log |

---

## **GET /logs/cache**

Returns the cache logs and its count of last 24 hours.

- **Params**  
  None
- **Query**  
  None
- **Body**  
  None
- **Headers**  
  None
- **Cookie**  
  rds-session: `<JWT>`

- **Success Response:**

  - **Code:** 200
    - **Content:**
    ```
    {
        message: "Cache Logs returned successfully!",
        count: <LOGS_COUNT>,
        logs: [[Log Object 1](#log-object), [Log Object 2](#log-object)....],
    }
    ```

- **Error Responses:**
  - **Code:** 401
    - **Content:**
    ```
    {
        "statusCode": 401,
        "error": "Unauthorized",
        "message": "Unauthenticated User"
    }
    ```
  - **Code:** 500
    - **Content:**
    ```
    {
        "statusCode": 500,
        "error": "Internal Server Error",
        "message": "Something went wrong. Please contact admin"
    }
    ```

## **GET /logs/:type**

**Note :**

- This API can only be accessed by super user

Returns all logs according to the provided **type** of log.

- **Params**  
  None
- **Query**  
  None
- **Body**  
  None
- **Headers**  
  None
- **Cookie**  
  rds-session: `<JWT>`

- **Success Response:**

  - **Code:** 200
    - **Content:**
    ```
    {
        message: "Logs returned successfully!",
        logs: [[Log Object 1](#log-object), [Log Object 2](#log-object)....],
    }
    ```

- **Error Responses:**
  - **Code:** 401
    - **Content:**
    ```
    {
        "statusCode": 401,
        "error": "Unauthorized",
        "message": "Unauthenticated User"
    }
    ```
  - **Code:** 500
    - **Content:**
    ```
    {
        "statusCode": 500,
        "error": "Internal Server Error",
        "message": "Something went wrong. Please contact admin"
    }
    ```
