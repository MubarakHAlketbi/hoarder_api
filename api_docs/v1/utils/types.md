# Types Utility

## Overview
The types utility provides shared type definitions and validation schemas used across the Hoarder API. Currently, it includes a specialized Zod schema for handling boolean values passed as strings in URL query parameters.

## Technical Details

### String Boolean Schema
```typescript
const zStringBool = z
  .string()
  .refine((val) => val === "true" || val === "false", "Must be true or false")
  .transform((val) => val === "true");
```

#### Features
- Validates string input must be exactly "true" or "false"
- Transforms validated string to actual boolean value
- Provides type-safe boolean handling for query parameters

### Usage Example

```typescript
// In an API route
export const GET = (req: NextRequest) =>
  buildHandler({
    req,
    searchParamsSchema: z.object({
      isActive: zStringBool.optional(),
    }),
    handler: async ({ searchParams }) => {
      // searchParams.isActive is boolean | undefined
      const items = await getItems({ active: searchParams.isActive });
      return { status: 200, resp: items };
    },
  });
```

### Valid Requests
```
GET /api/endpoint?isActive=true   // searchParams.isActive = true
GET /api/endpoint?isActive=false  // searchParams.isActive = false
GET /api/endpoint                 // searchParams.isActive = undefined
```

### Invalid Requests
```
GET /api/endpoint?isActive=1      // Error: Must be true or false
GET /api/endpoint?isActive=yes    // Error: Must be true or false
GET /api/endpoint?isActive=       // Error: Must be true or false
```

## Notes
- Used primarily for query parameter validation
- Ensures consistent boolean handling across the API
- Provides better type safety than manual string parsing
- Integrates with Zod for runtime type validation