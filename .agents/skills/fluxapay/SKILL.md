```markdown
# fluxapay Development Patterns

> Auto-generated skill from repository analysis

## Overview
This skill provides guidance on contributing to the `fluxapay` TypeScript codebase. It covers established coding conventions, commit patterns, and key workflows—especially around enhancing the display of live FX rates and fiat equivalents in checkout flows. It also documents how to structure code, write tests, and use common repository commands.

## Coding Conventions

### File Naming
- Use **camelCase** for file names.
  - Example: `fxRateBadge.tsx`, `useFxRate.ts`

### Import Style
- Use **alias imports** for modules.
  - Example:
    ```typescript
    import { getFxRate } from '@/lib/api';
    ```

### Export Style
- Use **named exports**.
  - Example:
    ```typescript
    export function useFxRate() { ... }
    ```

### Commit Messages
- Follow **conventional commit** style.
- Use the `feat` prefix for features.
  - Example:
    ```
    feat: add live FX rate badge to checkout page
    ```

## Workflows

### Add or Enhance Checkout FX Rate Display
**Trigger:** When someone wants to show or update live FX rates or fiat equivalents on checkout/invoice pages.  
**Command:** `/add-checkout-fx-rate`

**Step-by-step:**
1. **Implement or update API method for FX rates in API client**
    - Edit `fluxapay_frontend/src/lib/api.ts` to add or update a function for fetching FX rates.
    - Example:
      ```typescript
      export async function getFxRate(currency: string): Promise<number> {
        // Fetch FX rate from backend or external API
      }
      ```
2. **Create or update SWR hook for fetching FX rates**
    - Edit or create `fluxapay_frontend/src/hooks/useFxRate.ts`.
    - Example:
      ```typescript
      import useSWR from 'swr';
      import { getFxRate } from '@/lib/api';

      export function useFxRate(currency: string) {
        return useSWR(['fxRate', currency], () => getFxRate(currency));
      }
      ```
3. **Build or enhance UI component (FxRateBadge) to display FX rate and/or fiat equivalent**
    - Edit `fluxapay_frontend/src/components/checkout/FxRateBadge.tsx`.
    - Example:
      ```typescript
      import { useFxRate } from '@/hooks/useFxRate';

      export function FxRateBadge({ currency }: { currency: string }) {
        const { data: rate, isLoading } = useFxRate(currency);
        if (isLoading) return <span>Loading FX rate...</span>;
        return <span>1 {currency} ≈ {rate} USD</span>;
      }
      ```
4. **Embed or update the component in one or more checkout-related page(s)**
    - Update:
      - `fluxapay_frontend/src/app/pay/[payment_id]/page.tsx`
      - `fluxapay_frontend/src/app/pay/invoice/[invoice_id]/page.tsx`
    - Example:
      ```typescript
      import { FxRateBadge } from '@/components/checkout/FxRateBadge';

      // Inside your page component:
      <FxRateBadge currency="EUR" />
      ```

## Testing Patterns

- Test files follow the `*.test.*` pattern.
  - Example: `fxRateBadge.test.tsx`
- The specific testing framework is not detected, but tests are likely colocated with the files they test.
- Example test file structure:
  ```typescript
  import { render } from '@testing-library/react';
  import { FxRateBadge } from './FxRateBadge';

  test('renders FX rate', () => {
    // ...test implementation
  });
  ```

## Commands

| Command               | Purpose                                                         |
|-----------------------|-----------------------------------------------------------------|
| /add-checkout-fx-rate | Add or enhance live FX rate and fiat equivalent display in checkout/invoice flows |
```
