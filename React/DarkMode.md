تنظیم darkMode: 'class' به Tailwind میگه که به جای استفاده از تنظیمات پیش‌فرض مرورگر (سیستم عامل)، باید به صورت دستی با اضافه کردن کلاس dark به یک تگ والد (معمولاً html یا body) حالت تاریک رو فعال کنی

```js
/** @type {import('tailwindcss').Config} */
export default {
  darkMode: 'class', // برای دارک مود ضروریه
  content: ['./src/**/*.{html,js,jsx,ts,tsx}', './public/index.html'],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

برای تغییر رنگ بادی هنگام اعمال دارک مود ضروریه

مشخص میکنیم که در حالت عادی پس‌زمینه سفید و در حالت دارک مود، پس‌زمینه خاکستری تیره (gray-900) باشه.

این کد باید در فایل اصلی CSS شما (مثلاً index.css یا globals.css) قرار بگیره.

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  body {
    @apply bg-white dark:bg-gray-900;
  }
}
```

با استفاده از این میدلور، تم انتخاب شده کاربر در localStorage ذخیره میشه و حتی بعد از رفرش صفحه هم باقی میمونه.

و toggleTheme: این تابع وضعیت تم رو بین 'light' و 'dark' تغییر میده.

مقدار اولیه 'dark': فقط بار اول استفاده میشه. دفعات بعد، مقدار از localStorage خونده میشه.

یکپارچه‌سازی با Tailwind: بعد از اینکه مقدار theme رو از این استور خوندید، باید کلاس dark رو به html اضافه یا حذف کنید:

```ts
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

interface ThemeStoreType {
  theme: string;
  toggleTheme: () => void;
}

export const useThemeStore = create<ThemeStoreType>()(
  persist(
    (set) => ({
      theme: 'dark', // ساخت مقدار اولیه برای استور - ولی چون در لوکال استوریج ذخیره میشه دفعات بعد از اونجا مقدار میگیره

      // ساخت تابع تاگل تم
      toggleTheme: () => set((state) => ({ theme: state.theme === 'light' ? 'dark' : 'light' })),
    }),
    {
      name: 'theme-storage',
    }
  )
);
```

یوز افکت با وابستگی theme: هر وقت تم تغییر کنه، این افکت اجرا میشه و کلاس dark رو به document.documentElement (تگ html) اضافه یا حذف میکنه.

کلاس dark فقط: در Tailwind، کلاس dark: فقط در صورتی کار میکنه که تگ والد کلاس dark داشته باشه. یعنی برای روشن (light mode) نیازی به کلاس light نیست، فقط در حالت تاریک کلاس dark اضافه میشه.

استفاده از کلاس‌های dark:: در دکمه Logout، از dark:bg-gray-900 و dark:text-white استفاده شده که فقط وقتی کلاس dark روی html باشه اعمال میشن.

```jsx
const theme = useThemeStore((s) => s.theme); // دریافت مقدار خود تم
const toggleTheme = useThemeStore((s) => s.toggleTheme); // فانکشن تاگل کردن تم

// برای اینکه کلاس دارک در المنت ها اعمال بشه باید المنت پرنتشون کلاس دارک داشته باشه
useEffect(() => {
  // انتخاب برگ ترین المنت کل اپلیکیشن
  const root = window.document.documentElement;

  // توجه شود که تبدیلید فقط کلاس دارک دارد و با کلاس لایتی وجود ندارد
  if (theme === 'dark') {
    root.classList.add('dark');
  } else {
    root.classList.remove('dark');
  }
}, [theme]);

return (
  // دکمه تغییر تم
  <>
    <button onClick={toggleTheme}>{theme === 'light' ? 'Dark 🌙' : 'Light ☀️'}</button>
    <button className="bg-white text-black dark:bg-gray-900 dark:text-white" onClick={logout}>
      Logout
    </button>
  </>
  // استفاده از کلاس دارک در داخل المنت ها
);
```
