# Next.js Code Review Guidelines

## Components

- **Server vs Client**: Default to Server Components. Only use `"use client"` when you need interactivity (state, events, browser APIs). Flag unnecessary `"use client"` at the top of components.
- **Single responsibility**: One component, one purpose. Flag components mixing data fetching, business logic, and UI.
- **Props**: Flag components accepting excessive props (5+) — suggest breaking into smaller components or using composition.

## Data Fetching

- Fetch data in Server Components where possible — not in `useEffect`.
- Flag `useEffect` used purely for data fetching when a Server Component or React Query would be better.
- Flag missing `loading` and `error` states on async data.

## Performance

- Flag raw `<img>` tags — use `next/image` instead.
- Flag large imports that could be lazy-loaded with `dynamic()`.
- Flag missing `key` props on list items.

## State Management

- Flag business logic or API calls placed directly inside components — suggest moving to a custom hook or utility.
- Flag prop drilling beyond 2-3 levels — suggest Context or a state library.

## Security

- Flag environment variables prefixed with `NEXT_PUBLIC_` that contain sensitive values (keys, secrets).
- Flag user input used directly in rendering without sanitisation.

## API Routes

- Flag API routes with no input validation.
- Flag API routes missing authentication/authorisation checks.
- Flag sensitive data returned in responses unnecessarily.
