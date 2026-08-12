# TDD test patterns (reference)

Load when you need copy-paste examples. Follow project stack and **common-test-strategy** (prefer real test DB/fixtures over silent mocks unless the baseline allows it).

## Unit test (React Testing Library)

```typescript
import { render, screen, fireEvent } from '@testing-library/react'
import { Button } from './Button'

describe('Button', () => {
  it('should_call_onClick_when_clicked', () => {
    const handleClick = vi.fn()
    render(<Button onClick={handleClick}>Submit</Button>)
    fireEvent.click(screen.getByRole('button', { name: 'Submit' }))
    expect(handleClick).toHaveBeenCalledTimes(1)
  })
})
```

## API route integration test (Next.js)

```typescript
import { NextRequest } from 'next/server'
import { GET } from './route'

describe('GET /api/items', () => {
  it('should_return_200_with_array_when_valid', async () => {
    const request = new NextRequest('http://localhost/api/items')
    const response = await GET(request)
    const data = await response.json()
    expect(response.status).toBe(200)
    expect(Array.isArray(data.data)).toBe(true)
  })
})
```

## E2E (Playwright)

```typescript
import { test, expect } from '@playwright/test'

test('should_complete_core_flow', async ({ page }) => {
  await page.goto('/')
  await page.waitForLoadState('networkidle')
  await page.getByRole('link', { name: 'Items' }).click()
  await expect(page.getByRole('heading', { level: 1 })).toBeVisible()
})
```

## File layout (typical)

```
src/
├── components/Foo/Foo.tsx
├── components/Foo/Foo.test.tsx
├── app/api/items/route.ts
├── app/api/items/route.test.ts
└── e2e/items.spec.ts
```

Use the project's existing test runner config (`package.json`, `vitest.config`, `playwright.config`).
