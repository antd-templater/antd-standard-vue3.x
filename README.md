<h1>Vue3.x 开发规范</h1>

<br/>

<h2># 项目指南</h2>
<details>
<summary>项目依赖</summary>

```bash
  # 本地初始化 (生成 package.json)
  pnpm init

  # 本地安装 package.json 依赖包
  pnpm install

  # 全局安装 某个依赖包
  pnpm add -g [package]
  pnpm add -g [package]@[tag]
  pnpm add -g [package]@[version]

  # 本地安装 仅编译环境所需 依赖包
  pnpm add -D [package]
  pnpm add -D [package]@[tag]
  pnpm add -D [package]@[version]

  # 本地安装 编译及生产环境所需 依赖包
  pnpm add [package]
  pnpm add [package]@[tag]
  pnpm add [package]@[version]

  # 本地升级 某个依赖包
  pnpm upgrade [package]
  pnpm upgrade [package]@[tag]
  pnpm upgrade [package]@[version]

  # 本地移除 某个依赖包
  pnpm remove [package]

  # 本地检查 依赖包情况
  # <red>    : Major Update backward-incompatible updates --- (不建议更新)
  # <yellow> : Minor Update backward-compatible features ---- (可以更新)
  # <green>  : Patch Update backward-compatible bug fixes --- (建议更新)
  pnpm outdated

  # 本地更新 一键按需升级
  # Press <space> to select ----------------------- (空格切换选中)
  # Press <a> to toggle all ----------------------- (所有依赖选中)
  # Press <i> to invert selection ----------------- (所有依赖反选)
  # Press <Enter> install selected dependencies --- (所选依赖安装)
  pnpm up -i --latest
```

</details>

<details>
<summary>项目结构</summary>

