# APIs — Application Programming Interface

This document and the APIs herein are subject to change at any time. We will version the API, but may deprecate early versions aggressively.

## Authentication

All `/v1` APIs require HTTPS requests and HTTP-Auth.

## Keep in Mind

- Please wait **at least one second** between HTTP queries, or you are likely to get automatically throttled. If you are releasing a library to access the API, you **MUST** do this.
- Please watch for 500 or 999 errors and back-off appropriately. It means that you have been throttled.
- Please set your User-Agent to something identifiable. The default identifiers like `Java/1.4.3` or `lwp-perl` etc tend to get banned from time to time.
- If you are releasing software or a service for other people to use, your software or service MUST NOT add any links without a user’s explicit direction. Likewise, you **MUST NOT** modify any urls except under the user’s explicit direction.

# Methods

## Last Update

## `/v1/posts/update` — Check to see when a user last posted an item

Returns the last updated time for the user, as well as the number of new items in the user’s inbox since it was last visited.

Use this before calling posts/all to see if the data has changed since the last fetch.

### Example Response

```xml
<update time="2005-03-28T17:25:52Z" inboxnew="0" />
```

## Posts

## `/v1/posts/add?` — add a new bookmark

Add a post to Delicious.

### Arguments

- `&url={URL}` (required) — The url of the item.
- `&description={...}` (required) — The description of the item.
- `&extended={...}` (optional) — Botes for the item.
- `&tags={...}` (optional) — Tags for the item (comma delimited).
- `&dt={CCYY-MM-DDThh:mm:ssZ}` (optional) — Datestamp of the item (format “CCYY-MM-DDThh:mm:ssZ”). Requires a LITERAL “T” and “Z” like in ISO8601 at http://www.cl.cam.ac.uk/~mgk25/iso-time.html for Example: `1984-09-01T14:21:31Z`
- `&replace=no` (optional) — Don’t replace post if given url has already been posted.
- `&shared=no` (optional) — Make the item private

### Example Response

If the post was successful:

```xml
<result code="done" />
```

If the post failed:

```xml
<result code="something went wrong" />
```

## `/v1/posts/delete?` — delete an existing bookmark

Delete a post from Delicious.

### Arguments

- `&url={URL}` (required) — The URL of the item.

### Example Response

```xml
<result code="done" />
```

## `/v1/posts/get?` — get bookmark for a single date, or fetch specific items

Returns one or more posts on a single day matching the Arguments. If no date
or url is given, most recent date will be used.

### Arguments

- `&tag={TAG}+{TAG}+...+{TAG}` (optional) — Filter by this tag.
- `&dt={CCYY-MM-DDThh:mm:ssZ}` (optional) — Filter by this date, defaults to the most recent date on which bookmarks were saved.
- `&url={URL}` (optional) — Fetch a bookmark for this URL, regardless of date.  Note: Be sure to URL-encode the argument value.
- `&hashes={MD5}+{MD5}+...+{MD5}` (optional) — Fetch multiple bookmarks by one or more URL MD5s regardless of date, separated by URL-encoded spaces (ie. `‘+’`).
- `&meta=yes` (optional) — Include change detection signatures on each item in a ‘meta’ attribute. Clients wishing to maintain a synchronized local store of bookmarks should retain the value of this attribute — its value will change when any significant field of the bookmark changes.
- `&tag_separator=comma` (optional) - Use commas instead of spaces to separate tags (recommended for multi-word tag disambiguation.)

### Example

```shell
$ curl https://user:passwd@api.delicious.com/v1/posts/get?tag=webdev&meta=yes
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<posts dt="2005-11-28" tag="webdev" user="user">
<post href="http://www.howtocreate.co.uk/tutorials/texterise.php?dom=1"
    description="JavaScript DOM reference"
    extended="dom reference"
    hash="c0238dc0c44f07daedd9a1fd9bbdeebd"
    meta="92959a96fd69146c5fe7cbde6e5720f2"
    others="55" tag="dom javascript webdev" time="2005-11-28T05:26:09Z" />
</posts>
```

