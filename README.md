# [<img src="https://delicious.com/img/logo.png" alt="Delicious logo" width="28">](http://delicious.com/) Delicious API Documentation

This document and the APIs herein are subject to change at any time. We will version the API, but may deprecate early versions aggressively.

## Keep in Mind

- Please wait **at least one second** between HTTP queries, or you are likely to get automatically throttled. If you are releasing a library to access the API, you **MUST** do this.
- Please watch for 500 or 999 errors and back-off appropriately. It means that you have been throttled.
- Please set your User-Agent to something identifiable. The default identifiers like `Java/1.4.3` or `lwp-perl` etc tend to get banned from time to time.
- If you are releasing software or a service for other people to use, your software or service MUST NOT add any links without a user’s explicit direction. Likewise, you **MUST NOT** modify any urls except under the user’s explicit direction.

## Authentication

All `/v1` APIs require HTTPS requests and HTTP-Auth.

## Objects

* [Posts](https://github.com/avos/delicious-api/blob/master/api/Posts.md)
* [Tags](https://github.com/avos/delicious-api/blob/master/api/Tags.md)
* [Tag Bundles](https://github.com/avos/delicious-api/blob/master/api/TagBundles.md)

## Feedback

Contact [feedback@delicious.com](mailto:feedback@delicious.com) if you have any question with using our APIs.
