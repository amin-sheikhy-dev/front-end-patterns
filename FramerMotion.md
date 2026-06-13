### AnimatePresence

```jsx
import { motion, AnimatePresence } from 'framer-motion';
import { useState } from 'react';

function ToggleButton() {
  const [isVisible, setIsVisible] = useState(true);

  return (
    <div>
      <button onClick={() => setIsVisible(!isVisible)}>Toggle</button>

      <AnimatePresence>
        {isVisible && (
          <motion.div
            initial={{ opacity: 0, y: 20 }}
            animate={{ opacity: 1, y: 0 }}
            exit={{ opacity: 0, y: -20 }} // انیمیشن خروج
            transition={{ duration: 0.5 }}
          >
            Bye😊
          </motion.div>
        )}
      </AnimatePresence>
    </div>
  );
}
```

---

### AnimatePresence_WhileHover

باید داخل کامپوننت AnimatePresence چیزیای که شرطی رندر میشن رو بزاریم

تایپ میکنه چجور بین انیمیشن ها حرکت کنیم transition

یعنی انیمیشن مثل فیز واقعی رفتار میکنه Spring

و 2 نوع spring و tween داریم

میگه فنر چقدر محکم میشه stiffness

یعنی المان چقدر سنگینه mass

```jsx
return (
  <>
    <AnimatePresence>{isCreatingNewChallenge && <NewChallenge onDone={handleDone} />}</AnimatePresence>

    <header id="main-header">
      <h1>Your Challenges</h1>

      <motion.button
        onClick={handleStartAddNewChallenge}
        className="button"
        whileHover={{ scale: 1.04 }}
        transition={{ type: 'spring', stiffness: 500 }}
      >
        Add Challenge
      </motion.button>
    </header>
  </>
);
```

---

### ArrayValue

این کد یک انیمیشن چند مرحله‌ای (Keyframes) با کنترل دقیق زمان‌بندی رو نشون میده

این آرایه دقیقاً مشخص میکنه که هر کلیپفریم (مرحله) در چه زمانی از کل انیمیشن (از 0 تا 1) اجرا بشه

```jsx
<motion.div
  animate={{
    scale: [1, 1.5, 1.5, 1, 1],
    rotate: [0, 0, 180, 180, 0],
    borderRadius: ['0%', '0%', '50%', '50%', '0%'],
  }}
  transition={{
    duration: 2,
    ease: 'easeInOut',
    times: [0, 0.2, 0.5, 0.8, 1], // کنترل زمان دقیق برای هر کلیپفریم
    repeat: Infinity, // اجرای بینهایت
    repeatDelay: 1,
  }}
/>
```

---

### useScroll_useTransform

این کد استفاده از useScroll و useTransform در Framer Motion برای انیمیشن‌های مبتنی بر اسکرول رو نشون میده

و useTransform مقدار اسکرول رو به مقادیر انیمیشنی تبدیل میکنه. مثلاً وقتی از 0 به 200 پیکسل اسکرول بشه، opacityCity از 1 به 0.5 میره.

```jsx
export default function WelcomePage() {
  const { scrollY } = useScroll();

  // این هوک مقادیر و تبدیل میکنه به اونی که میخایم - ارکمان اول هوک رو میدیم بعدش و ارکمان دوم میگیم مثلا به 200 پیکسل رسید عدد 0.5
  const opacityCity = useTransform(scrollY, [0, 200, 300, 500], [1, 0.5, 0.4, 0]);
  // که انیمیشن هم برای عکس بشه
  const yCity = useTransform(scrollY, [0, 200], [0, -100]);

  const opacityHero = useTransform(scrollY, [0, 300, 500], [1, 1, 0]);
  const yHero = useTransform(scrollY, [0, 200], [0, -200]);

  const scaleText = useTransform(scrollY, [0, 300], [1, 1.5]);
  const yText = useTransform(scrollY, [0, 200, 300, 500], [0, 50, 50, 300]);

  return (
    <>
      <header id="welcome-header">
        {/* بدیم برای تغییر کسنت */}
        <motion.div style={{ scale: scaleText, y: yText }} id="welcome-header-content">
          <h1>Ready for a challenge?</h1>
          <Link id="cta-link" to="/challenges">
            Get Started
          </Link>
        </motion.div>
        // حالا مقدار اوسیتین و تنظیم میکنم - این کامپوننت هم به توسط فریم موشن پشتیبانی میشه
        <motion.img style={{ opacity: opacityCity, y: yCity }} src={cityImg} alt="A city skyline touched by sunlight" id="city-image" />
        <motion.img style={{ y: yHero, opacity: opacityHero }} src={heroImg} alt="A superhero wearing a cape" id="hero-image" />
      </header>
    </>
  );
}
```

---

### variant_initial_animate_exit

initial = یعنی قبل از اینکه انیمیشن شروع بشه المان از این حالت میاد

animate = در واقع میگم که وقتی المنت در صفحه هست چجور باشه - چیزی مثل حالت وسط یعنی بعد ورود و قبل خروج

exit = حالت انیمیشن وقتی کامپوننت از DOM حذف میشه

variants = یک راه برای تعریف چندین حالت انیمیشن برای یک کامپوننت است

```jsx
return createPortal(
  <>
    <div className="backdrop" onClick={onClose} />

    <motion.dialog
      className="modal"
      open
      variants={{
        visible: { opacity: 1, scale: 1, transition: { type: 'spring', stiffness: 70 } },
        hidden: { opacity: 0, scale: 0 },
      }}
      initial="hidden"
      animate="visible"
      exit="hidden"
    >
      <h2>{title}</h2>
      {children}
    </motion.dialog>
  </>
);
```

این الان exit اش کار نمیکنه چون نمیدونه کی داره از دام حذف میشه و بخاطر این باید به کامپوننت پرنت بریم و به سری تغییرات انجام بدیم

برو فایل Header.jsx و کامپوننت AnimatePresence رو ببین که ساخته شده توسط کتابخونه است

---

### WhileDrag

```jsx
export default function SnapBackDrag() {
  return (
    <div>
      <motion.div
        // میتونی سطح حرکت هم مشخص کنی
        // drag="x"
        // drag="y"

        drag // فعال کردن درگ
        dragConstraints={{ left: -100, right: 100, top: -100, bottom: 100 }} // محدوده حرکت
        whileDrag={{
          scale: 0.95, // کوچک‌تر شدن حین درگ
          backgroundColor: '#ff6b6b', // تغییر رنگ
          boxShadow: '0px 10px 30px rgba(0,0,0,0.3)', // سایه عمیق‌تر
          cursor: 'grabbing', // تغییر نشانگر ماوس
        }}
        dragSnapToOrigin={true} // پس از رها کردن به نقطه اول برگرد
        transition={{
          type: 'spring', // حرکت فیری
          stiffness: 300, // سفتی فنر
          damping: 20, // میزان مقاومت
        }}
      />
    </div>
  );
}
```

---

### WhileTap

```jsx
import { motion } from 'framer-motion';

<motion.div whileTap={{ scale: 0.9 }} className="box">
  ایونت کلیک
</motion.div>;
```
