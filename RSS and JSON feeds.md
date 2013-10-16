# RSS and JSON feeds

## What are Delicious Feeds?

For most pages within Delicious, there are associated read-only data feeds for bookmarks and other information displayed in a browser. These feeds come in several formats -- including [RSS](http://en.wikipedia.org/wiki/RSS_(protocol)) and [JSON](http://json.org) -- and offer windows onto what’s going on at Delicious that you can use in news readers, blogs, or your own third-party applications.

## What is RSS?

RSS is a format used by many news sites and blogs to publish content on the web. Using RSS to publish content enables [news readers](http://www.opencommunity.co.uk/vienna2.php) and [personalized start pages](http://my.yahoo.com) to pull new stuff all into one place. Applications that understand RSS can do the footwork and provide a one-stop shop to help readers keep on top of things without needing to surf all over the web in person. 

Accordingly, Delicious offers links to companion RSS feeds on most Profile pages to help you keep track of links from other users. Look for the “rss” link just under a user’s profile picture. 

In addition, many web browsers -- such as [Firefox](http://www.mozilla.org/en-US/firefox/livebookmarks.html) and [Internet Explorer](http://windows.microsoft.com/en-NZ/windows-vista/Using-feeds-RSS) -- automatically detect RSS feeds associated with pages and offer convenient ways to subscribe.

## What is JSON?

JSON, which stands for [JavaScript Object Notation](http://json.org), is a lightweight data-interchange format easily used in browser-based mashups, blog badges, and other scenarios including server-side and desktop applications. You can use feeds at Delicious in JSON format to fetch, remix, and mashup a variety of data for use in your own custom applications and browser-based hacks.

## How fresh are the feeds?

Since feeds at Delicious see quite a bit of high-volume use, it’s not practical to offer immediately fresh results at all times. In particular, RSS feeds may not be updated more than twice an hour, and JSON feeds are likely to show a degree of staleness at times as well. Polling feeds more often will not yield better data, and may result in HTTP 503 Unavailable errors indicating either that your application has been blocked or the servers are otherwise temporarily throttling your requests.

## What API Feeds are available at Delicious?

Feeds at Delicious all share the following base URL prefix:

  - `http://feeds.delicious.com/v2/{format}/`

The placeholder {format} specifies the output format for the feed, which
currently includes the values rss and json.

All feed formats accept the following query-string parameters:

  - `?count={1..100}`: Limit the results to the given number, between 1 and 100 (default 15)

For RSS format feeds, Delicious performs user-agent detection and, for certain
news readers, includes enhanced HTML content in items while minimizing the
amount of machine-readable metadata. This feature can be intentionally
switched on or disabled with the following URL query-string parameters:

  - `?plain`: Disable enhanced item HTML content.
  - `?fancy`: Enable enhanced item HTML content.

To help enable JSON-based mashups, there are a few URL query-string parameters
to tweak the JSON output:

  - `?callback={js call}`: Allows the inclusion of a wrapper function call around the JSON data. The value is filtered by a whitelist consisting of these characters: 0-9 a-z A-Z ()[],._-+=/|\\~?!#$^*: '\"

The following feed URL patterns use these placeholders:

  - `{format}`: replaced with either “rss” or “json”
  - `{username}`: replaced with a user’s login name on delicious
  - `{tag[+tag+...+tag]}`: replaced with a tag or an intersection of tags.
  - `{url md5}`: is intended for the MD5 hash of a URL.
  - `{key}`: a security key for the feed, which can be found via the page associated with the feed (eg. inbox, network or bookmarks). Allows visibilty to otherwise private data.

## Finally, what are the URLs for Delicious RSS Feeds?

### Recent bookmarks

`http://feeds.delicious.com/v2/{format}/recent`

### Recent bookmarks by tag

`http://feeds.delicious.com/v2/{format}/tag/{tag[+tag+...+tag]}`

### Bookmarks for a specific user

`http://feeds.delicious.com/v2/{format}/{username}`

### Private bookmarks for a specific user

`http://feeds.delicious.com/v2/{format}/{username}?private={key}`

### Bookmarks for a specific user by tag(s)

`http://feeds.delicious.com/v2/{format}/{username}/{tag[+tag+...+tag]}`

### Private bookmarks for a specific user by tag(s)

`http://feeds.delicious.com/v2/{format}/{username}/{tag[+tag+...+tag]}?private={key}`

### Public summary information about a user

`http://feeds.delicious.com/v2/{format}/userinfo/{username}`

### A list of all public tags for a user

`http://feeds.delicious.com/v2/{format}/tags/{username}`

### A list of related public tags for a user tag combination

`http://feeds.delicious.com/v2/{format}/tags/{username}/{tag[+tag+...+tag]}`

### Bookmarks from a user’s subscriptions

`http://feeds.delicious.com/v2/{format}/subscriptions/{username}`

### Private feed for a user’s inbox bookmarks from others

`http://feeds.delicious.com/v2/{format}/inbox/{username}?private={key}`

### Bookmarks from members of a user’s network

`http://feeds.delicious.com/v2/{format}/network/{username}`

### Bookmarks from members of a user’s network by tag

`http://feeds.delicious.com/v2/{format}/network/{username}/{tag[+tag+...+tag]}`

### A list of a user’s network members

`http://feeds.delicious.com/v2/{format}/networkmembers/{username}`

### Recent bookmarks for a URL

`http://feeds.delicious.com/v2/{format}/url/{url md5}`

### Summary information about a URL

`http://feeds.delicious.com/v2/json/urlinfo/{url md5}`
