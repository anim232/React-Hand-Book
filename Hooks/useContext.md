### **`useContext` Hook**

**`useContext`** হুকটি React এর একটি শক্তিশালী টুল যা কম্পোনেন্টগুলির মধ্যে ডেটা শেয়ার করার জন্য ব্যবহৃত হয়। এটি **Context API** এর অংশ এবং এটি বিশেষভাবে উপকারী যখন আপনার অ্যাপ্লিকেশনে গ্লোবাল স্টেট ম্যানেজমেন্টের প্রয়োজন হয়, বা যখন আপনি নির্দিষ্ট ডেটা একাধিক কম্পোনেন্টে শেয়ার করতে চান, তবে `useContext` ব্যবহার করা হয়।

---

### **কীভাবে কাজ করে `useContext`?**

`useContext` হুকটি একটি Context এর মান (value) পড়তে এবং সাবস্ক্রাইব করতে ব্যবহৃত হয়। 

`useContext` একটি বিশেষ context কে রিড করতে সক্ষম যা আপনি `createContext` দিয়ে তৈরি করেছেন এবং এটি **Context.Provider** এর মাধ্যমে কম্পোনেন্ট ট্রি-তে পাস করা হয়েছে।

---

### **সিনট্যাক্স:**

```javascript
const value = useContext(SomeContext);
```

এখানে:

- **`SomeContext`**: এটি সেই context যার মান আপনি পড়তে চান। আপনি এটি **`createContext`** দিয়ে তৈরি করেছেন।
- **`value`**: এটি সেই context এর মান যা আপনি উপরের context provider থেকে পেয়েছেন।

---

### **ব্যবহার (Usage)**

1. **Context তৈরি করা**: প্রথমে একটি context তৈরি করতে হবে `createContext` ব্যবহার করে।

    ```javascript
    import { createContext } from 'react';

    const ThemeContext = createContext('light');
    ```

2. **Context প্রদান করা (Providing Context)**: এরপর **`ThemeContext.Provider`** ব্যবহার করে context এর মান প্রদান করুন।

    ```javascript
    import { ThemeContext } from './ThemeContext';

    function App() {
      return (
        <ThemeContext.Provider value="dark">
          <MyComponent />
        </ThemeContext.Provider>
      );
    }
    ```

3. **Context ব্যবহার করা (Consuming Context)**: অবশেষে, যে কম্পোনেন্টে আপনি context এর মান ব্যবহার করতে চান সেখানে **`useContext`** ব্যবহার করবেন।

    ```javascript
    import { useContext } from 'react';
    import { ThemeContext } from './ThemeContext';

    function MyComponent() {
      const theme = useContext(ThemeContext);
      return <div>The current theme is {theme}</div>;
    }
    ```

এই উদাহরণে, `MyComponent` কম্পোনেন্টটি `ThemeContext` থেকে `theme` মানটি পাবে এবং সেটি UI তে প্রদর্শন করবে। `ThemeContext.Provider` এর মাধ্যমে `value="dark"` পাস করা হয়েছিল, তাই `theme` হবে `"dark"`।

---

### **`useContext` হুকের প্যারামিটার**

- **SomeContext**: এটি সেই context যেটি আপনি `createContext` দিয়ে তৈরি করেছেন। 
    - এটি context এর মান ধারণ করে না, তবে এটি একটি রেফারেন্স হিসেবে কাজ করে, যা কম্পোনেন্টগুলিকে জানায় কী ধরনের তথ্য তারা প্রদান বা গ্রহণ করতে পারে।
- **returns**: `useContext` সেই context এর মান রিটার্ন করে, যেটি আপনি **`Provider`** দিয়ে পাস করেছেন।

---

### **`useContext` থেকে রিটার্ন কী হয়?**

`useContext` হুকটি যে value রিটার্ন করে তা হলো:

- **context value**: এটি সেই মান যা আপনি context provider থেকে পেয়েছেন।
- যদি কোনো **Provider** উপরে না থাকে, তবে **createContext** এর ডিফল্ট মান রিটার্ন হবে।

**উদাহরণ:**

```javascript
import { useContext } from 'react';

const ThemeContext = createContext('light');  // Default value 'light'

function MyComponent() {
  const theme = useContext(ThemeContext);
  return <div>The current theme is {theme}</div>;
}
```

