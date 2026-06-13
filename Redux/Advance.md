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
