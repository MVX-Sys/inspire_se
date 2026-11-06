# Plan: Fix Coupon Creation RLS Error

The user reported an "RLS policy violation" when trying to create a new coupon in the admin panel. This is happening because the server functions responsible for managing coupons are using the client-side Supabase instance (which follows RLS) but are not yet protected by the authentication middleware that would allow them to act as a privileged user, and the database RLS policies for the `cupons` table might be too restrictive.

## Technical Details

1. **Server Function Security**:
   - `saveCupon` and `deleteCupon` in `src/lib/coupons.functions.ts` currently use the public `supabase` client and lack the `.middleware([requireSupabaseAuth])` wrapper.
   - This means they execute as an `anon` user if called without a token or as the `authenticated` user but without the context needed to verify admin/staff status in a way that RLS respects reliably for writes.
   - I will add the `requireSupabaseAuth` middleware and use the `context.supabase` instance.

2. **Database Permissions**:
   - The `cupons` table needs RLS policies that allow `admin` and `funcionario` (with `cupons.manage` permission) to perform CRUD operations.
   - I'll ensure `GRANT` statements are correctly applied for `authenticated` and `service_role`.

3. **RBAC Check**:
   - I'll add a helper to verify that the user has the `cupons.manage` permission or is an `admin` before proceeding with the write.

## Proposed Changes

### Backend (Database Migration)
- Update RLS policies for the `cupons` table:
  - Enable RLS.
  - Policy for `SELECT`: Allow `anon` and `authenticated` (so customers can validate coupons).
  - Policy for `INSERT/UPDATE/DELETE`: Allow if `public.has_role(auth.uid(), 'admin')` OR `public.has_permission(auth.uid(), 'cupons.manage')`.
- Ensure `GRANT ALL ON public.cupons TO authenticated, service_role`.

### Infrastructure
- Modify `src/lib/coupons.functions.ts`:
  - Add `requireSupabaseAuth` middleware to `saveCupon` and `deleteCupon`.
  - Use `context.supabase` for database operations.
  - Implement an `assertCouponManager` check similar to `assertAdmin`.

## Verification Plan
- I will attempt to "dry run" the logic by checking if the server functions correctly use the authenticated context.
- Since I cannot visually confirm the fix in the admin panel without a login, I will rely on the successful application of migrations and code changes.
