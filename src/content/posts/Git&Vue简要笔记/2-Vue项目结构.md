---
title: Vue项目结构
published: 2026-03-30
description: "Vue 3项目目录结构详解与技术栈说明"
tags: ["Vue", "Vite", "TypeScript"]
category: Git&Vue
draft: false
---


## 技术栈

- **框架**: Vue 3 (beta 版本)

- **构建工具**: Vite (beta 版本)

- **语言**: TypeScript 5.9

- **状态管理**: Pinia 3.0

- **路由**: Vue Router 5.0

- **代码规范**: ESLint + Oxlint

- **代码格式化**: Oxfmt

- **Node 版本要求**: ^20.19.0 || >=22.12.0


## 目录结构详解

### 根目录文件
#### 配置文件
- **`package.json`** - 项目依赖和脚本配置

  - 定义项目依赖、开发依赖

  - 配置 npm scripts（dev、build、lint、format 等）

- **`tsconfig.json`** - TypeScript 主配置文件

  - 使用项目引用（project references）管理配置

- **`tsconfig.app.json`** - 应用程序的 TypeScript 配置

- **`tsconfig.node.json`** - Node 环境的 TypeScript 配置（用于 vite.config.ts 等）

- **`vite.config.ts`** - Vite 构建工具配置

  - 配置 Vue 插件

  - 配置 Vue DevTools 插件

  - 设置路径别名 `@` 指向 `./src`

- **`eslint.config.ts`** - ESLint 代码检查配置

- **`.oxlintrc.json`** - Oxlint 配置（快速 linter）

- **`.oxfmtrc.json`** - Oxfmt 配置（代码格式化工具）

  

#### 其他文件

- **`README.md`** - 项目说明文档

  - IDE 和浏览器设置建议

  - 项目启动命令

- **`index.html`** - 应用入口 HTML 文件

  - Vite 项目的入口点

- **`env.d.ts`** - 环境类型声明文件

  - 声明 `.vue` 文件的类型定义

- **`.editorconfig`** - 编辑器配置

  - 统一不同编辑器的代码风格

- **`.gitignore`** - Git 忽略文件配置

- **`.gitattributes`** - Git 属性配置

  

### 主要目录
#### **`src/`** - 源代码目录
项目的核心源代码目录，包含所有业务逻辑和组件。
##### `src/main.ts`
- 应用程序的入口文件

- 初始化 Vue 应用

- 配置全局插件（Pinia、Vue Router 等）

- 挂载应用到 DOM
##### `src/App.vue`

- 根组件

- 应用程序的主组件

- 包含整个应用的基础布局

##### `src/assets/`

- 静态资源目录

- 存放图片、字体、样式等资源文件

- 会被构建工具处理

##### `src/components/`

- 可复用组件目录

- 存放 Vue 组件（.vue 文件）

- 按功能或模块组织组件

  

##### `src/views/`

- 页面级组件目录

- 存放路由对应的页面组件

- 每个路由通常对应一个 view

  

##### `src/router/`

- 路由配置目录

  - `index.ts` - 路由配置文件

- 定义应用的路由规则

- 配置路由守卫和中间件

  

##### `src/stores/`

- Pinia 状态管理目录

- 存放 Pinia store 文件

- 管理应用的全局状态

  

#### **`public/`** - 公共静态资源目录

- 存放不经过构建处理的静态文件

- 文件会被直接复制到构建输出目录

- 通过绝对路径引用

  - `favicon.ico` - 网站图标

  

#### **`node_modules/`** - 依赖包目录
- 存放 npm 安装的所有依赖包

- 由 `npm install` 自动生成

- 不应提交到版本控制

  

#### **`.vscode/`** - VS Code 配置目录
- VS Code 编辑器的项目配置

- 可能包含：

  - 推荐的扩展插件

  - 工作区设置

  - 调试配置


#### **`.git/`** - Git 版本控制目录

- Git 仓库的元数据目录

- 存储版本历史和配置

- 不应手动修改

  

## NPM Scripts 说明
### 开发命令

  

```bash

npm run dev          # 启动开发服务器（热重载）

npm run preview      # 预览生产构建

```

  

### 构建命令

```bash

npm run build        # 生产构建（包含类型检查）

npm run build-only   # 仅构建，不进行类型检查

npm run type-check   # TypeScript 类型检查

```

  

### 代码质量

```bash

npm run lint         # 运行所有 linter（oxlint + eslint）

npm run lint:oxlint  # 运行 oxlint（快速检查）

npm run lint:eslint  # 运行 eslint

npm run format       # 使用 oxfmt 格式化代码

```

  

## 学习路径建议

  

1. 从 `src/main.ts` 入口文件开始

2. 理解 `src/App.vue` 根组件

3. 学习路由配置 `src/router/index.ts`

4. 实践组件开发（components 和 views）

5. 掌握状态管理（stores）

6. 熟悉构建工具配置（vite.config.ts）
