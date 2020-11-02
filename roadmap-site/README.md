# Challenges

## User Object


same as the users object on Users Api
- [User object](/users#user-object)


## **Requests**

|               Route                |           Description           |
| :--------------------------------: | :-----------------------------: |
|  [GET /challenges](#get-challenges)| Returns all challenges          |
|[POST /challenges](#post-challenges)|       Creates new challenge     |
|[POST /challenge/subscribe](#post-challengesubscribe) | subscribes users to challenge|

## **GET /challenges**

> returns all the active and completed challenges

## Response

```
[
    {
        "id":"<challenge_id>",
        "title": "Sherlock and Anagrams",
        "level": "Easy",
        "start_date": "10/05/2020",
        "end_date": "10/25/2020",
        "participants": [
            <user_object>,
            <user_object>
        ],
        "is_active": 1,
        "is_user_subscribed":1
    },
    {
        "id":"<challenge_id>",
        "title": "Sherlock and Anagrams",
        "level": "Easy",
        "start_date": "10/05/2020",
        "end_date": "10/25/2020",
        "participants": [
            <user_object>,
            <user_object>
        ],
        "is_active": 0,
        "is_user_subscribed":0
    }
]

```

## **POST /challenges**

## Body

```
{
    "title":"Sherlock and Anagrams",
    "level":"Easy",
    "start_date":"10/05/2020",
    "end_date":"10/22/2020"
}
```

## Response

Return the same response from [GET /challenges](#get-challenges)

## **POST /challenge/subscribe**

> This request allows users to subscribe to active challenges

## Body

Request

```
    {
        "challenge_id":<unique challenge id>,
        "user_id":<userid>
    }
```

Response

```
{
    "challenge_id":<unique challenge id>
    "is_user_subscribed":1
}
```
