```jsx
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';

const queryClient = new QueryClient(); // ایجاد یک کوریتی کلاینت

export default function App() {
  return (
    // حالا کوریتی کلاینت رو داخل پرووایدر میزاریم
    <QueryClientProvider client={queryClient}>
      <RouterProvider router={router} />
    </QueryClientProvider>
  );
}
```
