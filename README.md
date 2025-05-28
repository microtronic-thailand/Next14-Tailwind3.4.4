# 🚀 Next.js 14 + Tailwind CSS 3.4.4 (Manual Setup)
นี่คือคู่มือสําหรับการติดตั้ง Next.js 14 และ Tailwind CSS 3.4.4 ด้วยตนเอง พร้อมอธิบายเหตุผลเกี่ยวกับ Tailwind v4

โปรเจกต์นี้สาธิตวิธีการตั้งค่าโปรเจกต์ Next.js 14 ใหม่ และผสานรวม Tailwind CSS 3.4.4 ด้วยตนเอง เนื่องจาก Tailwind CSS v4 (ซึ่งกำลังอยู่ในช่วงพัฒนา) ได้ถอดไฟล์ `cli.js` ออกไป ทำให้คำสั่ง `npx tailwindcss init -p` ไม่สามารถใช้งานได้โดยตรงเหมือนเดิม

## ✨ ทำไมต้อง Manual Setup?

ในขณะที่ Next.js CLI มีตัวเลือกในการติดตั้ง Tailwind CSS ให้อัตโนมัติ ปัญหาสามารถเกิดขึ้นได้หากคุณ :

1.  ต้องการควบคุมเวอร์ชันของ Tailwind CSS ที่แม่นยำ (เช่น ต้องการใช้ `3.4.4` เพื่อหลีกเลี่ยง Breaking Changes ของ v4)
2.  บังเอิญติดตั้ง Tailwind CSS v4 Pre-release โดยไม่ได้ตั้งใจ ซึ่งจะทำให้คำสั่ง `npx tailwindcss init -p` ล้มเหลวด้วยข้อความ error : `npm error could not determine executable to run`

คู่มือนี้จะช่วยให้คุณตั้งค่า Next.js 14 และ Tailwind CSS 3.4.4 ได้อย่างราบรื่นโดยไม่ต้องพึ่งพาคำสั่งอัตโนมัติ

## 📋 ขั้นตอนการติดตั้ง

### 1. สร้างโปรเจกต์ Next.js 14 ใหม่

เปิด Terminal หรือ Command Prompt ของคุณ แล้วรันคำสั่งเพื่อสร้างโปรเจกต์ Next.js :

```bash
npx create-next-app@latest my-next14-tailwind-manual
```

เมื่อระบบถามคำถาม ให้เลือกตามนี้ (โดยเฉพาะส่วนของ Tailwind CSS ให้เลือก `No`) :

-   `Would you like to use TypeScript?` (แนะนำ : `No`)
-   `Would you like to use ESLint?` (แนะนำ : `Yes`)
-   `Would you like to use Tailwind CSS?` (**สำคัญมาก! เลือก `No`**)
-   `Would you like to use src/ directory?` (แนะนำ : `No`)
-   `Would you like to use App Router? (recommended)` (แนะนำ : `Yes`)
-   `Would you like to customize the default import alias?` (แนะนำ : `No`)

จากนั้น เข้าไปในโฟลเดอร์โปรเจกต์ของคุณ :

```bash
cd my-next14-tailwind-manual
```

### 2. ติดตั้ง Tailwind CSS และ PostCSS

เราจะติดตั้ง Tailwind CSS เวอร์ชันที่ระบุ (`3.4.4`) พร้อมกับ PostCSS และ Autoprefixer :

```bash
npm install -D tailwindcss@3.4.4 postcss autoprefixer
```

หรือหากคุณใช้ Yarn :

```bash
yarn add -D tailwindcss@3.4.4 postcss autoprefixer
```

### 3. กำหนดค่า Tailwind CSS และ PostCSS ด้วยตนเอง

#### 3.1 สร้างไฟล์ `tailwind.config.js`

สร้างไฟล์ชื่อ `tailwind.config.js` ที่ Root ของโปรเจกต์ (ในโฟลเดอร์ `my-next14-tailwind-manual`) และเพิ่มโค้ดต่อไปนี้ :

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    './pages/**/*.{js,ts,jsx,tsx,mdx}',
    './components/**/*.{js,ts,jsx,tsx,mdx}',
    './app/**/*.{js,ts,jsx,tsx,mdx}',
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

#### 3.2 สร้างไฟล์ `postcss.config.js`

สร้างไฟล์ชื่อ `postcss.config.js` ที่ Root ของโปรเจกต์ และเพิ่มโค้ดต่อไปนี้ :

```javascript
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}
```

### 4. Import Tailwind CSS เข้าสู่ไฟล์ CSS หลัก

ค้นหาไฟล์ CSS หลักของโปรเจกต์ ซึ่งโดยปกติจะอยู่ที่ `app/globals.css` (สำหรับ App Router) หรือ `styles/globals.css` (สำหรับ Pages Router) จากนั้นเพิ่ม `@tailwind` directives เหล่านี้ที่ด้านบนสุดของไฟล์ :

```css
/* app/globals.css */

@tailwind base;
@tailwind components;
@tailwind utilities;

/* เพิ่ม CSS ของคุณเองที่นี่ */
/* ... */
```

### 5. ทดสอบการใช้งาน Tailwind CSS

ตอนนี้ทุกอย่างพร้อมแล้ว! รันโปรเจกต์ Next.js ของคุณ :

```bash
npm run dev
```

หรือ:

```bash
yarn dev
```

เปิดเบราว์เซอร์ไปที่ `http://localhost:3000`

เปิดไฟล์ `app/page.js` (หรือ `pages/index.js`) และลองเพิ่มคลาส Tailwind CSS เข้าไปใน JSX ของคุณ เช่น :

```javascript
export default function Home() {
  return (
    <main className="flex min-h-screen flex-col items-center justify-center p-8 bg-purple-50">
      <h1 className="text-5xl font-extrabold text-indigo-700 mb-6">
        Next.js 14 + Tailwind CSS 3.4.4
      </h1>
      <p className="text-xl text-gray-800 mb-10">
        ติดตั้งด้วยตนเอง ก็ทำได้ไม่ยากเลย!
      </p>
      <button className="px-8 py-4 bg-teal-500 text-white font-semibold rounded-full shadow-lg hover:bg-teal-600 transition-transform transform hover:scale-105">
        กดที่นี่!
      </button>
    </main>
  );
}
```

บันทึกไฟล์และตรวจสอบในเบราว์เซอร์ คุณควรจะเห็นการเปลี่ยนแปลงการออกแบบที่เกิดจากคลาส Tailwind CSS แสดงผลออกมาอย่างถูกต้อง

## 🎉 สรุป

ด้วยขั้นตอนเหล่านี้ คุณสามารถติดตั้งและกำหนดค่า Next.js 14 และ Tailwind CSS 3.4.4 ได้อย่างมั่นใจ แม้ว่า Tailwind CSS v4 จะนำการเปลี่ยนแปลงที่สำคัญมาสู่กระบวนการ Build ก็ตาม นี่เป็นวิธีที่แข็งแกร่งในการเริ่มต้นโปรเจกต์ของคุณโดยควบคุมเวอร์ชันและหลีกเลี่ยงปัญหาความเข้ากันได้

---

**ผู้เขียน :** [Daruma.K](https://github.com/DarumaKlang)
```