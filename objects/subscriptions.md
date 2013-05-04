Subscriptions
==========


`GET /subscriptions.json`
-------------------------------

Returns all feeds that the user is subscribed to.

**Response**

    [
        {
            "id": 42,
            "title": "Ars Technica",
            "feed_url": "http://feeds.arstechnica.com/arstechnica/index/",
            "site_url": "http://arstechnica.com/",
            "description": "The Art of Technology",
            "last_build_date": 1367708878,
            "last_build_date_human": "2013-05-04T18:07:58",
            "image_url": null
        }
    ]
    
* An empty array should be returned if the user has not subscribed to any feeds.
* "id" is a global representation of the feed, regardless of user. If two users are subscribed to the Ars Technica feed in the above example, the same ID would be returned for both users. (How the server manages feeds and subscriptions internally is of no business to the API.)
* "image_url" is the channel image found in the RSS 2.0 spec. Return null if not set.

**Parameters**

None

**Status Codes**

* `200 OK` — Success


`GET /subscriptions/42.json`
-----------------------------------

Returns the details for an individual feed with an ID of 42.

**Response**

    {
        "id": 42,
        "title": "Ars Technica",
        "feed_url": "http://feeds.arstechnica.com/arstechnica/index/",
        "site_url": "http://arstechnica.com/",
        "description": "The Art of Technology",
        "last_build_date": 1367708878,
        "last_build_date_human": "2013-05-04T18:07:58",
        "image_url": null
    }

**Parameters**

None

**Status Codes**

* `200 OK` — Success
* `404 Not Found` — The user does not have a subscription with this ID.


`POST /subscriptions.json`
---------------------------------

Subscribes the user to the feed specified in the POST data.

**Response**

Subscription object is returned on success or if the user is already subscribed to the feed. See `GET /subscriptions/42.json`.

**Parameters**

* `feed_url` — The feed to subscribe the active user to. e.g. `http://www.theverge.com/rss/index.xml`

**Status Codes**

* `201 Created` — Success
* `302 Found` — The user is already subscribed to this feed.
* `404 Not Found` — The supplied feed URL is bad.


`DELETE /subscriptions/42.json`
----------------------------------------

Unsubscribes the user from the subscription with the ID of 3.

**Response**

None

**Parameters**

None

**Status Codes**

* `410 Gone` — Success
* `404 Not Found` — The user does not have a subscription with this ID.