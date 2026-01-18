Very well. This lesson elevates your understanding from **basic authentication** to **enterprise-grade request control**. Middleware is one of the most powerful (and misunderstood) features in Next.js.

---

# Lesson 11: Advanced Middleware — Roles, Rewrites, and Access Control

This lesson focuses on **controlling requests before they reach your application logic**.

---

## Lesson Objectives

By the end of this lesson, you will understand:

1. How middleware actually executes
2. Role-based access control (RBAC)
3. Request redirects vs rewrites
4. Protecting both pages and APIs
5. Where middleware should and should not be used

---

## 1. Middleware Execution Model (Important)

Middleware runs:

* On the **server edge**
* Before pages **and** API routes
* On **every matched request**

### Order of Execution

```
Request
  ↓
Middleware
  ↓
Page / API Route
```

This makes middleware ideal for **security and routing decisions**.

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2A0MwX2ehe6oa_JO8xmcsMVA%402x.jpeg)

![Image](https://miro.medium.com/1%2AmmE6E4a9Tsa2KCVuzaqY5g.png)

![Image](https://pagepro.co/blog/wp-content/webp-express/webp-images/uploads/2024/10/Middleware-processing-request.png.webp)

---

## 2. Redirect vs Rewrite (Critical Difference)

### Redirect

* Changes URL in browser
* User sees new URL
* Extra round trip

```js
NextResponse.redirect(new URL("/login", request.url));
```

### Rewrite

* Keeps URL same
* Serves different content
* Invisible to user

```js
NextResponse.rewrite(new URL("/login", request.url));
```

### Rule of Thumb

* **Auth failures → Redirect**
* **Internal routing logic → Rewrite**

---

## 3. Role-Based Access Control (RBAC)

Instead of a simple `auth=true`, real apps use **roles**.

### Example Cookie

```txt
role=admin
```

---

### Middleware with Roles

```js
import { NextResponse } from "next/server";

export function middleware(request) {
  const role = request.cookies.get("role")?.value;
  const path = request.nextUrl.pathname;

  if (path.startsWith("/admin") && role !== "admin") {
    return NextResponse.redirect(
      new URL("/unauthorized", request.url)
    );
  }
}
```

### Result

* `/admin` → admin only
* `/dashboard` → authenticated users
* `/public` → everyone

---

## 4. Multiple Route Protection Rules

### Clean Pattern

```js
export function middleware(request) {
  const auth = request.cookies.get("auth");
  const role = request.cookies.get("role");
  const path = request.nextUrl.pathname;

  if (!auth && path.startsWith("/dashboard")) {
    return NextResponse.redirect(new URL("/login", request.url));
  }

  if (path.startsWith("/admin") && role !== "admin") {
    return NextResponse.redirect(new URL("/unauthorized", request.url));
  }
}
```

This is **clear, readable, and scalable**.

---

## 5. Middleware Matcher (Mandatory for Performance)

Never let middleware run globally unless required.

```js
export const config = {
  matcher: ["/dashboard/:path*", "/admin/:path*"],
};
```

This ensures:

* Faster requests
* Lower cost on edge
* Predictable behavior

---

## 6. Protecting API Routes with Middleware

Middleware also runs on APIs.

### Example

```js
export const config = {
  matcher: ["/api/admin/:path*"],
};
```

Same auth logic applies.
This prevents **direct API abuse**.

---

## 7. When NOT to Use Middleware

❌ Database access
❌ Heavy computation
❌ Business logic
❌ Data fetching

Middleware should be:

* Fast
* Deterministic
* Stateless

> Think of middleware as a **security gate**, not a controller.

---

## 8. Rate Limiting (Conceptual Overview)

Middleware is often used for:

* Rate limiting
* Bot detection
* Geo-based restrictions

Example idea (conceptual):

```js
if (tooManyRequestsFromIP) {
  return new NextResponse("Too Many Requests", { status: 429 });
}
```

Actual implementations typically use Redis or edge KV stores.

---

## 9. Common Middleware Mistakes (Avoid These)

1. Running middleware globally without matcher
2. Doing database calls inside middleware
3. Storing complex state in cookies
4. Using middleware for UI decisions
5. Overengineering simple access checks

Avoiding these keeps your app **fast and maintainable**.

---

## Lesson 11 Summary

You now understand:

* Middleware execution flow
* Redirect vs rewrite
* Role-based access control
* API protection
* Performance best practices

At this stage, your architecture aligns with **production SaaS applications**.

---

## Homework (Important)

1. Add roles (`user`, `admin`) to cookies
2. Protect `/admin` routes
3. Protect `/api/admin` endpoints
4. Create `/unauthorized` page

---

## Next Lesson (Lesson 12 Preview)

We will cover:

* **Forms and Mutations**
* Server Actions
* Replacing traditional POST APIs
* Secure form handling

When ready, reply exactly with:

> **Continue to Lesson 12**

We will continue methodically.
