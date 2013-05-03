Standardized Open Feed API (SOFA)
====

### Draft 1.0

Why can't I take my preferred RSS client (e.g. Reeder, NetNewsWire) and pair it up with the backend of my choice, such as NewsBlur, FeedBin, Feedly, or a custom server I host myself? In the wake of the Google Reader shutdown, there are more viable options than ever for web-based RSS reading and syncing. But native clients only support so many of them.

The way I see it, I should be able to choose one of each, a backend for parsing and syncing subscriptions, and a client for each platform I may use.

That's the goal of this manifesto: to create a specification for an open standard for synchronizing feeds, a simple API that lets a client request and update subscriptions and items from a remote server.