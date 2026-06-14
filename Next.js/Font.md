مسیر فونت‌ها (../assets/fonts/...) باید نسبت به فایل layout.tsx درست باشد.

قرار دادن className={yekan.variable} روی <html> صحیح است و باعث می‌شود متغیر --font-yekan در کل برنامه در دسترس باشد.

```tsx
import localFont from 'next/font/local';
import './globals.css';

const yekan = localFont({
  src: [
    { path: '../assets/fonts/Yekan-Regular.woff2', weight: '400', style: 'normal' },
    { path: '../assets/fonts/Yekan-SemiBold.woff2', weight: '600', style: 'normal' },
    { path: '../assets/fonts/Yekan-Bold.woff2', weight: '700', style: 'normal' },
  ],
  variable: '--font-yekan',
});

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="fa" dir="rtl" className={yekan.variable}>
      <body>{children}</body>
    </html>
  );
}
```

اگر از Tailwind استفاده می‌کنی، حتی بهتر است فونت را در tailwind.config.ts تعریف کنی و با کلاس‌هایی مثل font-yekan از آن استفاده کنی.

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  darkMode: 'class', // برای دارک مود ضروریه
  content: ['./src/**/*.{js,ts,jsx,tsx,mdx}'],
  theme: {
    extend: {
      // برای اضافه کردن فونت
      fontFamily: { sans: ['var(--font-yekan)', 'sans-serif'] },
    },
  },
  plugins: [],
};
```

بهتر است فونت را روی body اعمال کنیم تا کل برنامه از آن استفاده کند

```css
body {
  font-family: var(--font-yekan), sans-serif;
}
```

اگر بخواهی فونت روی همه عناصر اعمال شود، می‌توانی به جای body از این استفاده کنی:

ولی اعمال آن روی body معمولاً بهتر و بهینه‌تر است.

```css
* {
  font-family: var(--font-yekan), sans-serif;
}
```
