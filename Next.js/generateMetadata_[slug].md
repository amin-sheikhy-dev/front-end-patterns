این تابع برای تولید متادیتای پویا `(SEO)` در `Next.js` استفاده میشه. در اینجا از `slug` گرفته شده از `params` استفاده میکنه تا عنوان و توضیحات غذا رو برگردونه.

و `params`: در `Next.js App Router`، `params` یک `Promise` هست که باید با `await` مقدارش رو گرفت.

و `notFound()`: اگر غذا پیدا نشه، تابع `notFound()` صدا زده میشه که کاربر رو به صفحه `404 (Not Found)` هدایت میکنه.

```tsx
import { Metadata } from 'next';
import { notFound } from 'next/navigation';
import { getMeal } from '@/lib/meals';
import { ApiType } from '@/lib/definitions';

export async function generateMetadata({ params }: { params: Promise<{ slug: string }> }): Promise<Metadata> {
  const { slug } = await params;

  const meal: ApiType | undefined = getMeal(slug);

  if (!meal) notFound();

  return { title: meal.title, description: meal.summary };
}
```

صفحه اصلی: MealDetailsPage هم همون params رو دریافت میکنه و اطلاعات غذا رو فچ میکنه.

و TypeScript: از تایپ‌های Metadata و Promise استفاده شده که نشون میده این کد با TypeScript نوشته شده.

```tsx
export default async function MealDetailsPage({ params }: { params: Promise<{ slug: string }> }) {
  const { slug } = await params;

  const meal: ApiType | undefined = getMeal(slug);

  if (!meal) notFound();

  return (
    // component....
  )
}
```

اجرای این تابع که توسط خود نکست ارائه شده باعث میشه اجرای بقیه کد متوقف شه و نزدیک ترین صفحه ارور با نات فوند اجرا شه

```tsx
import { notFound } from 'next/navigation';

if (!var) notFound();
```