```bash
  ├── .husky                               # 由 npx husky init 生成 (Git Hook 工具)
  │   ├── commit-msg                       # 规范 git message 提交和格式检查 (使用 commitlint)
  │   ├── pre-commit                       # 执行 git 提交时, 对提交源码进行检查 (使用 lint-staged)
  │
  ├── .vscode                              # VSCode 编辑器配置
  │   ├── extensions.json                  # VSCode 推荐安装的插件
  │   ├── launch.json                      # VSCode 本地开发调试配置
  │   ├── settings.json                    # VSCode 项目开发风格/格式化配置 (ESLint、Prettier等)
  │
  ├── dist                                 # 由 pnpm build 构建的本地工程
  ├── node_modules                         # 由 pnpm install 创建的本地依赖包
  │
  ├── public                               # 静态资源 (不参与构建)
  │   ├── favicon.ico                      # 网站 favicon.ico 图标
  │   ├── logo.png                         # 网站 logo 图片, 通常在 html 模版中引用
  │   ├── msw.js                           # mock service worker (mock response)
  │
  ├── src                                  # 源代码
  │   ├── api                              # 定义与后端交互的接口
  │   │   ├── user.js                      # 示例: 规范1 - 文件名 根据后台接口 (如 /user/add 定义user)
  │   │   ├── auth.js                      # 示例: 规范2 - 文件名【 camelCase 】命名
  │   │
  │   ├── assets                           # 静态资源
  │   │   ├── logo                         # 示例: 规范1 - 分组名 根据内容进行文件夹
  │   │   │   ├── logo_light.png           # 示例: 规范2 - 文件名【 kebab_case 】命名
  │   │
  │   ├── components                       # 公共组件
  │   │   ├── BaseSearchQuery              # 示例: 建议1 - 文件名 以类型化单词开头 (如 Base)
  │   │   ├── BaseIconSelect               # 示例: 规范1 - 文件名 应倾向于完整单词而不是缩写
  │   │   ├── BaseSvgIcon                  # 示例: 规范2 - 文件名【 PascalCase 】命名
  │   │
  │   ├── configure                        # 默认配置 (建议【 default + 类型 】+【 camelCase 】命名)
  │   │   ├── defaultRouter.ts             # 预设定义 常规路由 (根路由、外部路由、异常路由、静态路由)
  │   │   ├── defaultSettings.ts           # 预设定义 前端整体 风格/主题/布局 等配置选项，在 store/app.ts 中用于初始值
  │   │   ├── presetDirective.ts           # 预设定义 Vue指令 操作权限 (示例 v-action)
  │   │   ├── presetEnvironment.ts         # 预设定义 环境变量 (源自于 .env.xxxx 配置)
  │   │   ├── presetThemeColors.ts         # 预设定义 主题色库 (例 极客蓝、拂晓蓝、薄暮)
  │   │
  │   ├── declare                          # 全局 TS 类型定义 (建议【 PascalCase 】命名)
  │   │   ├── Axios.d.ts                   # 预设定义 Axios 相关类型 (AxiosSorter ...)
  │   │   ├── Global.d.ts                  # 预设定义 JSX API / Window API 类型
  │   │   ├── ImportMeta.d.ts              # 预设定义 Vite Environment 类型
  │   │
  │   ├── layout                           # 布局组件库
  │   │   ├── components                   # 储存仅布局组件依赖的组件
  │   │   │   ├── LayoutAvatar             # 基础布局组件 - 头像组件 Avatar
  │   │   │   ├── LayoutBreadcrumb         # 基础布局组件 - 面包屑组件 Breadcrumb
  │   │   │   ├── LayoutLogo               # 基础布局组件 - 图标栏组件 Logo
  │   │   │   ├── LayoutMultiTab           # 基础布局组件 - 多标签组件 MultiTab
  │   │   │   ├── LayoutSettingDrawer      # 基础布局组件 - 配置选项组件 SettingDrawer
  │   │   │                                #
  │   │   ├── BasicLayout.tsx              # 基础布局组件 (含 Layout.Sider、Layout.Header、Layout.Content + RouterView、Avatar ...)
  │   │   ├── BlankLayout.tsx              # 空白布局组件 (仅有 RouterView)
  │   │   ├── PageFrame.tsx                # 页面 Frame 布局组件 (适用 iframe 外部资源访问)
  │   │   ├── PageView.tsx                 # 页面/路由布局组件 (加载 Layout.Content/RouterView 对应的 Vue 组件, 指定了容器样式)
  │   │   ├── RouteView.tsx                # 页面/路由布局组件 (加载 Layout.Content/RouterView 对应的 Vue 组件, 无相关样式)
  │   │   ├── UserLayout.tsx               # 用户路由布局组件 (仅有 RouterView + 定制容器样式，一般用于用户登录页的路由)
  │   │
  │   ├── mock                             # 模拟数据交互 - (规范与api保持一致)
  │   │   ├── user                         # 示例: 用户接口
  │   │   │   ├── addUserInfo.ts           # 示例: 用户接口 - 新增
  │   │   │   ├── getUserInfoList.ts       # 示例: 用户接口 - 查询
  │   │   │                                #
  │   │   ├── setup.ts                     # 定义 setupWorker (配合 public/msw.js 实现 mock response)
  │   │
  │   ├── model                            # 数据模型 TS 类型定义 (建议【 PascalCase 】命名)
  │   │   ├── Tree.d.ts                    # 定义 Tree 树形结构模型
  │   │   ├── User.d.ts                    # 定义 User 用户信息模型
  │   │
  │   ├── plugin                           # 定义引用第三方插件
  │   │   ├── dayjs.ts                     # 导入引用 dayjs 日期插件
  │   │   ├── pinia.ts                     # 导入引用 pinia 状态管理库 (集成了 pinia-plugin-persist)
  │   │   ├── vue-ls.ts                    # 导入引用 vue-ls 储存管理
  │   │
  │   ├── router                           # 动态路由处理
  │   │   ├── generate-routes.ts           # 解析/转换/生成 动态路由 (Route)
  │   │   ├── generate-typing.d.ts         # 解析/转换/生成 类型定义
  │   │
  │   ├── store                            # pinia 状态储存 (建议【 camelCase 】命名)
  │   │   ├── app.ts                       # 定义存储 前端整体 风格/主题/布局 等配置
  │   │   ├── router.ts                    # 定义存储 Vue路由 (动态路由、静态路由、解析生成动态路由)
  │   │   ├── tag.ts                       # 定义存储 多标签页 (记录标签、缓存标签、移除标签)
  │   │   ├── user.ts                      # 定义存储 用户信息 (用户登录/退出、用户信息)
  │   │
  │   ├── styles                           # 样式定义/覆盖 (建议【 kebab_case 】命名)
  │   │   ├── customize.less               # 自定义样式 (例 flex-auto、flex-none、text-ellipsis)
  │   │   ├── normalize.less               # 重置默认样式 (关于 html、body、h1 ~ h6、p、-webkit-scrollbar)
  │   │   ├── nprogress.less               # 重写覆盖 nprogress 进度条样式
  │   │   ├── override.less                # 覆盖 ant design vue 组件样式 (目前仅覆盖 ant-drawer 部分样式)
  │   │
  │   ├── utils                            # 工具类方法 (建议【 camelCase 】命名)
  │   │   ├── common.ts                    # 定义 通用工具类 (数值四舍五入、输出指定格式的日期、取出节点文本、封装请求参数)
  │   │   ├── request.ts                   # 定义 Axios 实例 (Request拦截器、Response拦截器、预设 baseURL / timeout)
  │   │   ├── router.ts                    # 定义 layout 中 FrameView 布局组件中 提取 route link API
  │   │
  │   ├── views                            # 视图路由组件库 (建议【 PascalCase 】.vue 命名)
  │   │   ├── auth                         # 模块 - 认证管理
  │   │   │   ├── Login.vue                # 组件 - 用户登录
  │   │   │
  │   │   ├── error                        # 模块 - 异常管理
  │   │   │   ├── PageError403.vue         # 组件 - 异常 403
  │   │   │   ├── PageError404.vue         # 组件 - 异常 404
  │   │   │   ├── PageError500.vue         # 组件 - 异常 500
  │   │   │
  │   │   ├── system                       # 模块 - 系统管理
  │   │   │   ├── components               #
  │   │   │   │   ├── OrganizeManage       # 子组件库 - OrganizeManage
  │   │   │   │   ├── ResourceManage       # 子组件库 - ResourceManage
  │   │   │   │   ├── RoleManage           # 子组件库 - RoleManage
  │   │   │   │   ├── UserManage           # 子组件库 - UserManage
  │   │   │   │                            #
  │   │   │   ├── OrganizeManage.vue       # 组织管理 (路由 /system/OrganizeManage)
  │   │   │   ├── ResourceManage.vue       # 资源管理 (路由 /system/ResourceManage)
  │   │   │   ├── RoleManage.vue           # 角色管理 (路由 /system/RoleManage)
  │   │   │   ├── UserManage.vue           # 用户管理 (路由 /system/UserManage)
  │   │
  │   ├── App.vue                          # 顶层路由组件 (处理 全局 Theme/Token/Size)
  │   ├── main.less                        # 样式入口文件
  │   ├── main.ts                          # 主入口文件
  │   ├── permission.ts                    # 路由权限拦截器
  │   ├── router.constant.ts               # 配置静态路由
  │   ├── router.dynamic.ts                # 配置动态路由 (借助 vite 的 import.meta.glob 导入 src/views 目录下路由)
  │   ├── router.ts                        # 初始化 Router 实例
  │
  ├── test                                 # 测试脚本
  │   ├── utils.test.ts                    # 示例 (建议 xxx.test.ts 命名)
  │
  ├── .cz-message.cjs                      # 指定 cz-message-helper 配置选项 (using by git cz)
  ├── .editorconfig                        # 指定项目的编码规范
  ├── .env                                 # 默认基础环境配置
  ├── .env.development                     # 本地开发环境配置, 会覆盖 .env 文件同名属性配置
  ├── .env.production                      # 正式运行环境配置, 会覆盖 .env 文件同名属性配置
  ├── .env.test                            # 测试运行环境配置, 会覆盖 .env 文件同名属性配置
  ├── .eslintignore                        # 指定 eslint 哪些文件不需要校验
  ├── .eslintrc-auto-import.json           # 是由 unplugin-auto-import/vite 插件自动生成 (在 eslint extends 中配置)
  ├── .eslintrc.cjs                        # 指定 eslint 校验的规则配置
  ├── .gitattributes                       # 指定 git 使用的文件和路径的属性
  ├── .gitignore                           # 指定 git 哪些文件不需要添加到版本管理中
  ├── .lintstagedrc.js                     # 指定 lint-staged 配置选项
  ├── .npmignore                           # 指定 npm publish 哪些文件被忽略 (比 .gitignore 优先级高)
  ├── .npmrc                               # 指定 npm 运行时的配置选项
  ├── .prettierignore                      # 指定 prettier 哪些文件不需要校验
  ├── .prettierrc.js                       # 指定 prettier 格式的规则配置
  ├── .release-it.json                     # 指定 release-it 配置选项
  ├── .stylelintrc.js                      # 指定 stylelint 配置选项
  ├── auto-imports.d.ts                    # 是由 unplugin-auto-import/vite 插件自动生成
  ├── commitlint.config.js                 # 指定 @commitlint/cli、@commitlint/config-conventional 的配置选项
  ├── components.d.ts                      # 是由 unplugin-vue-components/vite 插件自动生成
  ├── index.html                           # 编译构建所需的 html 模版文件
  ├── LICENSE                              # 前端项目许可文件
  ├── package.json                         # 前端项目配置文件
  ├── pnpm-lock.yaml                       # pnpm 安装依赖包版本锁定文件
  ├── README.md                            # 前端项目介绍文件
  ├── tsconfig.json                        # typescript 配置文件
  ├── tsconfig.node.json                   # typescript 子配置文件 (在 tsconfig.json 的 references 选项中进行引用)
  ├── vite.config.ts                       # Vite 配置文件 (dev server / run build)
  ├── volar.config.js                      # Volar 配置文件 (配合 volar-service-prettyhtml 插件)
```

