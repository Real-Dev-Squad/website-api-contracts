# Task Requests

### Task Request Object:

```
{
    taskId: <string>
		externalIssueId: <string>
    requestType: ASSIGNMENT | CREATION
    requestors : <Array<Requestors>>
    status : PENDING | APPROVED | DENIED
    createdAt: <epoch>
    createdBy: <string>
    lastModifiedAt: <epoch>
    lastModifiedBy: <epoch>
}

```

### Requestors Object:

```
{
	userId: <string>
	status: APPROVED | PENDING | NOT_ASSIGNED
	proposedDeadline: <epoch>
}

```

## **Requests**

| Route | Description |
| --- | --- |
| notion://www.notion.so/ajeyakrishna-karanth/Rough-3fa42d091c5243f2956e4c266cd805f4#get-task-requests | Return all task requests |
| notion://www.notion.so/ajeyakrishna-karanth/Rough-3fa42d091c5243f2956e4c266cd805f4#get-task-requests-id | Return task request details for the provided id |
| notion://www.notion.so/ajeyakrishna-karanth/Rough-3fa42d091c5243f2956e4c266cd805f4#post-task-requests | Creates new task request |
| notion://www.notion.so/ajeyakrishna-karanth/Rough-3fa42d091c5243f2956e4c266cd805f4#patch-task-requests-id | Updates task request |

## **GET /task-requests**

Return all the task-requests

- **Params**
None
- **Query**
    - Optional: `dev=[boolean]` (`dev` is passed to get all task requests in the developer mode with features that are flagged)
    - Optional: `q=[string]` (`q` can have the following values)
        - Optional: `status=[string]` (`status` is a case insensitive string with one of the following values [APPROVED,PENDING, DENIED] )
        - Optional: `taskId=[string]` (`taskId` is the taskId related to the task request)
        - Optional: `requestors=[string]` (`requestors` is the userId of the requestor )
        - Optional: `requestType=[string]` (`requestType` is the type of the task request. Allowed values are [ASSIGNMENT, CREATION] )
    - Optional: `size=[integer]` (`size` is the number of task requests per page. Range of value is 1-100.)
    - Optional: `cursor=[string]` (`cursor` is id of the document to get next page of results from that document)
    - Optional: `order=[string]` (`order` specifies the sorting order [asc,desc])
    - Optional: `orderBy=[string]` (`orderBy` sorts the response based on value provided. Allowed values [requestorCount, createdTime])
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
Content: {
				   'statusCode': 500,
				   'error': 'Internal Server Error',
				   'message': 'An internal server error occurred'
			   }
```

## **GET /task-requests/:id**

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
	Code: 404
	Content: {
			      message:  "Task request not found",
			     }

```

## **POST /task-requests**

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

## **PATCH /task-requests/:id**

- **Params***Required:* `id=[string]`
- **Query**
    - Optional: `dev=[boolean]` (`dev` is passed to get all tasks requests in the developer mode with features that are flagged)
- **Headers**
Content-Type: application/json
- **Body**

```
	content: { <task_request_object> }

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
	Code: 404
	Content: {
			      message:  "Task request not found",
			     }

```