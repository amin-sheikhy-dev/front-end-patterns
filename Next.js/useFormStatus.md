چون از هوک useFormStatus استفاده می‌کنه که فقط در Client کار می‌کنه.

یک هوک از react-dom هست که وضعیت ارسال فرم (Pending) رو نشون میده.
pending: وقتی فرم در حال ارسال هست، مقدار true میشه و دکمه غیرفعال (Disabled) میشه و متن "Submitting..." نمایش داده میشه.

```tsx
'use client';

import { useFormStatus } from 'react-dom';

export default function MealsFormSubmit() {
  const { pending } = useFormStatus(); // وضعیت ارسال فرم

  return <button disabled={pending}>{pending ? 'Submitting...' : 'Share Meal'}</button>;
}
```
