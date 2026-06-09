---
name: frontend-standards-merged
description: 面向本仓库的合并版 Vue 3 + Vite + TypeScript 前端工程规范技能，整合分层、命名、注释、组件、TypeScript、API、Store、样式、评审与交付标准，适合作为单文件团队规范。
---

# Frontend Standards Merged

## 适用场景

凡是处理本仓库以下目录中的前端代码，都必须使用本技能：

- `src/views/**`
- `src/components/**`
- `src/modules/**`
- `src/api/**`
- `src/store/**`
- `src/stores/**`
- `src/router/**`
- `src/utils/**`
- `src/types/**`
- `src/styles/**`
- `src/model/**`
- `src/configs/**`
- `src/constants/**`
- `src/directives/**`
- `src/layout/**`
- `src/assets/icons/**`
- `mock/**`

本仓库默认技术栈如下：

- `Vue 3`
- `Vite`
- `TypeScript`
- `Element Plus`
- `Vue Router 4`
- `Pinia 2`
- `Vuex 4`（历史类型声明或旧项目兼容可存在，新增状态管理默认不使用 Vuex）
- `SCSS`
- `Axios`
- `Dayjs`
- `unplugin-auto-import`（仅自动引入 `vue` 与 `vue-router` 常用 API）
- `vite-plugin-mock`
- `vite-plugin-svg-icons`
- `vite-plugin-vue-setup-extend`

## 规范优先级

1. 用户显式要求
2. 本技能规则
3. 仓库历史实现

执行原则：

- 新增代码必须完全按本技能执行。
- 修改历史代码时，优先在当前改动范围内向本技能收敛。
- 不做无业务收益的大范围“顺手重构”。
- 当仓库历史实现与本技能冲突时，历史实现只作为兼容依据，不作为新增代码继续扩散的理由。
- ESLint 当前允许的写法不等于团队推荐写法；新增代码按本技能中更严格的类型、分层、命名和注释要求执行。

## 目标

- 保证代码可读、可改、可查、可评审。
- 保证视图层、模块层、接口层、状态层、工具层边界清晰。
- 保证命名、类型、注释可以脱离作者个人记忆独立成立。
- 保证新增代码质量接近成熟大厂前端工程规范，而不是只满足运行结果。

## 默认工作方式

每次开发都按下面顺序思考：

1. 当前改动属于哪一层。
2. 现有仓库里是否已有可复用模式。
3. 这段状态应留在页面、模块还是全局。
4. 这段逻辑应留在视图、模块、API、Store 还是工具层。
5. 输入类型、输出类型、展示类型是否应该拆开。
6. 函数命名、变量命名、注释、副作用说明是否清楚。
7. 是否存在页面层下沉过重、接口层上浮过多、工具层语义丢失等边界问题。
8. 是否已有 `src/components/Ui`、`src/components/Bs`、`src/utils`、`src/model`、`src/constants`、`src/stores` 中的可复用能力。
9. 是否需要新增 API、Model、Store、Router、Directive、Config、Demo 示例或全局组件注册。
10. 新增路由页面时，是否已同步创建或更新对应路由配置。
11. 开发公用组件时，是否已在 `src/views/Demo` 添加对应使用示例。
12. 提交前是否通过本规范中的检查项。

## 架构与分层

### 基本原则

- 视图层只组织页面和交互，不承载接口细节。
- 模块层承接单业务域的中等复杂度逻辑。
- API 层只处理后端通信。
- Store 层只处理共享状态。
- Utils 层只处理纯函数与无 UI 依赖能力。
- Types 层只处理共享类型与声明。
- Model 层只处理业务模型、接口模型、框架模型和跨模块结构类型。
- Constants 层只处理跨页面稳定常量、枚举和提示文本。
- Configs 层只处理运行配置、代理配置和项目级开关。
- Directives 层只处理全局或可复用指令注册。
- Styles 层只处理视觉基础设施。

### 目录职责

- `src/views/**`
  页面入口层。负责页面布局、页面级状态、页面级生命周期、页面行为编排。
- `src/components/**`
  通用组件层。放基础 UI 组件和跨页面复用的通用业务组件；模板仓库中 `src/components/Ui` 为基础 UI 组件，`src/components/Bs` 为跨页面业务基础组件或业务系统组件。
