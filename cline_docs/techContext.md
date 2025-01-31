# Technical Context

## Technologies Used

-   **Programming Language:** TypeScript
-   **Framework:** Next.js (inferred from `next/server` usage and project structure)
-   **API:** tRPC
-   **Schema Validation:** `zod`
-   **Database:** Not explicitly specified, but likely a database is used for data persistence (e.g., PostgreSQL, MySQL, MongoDB).
-   **Asset Storage:** Not explicitly specified, but likely a file storage system is used (e.g., local file system, cloud storage).
-   **Package Manager:** Not explicitly specified, but likely npm or yarn.
-   **Other Libraries:**
    -   `@trpc/server`
    -   `@hoarder/shared` (likely a custom library or set of utilities)
    -   `@hoarder/db` (likely a custom database interaction library)

## Development Setup

-   The project likely uses a development server that can be started with a command like `npm run dev` or `yarn dev`.
-   API endpoints are likely accessible during development at a URL like `http://localhost:3000/api`.
-   tRPC endpoints are likely accessible at `http://localhost:3000/api/trpc`.

## Technical Constraints

-   **Maximum Asset Size:** The `MAX_UPLOAD_SIZE_BYTES` constant in `code/assets/route.ts` indicates a constraint on the maximum size of uploaded assets. The value is determined by `serverConfig.maxAssetSizeMb` which is not defined in the code provided.
-   **Demo Mode:** The `serverConfig.demoMode` check in `code/assets/route.ts` and `code/v1/bookmarks/singlefile/route.ts` suggests a demo mode that might restrict certain operations, specifically mutations are not allowed.
-   **Supported Upload Asset Types**: The `SUPPORTED_UPLOAD_ASSET_TYPES` constant in `code/assets/route.ts` indicates a constraint on the types of assets that can be uploaded. The specific types are not defined in the code provided.