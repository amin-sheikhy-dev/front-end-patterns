چون کپی کردن به کلیپ‌بورد فقط در سمت کاربر انجام می‌شود، کامپوننت باید use client داشته باشد.

```tsx
'use client';

export default function CopyButton({ text }: { text: string }) {
  async function handleCopy() {
    try {
      await navigator.clipboard.writeText(text);

      console.log('Copied!');
    } catch (error) {
      console.error(error);
    }
  }

  return <button onClick={handleCopy}>Copy</button>;
}
```

استفاده

```tsx
<CopyButton text="https://github.com/amin-sheikhy-dev" />

<CopyButton text="sheikhy.dev@gmail.com" />

<CopyButton text="https://amin-sh-portfolio.vercel.app/" />
```
