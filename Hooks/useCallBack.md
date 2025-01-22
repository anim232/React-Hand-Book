`useCallback` হল React-এর একটি হুক যা একটি ফাংশনের সংজ্ঞা ক্যাশে করতে ব্যবহৃত হয়, যাতে রেন্ডারিংয়ের পর পরবর্তী রেন্ডারে সেই ফাংশন পুনরায় তৈরি না হয়। এটি বিশেষ করে তখন ব্যবহৃত হয় যখন আপনি কোন ফাংশনকে কম্পোনেন্টে বারবার ডিফাইন না করে, তার পূর্ববর্তী রেন্ডারে ক্যাশে করা ফাংশনটি ব্যবহার করতে চান।

এখানে `useCallback` এর ব্যবহার এবং এর প্যারামিটার, রিটার্ন এবং কিছু গুরুত্বপূর্ণ বিষয় নিচে ব্যাখ্যা করা হলো:

### উদাহরণ:
```javascript
import { useCallback } from 'react';

export default function ProductPage({ productId, referrer, theme }) {
  const handleSubmit = useCallback((orderDetails) => {
    post('/product/' + productId + '/buy', {
      referrer,
      orderDetails,
    });
  }, [productId, referrer]);

  // আরো UI কোড এখানে
}
```

### প্যারামিটার:
1. **fn (ফাংশন)**: এটি সেই ফাংশন যা আপনি ক্যাশে করতে চান। এই ফাংশনটি যেকোনো আর্গুমেন্ট নিতে পারে এবং যেকোনো মান রিটার্ন করতে পারে। React ফাংশনটি প্রথম রেন্ডারেই আপনাকে ফিরিয়ে দিবে এবং পরবর্তী রেন্ডারগুলোতে, যদি ডিপেন্ডেন্সি (নির্ভরশীল মান) পরিবর্তিত না হয়, React আগের রেন্ডারে ক্যাশে করা ফাংশনটি আপনাকে আবার দিবে।
   
2. **dependencies (ডিপেন্ডেন্সি)**: এটি একটি অ্যারে, যেখানে আপনি যেসব রিয়েক্টিভ (প্রতিক্রিয়াশীল) ভ্যালু ব্যবহার করছেন সেগুলো উল্লেখ করবেন, যেমন: `props`, `state`, অথবা কম্পোনেন্টের মধ্যে ডিক্লেয়ার করা ভ্যারিয়েবল ও ফাংশন। যদি কোন ডিপেন্ডেন্সির মান পরিবর্তিত না হয়, তবে React আগের ক্যাশ করা ফাংশনটি রিটার্ন করবে। যদি কোনো ডিপেন্ডেন্সি পরিবর্তিত হয়, তাহলে নতুন ফাংশনটি রিটার্ন করবে।

### রিটার্ন:
- প্রথম রেন্ডারেই, `useCallback` আপনার দেওয়া ফাংশনটিকে ফিরিয়ে দিবে।
- পরবর্তী রেন্ডারগুলোতে, যদি ডিপেন্ডেন্সি পরিবর্তিত না হয়, React আগের রেন্ডারের ক্যাশ করা ফাংশনটি দিবে। অন্যথায়, এটি নতুন ফাংশনটি রিটার্ন করবে।

### সতর্কতা:
- `useCallback` একটি হুক, তাই এটি শুধুমাত্র কম্পোনেন্টের টপ লেভেলে অথবা আপনার নিজের হুকগুলোর মধ্যে ব্যবহার করা উচিত। এটি লুপ বা কন্ডিশনে ব্যবহার করা যাবে না। যদি আপনাকে এমন কিছু করতে হয়, তবে নতুন একটি কম্পোনেন্ট তৈরি করুন এবং তার মধ্যে স্টেট মুভ করুন।
  
- React ফাংশনটি ক্যাশে রাখবে যতক্ষণ না কিছু পরিবর্তন হয়। তবে, ডেভেলপমেন্ট পরিবেশে যখন আপনি কম্পোনেন্টের ফাইলটি এডিট করবেন, React সেই ক্যাশে করা ফাংশনটি মুছে ফেলতে পারে। তাছাড়া, যখন কম্পোনেন্টের ইনিশিয়াল মাউন্টে সাসপেন্ড হয়, React ক্যাশে করা ফাংশনটি মুছে ফেলবে।

### উপকারিতা:
- যদি একটি কম্পোনেন্টে রেন্ডারিংয়ের সময় একই ফাংশন বারবার তৈরি হতে থাকে, তা হলে পারফরম্যান্সের সমস্যা হতে পারে। `useCallback` এই ধরনের সমস্যা এড়াতে সাহায্য করে। 

এটি সাধারনত ব্যবহার করা হয় যখন আপনি কম্পোনেন্টের মধ্যে কোনো ইভেন্ট হ্যান্ডলার বা কলব্যাক ফাংশন ব্যবহার করছেন যা বারবার রেন্ডার হওয়ার দরকার নেই। 

এছাড়া, আপনি যদি কম্পোনেন্টের ভিতরে `useMemo` বা `useEffect` ব্যবহার করেন, তবে তাদের সাথে `useCallback`-এর সঠিক ব্যবহার পারফরম্যান্স উন্নত করতে সাহায্য করতে পারে।

### **React-এ Component এর পুনরায় রেন্ডারিং এড়ানো (Skipping Re-rendering of Components)**
React-এ যখন আমরা রেন্ডারিং পারফরম্যান্স অপটিমাইজ করি, তখন প্রায়ই আমাদের চাইল্ড কম্পোনেন্টগুলিতে যে ফাংশন পাস করি তা ক্যাশে (cache) করার প্রয়োজন হয়। এর মাধ্যমে, কম্পোনেন্টের পুনরায় রেন্ডারিংয়ের সময় ওই ফাংশনটি পুনরায় তৈরি না হয়ে, আগের ডিফাইন করা ফাংশনটি ব্যবহার করা হয়। এর জন্য `useCallback` হুক ব্যবহার করা হয়। আসুন পুরো বিষয়টি বিস্তারিতভাবে বুঝে নেওয়া যাক।

---

### **`useCallback` হুক কীভাবে কাজ করে?**

`useCallback` হুক ব্যবহার করে আপনি একটি ফাংশন ক্যাশে করতে পারেন, যাতে ফাংশনটি পুনরায় রেন্ডার হওয়ার পরেও পরিবর্তিত না হয় (যতক্ষণ না তার নির্ভরশীল মানগুলো পরিবর্তিত হয়)।

```javascript
import { useCallback } from 'react';

function ProductPage({ productId, referrer, theme }) {
  const handleSubmit = useCallback((orderDetails) => {
    post('/product/' + productId + '/buy', {
      referrer,
      orderDetails,
    });
  }, [productId, referrer]);
  // ...
}
```

এখানে দুটি জিনিস দরকার:
1. **ফাংশন (function)**: যে ফাংশনটি আপনি ক্যাশে করতে চান।
2. **ডিপেন্ডেন্সি (dependencies)**: এই ফাংশনটির ভিতরে যেসব রিয়েক্টিভ মান (reactive values) ব্যবহার হচ্ছে, যেমন `props` বা `state`। এসব মানের ওপর ভিত্তি করে React জানে কখন ফাংশনটি পুনরায় তৈরি করতে হবে।

---

### **কীভাবে কাজ করে?**

- **প্রথম রেন্ডার**: প্রথমবার `useCallback` ব্যবহার করলে, আপনি যে ফাংশনটি দিয়েছেন তা React ফিরিয়ে দেবে।
- **পরবর্তী রেন্ডার**: পরবর্তী রেন্ডারগুলোতে, React আগের রেন্ডারের ডিপেন্ডেন্সির সাথে নতুন ডিপেন্ডেন্সি তুলনা করবে। যদি ডিপেন্ডেন্সি পরিবর্তিত না হয়, তাহলে React আগের ক্যাশে করা ফাংশনটি ফেরত দেবে। অন্যথায়, নতুন ফাংশনটি রিটার্ন করবে।

