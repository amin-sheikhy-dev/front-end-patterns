تابعی که به هوک میدیم و از جای دیگه ایمپورت میشه

```jsx
export async function createNewEvent(eventData) {
  const response = await fetch('http://localhost:3000/events', {
    method: 'POST',
    body: JSON.stringify(eventData),
    headers: {
      'Content-Type': 'application/json',
    },
  });

  if (!response.ok) {
    const error = new Error('An error occurred while creating the event');
    error.code = response.status;
    error.info = await response.json();
    throw error;
  }

  const { event } = await response.json();
  return event;
}
```

queryClient باید ایمپورت شود

```jsx
import { createNewEvent, queryClient } from '../http.js';
```

هوک useMutation یک هوک در ری اکت کوئری است که برای ارسال تغییرات به سرور استفاده میشود

یعنی وقتی میخواهید داده‌ای اضافه یا حذف یا ویرایش کنید از useMutation استفاده میکنید

- mutate = یک تابع است که وقتی میخواهید تغییرات را ارسال کنید فراخوانی میکنید

- isPending = وضعیت ارسال تغییرات را نشان میدهد اگر در حال ارسال باشد ترو است

- isError = نشان میدهد که آیا در ارسال تغییرات خطایی رخ داده است یا نه

- error = جزئیات خطا را در صورت وجود به ما می‌دهد

و mutationFn تابعی است که عملیات واقعی روی سرور را انجام میدهد و یک تابع async میگیرد

تابع onSuccess تابعی هست که هنگامی اجرا میشود که mutate با موفقیت اجرا شود

```jsx
const { mutate, isPending, isError, error } = useMutation({
  mutationFn: createNewEvent,

  onSuccess() {
    // invalidateQueries شوند fetch باعث میشود که کوئری های مشخص شده دوباره
    // exact = شود fetch یعنی فقط کوئری با همین کلید دقیقا دوباره
    queryClient.invalidateQueries({ queryKey: ['events'], exact: true });

    navigate('/events');
  },
});

function handleSubmit(formData) {
  mutate({ event: formData });
}
```
