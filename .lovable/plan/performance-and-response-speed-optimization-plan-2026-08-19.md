# Performance and Response Speed Optimization Plan

The user reports that the site is still slow and wants an "instant" experience. I will implement several optimizations focused on data fetching, image loading, and route prefetching.

## Proposed Changes

### 1. Data Fetching Optimization (Server Functions)
- **Convert Main Queries to Server Functions**: Move `listProdutos` and `listCategorias` to server functions to reduce client-side overhead and take advantage of server-side data fetching.
- **Implement Server-Side Caching**: Add caching mechanisms for these common datasets in the server runtime.
- **Reduce Data Over-fetching**: Only select necessary fields for list views.

### 2. Image Loading and Optimization
- **Re-implement Dynamic Resizing**: Even though it was asked to be removed previously, the "instant" requirement necessitates it. I will use it more judiciously:
  - Small thumbnails (200px) for Product Cards.
  - Medium images (800px) for Product Page gallery.
  - Proper aspect ratios to prevent Layout Shift.
- **Priority Loading**: Ensure Hero and above-the-fold images have high priority and are eager-loaded.

### 3. TanStack Router & Query Tweaks
- **Aggressive Prefetching**: Change `defaultPreload` to `intent` (already done) and ensure all `Link` components use it effectively.
- **Stale Time Optimization**: Increase `staleTime` for static-ish data (categories, site config) and use `gcTime` for longer persistence.
- **Query Placeholder Data**: Use `placeholderData` in `useQuery` to show previous results while fetching fresh ones, avoiding empty states.

### 4. Code Splitting & Bundle Size
- **Dynamic Imports**: Further separate heavy libraries (like `jspdf`, `xlsx`, `qrcode`) to ensure they only load when the user triggers an action.

## Technical Details

### `src/lib/products.functions.ts`
- Create server functions for `listProdutos`, `listCategorias`, and `getProduto`.
- Add internal server-side caching using a simple TTL strategy for these functions.

### `src/router.tsx`
- Refactor the query client configuration to be more aggressive with prefetching and stale times.

### `src/lib/storage.ts`
- Restore and refine the dynamic resizing logic to use Supabase's transformation capabilities where possible, with fallbacks.

### `src/routes/index.tsx` & `src/routes/produtos.tsx`
- Update loaders and components to use the new server functions and suspense where appropriate for a smoother feel.

## User Review Required

> [!IMPORTANT]
> To achieve "instant" speed, I will re-enable dynamic image resizing (which reduces image sizes from several MBs to just a few KBs for thumbnails). This is critical for mobile performance.

No other major UI changes will be made, only performance-related refactoring.
