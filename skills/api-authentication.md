---
title: "Api Authentication"
tags: ["skill", "auto-synced", "authentication", "jwt", "nextjs", "security", "api"]
updated: "2026-05-05T17:18:32.690Z"
---

# API Autentifikacija

## JWT Middleware

```typescript
import { getToken } from "next-auth/jwt";

export async function verifyAuth(req: NextRequest) {
  const token = await getToken({ req, secret: process.env.NEXTAUTH_SECRET });
  if (!token) throw new AppError("Unauthorized", "UNAUTHORIZED", 401);
  return token;
}
```

## Zaštita route-a

```typescript
export async function GET(req: NextRequest) {
  const token = await verifyAuth(req).catch(() => null);
  if (!token) {
    return NextResponse.json({ error: "Unauthorized" }, { status: 401 });
  }
  // ... ostatak handlera
}
```

## CRON Secret

```typescript
function verifyCronSecret(req: NextRequest): boolean {
  const secret = process.env.CRON_SECRET;
  if (!secret) return true; // dev mode
  const auth = req.headers.get("authorization");
  return auth === `Bearer ${secret}`;
}
```