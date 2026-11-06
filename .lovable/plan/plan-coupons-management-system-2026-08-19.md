# Plan - Coupons Management System

The user wants to add a "Cupons" (Coupons) tab to the admin panel. This system will allow configuring discount coupons with various rules: percentage discount, specific product application, minimum quantity requirement, and validity period.

## User Review Required

> [!IMPORTANT]
> - Coupons will be applied at the checkout.
> - The current implementation will handle percentage discounts.
> - I will add a `cupons` table to the database.

## Proposed Changes

### Database Schema
- Create `public.cupons` table:
    - `id` (uuid, pk)
    - `codigo` (text, unique, e.g., "VERAO20")
    - `tipo_desconto` (text: 'percentual')
    - `valor_desconto` (numeric, e.g., 10 for 10%)
    - `quantidade_minima_itens` (int, default 0)
    - `validade` (timestamp)
    - `ativo` (boolean, default true)
    - `produtos_ids` (uuid[], nullable - if empty, applies to all)
    - `created_at` (timestamp)
- Enable RLS and add policies for admins to manage and all authenticated users to read (for validation during checkout).

### Admin Panel
- Create `src/routes/_authenticated/admin.cupons.tsx`:
    - A list of existing coupons.
    - A form to create/edit coupons.
    - Fields for: code, percentage, minimum items, expiry date, and active status.
- Update `src/routes/_authenticated/admin.tsx`:
    - Add "Cupons" to the navigation menu.

### Server Functions
- Create `src/lib/coupons.functions.ts` for CRUD operations and a `validateCoupon` function.

### Frontend - Checkout
- Update `src/lib/cart.tsx` to handle a `coupon` state (optional, or just handle it in checkout).
- Update `src/routes/checkout.tsx`:
    - Add a "Cupom de Desconto" input field.
    - Validate the coupon against the server.
    - Calculate and display the discount if valid.
    - Include the coupon code in the WhatsApp message and PDF.

## Technical Details
- SQL migration for the new table and RLS.
- New route file using TanStack Router.
- Zod validation for coupon inputs.

## Verification Plan

### Manual Verification
1. Create a coupon in the admin panel (e.g., "TESTE10", 10% discount, min 2 items).
2. Go to the store, add 1 item to the cart, try to apply "TESTE10" at checkout -> should fail (min 2 items).
3. Add a second item, apply "TESTE10" -> should apply 10% discount to the total.
4. Verify the discount appears in the total summary.
5. Finalize the order and check if the WhatsApp message and PDF include the coupon.
