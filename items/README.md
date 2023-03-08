# ItemTags

# itemTags Object

```
{
    "id": "DuNaDRR1fgzDQO3Qh4Aj",
    "itemId": "Ts3ZiJdD4OOPThZrBpc0",
    "tagType": "SKILL",
    "levelName": "test-level-value",
    "levelId": "b0Fnaz6T9yh3ljkKgmPy",
    "levelValue": 2,
    "tagId": "QHuZ5rI9CjWPDhPsxZ61",
    "itemType": "USER",
    "tagName": "EMBER"
  }
```

## **Requests**

|               Route                |    Description    |
| :--------------------------------: | :---------------: |
|      [GET /items/filter](#get-itemsfilter)      | Returns items based on filter |
|      [POST /items](#post-items)      | Creates an item object used for assigning levels and skills |
|      [DELETE /items](#get-tasksself)      | Deletes the items object |

## **GET /items/filter**

Returns all items based on filter

- **Params**  
  None
- **Query**  
  Optional: `itemType` (`itemType` is the type of the item which we have added the tag to, e.g. USER, TASK, it need to be uppercase) <br> 
  
  Optional: `itemId`
  (`itemId` is the Id of items for which all the tags we are trying to fetch, for example, if we want to fetch all the tags that are there on a particular user we can filter it by providing the userId in this query) <br> 
  
  Optional: `levelId` (`levelId` is used when we want ot filter the items based on the levels, this can be used to fetch all the items of a particular level) <br>
  
  Optional: `levelName` (`levelName` is the name of level that we have added in the levels object of db, we can use this to filter the items based on a levelName) <br>
  
  Optional: `levelNumber` (`levelNumber` is the number that we have added in the level object in our RDS db, e.g 1, 2 ,3 etc.)
  
  Optional: `tagId` (`tagId` is the doc id of the tag that we added to a particular item, example of tags are react, ember, etc.)

  Optional: `tagType` (`tagType` is the type of tag that we are finding, currently we are only using one tag, i.e SKILL.)


- **Body**  
  None
- **Headers**  
  Content-Type: application/json
- **Cookie**  
  rds-session: `<JWT>`
- **Success Response:**
- **Code:** 200
  - **Content:**

```
{
  "message": "Items fetched Successfully",
  "data": [
    {items_object}
    {items_object}
    {items_object}
  ]
}

```

- **Error Response:**
  - **Code:** 401
    - **Content:**
      `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 404
    - **Content:**
      `{ 'statusCode': 404, 'error': 'Bad Request', message: '\"itemType\" must only contain uppercase characters' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **POST /items**

- **Params**  
  None
- **Query**  
  None
- **Headers**  
  Content-Type: application/json
- **Body** `{ 
    itemId: <item-id>,
    itemType: <item-type>,
    tagPayload: [
      tagId: <tag-id>,
      levelId: <level-id>,
    ] 
  }`
- **Success Response:**
- **Code:** 200
  - **Content:**

```
{
  "message": "Tags added successfully!",
  "itemId": <item-id>
}
```

- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`

## **DELETE /items**

- **Params**  
  None
- **Query**  
  None
- **Headers**  
  Content-Type: application/json
- **Body** `{ 
    itemId: <item-id>,
    tagId: <tag-id>,
  }`
- **Success Response:**
- **Code:** 200
  - **Content:**

```
{
  "message": "Tags removed successfully!",
  "itemId": <ItemId>,
  "tagId": <tagId>,
}
```

- **Error Response:**
  - **Code:** 401
    - **Content:** `{ 'statusCode': 401, 'error': 'Unauthorized', 'message': 'Unauthenticated User' }`
  - **Code:** 500
    - **Content:** `{ 'statusCode': 500, 'error': 'Internal Server Error', 'message': 'An internal server error occurred' }`