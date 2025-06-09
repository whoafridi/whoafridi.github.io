---
layout: post
title: ইভেন্ট লুপ নিয়ে কথাবার্তা
subtitle: ইভেন্ট লুপ
tags: [javascript, interview]
permalink: /event-loop/
---

## 🔁 What is the Event Loop?

The **Event Loop** is the mechanism that allows JavaScript (which is **single-threaded**) to perform **non-blocking I/O operations** by **offloading tasks** like timers, fetch calls, etc., to the environment (e.g., browser or Node.js) and handling them asynchronously.

---

## 🧠 Core Concepts

### 1. 🧩 **Call Stack**

- Executes synchronous code.
- Follows **LIFO** (Last In, First Out).
- Only one operation runs at a time.

```js
function a() {
  console.log("A");
  b();
}
function b() {
  console.log("B");
}
a();
```

---

### 2. 🎡 **Event Loop**

- Constantly **monitors the call stack and queues**.
- When the call stack is empty, it pushes the next task (macro or micro) from the appropriate queue into the stack.

---

### 3. 🕒 **Macrotask Queue (Task Queue)**

Tasks here include:

- `setTimeout`
- `setInterval`
- `setImmediate` (Node.js)
- I/O tasks
- UI rendering

🧠 **Macrotasks are scheduled** after the current script and all microtasks complete.

---

### 4. ⚡ **Microtask Queue**

Tasks here include:

- `Promise.then`, `Promise.catch`, `Promise.finally`
- `queueMicrotask()`
- `MutationObserver`

🧠 **Microtasks are run immediately after the current task**, before any macrotask.

---

## 🧬 Advanced Example with Multiple Queues

```js
console.log("1");

setTimeout(() => {
  console.log("2");
  Promise.resolve().then(() => console.log("3"));
}, 0);

Promise.resolve().then(() => console.log("4"));

setTimeout(() => {
  console.log("5");
}, 0);

console.log("6");
```

### ✅ Execution Order:

```
1 (sync)
6 (sync)
4 (microtask)
2 (macrotask)
3 (microtask inside macrotask)
5 (macrotask)
```

### 🔚 Output:

```
1
6
4
2
3
5
```

---

---

## 🔁 **JavaScript Event Loop কী?**

**JavaScript একক থ্রেডে (single-threaded)** চলে — মানে একসাথে একটাই কাজ করতে পারে।

**তাহলে প্রশ্ন আসে:**  
👉 টাইমার, API call, ইউজারের ক্লিক ইভেন্ট — এগুলো একসাথে কিভাবে হয়?

এই সমস্যার সমাধান দেয় **Event Loop**।

---

## 🧠 বুঝতে হবে ৪টি জিনিস:

1. 🧱 Call Stack (সরাসরি যে কোড চলছে)
2. ⏱️ Macrotask Queue (বড় কাজের কিউ — `setTimeout`, `setInterval`)
3. ⚡ Microtask Queue (ছোট কাজের কিউ — `Promise.then`)
4. 🔁 Event Loop (যে সবকিছুর মধ্যে সমন্বয় করে)

---

## 🎬 বাস্তব উদাহরণ দিয়ে ব্যাখ্যা:

ধরো তুমি রান্নাঘরে একাই রান্না করছো।  
তুমি (👉 JavaScript) একসাথে একাধিক রান্না করতে পারো না।

তুমি যদি বলো:

> 🍳 “ডিম সিদ্ধ হোক, আমি meantime-এ সবজি কাটি।”

তাহলে তুমি ডিম সিদ্ধ হওয়ার জন্য টাইমার দিয়ে অন্য কাজ করতে পারো।  
এই টাইমার/ডিম-সিদ্ধ হওয়া = **Macrotask**  
সবজি কাটা (সরাসরি) = **Call Stack**

---

## 📦 উদাহরণ কোড ১:

```js
console.log("Start");

setTimeout(() => {
  console.log("Timeout");
}, 0);

Promise.resolve().then(() => {
  console.log("Promise");
});

console.log("End");
```

### 🪜 ব্যাখ্যা:

1. `Start` – সরাসরি চলে যাবে → Call Stack
2. `setTimeout(..., 0)` – Browser বলে: “এইটা ০ms পর Macrotask Queue-তে রাখি”
3. `Promise.then(...)` – Microtask Queue-তে যায়
4. `End` – সরাসরি চলে যাবে → Call Stack