- `src/modules/**`
  业务模块层。放业务域专属的组件、组合式逻辑、常量、工具和类型。
- `src/api/**`
  接口层。只做请求定义、请求参数拼装、响应转发、请求实例和头部构建。
- `src/store/**`
  状态层旧称。只处理跨页面共享状态、权限状态、账户状态和持久状态；如果仓库同时存在 `src/stores/**`，新增代码优先使用 `src/stores/**`。
- `src/stores/**`
  Pinia 状态层。放 `defineStore` 模块、`setupStore` 入口和跨页面共享状态。
- `src/router/**`
  路由层。只处理路由注册、路由模块拆分、路由元信息和开发态路由拼装。
- `src/utils/**`
  工具层。只处理纯函数、格式化、解析、计算和通用辅助逻辑。
- `src/types/**`
  类型层。只处理框架补充声明、全局共享类型和通用类型扩展。
- `src/model/**`
  模型层。放业务域模型、分页模型、路由模型、后端协议模型和跨模块可复用结构类型。
- `src/configs/**`
  配置层。放项目配置、代理配置、运行时参数和环境相关配置读取。
- `src/constants/**`
  常量层。放 API 前缀、枚举、提示文本、跨页面稳定常量。
- `src/directives/**`
  指令层。放全局指令入口和业务指令模块，例如权限指令。
- `src/layout/**`
  布局层。放应用框架、导航、页签、默认布局等页面容器能力。
- `src/styles/**`
  样式基础设施层。只处理变量、混入、主题和公共样式。
- `src/assets/icons/**`
  SVG 图标源目录，由 `vite-plugin-svg-icons` 生成 symbol，新增图标应保证命名语义清晰。
- `mock/**`
  本地 mock 层。只处理 mock 路由和 mock 数据。

### 分层硬规则

- 页面层不直接拼接口地址。
- 页面层不直接定义长串协议转换逻辑。
- API 层不写 UI 提示、不写 DOM、不跳路由。
- 统一请求基础设施文件（如 `src/api/index.ts`）允许处理全局 loading、登录失效、统一错误提示和请求拦截；业务 API 文件仍不得写 UI 行为、DOM 操作和路由跳转。
- Store 层不保存页面一次性临时状态。
- Utils 层不依赖具体页面名和页面结构。
- Model 层不写运行逻辑，不依赖组件、路由实例、Store 实例和 DOM。
- Constants 层不放会随页面状态变化的数据。
- 组件内部不隐式依赖页面专属状态。
- 页面和组件层优先通过 `@/api/**`、`@/model/**`、`@/constants/**`、`@/stores/**` 访问跨层能力，不使用深层相对路径穿透边界。

### 何时抽到 `modules`

满足任一条件，优先考虑抽到 `src/modules/**`：

- 页面文件过长，职责难以一眼看清。
- 同一业务域已经包含组件、状态、类型、工具、常量中的两个及以上。
- 同一逻辑在多个页面复用，但放 `utils` 又会丢失业务语义。
- 页面中开始出现大量协议转换、状态桥接、行为编排。
- 同一页面目录下已经出现 `Components/`、`Composable/`、`types.ts`、`constants/` 中两个及以上角色，并且这些角色都服务同一个稳定业务域。

## 命名规范

### 总原则

- 名称必须能脱离上下文独立理解。
- 先表达业务含义，再表达技术实现。
- 不允许使用作者个人缩写、临时缩写、语义不明缩写。

### 文件与目录命名

- src下的目录统一使用小驼峰命名，避免冲突；模板仓库已有一级基础目录沿用现状，例如 `api`、`assets`、`components`、`configs`、`constants`、`directives`、`layout`、`model`、`router`、`stores`、`styles`、`types`、`utils`、`views`。
- Vue 组件文件统一使用 `PascalCase.vue`。
- views下以及所有下级的目录统一使用 `PascalCase`。
- 工具、常量、类型文件统一使用语义化 `camelCase.ts` 或 `index.ts`；`src/model/**`、`src/constants/**` 中已经形成的后端协议或枚举文件可沿用 `kebab-case.ts`，例如 `paging-param.ts`、`api-prefix.ts`。
- `src/components/Ui` 组件名称统一使用 `UiXxx`，`src/components/Bs` 组件名称统一使用 `BsXxx`，与全局注册名保持一致。
- Store 文件放在 `src/stores/modules/**` 时使用业务名 `camelCase.ts`，导出 `useXxxStore` 或 `useXxxStoreWithOut`。
- API 文件按后端业务域分组，例如 `src/api/sys/account.ts`；文件名表达资源或业务实体，不表达页面名。
- 不允许使用 `test1.ts`、`new.ts`、`copy.ts`、`final.ts`、`temp.ts` 等无语义文件名。

