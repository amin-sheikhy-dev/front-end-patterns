فایل `next.config.ts` `(یا next.config.js)` در `Next.js` در واقع فایل تنظیمات اصلی پروژه است؛ یعنی جایی که رفتار کلی اپلیکیشن را کنترل می‌کنی.

این فایل برای تنظیم چیزهایی استفاده می‌شود که مربوط به کد UI نیست، بلکه مربوط به کل `Next.js runtime` و `build system` است.

مثلا یکی از رفتاراش برای تنظیم عکساس که از چه `URL` ای وارد میشوند؟ و سپس باید اون `URL` که عکس ها از ان میایند را داخل این فایل بزاریم

```tsx
import type { NextConfig } from 'next';

const nextConfig: NextConfig = {
  images: {
    remotePatterns: [
      {
        protocol: 'https',
        hostname: 'www.themealdb.com',
      },
    ],
  },
};

export default nextConfig;
```

در اینجا ما همه دامنه هارو آزاد کردیم ولی این روش توصیه نمیشه چون که امنیت پایینی داره

```tsx
import type { NextConfig } from 'next';

const nextConfig: NextConfig = {
  images: {
    remotePatterns: [
      {
        protocol: 'https',
        hostname: '**', // ✅ همه دامنه‌های HTTPS
      },
    ],
  },
};

export default nextConfig;
```