এখানে, `ThemeContext` এর ডিফল্ট মান `'light'` হওয়ায়, যদি `ThemeContext.Provider` কম্পোনেন্টটি উপরে না থাকে, তবে `theme` এর মান `'light'` হবে।

---

### **`useContext` ব্যবহার করার সুবিধা (Benefits)**

1. **সহজ ডেটা শেয়ারিং**: 
   - যখন আপনি context ব্যবহার করেন, তখন একটি কম্পোনেন্ট থেকে অন্য কম্পোনেন্টে ডেটা শেয়ার করা অনেক সহজ হয়ে যায়, বিশেষ করে যেখানে আপনি props chaining বা prop drilling ব্যবহার করতে চাচ্ছেন না।
   
2. **গ্লোবাল স্টেট ম্যানেজমেন্ট**:
   - Context API এবং `useContext` হুক একসাথে গ্লোবাল স্টেট ম্যানেজমেন্ট করতে সাহায্য করে। এর মাধ্যমে আপনি অ্যাপ্লিকেশনের বিভিন্ন অংশে একে অপরের সাথে সম্পর্কিত ডেটা শেয়ার করতে পারেন।

3. **React রেন্ডারিং**:
   - `useContext` হুকটি আপনাকে ঐতিহাসিক (current) context মান প্রাপ্তি নিশ্চিত করে এবং রিঅ্যাক্ট সেই কম্পোনেন্টগুলোকে স্বয়ংক্রিয়ভাবে রেন্ডার করবে যেগুলো context এর মানে সাবস্ক্রাইব করেছে।

---

### **কোথায় এবং কখন ব্যবহার করবেন `useContext`?**

1. **গ্লোবাল স্টেট শেয়ারিং**:
   - যদি আপনার অ্যাপ্লিকেশনে কিছু ডেটা বা স্টেট একাধিক কম্পোনেন্টের মধ্যে শেয়ার করতে হয় এবং আপনি props পাস করার পরিবর্তে সরাসরি context থেকে তা পেতে চান, তখন `useContext` ব্যবহার করবেন।

   **উদাহরণ**: ইউজার প্রোফাইল, লোগিন স্ট্যাটাস, থিম সিলেকশন ইত্যাদি।

2. **কম্পোনেন্ট ট্রি এডিটিং**:
   - যখন আপনি আপনার কম্পোনেন্ট ট্রি তে একাধিক স্তরে ডেটা পাঠাতে চান এবং সেই ডেটা প্রাপ্তি কম্পোনেন্টগুলিতে সরাসরি উপভোগ করতে চান, তখন `useContext` এর মাধ্যমে এটি সহজ করা যায়।

3. **নির্দিষ্ট ডেটা অ্যাক্সেস করা**:
   - `useContext` আপনাকে নির্দিষ্ট context থেকে ডেটা সরাসরি অ্যাক্সেস করতে দেয়, যাতে আপনার কম্পোনেন্টে প্রয়োজনীয় ডেটা পাওয়া যায় এবং পুনরায় render করার প্রয়োজন পড়ে না।

---

### **`useContext` ব্যবহারের সাবধানতা (Caveats)**

1. **Provider এর উপরে থাকা প্রয়োজন**:
   - `useContext` হুকটি শুধুমাত্র তখন কাজ করবে যখন আপনার কম্পোনেন্টের উপরে একটি `Context.Provider` থাকবে, যেটি সংশ্লিষ্ট context এর মান পাস করবে।
   
   **যেমন**:
   ```javascript
   <ThemeContext.Provider value="dark">
     <MyComponent />
   </ThemeContext.Provider>
   ```
   এখানে `MyComponent` এর ভিতরে `useContext(ThemeContext)` কাজ করবে কারণ এটি `Provider` এর ভিতরে রয়েছে।

2. **Memoization স্কিপ করবেন না**:
   - Context এর মান পরিবর্তন হলে, React সেই কম্পোনেন্টগুলিকে রেন্ডার করবে যেগুলি ঐ context-এ সাবস্ক্রাইব করেছে। তবে, memoization এর মাধ্যমে রেন্ডার বন্ধ করার চেষ্টা করলে তা context এর পরিবর্তনকে থামাতে পারে না।

3. **ডুপ্লিকেট মডিউল সমস্যা**:
   - যদি আপনার বিল্ড সিস্টেমে ডুপ্লিকেট মডিউল থাকে (যেমন symlinks এর কারণে), এটি context ব্রেক করতে পারে। একই context (context object) পাস করা জরুরি, কারণ এটি `===` কমপ্যারিসন দ্বারা যাচাই হয়।

