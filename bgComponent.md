این کد مربوط به کامپوننت پس‌زمینه با next/image هست:

```tsx
import Image from 'next/image';
import bg from '@/assets/background-img.jpg';

export default function BackgroundImg() {
  return (
    <div className="fixed left-0 top-0 -z-10  h-full w-full ">
      <Image src={bg} alt="background" fill priority className="object-cover" />
    </div>
  );
}
```
