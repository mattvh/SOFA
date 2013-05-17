Standardized Open Feed API (SOFA)
==========================

### Draft 1

Why can't I take my preferred RSS client (e.g. Reeder, NetNewsWire) and pair it up with the backend of my choice, such as NewsBlur, FeedBin, Feedly, or a custom server I host myself? In the wake of the Google Reader shutdown, there are more viable options than ever for web-based RSS reading and syncing. But native clients only support so many of them.

The way I see it, I should be able to choose one of each, a backend for parsing and syncing subscriptions, and a client for each platform I may use.

That's the goal of this manifesto: to create a specification for an open standard for synchronizing feeds, a simple API that lets a client request and update subscriptions and items from a remote server. The end result should be as architecture-agnostic as possible, preferring a universal and sensible interface to one based on a certain implementation's internal workings.

If you have input, open an Issue.


Base URL
------------

Requests should be served from a common URL along the lines of `http://mydomain.tld/api` or `https://sofa.somefeedservice.tld`. This is the URL that users will enter into their client, along with their credentials. (Ideally this should involve the user entering the API URL, their username and their password into the app's settings.)

Either HTTP or HTTPS is acceptable, though the latter is of course preferred.

The URL structure of a request should look something like this:

    http://mydomain.tld/api/1/subscriptions.json
    
The "1" is the API version, which would change in whole or decimal increments as incompatible changes are made to future versions of the spec. The API endpoints follow the version.


Authentication
------------------

Users should be authenticated with basic HTTP authentication, the client supplying a username and password (or alternately a private API key) with each request.

Example:

    curl -u "username:password" -d "somevar=something" \
    http://mydomain.tld/api/1/subscriptions.json

    
Dates
-------

Any dates returned by the API should be in the form of a [Unix timestamp](http://en.wikipedia.org/wiki/Unix_timestamp), with a second "human readable time" field providing the date in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format. A timezone should not be specified in the ISO 8601 field, as UTC should be assumed.

Example:

    "pubdate": 1367708878
    "pubdate_human": "2013-05-04T18:07:58"


Results Format
------------------

Requests should return data formatted as JSON. Conceivably,  XML result sets could be returned as well, if the request has a `.xml` extension rather than `.json`. However, XML usage has been on the decline for some time (and there are many criticisms of the format). As such, implementing XML responses is entirely optional, and the rest of this document will focus exclusively on JSON.


API Objects
--------------

* [Subscriptions](objects/subscriptions.md)
* [Feed Items](objects/items.md)


Todo
------

* Add some sort of folder/tag taxonomy so a user's feed categorization can be properly described.