</details>

<br/>

<h2># 注释规范</h2>
<details>
<summary>文档注释 -- 常用于文件的摘要描述</summary>

```html
<!--
  * 404 页面
  * lin pengteng
  * 2024-04-03
-->
<template>
  <a-result title="404页面">
    <template #extra>
      <a-button>返回首页</a-button>
    </template>
  </a-result>
</template>
```

```ts
import T from 'ant-design-vue/es/table/Table'

/**
 * 表格组件
 * lin pengteng
 * 2024-04-03
 */
export default defineComponent({
  name: 'BaseTable',
  props: {
    ...T.props,
  },
  setup(props) {
    // ...
  },
})
```

```css
/**
 * 规范标签默认样式
 * lin pengteng
 * 2024-04-03
 */
html,
body,
#app,
#root {
  height: 100%;
}
```

</details>

<details>
<summary>方法注释 -- 常用于函数的摘要描述</summary>

```ts
import moment from 'moment'

/**
 * 根据格式转换 日期
 * lin pengteng
 * 2024-04-03
 */
export const takeTimeToDate = (date: Date | string | number, format?: string) => {
  if (date) {
    try {
      return moment(date, format)
    } catch {}
  }
  return null
}
```

</details>

<details>
<summary>多行注释 -- 常用于字段、逻辑、注解等多行描述</summary>

```ts
/* 
  这是一个临时储存区
  记录用户操作过的用户ID
 */
const CACHES = []
```

</details>

<details>
<summary>单行注释 -- 常用于字段、逻辑、注解等单行描述</summary>