```shell
$ curl https://user:passwd@api.delicious.com/v1/posts/get?url=http%3A%2F%2Fwww.yahoo.com%2F
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<posts user="user" dt="2007-12-11" tag="">
<post href="http://www.yahoo.com/"
    hash="2f9704c729e7ed3b41647b7d0ad649fe"
    description="Yahoo!"
    extended="My favorite site ever"
    tag="yahoo web search" time="2007-12-11T00:00:07Z" others="433" />
</posts>
```

## `/v1/posts/recent?` — fetch recent bookmarks

Returns a list of the most recent posts, filtered by argument. Maximum 100.

### Arguments

- `&tag={TAG}` (optional) — Filter by this tag.
- `&count={1..100}` (optional) — Number of items to retrieve (Default:15, Maximum:100).

### Example Response

```shell
$ curl https://user:passwd@api.delicious.com/v1/posts/recent
```

```xml
<posts tag="" user="user">
<post href="http://www.weather.com/" description="weather.com"
hash="6cfedbe75f413c56b6ce79e6fa102aba" tag="weather reference"
time="2005-11-29T20:30:47Z" />
  ...
<post href="http://www.nytimes.com/"
description="The New York Times — Breaking News, World News & Multimedia"
extended="requires login" hash="ca1e6357399774951eed4628d69eb84b"
tag="news media" time="2005-11-29T20:30:05Z" />
</posts>
```

## `/v1/posts/dates?` — list dates on which bookmarks were posted

Returns a list of dates with the number of posts at each date.

### Arguments

- `&tag={TAG}` (optional) — Filter by this tag

### Example

```xml
<dates tag="" user="user">
<date count="5" date="2005-11-29" />
<date count="15" date="2005-11-28" />
<date count="2" date="2005-11-26" />
<date count="2" date="2005-11-25" />
<date count="7" date="2005-11-23" />
<date count="20" date="2005-11-22" />
<date count="16" date="2005-11-21" />
<date count="4" date="2005-11-19" />
</dates>
```

## `/v1/posts/all?` — fetch all bookmarks by date or index range

Returns all posts. Please use sparingly. Call the update function to see if
you need to fetch this at all.

### Arguments

- `&tag={TAG}` (optional) — Filter by this tag.
- `&start={xx}` (optional) — Start returning posts this many results into the set.
- ` &results={xx}` (optional) — Return this many results.
- `&fromdt={CCYY-MM-DDThh:mm:ssZ}` (optional) — Filter for posts on this date or later
- `&todt={CCYY-MM-DDThh:mm:ssZ}` (optional) — Filter for posts on this date or earlier
- `&meta=yes` (optional) — Include change detection signatures on each item in a ‘meta’ attribute. Clients wishing to maintain a synchronized local store of bookmarks should retain the value of this attribute - its value will change when any significant field of the bookmark changes.

### Example

```shell
$ curl https://user:passwd@api.delicious.com/v1/posts/all
```

```xml
<posts tag="" user="user">
<post href="http://www.weather.com/" description="weather.com"
hash="6cfedbe75f413c56b6ce79e6fa102aba" tag="weather reference"
time="2005-11-29T20:30:47Z" />
...
<post href="http://www.nytimes.com/"
description="The New York Times - Breaking News, World News & Multimedia"
extended="requires login" hash="ca1e6357399774951eed4628d69eb84b"
tag="news media" time="2005-11-29T20:30:05Z" />
</posts>
```

## `/v1/posts/all?hashes` — fetch a change detection manifest of all items

Returns a change manifest of all posts. Call the update function to see if you need to fetch this at all.

This method is intended to provide information on changed bookmarks without the necessity of a complete download of all post data.

Each post element returned offers a `url` attribute containing an URL MD5, with an associated `meta` attribute containing the current change detection signature for that bookmark.

### Example

