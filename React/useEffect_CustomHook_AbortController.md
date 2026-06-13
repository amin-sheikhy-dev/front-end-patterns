useEffect

زمانی که کامپوننت اپ برای اولین بار رندر میشه اجرا میشه

بعد از اینکه کامپوننت رندر شد و توی صفحه نمایش داده شد اجرا میشه

```jsx
// CustomHook1
function useMovies(query, KEY) {
  const [movies, setMovies] = useState([]);
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState('');

  useEffect(() => {
    // Abort Controller
    const controller = new AbortController();

    async function fetchMovies() {
      try {
        setIsLoading(true);
        setError('');

        // باعث میشه که به ری کوئست در حال ارسال بود و بعدش به ری کوئست دیگه رده شد ری کوئست قبلی کنسل بشه
        const res = await fetch(`http://www.omdbapi.com/?apikey=${KEY}&s=${query}`, { signal: controller.signal });

        if (!res.ok) throw new Error('Something went wrong with fetching movies');

        const data = await res.json();

        // این بخشیه که از خود API میاد و نتیجه جستجو رو نشون میده
        if (data.Response === 'False') throw new Error('Movie not found');

        setMovies(data.Search);
        setError('');
      } catch (err) {
        // برای اینکه که اگر ابرت ارور بود الکی بیاد تو بلوک کج
        if (err.name !== 'AbortError') setError(err.message);
        // چون وقتی ری کوئست کنسل میشه در واقع یجور ارور هستش و میاد اینجا
      } finally {
        setIsLoading(false);
      }
    }

    if (query.length < 4) {
      setMovies([]);
      setError('');
      return;
    }

    fetchMovies();

    return () => controller.abort();
  }, [query, KEY]);

  // استفاده از مقادیری که کامپوننت هوک ریترن میکنه
  return { movies, isLoading, error };
}

// use CustomHook1
// بعد حالا جایی که هوک ریترن میکنه و تو داخل متغیر مربوطه ذخیره میکنیم
const { movies, isLoading, error } = useMovies(query, KEY);
```
