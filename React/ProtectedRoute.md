ابتدا باید با ریداکس و لوکال استوریج یا زوستند کانفیگ و منطق رو بنویسیم

```ts
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

interface AuthStoreType {
  isLoggedIn: boolean;
  login: () => void;
  logout: () => void;
}

// این مربوط به تایپ اسکریپت هست نه هیچ چیز دیگه
// بدون تایپ اسکریپت اینجوریه export const useAuthStore = create(
export const useAuthStore = create<AuthStoreType>()(
  persist(
    (set) => ({
      // توجه شود که چون در لوکال استوریج ذخیره میشه پس بعد از اینکه یکبار از تابع لاگین استفاده شه و مقدار ترو شه دیگه فالس نمیشه
      // مگه اینکه با تابع لاگ اوت دوباره فالس شه یعنی در کل فقط با تابع میتونی مقدار آن را عوض کرد نه هیچ چیز دیگه
      isLoggedIn: false,
      login: () => set({ isLoggedIn: true }),
      logout: () => set({ isLoggedIn: false }),
    }),
    {
      name: 'auth-storage', // کلید ذخیره‌سازی در لوکال استوریج
    }
  )
);
```

useEffect: این بخش باعث میشه اگر کاربری که قبلاً لاگین کرده (و isLoggedIn مقدار true داره) به طور خودکار به صفحه '/menu' هدایت بشه. این برای جلوگیری از دسترسی به صفحه لاگین توسط کاربران لاگین شده استفاده میشه.

handleSubmit: در این تابع، ابتدا چک میکنه که هر دو فیلد کاربر و رمز عبور پر باشن، سپس لاگین رو انجام میده و کاربر رو به '/menu' هدایت میکنه.

استفاده از replace: true: در useEffect از replace: true استفاده شده تا صفحه لاگین از تاریخچه مرورگر حذف بشه و کاربر با دکمه Back نتونه به صفحه لاگین برگرده.

```tsx
export default function Login() {
  const navigate = useNavigate();
  const isLoggedIn = useAuthStore((s) => s.isLoggedIn);
  const login = useAuthStore((s) => s.login);

  // if (isLoggedIn) navigate('/menu');
  useEffect(() => {
    if (isLoggedIn) {
      navigate('/menu', { replace: true });
    }
  }, [isLoggedIn, navigate]);

  const userNameInput = useRef<HTMLInputElement>(null);
  const userPasswordInput = useRef<HTMLInputElement>(null);

  function handleSubmit(e: React.SyntheticEvent): void {
    e.preventDefault();

    if (userNameInput.current?.value && userPasswordInput.current?.value) {
      login();
      navigate('/menu');
    }
  }
}
```

هدف کامپوننت: این کامپوننت فقط وقتی فرزندانش (children) رو رندر میکنه که کاربر لاگین کرده باشه.

شرط اصلی: اگر isLoggedIn برابر false باشه، کاربر به صفحه /login هدایت میشه و فرزندان رندر نمیشن.

```tsx
import type React from 'react';
import { Navigate } from 'react-router-dom';
import { useAuthStore } from '../../store/auth-store';

interface ProtectedRouteProps {
  children: React.ReactNode;
}

// این کامپوننت فقط در صورتی بچه هاشو رندر میکنه که کاربر لاگین کرده باشه
export default function ProtectedRoute({ children }: ProtectedRouteProps) {
  const isLoggedIn = useAuthStore((s) => s.isLoggedIn);

  // اگر کاربر لاگین نشده بود بچه هاشو رندر نمیشه و نویگیت میکنه به صفحه لاگین
  if (!isLoggedIn) return <Navigate to="/login" replace />;

  return <>{children}</>;
}
```

روش استفاده: کامپوننت ProtectedRoute به عنوان یک Wrapper (پوشش) دور کامپوننت‌هایی که نیاز به احراز هویت دارند قرار میگیره.

ساختار تودرتو: در این روش، کامپوننت‌های Menu و MenuItem فقط در صورتی رندر میشن که کاربر لاگین کرده باشه.

مسیرها: هر دو مسیر /menu و /menu/:id (جزئیات آیتم منو) محافظت شده هستن.

جریان کار: اگر کاربر لاگین نباشه، کامپوننت ProtectedRoute به جای رندر کردن Menu یا MenuItem، کاربر رو به صفحه /login هدایت میکنه.

```tsx
{
  path: '/menu',
  element: (
    <ProtectedRoute>
      <Menu />
    </ProtectedRoute>
  ),
},
{
  path: '/menu/:id',
  element: (
    <ProtectedRoute>
      <MenuItem />
    </ProtectedRoute>
  ),
},
```