```shell
$ curl https://user:passwd@api.delicious.com/v1/posts/all?hashes
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<posts>
  <post meta="d4b6adc08932f237ec1bfe8021629f4b" url="b14d529834a601567b14f76192e39e96"/>
  <post meta="77e1ec24a43bae61fb67586649683d30" url="2f9704c729e7ed3b41647b7d0ad649fe"/>
  <post meta="9bbc254e930cd5f719c35d97a5180852" url="96bdf631592930d5f540bb962dced0f2"/>
  <post meta="72d92edd9588c3aab5ea8a56d2d79328" url="c5f7ac7a3ec5f014723738c15891a896"/>
  ...
  <post meta="84aea258c8835a2adf226d0d4d439fb7" url="7e2c9d4c49569409d9b455cdf8189a77"/>
</posts>
```

## `/v1/posts/suggest` — fetch popular, recommended and network tags for a specific url

Returns a list of popular tags, recommended tags and network tags for a user.

This method is intended to provide suggestions for tagging a particular url.

### Arguments

- `&url={URL}` (required) — URL for which you’d like suggestions

### Example

```xml
$ curl https://user:passwd@api.delicious.com/v1/posts/suggest?url=http://yahoo.com
```

```xml
<?xml version=\"1.0\" encoding=\"UTF-8\"?>
<suggest>
<popular>yahoo!</popular>
<popular>yahoo</popular>
<popular>web</popular>
<popular>tools</popular>
<popular>searchengines</popular>
<recommended>yahoo!</recommended>
<recommended>yahoo</recommended>
<recommended>web</recommended>
<recommended>tools</recommended>
<recommended>search</recommended>
<recommended>reference</recommended>
<recommended>portal</recommended>
<recommended>news</recommended>
<recommended>music</recommended>
<recommended>internet</recommended>
<recommended>home</recommended>
<recommended>games</recommended>
<recommended>entertainment</recommended>
<recommended>email</recommended>
<network>for:Bernard</network>
<network>for:britta</network>
<network>for:deusx</network>
<network>for:joshua</network>
<network>for:stlhood</network>
<network>for:theteam</network>
</suggest>
```

## Tags

## `/v1/tags/get` — fetch all tags

Returns a list of tags and number of times used by a user.

### Example Response

```xml
<tags>
<tag count="1" tag="activedesktop" />
<tag count="1" tag="business" />
<tag count="3" tag="radio" />
<tag count="5" tag="xml" />
<tag count="1" tag="xp" />
<tag count="1" tag="xpi" />
</tags>
```

## `/v1/tags/delete?` — delete a tag from all posts

Delete an existing tag.

### Arguments

- `&tag={TAG}` (required) — Tag to delete

### Example Response

```xml
<result>done</result>
```

## `/v1/tags/rename?` — rename a tag on all posts

Rename an existing tag with a new tag name.

### Arguments

- `&old={TAG}` (required) — Tag to rename.
- `&new={TAG}` (required) — New tag name.

### Example Response

```xml
<result>done</result>
```

## Tag Bundles

## `/v1/tags/bundles/all?` — fetch tag bundles

Retrieve all of a user’s bundles.

### Arguments

- `&bundle={NAME}` (optional) — Fetch just the named bundle.

### Example response

```xml
<bundles>
<bundle name="music" tags="ipod mp3 music" />
</bundles>
```

## `/v1/tags/bundles/set?` — assign a set of tags to a bundle

Assign a set of tags to a single bundle, wipes away previous settings for
bundle.

### Arguments


- `&bundle={NAME}` (required) — Name of the bundle
- `&tags={TAG}+{TAG}+...+{TAG}` (required) — List of tags, comma-separated.

### Example response

If the bundle was created:

```xml
<result>ok</result>
```

If the bundle was not created:

```xml
<result>you must supply a bundle name and at least one tag</result>
```

## `/v1/tags/bundles/delete?` - delete a tag bundle

Delete a bundle.

### Arguments

- `&bundle={NAME}` (required) — Name of the bundle

### Example response

```xml
<result>done</result>
```
