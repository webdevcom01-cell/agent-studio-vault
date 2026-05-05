---
title: "Tailwind Css Guide"
tags: ["skill", "auto-synced", "tailwind", "css", "frontend", "design", "responsive"]
updated: "2026-05-05T18:06:24.833Z"
---

# Tailwind CSS

## Responsive design

```html
<div class="text-sm md:text-base lg:text-lg">
  Tekst se mijenja po breakpoint-ima
</div>

<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
  <!-- Responsive grid -->
</div>
```

## Flexbox i Grid

```html
<div class="flex items-center justify-between gap-4">
  <span>Lijevo</span>
  <span>Desno</span>
</div>
```

## Dark mode

```html
<div class="bg-white dark:bg-gray-900 text-black dark:text-white">
  Radi u oba moda
</div>
```

## Custom komponente

```typescript
// tailwind.config.ts
export default {
  theme: {
    extend: {
      colors: {
        brand: { DEFAULT: "#6366f1", dark: "#4f46e5" }
      }
    }
  }
}
```