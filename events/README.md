# Events

## Events object

```
{
    'id': "id", // This is event id
    'name': "string",
    'description': "string",
    'room_id': "string",
    'session_id': "string",
    'template_id': "string",
    'enabled': "boolean",
    'lock': "boolean",
    'region': <'in' | 'us' | 'eu' | 'auto'>,
    'created_by': "<USER_ID_OF_THE_USER_WHO_CREATED_THE_EVENT>",
    'peers': [
      <peer_object_id1>,
      <peer_object_id2>,
      <peer_object_id3>,
      <peer_object_id4>,
      ...
      <peer_object_idn>,
    ],
    'questions': [
      "<question_object_id1>",
      "<question_object_id2>",
      ...
      "<question_object_idn>"
    ],
    'comments': [
      "<comment_object_id1>",
      "<comment_object_id2>",
      ...
      "<comment_object_idn>",
    ],
    'status': <'active' | 'inactive'>
    'timestamps': {
      'created_at': timestamp,
      'updated_at': timestamp,
      }
}
```

## Peers object

```
{
  'id': string,
  'name': string,
  'joinedEvents': [
    {
      'event_id':"<event_object_id1>",
      'joined_at': timestamp,
      'left_at': timestamp,
      'role': <'host' | 'moderator' | 'guest' | 'maven'>,
    },
    {
      'event_id':"<event_object_id2>",
      'joined_at': timestamp,
      'left_at': timestamp,
      'role': <'host' | 'moderator' | 'guest' | 'maven'>,
    },
    ...
    {
      'event_id':"<event_object_idn>",
      'joined_at': timestamp,
      'left_at': timestamp,
      'role': <'host' | 'moderator' | 'guest' | 'maven'>,
    }
  ],
  'rds_user': {
    is_rds_user:boolean,
    user_id: "<USER_ID_OF_THE_USER_WHO_CREATED_THE_EVENT>"
  },
}
```

## Questions object

```
{
  'id': string,
  'created_by': "<USER_ID_OF_THE_USER_WHO_CREATED_THE_EVENT>",
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

| Route                                                 | Description                                                             |
| ----------------------------------------------------- | ----------------------------------------------------------------------- |
| [POST /events](#post---events)                        | Create a new event, either randomly or with the requested configuration |
| [GET /events](#get-events)                            | Get all the events                                                      |
| [POST /events/join](#post---eventsjoin)               | Generate an auth token for a peer to join a event                       |
| [GET /events/:id=<EVENT_ID>](#get---eventsidevent_id) | Retrieves the details of a specific active event.                       |
| [PATCH /events](#patch---events)                      | Update the event, make event enabled/disabled,                          |
| [PATCH /events/end](#patch---eventsend)               | Trigger this request to end an active event.                            |

## POST - /events

Create a new event, either randomly or with the requested configuration.

- **Params:**
  - None
- **Query**
  - None
- **Body**
  - `name=[string]` - required (`name` is the name of the event which is case-insensitive. Accepted characters are `a-z, A-Z, 0-9, and . - : _`)
  - `description=[string]` - required (`description` describes the usage of the event.)
  - template_id=[string] - required (template_id which can be found from dashboard)
  - region=[string] - required (region in which you want to create event.)
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
  - **Code:** 201
    - **Content:**
      ```json
      {
        "id": "<id>",
        "name": "<event_name>",
        "description": "This is a sample description for the event",
        "enabled": true,
        "created_by": "user_id",
        "room_id": "<room_id>",
        "template_id": "<template_id>",
        "region": "in",
        "created_at": "YYYY-MM-DDTHH:MM:SS.sssZ",
        "updated_at": "YYYY-MM-DDTHH:MM:SS.sssZ"
      }
      ```
- **Error Response:**
  - **Code:** 400
    - **Content:**
      `{ 'statusCode': 400, 'error': 'Bad Request', 'message': 'Invalid request body or missing required parameters' }`
  - **Code:** 401
    - **Content:**
      `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 500
    - **Content**
      `{ 'statusCode': 500, 'error': 'Internal server error', 'message': 'Couldn't create event. Please try again later'`

