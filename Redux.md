### 1_createSlice

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

---

### 2_StoreConfig

در این کد configureStore: یک تابع از Redux Toolkit است که به طور خودکار تنظیمات پیش‌فرض (مثل Redux DevTools و Middlewareها) را اضافه می‌کند.

و reducer: یک آبجکت است که نام هر اسلایس و ردیوسر مربوط به آن را مشخص می‌کند.

و بعد مسیر فایل‌ها: فرض بر این است که فایل‌های accountSlice.js و customerSlice.js در پوشه slices وجود دارند.

استفاده: بعد از ایجاد این فایل، باید در main.js یا index.js، استور را به کامپوننت اصلی (App) با Provider تزریق کنید:

```js
import { configureStore } from '@reduxjs/toolkit';
import accountReducer from './slices/accountSlice';
import customerReducer from './slices/customerSlice';

// کانفیگ و اکسپورت کردن استور و دادن آبجکت ورودی
export const store = configureStore({
  reducer: {
    customer: customerReducer,
    account: accountReducer,
  },
});
```

---

### 3_ProvideStore

```jsx
import App from './App';
import { Provider } from 'react-redux';
import ReactDOM from 'react-dom/client';
import { store } from './redux/store';

ReactDOM.createRoot(document.getElementById('root')).render(
  // پرووایدر کردن ریداکس برای کل اپلیکیشن و قرار دادن استور داخل آن
  <Provider store={store}>
    <App />
  </Provider>
);
```

---

### 4_useDispatch

```jsx
const [fullName, setFullName] = useState('');
const [nationalId, setNationalId] = useState('');

// با این هوک از فانکشن دیسپیچ استفاده میکنی
const dispatch = useDispatch();

function handleSubmit(e) {
  e.preventDefault();

  if (!fullName || !nationalId) return;

  // استفاده از دیسپیچ
  dispatch(createCustomer({ fullName, nationalId }));
}
```

```jsx
const dispatch = useDispatch();

function handleDeposit() {
  if (!depositAmount) return;

  dispatch(deposit(depositAmount));
  setDepositAmount('');
}

function handleWithdrawal() {
  if (!withdrawalAmount) return;

  dispatch(withdraw(withdrawalAmount));
  setWithdrawalAmount('');
}

function handleRequestLoan() {
  if (!loanAmount || !loanPurpose) return;

  dispatch(requestLoan({ amount: Number(loanAmount), purpose: loanPurpose }));
  setLoanAmount('');
  setLoanPurpose('');
}

function handlePayLoan() {
  dispatch(payLoan());
}
```

---

### 4_useSelector

با این هوک میشه دیتارو از استور بکشی بیرون

```jsx
const account = useSelector((store) => store.account);

// store.account = { balance: 0 , Loan: 0 , LoanPurpose: '' }
```

---

### Advance example

```js
import { createSlice } from '@reduxjs/toolkit';

const cartSlice = createSlice({
  name: 'cart',
  initialState: {
    items: [],
    totalQuantity: 0,
    totalAmount: 0,
  },

  reducers: {
    addItemToCart(state, { payload }) {
      const existingItem = state.items.find((item) => item.id === payload.id);
      state.totalQuantity++;

      if (!existingItem) {
        state.items.push({ id: payload.id, price: payload.price, quantity: 1, totalPrice: payload.price, name: payload.title });
      } else {
        existingItem.quantity++;
        existingItem.totalPrice = existingItem.totalPrice + payload.price;
      }
    },

    removeItemFromCart(state, { payload }) {
      const existingItem = state.items.find((item) => item.id === payload.id);
      state.totalQuantity--;

      if (existingItem.quantity === 1) {
        state.items = state.items.filter((item) => item.id !== payload.id);
      } else {
        existingItem.quantity--;
        existingItem.totalPrice = existingItem.totalPrice - existingItem.price;
      }
    },
  },
});

export const { addItemToCart, removeItemFromCart } = cartSlice.actions;
export default cartSlice.reducer;
```