### 文件与目录结构

- 每个路由页面都是对应 views 下的一个目录，避免重复，其中的结构为，注意如不需要相关功能，可不创建对应文件， 创建时请按照如下结构：
  - `index.vue`：页面入口文件，负责页面布局、页面级状态、页面级生命周期、页面行为编排。
  - `Components/`：页面内复用组件，避免与 `src/components/` 混淆, 如果这里的组件绝对的通用，请在`src/components/` 创建对应组件，注区分是UI组件还是业务组件（如果具体项目约定 Bs目录下的组件为UI组件，则按项目约定执行）；结合当前模板仓库，新增全局基础 UI 优先放 `src/components/Ui`，跨页面业务基础组件优先放 `src/components/Bs`，判断依据参照 `目录职责`中我给你的描述。
  - `Modules/`：页面专属模块。
  - `types.ts`：页面专属类型，如接口返回结构、组件 props 等。
  - `Composable/`：页面专属组合式逻辑, 如组合式逻辑、状态桥接、行为编排。
  - `constants/`：页面专属常量, 如常量、枚举等。
- 模板仓库历史上存在部分简单页面直接使用 `PascalCase.vue` 或 `index.vue`；新增路由页面优先使用目录承载，修改旧页面时只在当前范围内收敛，不为改目录而改目录。
- 创建新的路由页面时，必须同步创建或更新对应路由配置；静态路由、开发态路由和权限动态路由按 `Router 规则` 选择正确位置。
- 页面目录中的 `Components/`、`Modules/`、`Composable/` 使用 `PascalCase` 是为了和 `views` 下既有目录风格一致；公共 `src/modules/**` 如新建，则按业务域语义命名并保持同域内部一致。
- 页面专属类型只服务单页面时放 `types.ts`；被 API、Store、多个页面或组件复用时必须上移到 `src/model/**` 或 `src/types/**`。

### 变量命名

- 布尔变量必须带状态语义前缀：`is`、`has`、`can`、`should`、`need`。
- 展示状态优先使用：`isVisible`、`showDialog`、`isExpanded`、`isDisabled`、`isLoading`。
- 数组和集合必须体现集合语义：`sessionList`、`historyGroups`、`selectedIds`、`workspaceOptions`。
- 映射、索引、集合结构必须使用 `Map`、`Record`、`Set`、`Index` 等后缀。
- DOM 或实例引用必须使用 `xxxRef`。
- 从接口返回的原始数据不要直接命名为 `data` 长期流转；应在接收后转换为 `accountInfo`、`menuList`、`pagingResult` 等业务名。
- 不复制历史拼写错误，例如 `loadding`；除非正在兼容既有字段或接口协议，新增命名统一使用 `loading`。
- 禁止使用 `a`、`b`、`tmp`、`obj`、`data1`、`res1`、`flag` 这类变量名。

### 函数命名

- 函数名必须是动词或动词短语。
- 事件处理统一使用 `handleXxx`。
- 查询统一使用 `getXxx`、`fetchXxx`、`loadXxx`。
- 转换统一使用 `buildXxx`、`parseXxx`、`normalizeXxx`、`formatXxx`。
- 状态操作统一使用 `setXxx`、`toggleXxx`、`resetXxx`、`openXxx`、`closeXxx`。
- 持久化统一使用 `persistXxx`、`saveXxx`、`restoreXxx`。
- Pinia Store action 使用业务动作命名，例如 `setAccountInfo`、`initLayout`；如果 action 内部有异步请求，函数注释必须说明写入哪些 state。
- 组合式函数统一使用 `useXxx`，返回值应体现状态、动作和只读派生数据的边界。

### 类型与常量命名

