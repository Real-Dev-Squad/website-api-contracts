# Events

## Events object

```
{
  'name': string,
  'description': string,
  'room_id':string,
  'template_id': string,
  'enabled': boolean,
  'lock': boolean,
  'region': <'in' | 'us' | 'eu' | 'auto'>,
  'peers': [
    "peer_id": string,
    "peer_id": string,
    "peer_id": string
   ]
  'createdBy': {
    ref: 'User',
  },
  questions: [
     "question_id": string,
     "question_id": string,
     "question_id": string
  ],
  comments: [
    "comment_id": string,
    "comment_id": string,
    "comment_id": string
  ],
  status: <'active' | 'inactive'>,
  'timestamp': {
    'createdAt': new Date(),
    'updatedAt': new Date(),
  }
}
```

## Requests

| Route                                                       | Description                                                            |
| ----------------------------------------------------------- | ---------------------------------------------------------------------- |
| [POST /rooms](#post---rooms)                                | Create a new room, either randomly or with the requested configuration |
| [GET /rooms](#get-rooms)                                    | Get all the rooms                                                      |
| [POST /join](#post---join)                                  | Generate an auth token for a peer to join a room                       |
| [GET /rooms/:id=<ROOM_ID>](#get---roomsidroom_id)           | Retrieves the details of a specific active room.                       |
| [PUT /rooms](#put---rooms)                                  | Update the room, make room enabled/disabled,                           |
| [DELETE /rooms](#delete---rooms)                            | Trigger this request to end an active room.                            |
| [GET /session](#get---session)                              | To retrieve all the sessions.                                          |
| [GET /session/:status](#get---sessionstatus)                | To get currently running session.                                      |
| [GET /session/:id=<SESSION_ID>](#get---sessionidsession_id) | Retrieves the details of a specific active session.                    |

## POST - /rooms

Create a new room, either randomly or with the requested configuration.

- **Params:**
  - None
- **Query**
  - None
- **Body**
  - Optional: `name=[string]` (`name` is the name of the room which is case-insensitive. Accepted characters are `a-z, A-Z, 0-9, and . - : _`)
  - Optional: `description=[string]` (`description` describes the usage of the room.)
  - Optional: template_id=[string] (template_id which can be found from dashboard)
  - Optional: region=[string] (region in which you want to create room.)
  ```json
  {
    "name": "new-room-1662723668",
    "description": "This is a sample description for the room",
    "template_id": "<template_id that you wish to associate w/ the room>",
    "region": "in"
  }
  ```
- **Headers**
  - Content-Type: application/json
  - Authorization: Bearer <management_token>
- **Cookie**
  - rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 200
    - **Content:**
      ```json
      {
        "id": "<id>",
        "name": "<room_name>",
        "enabled": true,
        "description": "This is a sample description for the room",
        "customer": "<customer_id>",
        "recording_info": {
          "enabled": false
        },
        "template_id": "<template_id>",
        "region": "in",
        "created_at": "",
        "updated_at": ""
      }
      ```
- **Error Response:**
  - **Code:** 401
    - **Content:**
      `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 500
    - **Content**
      `{ 'statusCode': 500, 'error': 'Internal server error', 'message': 'Couldn't create room. Please try again later'`

## GET /rooms

Get all the rooms created

- Params:
  - enabled: true/false
  - hits: Number of rooms in one response
  - offset: Start of response
    - ex: For 1-10 hits: 10, offset: 0; 11-20 hits: 10, offset: 10;
- Query
  - None
- Body
  - None
- Headers
  - Content-Type: application/json
  - Authorization: Bearer <management_token>
- **Cookie**
  - rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 200
    - **Content:**
      ```jsx
      {
      	"limit": 10,
      	"data": [
      		<ROOM_OBJECT>,
      		<ROOM_OBJECT>,
      	]
      }
      ```
- **Error Response:**
  - **Code:** 401
    - **Content:**
      `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 500
    - **Content**
      `{ 'statusCode': 500, 'error': 'Internal server error', 'message': 'Couldn't get rooms. Please try again later'`

## POST - /join

Generate an auth token for a peer to join a room.

To join the event (for guests, host, maven, moderator)

- **Params:**
  - None
- **Query**
  - None
- **Body**
  - Required: **`room_id=[string]`** (The ID of the room to which the user belongs)
  - Required: **`user_id=[string]`** (The ID of the user for whom the token is being generated)
  - Required: **`role=[string]`** (The role of the user in the room)
  ```json
  {
    "room_id": "<room_id>",
    "user_id": "<user_id>",
    "role": "<role>"
  }
  ```
- **Headers**
  - Content-Type: application/json
- **Cookie**
  - rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 200
    - **Content:**
      ```json
      {
        "token": "<auth_token>",
        "msg": "Token generated successfully!",
        "success": true
      }
      ```
- **Error Response:**
  - **Code:** 500
    - **Content**
      **`{ "msg": "Some error occured!", "success": false }`**

## GET - /rooms/:id=<room_id>

Retrieves the details of a specific active room.

- **Params:**
  - **`id=[string]`** (The ID of the room to retrieve)
- **Query**
  - None
- **Body**
  - None
- **Headers**
  - Authorization: Bearer <management_token>
- **Cookie**
  - rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 200
    - **Content:**
      ```json
      {
        "id": "<room_id>",
        "name": "<room_name>",
        "customer_id": "<customer_id>",
        "session": {
          "id": "<session_id>",
          "created_at": "",
          "peers": ["peer_id", "peer_id", "peer_id"]
        }
      }
      ```
- **Error Response:**
  - **Code:** 401
    - **Content:**
      **`{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`**
  - **Code:** 404
    - **Content**
      **`{ 'statusCode': 404, 'error': 'Not Found', 'message': 'Room not found' }`**
  - **Code:** 500
    - **Content**
      **`{ 'statusCode': 500, 'error': 'Internal server error', 'message': 'Unable to retrieve room details' }`**

## PUT - /rooms

Update the room, make room enabled/disabled,

- **Params:**
  - None
- **Query**
  - None
- **Body**
  - Required: `id=[string]` (room id)
  - Required: `enabled=[boolean]` (Flag to indicate if the room is enabled)
  ```json
  {
    "id": "<room_id>",
    "enabled": false
  }
  ```
- **Headers**
  - Content-Type: application/json
  - Authorization: Bearer <management_token>
- **Cookie**
  - rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 200
    - **Content:**
      ```json
      {
        "message": "Room is enabled"
      }
      ```
- **Error Response:**
  - **Code:** 401
    - **Content:**
      `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 500
    - **Content**
      `{ 'statusCode': 500, 'error': 'Internal server error', 'message': 'Couldn't update room. Please try again later'`

## DELETE - /rooms

Trigger this request to end an active room.

- **Params:**
  - None
- **Query**
  - None
- **Body**
  - **`<room_id>`** (required) : string - The ID of the room to end an active room.
  - Required: **`reason=[string]`** (reason for ending the room)
  - Optional: **`lock=[boolean]`** (if true, no new peers will be allowed to join the room after it is ended.)
  ```json
  {
  		"id": "<room_id>"
      "reason": "Class has ended",
      "lock": false
  }
  ```
- **Headers**
  - Content-Type: application/json
  - Authorization: Bearer <management_token>
- **Cookie**
  - rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 200
    - **Content:**
      ```json
      {
        "message": "session is ending"
      }
      ```
- **Error Response:**
  - **Code:** 401
    - **Content:**
      **`{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`**
  - **Code:** 500
    - **Content**
      **`{ 'statusCode': 500, 'error': 'Internal server error', 'message': 'Couldn't end the room. Please try again later'}`**

## GET - /session

To retrieve all the sessions.

- Params:
  - hits: Number of sessions in one response
  - offset: Start of response
    - ex: For 1-10 hits: 10, offset: 0; 11-20 hits: 10, offset: 10;
- Query
  - None
- Body
  - None
- Headers
  - Content-Type: application/json
  - Authorization: Bearer <management_token>
- **Cookie**
  - rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 200
    - **Content:**
      ```jsx
      {
      	"limit": 10,
      	"data": [
      		<SESSION_OBJECT>,
      		<SESSION_OBJECT>,
      	]
      }
      ```
- **Error Response:**
  - **Code:** 401
    - **Content:**
      `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 500
    - **Content**
      `{ 'statusCode': 500, 'error': 'Internal server error', 'message': 'Couldn't get sessions. Please try again later'`

## GET - /session/:status

To get currently running session.

- Params:
  - active: true/false
  - hits: Number of sessions in one response
  - offset: Start of response
    - ex: For 1-10 hits: 10, offset: 0; 11-20 hits: 10, offset: 10;
- Query
  - None
- Body
  - None
- Headers
  - Content-Type: application/json
  - Authorization: Bearer <management_token>
- **Cookie**
  - rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 200
    - **Content:**
      ```jsx
      {
      	"limit": 10,
      	"data": [
      		<SESSION_OBJECT>,
      		<SESSION_OBJECT>,
      	]
      }
      ```
- **Error Response:**
  - **Code:** 401
    - **Content:**
      `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 500
    - **Content**
      `{ 'statusCode': 500, 'error': 'Internal server error', 'message': 'Couldn't get sessions. Please try again later'`

## GET - /session/:id=<SESSION_ID>

Retrieves the details of a specific active session.

- **Params:**
  - **`id=[string]`** (The ID of the session to retrieve)
- **Query**
  - None
- **Body**
  - None
- **Headers**
  - Authorization: Bearer <management_token>
- **Cookie**
  - rds-session: `<JWT>`
- **Success Response:**
  - **Code:** 200
    - **Content:**
      ```json
      {
        "id": "<session_id>",
        "room_id": "<room_id>",
        "customer_id": "<customer_id>",
        "active": false,
        "peers": {
          "peer_id": {},
          "peer_id": {}
        }
      }
      ```
- **Error Response:**
  - **Code:** 401
    - **Content:**
      **`{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`**
  - **Code:** 404
    - **Content**
      **`{ 'statusCode': 404, 'error': 'Not Found', 'message': 'Session not found' }`**
  - **Code:** 500
    - **Content**
      **`{ 'statusCode': 500, 'error': 'Internal server error', 'message': 'Unable to retrieve session details' }`**
