# Plan - Standardize Payment to PIX

The user wants to remove the payment method selection in the checkout process, as all payments will now be made via PIX. I will update the checkout logic and UI to reflect this.

## User Review Required

> [!NOTE]
> All references to "Money", "Credit Card", and "Debit Card" will be removed from the checkout process, and PIX will be set as the only option.

## Proposed Changes

### Checkout Route
- Remove `FormaPagamento` type options other than `PIX`.
- Remove the payment method selection UI (the toggle buttons).
- Hardcode the payment method to "PIX" in the state and when creating the order.
- Update the WhatsApp message generation to always state "PIX" as the payment method.
- Update the PDF generation parameters to always use "PIX".

## Technical Details
- Modify `src/routes/checkout.tsx`:
    - Update `FormaPagamento` type.
    - Set `useState<FormaPagamento>("PIX")` as the constant state.
    - Remove the `Field` component rendering the payment buttons.
- No database changes are required since "PIX" is already a valid value in the existing schema/enum.

## Verification Plan

### Automated Tests
- Not applicable for this UI change.

### Manual Verification
1. Navigate to the `/checkout` page.
2. Confirm that the "Forma de Pagamento" selection is gone.
3. Complete a test order and verify that:
    - The WhatsApp message explicitly mentions PIX.
    - The generated PDF mentions PIX.
    - The record in the database (via server function) is saved with PIX.
