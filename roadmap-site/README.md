# Challenges

## User Object
```

{
"user_id":<memberid>,
"first_name":string,
"last_name":string,
"yoe": number,
"company": string,,
"designation": string,
"img": string,
"github_id": string,
"linkedin_id": string,
"twitter_id": string,
"instagram_id": string,
"is_member":1
}
```

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

> body will be in JSON format

## Response

```
{
    id:<challenge_id>
}
```

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
    is_user_subscribed:1
}
```
