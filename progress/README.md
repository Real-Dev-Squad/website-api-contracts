# Progress

## **Requests**

|                  Route                   |                         Description                          |
| :--------------------------------------: | :----------------------------------------------------------: |
|        [GET /progress](#get-post)        | Retrieves progress entries based on the provided parameters. |
|     [POST /progress](#post-progress)     |                Creates a new progress entry.                 |
| [DELETE /progress/:id](#delete-progress) |       Deletes a progress entry with the specified ID.        |

## **GET /progress**

Retrieves progress entries based on the provided parameters.

- **Params**
  None
- **Query**  
  type (query parameter): Specifies the type of progress to filter (e.g., "task", "user").
  id (query parameter): Specifies the ID of the progress entry to retrieve (optional).
- **Body**  
  None
- **Headers**  
  None
- **Cookie**  
  None
- **Success Response:**
  - **Code:** 200
    - **Content:** 
    ```json
		{
      "message": "User progress retrieved successfully.",
      "progress": [
        { "<user_progress_object_1>" },
        { "<user_progress_object_2>" },
        
      ]
    }
    ```
- **Error Response:**
  - **Code:** 500
    - **Content:** 
    	```json
		{
			"statusCode": 500,
			"error": "Internal Server Error",
			"message": "An internal server error occurred"
		}
		```

- **Example:**
  GET /progress?type=task
    ```json
    {
			"message": "All User Progress found successfully.",
			"progress": [
				{
					"id": "progress1",
					"type": "task",
					"userId": "b0ad77cf461633s7745d59d8",
					"taskId": "d576s3cd4450df7698731b7a",
					"completed": "added modal progress",
					"planned": "write test for modal progress",
					"blockers": "None",
					"createdAt": 1654101900000
				},
				{
					"id": "progress2",
					"type": "task",
					"userId": "4d973377d1ad504b8cs6f576",
					"completed": "added controllers",
					"planned": "add model functions",
					"blockers": "Pending review",
					"date": "2023-04-30",
					"createdAt": 1654101900400
				}
			]
    }
    ```
	GET /progress?type=user&id=ab39f75s4341d870cd67d576
	```json
	{
		"message": "User progress retrieved successfully.",
		"progress": [
			{
				"id": "z5346177sd5f7476d8dac093",
				"type": "user",
				"userId": "ab39f75s4341d870cd67d576",
				"completed": "made backend changes for the feature a23",
				"planned": "make frontend changes for the feature a23",
				"blockers": "Pending review",
				"date": "2023-04-30",
				"createdAt": "1654101900400"
			}
		]
	}
	```
	GET /progress?type=user&id=d757s743d3c54bdf61a68079
	```json
	{
		"message": "No User progress found.",
		"progress": []
	}
	```



## **POST /progress**

	Creates a new progress entry.
- **Params**  
  None
- **Query**
  None
- **Body**  
  Attributes:
    -type (required, string): Specifies the type of the progress entry (e.g., "task", "user").
    -userId (required, string): User ID associated with the progress entry.
    -taskId (optional, string): Task ID associated with the progress entry (applicable for task progress only).
    -completed (required, string): Completed portion of the task or progress.
    -planned (required, string): Planned portion of the task or progress.
    -blockers (required, string): Blockers for the progress entry.
    -date (required, timestamp): Date of the progress entry (applicable for user progress only).
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 201
    - **Content:** 
    ```json
		{
			"message": "Progress entry created successfully.",
			"progress": { "<progress_object>" }
		}
    ```
- **Error Response:**
  - **Code:** 401
    - **Content:** 
	```json
	{
		"statusCode": 401,
		"error": "Unauthorized",
		"message": "Authentication credentials are missing or invalid."
	}
	```
  - **Code:** 400
    - **Content:** 
  	```json
	{
		"statusCode": 400,
		"error": "Bad Request",
		"message": "Task or user not found"
	}
	```

  - **Code:** 500
    - **Content:** 
    ```json
	{
		"statusCode": 500,
		"error": "Internal Server Error",
		"message": "The User Status could not be found as an internal server error occurred."
	}
    ```
- **Example:**
	POST /progress
	Content-Type: application/json
	Request-Body: 
	```json
	{
		"type": "task",
		"userId": "5d4c3f6079a8b775451db867",
		"taskId": "6f53574d684dacd0b791773s",
		"completed": "50%",
		"planned": "80%",
		"blockers": "Waiting for approval"
	}
	```
	Response:
	Status 201 Created
	Content-Type: application/json
	```json
	{
		"message": "Progress entry created successfully.",
		"progress": {
			"id": "5d4c3f6079a8b775451db867",
			"type": "task",
			"userId": "d8d7014sd69f535637c47ab7",
			"taskId": "167cfb7dd45ad967s3380547",
			"completed": "50%",
			"planned": "80%",
			"blockers": "Waiting for approval",
			"createdAt": 1654101900000
		}
	}
	```

## **DELETE /progress/:type/:id**

Deletes a progress entry with the specified ID. This route is restricted to super users only.

- **Params**  
  type (query parameter): Specifies the type of progress to delete (e.g., "task", "user").
  id (query parameter): Specifies the ID of the progress entry to retrieve (optional).
- **Query**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Body**
  None
- **Success Response:**
  - **Code:** 204
    - **Content:** 
	```json
		{
			"message": "Progress entry with ID '<id>' deleted successfully."
		}
	```
- **Error Response:**
  - **Code:** 401
    - **Content:** 
    ```json
		{
			"statusCode": 401,
			"error": "Unauthorized",
			"message": "Authentication credentials are missing or invalid."
		}
		```
  - **Code:** 403
    - **Content:** 
    ```json
		{
			"statusCode": 403,
			"error": "Forbidden",
			"message": "You do not have permission to access this resource."
		}
	```
  - **Code:** 404
    - **Content:** 
  	```json
		{
			"statusCode": 404,
			"error": "Not Found",
			"message": "Task or user not found"
		}
	```
  - **Code:** 500
    - **Content:** 
    ```json
		{
			"statusCode": 500,
			"error": "Internal Server Error",
			"message": "An internal server error occurred."
		}
    ```

- **Example:**
	Request:
	DELETE /progress/5d4c3f6079a8b775451db867
	Content-Type: application/json
	Response:
	status 200 OK
	Content-Type: application/json
	```json
	{
		"message": "Progress entry with ID '5d4c3f6079a8b775451db867' deleted successfully."
	}
	```
