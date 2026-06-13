به هوک ریاکت هست که اجازه میده بدون اینکه کل صفحه فریز بشه، به کار سنگین مثل سرور اکشن رو انجام بدی

isPending -> true/false (نشون میده کار تموم شده یا نه)

startTransition -> تابعی که کار سنگین رو توش میذاری

```tsx
const [isPending, startTransition] = useTransition();

function handleLogout() {
  // Server Action رو اینجا صدا میزنیم
  startTransition(() => logout());
}
```

حالا این تابع handleLogout رو توی دکمه های کامپوننت استفاده کن
