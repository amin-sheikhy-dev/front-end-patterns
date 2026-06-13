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
