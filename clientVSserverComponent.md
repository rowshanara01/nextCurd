# 🧩 Next.js Client & Server Components Example

This project demonstrates how to **separate Client and Server Components** in a **Next.js App Router** project, while also showing how to **fetch data from an API** on the server side and pass it to the client component.

---

## 📦 Features

- ✅ **Server Component** fetches data from an API using `fetch()`
- ✅ **Client Component** receives props and handles interactivity
- ✅ **App Router API Route** simulates backend data
- ✅ Clear separation of concerns between static and dynamic behavior

---

## 📁 Project Structure

```
nextjs-client-server-components/
├── app/
│   ├── page.tsx               # Home page entry point
│   ├── server-component.tsx   # Server-side rendering & data fetch
│   └── client-component.tsx   # Interactive component with 'use client'
├── app/api/data/route.ts      # API endpoint that returns JSON data
```

---

## 🧠 Component Roles

### 🔵 Server Component (`app/server-component.tsx`)
- Runs only on the server.
- Fetches data from the API: `/api/data`.
- Passes the data as props to the client component.

### 🟢 Client Component (`app/client-component.tsx`)
- Marked with `"use client"` directive.
- Receives `items` as props.
- Allows the user to interact (select an item) using `useState`.

---

## 🚀 Getting Started

### 1. Clone the repo

```bash
git clone https://github.com/your-username/nextjs-client-server-components.git
cd nextjs-client-server-components
```

### 2. Install dependencies

```bash
npm install
```

### 3. Run the development server

```bash
npm run dev
```

Visit `http://localhost:3000` in your browser.

---

## 🛠 Example Code Snippets

### 🔌 API Route (`/api/data`)
```ts
// app/api/data/route.ts

import { NextResponse } from 'next/server';

export async function GET() {
  const mockData = {
    items: ['Apple', 'Banana', 'Cherry', 'Date'],
  };
  return NextResponse.json(mockData);
}
```

---

### 🧠 Server Component
```tsx
// app/server-component.tsx

import ClientComponent from "./client-component";

export default async function ServerComponent() {
  const res = await fetch("http://localhost:3000/api/data", {
    cache: "no-store",
  });
  const data = await res.json();

  return (
    <div>
      <h1>Server Component</h1>
      <pre>{JSON.stringify(data, null, 2)}</pre>
      <ClientComponent items={data.items} />
    </div>
  );
}
```

---

### 🖱️ Client Component
```tsx
// app/client-component.tsx

'use client';

import { useState } from 'react';

export default function ClientComponent({ items }) {
  const [selected, setSelected] = useState(null);

  return (
    <div>
      <h2>Client Component</h2>
      {items.map((item, idx) => (
        <button key={idx} onClick={() => setSelected(item)}>
          {item}
        </button>
      ))}
      {selected && <p>You selected: {selected}</p>}
    </div>
  );
}
```

---

## 📚 Learn More

- [Next.js App Router Docs](https://nextjs.org/docs/app/building-your-application/routing)
- [Client vs Server Components](https://nextjs.org/docs/app/building-your-application/rendering/composition-patterns)

---

## 📄 License

This project is open-sourced under the MIT License.

---

### ✍️ Created by [Your Name](https://github.com/your-username)