```ts
import * as VueTypes from 'vue-types'

export default defineComponent({
  name: 'CustomButton',
  props: {
    // 按钮图标
    icon: VueTypes.string().def('filter'),
    // 按钮类型
    type: VueTypes.string().def('default'),
  },
  setup(props) {
    // ...
  },
})
```

</details>

<br/>

<h2># Vue 规范</h2>

<details>
<summary>组件顶级元素的顺序 <sup style="color: #f34d4d;">(必要)</sup></summary>

- `template`、`script` 和 `style` 顺序必须一致，之间空一行隔开

  ```html
  <template>
    <section class="container">
      <AButton>自定义</AButton>
    </section>
  </template>

  <script setup lang="ts">
    defineOptions({
      name: 'CustomButton',
    })
  </script>

  <style lang="less" scoped>
    .container {
      width: 100%;
      height: auto;
    }
  </style>
  ```

</details>

<details>
<summary>组件名由多个单词组成 <sup style="color: #f34d4d;">(必要)</sup></summary>

- 这样做可以避免跟现有以及未来 HTML 元素相冲突，因为所有的 HTML 元素名称都是单个单词的

  ```typescript
  // Bad
  export default defineComponent({
    name: 'Todo',
    // ...
  })

  // Good
  export default defineComponent({
    name: 'TodoComponent',
    // ...
  })
  ```

</details>

<details>
<summary>组件名应为完整单词而非缩写 <sup style="color: #f34d4d;">(必要)</sup></summary>

- 编辑器中自动补全已经让书写长命名的代价非常之低，而其带来的明确性却是非常宝贵的

  ```bash
    # Bad
    components/
      |- SdSettings.vue
      |- UProOpts.vue

    # Good
    components/
      |- StudentDashboardSettings.vue
      |- UserProfileOptions.vue
  ```

</details>

<details>
<summary>组件名中单词顺序按语境排序 <sup style="color: #f34d4d;">(必要)</sup></summary>

- 组件名应该以高级别的(通常是一般化描述的)单词开头，以描述性的修饰词结尾，组件间排序关系一目了然

  ```bash
    # Bad
    components/
      |- ClearSearchButton.vue
      |- RunSearchButton.vue

    # Good
    components/
      |- SearchButtonClear.vue
      |- SearchButtonRun.vue
  ```

</details>

<details>
<summary>高耦合子组件名以父组件名为前缀 <sup style="color: #f34d4d;">(必要)</sup></summary>

- 如果一个组件只在某个父组件的场景下有意义，这层关系应该体现在其名字或目录上

  ```bash
    # Bad
    components/
      |- TodoList.vue
      |- TodoItem.vue
      |- TodoButton.vue

    # Good
    components/
      |- TodoList.vue
      |- TodoListItem.vue
      |- TodoListItemButton.vue

    # Good
    components/
      |- TodoList/
      |- |- index.vue
      |- |- Item.vue
      |- |- ItemButton.vue
  ```

</details>

<details>
<summary>单文件组件文件名应 PascalCase 命名 <sup style="color: #f34d4d;">(必要)</sup></summary>

- 单文件组件的文件名应该始终是单词大写开头 PascalCase

  ```bash
    # Bad
    components/
      |- mycomponent1.vue
      |- myComponent2.vue
      |- Mycomponent3.vue
      |- my-component4.vue

    # Good
    components/
      |- MyComponent1.vue
      |- MyComponent2.vue
      |- MyComponent3.vue
      |- MyComponent4.vue
  ```

</details>

<details>
<summary>.vue 单文件模板应该只包含简单的表达式 <sup style="color: #f34d4d;">(必要)</sup></summary>

- 组件模板应该只包含简单的表达式，复杂的表达式则应该重构为计算属性或方法

  ```html
  <!-- Bad -->
  <template>
    <div class="container">{{ fullName.split(' ').map(function (word) { return word[0].toUpperCase() + word.slice(1) }).join(' ') }}</div>
  </template>

  <script setup lang="ts">
    import { ref } from 'vue'

    const fullName = ref('todo component')
  </script>

  <!-- Good -->
  <template>
    <div class="container">{{ computedFullName }}</div>
  </template>

  <script setup lang="ts">
    import { ref, computed } from 'vue'

    const fullName = ref('todo component')

    const computedFullName = computed(() => {
      const names = fullName.value.split(' ')
      return names.map(word => word[0].toUpperCase() + word.slice(1)).join(' ')
    })
  </script>
  ```

</details>

<details>
<summary>.vue 单文件的自闭合组件应省略闭合标签 <sup style="color: #f34d4d;">(必要)</sup></summary>

- 自闭合组件表示它们不仅没有内容，没有了额外的闭合标签，代码也更简洁

  ```html
  <!-- Bad -->
  <template>
    <MyComponent></MyComponent>
  </template>

  <!-- Good -->
  <template>
    <MyComponent />
  </template>
  ```

</details>

<details>
<summary>.html 文件的自闭合组件不能省略闭合标签 <sup style="color: #f34d4d;">(必要)</sup></summary>

- HTML 并不支持自闭合的自定义元素——只有官方的“空”元素

  ```html
  <!-- Bad -->
  <body>
    <div />
  </body>

  <!-- Good -->
  <body>
    <div></div>
  </body>
  ```

