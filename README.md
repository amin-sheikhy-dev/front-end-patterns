# 📚 مجموعه یادداشت‌های فنی React, Next.js و فرانت‌اند

> این فایل‌ها در اصل برای **مرور و یادآوری شخصی** من نوشته شده‌اند، اما از آنجا که ممکن است برای دیگران هم مفید باشند، این مخزن را عمومی کرده‌ام.  
> لطفاً در نظر داشته باشید که برخی یادداشت‌ها صرفاً خلاصه‌ی کدها و مفاهیم هستند و توضیحات گسترده ندارند.

---

## 📁 ساختار مخزن

| فایل                                      | موضوع                                                         | دسته‌بندی                |
| ----------------------------------------- | ------------------------------------------------------------- | ------------------------ |
| `bgComponent.md`                          | کامپوننت پس‌زمینه با `next/image`                             | Next.js / استایل         |
| `clipboard.md`                            | کپی متن به کلیپ‌بورد با Clipboard API                         | مرورگر / تعامل           |
| `ContextApi.md`                           | پیاده‌سازی Context API برای احراز هویت                        | React / State Management |
| `crypto.randomUUID.md`                    | تولید شناسه یکتا با `crypto.randomUUID()`                     | مرورگر / ابزار           |
| `FileReader-BrowserAPI.md`                | پیش‌نمایش عکس با FileReader                                   | مرورگر / فایل            |
| `FramerMotion.md`                         | انیمیشن‌های ورود/خروج، درگ، اسکرول با Framer Motion           | انیمیشن                  |
| `newUrl.md`                               | استخراج دامنه از URL با `new URL()`                           | ابزار / رشته             |
| `ReactQuery.md`                           | کانفیگ، useQuery، useMutation در TanStack Query               | داده / سرور              |
| `Redux.md`                                | ایجاد slice، استور، useDispatch، useSelector                  | State Management         |
| `sound-(BrowserAPI).md`                   | پخش صدا با `new Audio()`                                      | مرورگر / صدا             |
| `window.confirm.md`                       | دیالوگ تایید با `window.confirm`                              | مرورگر / تعامل           |
| `Zustand.md`                              | استورهای Theme، Auth، Todo، Meal با persist                   | State Management         |
| `DarkMode.md`                             | دو روش پیاده‌سازی دارک مود (Zustand + Tailwind و next-themes) | استایل / Next.js         |
| `document.title.md`                       | تغییر عنوان تب مرورگر با `document.title`                     | مرورگر / SEO             |
| `localStorage_CustomHook.md`              | هوک سفارشی برای ذخیره‌سازی در localStorage                    | ذخیره‌سازی / React       |
| `onMouseEnter_onMouseLeave.md`            | تشخیص هاور با رویدادهای موس                                   | React / رویدادها         |
| `ProtectedRoute.md`                       | محافظت از مسیرها (Zustand + React Router)                     | احراز هویت               |
| `Ref_KeyEvent_Form.md`                    | استفاده از ref، رویدادهای کیبورد، فرم‌های کنترل‌شده           | React / فرم              |
| `useCallback_React.memo.md`               | بهینه‌سازی با useCallback و memo                              | بهینه‌سازی               |
| `useEffect_CustomHook_AbortController.md` | هوک سفارشی با AbortController برای لغو fetch                  | داده / هوک               |
| `useMemo.md`                              | ذخیره‌سازی محاسبات سنگین با useMemo                           | بهینه‌سازی               |
| `useReducer.md`                           | مدیریت state پیچیده با useReducer                             | State Management         |
| `useTransition.md`                        | اجرای Server Action بدون مسدود کردن UI                        | React 19 / Next.js       |
| `cookie_proxy_action_useActionState.md`   | لاگین با Server Action، middleware، کوکی، useActionState      | Next.js / احراز هویت     |
| `error.tsx.md`                            | Error Boundary در Next.js App Router                          | Next.js                  |
| `Font.md`                                 | استفاده از فونت محلی با next/font/local                       | Next.js / استایل         |
| `generateMetadata_[slug].md`              | تولید متادیتای پویا در Next.js                                | Next.js / SEO            |
| `layout.tsx.md`                           | لی‌اوت اشتراکی در App Router                                  | Next.js                  |
| `loading.tsx.md`                          | fallback خودکار Suspense با loading.tsx                       | Next.js                  |
| `not-found.tsx.md`                        | صفحه 404 سفارشی                                               | Next.js                  |
| `page.tsx.md`                             | صفحه اصلی هر مسیر                                             | Next.js                  |
| `RouteGrouping_ParallelRoute.md`          | گروه‌بندی مسیرها و Route Parallel                             | Next.js                  |
| `scrollToSection.md`                      | اسکرول نرم به بخش‌های مختلف صفحه                              | React / تعامل            |
| `Suspense.md`                             | استفاده از Suspense برای fallback در حین fetch                | React                    |
| `useFormStatus.md`                        | نمایش وضعیت ارسال فرم در Server Actions                       | React 19 / فرم           |
| `usePathname.md`                          | تشخیص لینک فعال در Next.js                                    | Next.js                  |
| `useRouter.md`                            | ناوبری برنامه‌ای (back, push) در Next.js                      | Next.js                  |

