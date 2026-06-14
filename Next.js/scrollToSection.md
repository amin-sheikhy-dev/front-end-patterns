ابتدا به هر سکشن یا المنت یه ایدی باید داد

```tsx
export default function ContactSection() {
  return (
    <section id="contact">
      <h2>section contact</h2>
    </section>
  );
}
```

و همچنین باید این المنت ها باشن

```tsx
<section id="about">...</section>
<section id="project">...</section>
<section id="skills">...</section>
<section id="contact">...</section>
```

یک تابع هست که با دریافت sectionId، به اون بخش اسکرول می‌کنه با رفتار smooth (نرم).

```tsx
'use client';
import { useCallback } from 'react';

const headerItems = [
  { name: 'درباره من', id: 'about' },
  { name: 'پروژه های من', id: 'project' },
  { name: 'مهارت های من', id: 'skills' },
  { name: 'ارتباط با من', id: 'contact' },
];

export default function Header() {
  const scrollToSection = useCallback((sectionId: string) => {
    const element = document.getElementById(sectionId);

    if (element) element.scrollIntoView({ behavior: 'smooth' });
  }, []);

  return (
    <ul>
      {headerItems.map((item) => (
        <li key={item.id}>
          <button onClick={() => scrollToSection(item.id)}>{item.name}</button>
        </li>
      ))}
    </ul>
  );
}
```
