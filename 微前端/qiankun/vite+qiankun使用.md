## 主应用接入qiankun

使用vite构建vue3+ts项目（亲测可以与qiankun使用，借助插件）

```sh
npm create vite@latest base
```

安装`qiankun`

```sh
# npm
npm install qiankun
# yarn
yarn add qiankun
```

注册微应用`main.ts`

```ts
import { createApp } from "vue";
import "./style.css";
import App from "./App.vue";
import router from "./router";
createApp(App).use(router).mount("#app");
  
// 乾坤主应用
import { registerMicroApps, start } from "qiankun";

// 微应用列表，可以有多个
const apps = [
	{
	name: "app-1", // app name registered
	entry: "//localhost:3011",
	container: "#sub-app",
	activeRule: "/app-1",
	},
	{
	name: "app-2", // app name registered
	entry: "//localhost:3012",
	container: "#sub-app",
	activeRule: "/app-2",
	}
];
  
registerMicroApps(apps, {
	beforeLoad: [async (app) => console.log("before load", app.name)],
	beforeMount: [async (app) => console.log("before mount", app.name)],
	afterMount: [async (app) => console.log("after mount", app.name)],
});
  
start();
```

当微应用信息注册完之后，一旦浏览器的 url 发生变化，便会自动触发 qiankun 的匹配逻辑。 所有 activeRule 规则匹配上的微应用就会被插入到指定的 container 中，同时依次调用微应用暴露出的生命周期钩子。

- registerMicroApps(apps, lifeCycles?)
  注册所有子应用，qiankun会根据activeRule去匹配对应的子应用并加载
  
- start(options?)
  启动 qiankun，可以进行预加载和沙箱设置


## vite创建的微应用

目前无法直接在vite中使用qiankun，但是借助插件可以实现，只是由于这样占用了路由，所以在路由上更改的更加繁琐

创建微应用

```sh
npm create vite@latest app-1
```

安装插件

```sh
# npm
npm i vite-plugin-qiankun 
# yarn
yarn add vite-plugin-qiankun
```

配置

`vite.config.js`

```rust
import { defineConfig } from "vite";
import vue from "@vitejs/plugin-vue";
import qiankun from "vite-plugin-qiankun";
  
// https://vitejs.dev/config/
export default defineConfig({
  base: "/app-1",
  plugins: [
    vue(),
    qiankun("app-1", {
      useDevMode: true
    })
  ],
  server: {
    port: 3011,
    cors: true,
    origin: 'http://localhost:3011'
  },
});
```

base配置一定要与主应用中注册的`activeRule`一致，且路由也需要（vite可以不写，但是路由必须设置base与主要用一致，其他的不用更改）

`router/index.ts`

```ts
import {
  createRouter,
  createWebHistory,
  type RouteRecordRaw,
} from "vue-router";
  
const routes: RouteRecordRaw[] = [
  {
    path: "/home",
    name: "Home",
    component: () => import("../views/Home.vue"),
  },
];
  
const router = createRouter({
  history: createWebHistory("/app-2"),
  routes,
});
  
export default router;
```

`main.ts`

```ts
import { createApp } from "vue";
import "./style.css";
import App from "./App.vue";
  
// createApp(App).mount("#app");
  
import {
  renderWithQiankun,
  qiankunWindow,
} from "vite-plugin-qiankun/dist/helper";
import { App as VueApp } from "vue";
let app: VueApp<Element>;
if (!qiankunWindow.__POWERED_BY_QIANKUN__) {
  createApp(App).mount("#app");
} else {
  renderWithQiankun({
    // 生命周期
    mount(props) {
      console.log("--app 01 mount");
      app = createApp(App);
      app.mount(
        (props.container
          ? props.container.querySelector("#app")
          : document.getElementById("app")) as Element
      );
    },
    // 仅在第一次启动时调用
    bootstrap() {
      console.log("--app 1 bootstrap");
    },
    update() {
      console.log("--app 1 update");
    },
    unmount() {
      console.log("--app 1 unmount");
      app?.unmount();
    },
  });
}
```