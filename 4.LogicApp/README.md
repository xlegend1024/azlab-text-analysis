# Real-time scoring

Create logic to get real-time prediction

## Flow overview

![](../images/4.0.png)

It has three parts

### Recurrence

![](../images/4.1.png)

### Search Tweets

![](../images/4.2.png)

### For each

Select Twitter body

#### For each > Condition

Select TweetLanguageCode and type ```en```

![](../images/4.3.png)

#### For each > Condition > Ture

#### For each > Condition > Ture > Parse JSON

|parameter name|value|
|---|---|
|content|@{items('For_each')}|

Copy following json schema

```json
{
    "properties": {
        "CreatedAt": {
            "type": "string"
        },
        "CreatedAtIso": {
            "type": "string"
        },
        "Favorited": {
            "type": "boolean"
        },
        "MediaUrls": {
            "type": "array"
        },
        "OriginalTweet": {},
        "RetweetCount": {
            "type": "integer"
        },
        "TweetId": {
            "type": "string"
        },
        "TweetInReplyToUserId": {
            "type": "string"
        },
        "TweetLanguageCode": {
            "type": "string"
        },
        "TweetText": {
            "type": "string"
        },
        "TweetedBy": {
            "type": "string"
        },
        "UserDetails": {
            "properties": {
                "Description": {
                    "type": "string"
                },
                "FavouritesCount": {
                    "type": "integer"
                },
                "FollowersCount": {
                    "type": "integer"
                },
                "FriendsCount": {
                    "type": "integer"
                },
                "FullName": {
                    "type": "string"
                },
                "Id": {
                    "type": "integer"
                },
                "Location": {
                    "type": "string"
                },
                "ProfileImageUrl": {
                    "type": "string"
                },
                "StatusesCount": {
                    "type": "integer"
                },
                "UserName": {
                    "type": "string"
                }
            },
            "type": "object"
        },
        "UserMentions": {
            "items": {
                "properties": {
                    "FullName": {
                        "type": "string"
                    },
                    "Id": {
                        "type": "integer"
                    },
                    "UserName": {
                        "type": "string"
                    }
                },
                "required": [
                    "Id",
                    "FullName",
                    "UserName"
                ],
                "type": "object"
            },
            "type": "array"
        }
    },
    "type": "object"
}
```

![](../images/4.4.png)

#### For each > Condition > Ture > Compose

Copy next json text to _Input_ parameter

```json
[
  {
    "id": "@{body('Parse_JSON')?['TweetId']}",
    "text": "@{replace(string(body('Parse_JSON')?['TweetText']), '\"', '')}"
  }
]
```

![](../images/4.5.png)

#### For each > Condition > Ture > HTTP

|parameter name|value|
|---|---|
|Method|POST|
|URI|Copy from Azure Databricks Notebook 3|
|Headers|Authorization|
|Headers|Content-Type|
|Headers-Authorization|Bearer ******|
|Headers-Content-Type|application/json|
|Body|@{outputs('Compose')}|

![](../images/4.6.png)

#### For each > Condition > Ture > Parse JSON 2

|parameter name|value|
|---|---|
|content|json(body('HTTP'))|

```json
{
    "items": {
        "type": "string"
    },
    "type": "array"
}
```

![](../images/4.7.png)

#### For each > Condition > Ture >  _For each 2_

```json
@body('Parse_JSON_2')
```

![](../images/4.8.png)

#### For each > Condition > Ture >  For each 2 > _Parse JSON 3_

|parameter name|value|
|---|---|
|content|@{items('For_each')}|

Copy and pate following JSON schema to logic app

```json
{
    "properties": {
        "id": {
            "type": "string"
        },
        "prediction": {
            "maximum": 1,
            "minimum": 0,
            "type": "number"
        },
        "text": {
            "type": "string"
        }
    },
    "type": "object"
}
```

![](../images/4.9.png)

#### For each > Condition > Ture >  For each 2 > _Compose 2_

```json
{
  "body": @{items('For_each')},
  "id": @{guid()},
  "prediction": @{float(body('Parse_JSON_3')?['prediction'])},
  "tweetid": "@{body('Parse_JSON_3')?['id']}"
}
```

![](../images/4.10.png)

#### For each > Condition > Ture >  For each 2 > _Parse JSON 4_


```json
{
    "properties": {
        "body": {
            "properties": {},
            "type": "object"
        },
        "id": {
            "type": "string"
        },
        "prediction": {
            "type": "number"
        },
        "tweetid": {
            "type": "string"
        }
    },
    "type": "object"
}
```

#### For each > Condition > Ture >  For each 2 > _Create or Update document_

Add Parition Key value parameter

|parameter name|value|
|---|---|
|Database ID|socialmedia|
|Collection ID|twitter|
|Document|@{outputs('Compose_2')}|
|Parition key value|"@{body('Parse_JSON_4')?['id']}"|

![](../images/4.11.png)