তারপর,
👉 Event Loop দেখে যে Call Stack খালি, তখন:

- প্রথমে **Microtask Queue থেকে** → `Promise`
- তারপর **Macrotask Queue থেকে** → `Timeout`

### ✅ আউটপুট:

```
Start
End
Promise
Timeout
```

---

## 📊 সহজে মনে রাখার টিপস:

| ধরন        | কাজের উদাহরণ                       | কখন চলে                             |
| ---------- | ---------------------------------- | ----------------------------------- |
| Call Stack | `console.log()`, ফাংশন কল          | সঙ্গে সঙ্গে                         |
| Microtask  | `Promise.then()`, `queueMicrotask` | সব কাজ শেষে কিন্তু Macrotask-এর আগে |
| Macrotask  | `setTimeout()`, `setInterval()`    | সব Microtask শেষে                   |

---

## 🔄 Event Loop কী করে?

ধরা যাক একটা মেশিন:

```
[Call Stack] —> সব Sync কাজ
[Microtask Queue] —> ছোট কাজ (Promise)
[Macrotask Queue] —> টাইমার, ইভেন্ট

Event Loop:
  🔁 Call Stack খালি হলে → আগে Microtask Queue → তারপর Macrotask Queue
```

---

## 📌 উদাহরণ ২ (সব মিলে):

```js
console.log("1");

setTimeout(() => {
  console.log("2");
  Promise.resolve().then(() => console.log("3"));
}, 0);

Promise.resolve().then(() => console.log("4"));

console.log("5");
```

### 🪜 ব্যাখ্যা ধাপে ধাপে:

1. `1` → সরাসরি
2. `setTimeout` → Macrotask Queue-তে যাবে
3. `Promise.then("4")` → Microtask Queue
4. `5` → সরাসরি

---

🌀 তারপর:

- Microtask: `4`
- Macrotask: `2`, তারপর `3` (as it's a Promise inside timeout)

### ✅ আউটপুট:

```
1
5
4
2
3
```

---

## 🔚 সংক্ষেপে:

- **Call Stack** – সরাসরি কোড
- **Microtask Queue** – `Promise.then()`, আগে চলে
- **Macrotask Queue** – `setTimeout`, পরে চলে
- **Event Loop** – সব ঠিকঠাক টাইমিংয়ে চালায়

---

## 📊 Visual Diagram of Event Loop

```
                        🔁 JavaScript Event Loop

         ┌─────────────────────┐
         │     Call Stack      │ 👈 Sync code execution
         └─────────────────────┘
                    ▲
                    │
      ┌─────────────┴─────────────┐
      │                           │
┌──────────────┐         ┌──────────────────┐
│ Microtask Q  │         │  Macrotask Q     │
│ (Promises)   │         │ (setTimeout, etc)│
└──────────────┘         └──────────────────┘
      ▲                           ▲
      └───────┐        ┌──────────┘
              │        │
           ┌───────────────┐
           │  Event Loop   │ 🔁 প্রতিনিয়ত ঘুরে ঘুরে দেখে:
           └───────────────┘     1️⃣ Call Stack খালি?
                                 2️⃣ আগে Microtask
                                 3️⃣ তারপর Macrotask
```

📝 মনে রাখো:

- Microtasks সবসময় Macrotask-এর আগে চলে।
- `async/await`, `Promise.then()` — Microtask Queue-তে যায়।

## 🧪 async/await Example with Event Loop

```js
console.log("1");

setTimeout(() => {
  console.log("2");
}, 0);

async function test() {
  console.log("3");
  await Promise.resolve();
  console.log("4");
}

test();

console.log("5");
```

---

### 🪜 ব্যাখ্যা:

1. `1` → sync
2. `setTimeout()` → Macrotask Queue-তে
3. `3` → sync inside `test()` function
4. `await Promise.resolve()` → Microtask-এ `console.log("4")` রাখে
5. `5` → sync

Then:

- Microtask: `4`
- Macrotask: `2`

### ✅ Output:

```
1
3
5
4
2
```

ইন্টারভিউ প্রস্তুতির অংশ হিসেবে লেখা তাই খুব একটা গুছিয়ে লেখা হয় নি। অনেক জায়াগায় ইংরেজি বাংলা মিক্স করা হয়েছে। তারপরও যদি কেউ শর্ট টাইমে গাইড নিতে চায় নিতে পারে।
