# Next.js Review Guidelines

## Components & Rendering
- Flag if: "use client" added to layout, page with only static content, or component using async/await at top → HIGH
- Flag if: server component imports client-only API (window, document, localStorage, framer-motion) without "use client" → CRITICAL
- Flag if: obj.a.b.c chained access without ?. or guard on fetched/props data → HIGH
- Flag if: .map without stable key, or key={index} on reorderable list → MEDIUM

## State & Data Fetching
- Flag if: fetch/axios in useEffect without cleanup, AbortController, or ignore flag → HIGH
- Flag if: setState called in render body or after await without mounted check → HIGH
- Flag if: Context value is inline object/array literal in Provider (value={{...}}) → MEDIUM
- Flag if: useEffect dependency array missing referenced var, or [] with stale closure → HIGH
- Flag if: response.json() called without checking response.ok first → HIGH

## Auth (Firebase / Custom)
- Flag if: protected route guarded only by client-side useEffect redirect → CRITICAL
- Flag if: Firebase ID token stored in localStorage/cookie manually instead of getIdToken() per request → CRITICAL
- Flag if: API call to Django backend without Authorization: Bearer header on authed route → CRITICAL
- Flag if: onAuthStateChanged subscribed without unsubscribe in cleanup → MEDIUM
- Flag if: Firebase config object with apiKey committed in non-NEXT_PUBLIC env or server file → HIGH

## Performance
- Flag if: motion component animates layout properties (width, height, top, left) instead of transform/opacity → MEDIUM
- Flag if: <img> used instead of next/image for static/remote images → MEDIUM
- Flag if: large list rendered without virtualization or pagination (>100 items mapped) → MEDIUM
- Flag if: AnimatePresence wraps list without unique key per child → HIGH
