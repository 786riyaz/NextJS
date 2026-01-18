Very well. This lesson focuses on **user experience, resilience, and production polish**. Even well-architected applications feel poor without proper loading and error handling.

---

# Lesson 13: Loading, Error, and Not Found States in Next.js

Next.js provides **first-class, file-based UX states**—not ad-hoc spinners or try/catch blocks scattered everywhere.

---

## Lesson Objectives

By the end of this lesson, you will understand:

1. How loading states work in Next.js
2. Streaming and partial rendering
3. Error boundaries with `error.js`
4. Handling 404 with `not-found.js`
5. Production-grade UX patterns

---

## 1. The UX Problem in Traditional React

### Typical React Pattern

```js
const [loading, setLoading] = useState(true);
const [error, setError] = useState(null);
```

Problems:

* Boilerplate
* Repetition
* Inconsistent UX
* Hard to maintain

Next.js **solves this at the framework level**.

![Image](https://h8dxkfmaphn8o0p3.public.blob.vercel-storage.com/docs/light/loading-ui.png)

![Image](https://nextjs.org/_next/image?q=75\&url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Flearn%2Flight%2Fserver-rendering-with-streaming.png\&w=3840)

![Image](https://blog.appsignal.com/_next/image?q=90\&url=%2Fimages%2Fblog%2F2024-08%2Ferror-boundary.png\&w=3840)

---

## 2. `loading.js` — Route-Level Loading UI

### What It Does

> Displays while a route or layout is fetching data.

### File Placement

```txt
app/
└── users/
    ├── page.js
    └── loading.js
```

### Example

```jsx
export default function Loading() {
  return <p>Loading users...</p>;
}
```

### Behavior

* Automatically shown
* No state management
* Works with server data fetching

---

## 3. Streaming (Why `loading.js` Works)

Next.js streams HTML:

1. Sends layout immediately
2. Shows `loading.js`
3. Streams page content when ready

This makes apps feel **instant**, even with slow data.

---

## 4. `error.js` — Error Boundaries

### Purpose

> Catch runtime errors in a route segment.

### File Placement

```txt
app/
└── users/
    ├── page.js
    └── error.js
```

### Example

```jsx
"use client";

export default function Error({ error, reset }) {
  return (
    <div>
      <h2>Something went wrong</h2>
      <button onClick={reset}>Retry</button>
    </div>
  );
}
```

### Important

* Must be a **Client Component**
* Automatically wraps child routes
* `reset()` retries the render

---

## 5. `not-found.js` — Custom 404 per Route

### Purpose

> Handle missing data or invalid routes gracefully.

### File

```txt
app/
└── users/
    ├── [id]/
    │   ├── page.js
    │   └── not-found.js
```

### Example

```jsx
export default function NotFound() {
  return <h1>User not found</h1>;
}
```

### Triggering It

```js
import { notFound } from "next/navigation";

if (!user) {
  notFound();
}
```

---

## 6. Global vs Route-Scoped UX

| File                 | Scope      |
| -------------------- | ---------- |
| `app/loading.js`     | Entire app |
| `route/loading.js`   | That route |
| `route/error.js`     | That route |
| `route/not-found.js` | That route |

This allows **granular UX control**.

---

## 7. Production UX Pattern (Recommended)

* Skeleton loaders instead of spinners
* Clear error messages
* Retry options
* Friendly 404 pages

Example loading UI:

```jsx
export default function Loading() {
  return <div className="skeleton-list" />;
}
```

---

## 8. Common Mistakes (Avoid These)

1. Using `useState` for loading unnecessarily
2. Catching errors inside pages instead of `error.js`
3. Forgetting `"use client"` in `error.js`
4. Generic 404 pages everywhere
5. Over-animating loaders

---

## Lesson 13 Summary

You now understand:

* Route-level loading states
* Streaming UI
* Error boundaries
* Custom 404 handling
* Clean, scalable UX patterns

At this point, your apps will feel **fast, stable, and professional**.

---

## Homework (Important)

1. Add `loading.js` to a data-fetching route
2. Add `error.js` and simulate an error
3. Add `not-found.js` to a dynamic route
4. Verify behavior by breaking fetch logic

---

## Next Lesson (Lesson 14 Preview)

We will cover:

* **Deployment & Production Best Practices**
* Environment handling
* Vercel deployment
* Performance optimizations

When ready, reply exactly with:

> **Continue to Lesson 14**

We will proceed methodically.
