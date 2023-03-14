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
	   "peer_id",
     "peer_id",
     "peer_id"
   ]
  'description': string,
  'createdBy': {
    ref: 'User',
  },
	'timestamp': {
		'createdAt': "",
		'updatedAt': "",
	}
}
```

## Requests

| Method | Route                                                                                        | Description                                                            |
| ------ | -------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| POST   | [/create-room](#post-create-room)                                                            | Create a new room, either randomly or with the requested configuration |
| POST   | [/auth-token](#post-auth-token)                                                              | Generate an auth token for a peer to join a room                       |
| GET    | [/active-rooms/<room_id>](#get-active-roomsroom_id)                                          | Retrieves the details of a specific active room.                       |
| GET    | [/active-rooms/<room_id>/peers](#get-active-roomsroom_idpeers)                               | get the active peers in the room.                                      |
| POST   | [/active-rooms/<room_id>/remove-peers/peer_id](#post-active-roomsroom_idremove-peerspeer_id) | remove/disconnect a connected peer from an active room.                |
| POST   | [/active-rooms/<room_id>/end-room](#post-active-roomsroom_idend-room)                        | Trigger this request to end an active room.                            |

## POST /create-room

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
  - None
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

## POST /auth-token

Generate an auth token for a peer to join a room.

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
  - None
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

## GET /active-rooms/<room_id>

Retrieves the details of a specific active room.

- **Params:**
  - **`room_id=[string]`** (The ID of the room to retrieve)
- **Query**
  - None
- **Body**
  - None
- **Headers**
  - Authorization: Bearer <management_token>
- **Cookie**
  - None
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

## GET /active-rooms/<room_id>/peers

Get the list of peers in a particular active room.

- **Params:**
  - **`room_id=[string]`** (The ID specifies the unique identifier of the active room.)
- **Query**
  - None
- **Body**
  - None
- **Headers**
  - Authorization: Bearer <management_token>
- **Cookie**
  - None
- **Success Response:**
  - **Code:** 200 OK
  - **Content:**
    ```json
    {
      "peers": {
        "<peer_id>": {
          "id": "<peer_id>",
          "name": "<peer_name>",
          "user_id": "<user_id>",
          "metadata": "",
          "role": "guest",
          "joined_at": ""
        },
        "<peer_id>": {
          "id": "<peer_id>",
          "name": "<peer_name>",
          "user_id": "<user_id>",
          "metadata": "",
          "role": "maven",
          "joined_at": "2023-01-25T12:04:30.366388779Z"
        }
      }
    }
    ```
  - **Description:** This response returns a JSON object containing information about the peers in the active room.
  - **Attributes:**
    - **`id`** - Unique identifier of the peer.
    - **`name`** - Name of the peer.
    - **`user_id`** - User ID of the peer.
    - **`metadata`** - Metadata associated with the peer.
    - **`role`** - Role of the peer in the room.
    - **`joined_at`** - The time at which the peer joined the room.
- **Error Responses:**
  - **Code:** 401 Unauthorized
    - **Content:** **`{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`**
    - **Description:** This error response indicates that the provided authorization token is invalid or expired.
  - **Code:** 404 Not Found
    - **Content:** **`{ 'statusCode': 404, 'error': 'Not Found', 'message': 'Room not found' }`**
    - **Description:** This error response indicates that the specified room could not be found.

## POST /active-rooms/<room_id>/remove-peers/peer_id

Remove/disconnect a connected peer from an active room either with their peer_id or role.

<aside>
💡 **Note:**

- If `peer_id` is specified: connected peer will be disconnected
- If the `role` is specified: all the connected peers assigned with that particular role will be disconnected
- If both `peer_id` and `role` are specified -- peer will be disconnected
- Either of the one is required.
</aside>

- **Params:**
  - **`<room_id>`** (required) : string - The ID of the room to remove the peer from.
- **Query:**
  - None
- **Body:**
  - Required: **`peer_id=[string]`** - The ID of the peer to remove from the room.
  - Required: **`role=[string]`** - The role of the peer to remove from the room.
  - Optional: **`reason=[string]`** - The reason for removing the peer from the room.
  ```json
  {
    "peer_id": "<peer_id>",
    "role": "host",
    "reason": ""
  }
  ```
- **Headers:**
  - **`Content-Type: application/json`**
  - **`Authorization: Bearer <management_token>`**
- **Cookie:**
  - None
- **Success Response:**
  - **Code:** 200 OK
    - **Content:**
      ```json
      {
        "message": "peer remove request submitted"
      }
      ```
- **Error Response:**
  - **Code:** 401 Unauthorized
    - **Content:**
      ```json
      {
        "statusCode": 401,
        "error": "Unauthorized",
        "message": "Unauthenticated User"
      }
      ```
  - **Code:** 500 Internal Server Error
    - **Content:**
      ```json
      {
        "statusCode": 500,
        "error": "Internal server error",
        "message": "Couldn't remove peer from room. Please try again later"
      }
      ```

## POST /active-rooms/<room_id>/end-room

Trigger this request to end an active room.

- **Params:**
  - **`<room_id>`** (required) : string - The ID of the room to end an active room.
- **Query**
  - None
- **Body**
  - Required: **`reason=[string]`** (reason for ending the room)
  - Optional: **`lock=[boolean]`** (if true, no new peers will be allowed to join the room after it is ended.)
  ```json
  {
    "reason": "Class has ended",
    "lock": false
  }
  ```
- **Headers**
  - Content-Type: application/json
  - Authorization: Bearer <management_token>
- **Cookie**
  - None
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