---



`useContext` React এ context এর মাধ্যমে ডেটা শেয়ার করার একটি অত্যন্ত গুরুত্বপূর্ণ টুল। এটি বিভিন্ন কম্পোনেন্টের মধ্যে ডেটা শেয়ার করতে, গ্লোবাল স্টেট ম্যানেজমেন্ট করতে এবং সহজে ডেটা সাবস্ক্রাইব করতে ব্যবহৃত হয়। এটি বিশেষভাবে কার্যকরী যেখানে আপনি props চেইনিং বা prop drilling এ জটিলতা এড়াতে চান।


### `useContext` সম্পর্কে বিস্তারিত ব্যাখ্যা

`useContext` হল একটি React Hook যা আপনাকে React Context থেকে ডেটা পড়তে এবং subscribe করতে দেয়। এটি এমন সময়ে ব্যবহার করা হয় যখন আপনি একটি বড় অ্যাপ্লিকেশনে কোনো ডেটা বা স্টেট অনেক স্তরের (level) মধ্যে পাঠাতে চান এবং আপনি চাইছেন না যে প্রতিটি কম্পোনেন্টকে এই ডেটা বা স্টেট প্রপস হিসেবে পাঠানো হোক। `useContext` এর মাধ্যমে, আপনি একটি global state বা value অ্যাক্সেস করতে পারেন যেটি React কম্পোনেন্ট ট্রীতে কোথাও একটি `Context.Provider` দ্বারা প্রদান করা হয়।

এখন, আসুন একে একে `useContext` এর ব্যবহার, সুবিধা, এবং কখন এবং কোথায় এটি ব্যবহার করা উচিত সেটা বুঝি।

---

### **কেন এবং কোথায় ব্যবহার করবেন?**

`useContext` ব্যবহার করতে হলে, প্রথমে একটি context তৈরি করতে হয় `createContext` দ্বারা। এর মাধ্যমে আপনি একটি `Context.Provider` তৈরি করবেন যা আপনার অ্যাপ্লিকেশনের উপরে একটি value প্রদান করবে। তারপর, সেই value কে আপনার কম্পোনেন্টে `useContext` এর মাধ্যমে পড়তে পারবেন।

#### ১. **Context তৈরি করা**

প্রথমেই একটি context তৈরি করতে হবে:

```js
import { createContext } from 'react';

const ThemeContext = createContext('light');  // Default value 'light'
```

এখানে `ThemeContext` তৈরি করা হয়েছে, এবং এর ডিফল্ট মান হচ্ছে `'light'`। 

#### ২. **Context.Provider ব্যবহার করা**

এখন, আমরা `ThemeContext.Provider` ব্যবহার করে ডেটা প্রদান করব:

```js
import { ThemeContext } from './contextFile';  // Import the created context

function MyApp() {
  return (
    <ThemeContext.Provider value="dark">
      <Form />
    </ThemeContext.Provider>
  );
}
```

এখানে `MyApp` কম্পোনেন্টে `ThemeContext.Provider` ব্যবহার করা হয়েছে, যেখানে আমরা `value="dark"` দিয়েছি। এর মানে হচ্ছে, `ThemeContext` যে কোনো কম্পোনেন্টে ব্যবহৃত হলে, সেখানে `"dark"` value পাওয়া যাবে।

#### ৩. **useContext ব্যবহার করা**

এখন, যে কম্পোনেন্টে আপনি সেই context এর ডেটা চান, সেখানে `useContext` ব্যবহার করতে হবে:

```js
import { useContext } from 'react';
import { ThemeContext } from './contextFile';  // Import the context

function Button() {
  const theme = useContext(ThemeContext);  // Get the context value
  const className = `button-${theme}`;  // Use the context value
  return (
    <button className={className}>
      Sign up
    </button>
  );
}
```

এখানে, `Button` কম্পোনেন্ট `ThemeContext` থেকে value পেয়েছে যেটি `"dark"` ছিল, কারণ `MyApp` কম্পোনেন্টে `ThemeContext.Provider` এর মাধ্যমে `"dark"` পাঠানো হয়েছে।

---

### **এটি কীভাবে কাজ করে এবং কীভাবে সুবিধা দেয়?**

