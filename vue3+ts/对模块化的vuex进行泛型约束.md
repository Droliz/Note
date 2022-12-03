## 目录结构

![](../markdown_img/Pasted%20image%2020221201160616.png)

## index.ts

```ts
import { createStore, Store, useStore as baseStore } from "vuex";
import { App } from "vue";
import { InjectionKey } from "@vue/runtime-core";
  
import { store as userLogin, LoginState } from "./modules/userLogin"; // 管理登录相关
  
// 所有模块类型
export interface RootState {
  userLogin: LoginState;
}
  
const key: InjectionKey<Store<RootState>> = Symbol();
  
const store: Store<RootState> = createStore({
  modules: { userLogin },
});
  
export const initStore = (app: App<Element>) => {  // 或者导出store在main.ts中use
  app.use(store, key);
};
  
export function useStore() {   // 使用时用这个 useStore
  return baseStore(key);
}
```


## 子模块

```ts
import { Module } from "vuex";
import { RootState } from "../index";
  
export interface LoginState {   // 子模块的泛型约束
  menus: Array<number>;
}
  
export const store: Module<LoginState, RootState> = {
  namespaced: true,
  state: (): LoginState => ({
    menus: [],
  }),
  mutations: {
    setMenus(state: LoginState, value: []) {
      state.menus = value;
    },
  },
};
```


## main.ts

```ts
import { initStore } from "./store";

initStore(app);
```


## 使用

```ts
import { useStore } from '@/store/index'
const store = useStore()
console.log(store.state.userLogin.menus)
```