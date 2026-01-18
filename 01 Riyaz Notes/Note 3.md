Very well. We will proceed in a **tutor-led, structured manner**.

---

# Lesson 3: Routing, Layouts, and Navigation in Next.js

This lesson is **foundational**. If you understand this correctly, everything else in Next.js becomes easier.

---

## Lesson Objectives

By the end of this lesson, you will clearly understand:

1. File-based routing (deeply, not superficially)
2. `layout.js` and why it exists
3. Nested routes
4. Persistent UI (Navbar, Footer)
5. Navigation using `Link`

---

## 1. File-Based Routing (Revisited, but Deeper)

### Core Rule

> **Every folder inside `app/` is a route segment**
> **Every `page.js` defines a URL**

### Example Structure

```txt
app/
├── page.js            →  /
├── about/
│   └── page.js        →  /about
├── blog/
│   ├── page.js        →  /blog
│   └── post/
│       └── page.js    →  /blog/post
```

There is:

* No router configuration
* No route arrays
* No `<Routes>` or `<Route>`

This is intentional and enforces **clarity and convention**.

![Image](https://miro.medium.com/v2/resize%3Afit%3A1200/1%2Avq9llllDs4GNS9xq2TWdvg.png)

![Image](https://nextjs.org/_next/image?q=75\&url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Fdocs%2Flight%2Fproject-organization-colocation.png\&w=3840)

![Image](https://h8dxkfmaphn8o0p3.public.blob.vercel-storage.com/docs/dark/terminology-component-tree.png)

---

## 2. What Is `layout.js` and Why It Exists

### Problem in React (Traditional)

In React apps, you typically:

* Repeat Navbar/Footer in multiple pages
* Or wrap routes manually

This leads to:

* Boilerplate
* Mistakes
* Re-rendering issues

---

### Solution in Next.js: `layout.js`

> **A layout wraps all pages inside its folder and persists across navigation.**

---

## 3. Root Layout (Mandatory)

### File

```txt
app/layout.js
```

### Example

```jsx
export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>
        <header>
          <h2>My App</h2>
        </header>

        {children}

        <footer>
          <p>© 2026</p>
        </footer>
      </body>
    </html>
  );
}
```

### Key Concept

* `children` = current page
* Header & footer **do not re-render**
* Pages swap instantly inside layout

---

## 4. Nested Layouts (Very Important)

You can have **multiple layouts**, scoped to routes.

### Folder Structure

```txt
app/
├── layout.js              → Global layout
├── page.js                → Home
├── dashboard/
│   ├── layout.js          → Dashboard layout
│   └── page.js            → /dashboard
```

### `app/dashboard/layout.js`

```jsx
export default function DashboardLayout({ children }) {
  return (
    <section>
      <aside>Dashboard Menu</aside>
      <main>{children}</main>
    </section>
  );
}
```

### Result

* `/` → uses root layout
* `/dashboard` → uses root layout + dashboard layout

This is **hierarchical UI composition**, not routing hacks.

![Image](https://h8dxkfmaphn8o0p3.public.blob.vercel-storage.com/docs/light/nested-layouts-ui.png)

![Image](https://h8dxkfmaphn8o0p3.public.blob.vercel-storage.com/docs/dark/file-conventions-component-hierarchy.png)

![Image](https://i.sstatic.net/93Ne3.jpg)

---

## 5. Pages vs Components (Clear Separation)

### Rule of Thumb

| File Type   | Purpose            |
| ----------- | ------------------ |
| `page.js`   | Route entry point  |
| `layout.js` | Persistent UI      |
| components  | Reusable UI blocks |

### Recommended Structure

```txt
app/
├── page.js
├── layout.js
├── components/
│   ├── Navbar.js
│   └── Footer.js
```

---

## 6. Navigation in Next.js (Replacing react-router)

### ❌ Do NOT use `<a>` for internal routes

```html
<a href="/about">About</a>  ❌
```

### ✅ Use `Link`

```jsx
import Link from "next/link";

export default function Navbar() {
  return (
    <nav>
      <Link href="/">Home</Link>
      <Link href="/about">About</Link>
      <Link href="/blog">Blog</Link>
    </nav>
  );
}
```

### Why `Link`?

* Client-side navigation
* No full page reload
* Faster transitions
* Layouts stay mounted

---

## 7. Client vs Server Reminder (Critical Here)

Layouts and pages are:

* **Server Components by default**

If your Navbar needs state or effects:

```jsx
"use client";
```

Only add this **when required**.
Avoid converting layouts to client unless necessary.

---

## 8. Common Beginner Mistakes (Avoid These)

1. Putting logic in `page.js` instead of components
2. Using `<a>` instead of `Link`
3. Forgetting `children` in layout
4. Making everything `"use client"` unnecessarily
5. Trying to use `react-router`

---

## Lesson 3 Summary

You now understand:

* How routing truly works
* Why layouts exist
* How nested layouts compose UI
* How Next.js replaces react-router cleanly
* How persistent UI is achieved

This is **architectural knowledge**, not syntax memorization.

---

## Homework (Mandatory Before Lesson 4)

1. Create a layout for `/dashboard`
2. Add a sidebar in `dashboard/layout.js`
3. Add a `/dashboard/page.js`
4. Navigate using `Link`

If this feels natural, you are progressing correctly.

---

## Next Lesson (Lesson 4 Preview)

We will cover:

* **Server vs Client Components (in depth)**
* `"use client"` explained properly
* When NOT to use client components
* Performance implications

When ready, reply exactly with:

> **Continue to Lesson 4**

I will proceed accordingly.
