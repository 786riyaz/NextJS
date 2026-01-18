Very well. This lesson completes the **full-stack picture**. After this, you will clearly see how **Next.js can replace Express** for most MERN use cases.

---

# Lesson 8: API Routes in Next.js (Backend Inside the App)

This lesson is **critical for MERN developers**, because it changes how you think about backend architecture.

---

## Lesson Objectives

By the end of this lesson, you will understand:

1. What API routes are in Next.js
2. How they replace Express routes
3. Folder structure for APIs
4. HTTP methods (GET, POST, PUT, DELETE)
5. How frontend and backend coexist cleanly

---

## 1. Mental Shift: No Separate Express Server

### Traditional MERN

```
React (Frontend)
↓
Express (Backend)
↓
MongoDB
```

### Next.js Full Stack

```
Next.js (Frontend + Backend)
↓
MongoDB
```

One codebase. One deployment. Fewer moving parts.

![Image](https://miro.medium.com/1%2A6LyIlAxDmwMisIMfZ8blHg.png)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/0%2AV3DfDO28smDMilzK.jpg)

![Image](https://media2.dev.to/dynamic/image/width%3D1080%2Cheight%3D1080%2Cfit%3Dcover%2Cgravity%3Dauto%2Cformat%3Dauto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fm3plfw1i78wfgr0k83vf.png)

---

## 2. What Is an API Route?

> An **API Route** is a backend endpoint created inside the Next.js project.

It:

* Runs only on the server
* Can access databases and secrets
* Returns JSON (like Express)

---

## 3. API Route Folder Structure (Very Important)

### Location

```txt
app/api/
```

### Example

```txt
app/
└── api/
    └── products/
        └── route.js
```

This creates:

```
/api/products
```

---

## 4. Basic API Route (GET)

### `app/api/products/route.js`

```js
export async function GET() {
  return Response.json({
    success: true,
    products: [
      { id: 1, name: "Laptop" },
      { id: 2, name: "Phone" },
    ],
  });
}
```

### Test in Browser

```
http://localhost:3000/api/products
```

This is equivalent to:

```js
app.get("/api/products", ...)
```

---

## 5. HTTP Methods Mapping

| HTTP Method | Function                            |
| ----------- | ----------------------------------- |
| GET         | `export async function GET()`       |
| POST        | `export async function POST(req)`   |
| PUT         | `export async function PUT(req)`    |
| DELETE      | `export async function DELETE(req)` |

---

## 6. POST Request Example

### API

```js
export async function POST(request) {
  const body = await request.json();

  return Response.json({
    message: "Product created",
    data: body,
  });
}
```

### Client Call

```js
await fetch("/api/products", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ name: "Tablet" }),
});
```

No Axios required.

---

## 7. Dynamic API Routes

### Folder

```txt
app/api/products/[id]/route.js
```

### Code

```js
export async function GET(request, { params }) {
  return Response.json({
    productId: params.id,
  });
}
```

URL:

```
/api/products/123
```

---

## 8. API Routes vs Server Components (Important Distinction)

### Server Components

* Fetch data
* Render UI
* Used by pages

### API Routes

* Handle mutations (POST, PUT, DELETE)
* Authentication
* Webhooks
* External access

**Rule:**

> Read → Server Component
> Write → API Route

---

## 9. Environment Variables (Security)

Use:

```env
MONGODB_URI=your_secret_here
```

Access inside API:

```js
process.env.MONGODB_URI
```

✔ Safe
✔ Server-only
✔ Never exposed to browser

---

## 10. Common MERN Developer Mistakes

1. Recreating Express patterns unnecessarily
2. Using Axios out of habit
3. Putting heavy logic in client components
4. Forgetting API routes are server-only
5. Overusing API routes for read-only data

Avoid these and your architecture will stay clean.

---

## Lesson 8 Summary

You now understand:

* How backend works in Next.js
* API route structure
* HTTP methods
* Dynamic API routes
* When to use APIs vs Server Components

At this point, you are building **true full-stack Next.js applications**.

---

## Homework (Important)

1. Create `/api/users`
2. Support GET and POST
3. Call it from a page using `fetch`
4. Render returned JSON

---

## Next Lesson (Lesson 9 Preview)

We will cover:

* **Connecting MongoDB**
* Database utilities
* Server-only DB access
* Clean project structure

When ready, reply exactly with:

> **Continue to Lesson 9**

We will proceed systematically.
