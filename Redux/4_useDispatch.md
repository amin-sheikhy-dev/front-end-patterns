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
