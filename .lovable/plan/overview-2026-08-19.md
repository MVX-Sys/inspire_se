---
name: Performance Optimization Plan
description: Enhance site speed by implementing image optimization, preloading, and caching strategies.
type: feature
---

## Overview
The user reported that the site is slow, particularly in loading images and switching tabs. This plan focuses on technical optimizations to improve perceived and actual performance without breaking functionality.

## Technical Details

### 1. Image Optimization
- **Supabase Image Transformation:** Implement dynamic resizing for thumbnails using Supabase's image transformation service (if supported by the plan, otherwise fallback to standard optimizations).
- **Modern Image Formats:** Ensure images are served in WebP/AVIF format when possible.
- **Priority Loading:** Use `fetchpriority="high"` for Hero images and the first few product images on listing pages.
- **Blur-up Placeholders:** Implement a better skeleton or low-res placeholder system for images in `ProductCard`.

### 2. Navigation & Data Prefetching
- **TanStack Router Preloading:** Increase `defaultPreloadStaleTime` in `router.tsx` to enable aggressive prefetching of route data on hover.
- **Query Prefetching:** Implement `queryClient.prefetchQuery` in `onMouseEnter` handlers for navigation links.
- **Cache Optimization:** Review and tune `staleTime` and `gcTime` for frequently accessed data like categories and product lists.

### 3. Frontend Optimizations
- **Component Memoization:** Use `React.memo` for heavy components like `ProductCard` to prevent unnecessary re-renders during filtering.
- **CSS Improvements:** Ensure animations are GPU-accelerated and don't cause layout shifts.

### 4. Code Splitting
- **Dynamic Imports:** Move heavy libraries like `jspdf`, `xlsx`, and `qrcode` to dynamic imports (already partially done for `jszip`, but ensure others are strictly loaded on demand).

## Implementation Steps

### Phase 1: Image & Assets
- Modify `src/lib/storage.ts` to support transformation parameters.
- Update `src/components/product-card.tsx` to use optimized image sizes.
- Add `fetchpriority="high"` to the main Hero image in `src/routes/index.tsx`.

### Phase 2: Router & Query Tuning
- Update `src/router.tsx` to enable preloading (`intent`).
- Wrap `Link` components with prefetch logic where appropriate.
- Adjust `staleTime` in `QueryClient` defaults.

### Phase 3: Component Optimization
- Apply `useMemo` and `React.memo` to performance-critical components.
- Audit `src/routes/produtos.tsx` filtering logic for performance bottlenecks.
