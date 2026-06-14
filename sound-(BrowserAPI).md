```js
const sound = new Audio('path/to/sound.mp3');

// پخش صدا
sound.play();

// توقف صدا - اگر صدا هنوز درحال پخش باشد
sound.pause();

// تنظیم صدا (0 تا 1)
sound.volume = 0.5; // 50 درصد صدا

// پخش تکراری
sound.loop = true;

// زمان فعلی پخش (ثانیه)
// الان به ابتدا ریست کردیم
sound.currentTime = 0;

// سرعت پخش (1 = عادی / 2 = دو برابر)
sound.playbackRate = 1.5;

// آیا صدا در حال پخش است؟
sound.paused; // true یا false

// مدت زمان کل (ثانیه) - بعد از بارگذاری مشخص میشود
sound.duration;
```

و Audio در واقع ویژگی ای هستش که Browser در اختیار ما قرار میده

```jsx
import clickSound from './ClickSound.m4a';

// Sound API
const playSound = useCallback(
  function playSound() {
    if (!allowSound) return;

    const sound = new Audio(clickSound); // بهینه که صدارو خارج از کامپوننت پخشش

    sound.play();
  },
  [allowSound]
);

// روش جالبی که میشه باهاش استیتی که مقدارش وابسته به بقیه استیت ها هستش رو افکت انجام بدی
useEffect(() => {
  setDuration((number * sets * speed) / 60 + (sets - 1) * durationBreak);

  playSound();
}, [number, sets, speed, durationBreak, playSound]);
```
