---
layout: post
title: ইনস্টল ছাড়াই পাইথন 
tags: [python, programming]
permalink: /py/
---

শিরোনাম দেখে বুঝে ফেলেছেন আজকে আমরা কোন কিছু `install` করা ছাড়াই পাইথন দিয়ে প্রোগ্রামিং শিখব ।
প্রথমেই `https://repl.it` ওয়েবসাইট এ যান । ল্যাঙ্গুয়েজ হিসেবে পাইথন ৩ সিলেক্ট করুন । 

<p align="center">
<img src="https://user-images.githubusercontent.com/35966401/50602178-d4f80d00-0ee0-11e9-9124-393bf65948fb.png" alt="h">
</p>

ওয়েবসাইটের হোমে দেখতে এরকম ।
কাজ শেষ, এবার কোড করার পালা । চলুন শুরু করা যাক ।
প্রথমেই Hello, world ! প্রোগ্রাম রান করে দেখা যাক ।
```sh
print("Hello, world !")
```
অনেকটা পানির মত তাই না ? 
আরও কিছু কাজ দেখে নেই ।
# *Variable* 
প্রথমে একটু  `variable` দিয়ে শুরু করি । অন্যান্য ল্যাঙ্গুয়েজের মত পাইথনে কিভাবে  `variable`  ডিক্লেয়ার করতে হয় তা দেখে নেয়া যাক ।

উদাহরণ ঃ 
```sh 
name = 'X'
```
আমরা এখানে একটা  `variable`  ডিক্লেয়ার করলাম `name` নামে । যার একটা ভ্যালু দিলাম । কত সোজা না এভাবেই শিখে ফেললাম কিভাবে পাইথনে `variable` লিখতে হয় । আরও কিছু দেখে নেই ঃ
```py
age = 22
amount = 200.50
lan = 'Python'
```

# *Condition*
নামেই বুঝা যাচ্ছে কোন শর্ত  জুড়ে দিলেই হয়ে গেল । একটা উদাহরণ দিলে আরও বুঝা যাবে আশা করতেসি ।
আর যেটা না বললেই নয় পাইথনে ` if - else ` ব্লকে ` : ` দিতে হয় । এটা ছাড়া আর তেমন পার্থক্য নেই অন্যান্য ল্যাঙ্গুয়েজের সাথে ।
উদাহরণ ঃ
```sh
age = 17
if age < 18:
  print('Not eligible')
else:
  print('Eligible')
```
সদ্য শেষ হওয়া নির্বাচন থেকে উদাহরণটা নিলাম । বুঝতেই পারছেন আঠারোর কম হলে ভোট দিতে পারবে না ।

*আরেকটি বিষয় লক্ষণীয় , পাইথনে `indentation` না দিলে এররর দেয় । উদাহরন দিলে বুঝতে সুবিধা হবে ।*
```py
if age < 18:
print()
```
![image](https://user-images.githubusercontent.com/35966401/50645455-ccb1d780-0f9d-11e9-8289-d6661c771b58.png)

*এখন কথা আসলো এটা আসলে কি ? if-else ব্লকের পরে নিউ লাইন একটু দূরে সঃরে গেল (আগের উদাহরন)
প্রগ্রাম্মিঙ্গের ভাষায় এটাই indentation ।*

*আর গভীরে গেলাম না । উপরের উদাহরণগুলো চেষ্টা করুন । আল্লাহ হাফেয ।*