- `interface` 统一使用 `PascalCase`。
- props 类型统一使用 `XxxProps`。
- 响应体统一使用 `XxxResponse`、`XxxResult`、`XxxPayload`。
- 表单结构统一使用 `XxxFormModel`。
- 查询参数统一使用 `XxxQuery`。
- 常量统一使用 `UPPER_SNAKE_CASE`。
- 常量名必须表达语义和单位，例如 `VOICE_UNSUPPORTED_RESET_DELAY`。
- 页面或模块私有枚举值使用对象替代，不使用 TypeScript `enum` 或 `const enum`；对象名使用业务语义名，键名使用 `camelCase`，值对齐后端协议或业务常量值。
- 公用枚举必须放在 `src/constants/enum/**`，并按照模板仓库 `src/constants/enum` 规范定义：导出名使用 `enumXxx`，枚举项值使用 `new EnumValue(value, label)`，需要标签展示时链式调用 `setTagTypeXxx()`、`setTagColorXxx()`。
- `src/constants/enum/public.ts` 放所有部门同步更新的公用枚举；`src/constants/enum/business.ts` 放本项目专用的公共业务枚举；新增枚举后必须在 `src/constants/enum/index.ts` 汇总导出。
- 类型文件中的后端协议字段名可以对齐后端命名，但前端派生展示类型应使用清晰的业务命名。

页面或模块私有枚举推荐写法：

```ts
export const AssistantHistoryAction = {
  copy: 'copy',
  delete: 'delete',
  export: 'export',
  rename: 'rename',
} as const

export type AssistantHistoryAction = (typeof AssistantHistoryAction)[keyof typeof AssistantHistoryAction]
```

公用枚举推荐写法：

```ts
import { EnumValue } from '@/model/frame/enum-value'

export const enumApplyMaterialType = {
  need: new EnumValue(1, '必要材料'),
  noNeed: new EnumValue(2, '非必要材料'),
}

export default {
  enumApplyMaterialType,
}
```

禁止写法：

```ts
export enum AssistantHistoryAction {
  Copy = 'copy',
  Delete = 'delete',
  Export = 'export',
  Rename = 'rename',
}
```

## 注释规范

### 总原则

- 注释必须使用中文文本说明。
- 注释必须补充代码本身无法直接表达的信息。
- 注释必须说明业务目的、输入约束、返回语义、副作用、边界条件。
- 禁止写“设置值”“循环数组”“调用接口”这类无信息增量的注释。

### 必须写函数注释的情况

满足以下任一条件，函数必须写完整注释：

- 存在参数
- 返回值有业务语义
- 包含异步流程
- 存在副作用
- 分支较多
- 会修改响应式状态、缓存、URL、路由、DOM、定时器、流式连接

### 函数注释硬规则

- 有几个参数，就写几个 `@param`。
- 参数是对象时，必须解释关键字段。
- 非 `void` 且有业务意义的返回值，必须写 `@returns`。
- 涉及缓存、路由、DOM、定时器、请求、副作用时，必须写清影响面。
- 如果调用方需要满足前置条件，也要写清责任边界。

### 标准模板

```ts
/**
 * 根据会话列表生成按时间分组的历史记录，供侧边栏直接渲染。
 * @param sessions 原始会话列表，通常来自会话查询接口，允许为空数组。
 * @param timezone 用于生成分组标签的时区标识，缺省时使用当前浏览器时区。
 * @returns 返回按标签分组后的历史记录列表，结果可直接用于渲染。
 */
function normalizeHistoryGroups(
  sessions: SessionSummary[],
  timezone?: string,
): AgentHistoryGroup[] {}
```

### 对象参数模板

```ts
/**
 * 根据筛选条件查询会话列表。
 * @param query 查询条件对象。
 * @param query.keyword 模糊搜索关键词，可为空字符串。
 * @param query.workspace 工作区路径，用于限定搜索范围。
 * @param query.model 模型标识，用于过滤指定模型会话。
 * @returns 返回会话列表查询结果。
 */
function fetchSessions(query: SessionQuery): Promise<SessionListResult> {}
```

### 文件注释规则

- 新增 `.vue` 文件顶部必须添加文件头注释，说明标题、功能、说明、当前版本和开发信息。
- 新增普通 `.ts` 文件顶部必须添加文件头注释，说明功能、文件名、日期和作者；`.d.ts`、自动生成文件不套用普通 `.ts` 文件头模板。
- 工具文件顶部必须写清职责。
- 业务模块入口文件应说明模块边界。
- 修改历史文件时，如果文件已有旧格式文件头，优先在当前改动范围内补全为本规范模板；自动生成文件不手写文件头。
- `src/types/*.d.ts`、`auto-imports.d.ts`、第三方声明文件顶部必须说明声明目的或生成来源。