</details>

<details>
<summary>.vue 单文件的组件以 PascalCase 方式引用  <sup style="color: #f34d4d;">(必要)</sup></summary>

- 采用 PascalCase 风格，具有较高可读性，同时避免跟现有的以及未来的 HTML 元素相冲突

  ```html
  <!-- Bad -->
  <template>
    <new-component />
  </template>

  <script>
    import NewComponent from 'NewComponent'
  </script>

  <!-- Good -->
  <template>
    <NewComponent />
  </template>

  <script>
    import NewComponent from 'NewComponent'
  </script>
  ```

</details>

<details>
<summary>.jsx/.tsx 文件的组件以 PascalCase 方式使用  <sup style="color: #f34d4d;">(必要)</sup></summary>

- 使得代码的读者更容易分辨 Vue 组件和 HTML 元素

  ```typescript
    // Bad
    <script lang="ts">
    import NewComponent from 'NewComponent'

    export default defineComponent({
      name: 'TodoComponent',
      render () {
        return () => <new-component>
      }
    })
    </script>

    // Good
    <script lang="ts">
    import NewComponent from 'NewComponent'

    export default defineComponent({
      name: 'TodoComponent',
      render () {
        return () => <NewComponent>
      }
    }
    </script>
  ```

</details>

<details>
<summary>组件多个 attribute 应该分多行撰写 <sup style="color: #f34d4d;">(必要)</sup></summary>

- 组件多个 attribute 元素每个一行，更具可读性

  ```typescript
    // Bad
    <script lang="ts">
    import NewComponent from 'NewComponent'

    export default defineComponent({
      name: 'TodoComponent',
      render () {
        return () => <NewComponent type="button" color="#f34d4d">
      }
    })
    </script>

    // Good
    <script lang="ts">
    import NewComponent from 'NewComponent'

    export default defineComponent({
      name: 'TodoComponent',
      render () {
        return () => (
          <NewComponent
            type="button"
            color="#f34d4d"
          >
        )
      }
    }
    </script>
  ```

</details>

<details>
<summary>组件的 Prop 定义尽量详细 <sup style="color: #f34d4d;">(必要)</sup></summary>

- prop 定义尽量详细，至少需要指定类型，如果提供不正确的 prop，Vue 会帮助你捕获错误

  ```typescript
  // Bad
  export default defineComponent({
    props: ['status'],
  })

  // Good
  import { PropType } from 'vue'

  export default defineComponent({
    props: {
      status: {
        type: String as PropType<string>,
        default: '',
      },
    },
  })

  // Good
  import * as VueTypes from 'vue-types'

  export default defineComponent({
    props: {
      status: VueTypes.string().def(''),
    },
  })

  // Good
  <script setup lang="ts">
  export interface Props {
    status?: string
  }

  const props = defineProps<Props>()
  </script>
  ```

</details>

<details>
<summary>组件的 Prop 名应为驼峰式 <sup style="color: #f34d4d;">(必要)</sup></summary>

- 在声明 prop 及 模板和 JSX 使用时，其命名应使用 camelCase

  ```html
  <!-- Bad -->
  <template>
    <welcome-message :greeting-text="greetingText" />
  </template>

  <script>
    export default defineComponent({
      name: 'WelcomeMessage',
      props: {
        'greeting-text': VueTypes.string().def(''),
      },
    })
  </script>

  <!-- Good -->
  <template>
    <WelcomeMessage :greetingText="greetingText" />
  </template>

  <script lang="ts">
    export default defineComponent({
      name: 'WelcomeMessage',
      props: {
        greetingText: VueTypes.string().def(''),
      },
    })
  </script>

  <!-- Good -->
  <template>
    <WelcomeMessage :greetingText="greetingText" />
  </template>

  <script setup lang="ts">
    export interface Props {
      greetingText?: string
    }
    defineProps<Props>()
  </script>
  ```

</details>

<details>
<summary>避免将 v-if 和 v-for 用在一起 <sup style="color: #f34d4d;">(必要)</sup></summary>

- 为了不渲染本应该隐藏的列表, 则可将 v-if 移动至其父容器元素上

  ```html
  <!-- Bad -->
  <ul>
    <li v-for="user in users" v-if="shouldShowUsers" :key="user.id">{{ user.name }}</li>
  </ul>

  <!-- Good -->
  <ul v-if="shouldShowUsers">
    <li v-for="user in users" :key="user.id">{{ user.name }}</li>
  </ul>
  ```

- 根据某属性过滤列表中的项目, 则可替换为一个计算属性, 让其返回过滤后的列表

  ```html
  <!-- Bad -->
  <ul>
    <li v-for="user in users" v-if="user.isActive" :key="user.id">{{ user.name }}</li>
  </ul>

  <!-- Good -->
  <ul>
    <li v-for="user in activeUsers" :key="user.id">{{ user.name }}</li>
  </ul>
  ```

</details>

<details>
<summary>必须为 v-for 设置键值 key <sup style="color: #f34d4d;">(必要)</sup></summary>

