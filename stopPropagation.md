باعث میشه وقتی روی کامپوننت کلیک کردی به کامپوننت های زیری بابل نشه

یک مثال از اضافه و حذف به علاقه مندی ها

```tsx
'use client';

import { useNewsStore } from '@/store/news-store';
import { News } from '@/types/types';
import { useRouter } from 'next/navigation';
import React from 'react';

export default function AddFavoriteNews({ newsData }: { newsData: News }) {
  const router = useRouter();

  const favoriteNews = useNewsStore((s) => s.favoriteNews);
  const addFavoriteNews = useNewsStore((s) => s.addFavoriteNews);
  const removeFavoriteNews = useNewsStore((s) => s.removeFavoriteNews);

  const isExist = favoriteNews.some((n) => n.id === newsData.id);

  function handleFavorite(e: React.MouseEvent<HTMLButtonElement>) {
    e.stopPropagation();
    e.preventDefault();

    if (!newsData.authToken) {
      router.push('/sign-up');
      return;
    }

    if (isExist) {
      removeFavoriteNews(newsData);
    } else {
      addFavoriteNews(newsData);
    }
  }

  return (
    <button className="active:scale-80 hover:scale-110" onClick={handleFavorite}>
      {isExist ? '-' : '+'}
    </button>
  );
}
```

یک مثال از مدال که باعث نشه کلیک به المنت های زیری هم بره

```tsx
'use client';

import { AnimatePresence } from 'framer-motion';
import React, { useEffect, useState } from 'react';
import { createPortal } from 'react-dom';

interface ModalProps {
  children: React.ReactNode;
  isOpen: boolean;
  onClose: (value: boolean) => void;
}

export default function Modal({ children, isOpen, onClose }: ModalProps) {
  const [mounted, setMounted] = useState(false);

  useEffect(() => {
    // eslint-disable-next-line react-hooks/set-state-in-effect
    setMounted(true);
  }, []);

  if (!mounted) return null;

  return createPortal(
    <AnimatePresence>
      {isOpen && (
        <div onClick={() => onClose(false)} className="fixed inset-0 z-[9999] flex items-center justify-center bg-black/20 backdrop-blur-[15px]">
          <div className="z-[10000]" onClick={(e) => e.stopPropagation()}>
            {children}
          </div>
        </div>
      )}
    </AnimatePresence>,
    document.body
  );
}
```
