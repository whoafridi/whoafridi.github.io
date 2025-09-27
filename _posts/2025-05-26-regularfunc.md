---
layout: post
title: Regular Function এবং Arrow Function এর মধ্যে পার্থক্যগুলো খুঁটিনাটি বিশ্লেষণ
subtitle: ডিটেইলস গাইড
tags: [javascript, interview]
permalink: /regular-arrow-function/
---

**Regular Function** এবং **Arrow Function** এর মধ্যে পার্থক্যগুলো **বাংলায়** ৭টি পয়েন্টে বিস্তারিতভাবে ব্যাখ্যা করা হলো:

---

### ✅ ১. **Syntax (গঠন / সিনট্যাক্স)**

🔹 **Regular Function:**

```javascript
function add(a, b) {
  return a + b;
}
```

🔹 **Arrow Function:**

```javascript
const add = (a, b) => a + b;
```

📝 **ব্যাখ্যা:**
Arrow function ছোট ও কমপ্যাক্ট লেখার সুবিধা দেয়। `function` কীওয়ার্ড লাগে না, এবং যদি শুধু `return` থাকে, তাহলে `{}` না দিলেও চলে।

---

### ✅ ২. **Arguments binding (arguments অবজেক্ট ব্যবহার)**

🔹 **Regular Function:**
`arguments` অবজেক্ট সাপোর্ট করে — অর্থাৎ আপনি ফাংশনে কতগুলো আর্গুমেন্ট এসেছে তা access করতে পারবেন।

```javascript
function showArgs() {
  console.log(arguments);
}
showArgs(1, 2, 3); // Output: [1, 2, 3]
```

🔹 **Arrow Function:**
Arrow function নিজস্ব `arguments` অবজেক্ট পায় না।

```javascript
const showArgs = () => {
  console.log(arguments); // ❌ Error: arguments is not defined
};
```

📝 **ব্যাখ্যা:**
Arrow function-এর `arguments` থাকে না, বরং তার surrounding (outer) function থেকে নেয়।

---

### ✅ ৩. **this কিভাবে কাজ করে**

🔹 **Regular Function:**
`this` ডাইনামিক — মানে, যেখানে ফাংশন কল করা হয়, সেটার উপর নির্ভর করে `this` কাকে রেফার করবে।

```javascript
const obj = {
  name: "Regular",
  sayName: function () {
    console.log(this.name); // this = obj
  },
};
obj.sayName(); // Output: Regular
```

🔹 **Arrow Function:**
Arrow function-এর `this` ফিক্সড — এটি তার enclosing (বাইরের) scope-এর `this` ব্যবহার করে।

```javascript
const obj = {
  name: "Arrow",
  sayName: () => {
    console.log(this.name); // this ≠ obj; this = window/global
  },
};
obj.sayName(); // Output: undefined
```

📝 **ব্যাখ্যা:**
Arrow function এর মধ্যে `this` bind হয় না, এটি তার parent scope থেকে `this` গ্রহণ করে।

---

### ✅ ৪. **new কিওয়ার্ড দিয়ে ইনস্ট্যান্স তৈরি**

🔹 **Regular Function:**
Regular function কে constructor হিসেবে ব্যবহার করা যায় (`new` দিয়ে object তৈরি করা যায়)।

```javascript
function Person(name) {
  this.name = name;
}
const p = new Person("Hasan");
console.log(p.name); // Hasan
```

🔹 **Arrow Function:**
Arrow function constructor হিসেবে ব্যবহার করা যায় না।

```javascript
const Person = (name) => {
  this.name = name;
};
const p = new Person("Hasan"); // ❌ Error: Person is not a constructor
```

📝 **ব্যাখ্যা:**
Arrow function কে কখনো constructor হিসেবে ব্যবহার করা যায় না।

---

### ✅ ৫. **No duplicate named parameters (একই নামের প্যারামিটার)**

🔹 **Regular Function:**
Non-strict mode-এ Regular function এ একই নামে একাধিক প্যারামিটার ব্যবহার করা যায়।

```javascript
function demo(a, a) {
  console.log(a); // Latest value
}
demo(1, 2); // Output: 2
```

🔹 **Arrow Function:**
Arrow function-এ একই নামে একাধিক প্যারামিটার দেওয়া যায় না — Syntax error দেয়।

```javascript
const demo = (a, a) => {
  console.log(a);
}; // ❌ SyntaxError
```

📝 **ব্যাখ্যা:**
Arrow function strict mode অনুসরণ করে এবং ডুপ্লিকেট প্যারামিটার গ্রহণ করে না।

---

### ✅ ৬. **Function Hoisting (উপরের দিকে তুলে আনা)**

🔹 **Regular Function:**
Hoisting সাপোর্ট করে — ফাংশন ডিফাইন করার আগে কল করলেও কাজ করে।

```javascript
greet(); // Output: Hello

function greet() {
  console.log("Hello");
}
```

🔹 **Arrow Function:**
Hoisting সাপোর্ট করে না — ডিফাইন করার আগে কল করলে Error হয়।

```javascript
greet(); // ❌ TypeError

const greet = () => {
  console.log("Hello");
};
```

📝 **ব্যাখ্যা:**
Arrow function আসলে ভেরিয়েবল হিসেবে ডিফাইন হয়, তাই `let`/`const` এর মতো hoisting হয় না।

---

### ✅ ৭. **Methods হিসেবে ব্যবহার**

🔹 **Regular Function:**
Object এর মেথড হিসেবে খুব ভালো কাজ করে, কারণ এটি `this` ঠিকভাবে ধরে রাখতে পারে।

```javascript
const obj = {
  name: "Regular",
  show: function () {
    console.log(this.name); // Output: Regular
  },
};
obj.show();
```

🔹 **Arrow Function:**
Arrow function object মেথড হিসেবে ব্যবহারে সাবধানতা প্রয়োজন, কারণ এটি `this` bind করে না।

```javascript
const obj = {
  name: "Arrow",
  show: () => {
    console.log(this.name); // undefined
  },
};
obj.show();
```

📝 **ব্যাখ্যা:**
Object এর মেথড হিসেবে `arrow function` সঠিকভাবে `this` ব্যাবহার করতে পারে না।

---

### 🔚 **সারাংশ টেবিল আকারে:**

| বৈশিষ্ট্য                | Regular Function                | Arrow Function  |
| ------------------------ | ------------------------------- | --------------- |
| **Syntax**               | function keyword দরকার          | ছোট ও কমপ্যাক্ট |
| **arguments**            | পাওয়া যায়                     | পাওয়া যায় না  |
| **this**                 | Dynamic                         | Lexical (fixed) |
| **new keyword**          | Constructor হিসেবে ব্যবহারযোগ্য | নয়             |
| **Duplicate parameters** | non-strict mode এ চলে           | চলে না          |
| **Hoisting**             | হয়                              | হয় না           |
| **Object methods**       | ভালো কাজ করে                    | this হারায়      |

---
