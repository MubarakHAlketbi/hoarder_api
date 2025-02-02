# Bookmark Search Endpoint

## Overview
This endpoint provides search functionality for bookmarks, implementing cursor-based pagination for efficient result retrieval. It allows users to search through their bookmarks using a text query.

## Technical Details

### Endpoint Information
- **URL**: `/api/v1/bookmarks/search`
- **Method**: GET
- **Authentication**: Required

### Query Parameters
- `q` (required): Search query string
- `limit` (optional): Number of results to return per page
- `cursor` (optional): Pagination cursor for fetching next set of results
  - Version 1 cursor format: Simple numeric offset
  - Transformed internally to `{ ver: 1, offset: number }`

### Response Format
```typescript
{
  bookmarks: Bookmark[];
  nextCursor: string | null;
}
```

### Implementation
```typescript
import { NextRequest } from "next/server";
import { z } from "zod";
import { buildHandler } from "../../utils/handler";

export const dynamic = "force-dynamic";

export const GET = (req: NextRequest) =>
  buildHandler({
    req,
    searchParamsSchema: z.object({
      q: z.string(),
      limit: z.coerce.number().optional(),
      cursor: z
        .string()
        .pipe(z.coerce.number())
        .transform((val) => {
          return { ver: 1 as const, offset: val };
        })
        .optional(),
    }),
    handler: async ({ api, searchParams }) => {
      const bookmarks = await api.bookmarks.searchBookmarks({
        text: searchParams.q,
        cursor: searchParams.cursor,
        limit: searchParams.limit,
      });
      return {
        status: 200,
        resp: {
          bookmarks: bookmarks.bookmarks,
          nextCursor: bookmarks.nextCursor
            ? `${bookmarks.nextCursor.offset}`
            : null,
        },
      };
    },
  });
```

### Code Breakdown
- Uses Next.js server components with route handlers
- Implements request validation using Zod schemas
- Uses cursor-based pagination for efficient result fetching
- Transforms string cursor to internal format with version control
- Returns null nextCursor when no more results are available

## Usage Examples

### Basic Search
```bash
# Search for bookmarks containing "javascript"
curl "http://localhost:3000/api/v1/bookmarks/search?q=javascript&limit=10"
```

### Paginated Search
```bash
# First page
curl "http://localhost:3000/api/v1/bookmarks/search?q=javascript&limit=10"

# Next page (using cursor from previous response)
curl "http://localhost:3000/api/v1/bookmarks/search?q=javascript&limit=10&cursor=10"
```

## Notes
- The endpoint uses cursor-based pagination instead of offset-based for better performance
- Search results are always fresh due to `force-dynamic` export
- The cursor implementation is versioned (V1) to allow for future pagination improvements
- The cursor is transformed from a simple string to a structured object internally
- Response includes null nextCursor when there are no more results to fetch