1. **Data Passing Across Many Layers**
   `useContext` ব্যবহার করলে, একটি কম্পোনেন্ট থেকে অন্য কম্পোনেন্টে ডেটা পাঠানো সহজ হয়ে যায়, বিশেষ করে যখন অনেক স্তরের (nested) কম্পোনেন্ট থাকে। যেহেতু `useContext` কম্পোনেন্টের মধ্য দিয়ে ডেটা পাস করার জন্য props এর প্রয়োজন হয় না, এটি কোডকে আরও পরিষ্কার এবং সহজ করে।

2. **Avoiding Prop Drilling**
   যদি আপনার অ্যাপ্লিকেশনে বহু স্তরের কম্পোনেন্ট থাকে, তবে প্রতিটি স্তরের মধ্যে props পাঠানোর প্রয়োজন পড়তে পারে। এটি এক ধরনের "prop drilling" নামে পরিচিত। `useContext` এর মাধ্যমে, আপনি ডেটাকে সরাসরি context থেকে পড়তে পারেন, যাতে props ড্রিলিং থেকে মুক্তি পাওয়া যায়।

3. **State Sharing Across Components**
   `useContext` ব্যবহার করে, আপনি একটি global state বা variable বিভিন্ন কম্পোনেন্টের মধ্যে শেয়ার করতে পারেন। এইভাবে, একাধিক কম্পোনেন্টে একই state বা value ব্যবহার করা সম্ভব হয়।

---

### **কোথায় ব্যবহার করবেন?**

- **Global State Management**
  যদি আপনার অ্যাপ্লিকেশনের বিভিন্ন স্থানে একরকম ডেটা বা state শেয়ার করতে চান, তবে `useContext` একটি ভালো পছন্দ হতে পারে। যেমন: থিম পরিবর্তন, ভাষা পরিবর্তন ইত্যাদি।

- **React Hooks ফিচারের মধ্যে ব্যবহার**
  যখন আপনি একটি reusable function বা hook তৈরি করছেন যা কন্টেক্সটের মানের উপর নির্ভরশীল, তখন আপনি `useContext` ব্যবহার করতে পারেন।

- **Complex UI Components**
  যদি আপনার অ্যাপ্লিকেশন অনেক স্তরের (nested) কম্পোনেন্ট থাকে, এবং সেই কম্পোনেন্টগুলির মধ্যে একাধিক ক্ষেত্রে একই ডেটা প্রয়োজন হয়, তবে `useContext` এর মাধ্যমে তা সহজে পাস করা যায়।

---

### **কখন ব্যবহার করবেন?**

`useContext` ব্যবহার করার সময় কিছু বিষয় মাথায় রাখতে হবে:

1. **Context Provider-এর উপরে অবস্থান করা উচিত**  
   যেহেতু `useContext` কম্পোনেন্টের উপরে সরাসরি `Context.Provider` এর কাছ থেকে ডেটা গ্রহণ করে, এটি অবশ্যই `useContext` কল করার কম্পোনেন্টের উপরে থাকতে হবে। 

2. **Context-এ পরিবর্তন হলে পুনঃরেন্ডার হবে**  
   যেকোনো পরিবর্তন হলে, যেসব কম্পোনেন্ট `useContext` ব্যবহার করবে, সেগুলি পুনঃরেন্ডার হবে।

3. **প্রতিটি Context-এর জন্য আলাদা Context Provider ব্যবহার করুন**  
   একাধিক context একসাথে ব্যবহার করলে, প্রতিটি context-এর জন্য আলাদা `Context.Provider` থাকতে হবে।

---

### **সমস্যা/পিটফল**

1. **Provider অনুসন্ধান**
   `useContext` শুধুমাত্র সেই `Provider` এর মান ব্যবহার করবে, যা কলিং কম্পোনেন্টের উপরে অবস্থিত। এটি উপরের দিকে সন্ধান করে। এর মানে হল, যদি আপনি ভুলভাবে একই কম্পোনেন্টের মধ্যে context provide করেন এবং `useContext` কল করেন, তবে এটি আপনার প্রত্যাশিত মান নাও পেতে পারে।

