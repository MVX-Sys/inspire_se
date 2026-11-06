# Plan - Convert Admin Navigation to Select Menu

The user wants to change the horizontal tab-style navigation in the admin panel to a select menu (dropdown) style. This will save space and align with the requested UI update.

## User Review Required

> [!NOTE]
> The navigation items will move from a horizontal row of buttons to a single dropdown menu positioned next to the brand logo.

## Proposed Changes

### Admin Layout
- Modify `src/routes/_authenticated/admin.tsx`.
- Replace the `<nav>` mapping with a custom select-like dropdown component using `Select` from `@/components/ui/select` (or a native select if shadcn is not preferred for this specific simplified use, but I will check for shadcn first).
- Since I should keep it high quality, I'll use a `Select` component if available, or a well-styled native `<select>`.
- The dropdown will show the current active route and allow the user to switch between "Produtos", "Atendentes", "Vendas", etc.

## Technical Details
- I will check if `src/components/ui/select.tsx` exists.
- I will implement the change in `src/routes/_authenticated/admin.tsx`.
- I'll add a state or use the router's current location to determine the selected value.

## Verification Plan

### Manual Verification
1. Log in as admin and navigate to `/admin`.
2. Verify that the navigation is now a dropdown menu.
3. Switch between different admin pages (Vendas, Usuários, etc.) and confirm the navigation works correctly.
4. Check responsiveness on mobile and desktop.
