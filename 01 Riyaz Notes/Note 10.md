Very well. This lesson introduces **authentication and route protection**, which is mandatory for any serious application (dashboards, admin panels, SaaS).

---

# Lesson 10: Authentication, Middleware, and Protected Routes in Next.js

This lesson focuses on **conceptual correctness first**, then implementation patterns that scale.

---

## Lesson Objectives

By the end of this lesson, you will understand:

1. How authentication works in Next.js
2. Cookies and sessions (server-first approach)
3. Middleware and request interception
4. Protecting routes (pages + APIs)
5. Industry-standard patterns used in production

---

## 1. Authentication in Next.js — The Correct Mental Model

### Traditional MERN

```
React → Express → Auth Middleware → MongoDB
```

### Next.js

```
Next.js (Server Components + API Routes + Middleware)
```

Authentication is **server-driven**, not client-driven.

![Image](https://nextjs.org/_next/image?q=75\&url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Fdocs%2Flight%2Fauthentication-overview.png\&w=3840)

![Image](https://cdn.buttercms.com/un6jskHwSKeRyMl0kMoU)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2AwccGR2fmBvWBws-xQoeLhA.jpeg)

---

## 2. Cookies vs LocalStorage (Critical Rule)

### ❌ LocalStorage

* Client-only
* Vulnerable to XSS
* Not available on server

### ✅ Cookies (Correct)

* Sent automatically with requests
* Accessible in middleware
* Secure and HTTP-only

> **In Next.js, authentication = cookies + server validation**

---

## 3. Simple Authentication Flow (End-to-End)

1. User submits login form
2. API validates credentials
3. Server sets an **HTTP-only cookie**
4. Middleware checks cookie
5. Protected routes allowed or denied

---

## 4. Login API Route (Example)

### `app/api/login/route.js`

```js
import { cookies } from "next/headers";

export async function POST(request) {
  const { email, password } = await request.json();

  // Example check (replace with DB logic)
  if (email === "admin@test.com" && password === "123456") {
    cookies().set("auth", "true", {
      httpOnly: true,
      secure: true,
      path: "/",
    });

    return Response.json({ success: true });
  }

  return Response.json(
    { success: false },
    { status: 401 }
  );
}
```

This cookie:

* Cannot be accessed by JavaScript
* Is available to middleware
* Works across routes

---

## 5. Reading Cookies on the Server

### In Server Components

```js
import { cookies } from "next/headers";

const cookieStore = cookies();
const isAuth = cookieStore.get("auth");
```

---

## 6. Middleware (Very Important)

### What Is Middleware?

> Code that runs **before** a request reaches a page or API.

Uses:

* Auth checks
* Redirects
* Role-based access
* Logging

---

### Create Middleware

```txt
middleware.js
```

### Example

```js
import { NextResponse } from "next/server";

export function middleware(request) {
  const auth = request.cookies.get("auth");

  if (!auth && request.nextUrl.pathname.startsWith("/dashboard")) {
    return NextResponse.redirect(new URL("/login", request.url));
  }
}
```

---

## 7. Middleware Matcher (Performance Best Practice)

Limit where middleware runs.

```js
export const config = {
  matcher: ["/dashboard/:path*"],
};
```

Now middleware runs **only** for dashboard routes.

---

## 8. Protecting API Routes

Same idea applies to APIs.

```js
import { cookies } from "next/headers";

export async function GET() {
  const auth = cookies().get("auth");

  if (!auth) {
    return Response.json(
      { error: "Unauthorized" },
      { status: 401 }
    );
  }

  return Response.json({ data: "Secret data" });
}
```

---

## 9. Logout API

```js
import { cookies } from "next/headers";

export async function POST() {
  cookies().delete("auth");
  return Response.json({ success: true });
}
```

---

## 10. Where Forms Live (Important)

* Form UI → Client Component
* Auth logic → API Route
* Validation → Server
* Access control → Middleware

Clean separation = maintainable codebase.

---

## 11. Production Note: Auth Libraries

For real apps, use:

* NextAuth / Auth.js
* JWT + cookies
* OAuth providers

But **understanding this lesson first is mandatory**. Libraries are abstractions, not replacements for knowledge.

---

## Common Mistakes (Avoid These)

1. Storing auth state in React state
2. Using LocalStorage for tokens
3. Skipping middleware
4. Trusting client-side checks
5. Mixing auth logic with UI

Avoiding these puts you at **senior-level architectural thinking**.

---

## Lesson 10 Summary

You now understand:

* Server-first authentication
* Secure cookies
* Middleware-based protection
* Protected pages and APIs
* Scalable auth architecture

At this stage, you can build:

* Admin dashboards
* User portals
* Authenticated APIs
* SaaS-style applications

---

## Homework (Very Important)

1. Create `/login` page
2. Add login form (client component)
3. Call `/api/login`
4. Protect `/dashboard` using middleware
5. Add logout functionality

---

## Next Lesson (Lesson 11 Preview)

We will cover:

* **Middleware advanced usage**
* Role-based access (admin/user)
* Request rewriting
* Rate limiting concepts

When ready, reply exactly with:

> **Continue to Lesson 11**

We will proceed systematically.
