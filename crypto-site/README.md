| Endpoints                            | Description                  |
| ------------------------------------ | ---------------------------- |
| GET/shop                             | get all the products         |
| POST/save                            | save for later list for user |
| POST/purchase                        | enable user to make purchase |
| POST/send                            | allow user to send money     |
| POST/request                         | allow user to request money  |
| GET/transactions?user_id='',limit='' | get all transaction for user |
| GET/userinfo?user_id=''              | get all info related to user |

GET/shop
- Body
  - ```json
    {
       "Coffee": {
        "id": "Coffee",
        "image": "coffee.jpeg",
        "name": "Coffee",
        "usage": [
          "Can serve as ice-breaker",
          "Can be used as token of appreciation"
        ],
        "category": "Drink",
        "manufacturer": "Ankush",
        "price": 500
      }
    }
    ```
- Success Response
  - Code
    - 200
    - Content - Send the above body as response
- Error Response

  - Code
    - 404
  - Code
    - 500

POST/save
------
- Params
  - product id = [string] required
  - user id =[string] required
- Body
  - Success or Failure code / Whole list for save later item [TBD]
- Success Response
  - Code
    - 200
    - Content - Save successful
- Error Response
  - Code
    - 404
  - Code
    - 500

POST/purchase
------
- Params
  - user id =[string] required

- Body

  - ```json
    {
      amountToBeDeducted : some amount,
      items : []
    }
    ```
- Success Response

  - Code
    - 200
    - Content - Purchase successful

- Error Response

  - Code
    - 404 - Not enough money/quantity
  - Code
    - 500

POST/send
------
- Params
  - to =[string] required
  - from=[string] required
  - type=[string] required
  - amount = [number] required
- Body
  - Success or Failure code
- Success Response
  - Code
    - 200
    - Content - Transaction successful
- Error Response
  - Code
    - 404
  - Code
    - 500

POST/request
------
- Params
  - to = [string] required
  - from = [string] required
  - type = [string] required
  - amount = [number] required
- Body
  - Success/ Failure code
- Success Response
  - Code
    - 200
- Error Response
  - Code
    - 404
    - 500

GET/transactions?user_id,limit
------
- Query Params

  - user id =[string] required
  - limit = [number] optional

- Body

  - ```json
    {
        "type": "Credit",
        "amount": 2000,
        "sender": "John Doe",
        "receiver": "Foo Bar",
        "date": "August 15 2020"
    }
    ```
- Success Response
  - Code
    - 200
    - Content - Save successful

- Error Response

  - Code
    - 404
  - Code
    - 500

GET/userinfo?user_id
------
- Query Params

  - user id = [string] required
- Body
  ```json
  {
    "name": "John Doe",
    "photo": "url" ,
    "coins": {
      "brass": 200,
      "gold": 100,
      "silver": 300
    },
  	"notification":[],
      "orders": [],
      "transaction":['latest 7']
  }
  ```
  - Success/ Failure code
- Success Response

  - Code
    - 200

- Error Response
  - Code
    - 404
    - 500











