جلوگیری از ساخته شدن مجدد توابع در هر رندر کامپوننت

```jsx
const fnName = useCallback(fn, []);
```

همان تابع را ذخیره میکند useCallback

هر متغیری که در داخل تابع استفاده میشود و از محیط بیرونی آمده است با همان Dependency Array باید باشد

```jsx
const handleAddPost = useCallback(function handleAddPost(post) {
  setPosts((posts) => [post, ...posts]);
}, []);

// یا به صورت خلاصه تر Arrow Fn
const handleClearPosts = useCallback(() => {
  setPosts([]);
}, []);
```

اینجا Audio در واقع ویژگی ای هستش که Browser در اختیار ما قرار میده

```jsx
const playSound = useCallback(
  function playSound() {
    if (!allowSound) return;

    const sound = new Audio(clickSound);
    sound.play();
  },
  [allowSound]
);
```

ساز و کار ساخته شدن مجدد کامپوننت اینه که اگه کامپوننت پرنت ری رندر بشه کامپوننت های چایلید هم دوباره ری رندر میشن

برای ممو کردن کامپوننت که مجدد اون کامپوننت ساخته نشه

calculator: کامپوننتی که قراره ممو شه - یا میتونی خودشو بنزاری یا اسمشو

```jsx
export default memo(Calculator);
```

این کامپوننت رو فقط وقتی رندر مجدد میکنه که allowSound یا setAllowSound تغییر کنه. این باعث بهینه‌سازی عملکرد میشه
React.memo - useCallback

```jsx
import { memo } from 'react';

const ToggleSounds = React.memo(function ToggleSounds({ allowSound, setAllowSound }) {
  const handleClick = useCallback(() => {
    setAllowSound((allow) => !allow);
  }, [setAllowSound]);

  return <button onClick={handleClick}>{allowSound ? '🔊' : '🔇'}</button>;
});

export default ToggleSounds;
```

روشن بهتر وقتی که اکسپورت میکنی ممو کنین

```jsx
export default memo(ToggleSounds);
```
