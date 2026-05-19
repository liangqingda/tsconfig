# 预设说明

## `@liangqingda/tsconfig`

根配置，等价于 `base.json`，只包含通用的严格校验规则。

## `@liangqingda/tsconfig/base.json`

适合所有 TypeScript 项目的通用基础配置，主要提供：

- `strict`
- `skipLibCheck`
- `noUnusedLocals`
- `noUnusedParameters`
- `noFallthroughCasesInSwitch`
- `noUncheckedSideEffectImports`

## `@liangqingda/tsconfig/react.json`

用于 React 或其他基于 bundler 的前端项目，额外提供：

- `lib: ["ES2020", "DOM", "DOM.Iterable"]`
- `moduleResolution: "Bundler"`
- `jsx: "react-jsx"`
- `noEmit: true`

## `@liangqingda/tsconfig/node.json`

用于 Node.js 后端或工具脚本项目，额外提供：

- `target: "ES2022"`
- `lib: ["ES2023"]`
- `module: "NodeNext"`
- `moduleResolution: "NodeNext"`
- `types: ["node"]`

如果项目不是 ESM，或者仍然需要 CommonJS，请在业务项目中自行覆盖 `module` 和 `moduleResolution`。
