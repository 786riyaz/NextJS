Very well. This lesson turns your application into a **real production system** by adding persistent data storage. From this point onward, you are no longer building demos.

---

# Lesson 9: Connecting MongoDB in Next.js (Production-Grade Setup)

This lesson focuses on **correct architecture**, not shortcuts.

---

## Lesson Objectives

By the end of this lesson, you will understand:

1. Where MongoDB fits in Next.js
2. How to connect MongoDB safely
3. Reusable database connection patterns
4. Using MongoDB in API routes
5. Using MongoDB in Server Components
6. Common production mistakes (and how to avoid them)

---

## 1. Where MongoDB Lives in Next.js

### Important Rule

> **MongoDB connections must run ONLY on the server.**

That means:

* API Routes ✅
* Server Components ✅
* Client Components ❌

### Architecture

```
Browser
  ↓
Next.js Server (API / Server Components)
  ↓
MongoDB
```

![Image](https://yqintl.alicdn.com/59341befca62c4cb13ffab8db1ec502fea4945aa.png)

![Image](https://media2.dev.to/dynamic/image/width%3D800%2Cheight%3D%2Cfit%3Dscale-down%2Cgravity%3Dauto%2Cformat%3Dauto/https%3A%2F%2Fmermaid.ink%2Fimg%2Fpako%3AeNpVj81Ow0AMhF_F2nML9yAh5ecAh6IojYRE04ObuEkgscOuVwU1fXeWAJXwyeP5NCOfTS0NmcgcBznVHVqFMrurGMLEu3ToiTX6kbk4HZFvn-kAiZWTI7uH9fp-fijLHAp69-R0hmT3RB968-ogzh-hEK-0_w1MFjwVZqrVgcoM6W4j3EqWQIaKB3RXOF3ggtRbdosbsv8FbYkbF4rdJOxohji4FZuVGcmO2Dfhq_M3XxntaKTKRGFt0L5VpuJL4NCrbD-5NpFaTytjxbediY44uKD81KBS1mNrcbxeJ-QXkT99-QLdKGkZ%3Ftype%3Dpng)

![Image](https://images.ctfassets.net/e5382hct74si/kOA2be85nVBQhL0LA9zmO/71f1f53fbf206a18f791b38f8cb2fd6b/Without_React-1.png)

---

## 2. Installing MongoDB Driver

Inside your Next.js project:

```bash
npm install mongodb
```

No Mongoose yet. We start with the **native driver** to understand fundamentals clearly.

---

## 3. Environment Variables (Mandatory)

### `.env.local`

```env
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/mydb
```

### Rules

* Use `.env.local`
* Never commit it
* Never expose it to client code

Access it via:

```js
process.env.MONGODB_URI
```

---

## 4. Database Connection Utility (Best Practice)

### Why?

* Prevents multiple connections
* Works in development and production
* Reusable across API routes

---

### Create File

```txt
lib/mongodb.js
```

### Code

```js
import { MongoClient } from "mongodb";

const uri = process.env.MONGODB_URI;
let client;
let clientPromise;

if (!process.env.MONGODB_URI) {
  throw new Error("Please add MONGODB_URI to .env.local");
}

if (process.env.NODE_ENV === "development") {
  if (!global._mongoClientPromise) {
    client = new MongoClient(uri);
    global._mongoClientPromise = client.connect();
  }
  clientPromise = global._mongoClientPromise;
} else {
  client = new MongoClient(uri);
  clientPromise = client.connect();
}

export default clientPromise;
```

This is a **production-proven pattern**.

---

## 5. Using MongoDB in an API Route

### Example: `/api/users`

```txt
app/api/users/route.js
```

```js
import clientPromise from "@/lib/mongodb";

export async function GET() {
  const client = await clientPromise;
  const db = client.db();

  const users = await db.collection("users").find({}).toArray();

  return Response.json(users);
}

export async function POST(request) {
  const client = await clientPromise;
  const db = client.db();

  const body = await request.json();
  const result = await db.collection("users").insertOne(body);

  return Response.json({ insertedId: result.insertedId });
}
```

---

## 6. Using MongoDB in a Server Component (Read-Only)

You can fetch data **directly** in pages.

### Example

```jsx
import clientPromise from "@/lib/mongodb";

export default async function UsersPage() {
  const client = await clientPromise;
  const db = client.db();

  const users = await db.collection("users").find().toArray();

  return (
    <ul>
      {users.map(user => (
        <li key={user._id.toString()}>{user.name}</li>
      ))}
    </ul>
  );
}
```

### Rule Reminder

> Mutations → API Routes
> Reads → Server Components

---

## 7. Why Not Use MongoDB in Client Components?

❌ Exposes credentials
❌ Breaks security
❌ Impossible in production

If you see MongoDB code in client files — **it is a bug**.

---

## 8. Folder Structure (Clean & Scalable)

```txt
app/
├── api/
│   └── users/
│       └── route.js
├── users/
│   └── page.js
lib/
└── mongodb.js
```

This structure scales cleanly to large applications.

---

## 9. Common Production Mistakes (Avoid These)

1. Opening a new DB connection on every request
2. Using MongoDB directly in client components
3. Hardcoding credentials
4. Mixing API logic with UI logic
5. Overusing API routes for simple reads

Avoiding these puts you at **professional level**.

---

## Lesson 9 Summary

You now understand:

* Secure MongoDB integration
* Connection pooling
* Server-only database access
* Clean separation of concerns
* Production-ready patterns

At this stage, you can build:

* CRUD applications
* Dashboards
* Auth-backed systems
* Real SaaS backends

---

## Homework (Very Important)

1. Create a `users` collection in MongoDB
2. Add sample users
3. Fetch users in a Server Component
4. Add a POST API to insert users

---

## Next Lesson (Lesson 10 Preview)

We will cover:

* **Authentication in Next.js**
* Cookies & sessions
* Middleware
* Protected routes

When ready, reply exactly with:

> **Continue to Lesson 10**

We will proceed methodically.
