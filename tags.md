# Hoarder API Tags Endpoints

This document describes the API endpoints for managing tags in the Hoarder API, located in the `code/v1/tags` directory.

## `/v1/tags`

### `GET /v1/tags`

Retrieves a list of all tags.

**Response:**

-   `status`: 200
-   `resp`: An array of tag objects.

**Example:**

```
GET /v1/tags
```

```json
[
  {
    "tagId": "t1",
    "name": "tag1",
    "createdAt": "2023-10-27T18:00:00Z"
  }
]
```

## `/v1/tags/[tagId]`

### `GET /v1/tags/[tagId]`

Retrieves a specific tag by its ID.

**Parameters:**

-   `tagId`: The ID of the tag.

**Response:**

-   `status`: 200
-   `resp`: The tag object.

**Example:**

```
GET /v1/tags/t1
```

```json
{
  "tagId": "t1",
  "name": "tag1",
  "createdAt": "2023-10-27T18:00:00Z"
}
```

### `PATCH /v1/tags/[tagId]`

Updates a specific tag.

**Parameters:**

-   `tagId`: The ID of the tag.

**Request Body:**

Conforms to the `zUpdateTagRequestSchema` schema, excluding the `tagId` field.

**Response:**

-   `status`: 200
-   `resp`: The updated tag object.

**Example:**

```
PATCH /v1/tags/t1
```

```json
{
  "name": "updatedTag1"
}
```

```json
{
  "tagId": "t1",
  "name": "updatedTag1",
  "createdAt": "2023-10-27T18:00:00Z"
}
```

### `DELETE /v1/tags/[tagId]`

Deletes a specific tag.

**Parameters:**

-   `tagId`: The ID of the tag.

**Response:**

-   `status`: 204

**Example:**

```
DELETE /v1/tags/t1
```

## `/v1/tags/[tagId]/bookmarks`

### `GET /v1/tags/[tagId]/bookmarks`

Retrieves the bookmarks that are associated with a specific tag.

**Parameters:**

-   `tagId`: The ID of the tag.
-   `limit` (optional): A number representing the maximum number of bookmarks to return.
-   `cursor` (optional): A string representing the cursor for pagination.

**Response:**

-   `status`: 200
-   `resp`: An object containing an array of bookmarks and a `nextCursor` for pagination.

**Example:**

```
GET /v1/tags/t1/bookmarks?limit=10
```

```json
{
  "bookmarks": [
    {
      "bookmarkId": "1",
      "url": "https://example.com",
      "title": "Example Domain",
      "favourited": true,
      "archived": false,
      "createdAt": "2023-10-27T12:00:00Z"
    }
  ],
  "nextCursor": "2_2023-10-27T11:00:00Z"
}