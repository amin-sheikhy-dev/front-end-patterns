تابعی که در هوک قراره استفاده و در کامپوننت ایمپورت شه

```jsx
export async function fetchEvents({ signal, searchTerm, max }) {
  let URL = 'http://localhost:3000/events';

  if (searchTerm && max) {
    URL += '?search=' + searchTerm + '&max=' + max;
  } else if (searchTerm) {
    URL += '?search=' + searchTerm;
  } else if (max) {
    URL += '?max=' + max;
  }

  const response = await fetch(URL, { signal: signal });

  if (!response.ok) {
    const error = new Error('An error occurred while fetching the events');
    error.code = response.status;
    error.info = await response.json();
    throw error;
  }

  const { events } = await response.json();
  return events;
}
```

data = خود دیتای سرور

isLoading = isPending = فقط وقتی برای اولین بار دیتا میگیری

بعد از اینکه دیتا اومد دیگه هیچ وقت refetch = true نمیشه

isError = آیا درخواست شکست خورده؟

error = جزئیات خطا

```jsx
const { data, isPending, isError, error } = useQuery({
  queryKey: ['events', { max: 3 }],

  // این تابعی که بهش میدیم باید یک پرامیس ریترن کنه که
  // این تابع ورودی هایی دارد که ما ان را دی استراکچر میکنیم
  // { client: _a12 , queryKey: ['events', {max: 3}] , meta: undefined }
  queryFn: ({ signal, queryKey }) => fetchEvents({ signal, ...queryKey[1] }),

  // مدت زمانی که دیتا دارد محسوب میشود و نیازی به دوباره فچ شدن ندارد
  staleTime: 5 * 60 * 1000, // هر 5 دقیقه فچ شود

  // مدت زمانی که دیتا بعد از اینکه دیگر هیچ کامپوننتی از آن استفاده نمیکند در cache نگه داشته میشود قبل از اینکه پاک شود
  gcTime: 30 * 60 * 1000, // تا نیم ساعت استفاده شده نیمه و پاک نمیشه

  enabled: true, // به صورت پیش فرض true هست و میشه فن رو غیرفعال کرد یا ان را کپی کنید
});
```

# مثال 2

تابعی که در هوک قراره استفاده و در کامپوننت ایمپورت شه

```jsx
export async function fetchEvents({ signal, searchTerm, max }) {
  let URL = 'http://localhost:3000/events';

  if (searchTerm && max) {
    URL += '?search=' + searchTerm + '&max=' + max;
  } else if (searchTerm) {
    URL += '?search=' + searchTerm;
  } else if (max) {
    URL += '?max=' + max;
  }

  const response = await fetch(URL, { signal: signal });

  if (!response.ok) {
    const error = new Error('An error occurred while fetching the events');
    error.code = response.status;
    error.info = await response.json();
    throw error;
  }

  const { events } = await response.json();
  return events;
}
```

```jsx
const searchElement = useRef(null);
const [searchTerm, setSearchTerm] = useState('');

const { data, isFetching, isError, error } = useQuery({
  queryKey: ['events', { search: searchTerm }],

  queryFn: ({ signal }) => fetchEvents({ signal, searchTerm }),

  enabled: searchTerm !== '', // اگه سرچ ترم وجود نداشت فچ انجام نشود
});

function handleSubmit(event) {
  event.preventDefault();
  setSearchTerm(searchElement.current.value);
}
```
