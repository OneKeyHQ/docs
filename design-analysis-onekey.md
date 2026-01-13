# OneKey 官网设计分析报告

> 分析日期: 2025-11-25
> 分析网站: https://onekey.so/

---

## 核心设计理念

OneKey 官网的设计体现 **"安全、专业、克制"** 的硬件钱包品牌形象。

**关键词**: 黑白极简 | 绿色点缀 | 胶囊按钮 | 大圆角卡片

---

## 1. 色彩系统

### 1.1 主色调 - 黑白为主

| 用途 | 色值 | 说明 |
|------|------|------|
| **深色背景** | `#101111` / `#000` | 主要深色区块 |
| **浅色背景** | `#ffffff` | 主要浅色区块 |
| **主文字 (深色背景)** | `#ffffff` | 白色文字 |
| **主文字 (浅色背景)** | `#393c4e` | 深灰文字 |
| **次级文字** | `#6b7280` / `#9ca3af` | 灰色辅助文字 |

### 1.2 品牌绿色 - 克制使用

| 用途 | 色值 | 使用场景 |
|------|------|----------|
| **品牌绿** | `#00B812` | Logo |
| **亮绿** | `#4FF55F` | CTA 按钮背景 |
| **悬停绿** | `#33C641` | 按钮悬停状态 |

**重要**: 绿色仅用于：
- Logo
- 主要 CTA 按钮 (如 "立即购买")
- 链接悬停状态
- 成功/活跃状态指示

**不要用绿色于**:
- 大面积文字
- 次要按钮
- 卡片背景
- 边框

### 1.3 配色原则

```
黑白灰 (90%) + 绿色点缀 (10%)
```

---

## 2. 按钮系统

### 2.1 按钮形状 - 胶囊形

OneKey 使用 **全圆角胶囊按钮** (`border-radius: 100px`)，这是品牌识别的重要元素。

### 2.2 按钮变体

**Primary (主要按钮) - 绿色填充**
```css
.btn-primary {
  background: #4FF55F;
  color: #000000;
  border-radius: 100px;
  padding: 12px 24px;
  font-weight: 500;
}

.btn-primary:hover {
  background: #000000;
  color: #ffffff;
}
```

**Secondary (次要按钮) - 边框样式**
```css
.btn-secondary {
  background: transparent;
  color: #ffffff;  /* 或 #000 在浅色背景 */
  border: 1px solid currentColor;
  border-radius: 100px;
  padding: 12px 24px;
}

.btn-secondary:hover {
  background: #ffffff;
  color: #000000;
}
```

**Ghost (文字按钮)**
```css
.btn-ghost {
  background: transparent;
  color: inherit;
  padding: 8px 16px;
}

.btn-ghost:hover {
  color: #4FF55F;  /* 悬停变绿 */
}
```

### 2.3 按钮尺寸

| 尺寸 | 高度 | 内边距 | 字号 |
|------|------|--------|------|
| Small | 32px | 8px 16px | 14px |
| Medium | 40px | 12px 24px | 14px |
| Large | 48px | 14px 32px | 16px |

---

## 3. 卡片系统

### 3.1 产品卡片

```css
.product-card {
  border-radius: 24px;
  background: #101111;
  overflow: hidden;
  height: 560px;
  position: relative;
}

.product-card:hover {
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
  transform: translateY(-4px);
}
```

### 3.2 功能卡片

```css
.feature-card {
  border-radius: 16px;
  background: #18181b;  /* 或 #f4f4f5 浅色 */
  padding: 24px;
  border: 1px solid transparent;
}

.feature-card:hover {
  border-color: rgba(255, 255, 255, 0.1);
}
```

---

## 4. 字体系统

### 4.1 字体族

```css
font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto,
             'Helvetica Neue', Arial, sans-serif;
```

### 4.2 字号层级

| 层级 | 字号 | 字重 | 用途 |
|------|------|------|------|
| Display | 48-64px | 700 | Hero 大标题 |
| H1 | 40px | 600 | 页面标题 |
| H2 | 32px | 600 | 章节标题 |
| H3 | 24px | 600 | 子章节 |
| H4 | 20px | 500 | 卡片标题 |
| Body | 16-18px | 400 | 正文 |
| Small | 14px | 400 | 辅助文字 |
| Caption | 12px | 400 | 注释 |

---

## 5. 间距系统

### 5.1 基础单位

采用 **8px** 基础单位：

| Token | 值 | 用途 |
|-------|-----|------|
| `xs` | 8px | 紧凑元素间距 |
| `sm` | 16px | 小间距 |
| `md` | 24px | 标准间距 |
| `lg` | 32px | 大间距 |
| `xl` | 48px | 区块间距 |
| `2xl` | 64px | 页面级间距 |

### 5.2 Section 间距

- Section 上下: 80-120px
- 内容最大宽度: 1200px
- 卡片间距: 24px

---

## 6. 布局特点

### 6.1 深浅交替

OneKey 官网使用深色和浅色背景 **交替排列**，创造视觉节奏：

```
[深色 Hero]
[浅色 产品]
[深色 功能]
[浅色 特性]
[深色 Footer]
```

### 6.2 居中对齐

大部分内容居中，最大宽度 1200px。

---

## 7. 开发者文档应用建议

### 7.1 配色调整

```css
:root {
  /* 主要文字 */
  --text-primary: #1a1a1a;      /* 浅色背景 */
  --text-secondary: #6b7280;

  /* 背景 */
  --bg-light: #ffffff;
  --bg-dark: #101111;
  --bg-muted: #f4f4f5;

  /* 品牌绿 - 仅用于 CTA */
  --brand-green: #00B812;
  --brand-green-bright: #4FF55F;
}
```

### 7.2 按钮使用规则

| 场景 | 按钮类型 |
|------|----------|
| 主要操作 (开始使用、立即购买) | Primary 绿色 |
| 次要操作 (了解更多、GitHub) | Secondary 边框 |
| 文本链接 | Ghost 文字 |
| 导航链接 | 无边框，悬停变绿 |

### 7.3 Landing Page 结构建议

```
[深色 Hero] - 大标题 + 两个按钮 (绿色 + 边框)
[浅色 Integration Cards] - 白底 + 黑色卡片
[深色 Code Example] - 代码块展示
[浅色 Chain Support] - 链支持列表
[深色 Resources] - 开发者资源
```

---

## 8. 关键设计原则

1. **绿色克制**: 仅用于最重要的 CTA，其他用黑白灰
2. **胶囊按钮**: 全圆角是品牌识别
3. **大圆角卡片**: 16-24px，现代感
4. **深浅交替**: 创造视觉节奏
5. **专业克制**: 避免花哨，强调安全专业

---

## 9. Stripe vs OneKey 对比

| 方面 | Stripe | OneKey |
|------|--------|--------|
| 按钮圆角 | 4px (方正) | 100px (胶囊) |
| 主色使用 | 紫色大面积 | 绿色点缀 |
| 卡片圆角 | 8px | 16-24px |
| 整体风格 | 精确专业 | 现代科技 |
| 背景 | 纯白为主 | 黑白交替 |

**结论**: OneKey 开发者文档应该遵循 OneKey 品牌规范，而非完全照搬 Stripe。
