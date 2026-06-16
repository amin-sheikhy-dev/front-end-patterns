این کد مربوط به کامپوننت `NavLink` در `Next.js` هست که لینک فعال `(Active Link)` رو تشخیص میده:

و `usePathname` یک هوک در `Next.js` است که مسیر فعلی صفحه را برمی‌گرداند
مثلا

اگر کاربر در این URL باشد:

`https://example.com/products/12`

خروجی:

`/products/12`

مثلا بینی کدوم تب فعاله

---

```tsx
'use client';

import Link from 'next/link';
import { usePathname } from 'next/navigation';

export default function NavLink({ href, children }) {
  const path = usePathname();

  return (
    <Link href={href} className={path.startsWith(href) ? 'active' : undefined}>
      {children}
    </Link>
  );
}
```

این کد مربوط به کامپوننت `MainHeader` هست که از `NavLink` برای نویگیشن استفاده می‌کنه:

و `NavLink:` در اینجا برای لینک‌های نویگیشن استفاده شده که وقتی کاربر در صفحه مربوطه باشه، لینک فعال میشه.

```tsx
import Link from 'next/link';
import NavLink from './nav-link';

export default function MainHeader() {
  return (
    <header>
      <div>
        <Link href="/">NextNews</Link>
      </div>

      <nav>
        <NavLink href="/news">News</NavLink>

        <NavLink href="/archive">Archive</NavLink>
      </nav>
    </header>
  );
}
```
