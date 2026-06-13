**Local Storage**

لوکال استوریج درواقع جایی در مرورگر هست که دیتا توش ذخیره میکنیم

دوتا ورودی میگیره - اولی کلید هست - دومی هم ولیو هست

```js
localStorage.setItem('key', 'value');

localStorage.getItem(key);
```

توی لوکال استوریج همه چی به شکل استرینگ ذخیره میشه پس باید از متد جیسون استرینگفای استفاده بشه

همووقع مقدار اولیه استیت باید با توجه به سری محاسبات انجام بشه نوشتن به کال بک فانکشن میدیم و توی رندر اولیه اون رو اجرا میکنه

```jsx
function useLocalStorage(initialState, key) {
  const [value, setValue] = useState(() => {
    // برای گرفتن اینم کافی کلید مورد نظر رو بدیم
    const storedValue = localStorage.getItem(key);

    // چیزی که فانکشن ریترن میکنه میشه مقدار اولیه استیت
    // الان این چیزی که ریترن کردیم در واقع مقدار اولیه استیت هست و توی اولین رندر برای ما ست میشه
    return storedValue ? JSON.parse(storedValue) : initialState;
  });

  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(value));
    // اینجا واق و کلید همش تغییر میکنه و اپدیت میشه
  }, [value, key]);

  return [value, setValue];
}

// use CustomHook2
const [watched, setWatched] = useLocalStorage([], 'watched');
```
