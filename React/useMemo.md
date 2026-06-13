```jsx
const ValueName = useMemo(() => {}, []);
```

یک هوک که برای بهینه سازی محاسبات سنگین با ذخیره سازی نتایج قبلی عمل میکند

یک Callback Fn میگیره و مقدارش توی رندر اول حساب میشه

یک dependency array میگیره و هر زمان که تغییر کنه ان دوباره اجرا میشه

```jsx
const archiveOptions = useMemo(() => {
  return {
    show: false,
    title: `Post archive in addition to ${posts.length} main posts`,
  };
}, [posts.length]);
```

استفاده از useMemo ساخته نشدن مجدد آبجکت

```jsx
const workouts = useMemo(() => {
  return [
    {
      name: 'Full-body workout',
      numExercises: partOfDay === 'AM' ? 9 : 8,
    },
    {
      name: 'Arms + Legs',
      numExercises: 6,
    },
    {
      name: 'Arms only',
      numExercises: 3,
    },
    {
      name: 'Legs only',
      numExercises: 4,
    },
    {
      name: 'Core only',
      numExercises: partOfDay === 'AM' ? 5 : 4,
    },
  ];
}, [partOfDay]);
```