- 在组件上总是必须用 key 配合 v-for，以便维护内部组件及其子树的状态

  ```html
  <!-- Bad -->
  <ul>
    <li v-for="todo in todos">{{ todo.text }}</li>
  </ul>

  <!-- Good -->
  <ul>
    <li v-for="todo in todos" :key="todo.id">{{ todo.text }}</li>
  </ul>
  ```

</details>

<details>
<summary>应为组件样式设置作用域 <sup style="color: #f34d4d;">(必要)</sup></summary>

- 基于有作用域的样式可以避免与其他组件的样式发生冲突

  ```html
  <!-- Bad -->
  <template>
    <button class="btn btn-close">X</button>
  </template>

  <style>
    .btn-close {
      background-color: red;
    }
  </style>

  <!-- Good -->
  <template>
    <button class="btn btn-close">X</button>
  </template>

  <style scoped>
    .btn-close {
      background-color: red;
    }
  </style>
  ```

</details>

<details>
<summary>组件/实例各指令采用简写  <sup style="color: #42b983;">(推荐)</sup></summary>

- 用 : 表示 v-bind: , @ 表示 v-on: , # 表示 v-slot:

  ```html
  <!-- Bad -->
  <template>
    <div class="container">
      <template v-slot:header>
        <h1>A page title</h1>
      </template>
      <input v-bind:value="newValue" v-on:input="onInput" />
    </div>
  </template>

  <!-- Good -->
  <template>
    <div class="container">
      <template #header>
        <h1>A page title</h1>
      </template>
      <input :value="newValue" @input="onInput" />
    </div>
  </template>
  ```

</details>

<details>
<summary>组件/实例的选项顺序 <sup style="color: #42b983;">(推荐)</sup></summary>

- 组件/实例的选项应该有统一的顺序

  ```bash
    # 副作用 (触发组件外的影响)
    el

    # 全局感知 (要求组件以外的知识)
    name
    parent

    # 组件类型 (更改组件的类型)
    functional

    # 模板修改器 (改变模板的编译方式)
    delimiters
    comments

    # 模板依赖 (模板内使用的资源)
    components
    directives
    filters

    # 组合 (向选项里合并 property)
    extends
    mixins

    # 接口 (组件的接口)
    inheritAttrs
    model
    props/propsData

    # 本地状态 (本地的响应式 property)
    data
    computed

    # 监听事件 (通过响应式事件触发的回调)
    watch

    # 生命周期钩子 (按照它们被调用的顺序)
    beforeCreate
    created
    beforeMount
    mounted
    beforeUpdate
    updated
    activated
    deactivated
    beforeDestroy
    destroyed

    # 非响应式的 property
    methods

    # 渲染 (组件输出的声明式描述)
    template/render
    renderError
  ```

</details>

<details>
<summary>组件/实例的属性顺序 <sup style="color: #42b983;">(推荐)</sup></summary>

- 组件/实例的属性应该有统一的顺序

  ```bash
    # 引用 (提供组件的引用)
    is
    id
    ref

    # 双向绑定 (把绑定和事件结合起来)
    v-model

    # 列表渲染 (创建多个变化的相同元素)
    v-for

    # 条件渲染 (元素是否渲染/显示)
    v-if
    v-else-if
    v-else
    v-show
    v-cloak

    # 其他属性 (attribute 或 prop)
    key
    ...

    # 渲染方式 (改变元素的渲染方式)
    v-pre
    v-once
    v-html
    v-text

    # 事件 (组件事件监听器)
    v-on
  ```

</details>

<br/>

<h2># Git 规范</h2>
<details>
<summary>Git 分支设计 <sup style="color: #42b983;">(推荐)</sup></summary>

- 基于如下四种常用系统开发环境，而设计的 `Git` 五种分支类型

  - PRO 环境：用于生产环境
  - DEV 环境：用于开发者调试使用
  - FAT 环境：功能验收测试环境，用于测试环境下的测试人员测试使用
  - UAT 环境：生产预发布环境，用于生产环境下的测试人员测试使用

    <br/>

    | 分支    | 名称         | 命名规范 | 运行环境 |
    | :------ | :----------- | :------: | :------: |
    | master  | 主分支       |    /     |   PRO    |
    | release | 预上线分支   |    /     |   UAT    |
    | develop | 测试分支     |    /     |   FAT    |
    | feature | 需求开发分支 | feat-xxx |   DEV    |
    | hotfix  | 紧急修复分支 | fix-xxx  |   DEV    |

    <br/>

  ```bash
    # master 分支
    a. master 为主分支，用于部署到正式环境（PRO）
    b. 一般由 release 分支合并，任何情况下不允许直接在 master 分支上修改代码

    # release 分支
    a. release 为预上线分支，用于部署到预上线环境（UAT）始终保持与 master 分支一致
    b. 一般由 develop 或 hotfix 分支合并，不建议直接在 release 分支上直接修改代码
    c. 如果在 release 分支测试出问题，需要回归验证 develop 分支看否存在此问题

    # develop 分支
    a. develop 为测试分支，用于部署到测试环境（FAT），始终保持最新完成以及 bug 修复后的代码
    b. 可根据需求大小程度确定是由 feature 分支合并，还是直接在上面开发
    c. 一定是满足测试的代码才能往上面合并或提交。

    # feature 分支
    a. feature 为需求开发分支，命名规则为【 feat- 】开头，一旦该需求上线，分支本地预留 3-7 天后将其删除

    # hotfix 分支
    a. hotfix 为紧急修复分支，命名规则为【 fix- 】开头
    b. 当线上出现紧急问题需要马上修复时，需要基于 release 或 master 分支创建 hotfix 分支
    c. 修复完成后，再合并到 release 或 develop 分支，一旦修复上线，分支本地预留 1-3 天后将其删除
  ```

