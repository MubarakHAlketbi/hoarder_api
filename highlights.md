# Hoarder API Highlights Endpoints

This document describes the API endpoints for managing highlights in the Hoarder API, located in the `code/v1/highlights` directory.

## `/v1/highlights`

### `GET /v1/highlights`

Retrieves a list of highlights.

**Request Parameters:**

-   `limit` (optional): A number representing the maximum number of highlights to return.
-   `cursor` (optional): A string representing the cursor for pagination.

**Response:**

-   `status`: 200
-   `resp`: An object containing an array of highlights and a `nextCursor` for pagination.

**Example:**

```
GET /v1/highlights?limit=10
```

```json
{
  "highlights": [
    {
      "highlightId": "h1",
      "bookmarkId": "1",
      "text": "This is a highlight",
      "createdAt": "2023-10-27T14:00:00Z"
    }
  ],
  "nextCursor": "h2_2023-10-27T13:00:00Z"
}
```

### `POST /v1/highlights`

Creates a new highlight.

**Request Body:**

Conforms to the `zNewHighlightSchema` schema.

**Response:**

-   `status`: 201
-   `resp`: The created highlight object.

**Example:**

```
POST /v1/highlights
```

```json
{
  "bookmarkId": "1",
  "text": "This is a new highlight"
}
```

```json
{
  "highlightId": "h2",
  "bookmarkId": "1",
  "text": "This is a new highlight",
  "createdAt": "2023-10-27T15:00:00Z"
}
```

## `/v1/highlights/[highlightId]`

### `GET /v1/highlights/[highlightId]`

Retrieves a specific highlight by its ID.

**Parameters:**

-   `highlightId`: The ID of the highlight.

**Response:**

-   `status`: 200
-   `resp`: The highlight object.

**Example:**

```
GET /v1/highlights/h1
```

```json
{
  "highlightId": "h1",
  "bookmarkId": "1",
  "text": "This is a highlight",
  "createdAt": "2023-10-27T14:00:00Z"
}
```

### `PATCH /v1/highlights/[highlightId]`

Updates a specific highlight.

**Parameters:**

-   `highlightId`: The ID of the highlight.

**Request Body:**

Conforms to the `zUpdateHighlightSchema` schema, excluding the `highlightId` field.

**Response:**

-   `status`: 200
-   `resp`: The updated highlight object.

**Example:**

```
PATCH /v1/highlights/h1
```

```json
{
  "text": "This is an updated highlight"
}
```

```json
{
  "highlightId": "h1",
  "bookmarkId": "1",
  "text": "This is an updated highlight",
  "createdAt": "2023-10-27T14:00:00Z"
}
```

### `DELETE /v1/highlights/[highlightId]`

Deletes a specific highlight.

**Parameters:**

-   `highlightId`: The ID of the highlight.

**Response:**

-   `status`: 200
-   `resp`: The deleted highlight object.

**Example:**

```
DELETE /v1/highlights/h1
```

```json
{
  "highlightId": "h1",
  "bookmarkId": "1",
  "text": "This is an updated highlight",
  "createdAt": "2023-10-27T14:00:00Z"
}