---
layout: post
title: জাভাস্ক্রিপ্টের স্কোপ (Scope)
subtitle: scope
tags: [javascript, interview]
permalink: /javascript-scope/
---

### জাভাস্ক্রিপ্ট স্কোপ (Scope)

জাভাস্ক্রিপ্টে **স্কোপ** একটি ধারণা যা নির্ধারণ করে কোন ভ্যারিয়েবল বা ফাংশন কোথায় এক্সেসযোগ্য বা দৃশ্যমান থাকবে। স্কোপের মাধ্যমে একটি ভ্যারিয়েবল বা ফাংশনের লাইফটাইম এবং **অ্যাক্সেসিবিলিটি** নিয়ন্ত্রণ করা হয়।

জাভাস্ক্রিপ্টে মূলত তিনটি ধরনের স্কোপ রয়েছে:

1. **গ্লোবাল স্কোপ (Global Scope)**
2. **ফাংশন স্কোপ (Function Scope)**
3. **ব্লক স্কোপ (Block Scope)**

এগুলোকে একটু বিস্তারিতভাবে আলোচনা করি:

---

### ১. **গ্লোবাল স্কোপ (Global Scope)**

গ্লোবাল স্কোপ হচ্ছে সেই স্কোপ যেখানে কোনো ভ্যারিয়েবল বা ফাংশন যদি গ্লোবাল লেভেলে ডিক্লেয়ার করা হয়, তবে সে ভ্যারিয়েবল বা ফাংশন পুরো স্ক্রিপ্টের মধ্যে, যে কোনো জায়গায় এক্সেস করা যাবে।

যদি আপনি গ্লোবাল স্কোপে একটি ভ্যারিয়েবল ডিক্লেয়ার করেন, তা আপনার পুরো কোডে অ্যাক্সেসযোগ্য হবে।

#### উদাহরণ:

```javascript
var globalVar = "I am global";

function printGlobal() {
  console.log(globalVar); // গ্লোবাল ভ্যারিয়েবল এক্সেস করা হচ্ছে
}

printGlobal(); // Output: I am global
console.log(globalVar); // Output: I am global
```

এখানে, **`globalVar`** গ্লোবাল স্কোপে ডিক্লেয়ার করা হয়েছে, তাই এটি ফাংশন **`printGlobal()`**-এর ভিতরেও এক্সেস করা যেতে পারে এবং বাইরে থেকেও এক্সেসযোগ্য।

---

### ২. **ফাংশন স্কোপ (Function Scope)**

ফাংশন স্কোপ হলো সেই স্কোপ যা একটি ফাংশনের ভিতরে ডিক্লেয়ার হওয়া ভ্যারিয়েবলগুলোর জন্য প্রযোজ্য। এই ভ্যারিয়েবলগুলো শুধুমাত্র ফাংশনের ভিতরেই এক্সেস করা যায়।

যতটুকু স্কোপে একটি ভ্যারিয়েবল ডিক্লেয়ার করা হয়েছে, সেই স্কোপের বাইরে সেই ভ্যারিয়েবল অ্যাক্সেস করা সম্ভব নয়।

#### উদাহরণ:

```javascript
function myFunction() {
  var localVar = "I am inside a function"; // ফাংশন স্কোপে
  console.log(localVar); // Output: I am inside a function
}

myFunction();
console.log(localVar); // Error: localVar is not defined
```

এখানে, **`localVar`** শুধুমাত্র **`myFunction()`** এর ভিতরে এক্সেসযোগ্য, ফাংশনের বাইরে এটি অ্যাক্সেস করা সম্ভব নয়।

---

### ৩. **ব্লক স্কোপ (Block Scope)**

জাভাস্ক্রিপ্টের ES6 সংস্করণ থেকে **`let`** এবং **`const`** এর মাধ্যমে ব্লক স্কোপ এসেছে। ব্লক স্কোপ এমন একটি স্কোপ যেখানে একটি কোড ব্লক (যেমন `if`, `for`, `while` লুপ) ভিতরে ডিক্লেয়ার করা ভ্যারিয়েবল শুধুমাত্র ঐ ব্লকের ভিতরে অ্যাক্সেসযোগ্য থাকে।

**`var`** এর ক্ষেত্রে ব্লক স্কোপ থাকে না, কিন্তু **`let`** এবং **`const`** এর ক্ষেত্রে ব্লক স্কোপ কার্যকরী।

#### উদাহরণ:

```javascript
if (true) {
  let blockVar = "I am inside a block";
  const blockConst = "I am a constant inside a block";
  var blockVar2 = "I am with var"; // var does not have block scope
  console.log(blockVar); // Output: I am inside a block
}

console.log(blockVar); // Error: blockVar is not defined
console.log(blockConst); // Error: blockConst is not defined
console.log(blockVar2); // Output: I am with var
```

এখানে, **`blockVar`** এবং **`blockConst`** ব্লক স্কোপে ডিক্লেয়ার করা হয়েছে, ফলে এগুলো **`if`** ব্লকের বাইরে এক্সেস করা যাবে না। কিন্তু **`blockVar2`** `var` দিয়ে ডিক্লেয়ার করা হয়েছে, তাই এটি গ্লোবাল স্কোপে চলে আসে এবং বাইরের কোডেও এক্সেসযোগ্য হয়।