</details>

<details>
<summary>Git 版本号规范 <sup style="color: #42b983;">(推荐)</sup></summary>

- 版本号 Tag 采用三段式，v 版本.里程碑.序号，如：v1.0.0
  ```bash
    修改第1位 - 架构升级或架构重大调整
    修改第2位 - 新功能上线或者模块大的调整
    修改第3位 - bug修复上线、需求完善等调整
  ```

</details>

<details>
<summary>Git Commit 规范介绍 <sup style="color: #f34d4d;">(重要)</sup></summary>

- 目前社区流行的 commit 规范（来自于 Angular 团队的 commit 规范）

  ```bash
    # Commit Message 的三个部分：Header，Body 和 Footer, 注意两两之前空行间隔
    <type>(<scope>): <subject>
    <BLANK LINE>
    <body>
    <BLANK LINE>
    <footer>

    # Commit Message 之 Header 部分
    type（必需）--- 用于说明 commit 的类别
      a. init: 初始化
      b. feat: 新增feature
      c. fix: 修复bug
      d. docs: 仅仅修改了文档，如readme.md
      e. style: 仅仅是对格式进行修改，如逗号、缩进、空格等。不改变代码逻辑
      f. refactor: 代码重构，没有新增功能或修复bug
      g. perf: 优化相关，如提升性能、用户体验等
      h. test: 测试用例，包括单元测试、集成测试
      i. chore: 改变构建流程、或者增加依赖库、工具等
      j. revert: 版本回滚
      k. merge：代码合并
      l. sync：同步分支

    scope（可选）--- 用于说明 commit 影响范围，可以通过 src 名下文件夹定义，例如
      a. all or *
      b. api
      c. components
      d. utils
      e. views
      f. ...

    subject（必需）--- commit 内容的简短描述，不超过70个字符


    # Commit Message 之 Body 部分（可选）
    a. 对本次 commit 修改内容的具体描述, 可以分为多行
    b. 描述为什么修改, 做了什么样的修改, 以及开发的思路等等


    # Commit Message 之 footer 部分（可选，仅处理 不兼容 或 关闭 Issue使用）
    a. 处理当前代码与上个版本不兼容, 以 BREAKING CHANGE: 开头进行详细描述
    b. 当前 commit 关闭 issue，如 Closes #123, #245, #992
  ```

- 基于社区流行的 commit Message 示范

  ```bash
    # Commit Message - Header + Body
    init: Vue3.x 开发规范首次提交

    a. 包含了项目指南、注释规范、Vue 规范、Git规范
    b. 目前支持了 Vue3.x, 兼容 Vue2.x


    # Commit Message - 仅 Header
    docs(README.md): Vue3.x 开发规范完善 VSCode 开发等
  ```

</details>

<details>
<summary>Git Commit 规范校验 <sup style="color: #42b983;">(推荐)</sup></summary>

- 安装依赖

  ```bash
    pnpm add husky lint-staged @commitlint/cli @commitlint/config-conventional -D
  ```

- 初始化 husky (会在项目根目录下生成 .husky)

  ```bash
    npx husky init
  ```

- 在 .husky 名下 新增 commit-msg、pre-commit 两个 Git Hook

  ```bash
    # commit-msg 内容如下:
    # npx --no-install commitlint --edit $1

    # pre-commit 内容如下:
    # npx lint-staged
  ```

- 指定 commitlint 配置选项 (commitlint.config.js)

  ```js
  module.exports = {
    extends: ['@commitlint/config-conventional'],
    rules: {
      'type-enum': [2, 'always', ['fix', 'feat', 'begin', 'docs', 'style', 'refactor', 'chore', 'perf', 'test', 'merge', 'revert', 'wip']],
      'type-case': [0],
      'scope-case': [0],
      'subject-case': [0],
      'header-case': [0],
      'body-case': [0],
      'type-empty': [2, 'never'],
      'scope-empty': [0],
      'subject-empty': [2, 'never'],
      'body-empty': [0],
      'subject-full-stop': [0],
      'header-full-stop': [0],
      'body-full-stop': [0],
      'header-max-length': [2, 'always', 72],
      'body-leading-blank': [2, 'always'],
      'footer-leading-blank': [2, 'always'],
    },
  }
  ```

- 指定 lint-staged 配置选项 (.lintstagedrc.js)

  ```js
  module.exports = {
    'src/**/*.{js,jsx,ts,tsx,vue}': ['eslint --fix'],
  }
  ```

</details>

<details>
<summary>Git Commit 辅助工具 <sup style="color: #42b983;">(推荐)</sup></summary>

- 安装依赖

  ```bash
    # 全局安装 commitizen
    pnpm add commitizen -g

    # 本地安装 cz-message-helper
    pnpm add cz-message-helper -D
  ```

