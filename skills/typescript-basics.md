---
title: "Typescript Basics"
tags: ["skill", "auto-synced", "typescript", "javascript", "types", "interfaces"]
updated: "2026-05-05T14:13:52.421Z"
---

# TypeScript Osnove

TypeScript dodaje statičke tipove na JavaScript, što omogućava detekciju grešaka tokom razvoja.

## Osnovni tipovi

```typescript
let name: string = "Buky";
let age: number = 30;
let active: boolean = true;
let data: any = "bilo što";
```

## Interfejsi

```typescript
interface User {
  id: number;
  name: string;
  email?: string; // opcionalno polje
}
```

## Generici

```typescript
function identity<T>(arg: T): T {
  return arg;
}
const result = identity<string>("hello");
```

## Type Inference

TypeScript automatski zaključuje tip bez eksplicitne anotacije.