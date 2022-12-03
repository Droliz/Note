# setup语法糖

## 基本使用

要启用该语法，需要在 `<script>` 代码块上添加 `setup` ：

```vue
<script setup>
// code
</script>
```

里面的代码会被编译成组件 `setup()` 函数的内容。这意味着与普通的 `<script>` 只在组件被首次引入的时候执行一次不同，`<script setup>` 中的代码会在**每次组件实例被创建的时候执行**。

当使用 `<script setup>` 的时候，任何在 `<script setup>` 声明的顶层的绑定 (包括变量，函数声明，以及 import 导入的内容) 都能在模板中直接使用


## 命名空间组件

在一个文件夹下创建多个组件，然后在此文件夹下创建`index.js`文件用于向外暴露组件

在需要使用的地方导入`index.js`，通过`name.组件`的形式使用

```js
// components/layout/index.js
import Header from "./Header.vue";
import Main from "./Main.vue";
import Aside from "./Aside.vue";
  
export {
    Header,
    Main,
    Aside
}
```

使用

```vue
<template>
  <layout.Header></layout.Header>
  <layout.Main></layout.Main>
  <layout.Aside></layout.Aside>
</template>
  
<script setup>
import * as layout from './components/layout'
</script>
```

## 自定义指令

```vue
<script setup lang="ts">
const vMyDirective = {
  beforeMount: (el: HTMLElement) => {
    // 在元素上做些操作
    el.innerText = "a"
  }
}
</script>
```



## setup语法糖新增的方法

-   `defineProps`：子组件接收父组件中传来的props
-   `defineEmits`：子组件调用父组件中的方法
-   `defineExpose`：子组件暴露属性，可以在父组件中拿到


