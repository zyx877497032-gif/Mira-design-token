# Mira Design Token

Mira 设计系统的 Design Token，基于 W3C DTCG 标准格式，支持 Light / Dark 双主题。

---

## 文件说明

```
├── tokens-fixed.json     # 源 Token 文件（从 Figma 导出后修复）
├── sd.config.js          # Style Dictionary 构建配置
├── package.json
└── dist/                 # 构建产物（运行 npm run build 后生成）
    ├── tokens.base.css   # Tailwind 基础色板 CSS 变量
    ├── tokens.light.css  # Light 主题语义色 CSS 变量
    ├── tokens.dark.css   # Dark 主题语义色 CSS 变量
    └── tokens.js         # JS/TS ESM 对象
```

---

## 快速开始

### 1. 安装依赖

```bash
npm install
```

### 2. 构建 Token

```bash
npm run build
```

---

## 前端接入方式

### 方式一：直接引入 CSS（推荐）

把 `dist/` 目录下的 CSS 文件复制到项目，或通过 npm 引入：

```css
/* 在全局 CSS 或入口文件中引入 */
@import 'mira-design-token/dist/tokens.base.css';
@import 'mira-design-token/dist/tokens.light.css';
@import 'mira-design-token/dist/tokens.dark.css';
```

然后在代码里直接使用 CSS 变量：

```css
.button {
  background-color: var(--theme-light-colors-primary);
  color: var(--theme-light-colors-primary-foreground);
  border-radius: var(--theme-light-radius-md);
}
```

### 方式二：主题切换

Light / Dark 主题切换只需在 `<html>` 或容器元素上切换 `data-theme` 属性：

```html
<!-- Light 模式（默认） -->
<html data-theme="light">

<!-- Dark 模式 -->
<html data-theme="dark">
<!-- 或者 -->
<html class="dark">
```

### 方式三：JS/TS 项目

```typescript
import tokens from 'mira-design-token/dist/tokens.js';

// 使用 token 值
const primaryColor = tokens['Theme/Light'].colors.primary; // '#18181b'
const borderRadius = tokens['Theme/Light'].radius.md;       // 8
```

---

## Token 结构

| Token Set | 说明 |
|-----------|------|
| `Theme/Light` | Light 主题语义色、阴影、字体、尺寸 |
| `Theme/Dark` | Dark 主题语义色、阴影（结构同 Light） |
| `TailwindCSS/Default` | Tailwind 基础色板 + spacing/width/height 等原始值 |
| `Custom/Mode 1` | 排版复合 Token（heading-xl/lg/md/sm） |

### 主要语义色变量（举例）

| 变量名 | 用途 |
|--------|------|
| `--...-colors-background` | 页面背景 |
| `--...-colors-foreground` | 主要文字 |
| `--...-colors-primary` | 主操作色（按钮等） |
| `--...-colors-secondary` | 次要操作色 |
| `--...-colors-muted` | 弱化背景 |
| `--...-colors-border` | 边框色 |
| `--...-colors-destructive` | 危险/错误色 |
| `--...-colors-card` | 卡片背景 |

---

## 注意事项

### 字体

Token 中引用了以下字体，需要前端自行引入：

| Token | 字体 | 获取方式 |
|-------|------|----------|
| `font-sans` | Source Han Sans SC（思源黑体）| [Google Fonts](https://fonts.google.com/noto/specimen/Noto+Sans+SC) 或本地 |
| `font-title` | Source Han Serif SC（思源宋体）| 本地引入 |
| `font-english` | Inter | [Google Fonts](https://fonts.google.com/specimen/Inter) |
| `font-english-title` | Source Serif 4 | [Google Fonts](https://fonts.google.com/specimen/Source+Serif+4) |
| `font-mono` | Geist Mono | [Vercel](https://vercel.com/font) |
| `font-logo` | Geologica | [Google Fonts](https://fonts.google.com/specimen/Geologica) |

### Shadow Token

阴影 token 在源文件中是拆散的子属性（`color`、`offset-x` 等），Style Dictionary 构建时会自动合并为 CSS `box-shadow` 字符串。

---

## 从 Figma 更新 Token

1. 在 Figma 中使用 **Tokens Studio** 插件导出最新 token
2. 替换 `tokens-fixed.json`（注意保留已修复的内容，或重新运行修复脚本）
3. 运行 `npm run build` 重新生成产物
4. 提交到 GitHub
