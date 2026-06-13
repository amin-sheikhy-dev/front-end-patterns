تغییر مقدار هوک ref باعث رندر شدن مجدد کامپوننت نمیشه برعکس هوک استیت که تغییر باعث رندر شدن کامپوننت میشه

با استفاده از اتیریبوت ref که خود ری اکت داده میتونیم وصلش کنیم به کامپوننت و اسم متغیر هوک رو میدیم بهش

درواقع به آبجکت هست که با current مقدار خود المنت رو میگیریم

```tsx
export default function Form() {
  const goalRef = useRef<HTMLInputElement>(null);
  const summaryRef = useRef<HTMLInputElement>(null);

  function handleSubmit(event: SubmitEvent<HTMLFormElement>): void {
    event.preventDefault();

    const goalInput = goalRef.current.value;
    const summaryInput = summaryRef.current.value;

    console.log(goalInput, summaryInput);
  }

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label htmlFor="goal">Your goal</label>

        <input id="goal" type="text" ref={goalRef} />
      </div>

      <div>
        <label htmlFor="summary">Short summary</label>

        <input id="summary" type="text" ref={summaryRef} />
      </div>

      <button type="submit">Add Goal</button>
    </form>
  );
}
```

```jsx
function Search({ query, setQuery }) {
  const searchInput = useRef(null);

  useEffect(() => {
    function callback(e) {
      // اول مشخص کن کدوم دکمه فشرده شده
      if (e.code === 'Enter') {
        // هر کدی که خاستی داخل این مینویسی
        searchInput.current.focus();

        setQuery('');
      }
    }

    document.addEventListener('keydown', callback);

    return () => document.removeEventListener('keydown', callback);
  }, [setQuery]);

  return (
    <>
      <p>search movies</p>

      <input type="text" value={query} onChange={(e) => setQuery(e.target.value)} ref={searchInput} />
    </>
  );
}
```

این یک Custom Hook هست که به شما اجازه میده به فشرده شدن یک کلید خاص گوش بدید و یک تابع (action) رو اجرا کنید.

تابع callback هر بار که کاربر کلیدی رو فشار میده اجرا میشه و اگه e.code با کلید مورد نظر (key) یکی باشه، action() رو صدا میزنه.

```jsx
function useKey(key, action) {
  useEffect(() => {
    const callback = function (e) {
      if (e.code.toLowerCase() === key.toLowerCase()) action();
    };

    document.addEventListener('keydown', callback);

    return () => document.removeEventListener('keydown', callback);
  }, [key, action]);
}

// use
useKey('escape', callback);
```

onSubmit و onChange دوتا ایونت

ایونت onChange وقتی رخ میده که value تغییر کند

```jsx
function handleChange(e) {
  setDescription(e.target.value);
}

return (
  <form onSubmit={handleSubmit}>
    <h3>what do you need for your trip?</h3>

    <select value={quantity} onChange={(e) => setQuantity(Number(e.target.value))}>
      {Array.from({ length: 20 }, (_, i) => i + 1).map((num) => (
        <option value={num} key={num}>
          {num}
        </option>
      ))}
    </select>

    <input type="text" placeholder="your item..." value={description} onChange={(e) => setDescription(e.target.value)} />
    {/* هر دو روش اینپوت های کنترل شده */}
    <input type="text" placeholder="your item..." value={description} onChange={handleChange} />

    <button>Add</button>
  </form>
);
```
