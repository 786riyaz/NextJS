Very well. This lesson is **core to building real applications**. Many developers misunderstand data fetching in Next.js because they try to apply old React patterns.

---

# Lesson 5: Data Fetching in Next.js (SSR, SSG, ISR)

This lesson will **reframe how you think about APIs and rendering**.

---

## Lesson Objectives

By the end of this lesson, you will understand:

1. How data fetching works in Next.js (App Router)
2. Server-Side Rendering (SSR)
3. Static Site Generation (SSG)
4. Incremental Static Regeneration (ISR)
5. When to use each strategy (real-world rules)

---

## 1. Key Difference from React (Very Important)

### Traditional React

```js
useEffect(() => {
  fetch("/api/products")
}, [])
```

Problems:

* Runs **after** page load
* Poor SEO
* Loading spinners everywhere

---

### Next.js (Correct Way)

> **Data is fetched on the server before HTML is sent to the browser.**

This is not an optimization — this is the default behavior.

![Image](https://miro.medium.com/1%2ACLfxHCCH3RQrYp1l-kd7OQ.gif)

![Image](https://media2.dev.to/dynamic/image/width%3D800%2Cheight%3D%2Cfit%3Dscale-down%2Cgravity%3Dauto%2Cformat%3Dauto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F0z8cpipm5vsjlj11s6rz.png)

![Image](https://miro.medium.com/v2/resize%3Afit%3A2000/1%2AtYGTfXGNkeWxjPFr20WBDw.jpeg)

---

## 2. Server-Side Rendering (SSR)

### Definition

* Data is fetched **on every request**
* Always fresh
* Slightly slower than static

### Example

```jsx
export default async function ProductsPage() {
  const res = await fetch("https://dummyjson.com/products", {
    cache: "no-store",
  });

  const data = await res.json();

  return (
    <ul>
      {data.products.map(p => (
        <li key={p.id}>{p.title}</li>
      ))}
    </ul>
  );
}
```

### Key Line

```js
cache: "no-store"
```

This forces **SSR**.

### Use SSR When:

* Dashboard data
* Authenticated pages
* Frequently changing data
* Admin panels

---

## 3. Static Site Generation (SSG)

### Definition

* Data is fetched **at build time**
* Extremely fast
* Cached as static HTML

### Example

```jsx
export default async function BlogPage() {
  const res = await fetch("https://dummyjson.com/posts");
  const data = await res.json();

  return (
    <ul>
      {data.posts.map(post => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}
```

No special config → **SSG is the default**.

### Use SSG When:

* Blog pages
* Marketing pages
* Documentation
* Content rarely changes

---

## 4. Incremental Static Regeneration (ISR)

### Definition

* Page is static
* Regenerated **in the background**
* Fresh data after a set interval

### Example

```jsx
export default async function NewsPage() {
  const res = await fetch("https://dummyjson.com/posts", {
    next: { revalidate: 60 },
  });

  const data = await res.json();

  return (
    <ul>
      {data.posts.map(post => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}
```

### Meaning

```js
revalidate: 60
```

* Cache for 60 seconds
* After that, Next.js regenerates silently

### Use ISR When:

* News sites
* E-commerce product listings
* Public data that updates periodically

---

## 5. One Table to Rule Them All

| Strategy | Freshness | Speed     | Use Case       |
| -------- | --------- | --------- | -------------- |
| SSG      | Low       | Fastest   | Blogs, docs    |
| ISR      | Medium    | Very fast | Products, news |
| SSR      | High      | Slower    | Dashboards     |

---

## 6. Important: `fetch()` Is Special in Next.js

### Unlike browser `fetch`:

* Cached by default
* Deduplicated automatically
* Runs on the server
* Respects rendering strategy

You do **not** need:

* Axios
* useEffect
* Loading states (most of the time)

---

## 7. Dynamic Routes + Data Fetching (Preview)

Example:

```txt
app/products/[id]/page.js
```

```jsx
export default async function ProductDetail({ params }) {
  const res = await fetch(
    `https://dummyjson.com/products/${params.id}`
  );
  const product = await res.json();

  return <h1>{product.title}</h1>;
}
```

This is **SEO-friendly by default**.

We will cover this fully in the next lesson.

---

## 8. Common MERN Developer Mistakes

1. Using `useEffect` for fetching
2. Fetching from client unnecessarily
3. Using Axios by habit
4. Not understanding cache behavior
5. Overusing SSR

Avoid these, and your apps will scale better.

---

## Lesson 5 Summary

You now understand:

* How Next.js fetches data
* SSR vs SSG vs ISR
* Cache control
* Real-world decision rules
* Why Next.js apps feel fast

This knowledge is **non-negotiable** for production work.

---

## Homework (Important)

1. Create `/blog` page
2. Fetch posts using **SSG**
3. Convert it to **ISR (30 seconds)**
4. Observe behavior after refresh

---

## Next Lesson (Lesson 6 Preview)

We will cover:

* **Dynamic Routes**
* `[slug]` and `[id]`
* `generateStaticParams`
* SEO-friendly URLs

When ready, reply exactly with:

> **Continue to Lesson 6**

We will continue methodically.
