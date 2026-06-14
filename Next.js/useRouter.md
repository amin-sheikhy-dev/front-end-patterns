این یک Client Component است و باید در مرورگر رندر شود (چون از هوک useRouter استفاده می‌کند که فقط در Client کار می‌کند).

یک هوک از next/navigation است که برای ناوبری برنامه‌ای (Programmatic Navigation) استفاده می‌شود.

کاربر را به صفحه قبلی در تاریخچه مرورگر می‌برد (معادل دکمه Back مرورگر).

```tsx
'use client';
import { useRouter } from 'next/navigation';

export default function BackButton() {
  const router = useRouter();

  return (
    <main>
      <button onClick={() => router.back()}>Go Back</button>
    </main>
  );
}
```

```tsx
'use client';
import { useRouter } from 'next/navigation';

export default function AddButton({ room }: AddButtonProps) {
  const router = useRouter();

  function handleAddRoom() {
    addReservation(reservedRoom);
    router.push('/user-profile');
  }
}
```
