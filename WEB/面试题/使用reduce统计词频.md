## 使用reduce统计词频

```js
const str = 'fsakdhasjkdjajwdahkscx'  

const res = str.split("").reduce((a, b) => (a[b]++ || (a[b] = 1), a), {})  
  
console.log(res)
```