অথাৎ, `useCallback` ফাংশনটি ক্যাশে রাখে যতক্ষণ না তার নির্ভরশীল মানগুলো পরিবর্তিত হয়।

---

### **`useCallback` কোথায় কাজে আসে?**

ধরা যাক, আপনি `ProductPage` কম্পোনেন্ট থেকে `ShippingForm` কম্পোনেন্টে `handleSubmit` ফাংশনটি পাস করছেন:

```javascript
function ProductPage({ productId, referrer, theme }) {
  // ...
  return (
    <div className={theme}>
      <ShippingForm onSubmit={handleSubmit} />
    </div>
  );
}
```

এখন, আপনি লক্ষ্য করেছেন যে যখন `theme` প্রপ পরিবর্তিত হয়, তখন অ্যাপটি কিছু সময়ের জন্য ফ্রিজ হয়ে যায়। কিন্তু, যদি আপনি `<ShippingForm />` কম্পোনেন্টটি সরিয়ে দেন, তখন অ্যাপটি দ্রুত চলে। এই পরিস্থিতিতে, `ShippingForm` কম্পোনেন্টটি অপটিমাইজ করা দরকার।

### **React ডিফল্ট আচরণ:**
যখন একটি কম্পোনেন্ট রেন্ডার হয়, React তার সমস্ত চাইল্ড কম্পোনেন্টকে রেন্ডার করে। অর্থাৎ, যদি `ProductPage` একটি নতুন থিম পেয়ে রেন্ডার হয়, তাহলে `ShippingForm` কম্পোনেন্টও রেন্ডার হবে। এটি সমস্যা হতে পারে যদি রেন্ডারিং ধীর গতির হয়।

### **কীভাবে `memo` সাহায্য করে?**
আপনি যদি `ShippingForm` কম্পোনেন্টটি `memo` দিয়ে র‍্যাপ করেন, তবে এটি শুধুমাত্র তখন রেন্ডার হবে যখন তার প্রপস পরিবর্তিত হবে:

```javascript
import { memo } from 'react';

const ShippingForm = memo(function ShippingForm({ onSubmit }) {
  // ...
});
```

এখন, `ShippingForm` কম্পোনেন্ট শুধুমাত্র তখন রেন্ডার হবে যখন তার প্রপস পরিবর্তিত হবে। কিন্তু, এখানে সমস্যা হলো, আপনি যদি `handleSubmit` ফাংশনটি প্রত্যেক রেন্ডারেই নতুনভাবে ডিফাইন করেন, তবে `ShippingForm`-এর প্রপস কখনই আগের মতো থাকবে না, এবং `memo` অপটিমাইজেশন কাজ করবে না। 

### **ফাংশন ডিফাইনেশনে `useCallback` ব্যবহার না করা:**

```javascript
function ProductPage({ productId, referrer, theme }) {
  // প্রতি বার theme পরিবর্তিত হলে, handleSubmit একটি নতুন ফাংশন হবে...
  function handleSubmit(orderDetails) {
    post('/product/' + productId + '/buy', {
      referrer,
      orderDetails,
    });
  }
  
  return (
    <div className={theme}>
      {/* ... ফলে ShippingForm-এর প্রপস কখনই একই হবে না, এবং এটি প্রতি বার রেন্ডার হবে */}
      <ShippingForm onSubmit={handleSubmit} />
    </div>
  );
}
```

এখানে `handleSubmit` প্রতি রেন্ডারে একটি নতুন ফাংশন তৈরি হচ্ছে, তাই `ShippingForm`-এর প্রপস সবসময় ভিন্ন হবে, এবং এটি প্রতি রেন্ডারে রেন্ডার হবে।

### **ফাংশন ক্যাশে করার জন্য `useCallback` ব্যবহার:**

```javascript
function ProductPage({ productId, referrer, theme }) {
  // useCallback দিয়ে ফাংশন ক্যাশে করুন...
  const handleSubmit = useCallback((orderDetails) => {
    post('/product/' + productId + '/buy', {
      referrer,
      orderDetails,
    });
  }, [productId, referrer]); // ...যতক্ষণ না এই ডিপেন্ডেন্সিগুলো পরিবর্তিত হয়

  return (
    <div className={theme}>
      {/* ...ShippingForm একি প্রপস পাবে, তাই এটি রেন্ডার স্কিপ করতে পারবে */}
      <ShippingForm onSubmit={handleSubmit} />
    </div>
  );
}
```

এখানে, `useCallback` ব্যবহার করে `handleSubmit` ফাংশন ক্যাশে করা হচ্ছে। এর ফলে, `ShippingForm` একি প্রপস পাবে এবং এটি আগের রেন্ডারের মত রেন্ডার হতে পারবে না, অর্থাৎ এটি রেন্ডার স্কিপ করবে।

---

### **মনে রাখবেন:**

- `useCallback` শুধু তখনই ব্যবহার করুন যখন এটি পারফরম্যান্স অপটিমাইজেশনের জন্য প্রয়োজন। যদি আপনার কোড `useCallback` ছাড়া কাজ না করে, তবে প্রথমে মূল সমস্যাটি খুঁজে বের করে ঠিক করুন, তারপর `useCallback` ব্যবহার করুন।

---

এইভাবে, `useCallback` হুক ব্যবহারের মাধ্যমে আপনি আপনার কম্পোনেন্টগুলির অপ্রয়োজনীয় রেন্ডারিং কমিয়ে পারফরম্যান্স অপটিমাইজ করতে পারবেন।

### **`useCallback` এবং `useMemo` এর সম্পর্ক:**

`useMemo` এবং `useCallback` হুকগুলো প্রায়ই একসাথে ব্যবহৃত হয় যখন আপনি চাইল্ড কম্পোনেন্টের পারফরম্যান্স অপটিমাইজ করতে চান। এই দুইটি হুকের মধ্যে পার্থক্য আছে, কিন্তু তারা একে অপরের সাথে সম্পর্কিত, কারণ তারা উভয়ই কিছু ক্যাশে করতে সাহায্য করে। আসুন বিস্তারিতভাবে দেখি, কীভাবে তারা কাজ করে এবং একে অপরের সাথে সম্পর্কিত।

---

### **`useMemo` এবং `useCallback` কি?**

- **`useMemo`**: এটি একটি হুক যা একটি ফাংশন ক্যালকুলেট করে এবং তার রেজাল্ট ক্যাশে করে রাখে। এর ফলে, রেন্ডারিংয়ের সময় একি ক্যালকুলেশন বারবার করা হয় না, যতক্ষণ না নির্দিষ্ট ডিপেন্ডেন্সি পরিবর্তিত হয়।
  
- **`useCallback`**: এটি একটি হুক যা একটি ফাংশন ক্যাশে রাখে। এটি কেবল ফাংশনটির ডেফিনিশন ক্যাশে করে এবং ফাংশনটি রেন্ডার হওয়ার পর নতুন করে তৈরি না হয়ে আগের ফাংশনটি ব্যবহার করে।

---

### **কীভাবে তারা একে অপরের সাথে সম্পর্কিত?**

**1. `useMemo` কীভাবে কাজ করে:**

`useMemo` ফাংশনটির রেজাল্ট (অথবা আউটপুট) ক্যাশে রাখে। যখনই আপনার ফাংশনটি কোনো ভ্যালু ক্যালকুলেট করে, তা ক্যাশে করে রাখবে এবং পরবর্তী রেন্ডারে সেই ক্যাশ করা রেজাল্টটি ব্যবহার করবে, যতক্ষণ না তার নির্ভরশীল ডিপেন্ডেন্সি পরিবর্তিত হয়।

```javascript
import { useMemo } from 'react';

function ProductPage({ productId }) {
  const product = useData('/product/' + productId);

  const requirements = useMemo(() => { 
    return computeRequirements(product);  // Compute requirements only when product changes
  }, [product]); // Dependencies: product

  return (
    <div>
      <ShippingForm requirements={requirements} />
    </div>
  );
}
```

