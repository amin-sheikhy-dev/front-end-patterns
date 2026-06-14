این یک Error Boundary کامپوننت در Next.js App Router است. زمانی که در یک صفحه یا سگمنت خطایی رخ دهد (مثل خطای سرور، خطای شبکه، یا خطای JavaScript)، به جای نمایش صفحه سفید خطا، این UI دوستانه به کاربر نمایش داده می‌شود.

و reset: تابعی که با فراخوانی آن، Next.js تلاش می‌کند دوباره صفحه را رندر کند (بدون ریلود کامل صفحه)

و error: شی خطا حاوی اطلاعات خطا. message متن خطا، و digest یک شناسه یکتا برای خطا در لاگ‌های سرور است

```tsx
'use client';

export default function Error({ error, reset }: { error: Error & { digest: string }; reset: () => void }) {
  return (
    <div>
      <div>
        <h1>خطا</h1>
        <p>{error.message}</p>
      </div>

      <button onClick={reset}>تلاش مجدد</button>
    </div>
  );
}
```
