app/layout.tsx

این فایل برای ساختار مشترک بین چند صفحه استفاده میشه (مثل هدر، فوتر، نویگیشن).

و `children` یعنی محتوای هر `page.tsx` داخل `<main>` قرار می‌گیره.

```tsx
import './globals.css';

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="fa">
      <body>
        <header>
          <nav>
            <a href="/">خانه</a>
            <a href="/about">درباره ما</a>
          </nav>
        </header>

        <main>{children}</main>

        <footer>© 2025 تمام حقوق محفوظ است.</footer>
      </body>
    </html>
  );
}
```
