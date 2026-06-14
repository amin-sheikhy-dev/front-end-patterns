در `Next.js App Router`، اگر فایلی به نام `loading.jsx` (یا `loading.tsx`) در یک پوشه قرار بدی، به طور خودکار به عنوان `Fallback` برای `Suspense` اون مسیر استفاده میشه.

توجه شود که کامپوننت loading هم دقیقا مثل کامپوننت layout و پیج یک کامپوننت رزرو شده اس

کامپوننت `LoadingPage` دقیقاً مثل `Layout` و `Page` رفتار میکنه. یعنی اگر در روت (مثلاً `app/meals/loading.jsx`) قرار بگیره، به طور خودکار برای تمام مسیرهای فرزند اون روت اعمال میشه.

```tsx
export default function LoadingPage() {
  return <h1>Fetching Data...</h1>;
}
```
