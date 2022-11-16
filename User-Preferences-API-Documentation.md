# Documentation-User-Preferences-API

## Table of contents
- [Responsibilities](#responsibilities)
- [MongoDB](#mongodb)
- [User-Preferences-API](#user-preferences-api)
  - [Authentication](#authentication)
  - [Unit Testing](#unit-testing)

## Responsibilities
The User-Preferences-API is responsible for:
- saving/retrieving the layout of the dashboard to and from the database
- saving/retrieving themes/color-schemes to and from the database
- saving/retrieving Dashboard URL shortcuts to and from the database

## MongoDB
> MongoDB is a source-available cross-platform document-oriented database program. Classified as a NoSQL database program, MongoDB uses JSON-like documents with optional schemas. MongoDB is developed by MongoDB Inc. and licensed under the Server Side Public License (SSPL) which is deemed non-free by several distributions. [(Wikipedia MongoDB)](https://en.wikipedia.org/wiki/MongoDB)

We wanted to use a document-based database for the User-Preferences API. MongoDB is widely used, and would be a easier for us to learn since there is a lot of information about it on the internet. 


## User-Preferences-API

Since we are using MongoDB as database, we needed to structure the URL data as a JSON.
Our first ideas looked like this:
```json
{
    "userid": "1234",
    "urls": [
        {
            "url": "https://kanikeenkortebroekaan.nl/",
            "urlid" : "1234"
        },
        {
            "url": "https://kanikeenkortebroekaan.nl/",
            "urlid" : "2345"
        }
    ]
}
```
We successfully managed to use this format in our API, and retrieve the data in the front-end.
However, when we started thinking about how we were going to save the layout, we came to the conclusion that this approach would not really work together with a layout.

So to combine the URLs with the Layout, we came to a new JSON structure:
```json
{
    "userid": "e312eb90-88e3-4d3e-83be-9a59f3ec9c36",
    "columns":
    [
        {
            "cards":
            [
                {
                    "cardId":"i312eb90-88e3-4d3e-83be-9a59f3ec9c36",
                    "cardType":"URL"
                },
                {
                    "cardId": "a312eb90-88e3-4d3e-83be-9a59f3ec9c36",
                    "cardType":"Spotify-Statistics"
                }
            ]
        }
    ]
}
```
Then in the front-end whenever a card is from cardType URL, the front-end can make another request to a new collection in the database where the URLs are stored like this:
```json
{
    "cardId":"id van een mandje/card",
    "urls": 
    [
        {
            "urlId": 123456,
            "url":"https://www.netflix.com/"
        },
        {
            "urlId": 123457,
            "url":"https://minecraft.net/"
        },
        {
            "urlId": 123457,
            "url":"https://minecraft.net/"
        }
    ]
}
```
### Authentication
#### How do we authenticate users in the API?
Authentication is already done in the front-end using OAuth2.0 with Google Login. (More about this [here](https://docs.google.com/document/d/1FcSPYfOpofL5F_100IwEOF1PCGIBsGaKJo6o_Hl-EMo/edit#heading=h.n8jgu3kfz61x))
Because we have already authenticated the user on the front end, we can verify our users in the API with the 'id token' they send when making requests to the API.

Steps to verify:

Check if token is already known to API (API saves verified tokens in memory)
- If the API doesn't recognize the token or it's expired, it will verify it with google, 
  - when successful the token is added to the known tokens, and user id is returned.
  - if not successful it will raise an 401 httpException
- If the API recognizes the token it will return the user id.

These are the functions responsible for the verification. (code may change)
``` python
def is_token_verified(token):
    remove_expired_tokens()
    for verified_token in verified_tokens:
        if verified_token.id_token == token:
            return verified_token
    return None
```
``` python
def verify_token(token): 
    
    token_obj = is_token_verified(token)
    
    if token_obj is None:
        try:
            idinfo = id_token.verify_oauth2_token(token, requests.Request(), CLIENT_ID)
            userid = idinfo['sub']
            token_obj = verified_token(id_token=token, verified_date_time=datetime.now(), user_id=userid)
            verified_tokens.append(token_obj)
            return userid
        except ValueError:
            raise HTTPException(status_code=401, detail="Invalid token")
    else:
        return token_obj.user_id
```

### Unit Testing
To create unit tests on the API we use [Pytest](https://docs.pytest.org/).

To mock our database in the tests we use [mongomock](https://github.com/mongomock/mongomock).
Here is an example on how we mock the database in our tests:
``` python
collection_mock = mongomock.MongoClient().mocklayout_db.mocklayout_collection

def mock_init(mock_user_id):
    layout.layoutdb = collection_mock

    def verify_token_mock(token:str):
        return mock_user_id
    layout.verify_token = verify_token_mock
```
