این کد باعث میشه عنوان صفحه مرورگر (Tab Title) با نام فیلم تغییر کنه.

```jsx
useEffect(() => {
  if (!title) return;

  document.title = `Movie | ${title}`;

  // اینجا کلین اپ باعث میشه زمانی که کامپوننت ما ان مونت شد این افکت به بار دیگه اجرا بشه
  return () => (document.title = 'usePopcorn');
}, [title]);
```

Cleanup Function

کلین اپ فانکشن باعث میشه آخرین ریکوئستی که کاربر به سرور زده رو بهش نشون بدیم که خب خیلی مهمه