2. **Memoization Avoidance**
   `useContext` দ্বারা ফেরত পাওয়া মানগুলি যদি পরিবর্তিত হয়, তবে React সেই কম্পোনেন্টগুলোকে পুনঃরেন্ডার করবে, যেগুলি context এর সাথে যুক্ত। আপনি যদি `memo` ব্যবহার করেন, তা সত্ত্বেও নতুন context মানগুলি রেন্ডার হবে, কারণ React context এর মানগুলি পরিবর্তন হলে পুনঃরেন্ডার বাধ্যতামূলক।

---


`useContext` একটি অত্যন্ত শক্তিশালী React hook যা state বা value যে কোনও কম্পোনেন্টের মধ্যে শেয়ার করতে এবং পাঠাতে সহজ করে তোলে। তবে এর ব্যবহার সংক্রান্ত কিছু সীমাবদ্ধতা এবং কৌশল জানা থাকা উচিত, যাতে এটি আপনার অ্যাপ্লিকেশনে সঠিকভাবে কাজ করতে পারে।



### `Context` এর মাধ্যমে ডেটা আপডেট করা

React-এ `useContext` ব্যবহার করে কোনো `Context` থেকে ডেটা পড়া এবং সেই ডেটাকে কম্পোনেন্টগুলোর মধ্যে শেয়ার করা খুবই সাধারণ একটি কাজ। তবে, অনেক সময় আপনি চাইবেন যে আপনার `Context` সময়ের সাথে সাথে পরিবর্তিত হোক। এই ক্ষেত্রে, `useState` বা অন্য কোনো state management পদ্ধতি ব্যবহার করে `Context` এর মান আপডেট করতে হবে। 

আজ আমরা বুঝব **কীভাবে Context এর মাধ্যমে ডেটা আপডেট করতে হয়**, **কেন এটি ব্যবহার করা হয়**, এবং **কোথায় এবং কখন এটি ব্যবহার করতে হয়**।

---

### **কেন `Context` এবং `useState` ব্যবহার করবেন?**

React এ `Context` একটি গ্লোবাল স্টোরের মত কাজ করে, যেখান থেকে বিভিন্ন কম্পোনেন্টরা একই ডেটা ব্যবহার করতে পারে। যখন সেই ডেটা পরিবর্তন করতে হবে, তখন আপনি সেটি **state** এর মাধ্যমে আপডেট করেন এবং সেই পরিবর্তনগুলো **Context.Provider** এর মাধ্যমে অন্যান্য কম্পোনেন্টে পৌঁছিয়ে দেন।

### **কীভাবে এটি কাজ করে?**

আমরা যদি উদাহরণ হিসেবে `theme` নামের একটি ডেটা বিবেচনা করি (যেমন: "dark" বা "light"), এবং এই `theme` value কে পরিবর্তন করতে চাই, তবে আমরা নিচের মতো একটি structure তৈরি করতে পারি:

#### ১. **Context তৈরি করা**

প্রথমেই একটি `Context` তৈরি করতে হবে:

```js
import { createContext } from 'react';

const ThemeContext = createContext('dark');  // Default value
```

এখানে, `ThemeContext` তৈরি করা হয়েছে এবং এর ডিফল্ট মান হচ্ছে `"dark"`।

#### ২. **State এবং Context.Provider সেট আপ করা**

এখন `useState` ব্যবহার করে `theme` এর মান এবং `setTheme` ফাংশন তৈরি করা হবে, যা `theme` এর মান পরিবর্তন করবে।

```js
import React, { useState } from 'react';
import { ThemeContext } from './context';  // Importing the created context

function MyPage() {
  const [theme, setTheme] = useState('dark');  // Initial theme state

  return (
    <ThemeContext.Provider value={theme}>  {/* Providing the current theme value */}
      <Form />
      <Button onClick={() => setTheme('light')}>Switch to light theme</Button>
    </ThemeContext.Provider>
  );
}
```

এখানে:
- `useState('dark')` এর মাধ্যমে `theme` এর একটি state তৈরি করা হয়েছে। এর মান ডিফল্টে `"dark"`, তবে পরে এটি পরিবর্তন হবে।
- `ThemeContext.Provider` ব্যবহার করে `theme` এর মান `Form` এবং `Button` কম্পোনেন্টগুলিতে প্রদান করা হচ্ছে।
- `Button` ক্লিক করলে, `setTheme('light')` ফাংশনটি কল হবে, যা `theme` এর মান পরিবর্তন করবে এবং তারপর `ThemeContext` এর মান নতুন value `"light"` হবে।