`.vue` 文件头模板：

```vue
<!--
  标题：      ${FILE_NAME}
  功能：      功能描述
  说明：
  当前版本：   1.0
  开发信息：   Created by xxx ${DATE}
-->
```

`.ts` 文件头模板：

```ts
/**
 * 功能描述
 * @file  ${FILE_NAME}
 * @date ${DATE}
 * @author xxx
 */
```

模板变量说明：

- `${FILE_NAME}` 使用当前文件名，包含扩展名，例如 `AccountList.vue`、`account.ts`。
- `${DATE}` 使用创建或补充文件头当天日期，格式优先使用 `YYYY-MM-DD`。
- `功能描述` 必须替换为具体业务职责，不保留占位文本。

### 行内注释规则

- 只在复杂分支、兼容性处理、历史约束、协议转换场景使用。
- 不注释显而易见的赋值、循环、判断。
- 不允许用长注释掩盖糟糕命名。

## Vue 与组件规范

### 基础要求

- 默认优先使用 `<script setup lang="ts" name="">`。
- 模板仓库已启用 `vite-plugin-vue-setup-extend`，`name` 可直接写在 `<script setup>` 上；新增组件必须有稳定组件名，便于调试、缓存和全局注册。
- 历史组件或需要注册局部指令的组件可以保留 `defineComponent` 或额外普通 `<script lang="ts">`，但新增逻辑优先收敛到 `script setup`。
- `unplugin-auto-import` 已自动引入 `vue` 和 `vue-router` 常用 API；新增代码可以使用自动引入，但类型、业务模块、组件、Store、API 仍必须显式 import。
- 页面组件负责编排，模块组件负责业务承载，基础组件负责复用。
- 模板只保留渲染表达式，不堆积业务计算。
- 复杂逻辑应拆到 Composable、模块工具函数或模块内部状态逻辑中。

### 组件分类

- 基础组件：强调通用性、低业务耦合、强复用。
- 通用业务组件：强调业务表达，但仍可跨页面复用。
- 模块组件：服务单一业务域，不要求全局复用。
- 页面组件：页面入口，只负责编排。
- `Ui` 组件：模板仓库全局基础 UI 组件，命名为 `UiXxx`，由 `src/components/index.ts` 按需注册。
- `Bs` 组件：模板仓库跨页面业务基础组件，命名为 `BsXxx`，只注册稳定高频组件，避免无节制全局注册增加首屏负担。
- 开发或调整 `src/components/Ui`、`src/components/Bs` 等公用组件时，必须在 `src/views/Demo` 下添加或更新对应使用示例；`Ui` 示例优先放 `src/views/Demo/Ui`，`Bs` 示例优先放 `src/views/Demo/Bs`。

### 组件结构建议

推荐顺序：

1. 文件头注释
2. `template`
3. `script setup`
4. 常量
5. 类型
6. props / emits
7. 响应式状态
8. computed
9. methods
10. lifecycle
11. watch
12. `style`

在使用 `defineComponent` 的历史组件中，推荐顺序为：文件头注释、`template`、`script lang="ts"`、`name`、`props`、`emits`、`setup`、局部函数、`return`、`style`。

### props 与 emits

- props 名称必须表达用途。
- 布尔 props 使用 `is`、`show`、`disabled`、`loading` 等语义。
- 默认值通过 `withDefaults` 明确声明；保留运行时 props 写法时，必须使用 `default` 和必要的 `PropType<T>` 明确类型。
- 不允许用一个巨大对象 props 充当万能入参。
- 自定义事件名必须表达动作或结果，禁止 `change2`、`clickItem` 这类模糊命名。
- `v-model` 统一使用 `modelValue` 与 `update:modelValue`，派生可写状态用 `computed({ get, set })`。
- 事件定义优先使用类型化 `defineEmits`；保留数组写法时，事件名必须能从模板和调用方直接理解。

### 模板规则

