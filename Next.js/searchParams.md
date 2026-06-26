و `searchParams` در `App Router`:

فقط در `page.tsx` یا `layout.tsx` قابل دریافت است

به صورت `prop` می‌آید

مثل `React hook` نیست

در نکست جی اس `searchParams` یعنی همان `query string` داخل `URL` مثل:

`/products?category=phone&page=2`

---

و `searchParams` داده‌هایی هستند که بعد از `?` در `URL` می‌آیند.

### `/blog?tag=nextjs&sort=latest`

استفاده در `Server Component` (رایج‌ترین روش)

نکست جی اس خودش `searchParams` را به صفحه پاس می‌دهد:

```tsx
export default async function Page({ searchParams }: { searchParams: Promise<Record<string, string>> }) {
  const params: Record<string, string> = await searchParams;

  return (
    <div>
      <h1>Blog Page</h1>

      <p>Tag: {params.tag}</p>

      <p>Sort: {params.sort}</p>
    </div>
  );
}
```

خروجی:

> Tag: nextjs

> Sort: latest

---

استفاده در `Client Component`

در کلاینت باید از `hook` استفاده کنی:

```tsx
'use client';

import { useSearchParams } from 'next/navigation';

export default function Page() {
  const searchParams = useSearchParams();

  const category = searchParams.get('category');
  const page = searchParams.get('page');

  return (
    <div>
      <p>Category: {category}</p>
      <p>Page: {page}</p>
    </div>
  );
}
```

---

متدهای مهم `useSearchParams`

```tsx
searchParams.get('key'); // گرفتن مقدار
searchParams.has('key'); // چک کردن وجود
searchParams.toString(); // کل query string
```

---

تغییر `searchParams` (نکته مهم)

برای تغییر باید از `router` استفاده کنی:

```tsx
'use client';

import { useRouter, useSearchParams } from 'next/navigation';

export default function Page() {
  const router = useRouter();
  const searchParams = useSearchParams();

  function setPage(page) {
    const params = new URLSearchParams(searchParams);
    params.set('page', page);

    router.push('?' + params.toString());
  }

  return <button onClick={() => setPage(5)}>Go to page 5</button>;
}
```

---

یه مثال عالی

```tsx
'use client';
import { useRouter } from 'next/navigation';

export default function FilterButtons() {
  const router = useRouter();

  function setFilter(filter: string) {
    if (filter === 'all') {
      router.push('/rooms');
    } else {
      router.push(`/rooms?filter=${filter}`);
    }
  }

  return (
    <div>
      <button onClick={() => setFilter('all')}>همه اتاق ها</button>

      <button onClick={() => setFilter('small')}>۲ تا ۵ نفر</button>

      <button onClick={() => setFilter('medium')}>۶ تا ۸ نفر</button>
    </div>
  );
}
```

```tsx
import { getRooms } from '@/lib/utils';
import RoomItem from '@/components/rooms/room-item';
import FilterButtons from '@/components/rooms/filter-buttons';

interface RoomsPageProps {
  searchParams: Promise<{ filter?: string }>;
}

export default async function RoomsPage({ searchParams }: RoomsPageProps) {
  const { filter } = await searchParams;
  const rooms = await getRooms();

  let filteredRooms = rooms;

  if (filter === 'small') {
    filteredRooms = rooms.filter((room) => room.maxGuest >= 2 && room.maxGuest <= 5);
  } else if (filter === 'medium') {
    filteredRooms = rooms.filter((room) => room.maxGuest >= 6 && room.maxGuest <= 8);
  }

  return (
    <div>
      <FilterButtons currentFilter={filter ?? 'all'} />

      <main>
        {filteredRooms.map((room) => (
          <RoomItem key={room.id} {...room} />
        ))}
      </main>
    </div>
  );
}
```

---

مثال پیشرفته

کامپوننت کلاینت

```tsx
export default function Search() {
  const router = useRouter();

  const [date, setDate] = useState<string[]>([]);
  const [query, setQuery] = useState<string>('');
  const [domains, setDomains] = useState<string[]>([]);
  const [searchInTitle, setSearchInTitle] = useState<boolean>(false);

  function handleSearch(): void {
    if (!query && domains.length === 0) return;

    let url = '/search?';

    if (query.trim()) url += `query=${encodeURIComponent(query)}`;

    if (domains.length > 0) {
      const d = domains.length > 1 ? domains.join(',') : domains[0];
      url += `${url.includes('query=') ? '&' : ''}domains=${d}`;
    }

    if (date.length === 2) url += `&fromDate=${date[0]}&toDate=${date[1]}`;

    if (searchInTitle && query.trim()) url += '&searchInTitle=yes';

    url += '&page=1';

    router.push(url);
  }

  return <div>....</div>;
}
```

```tsx
import { searchNews } from '@/lib/api/news';
import { Data } from '@/types/types';

export const dynamic = 'force-dynamic';

export default async function SearchPage({ searchParams }: { searchParams: Promise<Record<string, string>> }) {
  const params: Record<string, string> = await searchParams;

  const data: Data | null = params.query || params.domains ? await searchNews(params) : null;

  const currentPage: number = Number(params.page) || 1;

  return <div>....</div>;
}
```
