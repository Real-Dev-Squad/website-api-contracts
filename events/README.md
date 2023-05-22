# Events

## Events object

```
{
  'name': string,
  'description': string,
  'event_id':string,
  'template_id': string,
  'enabled': boolean,
  'lock': boolean,
  'region': <'in' | 'us' | 'eu' | 'auto'>,
  'peers': [
    "<peer_id>",
    "<peer_id>",
    "<peer_id>"
   ]
  'created_by': {
    ref: 'User',
  },
  'questions': [
    "<question_object_id>",
    "<question_object_id>",
    "<question_object_id>"
  ],
  'comments: [
    "<comment_object_id>",
    "<comment_object_id>",
    "<comment_object_id>",
  ],
  'status': <'active' | 'inactive'>,
  'timestamp': {
    'created_at': timestamp,
    'updated_at': timestamp,
  }
}
```

## Peers object

```
{
  'id': string,
  'session_id': string,
  'name': string,
  'role': <'host' | 'moderator' | 'guest' | 'maven'>,
  'user_id': string,
  'joined_at': timestamp,
  'left_at': timestamp,
  'is_rds_user': boolean,
  // extends user model
}
```

## Questions object

```
{
	'id': string,
	'user_id': string,
	'question': string,
	'is_new': boolean, //default to true,
	'session_id': string,
	'timestamps': {
    'created_at': timestamp,
    'updated_at': timestamp,
	}
}
```

## Comments object

```
{
	'id': string,
	'comment': string,
	'comment_by': string, //user id of the user who commented
	'is_blocked': boolean,
	'is_pinned': boolean,
	'session_id': string,
	'timestamps': {
    'created_at': timestamp,
    'updated_at': timestamp,
	}
}

```

> Note: The <management_token> for the Authorization is form a connection between RDS backend and 100ms backend.

## Requests

| Route                                                        | Description                                                             |
| ------------------------------------------------------------ | ----------------------------------------------------------------------- |
| [POST /events](#post---events)                               | Create a new event, either randomly or with the requested configuration |
| [GET /events](#get-events)                                   | Get all the events                                                      |
| [POST /events/join](#post---eventsjoin)                      | Generate an auth token for a peer to join a event                       |
| [GET /events/:id=<EVENT_ID>](#get---eventsidevent_id)        | Retrieves the details of a specific active event.                       |
| [PUT /events](#put---events)                                 | Update the event, make event enabled/disabled,                          |
| [DELETE /events](#delete---events)                           | Trigger this request to end an active event.                            |
| [GET /sessions](#get---session)                              | To retrieve all the sessions.                                           |
| [GET /sessions/:status](#get---sessionstatus)                | To get currently running session.                                       |
| [GET /sessions/:id=<SESSION_ID>](#get---sessionidsession_id) | Retrieves the details of a specific active session.                     |

## POST - /events

Create a new event, either randomly or with the requested configuration.

- **Params:**
  - None
- **Query**
  - None
- **Body**
  - Optional: `name=[string]` (`name` is the name of the event which is case-insensitive. Accepted characters are `a-z, A-Z, 0-9, and . - : _`)
  - Optional: `description=[string]` (`description` describes the usage of the event.)
  - Optional: template_id=[string] (template_id which can be found from dashboard)
  - Optional: region=[string] (region in which you want to create event.)
  ```json
  {
    "name": "new-event-1662723668",
    "description": "This is a sample description for the event",
    "template_id": "<template_id that you wish to associate w/ the event>",
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
        "name": "<event_name>",
        "enabled": true,
        "description": "This is a sample description for the event",
        "customer": "<customer_id>",
        "recording_info": {
          "enabled": false
        },
        "template_id": "<template_id>",
        "region": "in",
        "created_at": "YYYY-MM-DDTHH:MM:SS.sssZ",
        "updated_at": "YYYY-MM-DDTHH:MM:SS.sssZ"
      }
      ```
- **Error Response:**
  - **Code:** 401
    - **Content:**
      `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 500
    - **Content**
      `{ 'statusCode': 500, 'error': 'Internal server error', 'message': 'Couldn't create event. Please try again later'`

## GET /events

Get all the created events.

- Params:
  - enabled: true/false
  - hits: Number of events in one response
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
      		<event_object>,
      		<event_object>,
      	]
      }
      ```
- **Error Response:**
  - **Code:** 401
    - **Content:**
      `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 500
    - **Content**
      `{ 'statusCode': 500, 'error': 'Internal server error', 'message': 'Couldn't get events. Please try again later'`

## POST - /events/join

Generate an auth token for a peer to join a event.

To join the event (for guests, host, maven, moderator)

- **Params:**
  - None
- **Query**
  - None
- **Body**
  - Required: **`event_id=[string]`** (The ID of the event to which the user belongs)
  - Required: **`user_id=[string]`** (The ID of the user for whom the token is being generated)
  - Required: **`role=[string]`** (The role of the user in the event)
  ```json
  {
    "event_id": "<event_id>",
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

## GET - /events/:id=<event_id>

Retrieves the details of a specific active event_id.

- **Params:**
  - **`id=[string]`** (The ID of the event to retrieve)
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
        "id": "<event_id>",
        "name": "<event_name>",
        "customer_id": "<customer_id>",
        "session": {
          "id": "<session_id>",
          "created_at": "YYYY-MM-DDTHH:MM:SS.sssZ",
          "peers": [
            <peer_object>,
            <peer_object>,
            <peer_object>
          ]
        }
      }
      ```
- **Error Response:**
  - **Code:** 401
    - **Content:**
      **`{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`**
  - **Code:** 404
    - **Content**
      **`{ 'statusCode': 404, 'error': 'Not Found', 'message': 'Event not found' }`**
  - **Code:** 500
    - **Content**
      **`{ 'statusCode': 500, 'error': 'Internal server error', 'message': 'Unable to retrieve event details' }`**

## PUT - /events

Update the event, make event enabled/disabled,

- **Params:**
  - None
- **Query**
  - None
- **Body**
  - Required: `id=[string]` (event id)
  - Required: `enabled=[boolean]` (Flag to indicate if the event is enabled)
  ```json
  {
    "id": "<event_id>",
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
        "message": "Event is enabled"
      }
      ```
- **Error Response:**
  - **Code:** 401
    - **Content:**
      `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 500
    - **Content**
      `{ 'statusCode': 500, 'error': 'Internal server error', 'message': 'Couldn't update event. Please try again later'`

## DELETE - /events

Trigger this request to end an active event.

- **Params:**
  - None
- **Query**
  - None
- **Body**
  - **`<event_id>`** (required) : string - The ID of the event to end an active event.
  - Required: **`reason=[string]`** (reason for ending the event)
  - Optional: **`lock=[boolean]`** (if true, no new peers will be allowed to join the event after it is ended.)
  ```json
  {
    "id": "<event_id>",
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
        "message": "Session is ending."
      }
      ```
- **Error Response:**
  - **Code:** 401
    - **Content:**
      **`{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`**
  - **Code:** 500
    - **Content**
      **`{ 'statusCode': 500, 'error': 'Internal server error', 'message': 'Couldn't end the event. Please try again later'}`**

## GET - /sessions

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
      		<session_object>,
      		<session_object>,
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

## GET - /sessions/:status

To get currently running sessions.

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
      		<session_object>,
      		<session_object>,
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

## GET - /sessions/:id=<SESSION_ID>

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
        "event_id": "<event_id>",
        "customer_id": "<customer_id>",
        "active": false,
        "peers": [
      		<peer_object>,
      		<peer_object>
      	]
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
