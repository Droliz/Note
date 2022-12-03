## 全局API转移

vue3将vue2的`Vue`实例身上的全局`api`转移到`app`实例上

|vue2|vue3|
|:--:|:--:|
|`Vue.config.xxx`|`app.config.xxx`|
|`Vue.config.productionTip`|**移除**|
|`Vue.component`|`app.component`|
|`Vue.directive`|`app.directive`|
|`Vue.mixin`|`app.mixin`|
|`Vue.use`|`app.use`|
|`Vue.prototype`|`app.config.globalProperties`|


## 其他改变

- `data` 选项必须声明为一个函数

- 过度类名更改

```js
// vue2
.v-enter,
.v-leave-to {}

.v-leave,
.v-enter-to {}


// vue3
.v-enter-from,
.v-leave-to {}

.v-leave-from,
.v-enter-to {}
```

- 移除`keyCode`作为`v-on`修饰符，同时也不支持`config.keyCodes`

- 移除`v-on.native`修饰符，像`click`这样的事件默认为原生事件，除非在子组件中使用`emits`声明

- 移除过滤器（filter）

