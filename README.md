# fe-base-vue-project

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Type-Check, Compile and Minify for Production

```sh
npm run build
```

### Lint with [ESLint](https://eslint.org/)

```sh
npm run lint
```

## Project Structure

```text
fe-base-vue-project/
├── .vscode
│   └── extensions.json
├── public
│   └── favicon.ico
├── src
│   ├── api/                        #存储api请求封装，后端请求
│   ├── assets/                     #存储静态资源文件，比如图像、样式表等
│   ├── components/                 #存储可复用组件库
│   ├── constants/                  #存储全局常量
│   ├── directives/                 #存储自定义指令
│   ├── hooks/                      #存储自定义hook
│   ├── icons/                      #存储图标字体资源
│   ├── router/                     #存储vue-router路由配置
│   ├── service/                    #实际业务逻辑处理
│   ├── stores/                     #存储状态管理配置
│   ├── styles/                     #存储通用css
│   ├── type-utils/                 #通用ts类型辅助函数
│   ├── types/                      #通用ts类型
│   │   ├── api/                    #调用api请求时需要的ts类型
│   │   ├── bo/                     #业务对象，后端返回的实际ts类型
│   │   └── vo/                     #显示层对象，前端业务实际需要的ts类型
│   ├── utils/                      #存储工具函数
│   ├── views/                      #存储页面级别的Vue组件
│   ├── App.vue                     #根Vue组件，通常包含整个应用的布局
│   └── main.ts                     #入口文件，启动和编译Vue应用
├── env.d.ts                        #类型声明文件
├── .env                            #存储通用环境变量
├── .env.dev                        #存储dev环境变量
├── .env.qa                         #存储qa环境变量
├── .env.prod                       #存储prod环境变量
├── eslint.config.ts                # ESLint 的代码检查配置文件
├── tsconfig.json                   #TS 总配置入口，引用下面两个子配置
├── tsconfig.app.json               #浏览器环境代码的 TS 检查规则（管 src/）
├── tsconfig.node.json              #Node 环境代码的 TS 检查规则（管 vite.config.ts 等）
└── vite.config.ts                  #Vite 构建工具配置文件
```

# 命名规范

## 文件夹

- 统一 kebab-case（小写 + 短横线）
- 禁止大写字母和下划线
- 示例：account-setting、user-profile

## vue单文件组件

### 页面级组件：

- 以 kebab-case （小写 + 短横线）创建文件夹，内部创建 index.vue
- 示例 : views/account-setting/index.vue

### 页面专属组件：

按复杂度分两种形态：

1. **轻量组件**：逻辑简单，直接以 PascalCase 命名单个 .vue 文件。
   示例：views/account-setting/components/AccountForm.vue

2. **重量组件**：为组件创建 kebab-case 文件夹，内部创建 index.vue
   及配套文件，结构与页面结构一致。

```
views/account-setting/components/account-form/
  ├── index.vue
  ├── type.ts
  ├── constant.ts
  ├── util.ts
  └── components/    # 该重量组件自己的子组件（可选）
```

**判断标准**：只需要内联类型 / 复用页面级文件 → 轻量形态；需要独立的 type.ts / util.ts / constant.ts → 重量形态。轻量组件变复杂后，随时可升级为文件夹形态。

## ts/js文件

- 统一 camelCase（小驼峰）
- 各目录命名规则：

| 目录     | 命名规则      | 示例              |
| -------- | ------------- | ----------------- |
| api/     | xxxApi.ts     | accountApi.ts     |
| service/ | xxxService.ts | studentService.ts |
| utils/   | xxxUtil.ts    | timeUtil.ts       |
| hooks/   | xxxHook.ts    | authHook.ts       |

- hooks/ 目录文件内部导出的组合式函数统一以 use 开头，如 useAuth()

二、view下，复杂页面的结构

```
views/account-setting/
  ├── index.vue         # 页面主组件
  ├── constant.ts       # 页面专属常量
  ├── util.ts           # 页面专属工具函数
  ├── type.ts           # 页面专属类型
  ├── index.scss        # 页面专属样式（类名加页面前缀，避免污染）
  └── components/       # 页面专属组件（PascalCase）轻量组件。 重量组件按照新建文件夹方式
      └── AccountForm.vue
```

#### 配套文件说明

- 只有页面主组件自己用到的类型/工具/常量，才放在页面根目录的配套文件里
- 组件和页面共用的类型/工具，放在页面根目录，两边都从这里导入
- 多个页面共用的，提升到 src/types/、src/utils/、src/constants/