---

## 🚀 راهنمای سریع

### برای یادگیری مفاهیم پایه React:

- `useReducer.md` ، `ContextApi.md`
- `Ref_KeyEvent_Form.md` (کار با ref و فرم)
- `useEffect_CustomHook_AbortController.md` (هوش سفارشی و درخواست شبکه)

### برای مدیریت state پیشرفته:

- `Redux.md` (اگر پروژه از Redux استفاده می‌کند)
- `Zustand.md` (سبک‌تر و مدرن‌تر – پیشنهادی)

### برای بهینه‌سازی عملکرد:

- `useCallback_React.memo.md`
- `useMemo.md`

### برای کار با Next.js (App Router):

- `layout.tsx.md` ، `page.tsx.md` ، `loading.tsx.md` ، `error.tsx.md` ، `not-found.tsx.md`
- `generateMetadata_[slug].md` (SEO)
- `usePathname.md` ، `useRouter.md`
- `cookie_proxy_action_useActionState.md` (احراز هویت کامل با Server Action + middleware)

### برای انیمیشن:

- `FramerMotion.md` (شامل مثال‌های ورود/خروج، اسکرول، درگ)

### برای کار با مرورگر:

- `clipboard.md` ، `sound-(BrowserAPI).md` ، `window.confirm.md`
- `FileReader-BrowserAPI.md` (پیش‌نمایش عکس)
- `crypto.randomUUID.md`

### برای استایل و تم:

- `DarkMode.md` (دو روش پیاده‌سازی)
- `Font.md` (فونت محلی)
- `bgComponent.md` (تصویر پس‌زمینه بهینه با next/image)

---

## ⚠️ نکات مهم

- برخی فایل‌ها **بدون ایمپورت کامل** نوشته شده‌اند – لطفاً ایمپورت‌های لازم را بر اساس نیاز پروژه خود اضافه کنید.
- فایل `DarkMode.md` دو بار ارائه شده (روش دستی و روش next-themes) – روش دوم برای Next.js مناسب‌تر است.
- در `ReactQuery.md` فرض شده `queryClient` قبلاً ساخته شده – برای استفاده حتماً `QueryClientProvider` را در ریشه برنامه قرار دهید.
- تمام کدهای سمت کلاینت که از هوک‌های مرورگر استفاده می‌کنند (مثل clipboard, sound) باید در کامپوننت با `'use client'` قرار گیرند.

---

## 📝 پیشنهاد برای استفاده بهتر

1. **مطالعه بر اساس اولویت:** اگر مبتدی هستید، از فایل‌های پایه React (`useReducer`، `ContextApi`، فرم‌ها) شروع کنید.
2. **اجرای عملی کدها:** بسیاری از قطعه‌ها را می‌توانید در یک پروژه‌ی آزمایشی (مثلاً `create-react-app` یا `create-next-app`) تست کنید.
3. **کپی و سفارشی‌سازی:** آزادانه کدها را کپی کنید، اما حتماً با نیاز خود تطبیق دهید.

---

## 🤝 مشارکت

اگر مطلب ناقص یا اشتباهی دیدید، خوشحال می‌شوم از طریق Issue یا Pull Request مطلع شوم.  
(این مخزن بیشتر جنبه‌ی شخصی دارد، اما بهبود همیشه خوشایند است.)

---

**ایجاد شده با** 💙 **برای یادگیری سریع‌تر و مرور آسان‌تر.**
