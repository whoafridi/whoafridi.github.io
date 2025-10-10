---
layout: post
title: Reconciliation (রিকনসাইলিয়েশন) কী?
subtitle: ডিটেইলস গাইড
tags: [javascript, interview, react]
permalink: /reconciliation/
---

## ✅ **Reconciliation (রিকনসাইলিয়েশন) কী?**

**Reconciliation** হলো React-এর এমন একটি প্রক্রিয়া, যার মাধ্যমে React বোঝে কোন অংশটা UI-তে পরিবর্তন হয়েছে এবং শুধুমাত্র সেই অংশটুকুই Real DOM-এ আপডেট করে।

---

### 🔍 সহজভাবে বললে:

> **রিকনসাইলিয়েশন** হচ্ছে React-এর সেই কৌশল, যার মাধ্যমে পুরনো ও নতুন Virtual DOM তুলনা করে শুধুমাত্র যেটুকু পরিবর্তন হয়েছে, সেটুকু Real DOM-এ প্রতিফলিত করে।

---

## 🧠 কিভাবে কাজ করে:

1. যখন কোনও **state** বা **props** পরিবর্তন হয়,

   - React নতুন একটি **Virtual DOM** তৈরি করে।
   - তারপর পুরনো Virtual DOM-এর সঙ্গে এটি **তুলনা (diffing)** করে।

2. যে অংশে পরিবর্তন হয়েছে, শুধু **সেই অংশটি** React Real DOM-এ আপডেট করে।
3. ফলে পুরো UI রেন্ডার না করে, **অপটিমাইজড ও দ্রুত আপডেট** হয়।

---

### 🏗 উদাহরণ:

আপনার UI:

```jsx
<ul>
  <li>Apple</li>
  <li>Banana</li>
  <li>Mango</li>
</ul>
```

আপনি এটি পরিবর্তন করলেন:

```jsx
<ul>
  <li>Apple</li>
  <li>Banana</li>
  <li>Orange</li> {/* শুধু Mango এর পরিবর্তে Orange */}
</ul>
```

➡️ React পুরো তালিকাটি (list) আবার রেন্ডার করবে না।
➡️ এটা বুঝে যাবে যে শুধু **তৃতীয় `<li>` পরিবর্তন হয়েছে**, তাই শুধু সেটুকুই আপডেট করবে।
এই প্রক্রিয়াটিকেই বলা হয় **Reconciliation**।

---

## 📌 সারাংশ:

| বিষয়               | ব্যাখ্যা                                                                                                                  |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------- |
| **Reconciliation** | React-এর সেই পদ্ধতি যা Virtual DOM-এর পুরোনো ও নতুন সংস্করণ তুলনা করে এবং শুধুমাত্র প্রয়োজনীয় অংশ Real DOM-এ আপডেট করে। |
| **উদ্দেশ্য**       | পারফরম্যান্স উন্নত করা ও অপ্রয়োজনীয় DOM আপডেট এড়ানো।                                                                   |

### ✅ **Definition of Reconciliation in React:**

**Reconciliation** is the process that **React uses to update the Real DOM** efficiently when your app's state or props change.

---

### 🔍 In Simple Terms:

> **Reconciliation is the way React figures out what has changed in the UI and updates only those parts, instead of re-rendering the entire page.**

---

### 🧠 How It Works:

1. When a component’s **state or props change**, React:

   - Creates a new **Virtual DOM tree**.
   - Compares it to the previous Virtual DOM (this is called **diffing**).

2. React then **identifies the differences** between the old and new trees.
3. Only the **changed elements** are updated in the **Real DOM**.

   - This is done to **improve performance** and reduce unnecessary updates.

---

### 🏗 Example:

Let's say your UI looks like this:

```jsx
<ul>
  <li>Apple</li>
  <li>Banana</li>
  <li>Mango</li>
</ul>
```

If you update it to:

```jsx
<ul>
  <li>Apple</li>
  <li>Banana</li>
  <li>Orange</li> {/* changed from Mango to Orange */}
</ul>
```

**React will not re-render the whole list.**
Through **reconciliation**, it will detect that only the last `<li>` has changed and will update **only that part** in the real DOM.

---

### 🧩 Why It Matters:

- Makes React **fast and efficient**.
- Reduces **DOM manipulation** (which is slow).
- Helps create **smooth UI updates**.

---

### 📌 Summary:

| Term               | Meaning                                                                                                                  |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| **Reconciliation** | The process React uses to compare the new Virtual DOM with the old one and update only what's necessary in the Real DOM. |
| **Goal**           | Efficient and minimal UI updates.                                                                                        |
