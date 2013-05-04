Subscriptions
==========


`GET /subscriptions.json`
-----------------------------

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
    
An empty array should be returned if the user has not subscribed to any feeds.

**Parameters**

None

**Status Codes**

* `200 OK` — Success