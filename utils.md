# Hoarder API Utilities

This document describes the utility functions used in the Hoarder API, located in the `code/v1/utils` directory.

## `handler.ts`

This file provides a standardized way to handle API requests and responses, leveraging `zod` for schema validation and `trpc` for error handling.

### `trpcCodeToHttpCode(code: TRPCError["code"])`

Converts TRPC error codes to HTTP status codes.

**Parameters:**

-   `code`: A TRPC error code.

**Returns:**

-   An HTTP status code corresponding to the given TRPC error code.

### `formatZodError(error: ZodError)`

Formats `zod` validation errors into a human-readable string.

**Parameters:**

-   `error`: A `zod` error object.

**Returns:**

-   A string representation of the validation error.

### `interface TrpcAPIRequest<SearchParamsT, BodyType>`

Defines the structure of the input to the handler function.

**Properties:**

-   `ctx`: The request context.
-   `api`: The tRPC client.
-   `searchParams`: The parsed search parameters, validated against the provided schema.
-   `body`: The parsed request body, validated against the provided schema.

### `buildHandler<SearchParamsT extends z.ZodTypeAny | undefined, BodyT extends z.ZodTypeAny | undefined, InputT extends TrpcAPIRequest<SearchParamsT, BodyT>>({ req, handler, searchParamsSchema, bodySchema })`

A generic function that handles parsing the request, validating the input, calling the handler function, and formatting the response or error.

**Parameters:**

-   `req`: The Next.js API request object.
-   `handler`: The function that processes the request and returns a response.
-   `searchParamsSchema`: An optional `zod` schema to validate the search parameters.
-   `bodySchema`: An optional `zod` schema to validate the request body.

**Returns:**

-   A Next.js API response object.

## `pagination.ts`

This file defines a `zod` schema for pagination parameters and a function to adapt the `nextCursor` format.

### `zPagination`

A `zod` schema that validates `limit` and `cursor` parameters.

-   `limit`: A number representing the maximum number of items per page. It has a maximum value defined by `MAX_NUM_BOOKMARKS_PER_PAGE`.
-   `cursor`: A string representing the cursor for pagination. It must contain an underscore and is transformed into an object with `id` and `createdAt` properties.

### `adaptPagination<T extends { nextCursor: z.infer<typeof zCursorV2> | null }>(input: T)`

Adapts the `nextCursor` format from an object with `id` and `createdAt` to a string format by concatenating the `id` and the ISO string representation of `createdAt` with an underscore.

**Parameters:**

-   `input`: An object containing a `nextCursor` property.

**Returns:**

-   The input object with the `nextCursor` property adapted to the string format.

## `types.ts`

This file defines custom `zod` schemas for specific data types.

### `zStringBool`

A `zod` schema that validates a string to be either `"true"` or `"false"` and transforms it into a boolean value.