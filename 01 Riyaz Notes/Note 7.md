Very well. This lesson is **essential for production and professional work**. Without proper metadata, even a technically perfect app performs poorly in search and sharing.

---

# Lesson 7: Metadata, SEO, and Social Sharing in Next.js

Next.js provides **first-class SEO support**. Unlike React SPAs, you do **not** need external libraries for basic SEO.

---

## Lesson Objectives

By the end of this lesson, you will understand:

1. What metadata is and why it matters
2. Static metadata
3. Dynamic metadata
4. SEO for dynamic routes
5. Open Graph & social sharing
6. Best practices used in real companies

---

## 1. What Is Metadata?

Metadata is information **about** your page, not visible UI.

Examples:

* Page title
* Description
* Keywords
* Social preview (image, title, description)

This affects:

* Google search ranking
* Link previews (WhatsApp, LinkedIn, Twitter)
* Browser tabs

![Image](https://cdn.sanity.io/images/tkl0o0xu/production/ee479f747173571f179d1d18be14aadd05c4d039-1200x374.png?dpr=1\&fit=min\&h=267\&q=95\&w=856)

![Image](https://svaerm.com/wp-content/uploads/2022/02/open-graph-meta-tags-rankmath-seo-twitter.jpg)

![Image](https://ieinteractiveservices.com/wp-content/uploads/2022/05/metadata-example_orig.png)

![Image](https://www.socialmediaexaminer.com/wp-content/uploads/2022/12/how-to-rank-in-google-search-results-page-optimization-metadata-title-tags-meta-descriptions-rich-snippets-search-engine-rankings-example-4.png)

---

## 2. Metadata in Next.js (App Router)

Next.js uses a **Metadata API**, not `<head>` tags.

You export metadata directly from:

* `page.js`
* `layout.js`

---

## 3. Static Metadata (Most Common)

### Example: Home Page

```js
export const metadata = {
  title: "My Store – Buy Quality Products",
  description: "An online store built with Next.js",
};
```

### Result

* `<title>` set automatically
* `<meta name="description">` injected

No Helmet. No extra libraries.

---

## 4. Metadata in Layouts (Global SEO)

### `app/layout.js`

```js
export const metadata = {
  title: {
    default: "My App",
    template: "%s | My App",
  },
  description: "Production-grade Next.js application",
};
```

### Result

* `/blog` → `Blog | My App`
* `/products` → `Products | My App`

This is **enterprise-grade SEO configuration**.

---

## 5. Dynamic Metadata (Critical for Blogs & Products)

### Use Case

Each blog/product page needs:

* Unique title
* Unique description

---

### Dynamic Metadata Function

```js
export async function generateMetadata({ params }) {
  const res = await fetch(
    `https://dummyjson.com/products/${params.id}`
  );
  const product = await res.json();

  return {
    title: product.title,
    description: product.description,
  };
}
```

### Important Notes

* Runs on the **server**
* Can fetch data
* SEO-friendly by default

---

## 6. Metadata for Dynamic Routes (Complete Example)

```js
export async function generateMetadata({ params }) {
  return {
    title: `Blog – ${params.slug}`,
    description: `Read article about ${params.slug}`,
  };
}

export default function BlogPost({ params }) {
  return <h1>{params.slug}</h1>;
}
```

Each URL now has **unique SEO**.

---

## 7. Open Graph (Social Media Sharing)

### Why It Matters

Without Open Graph:

* WhatsApp shows plain links
* LinkedIn previews look bad
* Twitter cards break

---

### Example

```js
export const metadata = {
  title: "Next.js Tutorial",
  description: "Learn Next.js step by step",
  openGraph: {
    title: "Next.js Tutorial",
    description: "Master Next.js with real examples",
    images: ["/og-image.png"],
  },
};
```

Put image in:

```txt
public/og-image.png
```

---

## 8. Metadata Hierarchy (Very Important)

Priority order:

1. `page.js` metadata
2. Nested `layout.js`
3. Root `layout.js`

Metadata **merges automatically**.

---

## 9. Common SEO Mistakes (Avoid These)

1. Using `<head>` manually
2. Forgetting dynamic metadata
3. Duplicate titles
4. Missing descriptions
5. Ignoring Open Graph

Avoiding these already puts you ahead of many developers.

---

## Lesson 7 Summary

You now understand:

* How SEO works in Next.js
* Static vs dynamic metadata
* Layout-level SEO
* Open Graph previews
* Professional SEO setup without libraries

This is **production-level knowledge**.

---

## Homework (Important)

1. Add metadata to `/blog`
2. Add dynamic metadata to `/blog/[slug]`
3. Add Open Graph image
4. Verify preview by inspecting page source

---

## Next Lesson (Lesson 8 Preview)

We will cover:

* **API Routes (Backend inside Next.js)**
* Replacing Express
* REST APIs in `app/api`
* Connecting MongoDB

When ready, reply exactly with:

> **Continue to Lesson 8**

We will proceed methodically, as before.
