---
title: "Nextjs Api Routes"
tags: ["skill", "auto-synced", "nextjs", "api", "typescript", "backend", "rest"]
updated: "2026-05-05T17:31:52.744Z"
---

# Next.js API Routes

## App Router

```typescript
// src/app/api/users/route.ts
import { NextRequest, NextResponse } from "next/server";

export async function GET(req: NextRequest) {
  const users = await prisma.user.findMany();
  return NextResponse.json(users);
}

export async function POST(req: NextRequest) {
  const body = await req.json();
  const user = await prisma.user.create({ data: body });
  return NextResponse.json(user, { status: 201 });
}
```

## Dinamički route

```typescript
// src/app/api/users/[id]/route.ts
export async function GET(
  req: NextRequest,
  { params }: { params: { id: string } }
) {
  const user = await prisma.user.findUnique({ where: { id: params.id } });
  if (!user) return NextResponse.json({ error: "Not found" }, { status: 404 });
  return NextResponse.json(user);
}
```