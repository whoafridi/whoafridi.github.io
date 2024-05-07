---
layout: post
title: Puppeteer দিয়ে ওয়েব স্ক্র্যাপিং পর্ব ২
subtitle: ওয়েব স্ক্র্যাপিং পর্ব ২
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

আমরা `github` লিখে সার্চ করে প্রথম পেইজের সকল আউটপুটকে বের করে এনেছি ।
আর এভাবেই আমাদের প্রথম ওয়েব স্ক্র্যাপিং এর কাজকর্ম হয়ে গেল । পরবর্তী পর্বে আমরা ই-কমার্স এর প্রোডাক্ট এর প্রাইস, নাম বের করা নিয়ে আলোচনা করব । আপাতত এ পর্যন্তই কোন প্রশ্ন থাকলে আমাকে [ফেসবুকে](https://facebook.com/whoafridi) যোগাযোগ করতে পারেন ।
