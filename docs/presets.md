# 预设说明

这个包目前提供 3 份可复用预设：

- `@liangqingda/tsconfig`
- `@liangqingda/tsconfig/react.json`
- `@liangqingda/tsconfig/node.json`

其中：

- `@liangqingda/tsconfig` 等价于 `@liangqingda/tsconfig/base.json`
- `react.json` 和 `node.json` 都是在 `base.json` 的基础上继续补充运行环境相关配置

如果你只想复用一套严格校验规则，用 `base.json` 即可；如果项目已经明确是 React 前端或 Node.js 项目，直接用对应预设更省事。

## `@liangqingda/tsconfig`

根配置只做一件事：`extends: "./base.json"`。

也就是说，下面两种写法是完全等价的：

```json
{
  "extends": "@liangqingda/tsconfig"
}
```

```json
{
  "extends": "@liangqingda/tsconfig/base.json"
}
```

适用场景：

- 纯 TypeScript 库
- 还没确定运行环境的项目
- 想先启用统一的严格规则，再在本地项目里自行覆盖其他编译选项

## `@liangqingda/tsconfig/base.json`

通用基础配置，适合所有 TypeScript 项目。它只放那些和具体运行环境无关、但普遍值得开启的校验规则。

包含的选项如下：

- `strict: true`
  开启 TypeScript 的严格模式。它会收紧空值、隐式 `any`、函数参数和返回值推断等一整组检查，目标是尽早暴露类型不明确的问题。
- `skipLibCheck: true`
  跳过依赖包里 `.d.ts` 声明文件的完整类型检查。这样可以减少编译时间，也能避免被第三方类型定义里的历史问题卡住。
- `noUnusedLocals: true`
  局部变量声明后如果没有被使用，会直接报错。适合尽早清理无效代码和遗留变量。
- `noUnusedParameters: true`
  函数参数如果没有被使用，会报错。这样能提醒你删除无效参数，或者明确表达“这个参数就是暂时保留的”。
- `noFallthroughCasesInSwitch: true`
  防止 `switch` 里的 case 意外贯穿到下一个分支。常见于漏写 `break`、`return` 时的低级 bug。
- `noUncheckedSideEffectImports: true`
  对只为了副作用执行的导入也做更严格的检查，比如 `import "./setup"` 这类写法。它能减少导入路径写错却不容易发现的问题。

一句话理解：`base.json` 关心的是“代码写得够不够严谨”，不关心“代码最终跑在浏览器还是 Node.js”。

## `@liangqingda/tsconfig/react.json`

用于 React 或其他依赖 bundler 的前端项目，比如 Vite、Rspack、Webpack、Rollup 一类构建流程。

它继承自 `base.json`，并额外补充了这些前端常用配置：

- `target: "ES2020"`
  以 ES2020 作为输出目标，默认面向较新的浏览器和现代前端工具链。
- `useDefineForClassFields: true`
  使用更接近现代 JavaScript 标准的 class fields 语义，减少类字段在不同编译行为下的偏差。
- `lib: ["ES2020", "DOM", "DOM.Iterable"]`
  同时启用现代 JavaScript 能力和浏览器环境类型声明。
  - `ES2020` 提供 Promise、String、Array 等标准库能力的类型定义
  - `DOM` 提供 `window`、`document`、`HTMLElement` 等浏览器 API
  - `DOM.Iterable` 让部分 DOM 集合具备可迭代类型支持
- `module: "ESNext"`
  保留现代 ESM 模块语法，交给 bundler 处理模块打包和转换。
- `moduleResolution: "Bundler"`
  按 bundler 的解析方式处理模块，更适合前端工程里的包导出、别名和现代依赖结构。
- `allowImportingTsExtensions: true`
  允许直接导入带 `.ts` / `.tsx` 扩展名的模块，适合部分前端工具链和工作区内联调场景。
- `isolatedModules: true`
  要求每个文件都能被独立编译，避免使用某些只能依赖 TypeScript 全局分析的语法特性，更贴合 Babel、SWC、esbuild 这类单文件转换工具。
- `moduleDetection: "force"`
  强制把文件按模块看待，避免文件意外落入全局脚本作用域。
- `noEmit: true`
  TypeScript 只负责类型检查，不负责输出构建产物；真正的打包结果由 bundler 生成。
- `jsx: "react-jsx"`
  使用 React 17+ 的自动 JSX 运行时，通常不需要再手动 `import React`。

一句话理解：`react.json` 适合“TypeScript 负责检查，构建工具负责产物”的现代前端项目。

## `@liangqingda/tsconfig/node.json`

用于 Node.js 后端服务、CLI 工具、脚本项目。

它同样继承自 `base.json`，并补充了这些 Node.js 相关配置：

- `target: "ES2022"`
  以较新的 JavaScript 语法为输出目标，更贴近现代 Node.js 运行时的原生能力。
- `lib: ["ES2023"]`
  启用较新的标准库类型定义，让新一些的 JavaScript 内置 API 也能获得正确类型提示。
- `module: "NodeNext"`
  使用 Node.js 现代模块系统规则。它会结合文件扩展名和 `package.json` 里的 `type` 字段来判断当前文件是 ESM 还是 CommonJS。
- `moduleResolution: "NodeNext"`
  让 TypeScript 按 Node.js 现代模块解析方式查找依赖，与上面的 `module: "NodeNext"` 配套使用。
- `types: ["node"]`
  显式引入 Node.js 全局类型，比如 `process`、`Buffer`、`__dirname`，以及 `fs`、`path` 等内置模块的声明。

一句话理解：`node.json` 适合“代码最终直接跑在 Node.js 里”的项目。

如果项目不是 ESM，或者你明确需要 CommonJS 输出，可以在业务项目里自行覆盖，例如：

```json
{
  "extends": "@liangqingda/tsconfig/node.json",
  "compilerOptions": {
    "module": "CommonJS",
    "moduleResolution": "Node"
  }
}
```
