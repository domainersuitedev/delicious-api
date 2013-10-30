# Tags

Tags are small text created by user to describe a link. It helps people to organized a large amount of links.

* [`/v1/tags/get`](#v1tagsget--fetch-all-tags) — fetch all tags
* [`/v1/tags/delete?`](#v1tagsdelete--delete-a-tag-from-all-posts) — delete a tag from all posts
* [`/v1/tags/rename?`](#v1tagsrename--rename-a-tag-on-all-posts) — rename a tag on all posts

---

## `/v1/tags/get`

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

## `/v1/tags/delete?`

Delete an existing tag from all posts

### Arguments

- `&tag={TAG}` (required) — Tag to delete

### Example Response

```xml
<result>done</result>
```

## `/v1/tags/rename?`

Rename an existing tag with a new tag name.

### Arguments

- `&old={TAG}` (required) — Tag to rename.
- `&new={TAG}` (required) — New tag name.

### Example Response

```xml
<result>done</result>
```

