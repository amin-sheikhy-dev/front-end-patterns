یک کامپوننت از React هست که به شما اجازه میده تا زمانی که یک کامپوننت (مثل Meals) در حال دریافت داده (Fetch) هست، یک Fallback (نمایش موقت) نشون بدید.

داخل فال بک کامپونتی که میجای موقع فچ شدن نشون بد رو میزاری

و `<Meals />`: کامپونتی که طول میکشه تا فچ شه

```tsx
import { Suspense } from 'react';

<Suspense fallback={<h1>Fetching Meals...</h1>}>
  <Meals />
</Suspense>;
```
