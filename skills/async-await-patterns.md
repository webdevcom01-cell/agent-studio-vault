---
title: "Async Await Patterns"
tags: ["skill", "auto-synced", "typescript", "async", "promises", "javascript", "patterns"]
updated: "2026-05-05T18:06:23.258Z"
---

# Async/Await Patterns

## Basic async/await

```typescript
async function fetchUser(id: string): Promise<User> {
  const res = await fetch(`/api/users/${id}`);
  if (!res.ok) throw new Error(`HTTP ${res.status}`);
  return res.json();
}
```

## Paralelno izvršavanje

```typescript
// Paralelno - brže!
const [user, posts] = await Promise.all([
  fetchUser(id),
  fetchPosts(id)
]);

// Promise.allSettled — ne prekida na grešci
const results = await Promise.allSettled([
  fetchA(), fetchB(), fetchC()
]);
```

## Error handling

```typescript
async function safeCall<T>(fn: () => Promise<T>): Promise<[T, null] | [null, Error]> {
  try {
    return [await fn(), null];
  } catch (err) {
    return [null, err as Error];
  }
}

const [data, error] = await safeCall(() => fetchUser(id));
```