এখানে `useMemo` ব্যবহার করে আমরা `computeRequirements` ফাংশনের রেজাল্ট ক্যাশে করছি। এটি শুধুমাত্র তখনই পুনরায় ক্যালকুলেট হবে যখন `product` পরিবর্তিত হবে। এর ফলে, `ShippingForm` কম্পোনেন্টে `requirements` প্রপসটি পুনরায় রেন্ডার করার দরকার হবে না যদি `product` অপরিবর্তিত থাকে।

---

**2. `useCallback` কীভাবে কাজ করে:**

`useCallback` হুক ফাংশনটি ক্যাশে রাখে, কিন্তু এটি নিজে কখনও ফাংশনটি কল বা রান করে না। এটি কেবল একটি ফাংশনের রেফারেন্স (অথবা ডেফিনিশন) ক্যাশে রাখে এবং ফাংশনটি তখনই পরিবর্তিত হয় যখন তার নির্ভরশীল ডিপেন্ডেন্সি (যেমন: `productId`, `referrer`) পরিবর্তিত হয়।

```javascript
import { useCallback } from 'react';

function ProductPage({ productId, referrer }) {
  const handleSubmit = useCallback((orderDetails) => {
    post('/product/' + productId + '/buy', {
      referrer,
      orderDetails,
    });
  }, [productId, referrer]); // Dependencies: productId, referrer

  return (
    <div>
      <ShippingForm onSubmit={handleSubmit} />
    </div>
  );
}
```

এখানে, `handleSubmit` ফাংশনটি `useCallback` দিয়ে ক্যাশে করা হয়েছে। এই ফাংশনটি শুধুমাত্র তখনই পরিবর্তিত হবে যখন `productId` অথবা `referrer` পরিবর্তিত হবে। এর ফলে, আপনি `ShippingForm` কম্পোনেন্টে প্রতিবার নতুন `handleSubmit` ফাংশন পাস না করে আগের ক্যাশ করা ফাংশনটি ব্যবহার করতে পারবেন।

---

### **`useMemo` বনাম `useCallback` - পার্থক্য:**

- **`useMemo`**: এটি **ফাংশনের রেজাল্ট** (অথবা আউটপুট) ক্যাশে রাখে। উদাহরণস্বরূপ, যদি আপনার কোনো ক্যালকুলেশন বা প্রসেসিং করতে হয় (যেমন: ডাটা ফিল্টার করা), তবে `useMemo` ব্যবহৃত হয়।
  
- **`useCallback`**: এটি **ফাংশনটির রেফারেন্স বা ডেফিনিশন** ক্যাশে রাখে। এটি শুধুমাত্র তখনই কার্যকর হয় যখন আপনি একটি ফাংশনকে চাইল্ড কম্পোনেন্টে পাস করতে চান এবং চান যে ওই ফাংশনটি পুনরায় তৈরি না হয়ে আগের রেফারেন্সটি ব্যবহার করা হোক।

### **সিম্প্লিফাইড দৃষ্টিভঙ্গি:**

যদি আমরা `useCallback` কে `useMemo` এর মতো চিন্তা করি, তবে এটি এই রকম হবে:

```javascript
// Simplified implementation (inside React)
function useCallback(fn, dependencies) {
  return useMemo(() => fn, dependencies);
}
```

এখানে, `useCallback` বাস্তবিকভাবে `useMemo`-এর একটি বিশেষায়িত সংস্করণ, যেখানে এটি শুধু ফাংশন ক্যাশে করে এবং ফাংশনটি কেবল রেন্ডারিংয়ের সময় ব্যবহৃত হয়।

---

### **উদাহরণ সহ বিশ্লেষণ:**

```javascript
import { useMemo, useCallback } from 'react';

function ProductPage({ productId, referrer }) {
  const product = useData('/product/' + productId);

  // `useMemo` caches the result of `computeRequirements`
  const requirements = useMemo(() => { 
    return computeRequirements(product); 
  }, [product]);

  // `useCallback` caches the `handleSubmit` function itself
  const handleSubmit = useCallback((orderDetails) => { 
    post('/product/' + productId + '/buy', {
      referrer,
      orderDetails,
    });
  }, [productId, referrer]);

  return (
    <div>
      <ShippingForm requirements={requirements} onSubmit={handleSubmit} />
    </div>
  );
}
```

এখানে:
- **`useMemo`** ক্যাশে করছে `computeRequirements(product)` এর রেজাল্ট, যাতে `ShippingForm` কম্পোনেন্টটি `requirements` প্রপস হিসেবে আগের রেজাল্ট ব্যবহার করে, যতক্ষণ না `product` পরিবর্তিত হয়।
- **`useCallback`** ক্যাশে করছে `handleSubmit` ফাংশনটি, যাতে এটি শুধুমাত্র তখনই পরিবর্তিত হয় যখন `productId` বা `referrer` পরিবর্তিত হয়। এর ফলে, `ShippingForm` কম্পোনেন্টে নতুন ফাংশন পাস না করে আগের ফাংশনটি ব্যবহার করা হবে।

---

### **সংক্ষেপে বলা যায়:**

- **`useMemo`**: ক্যালকুলেশনের রেজাল্ট ক্যাশে রাখে।
- **`useCallback`**: ফাংশনের রেফারেন্স (ডেফিনিশন) ক্যাশে রাখে।

যখন আপনি চান যে কোনো ডাটা বা ফাংশন পুনরায় তৈরি না হয়ে আগের রেজাল্ট বা রেফারেন্স ব্যবহার করা হোক, তখন এই দুটি হুক ব্যবহৃত হয়। `useMemo` আপনার ক্যালকুলেশন বা কম্পিউটেশন সংরক্ষণ করে এবং `useCallback` ফাংশনের ডেফিনিশন সংরক্ষণ করে।

### **`useCallback` কি?**

`useCallback` হুকটি React-এ একটি পারফরম্যান্স অপটিমাইজেশন টুল যা আপনাকে ফাংশনগুলোর রেফারেন্স ক্যাশে করতে সাহায্য করে। এর মাধ্যমে আপনি একে অপরকে পাস করা ফাংশনগুলোকে পুনরায় তৈরি হওয়ার থেকে রক্ষা করতে পারেন, যাতে কম্পোনেন্ট রেন্ডারিংয়ে অপ্রয়োজনীয় পরিবর্তন না হয়। তবে, প্রশ্ন হল: **আপনি কি `useCallback` প্রতিটি জায়গায় ব্যবহার করবেন?**

---

### **`useCallback` ব্যবহার করবেন কি না?**

আপনার অ্যাপ্লিকেশন বা ইউজার ইন্টারফেসের ধরণ অনুযায়ী, `useCallback` ব্যবহার করা প্রয়োজন নাও হতে পারে। একে সর্বত্র ব্যবহার করা ঠিক নয় এবং এর কিছু সুবিধা ও সীমাবদ্ধতা রয়েছে।

### **যখন `useCallback` ব্যবহার করার সুবিধা আছে:**

1. **মেমোযুক্ত কম্পোনেন্টে প্রপস হিসেবে পাঠানো:**  
   যদি আপনি একটি ফাংশনকে এমন কম্পোনেন্টে প্রপস হিসেবে পাঠান যা `memo` দিয়ে র‍্যাপ করা হয়েছে, তাহলে `useCallback` ব্যবহার করা উপকারী হতে পারে। কারণ `memo` হুকটি কেবল তখনই কম্পোনেন্ট রেন্ডার করে যদি প্রপস পরিবর্তিত হয়। এই ক্ষেত্রে, `useCallback` ক্যাশে ফাংশন রেফারেন্স রাখে, ফলে কম্পোনেন্ট কেবল তখনই রেন্ডার হয় যখন ফাংশনের ডিপেন্ডেন্সি পরিবর্তিত হয়।

   ```javascript
   const handleClick = useCallback(() => {
     // Some action
   }, [dependency]);
   ```