- 不在模板里写复杂协议转换。
- 不在模板里写嵌套三元表达式。
- 条件过长时抽成计算属性或方法。
- 可点击元素必须有明确交互语义。
- `button` 默认写 `type="button"`。
- 图标按钮优先补 `aria-label`。
- 全局组件 `UiXxx`、`BsXxx` 可直接使用；非全局组件必须显式 import。
- `el-button`、原生 `button` 涉及表单时必须确认 `type`，避免默认提交副作用。

### 响应式规则

- DOM 节点和实例引用使用 `ref`。
- 派生状态使用 `computed`。
- 多字段局部状态使用 `reactive`。
- 单一标量不要硬塞进 `reactive`。
- `computed` 只做派生，不做副作用。
- `watch` 只做必要联动，不作为默认方案。
- 需要向子组件树共享表格、表单等上下文时可以使用 `provide/inject`，但必须明确注入 key 和数据结构，避免隐式全局依赖。
- 页面卸载时必须清理手动创建的定时器、事件监听、流式连接和第三方实例。

## TypeScript 规范

### 基础规则

- 所有新增代码必须使用 TypeScript。
- 公共函数参数和返回值必须可读、可查、可复用。
- 优先 `interface` 描述对象结构。
- 优先 `type` 描述联合、映射、工具类型。
- 非必要不使用 `any`。
- 使用 `unknown` 时必须先收窄再访问字段。
- 新增枚举语义不使用 TypeScript `enum`；页面或模块私有枚举使用 `as const` 对象和类型推导，公用枚举放到 `src/constants/enum/**` 并使用 `EnumValue` 规范。

### 类型设计

- 输入类型和输出类型分离。
- 查询参数、表单模型、列表项、详情项尽量分离。
- 后端原始结构与前端展示结构允许分离，通过 `normalize`、`parse`、`format` 函数转换。
- 共享类型必须沉淀到公共位置，避免在多个文件复制结构。
- `src/model/**` 优先承载业务模型、接口模型、分页模型、路由模型；`src/types/**` 优先承载全局声明、模块补充声明和框架扩展。
- `src/types/global.d.ts` 中的 `GwDynamicProp`、`Recordable`、`Window[propName]` 属于历史兜底能力，新增公共边界不应继续扩大动态类型范围。
- 自动生成文件如 `auto-imports.d.ts` 不手写业务类型；组件全局声明放在 `src/types/components.d.ts`。

### `any` 使用红线

- 禁止把 `any` 当作默认解法。
- 仅在第三方类型缺失、动态协议不可控、历史过渡场景下允许短暂使用。
- `any` 不得向外扩散为公共类型边界。
- 如果必须使用 `any`，必须把范围限制在函数内部或兼容层，并优先在出口转换为明确类型。
- 不使用 `Function` 作为公共回调类型，除非正在兼容历史声明；新增代码应写清参数和返回值。

## API、Store 与数据流规范

### API 规则

- 页面和组件层不直接拼接口地址。
- API 层只表达后端行为，不表达 UI 行为。
- URL 构建、请求头构建、请求实例配置允许抽成私有函数。
- API 层不做页面消息提示，不操作 DOM，不跳转路由。
- API 层不要无理由吞异常。
- 业务 API 文件沿用模板仓库的领域对象导出方式时，默认导出对象的方法必须按后端资源或业务动作命名。
- API 前缀统一从 `src/constants/api-prefix.ts` 或配置层读取，不在页面、组件或 Store 中散落硬编码前缀。
- `src/api/index.ts` 是统一请求实例和拦截器边界，可以集中处理 token、loading、统一错误、登录失效和 `customResponse`；除该基础设施外，普通 API 文件不得处理 UI 副作用。
- `request` 配置中的 `hideLoading`、`customResponse`、`__loading` 属于请求基础设施扩展字段，使用时必须说明调用方是否接管 loading 或错误提示。

### 参数与响应规则

- GET 请求使用 `params` 或 `query`。
- POST/PUT 请求体使用 `data` 或 `payload`。
- ID 参数必须使用清晰命名，例如 `sessionId`、`accountId`。
- 参数字段名优先对齐后端协议，函数名负责表达业务语义。
- 后端分页协议优先复用 `PagingParam`、`BsTableAxios` 等已有模型；需要新增分页结构时，必须说明 `page`、`pageSize`、`offset`、`records`、`showRecords` 的语义。
- 表格查询参数如需把对象转成字符串，优先复用已有 `parseParam` 或抽出明确转换函数，不在页面模板或组件模板中拼接。
- 文件流、下载、打印等特殊响应必须在函数名、返回类型或注释里说明返回的是原始响应、Blob、文件流还是业务体。

