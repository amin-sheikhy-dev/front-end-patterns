### ThemeStore

```ts
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

interface ThemeStoreType {
  theme: string;
  toggleTheme: () => void;
}

export const useThemeStore = create<ThemeStoreType>()(
  persist(
    (set) => ({
      theme: 'dark', // ساخت مقدار اولیه برای استور - ولی چون در لوکال استوریج ذخیره میشه دفعات بعد از اونجا مقدار میگیره

      // ساخت تابع تاگل تم
      toggleTheme() {
        set((state) => ({ theme: state.theme === 'light' ? 'dark' : 'light' }));
      },
    }),
    { name: 'theme-storage' }
  )
);
```

---

### AuthStore

```ts
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

interface AuthStoreType {
  isLoggedIn: boolean;
  login: () => void;
  logout: () => void;
}

// این مربوط به تایپ اسکریپت هست نه هیچ چیز دیگه
// بدون تایپ اسکریپت اینجوریه export const useAuthStore = create(
export const useAuthStore = create<AuthStoreType>()(
  persist(
    (set) => ({
      // توجه شود که چون در لوکال استوریج ذخیره میشه پس بعد از اینکه یکبار از تابع لاگین استفاده شه و مقدار ترو شه دیگه فالس نمیشه
      // مگه اینکه با تابع لاگ اوت دوباره فالس شه یعنی در کل فقط با تابع میتونی مقدار آن را عوض کرد نه هیچ چیز دیگه
      isLoggedIn: false,

      login: () => set({ isLoggedIn: true }),

      logout: () => set({ isLoggedIn: false }),
    }),
    { name: 'auth-storage' } // کلید ذخیره‌سازی در لوکال استوریج
  )
);
```

---

### RoomStore

```ts
import { Room } from '@/lib/db';
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

interface Reservations {
  room: Room;
  date: string[];
}

interface RoomStore {
  reservations: Reservations[];
  addReservation: (reservedRoom: Reservations) => void;
  removeReservation: (roomId: string) => void;
  clearAll: () => void;
}

export const useRoomStore = create<RoomStore>()(
  persist(
    (set) => ({
      // state
      reservations: [],

      // actions
      addReservation: (reservedRoom: Reservations) => set((state) => ({ reservations: [...state.reservations, reservedRoom] })),

      removeReservation(roomId: string) {
        set((state) => ({ reservations: state.reservations.filter((r) => r.room.id !== roomId) }));
      },

      clearAll: () => set({ reservations: [] }),
    }),
    { name: 'room-store' }
  )
);
```

---

### TodoStore

```ts
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

interface Todo {
  id: string;
  title: string;
  completed: boolean;
}

interface TodoState {
  todos: Todo[];
  addTodo: (title: string) => void;
  toggleTodo: (id: string) => void;
  removeTodo: (id: string) => void;
}

export const useTodoStore = create<TodoState>()(
  persist(
    (set) => ({
      todos: [],

      addTodo(title: string) {
        set((state) => ({ todos: [...state.todos, { id: crypto.randomUUID(), title, completed: false }] }));
      },

      toggleTodo(id: string) {
        set((state) => ({ todos: state.todos.map((todo) => (todo.id === id ? { ...todo, completed: !todo.completed } : todo)) }));
      },

      removeTodo: (id: string) => set((state) => ({ todos: state.todos.filter((todo) => todo.id !== id) })),
    }),
    { name: 'todo-storage' }
  )
);
```

---

### MealStore

```ts
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

interface Meal {
  imageSrc: string;
  title: string;
  id: string;
  source: string;
  instructions: string;
}

interface FavoriteStore {
  favoriteMeals: Meal[];
  addFavoriteMeal: (newMeal: Meal) => void;
  removeFavoriteMeal: (mealId: string) => void;
  removeAllFavoriteMeal: () => void;
}

export const useFavoriteStore = create<FavoriteStore>()(
  persist(
    (set) => ({
      favoriteMeals: [],

      addFavoriteMeal(newMeal) {
        set((state) => {
          const exists = state.favoriteMeals.some((meal) => meal.id === newMeal.id);

          if (exists) return state;

          return { favoriteMeals: [...state.favoriteMeals, newMeal] };
        });
      },

      removeFavoriteMeal(mealId) {
        set((state) => ({ favoriteMeals: state.favoriteMeals.filter((m) => m.id !== mealId) }));
      },

      removeAllFavoriteMeal() {
        set({ favoriteMeals: [] });
      },
    }),
    { name: 'favorite-store' }
  )
);
```

---

### بهترین پترن ها برای استفاده

```ts
const todos = useTodoStore((state) => state.todos);
const addTodo = useTodoStore((state) => state.addTodo);

const { toggleTodo, removeTodo, addTodo } = useTodoStore();
```

بدترین و غیر بهینه ترین نوع استفاده

```ts
const store = useTodoStore();
```
