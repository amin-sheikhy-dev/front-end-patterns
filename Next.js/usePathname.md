این کد مربوط به کامپوننت `NavLink` در `Next.js` هست که لینک فعال `(Active Link)` رو تشخیص میده:

این هوک بت فعلی رو به ما میده یعنی اونی که بعد دامنه قرار گرفته

مثلا
`/product/iphone`

کاربردش اینه بینی مثلا کدوم تب فعاله

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

این کد مربوط به کامپوننت MainHeader هست که از NavLink برای نویگیشن استفاده می‌کنه:

و NavLink: در اینجا برای لینک‌های نویگیشن استفاده شده که وقتی کاربر در صفحه مربوطه باشه، لینک فعال میشه.

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
