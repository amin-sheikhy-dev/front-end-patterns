```js
const str = [
  'https://tasteoftheplace.com/kenyan-beef-curry/',
  'http://allrecipes.co.uk/recipe/29578/chicken-katsu-curry.aspx',
  'https://www.bbcgoodfood.com/recipes/11753/nutty-chicken-curry',
];

str.forEach((s) => console.log(s.split('/')[2]));

str.forEach((s) => console.log(new URL(s).hostname));
```

خروجی

tasteoftheplace.com

allrecipes.co.uk

www.bbcgoodfood.com
