یک تابع مدرن در مرورگر است که یک شناسه یکتا (UUID) ایجاد می‌کند

یک رشته 36 کاراکتری یکتا میسازد مثلاً "e4567-e89b-12d3-a456-426614174000"

این مقدار بین رفرش‌های صفحه عوض می‌شود برای ثابت ماندن در لوکال استوریج ذخیره کنید

```js
let id = localStorage.getItem('userId');

if (!id) {
  id = crypto.randomUUID();
  localStorage.setItem('userId', id);
}
```