2. **একটি হুকের ডিপেন্ডেন্সি হিসেবে ব্যবহার:**  
   যদি আপনি একটি ফাংশনকে অন্য কোনো হুকের ডিপেন্ডেন্সি হিসেবে ব্যবহার করেন, যেমন `useEffect` বা `useCallback` এর ভিতরে, তাহলে `useCallback` ব্যবহার করা প্রয়োজনীয় হতে পারে। এতে আপনি একই ফাংশন রেফারেন্সটি পাবেন যতদিন না তার ডিপেন্ডেন্সি পরিবর্তিত হয়।

   ```javascript
   const handleSubmit = useCallback(() => {
     // Submit action
   }, [productId, referrer]);
   ```

### **যখন `useCallback` ব্যবহার করার কোনো দরকার নেই:**

1. **অপ্রয়োজনীয় মেমোইজেশন:**  
   যদি আপনি `useCallback` এমন জায়গায় ব্যবহার করেন যেখানে আপনার ফাংশন বা ডাটা খুবই কম পরিবর্তিত হয়, তবে এটি কোনো পারফরম্যান্স বেনিফিট দিতে নাও পারে। ফলস্বরূপ, কোডের জটিলতা বাড়তে পারে এবং রিডেবলনেস কমতে পারে।

   ```javascript
   const handleClick = useCallback(() => {
     // Some action
   }, []);  // Empty dependency
   ```

2. **মেমোইজেশন যখন কার্যকর না হয়:**  
   যদি আপনার অ্যাপ্লিকেশন এমনভাবে ডিজাইন করা হয় যেখানে একটি মান "সবসময় নতুন" হয়, তবে মেমোইজেশন কার্যকর হবে না। উদাহরণস্বরূপ, আপনি যদি প্রতি রেন্ডারে নতুন একটি ফাংশন বা অবজেক্ট তৈরি করেন, তবে তা মেমোইজেশন ব্রেক করে দেয়।

   ```javascript
   const handleClick = () => {
     // New function every time
   };
   ```

3. **কোড রিডেবিলিটি কমানো:**  
   অতিরিক্ত `useCallback` ব্যবহার কোডের রিডেবিলিটি কমাতে পারে। আপনার কোড যদি খুব জটিল হয় এবং যেখানে এটি প্রয়োজনীয় নয়, সেখানে অতিরিক্ত `useCallback` ব্যবহারের কারণে কোড বুঝতে সমস্যা হতে পারে।

---

### **কোড অপটিমাইজেশনে কিছু সাধারণ টিপস:**

1. **বিশ্বস্ত (Pure) কম্পোনেন্ট রেন্ডারিং:**  
   আপনার কম্পোনেন্ট যদি কোনো সমস্যা সৃষ্টি করে বা কোনো দৃশ্যমান বাগ তৈরি করে যখন তা রেন্ডার হয়, তবে এটি একটি বাগ হতে পারে এবং আপনি সঠিকভাবে সমস্যাটি সমাধান করবেন, মেমোইজেশন দিয়ে তা ঢাকবেন না।

2. **অপ্রয়োজনীয় `useEffect` ব্যবহার থেকে বিরত থাকুন:**  
   `useEffect` এর মাধ্যমে যদি আপনার স্টেট আপডেট হয় এবং এতে প্রতিবার রেন্ডার হয়, তবে তা পারফরম্যান্স ইস্যু তৈরি করতে পারে। আপনি যেকোনো ফাংশন বা অবজেক্টকে `useEffect` থেকে সরিয়ে নিতে পারেন যাতে বারবার রেন্ডার না হয়।

3. **লোকাল স্টেট ব্যবহার করুন:**  
   যদি আপনার কম্পোনেন্টে কোনো সাময়িক (transient) স্টেট থাকে, যেমন: ফর্ম বা হোভার ইফেক্ট, তাহলে সেটা সর্বোচ্চ স্তরে বা গ্লোবাল স্টেটে রাখবেন না। এতে unnecessary রেন্ডার কমে যাবে।

4. **React ডেভেলপার টুলস ব্যবহার করুন:**  
   যদি আপনার অ্যাপ্লিকেশন এখনও স্লো মনে হয়, তাহলে **React Developer Tools profiler** ব্যবহার করুন। এটি আপনাকে সঠিকভাবে বুঝতে সাহায্য করবে কোন কম্পোনেন্টগুলোতে মেমোইজেশন দরকার, এবং সেখানে `useCallback` বা `useMemo` যোগ করা উচিত।

---

### **সারাংশ:**

আপনি যদি একটি অ্যাপ্লিকেশন তৈরি করেন যেখানে বেশিরভাগ ইন্টারঅ্যাকশন কোর্স (যেমন, পুরো পেজ বা সেকশন পরিবর্তন) হয়, তবে `useCallback` বা মেমোইজেশন সাধারণত প্রয়োজনীয় হবে না। তবে যদি আপনার অ্যাপ্লিকেশনটি গঠনমূলক হয় এবং প্রতিটি ইন্টারঅ্যাকশন ছোট (যেমন, ড্রয়িং অ্যাপ যেখানে শেপস মুভ করা হয়), তবে `useCallback` বা মেমোইজেশন ব্যবহারের মাধ্যমে পারফরম্যান্স উন্নত হতে পারে। 

মোটের উপর, আপনি **প্রতিটি কম্পোনেন্টে `useCallback`** ব্যবহার করবেন না, তবে যেখানে এটি সত্যিই প্রয়োজন, সেখানে এটি ব্যবহার করুন।



### **`useCallback` এবং `memo` দিয়ে রেন্ডারিং স্কিপ করা -**

React অ্যাপ্লিকেশনে, আপনি যখন রেন্ডারিং অপটিমাইজেশন করতে চান, তখন **`useCallback`** এবং **`memo`** হুক ব্যবহার করতে পারেন। এগুলি আপনাকে কার্যকরভাবে কম্পোনেন্টের রেন্ডারিং কমাতে সাহায্য করে এবং আপনার অ্যাপ্লিকেশনের পারফরম্যান্স উন্নত করে। এখানে আমরা দেখব কীভাবে এই হুকগুলির মাধ্যমে **re-rendering** কমানো যায়।

### **কীভাবে `useCallback` এবং `memo` কাজ করে?**

`useCallback` একটি হুক যা ফাংশন ক্যাশে করে, অর্থাৎ এটি একটি ফাংশন তৈরি করার পরে, এটি পুনরায় সেই ফাংশনটিকে ব্যবহার করে যদি তার **ডিপেন্ডেন্সি** পরিবর্তিত না হয়। আর `memo` হুকটি একটি কম্পোনেন্টকে রেন্ডার থেকে বাঁচাতে সাহায্য করে, যদি তার প্রপস আগের মতোই থাকে। এই দুইটি একসাথে ব্যবহার করলে আপনি অনেক সময় unnecessary রেন্ডারিং এড়াতে পারেন।

### **প্রথম উদাহরণ: `useCallback` এবং `memo` দিয়ে রেন্ডারিং স্কিপ করা**

ধরা যাক, আপনার একটি অ্যাপ্লিকেশন আছে যেখানে **ShippingForm** কম্পোনেন্ট আছে, এবং আপনি চান যে এই কম্পোনেন্টটি কেবল তখনই রেন্ডার হোক যখন তার প্রপস পরিবর্তিত হয়, অন্যথায় না। আমরা একটি উদাহরণ দেখব যেখানে `useCallback` এবং `memo` একসাথে ব্যবহার করা হয়েছে।

#### **App.js:**

এখানে আপনি একটি সিম্পল `ProductPage` কম্পোনেন্ট ব্যবহার করছেন যেখানে একটি টগলেবল `theme` প্রপস দেওয়া হচ্ছে।

```javascript
import { useState } from 'react';
import ProductPage from './ProductPage.js';

export default function App() {
  const [isDark, setIsDark] = useState(false);
  return (
    <>
      <label>
        <input
          type="checkbox"
          checked={isDark}
          onChange={e => setIsDark(e.target.checked)}
        />
        Dark mode
      </label>
      <hr />
      <ProductPage
        referrerId="wizard_of_oz"
        productId={123}
        theme={isDark ? 'dark' : 'light'}
      />
    </>
  );
}
```

