## call函数

### API说明

就是函数对象的call方法

```js
call(function, object, ...args)
```

call函数会执行`function`，然后使`function`中的`this`为`object`，最后将后面的n个参数传给function

**使用 `call()` 方法，可以编写能够在不同对象上使用的方法**

### 实现说明

```js
function call(fn, obj, ...args) {

    // 全局对象
    obj = obj || globalThis   // es11 globalThis 表示全局对象
    
    // 改变函数的 this  => 不好实现，但是可以给 obj 添加 fn 方法
    obj.temp = fn
    
    // 调用 temp 方法
    let res = obj.temp(...args)
    
    // 删除 temp 方法  （复原）
    delete obj.temp
    
    return res
}
```

### 使用

```js
function add(a, b) {
    console.log(this)
    return a + b + this.c
}
  
let obj = {
    c: 4
}
  
let k = call(add, obj, 1, 2)   // 如果 obj 为null或undefined那么就取全局对象
console.log(k);
```


## apply函数

### API说明

```js
apply(function, object, args)
```

功能上与call相同，不同的是参数以数组的形式传递

### 实现说明

```js
function apply(fn, obj, args) {

    // 全局对象
    obj = obj || globalThis   // es11 globalThis 表示全局对象
    
    // 改变函数的 this  => 不好实现，但是可以给 obj 添加 fn 方法
    obj.temp = fn
    
    // 调用 temp 方法
    let res = obj.temp(...args)
    
    // 删除 temp 方法  （复原）
    delete obj.temp
    
    return res
}
```

### 使用

```js
function add(a, b) {
    console.log(this)
    return a + b + this.c
}
  
let obj = {
    c: 4
}
  
let k = apply(add, obj, [1, 2])   // 如果 obj 为null或undefined那么就取全局对象
console.log(k);
```

## bind函数

### API说明

```js
apply(function, object, args)
```

等同于函数对象的 bind 方法，不同于call与apply的是会返回一个新的函数，在新函数内部调用

### 实现说明

```js
// 方法 1
function bind(fn, obj, ...args) {

    return function() {   // 相当于在内部执行 call
        // 全局对象
        obj = obj || globalThis   // es11 globalThis 表示全局对象

        // 改变函数的 this  => 不好实现，但是可以给 obj 添加 fn 方法
        obj.temp = fn

        // 调用 temp 方法
        let res = obj.temp(...args)

        // 删除 temp 方法  （复原）
        delete obj.temp
        return res
    }
}

// 方法 2
function bind(fn, obj, ...args) {
	return function(...args2) {
		return call(fn, obj, ...args, ...args2)
	}
}
```

### 使用

```js
function add(a, b) {
    console.log(this)
    return a + b + this.c
}
  
let obj = {
    c: 4
}

// 方法 1
let k = apply(add, obj, 1, 2)   // 如果 obj 为null或undefined那么就取全局对象
k()

// 方法 2
k = apply(add, obj)
k(1, 2)
```