---

### স্কোপ চেইন (Scope Chain)

যখন কোনো ভ্যারিয়েবল এক্সেস করা হয়, তখন জাভাস্ক্রিপ্ট প্রথমে সেই ভ্যারিয়েবলটি বর্তমান স্কোপে খোঁজে। যদি ভ্যারিয়েবলটি বর্তমান স্কোপে না পাওয়া যায়, তাহলে এটি তার প্যারেন্ট স্কোপে খোঁজা হয়, এবং এভাবে একে একে উচ্চতর স্কোপগুলোতে খোঁজ চলতে থাকে যতক্ষণ না গ্লোবাল স্কোপে গিয়ে ভ্যারিয়েবলটি পাওয়া যায় বা **`undefined`** রিটার্ন হয়।

এটি **স্কোপ চেইন (Scope Chain)** নামে পরিচিত।

#### উদাহরণ:

```javascript
var globalVar = "I am global";

function outerFunction() {
  var outerVar = "I am outer";

  function innerFunction() {
    var innerVar = "I am inner";
    console.log(innerVar); // "I am inner"
    console.log(outerVar); // "I am outer"
    console.log(globalVar); // "I am global"
  }

  innerFunction();
}

outerFunction();
```

এখানে, **`innerFunction`** প্রথমে নিজ স্কোপে, তারপর প্যারেন্ট ফাংশন **`outerFunction`** এর স্কোপে এবং তারপর গ্লোবাল স্কোপে **`globalVar`** এক্সেস করতে সক্ষম।

---

### ফাংশন স্কোপের সাথে ব্লক স্কোপের পার্থক্য

ফাংশন স্কোপে **`var`** ভ্যারিয়েবলটি একটি পুরো ফাংশনকে কভার করে, যেখানে ব্লক স্কোপে **`let`** এবং **`const`** ভ্যারিয়েবলগুলোকে শুধুমাত্র ওই নির্দিষ্ট ব্লক (যেমন `if`, `for` লুপ) এর মধ্যে সীমাবদ্ধ থাকে।

#### উদাহরণ:

```javascript
function testVar() {
  if (true) {
    var x = 10; // var has function scope
  }
  console.log(x); // Output: 10
}

testVar();

function testLet() {
  if (true) {
    let y = 20; // let has block scope
  }
  console.log(y); // Error: y is not defined
}

testLet();
```

এখানে, **`var x`** ফাংশন স্কোপে রয়েছে, তাই এটি ফাংশনের বাইরে থেকেও অ্যাক্সেসযোগ্য। কিন্তু **`let y`** ব্লক স্কোপে রয়েছে, ফলে এটি ব্লক (যেমন `if` স্টেটমেন্ট) এর বাইরে অ্যাক্সেসযোগ্য নয়।

---

### স্কোপের সাথে **`this`** কীভাবে কাজ করে?

জাভাস্ক্রিপ্টে **`this`** এর মান স্কোপের উপর নির্ভর করে। **`this`** গ্লোবাল স্কোপে গ্লোবাল অবজেক্টকে নির্দেশ করে (যেমন ব্রাউজারে `window`), ফাংশনে এটি সেই ফাংশনের কন্টেক্সট বা কাকে কল করা হয়েছে তার উপর নির্ভর করে, এবং ক্লাসে এটি সেই ক্লাসের ইনস্ট্যান্সকে নির্দেশ করে।

#### উদাহরণ:

```javascript
var a = "global";

function exampleFunction() {
  var a = "function scope";
  console.log(this.a); // In non-strict mode, it will refer to the global object (window in browsers)
}

exampleFunction(); // Output: undefined (if strict mode is not enabled)
```

এখানে, **`this.a`** গ্লোবাল স্কোপে **`a`** কে নির্দেশ করবে, যদি `a` গ্লোবাল স্কোপে থাকে, তবে এর মান পাওয়া যাবে।

---

### সারাংশ

- **গ্লোবাল স্কোপ**: পুরো কোডের মধ্যে এক্সেসযোগ্য, যদি ভ্যারিয়েবল বা ফাংশন গ্লোবাল স্কোপে ডিক্লেয়ার করা হয়।
- **ফাংশন স্কোপ**: একটি ফাংশনের ভিতরে ডিক্লেয়ার করা ভ্যারিয়েবল শুধুমাত্র ঐ ফাংশনের ভিতরে এক্সেসযোগ্য।
- **ব্লক স্কোপ**: **`let`** ও **`const`** দিয়ে ডিক্লেয়ার করা ভ্যারিয়েবল শুধুমাত্র ঐ ব্লকের ভিতরে এক্সেসযোগ্য।
- **স্কোপ চেইন**: জাভাস্ক্রিপ্টে একাধিক স্কোপ থাকে, এবং যখন একটি ভ্যারিয়েবল এক্সেস করা হয়, তখন এটি স্কোপ চেইনের মাধ্যমে খোঁজা হয়।
