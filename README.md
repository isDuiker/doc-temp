# doc-temp

一个文档模板站，使用 Markdown 编写，基于 [VitePress 2](https://vitepress.dev/) 构建。

当前模板已配置支持中文界面、暗黑模式和自动生成的侧边栏。可根据需要基于此模板配置其他需要的内容

## 技术栈

- **文档框架**: VitePress 2.0.0-alpha.21（内置 Vite 8 + Vue 3.5）
- **侧边栏**: vitepress-sidebar 1.41，按目录自动生成
- **包管理**: pnpm（也兼容 npm）

## 目录结构

```
doc-temp/
├── docs/
│   ├── .vitepress/
│   │   ├── config.ts            # 站点配置（导航、页脚、侧边栏等）
│   │   └── dist/                # 构建产物（已忽略）
│   └── src/                     # Markdown 文档源目录
│       ├── index.md             # 首页
│       ├── 02_Css/
│       │   ├── 01_scss入门基础.md
│       │   └── 02_scss进阶技巧.md
│       └── 03_JavaScript/
│           └── 01-字符串的特性.md
├── package.json
└── README.md
```

## 快速开始

### 环境要求

- Node.js 20.19+ 或 22.12+（推荐 22 LTS）
- pnpm 或 npm

### 安装依赖

```bash
pnpm install
# 或
npm install
```

### 本地开发

```bash
pnpm docs:dev
# 或
npm run docs:dev
```

启动后访问 <http://localhost:5173/>。Markdown 修改会热更新，无需重启。

### 构建与预览

```bash
pnpm docs:build     # 构建到 docs/.vitepress/dist
pnpm docs:preview   # 本地预览构建产物
```

预览地址默认为 <http://localhost:4173/>，以终端输出为准。

## 编写文档

### 新增文章

1. 在 `docs/src/` 下按分类新建目录（如 `04_Network/`）；
2. 新建 `.md` 文件，以一级标题开头；
3. 保存后侧边栏会自动更新，无需手动配置。

### 文件命名与排序

- 目录名和文件名支持数字前缀：`01-`、`01_`、`01.`，数字越小越靠前；
- 前缀只用于排序，生成 URL 时会自动去掉；
- 目录名会参与路由。例如 `02_Css/01_scss入门基础.md` 对应 `/Css/scss入门基础`；
- 也可以在 frontmatter 中用 `order` 手动排序（值越小越靠前），未指定时排在后面：

```md
---
order: 1
---

# scss 入门基础
```

### 站内链接

站点开启了 `cleanUrls`，链接不需要写 `.md` 后缀，也不带数字前缀：

```md
[scss 进阶技巧](/Css/scss进阶技巧)
```

### 侧边栏规则

侧边栏由 `vitepress-sidebar` 自动生成，配置位于 `docs/.vitepress/config.ts`：

- 使用数字前缀排序，并在排序后隐藏编号；
- 未设置 `order` 的文章默认排在最后；
- 目录默认展开（`collapsed: false`）。

## 站点配置

主要配置在 `docs/.vitepress/config.ts`：

| 配置 | 当前值 | 说明 |
| --- | --- | --- |
| `base` | `/` | 部署根路径，站点通过域名根目录访问 |
| `srcDir` | `src` | 文档源目录为 `docs/src` |
| `cleanUrls` | `true` | URL 不带 `.html` 后缀 |

构建产物默认输出到 `docs/.vitepress/dist`。

导航栏、社交链接、页脚、大纲和“上一页/下一页”等文案也都在这份配置中修改。

## 脚本命令

| 命令 | 说明 |
| --- | --- |
| `pnpm docs:dev` | 启动开发服务器（热更新） |
| `pnpm docs:build` | 构建静态站点 |
| `pnpm docs:preview` | 本地预览构建产物 |

使用 npm 时对应 `npm run docs:dev`、`npm run docs:build`、`npm run docs:preview`。

## 部署

1. 执行 `pnpm docs:build`，产物位于 `docs/.vitepress/dist/`；
2. 将 `docs/.vitepress/dist/` 上传到静态托管服务（Nginx、GitHub Pages、对象存储等）；
3. 如果部署在子路径（如 `/docs/`），先修改 `config.ts` 中的 `base` 再重新构建。

以 Nginx 为例：

```nginx
location / {
    root /var/www/doc-temp/docs/.vitepress/dist;
    try_files $uri $uri.html $uri/ =404;
}
```

### Netlify

构建命令使用 `pnpm run docs:build`，发布目录使用 `docs/.vitepress/dist`。Netlify 识别到 VitePress 后通常会自动填入，确认配置无误后重新部署即可。

## 参考

- [VitePress 官方文档](https://vitepress.dev/zh/)
- [vitepress-sidebar 配置说明](https://github.com/jooy2/vitepress-sidebar)
