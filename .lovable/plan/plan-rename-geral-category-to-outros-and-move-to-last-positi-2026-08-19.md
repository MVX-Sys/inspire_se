# Plan: Rename "Geral" category to "Outros" and Move to Last Position

The user wants to rename the existing "Geral" category to "Outros" and ensure it appears as the last category in all listings. Analysis shows "Geral" exists in the database with `ordem: 0` and is currently filtered out in the `CategoriesSection` on the home page.

## Proposed Changes

### Database Updates
- Rename the category "Geral" to "Outros" in the `public.categorias` table.
- Update its `slug` to "outros".
- Set its `ordem` to a high value (e.g., 99) to ensure it appears last when ordered by `ordem ASC`.

### Frontend Updates

#### 1. Home Page
- **File:** `src/routes/index.tsx`
- **Change:** Remove the manual filter `c.slug !== "geral"` in the `CategoriesSection` so the new "Outros" category is visible.

#### 2. Admin Panel
- **File:** `src/routes/_authenticated/admin.tsx` (and related components)
- **Change:** Ensure that when products are created/edited, the "Outros" category is correctly displayed in the selection list.
- **Verification:** Since categories are fetched from the database and sorted by `ordem`, the "Outros" category should naturally appear last.

## Verification Plan

### Manual Verification
- Check the home page "Categorias" carousel to see "OUTROS" at the end.
- Check the filter sidebar in `/produtos` to see "Outros" at the bottom of the category list.
- Check the admin product form to ensure "Outros" is available and listed last.
- Verify that products previously (if any) or newly assigned to this category display correctly.

### Automated Checks
- Run a SQL query to verify the name, slug, and order in the database.
