# Plan - Fix Banner Media Uploads and Expand Image Format Support

The user reported that a banner image failed to load after upload and requested support for a wider range of image formats (JPEG, PNG, WebP, HEIC, AVIF, JPEG XL) across the application.

## User Review Required

> [!IMPORTANT]
> - **HEIC Conversion:** HEIC and JPEG XL are not natively supported by all browsers. I will implement client-side conversion for HEIC (already partially present in product forms) and ensure it's applied to banner uploads as well.
> - **JPEG XL Support:** Browser support for JPEG XL is currently limited (mostly behind flags or in specific versions). I will add it to the allowed file selection, but full rendering depends on the user's browser capabilities.

## Proposed Changes

### 1. Storage & Upload Fixes
- **Storage Policies:** Verify and ensure that `product-images` (or the relevant bucket used by banners) has proper public read access so uploaded media is visible.
- **Upload Path Consistency:** The `admin.index.tsx` uses `uploadImage` which puts files in `uploads/`, but then tries to get a public URL from the `produtos` bucket (which might be the wrong bucket name or configured differently than `product-images`). I will standardize this to use the `product-images` bucket for all site assets.

### 2. Expanded Format Support
- **HEIC Conversion Utility:** Extract the HEIC conversion logic from `ProductForm` into a shared utility in `src/lib/images.ts` to make it reusable.
- **Banner Uploads:** Update the banner upload logic in `admin.index.tsx` to:
    - Accept expanded file types (`.heic`, `.heif`, `.avif`, `.jxl`).
    - Automatically convert HEIC/HEIF to JPEG before upload.
- **Product Uploads:** Update `ProductForm` to also accept `AVIF` and `JPEG XL`.

### 3. UI Improvements
- **Banner Preview:** Add a fallback mechanism in the Banner configuration UI to show an error state if an image fails to load.
- **Accept Attributes:** Update all `<input type="file">` elements to explicitly list the new supported extensions.

## Technical Details

### `src/lib/images.ts` (New)
- `convertHeicToJpeg(file: File): Promise<File>`: Using `heic2any`.
- `isSupportedImage(file: File): boolean`: Validation helper.

### `src/routes/_authenticated/admin.index.tsx`
- Update the `onChange` handler for banner uploads to use the new conversion utility.
- Fix the `publicUrl` generation logic to point to the correct Supabase bucket (`product-images`).

### `src/components/product-form.tsx`
- Refactor existing HEIC logic to use the new shared utility.
- Expand `accept` attribute on file inputs.

### `src/lib/storage.ts`
- Ensure `uploadImage` and `getImageUrl` handle the expanded formats correctly.

---

One short sentence: I will fix the banner media uploads and add support for HEIC, AVIF, and JPEG XL formats across the site.
