
# floor与parseInt区别

## floor

标准库的`Math.floor`可以将小数取整

取整的规则是按坐标轴向左取整，这也就意味着`-1.1`会被取整为`-2`


```js
console.log(Math.floor(-1.1))   // -2
```

## parseInt

`parseInt`也可以将小数取整的

取整规则是向`0`取整，这也意味着`-1.1`会被取整为`-1`

`parseInt`一般是用于字符串转数字（**转的过程中也会进行取整**）

```js
console.log(parseInt("-1.1"))   // -1
console.log(parseInt(-1.1))     // -1
```