### 错误处理规则

- 区分网络错误、参数错误、空态错误和业务错误。
- 用户提示由 UI 层或调用层负责。
- 日志输出要克制，禁止残留无意义 `console.log`。
- 如果捕获后吞掉错误，必须有明确理由。
- 统一请求拦截器已经处理的全局错误，不要在每个业务 API 方法里重复弹提示。
- 使用 `await-to-js` 等错误捕获工具时，必须处理 `err` 分支；不能只为绕过 `try/catch` 而忽略失败路径。

### Store 规则

- 新增全局状态默认使用 Pinia，目录为 `src/stores/modules/**`。
- Store 文件导出 `useXxxStore`，需要在非 setup 场景使用时可提供 `useXxxStoreWithOut`，但必须沿用仓库已有 `src/stores/index.ts` 的 Pinia 实例模式。
- `state` 存事实数据，不存重复推导值。
- `getter` 存派生读取逻辑，不存副作用。
- `mutation` 只做同步赋值（仅针对 Vuex 4 历史兼容；Pinia 新增代码不写 mutation）。
- `action` 负责异步、流程控制、提交状态与错误处理。
- Pinia `actions` 中允许直接修改 state，但必须保持一个 action 一个业务动作，不把多个页面流程硬塞到同一个 action。
- 页面私有状态不进入全局 store。
- 会写入缓存、DOM class、全局配置或权限状态的 Store action 必须在注释中说明副作用。

## Router 与样式规范

### Router 规则

- 路由元信息必须与页面职责一致。
- 静态路由、动态路由、开发态路由要清晰分开。
- 路由按业务域拆分，禁止把所有路由堆在一个文件。
- 页面组件优先使用动态 `import()` 懒加载。
- 路由类型优先使用 `GwRouteRecordRaw`、`GwRouteMeta` 等已有模型，不重复定义相同结构。
- 静态页面放在 `constantRoutes`，开发态演示路由按模块拆到 `src/router/modules/**`，权限动态路由交给权限流程处理。
- 路由 `name`、`path`、`meta.title` 必须稳定，不随展示文案临时变化；需要隐藏菜单时使用既有 `hidden` 或 `meta.hidden` 约定。
- 新增路由页面必须同步补齐路由配置，页面路径、路由 `path`、`name`、`meta.title` 和懒加载组件路径必须一致可追踪。
- 新增 Demo 示例如果需要路由访问，应同步补到开发态路由模块，例如 `src/router/modules/frame/**`。

### 样式规则

- 优先复用 `src/styles/_var.scss`、`src/styles/_mixin.scss`、`src/styles/theme/index.scss`。
- 颜色、字号、间距、层级优先走变量，不随意写魔法值。
- 组件样式默认局部化，公共主题覆盖样式集中管理。
- 样式命名优先表达结构与语义，不表达偶然视觉状态。
- 对 Element Plus 的覆盖样式必须集中、克制、可追踪。
- Vite 已通过 `additionalData` 全局注入 `_var.scss` 和 `_mixin.scss`，组件样式中可直接使用变量和 mixin，不重复 `@use`。
- 全局样式入口为 `src/styles/index.scss`，公共样式拆在 `util.scss`、`form.scss`、`public.scss`、`element/index.scss`、`theme/index.scss`。
- 组件私有样式优先使用 `scoped`；确需影响 Element Plus 内部结构或全局布局时，说明原因并放到公共样式或明确命名的非 scoped 区域。
- BEM 或 `block__element--modifier` 风格可用于复杂组件，类名必须表达结构和业务语义。

## 工程化与格式规范

### 格式化规则

- Prettier 使用模板仓库配置：`printWidth: 120`、`tabWidth: 2`、`semi: false`、`singleQuote: true`、`trailingComma: 'es5'`、`bracketSpacing: true`、`arrowParens: 'always'`、`vueIndentScriptAndStyle: true`、`endOfLine: 'auto'`。
- 新增和修改文件必须匹配 Prettier 输出，不手动维持与格式化器冲突的排版。
- TypeScript、Vue、SCSS 中统一不写分号，字符串优先单引号。

