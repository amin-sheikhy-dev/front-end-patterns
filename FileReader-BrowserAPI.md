```tsx
'use client';

import { useRef, useState } from 'react';
import Image from 'next/image';

export default function ImagePicker({ label, name }) {
  const [image, setImage] = useState(null);
  const imageInputRef = useRef(null);

  function handleImageChange(e) {
    // اول فایل رو از اینپوت بگیر
    const file = e.target.files[0];

    if (!file) return;

    // بعد فایل ریدر بساز
    const reader = new FileReader();

    // بعد اینکه فایل ریدر دستش رسید دستش توی این تابع میگی چیکار کنه
    reader.onload = () => {
      // بعد اینکه فایل ریدر دستش رسید نتیجه رو ذخیره میکنی داخل استیت
      setImage(reader.result);
    };

    // اینجا همونجاس که فایلو میدیم بهش
    reader.readAsDataURL(file);

    // میشه اینارو بهش داد
    // reader.readAsText(file) خروجی متن میده
    // reader.readAsDataURL(file) خروجی عکس میده
    // reader.readAsArrayBuffer(file) خروجی باینری میده
  }

  return (
    <div>
      <label htmlFor={name}>{label}</label>

      <div>
        <div>
          {!image && <p>No image picked yet.</p>}
          {image && <Image src={image} alt="Image preview" fill />}
        </div>

        <input type="file" id={name} name={name} onChange={handleImageChange} accept="image/png, image/jpg" ref={imageInputRef} />

        {image && (
          <button type="button" onClick={() => setImage(null)}>
            X
          </button>
        )}

        <button type="button" onClick={() => imageInputRef.current.click()}>
          Pick an Image
        </button>
      </div>
    </div>
  );
}
```

برای استخراج عکس ها است name={name}

برای اینه که چه فایل هایی قبول که accept="image/png, image/jpg"

htmlFor={name} ، id={name} برای اینه که اینپوت و لیبل رو به هم ربط بدیم
