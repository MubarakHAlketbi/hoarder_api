# Pagination Utility

## Overview
The pagination utility provides standardized cursor-based pagination functionality for the Hoarder API. It implements a cursor format that combines ID and timestamp for reliable pagination ordering.

## Technical Details

### Pagination Schema
```typescript
const zPagination = z.object({
  limit: z.coerce.number().max(MAX_NUM_BOOKMARKS_PER_PAGE).optional(),
  cursor: z
    .string()
    .refine((val) => val.includes("_"), "Must be a valid cursor")
    .transform((val) => {
      const [id, createdAt] = val.split("_");
      return { id, createdAt };
    })
    .pipe(z.object({ id: z.string(), createdAt: z.coerce.date() }))
    .optional(),
});
```

### Key Features

#### 1. Cursor Format
- Format: `{id}_{timestamp}`
- Example: `123_2024-02-02T18:30:00.000Z`
- Components:
  - `id`: Resource identifier
  - `timestamp`: ISO formatted creation date

#### 2. Pagination Parameters
- `limit`: Optional number of items per page
  - Maximum value enforced by `MAX_NUM_BOOKMARKS_PER_PAGE`
  - Coerced to number from string input
- `cursor`: Optional pagination cursor
  - String format: `id_timestamp`
  - Transformed into object with `id` and `createdAt`

#### 3. Response Adaptation
The `adaptPagination` function transforms internal cursor format to client-friendly string:

```typescript
function adaptPagination<T extends { nextCursor: CursorV2 | null }>(input: T) {
  const { nextCursor, ...rest } = input;
  if (!nextCursor) {
    return input;
  }
  return {
    ...rest,
    nextCursor: `${nextCursor.id}_${nextCursor.createdAt.toISOString()}`,
  };
}
```

## Usage Example

### Request
```typescript
// GET /api/v1/bookmarks?limit=10&cursor=123_2024-02-02T18:30:00.000Z
export const GET = (req: NextRequest) =>
  buildHandler({
    req,
    searchParamsSchema: zPagination,
    handler: async ({ searchParams }) => {
      // searchParams.cursor is automatically transformed to:
      // { id: "123", createdAt: Date("2024-02-02T18:30:00.000Z") }
      const result = await getItems(searchParams);
      return { status: 200, resp: adaptPagination(result) };
    },
  });
```

### Response
```json
{
  "items": [...],
  "nextCursor": "124_2024-02-02T18:31:00.000Z"
}
```

## Implementation Notes

1. Cursor Validation
   - Ensures cursor contains both ID and timestamp
   - Validates timestamp is a valid date
   - Transforms string input into structured object

2. Pagination Flow
   - Client sends cursor string in query params
   - Server validates and transforms cursor
   - Query uses cursor for efficient pagination
   - Response adapted back to string format

3. Type Safety
   - Uses Zod for runtime validation
   - Maintains type safety through generics
   - Integrates with shared type definitions

4. Best Practices
   - Cursor-based pagination for consistency
   - ISO timestamp format for universal compatibility
   - Automatic type coercion for better DX
   - Maximum limit enforcement for performance

## Notes
- Always use `adaptPagination` when returning paginated responses
- Cursor format ensures stable ordering even with data changes
- Integrates with `@hoarder/shared/types/pagination` for consistency
- Maximum items per page is defined in shared constants