এখানে `ProductPage` কম্পোনেন্টের `theme` প্রপস পরিবর্তন করলে কম্পোনেন্টটি রেন্ডার হবে, কিন্তু `ShippingForm` কম্পোনেন্টের রেন্ডারিং কমানোর জন্য `useCallback` এবং `memo` ব্যবহার করা হবে।

#### **ProductPage.js:**

এখানে, `useCallback` হুক ব্যবহার করা হয়েছে যাতে `handleSubmit` ফাংশনটি **ক্যাশে** রাখা যায়। ফলে `ShippingForm` কম্পোনেন্ট কেবল তখনই রেন্ডার হবে যখন `productId` বা `referrer` পরিবর্তিত হবে।

```javascript
import { useCallback } from 'react';
import ShippingForm from './ShippingForm.js';

export default function ProductPage({ productId, referrer, theme }) {
  const handleSubmit = useCallback((orderDetails) => {
    post('/product/' + productId + '/buy', {
      referrer,
      orderDetails,
    });
  }, [productId, referrer]);

  return (
    <div className={theme}>
      <ShippingForm onSubmit={handleSubmit} />
    </div>
  );
}

function post(url, data) {
  console.log('POST /' + url);
  console.log(data);
}
```

এখানে `handleSubmit` ফাংশনকে `useCallback` দিয়ে ক্যাশে করা হয়েছে, এবং `ShippingForm` কম্পোনেন্টে **memo** হুক ব্যবহার করা হয়েছে, যাতে এটি কেবল তখনই রেন্ডার হয় যখন এর প্রপস পরিবর্তিত হয়।

#### **ShippingForm.js:**

এখানে, `ShippingForm` কম্পোনেন্টটি **artificially slowed down** (কৃত্রিমভাবে ধীর করা) হয়েছে যাতে আপনি দেখতে পারেন কিভাবে ফাংশন ক্যাশিং এবং কম্পোনেন্ট মেমোইজেশন পারফরম্যান্সে সহায়তা করে।

```javascript
import { memo, useState } from 'react';

const ShippingForm = memo(function ShippingForm({ onSubmit }) {
  const [count, setCount] = useState(1);

  console.log('[ARTIFICIALLY SLOW] Rendering <ShippingForm />');
  let startTime = performance.now();
  while (performance.now() - startTime < 500) {
    // Do nothing for 500 ms to emulate extremely slow code
  }

  function handleSubmit(e) {
    e.preventDefault();
    const formData = new FormData(e.target);
    const orderDetails = {
      ...Object.fromEntries(formData),
      count
    };
    onSubmit(orderDetails);
  }

  return (
    <form onSubmit={handleSubmit}>
      <p><b>Note: <code>ShippingForm</code> is artificially slowed down!</b></p>
      <label>
        Number of items:
        <button type="button" onClick={() => setCount(count - 1)}>–</button>
        {count}
        <button type="button" onClick={() => setCount(count + 1)}>+</button>
      </label>
      <label>
        Street:
        <input name="street" />
      </label>
      <label>
        City:
        <input name="city" />
      </label>
      <label>
        Postal code:
        <input name="zipCode" />
      </label>
      <button type="submit">Submit</button>
    </form>
  );
});

export default ShippingForm;
```

এখানে, `ShippingForm` কম্পোনেন্ট কৃত্রিমভাবে ধীর করা হয়েছে ৫০০ মিলিসেকেন্ডের জন্য, যেন আপনি দেখতে পারেন যখন **theme** টগল করবেন, **ShippingForm** কম্পোনেন্টটি পুনরায় রেন্ডার না হয়ে স্কিপ হবে।

### **এখানে কী ঘটছে?**

1. **কাউন্টার ইনক্রিমেন্ট** করার সময় `ShippingForm` কম্পোনেন্টটি রেন্ডার হবে কারণ তার স্টেট পরিবর্তিত হচ্ছে (বাটন ক্লিক করলে `count` আপডেট হয়)। কিন্তু যখন আপনি **theme** টগল করবেন, তখন `ShippingForm` পুনরায় রেন্ডার হবে না, কারণ `handleSubmit` ফাংশনটি পরিবর্তিত হয়নি। `handleSubmit` ফাংশনটি **`useCallback`** হুকের মাধ্যমে ক্যাশে করা হয়েছে, এবং তার **ডিপেন্ডেন্সি** (`productId` এবং `referrer`) পরিবর্তিত হয়নি।
   
2. **`memo` হুক** নিশ্চিত করে যে `ShippingForm` কেবল তখনই রেন্ডার হবে যখন এর প্রপস পরিবর্তিত হবে। কারণ এখানে `onSubmit` প্রপসটি একই ফাংশন রেফারেন্স পাচ্ছে, তাই রেন্ডারিং স্কিপ হচ্ছে।

### **সারাংশ:**
- **`useCallback`** ফাংশনকে ক্যাশে করে, ফলে ফাংশন পুনরায় তৈরি হয় না যদি তার ডিপেন্ডেন্সি পরিবর্তিত না হয়।
- **`memo`** কম্পোনেন্টকে ক্যাশে করে, ফলে কম্পোনেন্ট কেবল তখনই রেন্ডার হয় যখন তার প্রপস পরিবর্তিত হয়।
- এই দুইটি একসাথে ব্যবহার করলে unnecessary রেন্ডারিং এড়ানো যায় এবং পারফরম্যান্স উন্নত হয়, বিশেষ করে যখন আপনার অ্যাপ্লিকেশন ধীর রেন্ডারিং সমস্যায় আক্রান্ত।

এভাবেই `useCallback` এবং `memo` হুকগুলো ব্যবহার করে আপনি কম্পোনেন্ট রেন্ডারিংকে অপটিমাইজ করতে পারেন।


নিশ্চিত! আমি আপনাকে **`useCallback`** এবং **`memo`** হুক ব্যবহার করে কিভাবে রেন্ডারিং অপটিমাইজেশন করা যায়, সেটি বুঝানোর জন্য  ধাপে ধাপে ব্যাখ্যা করবো।

### **ধাপ ১: `useState` দিয়ে একটি সাধারণ অ্যাপ তৈরি করা**

প্রথমে, আমরা একটি সাধারণ অ্যাপ তৈরি করবো যেখানে একটি বাটন থাকবে, যেটি ক্লিক করলে একটি কাউন্টার মান বাড়াবে। এর পাশাপাশি, আমরা একটি **`theme`** (অর্থাৎ আলোর মোড বা ডার্ক মোড) টগল করার ফিচার যোগ করবো।

```javascript
import { useState } from 'react';

export default function App() {
  const [isDark, setIsDark] = useState(false);  // Dark mode স্টেট

  // Counter স্টেট
  const [counter, setCounter] = useState(0);

  return (
    <>
      <label>
        <input
          type="checkbox"
          checked={isDark}
          onChange={e => setIsDark(e.target.checked)}  // Theme টগল করার ফাংশন
        />
        Dark mode
      </label>

      <hr />

      <h1>Counter: {counter}</h1>

      <button onClick={() => setCounter(counter + 1)}>
        Increment Counter
      </button>

      <hr />

      {/* ProductPage কম্পোনেন্ট */}
      <ProductPage theme={isDark ? 'dark' : 'light'} />
    </>
  );
}
```

এখানে আমরা দুটি স্টেট ব্যবহার করেছি:
1. `isDark`: এটি আমাদের `theme` টগল করতে সাহায্য করবে (ডার্ক মোড বা লাইট মোড)।
2. `counter`: এটি কাউন্টার ভ্যালু বৃদ্ধি করবে।

