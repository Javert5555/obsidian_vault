Other people have answered that you should add a null-check, but Typescript also has a non-null assertion that you can use when you are _sure_ that the value is never null by adding the `!` operator to the end of your statement:

```javascript
const portalDiv = document.getElementById('your-element')!;
```