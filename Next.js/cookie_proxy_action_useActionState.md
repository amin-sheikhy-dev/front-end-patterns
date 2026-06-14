### این کد مربوط به صفحه لاگین با استفاده از `useActionState` و `Server Action` هست:

و `useActionState:` یک هوک `React` هست که برای مدیریت وضعیت فرم با `Server Actions` استفاده میشه. آرگومان اول تابع `login` `(یک Server Action)` و آرگومان دوم مقدار اولیه `state` `(اینجا {})` هست.

- و `state:` وضعیت فعلی فرم که توسط `Server Action` برگردونده میشه (مثلاً خطاها).

- و `formAction:` تابعی که به `action` فرم پاس داده میشه و فرم رو ارسال می‌کنه.

- و `isPending:` یک مقدار `Boolean` هست که نشون میده آیا فرم در حال ارسال هست یا نه (برای نمایش لودینگ و غیرفعال کردن دکمه).

```tsx
'use client';

import { login } from '@/lib/login';
import { useActionState } from 'react';

export default function LoginPage() {
  const [state, formAction, isPending] = useActionState(login, {});

  return (
    <form action={formAction}>
      <div>
        <label htmlFor="email">Email</label>

        <input type="email" id="email" name="email" placeholder="example@gmail.com" />

        {state.errors?.email && <p>{state.errors.email}</p>}
      </div>

      <div>
        <label htmlFor="password">Password</label>

        <input type="password" id="password" name="password" placeholder="••••••••" />

        {state.errors?.password && <p>{state.errors.password}</p>}
      </div>

      <button type="submit" disabled={isPending}>
        {isPending ? 'Wait...' : 'Login'}
      </button>
    </form>
  );
}
```

---

### این کد مربوط به `Server Action` برای لاگین با اعتبارسنجی و ست کردن کوکی هست:

- و `crypto.randomUUID():` یک توکن یکتا برای کوکی ایجاد میکنه.

- و `httpOnly:` `true:` فقط سرور میتونه کوکی رو بخونه (برای امنیت بالاتر).

- و `secure:` در محیط `production` فقط از طریق `HTTPS` کوکی ارسال میشه.

- و `maxAge:` تعیین میکنه کوکی چقدر در مرورگر باقی بمونه (اینجا ۲۴ روز).

- و `sameSite:` `'lax':` تعادل بین امنیت و دسترسی کاربر از سایت‌های دیگر ایجاد میکنه.

- و `redirect('/about'):` بعد از لاگین موفق، کاربر رو به صفحه `/about` هدایت میکنه.

```tsx
'use server';

import { cookies } from 'next/headers';
import { redirect } from 'next/navigation';

export interface FormState {
  errors?: { email: string; password: string };
  userInfo?: { email: string; name: string };
}

export async function login(prevState: FormState, formData: FormData): Promise<FormState> {
  // گرفتن دیتا های فرم
  const email = formData.get('email') as string;
  const password = formData.get('password') as string;

  const errors: FormState['errors'] = {};

  // ابتدا معتبر سازی
  if (!email.trim() || !email.includes('@')) errors.email = 'Invalid email format';

  if (!password.trim() || password.length < 3) errors.password = 'Password must have 3 characters';

  if (Object.keys(errors).length > 0) return { errors };

  // ساخت کوکی و توکن
  const cookieStore = await cookies();
  const token: string = crypto.randomUUID(); // یک استرینگ عول یونیک و یونیک بسازه

  // این کوکی مخصوص سروره
  cookieStore.set('auth-token', token, {
    httpOnly: true, // فقط سرور میتونه بخونش

    // وقتی حالت پروداکشن باشه ترو میره که در اصل باید ترو باشه
    secure: process.env.NODE_ENV === 'production',
    // development: secure=false ست میشه توی http
    // production: secure=true ست میشه توی https

    maxAge: 60 * 60 * 24, // تعیین میکنه کوکی کی وجود داره و پاک بشه - تا ثانیه تنظیم کردیم - ما 24 ساعت تنظیم کردیم

    path: '/', // تعیین میکنه کوکی در چه مسیرهایی از سایت قابل دسترسی باشه - همه صفحات سایت

    // sameSite: 'strict', // یعنی کاری از به لینک دیگه وارد سایت بشه کوکی وجود نداره

    sameSite: 'lax', // اینجوری کاربر از به سایت دیگه اومد وارد بشه کوکی وجود داره و بهترین حالت هم همینه
  });

  const userInfo: FormState['userInfo'] = {
    email: email,
    name: email.split('@')[0], // اسم از رو ایمیل
  };

  // توکن کلاینت
  // در حالت تمرینی مهم نیست ولی در کل بهتره کوکی ها داخل کلاینت ذخیره نشن و هرجا لازم بود از ای پی آی روت نکست یا از سرور گرفته بشه
  const clientToken = JSON.stringify(userInfo);
  cookieStore.set('client-token', clientToken, {
    httpOnly: false,
    secure: process.env.NODE_ENV === 'production',
    maxAge: 60 * 60 * 24,
    path: '/',
    sameSite: 'lax',
  });

  // باعث میشه تا این صفحه دوباره از سرور دیتا فج کنه و از دیتای قدیمی استفاده نکنه
  rivalidatePath('/about');
  // کاربر رو به صفحه ای که میخوای بفرست
  redirect('/about');
}
```

---

### این کد مربوط به Middleware در Next.js برای محافظت از روت‌ها با استفاده از کوکی هست:

- و `req.cookies.get('auth-token')?.value:` مقدار توکن رو از کوکی میخونه. اگر وجود نداشته باشه، `undefined` برمیگردونه.

