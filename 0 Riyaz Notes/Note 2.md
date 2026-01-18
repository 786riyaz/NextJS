
# Lesson 2: Creating Your First Next.js App

### Step 1: Requirements

Make sure you have:

* Node.js 18+
* npm or yarn

Check:

```bash
node -v
npm -v
```

---

### Step 2: Create a Next.js App

```bash
npx create-next-app@latest my-next-app
cd my-next-app
npm run dev
```

Open:

```
http://localhost:3000
```

You now have a **working Next.js application**.

---

## Understanding the Folder Structure (Very Important)

```txt
my-next-app/
│
├── app/                ← Main routing system
│   ├── page.js         ← Home page (/)
│   ├── layout.js       ← Root layout
│
├── public/             ← Images, fonts
├── package.json
```

### Key Rule

> **Folders define routes.**

---

## First Mental Model Shift (Critical)

### React (Old Way)

```jsx
<Route path="/about" component={About} />
```

### Next.js (New Way)

```txt
app/
 └── about/
     └── page.js   →  /about
```

No router configuration.
**The filesystem is the router.**

---

## Your First Page (Hands-On)

### `app/page.js`

```jsx
export default function Home() {
  return (
    <main>
      <h1>Welcome to Next.js</h1>
      <p>This page is server-rendered by default.</p>
    </main>
  );
}
```

That’s it. No ReactDOM, no BrowserRouter.

---

## Important Concept #1: Server Components (Default)

In Next.js (App Router):

* Components run **on the server by default**
* Smaller JS bundle
* Better performance

If you need browser features (state, effects):

```jsx
"use client";

import { useState } from "react";

export default function Counter() {
  const [count, setCount] = useState(0);

  return <button onClick={() => setCount + 1}>+</button>;
}
```

This line is **extremely important**:

```js
"use client";
```

We will revisit this in depth later.

---

## Summary of Today’s Lesson

You learned:

* What Next.js is
* Why it exists
* How it improves MERN apps
* How routing works
* Server vs client mindset
* How to create and run a project

---

## Homework (Do This Before Next Lesson)

1. Create a new route:

```txt
app/about/page.js
```

2. Render:

```jsx
export default function About() {
  return <h1>About Page</h1>;
}
```

3. Visit:

```
http://localhost:3000/about
```

---

## Next Lesson Preview

In the next session, I will teach:

* **Layouts**
* **Nested routing**
* **Reusable UI structure**
* **How Next.js replaces react-router**

When you are ready, reply with:

> **“Continue to Lesson 3”**

I will proceed step by step, exactly like a personal tutor.