এখন, যখন আপনি **theme** টগল করবেন বা **counter** বাড়াবেন, `ProductPage` কম্পোনেন্টটি পুনরায় রেন্ডার হবে। কিন্তু আমরা চাই, যে প্রপস **theme** বদলালেও **ShippingForm** কম্পোনেন্টটি রেন্ডার না হয় যদি প্রপস পরিবর্তন না হয়।

### **ধাপ ২: `ShippingForm` কম্পোনেন্ট তৈরি করা**

এখন, আমরা `ShippingForm` নামের একটি কম্পোনেন্ট তৈরি করবো। এটি একটি ফর্ম থাকবে যেটি কাউন্টারকে দেখাবে এবং ফর্ম সাবমিট করলে `onSubmit` ফাংশন কল করবে। এখানে আমরা `memo` হুক ব্যবহার করবো, যাতে কম্পোনেন্টটি unnecessary রেন্ডার না হয়।

```javascript
import { memo, useState } from 'react';

const ShippingForm = memo(function ShippingForm({ onSubmit, theme }) {
  const [count, setCount] = useState(1);

  console.log('[Rendering] ShippingForm');  // রেন্ডারিং চেক করতে লগ করা হবে

  return (
    <div className={theme}>
      <h2>Shipping Form</h2>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increase Count</button>
      <button onClick={onSubmit}>Submit</button>
    </div>
  );
});

export default ShippingForm;
```

এখানে, `memo` হুক ব্যবহার করা হয়েছে যাতে `ShippingForm` কেবল তখনই রেন্ডার হয় যখন তার **প্রপস** পরিবর্তিত হয়। অর্থাৎ, যখন **`onSubmit`** বা **`theme`** প্রপস পরিবর্তন হবে, তখনই কম্পোনেন্ট রেন্ডার হবে। অন্যথায়, এটি আগের মতই ক্যাশে থাকবে এবং পুনরায় রেন্ডার হবে না।

### **ধাপ ৩: `useCallback` ব্যবহার করা**

এখন, আমরা `ProductPage` কম্পোনেন্টে `useCallback` হুক ব্যবহার করবো। **`useCallback`** হুকের মাধ্যমে আমরা `handleSubmit` ফাংশনটিকে ক্যাশে করবো, যাতে এটি কেবল তখনই পরিবর্তিত হয় যখন তার **ডিপেন্ডেন্সি** (যেমন `productId`, `referrer`) পরিবর্তিত হয়। এতে করে আমরা unnecessary রেন্ডারিং থেকে বাঁচতে পারবো।

```javascript
import { useCallback } from 'react';
import ShippingForm from './ShippingForm';  // ShippingForm কম্পোনেন্টটি ইম্পোর্ট

export default function ProductPage({ theme }) {
  // handleSubmit ফাংশনটি useCallback দিয়ে ক্যাশে করা হচ্ছে
  const handleSubmit = useCallback(() => {
    console.log('Form submitted');
  }, []);  // এখানে কোন ডিপেন্ডেন্সি নেই, তাই এটি কেবল একবার তৈরি হবে।

  return (
    <div>
      <ShippingForm onSubmit={handleSubmit} theme={theme} />
    </div>
  );
}
```

এখানে:
- **`useCallback`** ব্যবহার করা হয়েছে যাতে `handleSubmit` ফাংশনটি শুধুমাত্র একবার তৈরি হয় এবং তার **ডিপেন্ডেন্সি** পরিবর্তিত না হলে আগের ফাংশনটি রিইউজ হবে।
- **`ShippingForm`** কম্পোনেন্টটি `memo` দিয়ে মোড়ানো, যাতে এটি শুধুমাত্র তখনই রেন্ডার হয় যখন তার প্রপস পরিবর্তিত হবে।

### **ধাপ ৪: পুরো অ্যাপটি একসাথে কাজ করা**

এখন, সব কিছু একসাথে রাখলে আমাদের অ্যাপটি দেখতে এরকম হবে:

```javascript
import { useState } from 'react';
import ProductPage from './ProductPage';  // ProductPage কম্পোনেন্ট ইম্পোর্ট

export default function App() {
  const [isDark, setIsDark] = useState(false);  // Dark mode স্টেট

  const [counter, setCounter] = useState(0);

  return (
    <>
      <label>
        <input
          type="checkbox"
          checked={isDark}
          onChange={e => setIsDark(e.target.checked)}
        />
        Dark mode
      </label>

      <hr />

      <h1>Counter: {counter}</h1>

      <button onClick={() => setCounter(counter + 1)}>Increment Counter</button>

      <hr />

      {/* ProductPage কম্পোনেন্ট */}
      <ProductPage theme={isDark ? 'dark' : 'light'} />
    </>
  );
}
```

এখন, **`ProductPage`** এবং **`ShippingForm`** কম্পোনেন্ট যখন রেন্ডার হবে:
1. **Counter** আপডেট হলে **ShippingForm** রেন্ডার হবে কারণ `count` পরিবর্তিত হচ্ছে।
2. **Theme** টগল করা হলে **ShippingForm** রেন্ডার হবে না কারণ `handleSubmit` ফাংশনটি `useCallback` দ্বারা ক্যাশে করা হয়েছে এবং তার ডিপেন্ডেন্সি পরিবর্তিত হয়নি।

### **চূড়ান্ত ফলাফল:**
- **`ShippingForm`** কম্পোনেন্ট কেবল তখনই রেন্ডার হবে যখন **`onSubmit`** বা **`theme`** পরিবর্তিত হবে।
- **`handleSubmit`** ফাংশনটি **`useCallback`** দিয়ে ক্যাশে করা হয়েছে, যাতে এটি কেবল একবার তৈরি হয় এবং তারপর পুনরায় রেন্ডার হবে না।

এভাবে **`useCallback`** এবং **`memo`** হুক ব্যবহার করে আপনি **unnecessary re-renders** কমাতে পারবেন এবং আপনার অ্যাপ্লিকেশনের পারফরম্যান্স উন্নত করতে পারবেন।

### **সংক্ষেপে:**
- **`useCallback`**: ফাংশন ক্যাশে করতে ব্যবহৃত হয়, যাতে ফাংশন শুধুমাত্র প্রয়োজনীয় সময়ে পরিবর্তিত হয়।
- **`memo`**: কম্পোনেন্ট ক্যাশে করে, যাতে প্রপস পরিবর্তিত না হলে কম্পোনেন্ট পুনরায় রেন্ডার না হয়।


### উদাহরণ: **`useCallback` এবং `memo` সহ একটি সিম্পল টাস্ক ম্যানেজমেন্ট অ্যাপ**

এই উদাহরণে, আমরা একটি টাস্ক ম্যানেজমেন্ট অ্যাপ তৈরি করবো যেখানে একটি টাস্ক লিস্ট থাকবে, এবং আপনি যে কোনো টাস্ক সম্পন্ন করলে, সেটি ফিল্টার বা ডিলিট করা যাবে।

### **ধাপ ১: `TaskList` কম্পোনেন্ট তৈরি করা**

প্রথমে, আমরা একটি `TaskList` কম্পোনেন্ট তৈরি করব যেটি বিভিন্ন টাস্ক শো করবে। এখানে, `TaskList` কম্পোনেন্টটি একটি **`memo`** হুকের মাধ্যমে মেমোইজ (ক্যাশে) করা হবে যাতে কম্পোনেন্টটি শুধুমাত্র প্রপস পরিবর্তিত হলে রেন্ডার হয়।

```javascript
import React, { memo } from 'react';

const TaskList = memo(({ tasks, onComplete, onDelete }) => {
  console.log("Rendering TaskList"); // দেখার জন্য যে TaskList কম্পোনেন্টটি কখন রেন্ডার হচ্ছে

  return (
    <div>
      <h2>Task List</h2>
      <ul>
        {tasks.map(task => (
          <li key={task.id}>
            {task.name}
            <button onClick={() => onComplete(task.id)}>Complete</button>
            <button onClick={() => onDelete(task.id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
});

export default TaskList;
```

**ব্যাখ্যা:**

