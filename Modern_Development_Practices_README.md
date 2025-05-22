# 🚀 Modern Development Practices

This document explains key modern development practices: **Test-Driven Development (TDD)** and **BFF/API-Based Separation of Concerns**, especially in the context of frameworks like Yii.

---

## ✅ 1. Test-Driven Development (TDD)

### 📘 What is TDD?

**Test-Driven Development (TDD)** is a software development process where tests are written before the actual code. The process follows a simple cycle:

1. **Write a test** that defines a desired function or improvement.
2. **Run the test** and watch it fail (since the feature doesn’t exist yet).
3. **Write the code** to make the test pass.
4. **Refactor the code** while keeping the test green (passing).

### 🧪 Example in Yii2 (PHPUnit):

```php
public function testUserSignup() {
    \$model = new SignupForm();
    \$model->email = 'test@example.com';
    \$model->password = '123456';

    \$this->assertTrue(\$model->signup());
}
```

### 🎯 Benefits of TDD

- Early bug detection
- Cleaner, modular code
- Safe refactoring
- Serves as documentation
- Increases developer confidence

---

## 🔄 2. BFF/API-Based Separation of Concerns
### 📘 What is BFF (Backend For Frontend)?

A BFF is a backend service layer customized to a specific frontend client (e.g., Web app, Mobile app). Instead of a single generic API, you create tailored backends for each client, so each frontend gets exactly what it needs — no more, no less.

---

### 🧠 Example Scenario

Imagine you have:

- A Web frontend built with React or Next.js
- A Mobile app
- A Backend API that talks to your database and other services

Instead of having the frontend talk directly to the generic backend API, you introduce a BFF layer for each frontend:

```
Frontend Web    ---> Web BFF    ---> Backend API / Database
Frontend Mobile ---> Mobile BFF ---> Backend API / Database
```

Each BFF formats, aggregates, or adapts data specifically for its frontend.

---

### 🔧 Simple BFF Example in Node.js (Express)

Let’s say the backend API has raw product data:

```json
// Backend API: GET /products
[
  { "id": 1, "name": "Product A", "price": 100, "internalCode": "X123" },
  { "id": 2, "name": "Product B", "price": 200, "internalCode": "Y456" }
]
```

### 1. Web BFF: Sends full product info, including price

```js
// web-bff.js
const express = require('express');
const fetch = require('node-fetch');
const app = express();

app.get('/products', async (req, res) => {
  const response = await fetch('http://backend-api/products');
  const products = await response.json();

  // Send all fields needed by web
  res.json(products);
});

app.listen(3001, () => console.log('Web BFF running on port 3001'));
```

### 2. Mobile BFF: Sends limited product info (no price), to reduce data usage

```js
// mobile-bff.js
const express = require('express');
const fetch = require('node-fetch');
const app = express();

app.get('/products', async (req, res) => {
  const response = await fetch('http://backend-api/products');
  const products = await response.json();

  // Map to only fields mobile needs
  const mobileProducts = products.map(p => ({
    id: p.id,
    name: p.name,
  }));

  res.json(mobileProducts);
});

app.listen(3002, () => console.log('Mobile BFF running on port 3002'));
```

---

## ❓ Why BFF?

- Web frontend needs full data, with price and internal codes for admin features.
- Mobile frontend only needs light data, so mobile BFF strips unnecessary fields to save bandwidth.

Each BFF can also handle authentication, caching, aggregation from multiple APIs, or special formatting for their clients.

---

### 🧪 How This Looks in Practice

| Client          | Calls         | Response Example |
|------------------|----------------|------------------|
| Web frontend     | GET /products  | `[{"id":1,"name":"Product A","price":100,"internalCode":"X123"}, ...]` |
| Mobile frontend  | GET /products  | `[{"id":1,"name":"Product A"}, {"id":2,"name":"Product B"}]` |

---

### 💡 When to Use BFF

Use BFF when:
- You have **multiple types of clients** (web, mobile, IoT)
- You want **clean separation** of logic and roles
- You need to **optimize** performance per client
- You plan to build a **scalable, modern API architecture**

---

## 📌 Final Notes

BFF isn’t always required for small projects, but it becomes very useful as you scale and diversify your frontends.