- 在 package.json 指定 commitizen、cz-message-helper 配置

  ```json
  {
    "name": "@antd-templater/template-3.x",
    "description": "后台管理系统模版 - 基于 Vue3.x Ant Design Vue 组件库",

    "config": {
      "cz-message-helper": {
        "config": ".cz-message.cjs"
      },
      "commitizen": {
        "path": "node_modules/cz-message-helper"
      }
    }
  }
  ```

- 创建 cz-message-helper 配置文件 .cz-message.cjs

  ```js
  /**
   * Commit message helper for commitizen
   * https://github.com/linpengteng/cz-message-helper
   */
  module.exports = {
    language: 'cn', // 支持 en | cn
  }
  ```

- 如何使用 cz-message-helper (using by git cz)

  <p align="center">
    <img 
      style="width: 100%; margin: 0 auto;" 
      src="https://linpengteng.github.io/resource/cz-message-helper/command.v1.png" 
      alt="cz-message-helper"
    >
  </p>

</details>

<br/>

<h2># VSCode 推荐</h2>
<details>
<summary>常用插件推荐</summary>

- 基于功能性分类: Git 分支管理、代码智能提示、校验优化代码

  ```bash
    # Git分支管理
    name:        GitLens — Git supercharged
    author:      GitKraken
    description: 增强内置的 Git 功能, 一目了然地可视化代码作者身份, 无缝导航和探索 Git 存储库等等


    # 代码智能提示
    name:        Vue 3 Snippets
    author:      hollowtree
    description: Vue2.x 和 Vue3.x 代码片段智能提示


    # 校验优化代码
    name:        ESLint
    author:      Dirk Baeumer
    description: 将 ESLint JavaScript 集成到 VSCode 中
    attention:   需要 pnpm install 相关依赖

    name:        Prettier - Code formatter
    author:      Prettier
    description: 使用 prettier 格式化代码
    attention:   需要 pnpm install 相关依赖

    name:        Vue - Official
    author:      Vue (vuejs.org)
    description: 官方为Vue构建的语言支持扩展

  ```

</details>

<details>
<summary>项目开发配置</summary>

- 项目根目录下建立 .vscode/settings.json 文件，统一开发配置
  ```json
  {
    "[css]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[less]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[scss]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[stylus]": {
      "editor.defaultFormatter": "thisismanta.stylus-supremacy"
    },
    "[html]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[javascript]": {
      "editor.defaultFormatter": "dbaeumer.vscode-eslint"
    },
    "[typescript]": {
      "editor.defaultFormatter": "dbaeumer.vscode-eslint"
    },
    "[jsonc]": {
      "editor.defaultFormatter": "vscode.json-language-features"
    },
    "[json]": {
      "editor.defaultFormatter": "vscode.json-language-features"
    },
    "[vue]": {
      "editor.defaultFormatter": "dbaeumer.vscode-eslint"
    },
    "editor.codeActionsOnSave": {
      "source.fixAll": "explicit"
    },
    "editor.tabSize": 2,
    "editor.formatOnSave": true,
    "editor.formatOnPaste": true,
    "editor.detectIndentation": false,
    "editor.renderControlCharacters": true,
    "eslint.format.enable": true,
    "eslint.probe": ["javascript", "javascriptreact", "typescriptreact", "typescript", "html", "wxml", "vue"],
    "prettier.semi": false,
    "prettier.useTabs": false,
    "prettier.tabWidth": 2,
    "prettier.printWidth": 200,
    "prettier.singleQuote": true,
    "prettier.bracketSpacing": true,
    "prettier.bracketSameLine": false,
    "prettier.jsxSingleQuote": false,
    "prettier.vueIndentScriptAndStyle": false,
    "prettier.htmlWhitespaceSensitivity": "ignore",
    "prettier.quoteProps": "consistent",
    "prettier.arrowParens": "avoid",
    "prettier.trailingComma": "es5",
    "volar.completion.preferredAttrNameCase": "camel",
    "volar.completion.preferredTagNameCase": "kebab",
    "volar.codeLens.references": false,
    "volar.icon.splitEditors": false,
    "stylusSupremacy.insertColons": true,
    "stylusSupremacy.insertBraces": false,
    "stylusSupremacy.insertSemicolons": false,
    "stylusSupremacy.insertNewLineAroundImports": false,
    "stylusSupremacy.insertNewLineAroundBlocks": false
  }
  ```

</details>

<details>
<summary>项目脚本指令</summary>

- 命令行 Prettier 一键格式化，需 [.prettierignore](https://github.com/antd-templater/antd-template-vue3.x/blob/main/.prettierignore)、[.prettierrc](https://github.com/antd-templater/antd-template-vue3.x/blob/main/.prettierrc) 配置

  ```bash
    npx prettier --write --loglevel warn "src/**/*.vue"
  ```

- 命令行 ESlint 一键校验并格式化，需 [.eslintignore](https://github.com/antd-templater/antd-template-vue3.x/blob/main/.eslintignore)、[.eslintrc.cjs](https://github.com/antd-templater/antd-template-vue3.x/blob/main/.eslintrc.cjs) 配置
  ```bash
    npx eslint --fix --quiet src --ext .vue,.tsx,.jsx,.ts,.js
  ```

</details>
