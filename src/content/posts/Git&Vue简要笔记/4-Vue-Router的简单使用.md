---
title: Vue-Router的简单使用
published: 2026-03-31
description: "Vue Router基础用法、动态加载、动态路由与基于文件的路由"
tags: ["Vue", "Vue Router", "前端路由"]
category: Git&Vue
draft: false
---

## 基本用法
![](Pasted%20image%2020260330222838.png)
![](Pasted%20image%2020260330222922.png)
完成代码书写后，再去router文件夹的index.ts下配置路由：
![](Pasted%20image%2020260330222946.png)
一切看起来都很好，但是这里有一个性能陷阱，我们稍后了解。

## 动态加载dynamic loading
现在的页面有一个隐藏的性能问题，我们打开网页，按下F12进入浏览器的开发者模式，进入`network`去抓包网络请求：
![](Pasted%20image%2020260331125322.png)

发现即使只是去首页，浏览器也会请求所有的组件，这绝对是一个性能陷阱，同时也会损害用户的网络流量。现代JavaScript引入了动态加载，我们只需要在`src/router/index.ts`中，完成以下的修改就行：
![](Pasted%20image%2020260331125641.png)
![](Pasted%20image%2020260331125706.png)
然后就再次进入网页发现，每一个页面只请求自己的组件了：
![](Pasted%20image%2020260331125849.png)


## 动态路由dynamic router
在`src`目录中的`router`目录下的`index.ts`中，在routes数组下添加如下代码即可创建动态路由，动态路由的声明语法是:`/xxx/:xx`。
![](Pasted%20image%2020260331124348.png)
这样我们就能声明一个动态的路由，然后可以在对应的组件中获取并使用，这样对于某些具体的商品或者项目页面，我们就可以只创建一个组件，然后复用：
![](Pasted%20image%2020260331124525.png)
## 简单的错误处理，基于正则表达式匹配
![](Pasted%20image%2020260331140306.png)

## **基于文件的路由(Vue-Router v5内置)**
以前这个功能需要安装插件：[unplugin-vue-router](https://uvr.esm.is/)。但是在2026年初，Vue-Router v5.x版本已经将其内置，具体可以参考[PR](https://github.com/posva/unplugin-vue-router/issues/787)。

有了基于文件的路由，我们就不用手动的去添加路由了，而是直接创建对应的文件夹和文件就可以创建路由。

### 1. 修改 `vite.config.ts`

将路由插件直接从 `vue-router/vite` 导入，并确保它在 `vue()` 插件之前执行。
```typescript
import { fileURLToPath, URL } from "node:url";
import { defineConfig } from "vite";
import vue from "@vitejs/plugin-vue";
import vueDevTools from "vite-plugin-vue-devtools";
// 核心变更：使用官方内置的 vite 插件
import VueRouter from "vue-router/vite";
// https://vite.dev/config/
export default defineConfig({
  plugins: [
    VueRouter({
      // 默认监听 src/pages 目录
      routesFolder: "src/pages",
    }),
    // ⚠️ Vue must be placed after VueRouter()
    vue(),
    vueDevTools(),
  ],

  resolve: {

    alias: {

      "@": fileURLToPath(new URL("./src", import.meta.url)),

    },

  },

});
```

### 2. 修改 `tsconfig.json`
添加此插件后，启动开发服务器（通常为 npm run dev），即可在 typed-router.d.ts 文件中生成首个类型版本。该文件需添加至 tsconfig.json 配置中，同时需配置 `"moduleResolution": "Bundler"`。配置效果如下：
```json
{
  "include": [
    // other files...
    "./typed-router.d.ts"
  ],
  "compilerOptions": {
    // ...
    "moduleResolution": "Bundler",
    // ...
  }
}
```
![](Pasted%20image%2020260331143246.png)
### Volar 插件

为了在单文件组件中获得最佳的 TypeScript 使用体验，请将以下 Volar 插件添加至你的 tsconfig.json（或` tsconfig.app.json`）文件中：
```json
{
  "vueCompilerOptions": {
    "plugins": [
      "vue-router/volar/sfc-route-blocks",
      "vue-router/volar/sfc-typed-router",
    ],
  },
}
```
- vue-router/volar/sfc-route-blocks—— 在单文件组件（SFC）中启用`<route>`代码块，用于定义页面级路由元数据

- vue-router/volar/sfc-typed-router—— 使`useRoute()`根据当前页面组件返回带类型的路由对象，从而让`route.params`具备正确的类型定义

## 迁移现有项目

将你的页面组件移至 `src/pages` 目录，并按相应规则重命名。以下是迁移示例。给定如下路由配置：
```typescript
import { createRouter, createWebHistory } from "vue-router";
import { routes, handleHotUpdate } from "vue-router/auto-routes";

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes,
});

// This will update routes at runtime without reloading the page
if (import.meta.hot) {
  handleHotUpdate(router);
}

export default router;
```
![](Pasted%20image%2020260331144033.png)
然后将`views`文件夹重命名为`pages`文件夹：
- 将 src/pages/Home.vue 重命名为 src/pages/index.vue
- 将 src/pages/User.vue 重命名为 src/pages/users/[id].vue
- 将 src/pages/About.vue 重命名为 src/pages/about.vue

### 路由文件夹结构​

默认情况下，该插件会检查 `src/pages` 目录下的所有 `.vue` 文件，并根据文件名生成对应的路由结构。这样一来，你在为应用添加路由时无需再手动维护路由数组，只需将新的 `.vue` 组件添加到路由文件夹中，剩余工作交由插件处理即可！

我们来看一个简单的示例：
```text
src/pages/
├── index.vue
├── about.vue
└── users/
    ├── index.vue
    └── [id].vue
```
这将生成以下路由：

- /：渲染 `index.vue` 组件
- /about：渲染 `about.vue` 组件
- /users：渲染 `users/index.vue` 组件
- /users/:id：渲染 `users/[id].vue` 组件，其中 id 会成为路由参数。

#### 索引路由
任何 `index.vue`（必须全小写）文件都会生成一个空路径路由（类似于 index.html 文件）：
- `src/pages/index.vue`：生成 / 路由
- `src/pages/users/index.vue`：生成 /users 路由

#### 嵌套路由
嵌套路由可通过在同名文件夹旁创建 .vue 文件来自动定义。若同时创建 `src/pages/users/index.vue`和 `src/pages/users.vue` 组件，`src/pages/users/index.vue` 将会在 `src/pages/users.vue` 的 `<RouterView>` 标签内渲染。

换言之，给定如下文件夹结构：
```text
src/pages/
├── users/
│   └── index.vue
└── users.vue
```
总之，替换完的文件夹长这样：
![](Pasted%20image%2020260331145741.png)
`App.vue`还是只需要在`template`中渲染一个`<RouterView>`就行：
![](Pasted%20image%2020260331145822.png)
然后首页组件直接写在`src/pages/index.vue`中就可以：
![](Pasted%20image%2020260331145847.png)
### ESLint报错解决
现在vscode会报错，这个需要在ESLint中解决：
![](Pasted%20image%2020260331150625.png)
在`eslint.config.ts`中添加以下的设置就可以避免报错。注意不要直接在第一个对象里面设置`rules`，因为会被下面的`pluginVue`覆盖。必须在最后新加一个对象，设置rules。
![](Pasted%20image%2020260331151412.png)
