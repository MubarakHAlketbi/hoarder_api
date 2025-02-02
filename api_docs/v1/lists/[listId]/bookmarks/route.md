# List Bookmarks Endpoint

## Overview
This endpoint retrieves bookmarks associated with a specific list, implementing pagination for efficient data retrieval.

## Technical Details

### Endpoint Information
- **URL**: `/api/v1/lists/:listId/bookmarks`
- **Method**: GET
- **Authentication**: Required
- **Dynamic**: Yes (force-dynamic enabled)

### Query Parameters
Uses `zPagination` schema for pagination parameters:
- `limit` (optional): Number of items per page
- `cursor` (optional): Cursor for pagination

## Operation

### Get List Bookmarks (GET)
Retrieves a paginated list of bookmarks associated with a specific list.

#### Parameters
- `listId` (path parameter): The unique identifier of the list
- Pagination parameters in query string

#### Response
- **Status**: 200 OK
- **Body**: Paginated bookmarks response
```typescript
{
  items: {
    id: string;
    // Other bookmark properties
  }[];
  meta: {
    nextCursor: string | null;
    // Other pagination metadata
  };
}
```

### Implementation
```typescript
import { NextRequest } from "next/server";
import { buildHandler } from "@/app/api/v1/utils/handler";
import { adaptPagination, zPagination } from "@/app/api/v1/utils/pagination";

export const dynamic = "force-dynamic";

export const GET = (req: NextRequest, params: { params: { listId: string } }) =>
  buildHandler({
    req,
    searchParamsSchema: zPagination,
    handler: async ({ api, searchParams }) => {
      const bookmarks = await api.bookmarks.getBookmarks({
        listId: params.params.listId,
        ...searchParams,
      });
      return { status: 200, resp: adaptPagination(bookmarks) };
    },
  });
```

## Usage Examples

### Get List Bookmarks (First Page)
```bash
curl "http://localhost:3000/api/v1/lists/123/bookmarks?limit=10" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

### Get List Bookmarks (With Cursor)
```bash
curl "http://localhost:3000/api/v1/lists/123/bookmarks?limit=10&cursor=next_page_cursor" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

## Notes
- Uses the common buildHandler utility for consistent error handling
- Implements cursor-based pagination through zPagination schema
- Adapts response format using adaptPagination utility
- Force-dynamic ensures fresh data on each request
- Bookmarks are filtered by list ID
- Response includes pagination metadata for navigating through results
- Cursor format follows the standard pagination implementation