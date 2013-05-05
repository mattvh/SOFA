Feed Items
========


`GET /items.json`
----------------------

Returns subscription entries for a user. Defaults to *all* from the user's subscriptions, be they read or unread, unless limited by GET parameters.

**Response**

    [
        {
            "id": 3571,
            "feed_id": 36,
            "title": "Terry Pratchett Announces New Discworld Novel \201cRaising Steam\201d",
            "url": "http:\/\/www.fantasyfolder.com\/news\/terry-pratchett-announced-new-discworld-novel-raising-steam\/",
            "pubdate:  1362854961,
            "pubdate_human": "2013-03-09T12:49:21",
            "read": true,
            "bookmarked": false,
            "category": [
                "News"
            ]
            "comments_url": "http:\/\/www.fantasyfolder.com\/news\/terry-pratchett-announced-new-discworld-novel-raising-steam\/#comments",
            "guid": "http:\/\/www.fantasyfolder.com\/?p=942",
            "content": "[...]",
            "summary": "[...]",
            "enclosures": [
                {
                    "url": "http://cdn.somedomain.tld/audio/somefile.mp3",
                    length: 24986239,
                    type: "audio/mpeg"
                }
            ]
        }
    ]

* If a field is not found in a feed item, or the server application does not support it (e.g. enclosures), the field should contain a null value. Optimally, all fields should be supported by the server in order to maintain the maximum functionality for client applications.
* The `enclosures` field should contain an array of any enclosures found in the feed. RSS 2.0 feeds commonly have multiple enclosures, despite the fact that the authors did not intend for more than one to be used per item. The RSS 2.0 spec is ambiguous as a result, but common usage dictates support for multiple enclosures.
* `content` should contain the full article content made available by the source feed, while `summary` would have a brief extract, such as the first few sentences. (Some feeds contain both.)
* Endpoint returns an empty array if there are no items matching the specified criteria.

**Parameters**

* `feed_id: integer` — Show results from a specific subscription, using its ID.
* `page: integer` — Results from all requests to `GET /items.json` should be limited to ~100 items per page, with the `page` parameter allowing pagination. Otherwise you could end up with absurdly huge requests...
* `read: boolean` — Only show items that are unread (false) or unread (true).
* `bookmarked: boolean` — If you have a "starring" or "saving" system, limit the results to items that have been marked.
* `since: [Unix or ISO 8601 time]` — Return items since this date.

**Status Codes**

* `200 OK` — Success

**Notes for Discussion**

As it stands, items belonging to a subscription are queried using the `feed_id` parameter. Is that the best way to handle it, or should it work more like this:

    GET /subscriptions/42/entries.json
    
On one hand, it feels more "REST-y," on the other hand, the current seems semantically okay as well.


`GET /items/3571.json`
----------------------------

Returns a single item by its unique ID.

**Response**

    {
        "id": 3571,
        "feed_id": 36,
        "title": "Terry Pratchett Announces New Discworld Novel \201cRaising Steam\201d",
        "url": "http:\/\/www.fantasyfolder.com\/news\/terry-pratchett-announced-new-discworld-novel-raising-steam\/",
        "pubdate:  1362854961,
        "pubdate_human": "2013-03-09T12:49:21",
        "read": true,
         "bookmarked": false,
        "category": [
            "News"
        ]
        "comments_url": "http:\/\/www.fantasyfolder.com\/news\/terry-pratchett-announced-new-discworld-novel-raising-steam\/#comments",
        "guid": "http:\/\/www.fantasyfolder.com\/?p=942",
        "content": "[...]",
        "summary": "[...]",
        "enclosures": [
            {
                "url": "http://cdn.somedomain.tld/audio/somefile.mp3",
                length: 24986239,
                type: "audio/mpeg"
            }
        ]
    }

**Parameters**

None

**Status Codes**

* `200 OK` — Success
* `404 Not Found` — No item was found with this ID.
* `403 Not Found` — The user is not subscribed to the feed this item belongs to.


`POST /items/3571/mark_as_read.json`
-----------------------------------------------

Sets the item's `read` field to `true` or `false`, according to the supplied parameter.

**Response**

None

**Parameters**

* `read: boolean` — Set the item's read/unread flag.

**Status Codes**

* `200 OK` — Success
* `404 Not Found` — No item was found with this ID.
* `403 Not Found` — The user is not subscribed to the feed this item belongs to.


`POST /items/3571/bookmark.json`
-----------------------------------------------

Sets the item's `bookmarked` field to `true` or `false`, according to the supplied parameter.

**Response**

None

**Parameters**

* `bookmark: boolean` — Set the item's bookmark flag.

**Status Codes**

* `200 OK` — Success
* `404 Not Found` — No item was found with this ID.
* `403 Not Found` — The user is not subscribed to the feed this item belongs to.