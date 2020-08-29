#Members

- Member object

```
{
	"id": string,
	"first_name": string,
	"last_name": string,
	"yoe": number,
	"company": string,,
	"designation": string,
	"img": string,
	"github_id": string,
	"linkedin_id": string,
	"twitter_id": string,
  "instagram_id": string
}
```

## **GET /member**

Returns all members in the system.

- **URL Params**  
  None
- **Body**  
  None
- **Headers**  
  Content-Type: application/json
- **Success Response:**
- **Code:** 200  
  **Content:**

```
{
  message: "Members returned successfully!"
  members: [
           {<member_object>},
           {<member_object>},
           {<member_object>}
         ]
}
```

## **GET /member/:id**

Returns the specified member.

- **URL Params**  
  _Required:_ `id=[string]`
- **Body**  
  None
- **Headers**  
  Content-Type: application/json
- **Success Response:**
- **Code:** 200  
  **Content:** `{ "message": "Member returned successfully!", "member": <member_object> }`
- **Error Response:**
  - **Code:** 404  
     **Content:** `{ error: 'Not Found', message: "Member doesn't exist" }`  
     OR
  - **Code:** 401  
    **Content:** `{ "statusCode": 401, "error": "Unauthorized", "message": "Unauthenticated User" }`

## **POST /member**

Creates a new Member.

- **URL Params**  
  None
- **Headers**  
  Content-Type: application/json
- **Body** `{ <member_object> }`
- **Success Response:**
- **Code:** 200  
  **Content:** `{ <member_object> }`
- **Error Response:**
  - **Code:** 400  
     **Content:** `{ error: 'Bad Request', message: "Member already exists" }`
