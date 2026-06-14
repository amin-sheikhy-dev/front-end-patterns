این اسم کامپوننت هم دقیقا یک کامپوننت با اسم رزرو شدس

و اگر در روتي استفاده شود در روت ها و کامپوننت های چایلند هم اعمال میشود دقیقا مثل کامپوننت لی اوت

این فایل معمولاً در مسیر `app/not-found.js` یا `app/not-found.tsx` قرار میگیره و در `Next.js` وقتی کاربر به مسیری برخورد کنه که وجود نداره، این صفحه نمایش داده میشه.

```tsx
import BackButton from '@/components/Buttons/BackButton';

export default function NotFoundPage() {
  return (
    <main>
      <h1>Not found</h1>

      <p>we could not find the requested page or resource</p>

      <BackButton />
    </main>
  );
}
```