## GET /events

Get all the created events.

- Params
  - None
- Query
  - enabled: true/false
  - limit: Number of events in one response
  - offset: Start of response
    - ex: For 1-10 limit: 10, offset: 0; 11-20 limit: 10, offset: 10;
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
      ```json
      {
        "limit": 10,
        "data": ["<event_object>", "<event_object>"],
        "start": "<room_id>"
      }
      ```
- **Error Response:**
  - **Code:** 400
    - **Content:**
      `{ 'statusCode': 400, 'error': 'Bad Request', 'message': 'Invalid request body or missing required parameters' }`
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
- **Success Response:**
  - **Code:** 201
    - **Content:**
      ```json
      {
        "token": "<auth_token>",
        "message": "Token generated successfully!",
        "success": true
      }
      ```
- **Error Response:**
  - **Code:** 500
    - **Content**
      `{ "message": "Some error occured!", "success": false }`

## GET - /events/:id=<event_id>

Retrieves the details of a specific active event_id.

- **Params:**
  - **`id=[string]`** (The ID of the event to retrieve)
- **Query**
  - None
- **Body**
  - isActiveRoom=[boolean] - required (isActiveRoom indicates whether the room is active or not)
- **Headers**
  - Authorization: Bearer <management_token>
- **Cookie**
  - rds-session: `<JWT>`
- **Success Response:**
  - if room is active
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
          "peers": ["<peer_object>", "<peer_object>", "<peer_object>"]
        }
      }
      ```
  - if room is not active
    - **Code:** 200
    - **Content:**
      ```json
      {
        "id": "<id>",
        "name": "<event_name>",
        "description": "This is a sample description for the event",
        "enabled": true,
        "created_by": "user_id",
        "room_id": "<room_id>",
        "template_id": "<template_id>",
        "region": "in",
        "created_at": "YYYY-MM-DDTHH:MM:SS.sssZ",
        "updated_at": "YYYY-MM-DDTHH:MM:SS.sssZ"
      }
      ```
- **Error Response:**
  - **Code:** 400
    - **Content:**
      `{ 'statusCode': 400, 'error': 'Bad Request', 'message': 'Invalid request body or missing required parameters' }`
  - **Code:** 401
    - **Content:**
      `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 404
    - **Content**
      `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'Event not found' }`
  - **Code:** 500
    - **Content**
      `{ 'statusCode': 500, 'error': 'Internal server error', 'message': 'Unable to retrieve event details' }`

## PATCH - /events

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
        "message": "Event is enabled."
      }
      ```
- **Error Response:**
  - **Code:** 400
    - **Content:**
      `{ 'statusCode': 400, 'error': 'Bad Request', 'message': 'Invalid request body or missing required parameters' }`
  - **Code:** 401
    - **Content:**
      `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 404
    - **Content**
      `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'Event not found' }`
  - **Code:** 500
    - **Content**
      `{ 'statusCode': 500, 'error': 'Internal server error', 'message': 'Couldn't update event. Please try again later'`

## PATCH - /events/end

Trigger this request to end an active event.

- **Params:**
  - None
- **Query**
  - None
- **Body**
  - **`<event_id>`** (required) : string - The ID of the event to end an active event.
  - **`reason=[string]`** - required (reason for ending the event)
  - **`lock=[boolean]`** - required (if true, no new peers will be allowed to join the event after it is ended.)
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
        "message": "Event ended successfully."
      }
      ```
- **Error Response:**
  - **Code:** 400
    - **Content:**
      `{ 'statusCode': 400, 'error': 'Bad Request', 'message': 'Invalid request body or missing required parameters' }`
  - **Code:** 401
    - **Content:**
      `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 404
    - **Content**
      `{ 'statusCode': 404, 'error': 'Not Found', 'message': 'Event not found' }`
  - **Code:** 500
    - **Content**
      `{ 'statusCode': 500, 'error': 'Internal server error', 'message': 'Couldn't end the event. Please try again later'}`