1. **`memo` হুক:** `TaskList` কম্পোনেন্টের চারপাশে `memo` হুক ব্যবহার করা হয়েছে, এর মানে হচ্ছে কম্পোনেন্টটি শুধুমাত্র তখনই রেন্ডার হবে যখন তার প্রপস পরিবর্তিত হবে। যদি টাস্ক লিস্টে কোনো পরিবর্তন না হয়, তাহলে `TaskList` কম্পোনেন্টটি পুনরায় রেন্ডার হবে না। এর ফলে অ্যাপের পারফরম্যান্স উন্নত হবে।

2. **`onComplete` এবং `onDelete` ফাংশন:** যখন আপনি টাস্ক সম্পন্ন করবেন অথবা ডিলিট করবেন, তখন এই ফাংশনগুলো কল হবে। এগুলোর মধ্যে `onComplete` এবং `onDelete` টাস্কের আইডি পাঠাবে।

### **ধাপ ২: `useCallback` দিয়ে ফাংশন ক্যাশে করা**

এখন, আমরা `TaskList` কম্পোনেন্টে যে ফাংশনগুলো প্রপস হিসেবে পাঠাচ্ছি, সেগুলোর মধ্যে **`useCallback`** হুক ব্যবহার করবো, যাতে ওই ফাংশনগুলো শুধুমাত্র তখনই পরিবর্তিত হয় যখন তাদের নির্দিষ্ট ডিপেন্ডেন্সি পরিবর্তিত হয়। এটি unnecessary re-renders কমাতে সাহায্য করবে।

```javascript
import React, { useState, useCallback } from 'react';
import TaskList from './TaskList';

export default function TaskApp() {
  const [tasks, setTasks] = useState([
    { id: 1, name: 'Do homework' },
    { id: 2, name: 'Wash dishes' },
    { id: 3, name: 'Read a book' }
  ]);

  // Task complete করার ফাংশন
  const completeTask = useCallback((taskId) => {
    setTasks(prevTasks => 
      prevTasks.map(task => 
        task.id === taskId ? { ...task, completed: true } : task
      )
    );
  }, [setTasks]);  // `setTasks` আসলেই পরিবর্তিত হয় না, কিন্তু `useCallback` দিয়ে এটি ক্যাশে করেছি

  // Task delete করার ফাংশন
  const deleteTask = useCallback((taskId) => {
    setTasks(prevTasks => prevTasks.filter(task => task.id !== taskId));
  }, [setTasks]);

  return (
    <div>
      <TaskList tasks={tasks} onComplete={completeTask} onDelete={deleteTask} />
    </div>
  );
}
```

**ব্যাখ্যা:**

1. **`useCallback` হুক:** `completeTask` এবং `deleteTask` ফাংশনগুলোকে `useCallback` দিয়ে ক্যাশে করা হয়েছে। এই ফাংশনগুলো কেবল তখনই নতুনভাবে তৈরি হবে যখন তাদের ডিপেন্ডেন্সি পরিবর্তিত হবে, অর্থাৎ এখানে `setTasks`।

2. **`setTasks`**: React এর `useState` হুকের মাধ্যমে `tasks` স্টেট আপডেট করা হচ্ছে। যদিও `setTasks` এর মান কখনো পরিবর্তিত হয় না, কিন্তু `useCallback` হুক ব্যবহার করে ফাংশনগুলোকে ক্যাশে করা হয়েছে। এর ফলে যখন টাস্কের আইডি বা অন্যান্য ডিপেন্ডেন্সি পরিবর্তন হয়, তখনই এই ফাংশনগুলো নতুনভাবে তৈরি হবে।

### **ধাপ ৩: প্রোডাকশন কোডে ফাংশন এবং কম্পোনেন্ট ব্যবহার**

এখন, যখন `TaskApp` কম্পোনেন্ট রেন্ডার হয়, তখন **`TaskList`** কম্পোনেন্টটি রেন্ডার হবে কিন্তু কেবল তখনই যখন **`tasks`** বা **`onComplete`** বা **`onDelete`** প্রপসের কোনো পরিবর্তন হবে।

### **কেন এবং কখন ব্যবহার করবেন:**

1. **`useCallback` ব্যবহারের কারণ:**  
   যখন আপনি কোন ফাংশনকে প্রপস হিসেবে চাইল্ড কম্পোনেন্টে পাঠান, এবং সেই ফাংশনটি চাইল্ড কম্পোনেন্টে অপ্রয়োজনীয় রেন্ডার সৃষ্টি করছে, তখন **`useCallback`** হুক ব্যবহার করা উচিত। এটি ফাংশনটিকে ক্যাশে করে রাখবে এবং শুধুমাত্র যখন প্রয়োজন হবে তখন পুনরায় তৈরি করবে। এর ফলে আপনার অ্যাপের পারফরম্যান্স বৃদ্ধি পাবে।

   - উদাহরণ: `onComplete` এবং `onDelete` ফাংশনগুলোকে `TaskList` কম্পোনেন্টে পাঠানো হচ্ছে, যেগুলো যদি প্রতিবার নতুন করে তৈরি হত, তবে `TaskList` কম্পোনেন্টটি অপ্রয়োজনীয়ভাবে রেন্ডার হতো। **`useCallback`** ফাংশনগুলোকে শুধুমাত্র প্রয়োজনীয় ডিপেন্ডেন্সির উপর ভিত্তি করে ক্যাশে করে রাখে, যাতে তারা নতুন করে তৈরি না হয়।

2. **`memo` ব্যবহারের কারণ:**  
   যখন আপনি কোন কম্পোনেন্টে **অনেক বেশি রেন্ডারিং** দেখেন এবং মনে হয় কম্পোনেন্টটি প্রপস পরিবর্তন ছাড়া বারবার রেন্ডার হচ্ছে, তখন **`memo`** ব্যবহার করে সেই কম্পোনেন্টটি ক্যাশে করা উচিত। এটি শুধুমাত্র তখনই রেন্ডার হবে যখন তার প্রপস বা স্টেট পরিবর্তিত হবে।

   - উদাহরণ: `TaskList` কম্পোনেন্টটি `memo` হুক দিয়ে মেমোইজড। এর ফলে যখন **`tasks`** পরিবর্তিত হয়, তখনই কম্পোনেন্টটি রেন্ডার হবে, কিন্তু যখন **`tasks`** পরিবর্তিত না হয়, তখন এটি আগের রেন্ডার থেকে ক্যাশে করা কম্পোনেন্টই দেখাবে।

### **নিষ্কর্ষ (Conclusion):**
1. **`useCallback`** হুক ব্যবহার করুন যখন:
   - আপনি ফাংশনকে একটি চাইল্ড কম্পোনেন্টে পাঠাচ্ছেন এবং সেই ফাংশনটি অপ্রয়োজনীয়ভাবে কম্পোনেন্টের রেন্ডার বৃদ্ধি করছে।
   - আপনি নিশ্চিত করতে চান যে ফাংশনটি শুধুমাত্র প্রয়োজনীয় সময়ে নতুনভাবে তৈরি হবে।

2. **`memo`** হুক ব্যবহার করুন যখন:
   - আপনি চান একটি কম্পোনেন্ট শুধুমাত্র তখনই রেন্ডার হোক যখন তার প্রপস বা স্টেট পরিবর্তিত হয়। এটি আপনার কম্পোনেন্টের পারফরম্যান্স উন্নত করবে।

এভাবে **`useCallback`** এবং **`memo`** হুক ব্যবহার করে আপনি আপনার React অ্যাপের পারফরম্যান্স অপটিমাইজ করতে পারবেন এবং অপ্রয়োজনীয় রেন্ডারিং কমাতে সক্ষম হবেন।

### **Memoized Callback দিয়ে State আপডেট করা এবং Excessive Effect রেন্ডারিং বন্ধ করা**

### 1. **Memoized Callback দিয়ে State আপডেট করা**

