```js
import { createSlice } from '@reduxjs/toolkit';

// مرحله اول: ساخت اسلایس و دادن آبجکت ورودی
const accountSlice = createSlice({
  // مرحله دوم: مشخص کردن اسم اسلایس - برای گرفتن دیتا از استور استفاده میشه
  name: 'account',

  // مرحله سوم: مشخص کردن مقدار های اولیه استیت
  initialState: {
    balance: 0,
    loan: 0,
    loanPurpose: '',
  },

  // مرحله چهارم: ساخت فانکشن های ردیوسر و اکشن ها
  reducers: {
    deposit(state, action) {
      state.balance = state.balance + action.payload;
    },

    withdraw(state, action) {
      if (state.balance < action.payload) return;
      state.balance = state.balance - action.payload;
    },

    // توی خود کامپوننت یک آبجکت هستش action
    requestLoan(state, action) {
      if (state.loan > 0) return;
      state.loan = action.payload.amount;
      state.loanPurpose = action.payload.purpose;
      state.balance = state.balance + action.payload.amount;
    },

    payLoan(state) {
      state.balance = state.balance - state.loan;
      state.loan = 0;
      state.loanPurpose = '';
    },
  },
});

// مرحله پنجم: اکسپورت کردن اکشن ها - برای استفاده در کامپوننت ها - ربطی به استور نداره
export const { deposit, withdraw, requestLoan, payLoan } = accountSlice.actions;

// مرحله ششم: اکسپورت کردن اسلایس برای استفاده در استور
export default accountSlice.reducer;
```
