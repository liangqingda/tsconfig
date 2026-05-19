# 使用方式

## 安装

```bash
pnpm add -D typescript @liangqingda/tsconfig
```

## React 项目

```json
{
  "extends": "@liangqingda/tsconfig/react.json",
  "include": ["src"]
}
```

## Node.js 项目

请先安装 Node.js 类型声明：

```bash
pnpm add -D @types/node
```

```json
{
  "extends": "@liangqingda/tsconfig/node.json",
  "include": ["src/**/*.ts"]
}
```

如果项目使用 CommonJS，或需要产物输出目录，请在本地项目里继续覆盖：

```json
{
  "extends": "@liangqingda/tsconfig/node.json",
  "compilerOptions": {
    "module": "CommonJS",
    "moduleResolution": "Node",
    "outDir": "dist"
  },
  "include": ["src/**/*.ts"]
}
```

## 推荐做法

下面这些配置建议保留在业务项目本地，而不是放到共享包里：

- `include`
- `exclude`
- `references`
- `paths`
- `rootDir`
- `outDir`
- `tsBuildInfoFile`

这些配置通常依赖具体项目的目录结构或构建方式，不适合直接跨项目复用。
