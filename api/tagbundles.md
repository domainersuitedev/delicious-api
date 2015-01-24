# Tag Bundles

Tag Bundle is a collection of tags. Use Tag Bundle to filter links by a set of tags at a time.

* [`/v1/tags/bundles/all?`](#v1tagsbundlesall) — Fetch tag bundles
* [`/v1/tags/bundles/set?`](#v1tagsbundlesset) — Assign a set of tags to a bundle'
* [`/v1/tags/bundles/delete?`](#v1tagsbundlesdelete) - Delete a tag bundle

---

## `/v1/tags/bundles/all?`

Retrieve all of a user’s bundles.

### Arguments

- `&bundle={NAME}` (optional) — Fetch just the named bundle.

### Example response

```xml
<bundles>
<bundle name="music" tags="ipod mp3 music" />
</bundles>
```

## `/v1/tags/bundles/set?`

Assign a set of tags to a single bundle, wipes away previous settings for bundle.

### Arguments

- `&bundle={NAME}` (required) — Name of the bundle.
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

## `/v1/tags/bundles/delete?`

Delete a tag bundle.

### Arguments

- `&bundle={NAME}` (required) — Name of the bundle.

### Example response

```xml
<result>done</result>
```
