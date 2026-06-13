این الان با توجه به انتخاب کاربر true یا false ریترن میکنه

```jsx
function handleDelete() {
  const confirmed = window.confirm('are you sure you want to delete all items ?');

  if (confirmed) deleteItem([]);
}
```
