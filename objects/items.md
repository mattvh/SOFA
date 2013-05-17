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
            "author": "Matt Harzewski",
            "read": true,
            "bookmarked": false,
            "categories": [
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
* Results from all requests to `GET /items.json` should be limited to 100 items per page by default, with the `limit` and `offset` parameters allowing pagination. Otherwise you could end up with absurdly huge requests. See the parameters listing for further details.

**Parameters**

* `feed_id: integer` — Show results from a specific subscription, using its ID.
* `limit: integer` — The number of results to return. Default should be 100, maximum 200.
* `offset: integer` — To access results past `limit`, the `offset` parameter should be used. e.g. the second "page" of results, with the default limit of 100, would be an offset of 100.
* `read: boolean` — Only show items that are unread (false) or unread (true).
* `bookmarked: boolean` — If you have a "starring" or "saving" system, limit the results to items that have been marked.
* `since: [Unix or ISO 8601 time]` — Return items since this date.

**Status Codes**

* `200 OK` — Success

**Pagination and HTTP Headers**

SOFA uses the HTTP Link header to supply clients with pagination hinting. A paginated result set should send the Link header with two URLs: one matching the next set of results and one matching the last. (Both should retain any query variables set by the client.) When accessing the last result set, the "next" link should be omitted.

*(Discuss: While this seems like a good idea in theory, it may not be terribly necessary in real-world usage...and is extra work to implement.)*

    Link: <http://domain.tld/sofa/1/items.json?offset=100&limit=100>; rel='next',
    <http://domain.tld/sofa/1/items.json?offset=900&limit=100>; rel='last'

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
        "author": "Matt Harzewski",
        "read": true,
         "bookmarked": false,
        "categories": [
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
* `403 Forbidden` — The user is not subscribed to the feed this item belongs to.


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
* `403 Forbidden` — The user is not subscribed to the feed this item belongs to.


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
* `403 Forbidden` — The user is not subscribed to the feed this item belongs to.


`GET /items/unread_count.json`
-----------------------------------------

Returns the number of unread items for the user.

**Response**

    {
        "count": 371
    }

**Parameters**

None

**Status Codes**

* `200 OK` — Success