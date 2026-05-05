---
title: "Prisma Orm Guide"
tags: ["skill", "auto-synced", "prisma", "database", "orm", "typescript", "postgresql"]
updated: "2026-05-05T17:18:25.835Z"
---

# Prisma ORM

## Schema definicija

```prisma
model User {
  id        String   @id @default(cuid())
  email     String   @unique
  name      String?
  posts     Post[]
  createdAt DateTime @default(now())
}

model Post {
  id       String @id @default(cuid())
  title    String
  content  String?
  author   User   @relation(fields: [authorId], references: [id])
  authorId String
}
```

## CRUD Operacije

```typescript
// Kreiranje
const user = await prisma.user.create({
  data: { email: "user@example.com", name: "Buky" }
});

// Čitanje sa relacijom
const users = await prisma.user.findMany({
  include: { posts: true },
  where: { email: { contains: "@example.com" } }
});

// Update
await prisma.user.update({
  where: { id: userId },
  data: { name: "Novi naziv" }
});

// Delete
await prisma.user.delete({ where: { id: userId } });
```