### ESLint 与自动导入

- ESLint 当前同时存在根目录 `.eslintrc.cjs` 和 `src/eslint.config.js`，规则核心为 `vue3-recommended`、`@typescript-eslint/recommended`、`prettier`。
- 未使用变量会报错；确需保留的参数或变量必须使用 `_` 前缀。
- `@typescript-eslint/no-explicit-any` 当前关闭，但本技能仍要求新增公共边界避免 `any`。
- `vue/component-definition-name-casing` 要求组件定义名使用 `PascalCase`。
- `auto-imports.d.ts` 由工具生成，不手写修改；新增自动导入范围必须同步考虑 ESLint 生成文件。

### 构建与别名

- 默认别名为 `@ -> src`，并额外存在 `views -> src/views`、`utils -> src/utils`；新增代码优先使用 `@/`，避免深层相对路径。
- 开发端口默认为 `8033`，代理规则来自 `src/configs/proxy.ts`。
- 生产构建会通过 esbuild 丢弃 `console` 和 `debugger`，但开发代码仍不得残留无意义调试输出。
- SVG 图标通过 `src/assets/icons` 和 `vite-plugin-svg-icons` 注册，组件侧优先使用既有 `UiIcon` 或 Element Plus 图标注册方式。

## 工程红线

以下行为默认禁止：

- 页面直接写请求细节
- 组件直接读写全局缓存而无封装
- 重复定义相同类型
- 用 `any` 掩盖类型问题
- 扩散 `GwDynamicProp`、`Recordable<any>`、`Function` 到新增公共 API
- 新增 TypeScript `enum` 或 `const enum`
- 用大段注释掩盖糟糕命名
- 在模板中用嵌套三元表达式处理复杂渲染
- 为一次性逻辑创建永久抽象负担
- 将多个业务职责硬塞进单文件
- 手写或修改自动生成的 `auto-imports.d.ts`
- 在页面、组件、Store 中散落 API 前缀、代理地址和后端路径硬编码
- 新增路由页面但没有同步路由配置
- 开发公用组件但没有在 `src/views/Demo` 添加或更新使用示例
- 新增 `.vue` 或普通 `.ts` 文件但没有按模板添加文件头注释
- 无节制把低频组件注册为全局组件
- 新增代码继续复制历史拼写错误、过宽的 `any` 或无类型回调

## 评审与交付检查

提交前至少检查以下内容：

- 改动是否落在正确目录。
- 页面、模块、API、Store、工具边界是否清楚。
- 命名是否脱离上下文也能理解。
- 新增 `.vue`、普通 `.ts` 文件是否按模板添加文件头注释。
- 函数注释是否逐个写明参数。
- 返回值和副作用是否说明清楚。
- 是否存在本可避免的 `any`。
- 是否新增了 TypeScript `enum`，如有必须按作用域改为页面私有 `as const` 对象或 `src/constants/enum/**` 公用枚举。
- 公用枚举是否放在 `src/constants/enum/public.ts` 或 `src/constants/enum/business.ts`，并同步到 `src/constants/enum/index.ts`。
- 是否复制了相同类型结构而未沉淀。
- 是否复用了现有组件、常量、类型、工具和样式变量。
- 是否残留调试代码和无意义日志。
- 是否符合仓库 ESLint、Prettier、TypeScript 约束。
- 是否优先使用 `src/stores` 的 Pinia 模式，而不是新增 Vuex 写法。
- 新增路由页面是否同步创建或更新了对应路由配置。
- 公用组件新增或调整后，是否在 `src/views/Demo/Ui`、`src/views/Demo/Bs` 或对应 Demo 分类下补充使用示例。
- 是否把共享接口模型放到 `src/model/**` 或 `src/types/**`，而不是散落在页面中。
- 是否使用 `@/` 别名并避免跨层深层相对路径。
- 是否需要同步全局组件注册、全局组件类型声明、路由模型或自动导入声明。
- 交付前优先运行 `npm run lint:eslint`、`npm run lint:prettier`；涉及类型或构建风险时追加 `npm run build`。

## 输出要求

- 输出必须能够直接用于本仓库。
- 这是合并版单文件规范，适用于统一培训、团队对齐、直接调用和单文件分发。
- 如果任务涉及具体编码，实现必须满足本技能中的全部硬规则。
