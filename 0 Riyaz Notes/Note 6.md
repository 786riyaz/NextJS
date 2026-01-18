Very well. This lesson is where Next.js starts to feel **powerful and elegant** compared to traditional MERN routing.

---

# Lesson 6: Dynamic Routes & SEO-Friendly URLs in Next.js

Dynamic routing is essential for:

* Product pages
* Blog posts
* User profiles

If you understand this lesson properly, you can build **real-world applications**.

---

## Lesson Objectives

By the end of this lesson, you will understand:

1. What dynamic routes are
2. How `[id]` and `[slug]` work
3. How `params` is passed
4. `generateStaticParams` (very important)
5. Best practices for SEO and performance

---

## 1. What Is a Dynamic Route?

A **dynamic route** is a URL whose segment changes dynamically.

Examples:

* `/products/1`
* `/products/2`
* `/blog/how-to-learn-nextjs`

In React + react-router:

```jsx
<Route path="/products/:id" />
```

In **Next.js**, the filesystem handles this.

---

## 2. Creating a Dynamic Route

### Folder Structure

```txt
app/
└── products/
    └── [id]/
        └── page.js
```

### URL Mapping

```txt
/products/1   →  id = 1
/products/99  →  id = 99
```

---

## 3. Accessing Route Parameters

### Example

```jsx
export default async function ProductPage({ params }) {
  const { id } = params;

  const res = await fetch(
    `https://dummyjson.com/products/${id}`
  );
  const product = await res.json();

  return <h1>{product.title}</h1>;
}
```

### Key Concept

* `params` is injected by Next.js
* No hooks needed
* Runs on the server

---

## 4. Dynamic Routes + SSG (Important)

By default, dynamic routes are **server-rendered**.

To pre-generate pages at build time, we use:

### `generateStaticParams`

---

## 5. `generateStaticParams` Explained (Critical)

### Purpose

> Tell Next.js **which dynamic pages to build in advance**

---

### Example

```jsx
export async function generateStaticParams() {
  const res = await fetch("https://dummyjson.com/products");
  const data = await res.json();

  return data.products.map(product => ({
    id: product.id.toString(),
  }));
}
```

### Full Page Example

```jsx
export async function generateStaticParams() {
  const res = await fetch("https://dummyjson.com/products");
  const data = await res.json();

  return data.products.map(p => ({
    id: p.id.toString(),
  }));
}

export default async function ProductPage({ params }) {
  const res = await fetch(
    `https://dummyjson.com/products/${params.id}`
  );
  const product = await res.json();

  return <h1>{product.title}</h1>;
}
```

---

## 6. When to Use `generateStaticParams`

| Scenario      | Use It? |
| ------------- | ------- |
| Blog posts    | Yes     |
| Product pages | Yes     |
| User profiles | No      |
| Dashboards    | No      |

Rule:

> Use it only for **public, predictable content**

---

## 7. Slug vs ID (SEO Concept)

### ID-Based

```
/products/23
```

### Slug-Based (Better SEO)

```
/blog/learn-nextjs-fast
```

### Folder

```txt
blog/
└── [slug]/
    └── page.js
```

### Example

```jsx
export default async function BlogPost({ params }) {
  const post = await getPostBySlug(params.slug);
  return <h1>{post.title}</h1>;
}
```

---

## 8. Handling 404 (Not Found)

Next.js provides:

```js
notFound()
```

### Example

```jsx
import { notFound } from "next/navigation";

export default async function ProductPage({ params }) {
  const res = await fetch(
    `https://dummyjson.com/products/${params.id}`
  );

  if (!res.ok) {
    notFound();
  }

  const product = await res.json();
  return <h1>{product.title}</h1>;
}
```

---

## 9. Common Mistakes (Avoid These)

1. Using `useRouter` unnecessarily
2. Fetching params from client
3. Forgetting `toString()` in static params
4. Pre-rendering user-specific pages
5. Overusing dynamic routes

---

## Lesson 6 Summary

You now understand:

* Dynamic routing via filesystem
* `[id]` and `[slug]`
* `params`
* `generateStaticParams`
* SEO-friendly URLs
* Proper error handling

You can now build:

* Blogs
* Product pages
* Public landing pages

---

## Homework (Important)

1. Create `/blog/[slug]`
2. Use dummy data
3. Add `generateStaticParams`
4. Render title using slug

---

## Next Lesson (Lesson 7 Preview)

We will cover:

* **Metadata & SEO**
* Dynamic `<title>` and `<meta>`
* Open Graph
* Social sharing optimization

When ready, reply exactly with:

> **Continue to Lesson 7**

We will continue systematically.
