این کامپوننت وقتی موس روی div قرار بگیره متن "Hovered" رو نشون میده و وقتی موس خارج بشه متن "Hover Me" نمایش داده میشه.

```jsx
function Hover() {
  const [isHovered, setIsHovered] = useState(false);

  return (
    <div onMouseEnter={() => setIsHovered(true)} onMouseLeave={() => setIsHovered(false)}>
      {isHovered ? 'Hovered' : 'Hover Me'}
    </div>
  );
}
```
