---
title: "Error Handling Typescript"
tags: ["skill", "auto-synced", "typescript", "error-handling", "patterns", "best-practices"]
updated: "2026-05-05T18:06:29.568Z"
---

# Error Handling

## Custom Error klase

```typescript
class AppError extends Error {
  constructor(
    message: string,
    public code: string,
    public statusCode: number = 500
  ) {
    super(message);
    this.name = "AppError";
  }
}

class NotFoundError extends AppError {
  constructor(resource: string) {
    super(`${resource} nije pronađen`, "NOT_FOUND", 404);
  }
}
```

## Result type pattern

```typescript
type Result<T, E = Error> = 
  | { success: true; data: T }
  | { success: false; error: E };

async function findUser(id: string): Promise<Result<User>> {
  try {
    const user = await prisma.user.findUnique({ where: { id } });
    if (!user) return { success: false, error: new NotFoundError("User") };
    return { success: true, data: user };
  } catch (err) {
    return { success: false, error: err as Error };
  }
}
```