اول createContext: ایجاد کانتکست با مقدار اولیه null.

```tsx
import React, { createContext, useEffect, useState } from 'react';

interface AuthContextType {
  isLoggedIn: boolean;
  login: () => void;
  logout: () => void;
}

interface AuthProviderProps {
  children: React.ReactNode;
}

// اول کانتکست رو مسازی
export const AuthContext = createContext<AuthContextType | null>(null);

// بعد کامپوننت پرووایدرش رو مسازی همراه چیلدرن هاش
export function AuthProvider({ children }: AuthProviderProps) {
  // اینکه مقدار اولیه استیت رو از لوکال استوریج میگیریم باعث میشه که کاربر حتی بعد خارج شدن باز هم احراز هویت شده بمونه
  const [isLoggedIn, setIsLoggedIn] = useState<boolean>(localStorage.getItem('isAuthenticated') === 'true');

  // این هوک باعث میشه تا هر وقت استیت تغییر کرد در لوکال استوریج ذخیره کنه
  useEffect(() => {
    localStorage.setItem('isAuthenticated', isLoggedIn ? 'true' : 'false');
  }, [isLoggedIn]);

  function login(): void {
    setIsLoggedIn(true);
  }

  function logout(): void {
    setIsLoggedIn(false);
  }

  // و بعش ریترن میکنی و چیلدرن هاشو میبراری داخل کامپوننت
  return <AuthContext.Provider value={{ isLoggedIn, login, logout }}>{children}</AuthContext.Provider>;
}
```

و بعدش کل اپلیکیشن رو داخل پرووایدرش میزاری

AuthProvider: این کامپوننت باید در بالاترین سطح اپلیکیشن قرار بگیره تا تمام کامپوننت‌ها به کانتکست احراز هویت دسترسی داشته باشن.

```tsx
import { AuthProvider } from './context/auth-provider';

createRoot(document.getElementById('root')!).render(
  <AuthProvider>
    <App />
  </AuthProvider>
);
```

useContext: این هوک برای دسترسی به مقدار کانتکست در کامپوننت استفاده میشه.

AuthContext: کانتکستی که قبلاً با createContext ایجاد کردید و توسط AuthProvider مقداردهی شده.

کاربرد: این روش باعث میشه بدون نیاز به Prop Drilling، مقادیر کانتکست در هر کامپوننتی قابل دسترسی باشه.

```tsx
import { useContext } from 'react';
import { AuthContext } from '../../context/auth-provider';

export default function Login() {
  // و بعد در کامپوننت دلخواه استفاده میکنی
  const { isLoggedIn, login } = useContext(AuthContext);
}
```