যখন আপনি React-এ `useCallback` হুক ব্যবহার করেন, তখন কখনো কখনো আপনাকে পুরনো state-এর ভিত্তিতে নতুন state আপডেট করতে হতে পারে। সাধারণত, আপনি যখন state-এ কিছু আপডেট করেন, তখন সেই state-টিকে dependency হিসেবে পাঠানো হয়। কিন্তু, যদি আপনি state-এর আগের মান ব্যবহার করে নতুন state গণনা করেন, তাহলে `useCallback` ব্যবহার করে state আপডেট করার একটি উন্নত উপায় রয়েছে, যা unnecessary re-renders এড়াতে সাহায্য করে।

#### **উদাহরণ ১: `todos` কে Dependency হিসেবে ব্যবহার করা**

```javascript
function TodoList() {
  const [todos, setTodos] = useState([]);

  const handleAddTodo = useCallback((text) => {
    const newTodo = { id: nextId++, text };
    setTodos([...todos, newTodo]); // `todos` কে dependency হিসেবে দেয়া হয়েছে
  }, [todos]);
}
```

এখানে, `handleAddTodo` ফাংশনটি `todos` কে dependency হিসেবে গ্রহণ করছে। প্রতিবার যখন `todos` পরিবর্তিত হয়, তখন `handleAddTodo` নতুন ফাংশন তৈরি হবে। তবে, `todos` এর মানের উপর ভিত্তি করে নতুন তালিকা তৈরি করা হচ্ছে। এর ফলে, প্রতিবার `todos` পরিবর্তিত হলে নতুন ফাংশন তৈরি হওয়া এবং unnecessary re-renders হতে পারে।

#### **সমস্যা কী?**
- এই ক্ষেত্রে `todos` কে dependency হিসেবে দেয়া হয়েছে, যার ফলে প্রতিবার যখন `todos` আপডেট হবে, তখন `handleAddTodo` নতুন করে তৈরি হবে এবং unnecessary re-render হবে।

#### **উন্নত পদ্ধতি: Updater Function ব্যবহার করা**

আপনি `todos` কে dependency হিসেবে না দিয়ে, `setTodos` এর মধ্যে একটি updater function পাঠাতে পারেন। এটি React কে বলে দিবে কিভাবে আগের state ব্যবহার করে নতুন state তৈরি করতে হবে।

```javascript
function TodoList() {
  const [todos, setTodos] = useState([]);

  const handleAddTodo = useCallback((text) => {
    const newTodo = { id: nextId++, text };
    setTodos(todos => [...todos, newTodo]); // এখানে `todos` কে dependency হিসেবে নেয়া হয়নি
  }, []); // ✅ এখন `todos` কে dependency হিসেবে দিতে হবে না
}
```

এখানে, `setTodos` একটি updater function নিচ্ছে যা আগের `todos` ব্যবহার করে নতুন তালিকা তৈরি করছে। এর ফলে, `todos` পরিবর্তিত হওয়ার পরেও `handleAddTodo` ফাংশনটি আর নতুনভাবে তৈরি হবে না। শুধু প্রয়োজনীয় আপডেট হবে, যা কর্মক্ষমতা উন্নত করবে।

### 2. **Effect-এর অযথা পুনরায় রেন্ডারিং বন্ধ করা**

যখন আপনি `useEffect` ব্যবহার করেন, তখন আপনাকে যে ফাংশনটি কল করতে হবে, তার উপর নির্ভর করে dependencies ঠিকভাবে সংজ্ঞায়িত করা উচিত। যদি আপনার `useEffect` এমনভাবে সেট করা হয় যে এটি অযথা বার বার ট্রিগার হয়, তবে কর্মক্ষমতা সমস্যা হতে পারে।

#### **উদাহরণ ২: `Effect`-এ Function ব্যবহার**

```javascript
function ChatRoom({ roomId }) {
  const [message, setMessage] = useState('');

  function createOptions() {
    return {
      serverUrl: 'https://localhost:1234',
      roomId: roomId
    };
  }

  useEffect(() => {
    const options = createOptions();
    const connection = createConnection(options);
    connection.connect();
    return () => connection.disconnect();
  }, [createOptions]); // 🔴 সমস্যা: প্রতিবার `createOptions` পরিবর্তিত হচ্ছে
}
```

এখানে, `createOptions` ফাংশনটি `useEffect` এর মধ্যে ব্যবহার করা হচ্ছে। কিন্তু `createOptions` কে যদি dependency হিসেবে ব্যবহার করা হয়, তাহলে `createOptions` ফাংশনটি প্রতিবার রেন্ডার হওয়াতে `useEffect` বার বার ট্রিগার হবে, যার ফলে unnecessary connection তৈরি হবে এবং `disconnect` হবে, যা কর্মক্ষমতা খারাপ করবে।

#### **উন্নত পদ্ধতি: `useCallback` ব্যবহার করে Function Memoize করা**

আপনি `createOptions` ফাংশনটি `useCallback` হুকের মাধ্যমে memoize করতে পারেন, যাতে ফাংশনটি শুধু তখনই পরিবর্তিত হয় যখন `roomId` পরিবর্তিত হয়। এটি unnecessary re-renders এবং `useEffect` পুনরায় ট্রিগারিং বন্ধ করবে।

```javascript
function ChatRoom({ roomId }) {
  const [message, setMessage] = useState('');

  const createOptions = useCallback(() => {
    return {
      serverUrl: 'https://localhost:1234',
      roomId: roomId
    };
  }, [roomId]); // ✅ শুধুমাত্র `roomId` পরিবর্তিত হলে `createOptions` পরিবর্তিত হবে

  useEffect(() => {
    const options = createOptions();
    const connection = createConnection(options);
    connection.connect();
    return () => connection.disconnect();
  }, [createOptions]); // ✅ এখন `createOptions` শুধুমাত্র যখন পরিবর্তিত হবে তখনই পরিবর্তিত হবে
}
```

এখানে `createOptions` ফাংশনটি `useCallback` এর মাধ্যমে memoize করা হয়েছে, যাতে এটি শুধুমাত্র তখনই পরিবর্তিত হয় যখন `roomId` পরিবর্তিত হয়। এটি `useEffect` এর unnecessary পুনরায় ট্রিগারিং বন্ধ করে।

#### **আরও সহজ সমাধান: Function-টিকে সরাসরি `useEffect` এর মধ্যে রাখা**

আপনি `createOptions` ফাংশনটিকে `useEffect` এর মধ্যে সরাসরি ব্যবহার করতে পারেন, যাতে আর `useCallback` এর প্রয়োজন না হয় এবং কোডটি আরও পরিষ্কার হয়।

```javascript
function ChatRoom({ roomId }) {
  const [message, setMessage] = useState('');

  useEffect(() => {
    function createOptions() {
      return {
        serverUrl: 'https://localhost:1234',
        roomId: roomId
      };
    }

    const options = createOptions();
    const connection = createConnection(options);
    connection.connect();
    return () => connection.disconnect();
  }, [roomId]); // ✅ শুধুমাত্র `roomId` পরিবর্তিত হলে `useEffect` চলবে
}
```

এখানে, `createOptions` ফাংশনটি সরাসরি `useEffect` এর মধ্যে রাখা হয়েছে, এবং `roomId` পরিবর্তিত হলে `useEffect` পুনরায় চলবে।

### **সারাংশ**
- **`useCallback`**: এটি একটি ফাংশনকে memoize করে, যাতে শুধুমাত্র প্রয়োজনীয় পরিবর্তন হলে সেই ফাংশনটি নতুন করে তৈরি হয়।
- **State update with updater function**: `setTodos(todos => [...todos, newTodo])` ব্যবহার করে আমরা state update করতে পারি আগের state-এর উপর ভিত্তি করে, এবং এতে unnecessary dependency এবং rerenders এড়ানো যায়।
- **Effect dependencies**: যখন আপনি `useEffect` ব্যবহার করেন, তখন ফাংশনগুলি ঠিকভাবে memoize বা সরাসরি `useEffect` এর মধ্যে রাখতে হবে, যাতে unnecessary rerenders এবং function call avoid হয়।
