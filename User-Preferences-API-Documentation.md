# Documentation-User-Preferences-API

## Table of contents
- [Responsibilities](#responsibilities)
- [MongoDB](#mongodb)

## Responsibilities
The User-Preferences-API is responsible for:
- saving/retrieving the layout of the dashboard to and from the database
- saving/retrieving themes/color-schemes to and from the database
- saving/retrieving Dashboard URL shortcuts to and from the database

## saving/retrieving Dashboard URL shortcuts and the Layout to and from the database

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