#### ৩. **`useContext` ব্যবহার করে Context পড়া**

এখন, যেকোনো কম্পোনেন্টে `useContext` ব্যবহার করে আপনি `theme` এর মান পড়তে পারবেন। যেমন, `Button` কম্পোনেন্টে:

```js
import { useContext } from 'react';
import { ThemeContext } from './context';

function Button({ children, onClick }) {
  const theme = useContext(ThemeContext);  // Get the theme value from Context
  const className = `button-${theme}`;  // Use the theme to determine button style

  return (
    <button className={className} onClick={onClick}>
      {children}
    </button>
  );
}
```

এখানে, `Button` কম্পোনেন্ট `ThemeContext` থেকে `theme` value পেয়ে তা `className` এর মধ্যে ব্যবহার করছে। 

---

### **এটি কেন ব্যবহার করবেন?**

#### **1. Global State Management (গ্লোবাল স্টেট ম্যানেজমেন্ট)**

আপনি যখন চান যে একাধিক কম্পোনেন্ট একসাথে একই ডেটা শেয়ার করুক, তখন `useContext` এবং `useState` এর সংমিশ্রণ ব্যবহার করা খুবই কার্যকর। উদাহরণস্বরূপ, একটি `theme` এর মান বিভিন্ন কম্পোনেন্টে শেয়ার করা হতে পারে, এবং যখন এই `theme` পরিবর্তন হবে, তখন সব কম্পোনেন্ট স্বয়ংক্রিয়ভাবে পুনঃরেন্ডার হবে।

#### **2. সহজ State Updates (সহজ স্টেট আপডেট)**

যখন `Context` কে স্টেট হিসেবে ব্যবহার করা হয়, তখন এটি `setState` এর মাধ্যমে ডেটার পরিবর্তন ঘটায়, যা React অ্যাপ্লিকেশনকে দ্রুত এবং সুনির্দিষ্টভাবে পরিবর্তনগুলো পরিচালনা করতে সক্ষম করে।

#### **3. Clean and Simple Code (পরিষ্কার এবং সহজ কোড)**

`Context` এবং `useState` এর সমন্বয়ে, আপনি কম্পোনেন্টগুলির মধ্যে props ড্রিলিং থেকে মুক্তি পান এবং ডেটা স্থানান্তরের কোডটি আরও পরিষ্কার এবং সহজ হয়।

---

### **কোথায় এবং কখন ব্যবহার করবেন?**

#### **কোথায়?**

- **Global Theme or Settings**: অ্যাপ্লিকেশন বা সাইটের থিম পরিবর্তন করতে চাইলে, যেমন "light" বা "dark" থিম।
- **User Authentication**: একজন ব্যবহারকারী লগ ইন করেছেন কিনা, এবং সে যদি লগ ইন থাকে, তার তথ্য কোথাও ব্যবহার করতে চাইলে।
- **Language Setting**: অ্যাপ্লিকেশনের ভাষা পরিবর্তন করতে চাইলে।
- **Dynamic Data Sharing**: যখন ডেটা বা state একাধিক স্তরের (nested) কম্পোনেন্টে পাঠানোর প্রয়োজন।

#### **কখন?**

- যখন আপনি চান যে, কম্পোনেন্টগুলির মধ্যে কিছু ডেটা গ্লোবালি শেয়ার করা হোক।
- যখন ডেটা পরিবর্তন হলে সেই পরিবর্তনটি সব কম্পোনেন্টে ছড়িয়ে পড়ুক এবং তাদের পুনঃরেন্ডার করা হোক।
- যখন props ড্রিলিং (props পাস করা) খুব বেশি বা জটিল হয়ে যায়।

---



React-এ `Context` এবং `useState` এর সংমিশ্রণ ব্যবহার করে আপনি যেকোনো ধরনের গ্লোবাল স্টেট সহজেই শেয়ার করতে এবং পরিবর্তন করতে পারবেন। এটি আপনার অ্যাপ্লিকেশনকে আরও modular, scalable এবং maintainable করে তোলে। Context ব্যবহার করে আপনি বড় অ্যাপ্লিকেশনগুলোতে state management অনেক সহজ করে ফেলতে পারেন, এবং `useState` এর মাধ্যমে সেই state পরিবর্তন করে অ্যাপ্লিকেশনের UI পুনঃরেন্ডার করতে পারেন।