- و `protectedRoute.some():` چک میکنه که آیا مسیر فعلی یکی از مسیرهای محافظت شده هست یا نه.

- و `needsAuth && !token:` اگر مسیر محافظت شده هست و توکن وجود نداره، کاربر به `/login` ریدایرکت میشه.

- و `token && (pathname === '/login' || pathname === '/'):` اگر توکن وجود داره و کاربر در صفحه لاگین یا صفحه اصلی هست، به `/about` ریدایرکت میشه (چون لاگین کرده و دیگه نباید صفحه لاگین رو ببینه).

- و `config.matcher:` مشخص میکنه که این `Middleware` فقط برای مسیرهای مشخص شده اجرا بشه (برای بهینه‌سازی).

```tsx
import { NextResponse, NextRequest } from 'next/server';

export function proxy(req: NextRequest) {
  // pathname مسیری که کاربر داره واردش میشه
  const pathname: string = req.nextUrl.pathname;

  // دریافت توکن که آیا کاربر احراز هویت شده یا خیر - کوکی 2 پراپرتی اسم و مقدار داره
  // در صورتی که کوکی وجود نداشته باشه انفایند برمیگردونه

  const token: string | undefined = req.cookies.get('auth-token')?.value; // این مقدار کوکی رو میده
  const token2 = req.cookies.get('auth-token')?.name; // این اسم کوکی رو میده

  // تعریف مسیرهایی که نیاز به محافظت داره
  const protectedRoute: string[] = ['/blog', '/menu'];

  // به ما به مقدار ترو یا فالس میده needsAuth
  const needsAuth: boolean = protectedRoute.some((route) => pathname.startsWith(route));
  // pathname.startsWith('/ANY_ROUTE') مقدار ترو یا فالس میده

  if (needsAuth && !token) {
    // کاربر لاگین نکرده بفرست به صفحه ورود
    return NextResponse.redirect(new URL('/login', req.url));
  }

  // اگر کاربر لاگین کرده و میخواد به صفحه لاگین یا به صفحه اصلی ریدایرکت شه
  if (token && (pathname === '/login' || pathname === '/')) {
    return NextResponse.redirect(new URL('/about', req.url));
  }

  return NextResponse.next();
}

// یعنی بروکسی فقط وقتی اجرا میشود که کاربر به این آدرسها برود - بهینه و سریع تر
// /:path* = یعنی تمام روت های زیرین
export const config = {
  matcher: ['/', '/blog/:path*', '/menu/:path*', '/login'],
};
```

---

### این کد مربوط به خواندن کوکی در Server Component هست:

- و `cookies():` یک تابع از `next/headers` هست که دسترسی به کوکی‌ها را در `Server Components` و `Server Actions` فراهم می‌کنه.

- و `await cookies():` در `Next.js 15+` و `cookies()` یک `Promise` برمی‌گردونه و باید با `await` صدا زده بشه.

- و `cookie.get('client-token'):` کوکی با نام `client-token` رو می‌خونه. اگر وجود نداشته باشه، `undefined` برمی‌گردونه.

- و `JSON.parse(token.value):` مقدار کوکی رو که به صورت رشته `(String)` هست به آبجکت تبدیل می‌کنه.

- و `token ? ... : null:` اگر توکن وجود نداشته باشه (کاربر لاگین نکرده باشه)، مقدار `null` میذاره.

```tsx
import { cookies } from 'next/headers';

export default async function UserProfilePage() {
  // cookies
  const cookie = await cookies();
  const token = cookie.get('client-token');
  const clientCookie = token ? JSON.parse(token.value) : null;
}
```

---

### این کد مربوط به `Server Action` برای خروج `(Logout)` و پاک کردن کوکی‌ها هست:

```tsx
'use server';

import { cookies } from 'next/headers';
import { redirect } from 'next/navigation';

// اگه بخای از این فانکشن در کلاینت استفاده کنی باید از هوک یوز ترانزیشن استفاده کنی
export async function logout() {
  const cookie = await cookies();

  // پاک کردن کوکی‌ها
  cookie.delete('auth-token');
  cookie.delete('client-token');

  redirect('/login');
}
```

---

### به هوک ریاکت هست که اجازه میده بدون اینکه کل صفحه فریز بشه، به کار سنگین مثل سرور اکشن رو انجام بدی

`isPending` ---> `true/false` (نشون میده کار تموم شده یا نه)

`startTransition` -> تابعی که کار سنگین رو توش میذاری

```tsx
const [isPending, startTransition] = useTransition();

function handleLogout() {
  // Server Action رو اینجا صدا میزنیم
  startTransition(() => logout());
}
```

حالا این تابع `handleLogout` رو توی دکمه های کامپوننت استفاده کن

---

### این کد مربوط به `Layout` اصلی با اضافه کردن `CookiesProvider` هست:

```tsx
import { CookiesProvider } from 'next-client-cookies';

export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <body>
        {/* اضافه کردن این پروایدر برای استفاده از کوکی ها در کلاینت */}
        <CookiesProvider>
          <ThemeProvider>{children}</ThemeProvider>
        </CookiesProvider>
      </body>
    </html>
  );
}
```

---

### این کد مربوط به خواندن کوکی در کلاینت با استفاده از `useCookies` هست:

```tsx
import { useCookies } from 'next-client-cookies';

export default function Header() {
  const cookies = useCookies();

  const clientCookie = cookies.get('client-token');
  // چون ممکنه مقدار کوکی هنوز ست نشده باشه اول میسنجیم مقدارشو
  const cookie = clientCookie ? JSON.parse(clientCookie) : null;

  // حال هرجا خاستیم ازش استفاده میکنیم
  console.log(cookie.name);
  console.log(cookie);
}
```
