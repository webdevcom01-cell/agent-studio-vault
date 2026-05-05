---
title: "React Hooks Guide"
tags: ["skill", "auto-synced", "react", "hooks", "javascript", "typescript", "frontend"]
updated: "2026-05-05T17:18:23.154Z"
---

# React Hooks

Hooks omogućavaju korišćenje stanja i životnog ciklusa u funkcionalnim komponentama.

## useState

```typescript
const [count, setCount] = useState<number>(0);
const [user, setUser] = useState<User | null>(null);
```

## useEffect

```typescript
useEffect(() => {
  fetchUser(userId).then(setUser);
  return () => { /* cleanup */ };
}, [userId]);
```

## useCallback

```typescript
const handleClick = useCallback(() => {
  setCount(c => c + 1);
}, []); // prazan niz = ne mijenja se
```

## Custom Hook

```typescript
function useLocalStorage<T>(key: string, initial: T) {
  const [value, setValue] = useState<T>(initial);
  // ...
  return [value, setValue] as const;
}
```