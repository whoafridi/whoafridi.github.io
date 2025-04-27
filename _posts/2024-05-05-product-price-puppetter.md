---
layout: post
title: Puppeteer দিয়ে ওয়েব স্ক্র্যাপিং পর্ব ২
subtitle: ওয়েব স্ক্র্যাপিং পর্ব ২
# cover-img: /img/puppeteer.webp
thumbnail-img: /img/puppeteer.webp
share-img: /img/puppeteer.webp
tags: [puppeteer, web scraping, web automation, javascript]
permalink: /product-scraping-puppeteer/
---

আসসালামুআলাইকুম। আশা করি সবাই ভালো আছেন। আজকের দ্বিতীয় পর্ব আমরা ই-কমার্স এর প্রোডাক্ট এর ডিটেলস (প্রাইস, নাম) বের করা নিয়ে আলোচনা করব । প্রোডাক্ট এর ডিটেলস বের করার জন্যে আমরা [রায়ান`স](https://www.ryans.com) ওয়েবসাইটে যাবো এবং সেখান থেকে আমাদের প্রয়োজনীয় জিনিসপত্র বের করব ।

আমরা পূর্ববর্তী প্রজেক্টে নতুন একটি ফাইল খুলবো যার মধ্যে আমরা ই-কমার্সের প্রোডাক্ট ডিটেলস এর যাবতীয় জিনিসপত্র লিখব। আমরা নতুন একটি ফোল্ডারে নতুন করে সবকিছু করিনি তার কারণ হলো প্রতিবার Puppeteer ইনস্টলেশনে নতুন করে সেটআপ তৈরি হয় যা খুবই সময়ের সাপেক্ষ বিষয় । যদি কারোর প্রজেক্ট এর প্রয়োজনে নতুন করে তৈরি করা লাগে তাহলে আপনারা তা করতে পারেন কিন্তু আমাদের লার্নিং পারপাসে বারবার ইনস্টলেশন এর খুব বেশি প্রয়োজন নেই এবং বারবার ইনস্টলেশনে আমাদের শেখার থেকে অন্যান্য জিনিসপত্রে মনোযোগ বেশি চলে যাবে ।

[ বিশেষ দ্রষ্টব্য : এবার আমরা কিছু জিনিস একটু ইংরেজিতে লিখবো যাতে করে জিনিসপত্র বুঝতে সুবিধা হয় এবং অনেক ক্ষেত্রে এ ধরনের জিনিসপত্রের বঙ্গানুবাদ খুব বেশি পছন্দ সই হয় না ]

1. কমান্ড লাইন ইন্টারফেস/টার্মিনাল খুলতে হবে

2. প্রথমেই আমরা যেখানে আছি সেখান থেকে ডিরেক্টরিতে পরিবর্তন করে Puppeteer প্রজেক্ট ডিরেক্টরীতে যেতে হবে

```bash
cd puppeteer-project
```

3. VSCode এর puppeteer প্রজেক্ট ডিরেক্টরি নতুন একটি ফাইল product_details.js খুলতে হবে

4. ফাইলটিতে নিম্নলিখিত কোডগুলো কপি করে পেস্ট করতে হবে

```javascript
import puppeteer from "puppeteer";
import fs from "fs";

(async () => {
  const allProductsData = [];
  const browser = await puppeteer.launch({
    headless: false,
    defaultViewport: { width: 1110, height: 1080 },
    userDataDir: "/temp",
  });
  const page = await browser.newPage();

  const searchTerm = `dell monitor 24 inch`;

  try {
    // ওয়েবসাইটে যান
    await page.goto("https://www.ryans.com", {
      waitUntil: "networkidle2",
      timeout: 600000,
    });
    // অনুসন্ধান পদ্ধতিতে লিখুন
    await page.type("#search", searchTerm);
    // অনুসন্ধান বাটনে ক্লিক করুন
    await page.click('button[type="submit"]');
    // সমস্ত পণ্যের কার্ড লোড হওয়ার জন্য অপেক্ষা করুন
    await page.waitForSelector(".card.h-100");

    // সমস্ত পণ্যের তথ্য উপস্থাপন করুন
    const allProducts = await page.evaluate(() => {
      // সমস্ত পণ্যের কার্ড নির্বাচন করুন
      const products = [...document.querySelectorAll(".card.h-100")];
      // প্রতিটি পণ্যের উপর ম্যাপিং করুন যাতে প্রাসঙ্গিক তথ্য প্রাপ্ত হয়
      return products.map((product, index) => {
        const id = index + 1;
        // পণ্যের URL প্রাপ্ত করুন
        const urlElement = product.querySelector(".image-box a");
        const url = urlElement ? urlElement.href : "N/A";
        // পণ্যের ছবি প্রাপ্ত করুন
        const imgElement = product.querySelector(".image-box img");
        const feature_image = imgElement ? imgElement.src : "N/A";
        return { id, url, feature_image };
      });
    });

    // প্রতিটি পণ্য থেকে বিস্তারিত তথ্য স্ক্রেপ করার জন্য লুপ চালান
    for (const product of allProducts) {
      const productPage = await browser.newPage();
      // পণ্যের পৃষ্ঠায় যান
      await productPage.goto(product.url, {
        waitUntil: "networkidle2",
        timeout: 600000,
      });

      // বিস্তারিত পণ্যের তথ্য স্ক্রেপ করুন
      const productDescription = await scrapeProductData(productPage);
      productPage.close();
      // সাধারণ পণ্যের ডেটা সহ বিস্তারিত তথ্য যোগ করুন
      allProductsData.push({ ...product, ...productDescription });
    }
    // স্ক্রেপ করা পণ্যের সংখ্যা লগ করুন
    console.log(allProductsData.length);
    // স্ক্রেপ করা ডেটা জেসন ফাইলে লেখো
    fs.writeFileSync(
      `./products_${searchTerm}.json`,
      JSON.stringify(allProductsData, null, 2)
    );
  } catch (error) {
    // ত্রুটি হ্যান্ডেল করুন
    console.error("Error scraping ryans:", error);
  } finally {
    // ব্রাউজার বন্ধ করুন
    await browser.close();
  }
})();
```

```javascript
const scrapeProductData = async (productPage) => {
  // পণ্যের তথ্য স্ক্রেপ করুন
  const productData = await productPage.evaluate(() => {
    // পণ্যের শিরোনামের উপাদান নির্বাচন করুন
    const producttitleElement = document.querySelector(
      ".product_content.h-100 h1"
    );
    const product_title = producttitleElement
      ? producttitleElement.textContent.trim().replace(/\s{2,}/g, " ")
      : "N/A";

    // বিষয়বস্তু উপাদান নির্বাচন করুন
    const contentElement = document.querySelector(".product_content.h-100");
    const product_content = contentElement
      ? contentElement.textContent.trim().replace(/\s{2,}/g, " ")
      : "N/A";

    // পণ্যের বিবরণের উপাদান নির্বাচন করুন
    const productDetailsElement = document.querySelector(
      ".card-body.details-tab.seo-footer"
    );
    const product_details = productDetailsElement
      ? productDetailsElement.textContent.trim().replace(/\s{2,}/g, " ")
      : "N/A";

    // নির্দিষ্টকরণের উপাদান নির্বাচন করুন
    const specificationElement = document.querySelector(".specification-table");
    const product_specification = specificationElement
      ? specificationElement.textContent.trim().replace(/\s{2,}/g, " ")
      : "N/A";

    // পণ্যের ইমেজ উপাদান নির্বাচন করুন
    const productImgElements = [
      ...document.querySelectorAll(".order-lg-first img"),
    ];
    const product_images = productImgElements.map((e) => e.src);

    // পণ্যের মূল্যের উপাদান নির্বাচন করুন
    const productPrices = [...document.querySelectorAll(".price-block")];
    const product_price = productPrices.map((e) =>
      e.textContent.trim().replace(/\s{2,}/g, " ")
    );

    return {
      product_title,
      product_content,
      product_details,
      product_specification,
      product_images,
      product_price,
    };
  });
  return productData;
};
```

5. নিম্নলিখিত কমান্ড লিখে দেখতে হবে আউটপুট কি আসে

```bash
node product_details.js
```

এটি আসলে `Puppeteer` ব্যবহার করে একটি ওয়েব স্ক্র্যাপিং স্ক্রিপ্ট যা `ryans.com` থেকে "ডেল মনিটর 24 ইঞ্চি" সম্পর্কিত পণ্য সংগ্রহ করে এবং সেগুলোর বিস্তারিত তথ্য একটি JSON ফাইলে সংরক্ষণ করে। আমি এখানে কোডের প্রতিটি অংশ সম্পর্কে একটু ভেঙে বুঝিয়ে দিচ্ছি:

---

### 1. **`import puppeteer from "puppeteer";`**

এখানে `puppeteer` লাইব্রেরি ইনপোর্ট করা হয়েছে। এটি মূলত ওয়েব ব্রাউজার অটোমেশন এবং স্ক্র্যাপিং-এর জন্য ব্যবহৃত হয়।

### 2. **`import fs from "fs";`**

এখানে `fs` (ফাইল সিস্টেম) মডিউল ব্যবহার করা হচ্ছে, যাতে আমরা স্ক্র্যাপ করা ডেটা JSON ফাইল হিসেবে সংরক্ষণ করতে পারি।

### 3. **`(async () => { ... })();`**

এটা একটা অ্যাসিঙ্ক্রোনাস (asynchronous) ফাংশন, যেটার মধ্যে আমরা ওয়েব পেজের সাথে ইন্টারঅ্যাক্ট করতে `await` ব্যবহার করতে পারব। এইভাবে, পৃষ্ঠাটি সঠিকভাবে লোড হয়ে যাওয়ার পরই পরবর্তী ধাপে যেতে পারব।

### 4. **`const allProductsData = [];`**

এখানে একটা খালি অ্যারে তৈরি করা হয়েছে, যেখানে সমস্ত পণ্যের তথ্য সংগ্রহ করা হবে।

### 5. **`const browser = await puppeteer.launch({ ... });`**

এই লাইনে ব্রাউজার শুরু করা হচ্ছে। `headless: false` মানে ব্রাউজারটি ইউজার ইন্টারফেস সহ খুলবে, যাতে আমরা দেখতে পারি স্ক্র্যাপিং প্রক্রিয়া চলাকালীন কি ঘটছে। `defaultViewport` দিয়ে পেজের সাইজ নির্ধারণ করা হয়েছে এবং `userDataDir` ব্যবহার করে ইউজার ডেটা সংরক্ষিত হচ্ছে।

### 6. **`const page = await browser.newPage();`**

এখানে ব্রাউজারের একটি নতুন পেজ খুলে, সেটি ব্যবহার করার জন্য প্রস্তুত করা হচ্ছে।

### 7. **`const searchTerm = 'dell monitor 24 inch';`**

এটি সার্চ টার্ম হিসেবে "ডেল মনিটর 24 ইঞ্চি" সেট করা হচ্ছে, যার জন্য পণ্যগুলো স্ক্র্যাপ করা হবে।

### 8. **`await page.goto("https://www.ryans.com", { waitUntil: "networkidle2", timeout: 600000 });`**

এখানে `ryans.com` সাইটে চলে যাওয়া হচ্ছে। `waitUntil: "networkidle2"` মানে, পেজের সমস্ত রিসোর্স সম্পূর্ণভাবে লোড না হওয়া পর্যন্ত অপেক্ষা করা হচ্ছে।

### 9. **`await page.type("#search", searchTerm);`**

এটা হচ্ছে সার্চ বক্সে "ডেল মনিটর 24 ইঞ্চি" টাইপ করার প্রক্রিয়া।

### 10. **`await page.click('button[type="submit"]');`**

এখানে সার্চ বাটনে ক্লিক করা হচ্ছে, যাতে সার্চ কার্যক্রম শুরু হয়।

### 11. **`await page.waitForSelector(".card.h-100");`**

এখানে `.card.h-100` সিলেক্টরের জন্য অপেক্ষা করা হচ্ছে, যা নিশ্চিত করে যে পণ্যের তথ্য পেজে লোড হয়ে গেছে।

### 12. **`const allProducts = await page.evaluate(() => { ... });`**

এটা পেজের DOM থেকে সমস্ত পণ্যের তথ্য নিয়ে আসছে। পণ্যগুলোর URL এবং ইমেজ লিংক সংগ্রহ করা হচ্ছে।

### 13. **`for (const product of allProducts) { ... }`**

এখানে সমস্ত পণ্যের জন্য একটি লুপ চলানো হচ্ছে, যাতে প্রতিটি পণ্যের বিস্তারিত পেজ খুলে তার সব তথ্য স্ক্র্যাপ করা যায়।

### 14. **`const productPage = await browser.newPage();`**

প্রতিটি পণ্যের জন্য একটি নতুন পেজ তৈরি করা হচ্ছে, যাতে পণ্যের বিস্তারিত তথ্য সংগ্রহ করা যাবে।

### 15. **`await productPage.goto(product.url, { waitUntil: "networkidle2", timeout: 600000 });`**

এখানে প্রতিটি পণ্যের পৃষ্ঠা ওপেন করা হচ্ছে এবং সেখানে পেজটি সম্পূর্ণ লোড না হওয়া পর্যন্ত অপেক্ষা করা হচ্ছে।

### 16. **`const productDescription = await scrapeProductData(productPage);`**

এখানে `scrapeProductData` ফাংশন ব্যবহার করা হচ্ছে প্রতিটি পণ্যের বিস্তারিত তথ্য (যেমন পণ্যের শিরোনাম, বিবরণ, ছবি, দাম ইত্যাদি) স্ক্র্যাপ করার জন্য।

##### `scrapeProductData` ফাংশন:

এই ফাংশনটি পণ্যের বিস্তারিত তথ্য সংগ্রহ করে। এর মধ্যে:

- **`product_title`**: পণ্যের শিরোনাম
- **`product_content`**: পণ্যের সাধারণ বিবরণ
- **`product_details`**: পণ্যের বিস্তারিত বিবরণ
- **`product_specification`**: পণ্যের স্পেসিফিকেশন
- **`product_images`**: পণ্যের ইমেজ লিংক
- **`product_price`**: পণ্যের দাম

### 17. **`productPage.close();`**

পণ্যের পৃষ্ঠা বন্ধ করা হচ্ছে, যাতে পরবর্তী পণ্যটির জন্য নতুন পৃষ্ঠা খোলা যায়।

### 18. **`allProductsData.push({ ...product, ...productDescription });`**

এখানে স্ক্র্যাপ করা সমস্ত তথ্য `allProductsData` অ্যারেতে যোগ করা হচ্ছে।

### 19. **`console.log(allProductsData.length);`**

এটা কনসোলে স্ক্র্যাপ করা পণ্যের সংখ্যা দেখাবে, যেন আমরা জানি কতটি পণ্য সফলভাবে স্ক্র্যাপ করা হয়েছে।

### 20. **`fs.writeFileSync('./products_${searchTerm}.json', JSON.stringify(allProductsData, null, 2));`**

এটি স্ক্র্যাপ করা সমস্ত পণ্যের তথ্য একটি JSON ফাইলে সংরক্ষণ করবে, যাতে পরে আমরা এই ডেটা ব্যবহার করতে পারি।

### 21. **`catch (error) { console.error("Error scraping ryans:", error); }`**

এখানে যদি কোনো এরর ঘটে, তাহলে সেটা কনসোলে দেখানো হবে, যাতে সহজে বুঝতে পারি কোথায় সমস্যা হয়েছে।

### 22. **`finally { await browser.close(); }`**

সবশেষে, ব্রাউজার বন্ধ করা হচ্ছে, যাতে স্ক্র্যাপিং প্রক্রিয়া শেষে সিস্টেম রিসোর্সগুলো ফ্রি থাকে।

এটা একেবারে সোজা ভাষায় বললে, কোডটি `ryans.com` সাইট থেকে "ডেল মনিটর 24 ইঞ্চি" সম্পর্কিত পণ্যের তথ্য নিয়ে আসে এবং প্রতিটি পণ্যের বিস্তারিত তথ্য সংগ্রহ করে JSON ফাইলে সংরক্ষণ করে।

পরবর্তীতে আমরা বিভিন্ন ওয়েবসাইটের বিভিন্ন প্রোডাক্ট প্রাইজের কম্পারিজন দেখতে পারবো যার জন্য ওয়েব স্ক্র্যাপিং ব্যবহার করব। অনেকেই ভাবতে পারেন এটার ইউজকেস কি হবে? সেটা না হয় আমরা পরের পর্বেই দেখব। আপাতত এ পর্যন্তই, কোন প্রশ্ন থাকলে আমাকে [ফেসবুকে](https://facebook.com/whoafridi) যোগাযোগ করতে পারেন।
