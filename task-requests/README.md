
# Task Requests
#### Task Request Object:
```
{
    taskId: <string>
    externalIssueId: <string>
    requestType: ASSIGNMENT | CREATION
    users : <Array<users>>
    status : PENDING | APPROVED | DENIED
    taskTitle: <string>
    usersCount: <number>
    createdAt: <epoch>
    createdBy: <string>
    lastModifiedAt: <epoch>
    lastModifiedBy: <string>
}
```
#### Users Object:
```
{
	userId: <string>
	status: APPROVED | PENDING 
	proposedDeadline: <epoch>
	proposedStartDate: <epoch>
  description: <string>
}
```

## **Requests**

|               Route                |    Description    |
| :--------------------------------: | :---------------: |
|      [GET /taskRequests](#get-task-requests)      | Return all task requests |
|      [GET /taskRequests/:id](#get-task-requests-id)      | Return task request details for the provided id |
|     [POST /taskRequests](#post-task-requests)     | Creates new task request  |
|     [PATCH /taskRequests](#patch-task-requests) |   Updates task request   |
|     [PATCH /taskRequests/approve](#patch-task-requests-approve) |   Deprecated api, please dont use  |


## **GET /taskRequests**

Return all the task-requests

- **Params**  
  None
- **Query**  
  - Optional: `dev=[boolean]` (`dev` is passed to get all tasks requests in the developer mode with features that are flagged)
  - Optional: `q=[string]` (`q` can have the following values)
      - Optional: `status=[string]` (`status` is a case senstive string with one of the following values [approved,pending, denied] )
      - Optional: `request-type=[string]` (`request-type` is the request type of the task request. Allowed values are [assignment, creation] )
      - Optional: `sort=[string]` (`sort` sorts the response based on value provided. Allowed values [requestors-asc,requestors-desc,created-asc,created-desc])
  - Optional: `size=[integer]` (`size` is the number of task requests per page. Range of value is 1-100.)
  - Optional: `next=[string]` (`next` is id of the document to get next page of results from that document)
  - Optional: `prev=[string]` (`prev` is id of the document to get prev page of results from that document)
- **Body**  
  None
- **Headers**  
  None
- **Cookie**  
  rds-session: `<JWT SUPERUSER>`
- **Success Response:**
 ```
	Code: 200
	Content:{
			  message: 'Task requests returned successfully'
			  data: [
			           {<task_request_object>},
			           {<task_request_object>}
			        ]
			  next: <string>
			}
```
  - **Error Response:**
``` 
    Code: 500
	Content: { 'statusCode': 500, 
				'error': 'Internal Server Error', 
				'message': 'An internal server error occurred' 
		}
```
## **GET /taskRequests/:id**

Return task-request with `id`

- **Params**  
  id=<task_request_object_id>
- **Query**  
  - Optional: `dev=[boolean]` (`dev` is passed to get all tasks requests in the developer mode with features that are flagged)
 - **Body**  
  None
- **Headers**  
  None
- **Cookie**  
  rds-session: `<JWT SUPERUSER>`
- **Success Response:**
```
Code: 200
Content: {
			message: 'Task request returned successfully'
			data:{<task_request_object>}
	}
```
  - **Error Response:**
``` 
    Code: 500
	Content: { 
				'statusCode': 500, 
				'error': 'Internal Server Error', 
				'message': 'An internal server error occurred' 
		}
```
```
	Code: 404   (We return 404 when wrong task Request Id is requested.)
	Content: {
			   message:  "Task request not found",
		}
```

## **POST /taskRequests**

- **Params**  
  None
- **Query**  
    - Optional: `dev=[boolean]` (`dev` is passed to get all tasks requests in the developer mode with features that are flagged)
- **Headers**  
  Content-Type: application/json
- **Body** 
```
	content: { <task_request_object> }
```
- **Cookie**  
  rds-session: `<JWT>`
- **Success Response:**
```
	Code: 201
	Content: {
			  message: 'Task request created successfully'
			  taskRequest: {<task_request_object>}
		}
```
  - **Error Response:**
``` 
    Code: 500
	Content: { 'statusCode': 500, 
				'error': 'Internal Server Error', 
				'message': 'An internal server error occurred' 
		}
```
```
	Code: 401
	Content: {statusCode: 500, 'error': "Unauthorized User"}`
```
```
Code: 409
Content: {
			'statusCode': 409,
			'error': 'Conflict',
			'message': 'User is already requesting for the task'
	}
```
```
Code: 409
Content: {
			'statusCode': 409,
			'error': 'Conflict',
			'message': 'User does not exist'
	}
```
```		
Code: 409
Content:{
			'statusCode': 409,
			'error': 'Conflict',
			'message': 'User Status does not exist'
	}
```
```
Code: 409
Content: {
			'statusCode': 409,
			'error': 'Conflict',
			'message': "Task does not exist"
	}
```
    
## **PATCH /taskRequests/:**

- **Params**  
- **Query**  
  - Optional: `dev=[boolean]` (`dev` is passed to get all tasks requests in the developer mode with features that are flagged)
  - Required: `action=[approve | reject]` (`action` action either approves or rejects the task request of the given id)
- **Headers**  
  Content-Type: application/json
- **Body** 
```
	content: {
			taskRequestId : <id>
			userId : <userId>
		}
```
- **Cookie**  
  rds-session: `<JWT SUPERUSER>`
- **Success Response:**
```
	Code: 200
	Content: {
			  message: 'Task request updated successfully'
			  taskRequest: {<task_request_object>}
		}
```
  - **Error Response:**
``` 
    Code: 500
	Content: { 'statusCode': 500, 
				'error': 'Internal Server Error', 
				'message': 'An internal server error occurred' 
		}
```
```
Code: 409
Content: {
			'statusCode': 409,
			'error': 'Conflict',
			'message': 'User does not exist'
	}
```
```
	Code: 401
	Content: {statusCode: 500, 'error': "Unauthorized User"}`
```
