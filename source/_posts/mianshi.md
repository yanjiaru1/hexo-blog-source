---
title: mianshi
date: 2026-03-24 13:42:04
tags:html

---

1.SVG在web开发中如何使用，特别是在图标icon方面的应用？

2.是否了解过Canvas技术进行开发

3.是否了解过HTML Canvas?如何在Canvas上绘制一个圆？

4.解析一下浏览器async 、prefetch 和preload等标签的解析和作用？

5.HTML声明是什么意思

6.HTML的结构

7.描述HTML文档的渲染过程

8.解析语义化标签的使用方法，并举例说明

9.SVG和Canvas有什么区别？

10.比较Cookie、localStorage 和sessionStorage的区别以及各自的使用场景？

11.flex布局、概念、属性、给了个场景题让用flex布局实现

12.三栏布局

13.Docoment对象的操作和方法

14.BFC概念及触发方式

15.H5中的块元素和行内块元素

16.比较Canvas和SVG在绘图方面的优缺点和使用场景，并举例说明

17.用h5和js实现伪元素类似效果，这两种方法有什么区别

18.HTML语义化的好处，常用的语义化标签有哪些

19.link标签和import语句在引入外部资源时有什么区别

20.在HTML文件中，，如何正确的引入外部css文件？引入位置的选择会影响什么？

21.如果在HTML中不设置’device-width‘属性，会发生什么问题

22.viewprot属性可以设置为哪些值？这些值分别对应着什么含义？

23.meta标签的作用是什么？他在网页中扮演着什么角色？

24.HTML元素可以分为哪几类

25.Scirpt标签的加载顺序是怎样的，defer和async属性有什么作用和区别？

26.是否有H5移动端页面开发经验？

27.浏览器在获取到一个URL地址后，是如何渲染页面的？

28.SVG与Canvas在渲染机制、性能表现以及各自的潜在优劣方面有何区别？

29.如何使一个HTML元素具备拖拽（draggable）功能？

24.手写代码

---

## 1. SVG在web开发中如何使用，特别是在图标icon方面的应用？

### 使用方式

**内联方式（推荐用于图标）**
```html
<svg width="24" height="24" viewBox="0 0 24 24">
  <path fill="currentColor" d="M12 2L2 7l10 5 10-5-10-5z"/>
</svg>
```

**img标签引入**
```html
<img src="icon.svg" alt="icon">
```

**background-image**
```css
.icon {
  background-image: url('icon.svg');
}
```

**object/embed**
```html
<object type="image/svg+xml" data="icon.svg"></object>
```

### 图标系统最佳实践

**SVG Sprite（图标精灵）**
```html
<!-- 定义符号库 -->
<svg style="display:none">
  <symbol id="icon-home" viewBox="0 0 24 24">
    <path d="M12 2L2 7l10 5 10-5-10-5z"/>
  </symbol>
  <symbol id="icon-user" viewBox="0 0 24 24">
    <path d="M12 12c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4z"/>
  </symbol>
</svg>

<!-- 使用 -->
<svg class="icon"><use href="#icon-home"></use></svg>
```

**优势**：
- 矢量无损缩放
- CSS控制颜色（`fill: currentColor`）
- 体积小
- 支持动画

---

## 2. 是否了解过Canvas技术进行开发

**了解**。Canvas是HTML5提供的2D绘图API，通过JavaScript进行像素级绘制。

### 核心特点
- 即时模式绘图
- 像素级控制
- 适合动态、高频更新场景

### 常见应用
- 数据可视化（图表）
- 游戏开发
- 图像处理
- 动画效果

---

## 3. 是否了解过HTML Canvas？如何在Canvas上绘制一个圆？

**了解**。绘制圆的方法：

```html
<canvas id="myCanvas" width="400" height="400"></canvas>

<script>
  const canvas = document.getElementById('myCanvas');
  const ctx = canvas.getContext('2d');
  
  // 方法1：基本画圆
  ctx.beginPath();
  ctx.arc(200, 200, 100, 0, Math.PI * 2);
  ctx.fillStyle = 'red';
  ctx.fill();
  ctx.strokeStyle = 'blue';
  ctx.lineWidth = 3;
  ctx.stroke();
  
  // 方法2：带渐变
  const gradient = ctx.createRadialGradient(200, 200, 0, 200, 200, 100);
  gradient.addColorStop(0, 'yellow');
  gradient.addColorStop(1, 'orange');
  ctx.fillStyle = gradient;
  ctx.fill();
  
  // 方法3：半圆
  ctx.beginPath();
  ctx.arc(100, 100, 80, 0, Math.PI);
  ctx.fill();
</script>
```

---

## 4. 解析浏览器async、prefetch和preload等标签的解析和作用？



## async、prefetch 和 preload 的区别与作用

这三个都是用于优化网页资源加载的属性，但它们的工作方式和适用场景完全不同。

## 1. **async (异步加载)**

### 作用
异步加载脚本，下载过程中不阻塞 HTML 解析，下载完成后立即执行。

### 使用方式
```html
<script src="script.js" async></script>
```

### 执行时机
- 下载与 HTML 解析并行进行
- 下载完成后**立即暂停 HTML 解析**，执行脚本
- 脚本执行完毕后继续解析 HTML

### 适用场景
- **独立的、不依赖其他脚本的第三方代码**
- 统计代码（如 Google Analytics）
- 广告脚本
- 不操作 DOM 或操作无关紧要的脚本

### 特点
- ✅ 不阻塞 HTML 解析
- ⚠️ 执行顺序不确定（谁先下载完谁先执行）
- ⚠️ 不适合有依赖关系的脚本

---

## 2. **preload (预加载)**

### 作用
**当前页面**必需的资源，告诉浏览器尽快下载，优先级高。

### 使用方式
```html
<!-- HTML 中 -->
<link rel="preload" href="style.css" as="style">
<link rel="preload" href="font.woff2" as="font" crossorigin>

<!-- 或通过 HTTP Header -->
Link: <https://example.com/font.woff2>; rel=preload; as=font; crossorigin
```

### as 属性常见值
- `script` - JavaScript 文件
- `style` - CSS 文件
- `font` - 字体文件（必须加 crossorigin）
- `image` - 图片
- `fetch` - fetch/xhr 请求
- `document` - 文档

### 执行时机
- 浏览器**立即**以高优先级下载
- 下载后暂存在内存中，**不执行**
- 页面需要时直接使用

### 适用场景
- **关键资源**（首屏字体、CSS、JS）
- 字体文件（避免 FOIT/FOUT）
- 关键 CSS/JS
- 首屏大图

### 特点
- ✅ 强制浏览器提前下载
- ✅ 高优先级
- ⚠️ 需要配合实际使用（如 link 标签引用或代码动态加载）

---

## 3. **prefetch (预获取)**

### 作用
提示浏览器在**空闲时间**下载**未来页面**可能用到的资源。

### 使用方式
```html
<!-- 预获取下一页的资源 -->
<link rel="prefetch" href="next-page.js">
<link rel="prefetch" href="next-page.css">

<!-- DNS 预解析 -->
<link rel="dns-prefetch" href="https://api.example.com">

<!-- 预连接（DNS+TCP+TLS） -->
<link rel="preconnect" href="https://api.example.com">
```

### 执行时机
- 浏览器**空闲时**以**最低优先级**下载
- 下载后缓存，供后续页面使用

### 适用场景
- **下一页**需要的资源
- 用户可能点击的链接
- 非关键、可延迟的资源
- 预加载 API 数据

### 特点
- ✅ 利用空闲带宽
- ✅ 提升后续页面加载速度
- ⚠️ 当前页面不立即使用
- ⚠️ 优先级最低

---

## 对比总结

| 特性         | async          | preload          | prefetch         |
| ------------ | -------------- | ---------------- | ---------------- |
| **用途**     | 异步执行脚本   | 预加载当前页资源 | 预获取下一页资源 |
| **加载时机** | 立即并行下载   | 立即高优先级下载 | 空闲时下载       |
| **执行时机** | 下载完立即执行 | 使用时执行       | 下个页面使用     |
| **优先级**   | 中             | 高               | 低               |
| **阻塞渲染** | 执行时阻塞     | 不阻塞           | 不阻塞           |
| **典型场景** | 第三方统计     | 字体、关键CSS    | 下一页资源       |

---

## 实际使用示例

### 完整优化方案
```html
<!DOCTYPE html>
<html>
<head>
  <!-- 预加载关键资源 -->
  <link rel="preload" href="critical.css" as="style">
  <link rel="preload" href="main.js" as="script">
  <link rel="preload" href="font.woff2" as="font" crossorigin>
  
  <!-- 预连接第三方域名 -->
  <link rel="preconnect" href="https://api.example.com">
  
  <!-- 预获取下一页资源 -->
  <link rel="prefetch" href="next-page.html">
  <link rel="prefetch" href="next-page.js">
  
  <!-- 关键 CSS 立即使用 -->
  <link rel="stylesheet" href="critical.css">
  
  <!-- 异步加载统计脚本 -->
  <script src="analytics.js" async></script>
</head>
<body>
  <!-- 页面内容 -->
  
  <!-- 非关键脚本延迟加载 -->
  <script>
    // 动态加载非关键脚本
    if ('requestIdleCallback' in window) {
      requestIdleCallback(() => {
        const script = document.createElement('script');
        script.src = 'non-critical.js';
        document.body.appendChild(script);
      });
    }
  </script>
</body>
</html>
```

---

## 最佳实践建议

1. **关键资源用 preload**
   - 字体、关键 CSS、首屏图片

2. **第三方脚本用 async**
   - 确保不依赖其他脚本

3. **下一页资源用 prefetch**
   - 预测用户行为，提前加载

4. **第三方域名用 preconnect**
   - 减少 DNS 查询和连接时间

5. **避免过度使用**
   - 过多 preload 会争抢带宽
   - 按需使用，不要滥用

合理使用这三个属性可以显著提升页面加载速度和用户体验！

---

## 5. HTML声明是什么意思

**DOCTYPE声明**（Document Type Declaration）告诉浏览器当前页面使用的HTML版本。

```html
<!DOCTYPE html>  <!-- HTML5声明 -->
```

### 作用
- 触发**标准模式**渲染（避免怪异模式）
- 告诉浏览器如何解析HTML
- 缺失会导致CSS盒模型等行为异常

---

## 6. HTML的结构

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>页面标题</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <header><!-- 页眉 --></header>
  <main><!-- 主体内容 --></main>
  <footer><!-- 页脚 --></footer>
  <script src="script.js"></script>
</body>
</html>
```

---

## 7. 描述HTML文档的渲染过程

```
1. 解析HTML → 构建DOM树
2. 解析CSS → 构建CSSOM树
3. 合并DOM + CSSOM → 渲染树(Render Tree)
4. 布局(Layout) → 计算几何位置
5. 绘制(Paint) → 填充像素
6. 合成(Composite) → 图层合并显示
```

**关键点**：
- CSS阻塞渲染
- JS阻塞解析
- 关键渲染路径优化

---

## 8. 解析语义化标签的使用方法，并举例说明

语义化标签使用**有意义的标签**来描述内容。

```html
<!-- ❌ 非语义化 -->
<div class="header"></div>
<div class="nav"></div>
<div class="article"></div>

<!-- ✅ 语义化 -->
<header>
  <nav>
    <ul>
      <li><a href="/">首页</a></li>
    </ul>
  </nav>
</header>

<main>
  <article>
    <h1>文章标题</h1>
    <section>
      <h2>章节标题</h2>
      <p>内容段落</p>
    </section>
  </article>
</main>

<aside>
  <h3>侧边栏</h3>
</aside>

<footer>
  <address>联系方式</address>
</footer>
```

---

## 9. SVG和Canvas有什么区别？

| 特性     | SVG               | Canvas         |
| -------- | ----------------- | -------------- |
| **类型** | 矢量图            | 位图           |
| **渲染** | 保留模式          | 即时模式       |
| **DOM**  | 每个元素是DOM节点 | 无节点，像素级 |
| **缩放** | 无损              | 模糊           |
| **性能** | 元素多时慢        | 适合大量对象   |
| **事件** | 支持事件绑定      | 需坐标计算     |
| **适用** | 图标、地图、图表  | 游戏、图像处理 |

---

## 10. 比较Cookie、localStorage和sessionStorage的区别

| 特性         | Cookie         | localStorage     | sessionStorage  |
| ------------ | -------------- | ---------------- | --------------- |
| **容量**     | 4KB            | 5-10MB           | 5-10MB          |
| **生命周期** | 可设置过期时间 | 永久（手动清除） | 标签页关闭清除  |
| **作用域**   | 同源 + 路径    | 同源             | 同源 + 同标签页 |
| **通信**     | 自动携带请求头 | 不自动           | 不自动          |
| **使用场景** | 会话管理、认证 | 长期存储配置     | 临时表单数据    |

---

## 11. flex布局、概念、属性

### 概念
Flexbox是一维布局模型，用于在行或列方向上分配空间。

### 容器属性
```css
.container {
  display: flex;
  flex-direction: row | column;
  justify-content: center | space-between | flex-start;
  align-items: center | stretch;
  flex-wrap: wrap;
  gap: 20px;
}
```

### 项目属性
```css
.item {
  flex: 1;           /* 占据剩余空间比例 */
  align-self: center;
  order: 1;
}
```

### 场景题示例
**需求**：实现导航栏，左右两侧元素，中间居中

```html
<nav class="navbar">
  <div class="logo">Logo</div>
  <div class="nav-links">
    <a href="#">首页</a>
    <a href="#">关于</a>
  </div>
  <div class="user">用户</div>
</nav>

<style>
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
}
.nav-links {
  display: flex;
  gap: 20px;
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
}
/* 或使用margin:auto */
.logo { margin-right: auto; }
.user { margin-left: auto; }
</style>
```

---

## 12. 三栏布局

### 方法1：Flexbox
```css
.container {
  display: flex;
}
.left { width: 200px; }
.center { flex: 1; }
.right { width: 200px; }
```

### 方法2：Grid
```css
.container {
  display: grid;
  grid-template-columns: 200px 1fr 200px;
}
```

### 方法3：浮动（经典）
```css
.left { float: left; width: 200px; }
.right { float: right; width: 200px; }
.center { margin: 0 200px; }
```

### 方法4：定位
```css
.container { position: relative; }
.left { position: absolute; left: 0; width: 200px; }
.right { position: absolute; right: 0; width: 200px; }
.center { margin: 0 200px; }
```

---

## 13. Document对象的操作和方法

```javascript
// 获取元素
document.getElementById('id')
document.querySelector('.class')
document.querySelectorAll('div')
document.getElementsByClassName('class')

// 创建元素
const div = document.createElement('div')
const text = document.createTextNode('文本')

// 操作DOM
parent.appendChild(child)
parent.removeChild(child)
element.insertBefore(newNode, referenceNode)
element.replaceChild(newNode, oldNode)

// 属性操作
element.getAttribute('src')
element.setAttribute('class', 'active')
element.classList.add('active')

// 事件
element.addEventListener('click', handler)
```

---

## 14. BFC概念及触发方式

**BFC**（Block Formatting Context）块级格式化上下文，是CSS渲染中的独立区域。

### 触发方式
```css
/* 1. overflow不为visible */
overflow: hidden | auto | scroll

/* 2. 浮动 */
float: left | right

/* 3. 定位 */
position: absolute | fixed

/* 4. display */
display: inline-block | flex | grid | table-cell

/* 5. 根元素 */
html
```

### 作用
- 清除浮动
- 防止margin重叠
- 防止元素被浮动元素覆盖

---

## 15. H5中的块元素和行内块元素

### 块级元素
```html
<div>, <p>, <h1>-<h6>, <ul>, <li>, <header>, <footer>, <section>
```
- 独占一行
- 可设置宽高
- margin/padding 上下生效

### 行内元素
```html
<span>, <a>, <strong>, <em>, <img>, <input>, <button>
```
- 不换行
- 不可设置宽高
- margin/padding 水平生效

### 行内块元素
```css
display: inline-block;
```
- 兼具两者特点：不换行但可设置宽高

---

## 16. 比较Canvas和SVG在绘图方面的优缺点和使用场景

### Canvas
**优点**：
- 性能好，适合大量图形
- 像素级控制
- 适合动画和游戏

**缺点**：
- 分辨率依赖
- 无法事件绑定
- 文本渲染能力弱

**场景**：游戏、图表、实时数据可视化、图像滤镜

### SVG
**优点**：
- 无损缩放
- DOM操作友好
- 支持事件和交互
- 易于修改

**缺点**：
- 大量元素时性能差
- 不适合复杂动画

**场景**：图标系统、地图、数据可视化（少量数据）、流程图

---

## 17. 用H5和JS实现伪元素类似效果

```html
<div class="box" data-tooltip="提示文本">内容</div>

<style>
/* CSS伪元素方式 */
.box::before {
  content: "★";
  color: gold;
}

/* JS模拟伪元素效果 */
.js-before {
  position: relative;
}
.js-before::before {
  content: none; /* 禁用原有伪元素 */
}
</style>

<script>
// JS动态创建伪元素效果
const box = document.querySelector('.box');
const beforeElement = document.createElement('span');
beforeElement.textContent = '★';
beforeElement.style.cssText = `
  color: gold;
  margin-right: 5px;
`;
box.prepend(beforeElement);
</script>
```

### 区别
| 方面    | CSS伪元素 | JS模拟   |
| ------- | --------- | -------- |
| 性能    | 更好      | 一般     |
| 动态性  | 需改类    | 完全可控 |
| DOM污染 | 无        | 增加节点 |
| 交互    | 有限      | 完全支持 |

---

## 18. HTML语义化的好处

### 好处
1. **SEO优化** - 搜索引擎更好理解页面结构
2. **可访问性** - 屏幕阅读器更好导航
3. **代码可维护** - 结构清晰易读
4. **跨平台兼容** - 更统一的解析

### 常用语义化标签
```html
<header>    <!-- 页眉 -->
<nav>       <!-- 导航 -->
<main>      <!-- 主要内容 -->
<article>   <!-- 独立内容 -->
<section>   <!-- 章节 -->
<aside>     <!-- 侧边栏 -->
<footer>    <!-- 页脚 -->
<figure>    <!-- 插图 -->
<figcaption><!-- 插图说明 -->
<time>      <!-- 时间 -->
<address>   <!-- 联系方式 -->
<mark>      <!-- 高亮 -->
```

---

## 19. link标签和import语句的区别

| 特性         | link       | @import                     |
| ------------ | ---------- | --------------------------- |
| **加载时机** | 并行加载   | 串行加载（等待主CSS加载完） |
| **兼容性**   | 所有浏览器 | IE5+                        |
| **JS控制**   | 可动态修改 | 不可动态操作                |
| **性能**     | 更好       | 较差（阻塞渲染）            |
| **用法**     | HTML标签   | CSS规则                     |

```html
<!-- link方式（推荐） -->
<link rel="stylesheet" href="style.css">

<!-- @import方式（不推荐） -->
<style>
  @import url('style.css');
</style>
```

---

## 20. 如何正确引入外部CSS文件？

```html
<!-- ✅ 正确位置：<head>中 -->
<head>
  <link rel="stylesheet" href="style.css">
  <link rel="stylesheet" href="critical.css" media="print"> <!-- 打印样式 -->
  <link rel="preload" as="style" href="non-critical.css">
</head>
```

### 位置影响
- **放在head中**：避免FOUC（无样式内容闪烁），渐进式渲染
- **放在body底部**：会看到页面先无样式后突然变化，体验差

---

## 21. 如果不设置device-width会发生什么问题？

```html
<!-- 缺失viewport设置 -->
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

### 问题
1. **移动端默认视口宽度980px**，页面缩小显示
2. **字体过小**，需要用户缩放
3. **响应式失效**，媒体查询基于980px
4. **触控目标太小**，体验差

---

## 22. viewport属性可以设置哪些值？

```html
<meta name="viewport" content="
  width=device-width,           <!-- 视口宽度 = 设备宽度 -->
  initial-scale=1.0,            <!-- 初始缩放比例 -->
  minimum-scale=1.0,            <!-- 最小缩放 -->
  maximum-scale=1.0,            <!-- 最大缩放 -->
  user-scalable=no,             <!-- 是否允许缩放 -->
  viewport-fit=cover            <!-- 适配刘海屏 -->
">
```

---

## 23. meta标签的作用是什么？

meta标签提供HTML文档的**元数据**。

### 主要作用
```html
<!-- 字符编码 -->
<meta charset="UTF-8">

<!-- 视口配置 -->
<meta name="viewport" content="width=device-width">

<!-- SEO优化 -->
<meta name="description" content="页面描述">
<meta name="keywords" content="关键词">
<meta name="author" content="作者">

<!-- 浏览器兼容 -->
<meta http-equiv="X-UA-Compatible" content="IE=edge">

<!-- 移动端优化 -->
<meta name="format-detection" content="telephone=no">
<meta name="apple-mobile-web-app-capable" content="yes">
```

---

## 24. HTML元素可以分为哪几类

### 按显示类型
- **块级元素** - div, p, h1
- **行内元素** - span, a, strong
- **行内块元素** - img, input, button

### 按闭合方式
- **闭合标签** - `<div></div>`
- **空标签** - `<img>`, `<br>`, `<input>`

### 按语义
- **结构性** - header, nav, main
- **文本性** - p, span, em
- **嵌入性** - img, video, canvas

### 按内容模型
- **元数据** - meta, link, title
- **流内容** - 大多数body内元素
- **段落型内容** - p, h1
- **嵌入式内容** - img, video

---

## 25. Script标签的加载顺序

### 默认情况
```html
<script src="script.js"></script>  <!-- 阻塞解析，顺序执行 -->
```

### defer属性
```html
<script src="script.js" defer></script>
```
- 并行下载
- DOMContentLoaded前执行
- 保持顺序

### async属性
```html
<script src="script.js" async></script>
```
- 并行下载
- 下载完立即执行
- 不保证顺序

### 加载策略对比
```html
<head>
  <!-- 关键脚本：阻塞解析 -->
  <script src="critical.js"></script>
  
  <!-- 不阻塞解析 -->
  <script src="async.js" async></script>
  <script src="defer.js" defer></script>
</head>
<body>
  <!-- 推荐位置：body底部 -->
  <script src="footer.js"></script>
</body>
```

---

## 26. 是否有H5移动端页面开发经验？

**有经验**。移动端开发要点：

### 适配方案
```html
<!-- viewport设置 -->
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<!-- 1px边框问题 -->
.border {
  position: relative;
}
.border::after {
  content: '';
  position: absolute;
  bottom: 0;
  width: 100%;
  height: 1px;
  transform: scaleY(0.5);
  background: #ccc;
}

<!-- rem适配 -->
<script>
  function setRem() {
    const width = document.documentElement.clientWidth;
    document.documentElement.style.fontSize = width / 37.5 + 'px';
  }
</script>
```

### 常用库
- Vant、Ant Design Mobile
- 手势库（hammer.js）
- 滚动库（Better-scroll）

---

## 27. 浏览器渲染页面流程

```
1. DNS解析 → 获取IP
2. TCP连接（三次握手）
3. 发送HTTP请求
4. 接收响应（HTML/CSS/JS）
5. 解析HTML → DOM树
6. 解析CSS → CSSOM树
7. 执行JS（可能阻塞）
8. 构建渲染树
9. 布局（重排）
10. 绘制（重绘）
11. 合成显示
```

### 性能优化点
- 减少重排重绘
- 关键资源内联
- 懒加载非关键资源
- 使用transform代替top/left

---

## 28. SVG与Canvas渲染机制区别

| 维度         | SVG              | Canvas           |
| ------------ | ---------------- | ---------------- |
| **渲染模式** | 保留模式         | 即时模式         |
| **对象存储** | 维护DOM树        | 不保存，直接绘制 |
| **重绘**     | 修改属性自动重绘 | 需清空重绘全部   |
| **GPU加速**  | 不支持           | 可通过transform  |
| **内存占用** | 随元素数增长     | 相对固定         |
| **动画性能** | 元素多时卡顿     | 流畅             |

---

## 29. 如何使一个HTML元素具备拖拽功能？

### 原生HTML5拖拽
```html
<div id="dragElement" draggable="true" ondragstart="drag(event)">
  拖拽我
</div>
<div id="dropZone" ondrop="drop(event)" ondragover="allowDrop(event)">
  放置区域
</div>

<script>
function drag(event) {
  event.dataTransfer.setData("text", event.target.id);
  event.dataTransfer.effectAllowed = "copy";
}

function allowDrop(event) {
  event.preventDefault();
  event.dataTransfer.dropEffect = "copy";
}

function drop(event) {
  event.preventDefault();
  const data = event.dataTransfer.getData("text");
  const dragElement = document.getElementById(data);
  event.target.appendChild(dragElement);
}
</script>
```

### JS实现自定义拖拽
```javascript
const dragBox = document.getElementById('dragBox');
let isDragging = false;
let offsetX, offsetY;

dragBox.addEventListener('mousedown', (e) => {
  isDragging = true;
  offsetX = e.clientX - dragBox.offsetLeft;
  offsetY = e.clientY - dragBox.offsetTop;
  dragBox.style.cursor = 'grabbing';
});

document.addEventListener('mousemove', (e) => {
  if (!isDragging) return;
  dragBox.style.left = (e.clientX - offsetX) + 'px';
  dragBox.style.top = (e.clientY - offsetY) + 'px';
});

document.addEventListener('mouseup', () => {
  isDragging = false;
  dragBox.style.cursor = 'grab';
});
```

### 拖拽库
- **SortableJS** - 列表拖拽排序
- **Dragula** - 简单拖拽
- **interact.js** - 高级手势

---

30.手写代码：一个高度固定为200px,宽度自适应的盒子，盒子内部水平排列三个块级元素，左中右，左边宽度固定，右边宽度固定，中间宽度自适应，所有元素自适应

31.HTML行块元素

32.如何使用媒体查询、FLEX布局和rem单位来实现移动端页面的适配？说明vm/vh的作用？

33.页面加载资源阻塞

34.HTMLscript标签的作用和用法

35.DOMContentLoaded和Load事件有什么区别？

36.defer和async

37.如何使用Dom元素操作，向父元素添加一千个内容相同的元素？

38.为什么把script标签放在body的最后面

39.h5界面怎末限制不能放大缩小

40.浏览器的渲染原理、流程

41.HTML文本颜色变化检测

42.用Jscript代码实现获取页面上指定ID的DOM元素

43.获取DOM元素

44.如何向兄弟节点中插入一个新的DOM节点

45.描述一下iframe及其潜在的缺点

46.如何下载图片

47.js会阻塞html和css的渲染吗？为什么？为什么css不会阻塞？

48.浏览器存储

49.a标签有哪些属性

50.head标签里面有哪些标签

51.HTML结构中JS什么时候渲染，defer和async

52.src和href的区别

53.在一个原生的客户端容器中打开H5页面，怎末才能做到让用户打开网页的时候没有任何白屏，可以马上看到网页的内容？

54.设计一个不依赖任何第三方库的图片懒加载方案，并说明其实现原理和关键步骤。

55.使用Canvas实现了图像截取，能否更详细的介绍Canvas的具体实现步骤和方法？

56.如何使用原生js实现图像截取功能，用于上传头像，详细说明实现步骤？

57.如果将cookie数据放到localStorage中，会有什么影响？

58.浏览器有哪些存储数据的方式？

59.解释一下浏览器的重绘和重排

60.介绍一下前端三件套的基础知识

61.页面渲染时候，哪些情况需要用GPU

62.如何进行离线存储

63.DOM树是如何构建的

64.HTML的本质是什么

您说得对，我将以面试回答的形式，结合文字叙述来重新解答剩余问题：

---

## 30. 三栏布局实现（高度固定200px，左右固定宽度，中间自适应）

这道题考察的是经典的圣杯/双飞翼布局思想。我会从以下几个方案来回答：

**核心思路**：左右两侧宽度固定，中间区域自动填充剩余空间。

**方案一：Flexbox（推荐）**
这是最简洁的实现方式。父容器设置 `display: flex`，左右两侧设置固定宽度（如150px），中间设置 `flex: 1` 即可自动占据剩余空间。这种方式代码量最少，逻辑最清晰，也是目前生产环境最常用的方案。

**方案二：Grid布局**
父容器设置 `display: grid` 和 `grid-template-columns: 150px 1fr 150px`。`1fr` 表示占据一份剩余空间，和 flex 的 `flex:1` 原理类似。Grid 在处理二维布局时更强大，但一维场景下 flex 足够。

**方案三：浮动布局（传统方案）**
左侧左浮动，右侧右浮动，中间设置左右 margin 为两侧宽度。这种方式需要处理清除浮动的问题，且中间元素在 DOM 结构中必须放在最后，否则会出现布局错乱。这是 CSS2 时代的经典方案，现在已较少使用。

**方案四：绝对定位**
父容器相对定位，左右两侧绝对定位分别贴左右边界，中间设置左右 margin。这种方案的缺点是父容器高度无法被内容撑开，且绝对定位会脱离文档流，需要谨慎使用。

在实际开发中，我会优先选择 Flexbox 方案，因为它语义清晰、兼容性好、维护成本低。

---

## 31. HTML行块元素

这个问题考察对 CSS 盒模型和 display 属性的理解。

HTML 元素根据显示特性可以分为三类：

**行内元素**：典型代表有 span、a、strong、em 等。它们的特点是多个元素可以在同一行排列，宽度和高度由内容决定，无法通过 CSS 设置宽高，margin 和 padding 的上下方向不生效。通常用于包裹文本或小图标。

**块级元素**：典型代表有 div、p、h1-h6、ul、li 等。它们独占一行，宽度默认填满父容器，可以设置宽高和四方向边距。用于页面结构划分和内容分区。

**行内块元素**：典型代表有 img、input、button、select 等。它们结合了两者特点：不换行排列，但可以设置宽高和完整边距。常用于表单控件和媒体元素。

这三类元素可以通过 CSS 的 `display` 属性相互转换。理解它们的区别对于处理布局问题非常重要，比如常见的垂直居中、行内元素间距等问题都与元素类型有关。

---

## 32. 移动端适配方案

移动端适配是 H5 开发的核心问题，我会从三个层面来回答：

**媒体查询**：通过 `@media` 根据不同屏幕宽度应用不同样式。这是响应式设计的基础，可以针对手机、平板、桌面设置断点。但缺点是每个断点都要写样式，维护成本较高，适合结构相对固定的页面。

**Flex 弹性布局**：flex 是实现流式布局的关键技术。通过 `flex-wrap: wrap` 和 `flex` 属性实现元素的自适应排列，避免固定宽度带来的溢出问题。配合 `gap` 属性可以优雅处理间距。

**rem 适配**：rem 是相对于根元素字体大小的单位。核心思路是通过 JS 动态设置根元素 `font-size`，使所有使用 rem 的元素等比例缩放。通常以设计稿宽度（如 375px）为基准，设置根字体为 100px，开发时直接按设计稿尺寸除以 100 即可得到 rem 值。这种方案可以实现真正的等比缩放。

**vw/vh 的作用**：vw 是视口宽度的 1%，vh 是视口高度的 1%。它们的优势是完全基于视口，不需要 JS 计算。比如 `width: 100vw` 就是占满屏幕宽度，`font-size: 5vw` 可以让字体随屏幕大小变化。缺点是不适合所有场景，某些浏览器对 vw 的处理不够精细，通常与 rem 配合使用。

在实际项目中，我通常采用 rem + vw 混合方案：布局使用 rem，字体和间距使用 vw，兼顾精度和流畅度。

---

## 33. 页面加载资源阻塞

这个问题考察对浏览器渲染机制的理解。

**CSS 的阻塞特性**：CSS 会阻塞渲染，但不会阻塞 DOM 解析。浏览器为了避免出现无样式内容闪烁，会在 CSSOM 构建完成前暂停渲染。同时，CSS 会阻塞后续 JavaScript 的执行，因为 JS 可能会查询元素的样式信息。

**JavaScript 的阻塞特性**：默认情况下，`<script>` 标签会阻塞 DOM 解析。浏览器遇到 script 标签时会立即下载并执行，期间页面解析暂停。这就是为什么我们通常把脚本放在 body 底部的原因。

**解决方案**：通过 `async` 和 `defer` 属性可以改变脚本的加载行为。`async` 是下载完成后立即执行，不保证顺序；`defer` 是下载完成后等待 DOM 解析完毕再执行，保持顺序。现代开发中，关键脚本可以用 defer 放在 head 中，非关键脚本可以动态加载。

**图片和字体**：通常不阻塞渲染，但会触发重绘。字体加载可能导致 FOIT（字体闪烁），常用 `font-display: swap` 来优化体验。

---

## 34. HTML script 标签的作用和用法

script 标签用于在 HTML 中嵌入或引用 JavaScript 代码。我会从几个角度说明：

**基本用法**：可以通过内联方式在标签内写代码，也可以通过 `src` 属性引入外部文件。外部文件的优势在于可缓存、可复用。

**关键属性**：
- `async`：异步下载，下载完成后立即执行，适合独立的第三方脚本
- `defer`：异步下载，但等待 DOM 解析完成后按顺序执行，适合依赖 DOM 的脚本
- `type="module"`：将脚本视为 ES6 模块，默认具有 defer 行为，支持 import/export
- `integrity`：用于子资源完整性校验，确保 CDN 资源未被篡改
- `crossorigin`：处理跨域资源加载

**动态创建**：可以通过 `document.createElement('script')` 动态创建脚本，这种方式的优势是可以按需加载，常用于代码分割和懒加载。

---

## 35. DOMContentLoaded 和 Load 事件的区别

这两个事件是页面加载过程中的重要里程碑。

**DOMContentLoaded**：当 HTML 文档被完全解析，DOM 树构建完成后触发，此时不需要等待样式表、图片等资源加载。这是操作 DOM 的最早时机，通常在这个事件中初始化页面交互。

**Load**：当所有资源（图片、样式、字体、iframe 等）都加载完成后触发。如果需要在资源加载后执行某些操作，比如获取图片尺寸或等待第三方 SDK 初始化，应该监听 load 事件。

**执行顺序**：永远是 DOMContentLoaded 先触发，load 后触发。在页面性能优化中，我们应该尽量将非关键操作放在 load 事件中，避免阻塞首屏渲染。

还有一个 `readystatechange` 事件，document.readyState 会经历 loading → interactive → complete 三个阶段，interactive 状态与 DOMContentLoaded 大致对应。

---

## 36. defer 和 async 的深入对比

这两个属性用于控制外部脚本的加载行为，是性能优化的关键。

**共同点**：都是异步下载脚本，不会阻塞 HTML 解析。

**差异**：
- `async`：下载完成后立即执行，执行时会暂停 HTML 解析。多个 async 脚本的执行顺序不确定，谁先下载完谁先执行。适合与 DOM 无关的独立脚本，如统计代码、广告脚本。
- `defer`：下载完成后不立即执行，而是等待 HTML 完全解析完成后，在 DOMContentLoaded 事件之前按顺序执行。适合需要操作 DOM 或有依赖关系的脚本。

**选择原则**：
- 如果脚本需要操作 DOM 且不依赖其他脚本，用 defer
- 如果脚本完全独立且不关心顺序，用 async
- 如果脚本必须在加载时立即执行（如 polyfill），不加属性，放在 head 最前面

现代前端构建工具通常会为入口文件自动添加 `defer`，配合代码分割实现最优加载。

---

## 37. 向父元素添加大量相同元素

这个问题考察 DOM 操作性能优化的知识。

**问题所在**：在循环中频繁操作 DOM 会导致大量重排重绘，每次 `appendChild` 都会触发布局计算，严重影响性能。

**优化方案一：DocumentFragment**。创建一个虚拟的文档片段，在内存中完成所有元素的添加，最后一次性插入到 DOM 中。这样只触发一次重排。

**优化方案二：字符串拼接 + innerHTML**。将 HTML 字符串拼接后一次性赋值给 innerHTML。这种方式性能很好，但需要注意 XSS 安全问题。

**优化方案三：数组 join 法**。比字符串拼接更高效，因为数组 join 内部实现更优。

**优化方案四：分批渲染**。使用 `requestAnimationFrame` 或 `setTimeout` 分批添加元素，避免长时间阻塞主线程，提升页面响应性。

**性能对比**：1000 个元素场景下，DocumentFragment 比循环添加快约 10 倍，innerHTML 方式最快。如果元素数量极大（如 10000+），还需要考虑虚拟滚动等技术。

---

## 38. 为什么把 script 标签放在 body 的最后面

这是一个经典的性能优化问题，主要有三个原因：

**原因一：避免阻塞 DOM 解析**。浏览器解析 HTML 时遇到 script 标签会暂停解析，下载并执行 JS。如果脚本放在 head 中，会导致页面长时间白屏。放在底部时，页面内容可以先显示出来。

**原因二：确保 DOM 已加载**。放在底部的脚本执行时，前面的 DOM 元素都已经解析完成，可以直接访问，不需要监听 DOMContentLoaded 事件。

**原因三：提升用户体验**。用户可以更快看到页面内容，减少感知等待时间。

**现代替代方案**：随着 defer 和 async 属性的普及，现在更推荐将脚本放在 head 中并添加 defer，既能让浏览器提前下载脚本，又不会阻塞渲染。这样可以充分利用带宽，比单纯放在底部更优。

---

## 39. H5 界面如何限制缩放

移动端有时需要禁用用户缩放，以保持界面完整性。

**主要方法**：通过 viewport meta 标签设置 `user-scalable=no` 和 `maximum-scale=1.0`。这样用户就无法通过双指手势缩放页面。

**注意事项**：在某些 iOS 浏览器中，`user-scalable=no` 可能无效，这是出于可访问性考虑（视力障碍用户需要缩放）。此时可以通过 `touch-action` CSS 属性进行控制，或者监听 touchmove 事件并判断手势进行阻止。

**推荐做法**：除非有明确需求（如游戏、特定交互页面），否则不建议禁用缩放。现代移动端页面应该适配不同屏幕，而不是限制用户。如果确实需要禁用，应该配合其他无障碍措施。

---

## 40. 浏览器的渲染原理和流程

这是前端核心知识点，我会按顺序阐述：

**第一步：构建 DOM 树**。浏览器解析 HTML 字节流，经过字节→字符→Token→节点→DOM 树的过程。这个过程中如果遇到 script 标签且没有 defer/async，会阻塞解析。

**第二步：构建 CSSOM 树**。解析 CSS 文件或 style 标签，构建 CSS 对象模型。CSS 不会阻塞 DOM 解析，但会阻塞渲染，因为需要完整的 CSSOM 才能绘制。

**第三步：构建渲染树**。合并 DOM 和 CSSOM，过滤掉不可见元素（如 head、display:none），生成渲染树。

**第四步：布局（Layout/Reflow）**。计算每个节点在屏幕上的精确位置和大小，输出盒模型信息。这是一个递归过程，会遍历整个渲染树。

**第五步：分层（Layer）**。浏览器根据层叠上下文将页面分层，如根层、滚动层、transform 层等。独立层可以单独处理，提升性能。

**第六步：绘制（Paint）**。将每个层的每个像素填充，生成绘制指令列表。

**第七步：分块与光栅化**。将图层划分为图块，转换为位图。这个过程通常在 GPU 中完成。

**第八步：合成（Composite）**。将所有图层合并显示在屏幕上。合成过程在 GPU 中进行，效率很高。

**关键概念**：重排（Reflow）是布局属性的改变，开销最大；重绘（Repaint）是外观属性的改变，开销次之；合成是 transform 和 opacity 的改变，只消耗 GPU 资源，性能最好。

---

## 41. HTML 文本颜色变化检测

这个问题考察对 DOM 监听机制的理解。

**MutationObserver** 是检测 DOM 变化的推荐方式。可以监听元素的属性变化，包括 style 属性的改变。当颜色发生变化时，回调函数会被触发，可以获取变化前后的颜色值。

**CSS 过渡检测**：如果颜色是通过过渡效果改变的，可以监听 `transitionend` 事件，根据 `propertyName` 判断是否是颜色变化。

**事件触发检测**：如果颜色是用户交互导致的，可以在触发颜色变化的代码处同步执行检测逻辑。

**应用场景**：这种检测常用于调试工具、UI 自动化测试或特殊交互需求。实际开发中，更推荐通过状态管理来跟踪样式变化，而不是后置检测。

---

## 42-43. DOM 元素获取方法

这两个问题可以合并回答：

获取 DOM 元素有多种 API，各有特点：

**getElementById**：最快最精确的方式，按 ID 获取单个元素。ID 必须唯一，否则只返回第一个。

**querySelector / querySelectorAll**：使用 CSS 选择器语法，灵活性最高。querySelector 返回第一个匹配，querySelectorAll 返回静态 NodeList。这是现代开发中最常用的方式。

**getElementsByClassName / getElementsByTagName**：返回动态 HTMLCollection，会随着 DOM 变化实时更新。性能比 querySelector 稍好，但使用起来不如后者灵活。

**选择建议**：
- 有 ID 优先用 getElementById
- 复杂选择用 querySelector
- 需要实时更新集合用 getElementsByClassName

**注意事项**：getElementsBy 系列返回的 HTMLCollection 是实时的，使用时要小心遍历时 DOM 变化导致的问题。querySelectorAll 返回的是静态快照，更安全。

---

## 44. 向兄弟节点中插入新节点

这个问题考察 DOM 树操作 API。

**核心方法**：`insertBefore` 是 DOM 提供的原生方法，可以在参考节点之前插入新节点。要在参考节点之后插入，需要先找到父节点和参考节点的下一个兄弟节点。

**便捷方案**：`insertAdjacentElement` 方法更灵活，通过位置参数（beforebegin、afterbegin、beforeend、afterend）可以精确控制插入位置。其中 `afterend` 就是插入到兄弟节点之后。

**封装函数**：可以封装 `insertAfter` 函数，内部处理 nextSibling 的边界情况，如果没有下一个兄弟则用 appendChild。

**实际应用**：列表项插入、动态添加表单元素等场景经常用到。相比 appendChild，兄弟节点插入需要更精确的位置控制。

---

## 45. iframe 及其缺点

iframe 是内嵌框架，用于在页面中嵌入另一个 HTML 文档。

**主要缺点**：

**性能问题**：每个 iframe 都是一个独立的文档环境，需要加载自己的资源、维护自己的 DOM 树，内存开销大。多个 iframe 会显著增加页面加载时间，且会阻塞主页面的 load 事件。

**SEO 不友好**：搜索引擎的爬虫很难索引 iframe 中的内容，因为内容来自不同的 URL。这对网站排名有负面影响。

**安全风险**：跨域 iframe 通信需要谨慎处理 postMessage，容易产生安全漏洞。父页面和 iframe 之间的交互需要验证来源。

**交互体验差**：移动端 iframe 的滚动体验很差，内容往往无法完美适配屏幕。响应式设计在 iframe 内部实现困难。

**调试困难**：iframe 内部的错误调试、样式调试都比普通页面复杂。

**替代方案**：现代开发中，可以用 AJAX 动态加载内容、微前端框架或 Web Components 来替代 iframe 的使用场景。

---

## 46. 如何下载图片

图片下载主要涉及文件流处理和触发下载动作。

**核心思路**：创建一个指向图片资源的链接，通过 `<a>` 标签的 `download` 属性触发下载，而不是打开图片预览。

**实现方式**：
- 同源或允许跨域设置的图片，可以直接使用 a 标签 download 属性
- 跨域图片可以通过 Canvas 转换为 DataURL，再触发下载，但需要设置 crossOrigin 属性
- 更通用的方案是通过 fetch 获取图片的 Blob 数据，创建对象 URL，然后触发下载，最后释放 URL

**注意事项**：
- download 属性仅对同源 URL 或 blob/data URL 生效
- 跨域图片通过 Canvas 转换时会受 CORS 限制，需要服务端配合设置响应头
- 大文件下载要考虑内存问题，避免一次性读取整个文件

---

## 47. JS 会阻塞 HTML 和 CSS 的渲染吗？

**JS 与 HTML 的关系**：默认情况下，JS 会阻塞 HTML 解析。当浏览器遇到普通 script 标签时，会暂停 DOM 构建，下载并执行脚本，之后才继续解析。这是为了确保脚本执行时 DOM 结构是确定的。

**JS 与 CSS 的关系**：JS 会阻塞 CSS 渲染，但这里有个微妙的关系——CSS 也会阻塞 JS 执行。因为 JS 可能查询样式信息，浏览器会确保 CSSOM 构建完成后才执行 JS。这形成了一个依赖链：CSS → JS → DOM解析。

**CSS 为什么不阻塞 HTML**：CSS 不会阻塞 DOM 解析，因为浏览器可以一边解析 HTML 构建 DOM，一边下载 CSS。但 CSS 会阻塞渲染，因为浏览器需要完整的 CSSOM 才能确定每个元素的最终样式。

**优化策略**：通过 defer/async 改变 JS 加载行为，将关键 CSS 内联，非关键 CSS 延迟加载，可以减少阻塞对首屏的影响。

---

## 48. 浏览器存储方式

这个问题考察对不同存储方案的了解，我会从应用场景的角度阐述：

**Cookie**：容量最小（4KB），但最大的特点是会自动携带在 HTTP 请求头中。因此 Cookie 适合存储需要随请求发送的信息，如 session ID、认证 token。为了安全，应该设置 HttpOnly、Secure、SameSite 等属性。

**localStorage**：容量约 5-10MB，永久存储，除非手动清除。适合存储用户偏好设置、主题、语言等不敏感的数据。注意它不能跨域共享，且没有过期时间。

**sessionStorage**：容量同 localStorage，但生命周期是标签页级别，关闭标签页即清除。适合存储临时数据，如表单草稿、页面状态等。

**IndexedDB**：容量很大（几百 MB 甚至更多），支持存储大量结构化数据，提供事务和索引功能。适合离线应用、大量数据缓存、文件存储等复杂场景。

**CacheStorage**：Service Worker 配套的缓存 API，用于精细化控制 HTTP 缓存。可以实现离线可用、自定义缓存策略等高级功能。

**选择原则**：小数据用 localStorage，临时数据用 sessionStorage，认证用 Cookie，大数据用 IndexedDB，离线缓存用 CacheStorage。

---

## 49. a 标签的属性

a 标签是 HTML 中最基本的链接元素，我主要关注几个重要属性：

**href**：最重要的属性，定义链接目标。可以是 URL、锚点、JavaScript 代码（`javascript:void(0)`）或 `#`（空链接）。

**target**：指定打开方式，常用 `_blank` 新窗口打开。使用 `_blank` 时建议配合 `rel="noopener noreferrer"`，防止新页面通过 `window.opener` 访问原页面，避免安全风险。

**download**：告诉浏览器下载而不是导航，需要配合同源资源或 blob URL 使用。

**rel**：定义当前文档与链接文档的关系，常见值有 `noopener`（安全）、`nofollow`（SEO）、`noreferrer`（不发送 referer）。

**其他属性**：`hreflang` 指定目标语言，`type` 指定 MIME 类型，`media` 指定适用设备，`ping` 用于点击跟踪。

---

## 50. head 标签里的内容

head 是文档头部，存放元数据和资源引用，不直接显示在页面中。

**基础元数据**：`<title>` 定义页面标题，显示在浏览器标签页；`<meta charset="UTF-8">` 声明字符编码，必须放在最前面；`<base>` 设置相对 URL 的基准路径。

**SEO 相关**：description（页面描述）、keywords（关键词）、author（作者）、robots（爬虫指令）等 meta 标签。

**资源链接**：`<link>` 用于引入 CSS、设置 favicon、预加载资源。常用 rel 值有 stylesheet（样式）、icon（图标）、preload（预加载）、preconnect（预连接）、dns-prefetch（DNS预解析）。

**脚本**：`<script>` 可以放关键的内联脚本或引用外部文件，通常配合 defer 避免阻塞。

**移动端配置**：viewport 配置响应式布局，format-detection 控制电话号码识别，apple-mobile-web-app-capable 支持添加到主屏幕。

---

## 51. JS 什么时候渲染

JS 的渲染时机取决于加载方式和位置。

**普通 script**：在 HTML 解析到该标签时立即下载并执行，期间页面渲染暂停。这就是为什么 script 放在不同位置会影响渲染时机。

**async script**：异步下载，下载完成后立即执行。执行时机不确定，可能发生在 DOM 解析的任何阶段，但一定会暂停渲染。

**defer script**：异步下载，但执行时机是 DOM 解析完成后、DOMContentLoaded 事件之前。执行时不会阻塞渲染，因为所有 DOM 已经就绪。

**动态脚本**：通过 createElement 创建的 script 标签默认是异步行为，下载完成后立即执行。

**模块化脚本**：`type="module"` 的脚本默认具有 defer 行为，且支持按需加载和依赖管理。

在实际开发中，理解这些执行时机对于性能优化和避免竞态条件非常重要。

---

## 52. src 和 href 的区别

src 和 href 都是用于引用外部资源的属性，但语义和行为完全不同。

**src（source）**：表示嵌入资源，用于替换当前内容。浏览器遇到 src 时会暂停解析，下载并执行/渲染资源。常见于 script、img、iframe 等元素。src 的内容会成为页面的一部分。

**href（hypertext reference）**：表示引用关系，用于建立当前文档与目标资源的关联。浏览器遇到 href 时不会暂停解析，只是建立一种链接关系。常见于 link、a 等元素。

**行为差异**：最直观的区别是，script 用 src 会阻塞页面，而 link 用 href 不会阻塞（但 CSS 会阻塞渲染）。另外，src 的必须性更强，缺少 src 的 img 是无意义的，而 a 标签可以没有 href（只是普通文本）。

---

## 53. 如何避免 H5 页面白屏

白屏是 H5 页面的常见问题，我会从多个角度阐述优化策略：

**关键资源优化**：将首屏必需的 CSS 内联在 head 中，避免等待外部 CSS 加载。非关键 CSS 延迟加载。这样浏览器可以立即开始渲染，而不是等待网络请求。

**骨架屏**：在页面完全加载前，先展示一个占位框架。骨架屏能给用户明确的反馈，减少白屏的感知时间。可以在 HTML 中直接写骨架屏 HTML 结构，页面加载后替换为真实内容。

**预加载关键资源**：通过 `<link rel="preload">` 告知浏览器提前下载关键资源，如字体、首屏图片、核心 JS。preload 优先级高，能充分利用网络空闲时间。

**服务端渲染 SSR**：在服务端完成页面渲染，直接返回完整的 HTML，浏览器可以直接显示，不需要等待 JS 执行。这是解决白屏最彻底的方案，但会带来服务端成本。

**CDN 加速**：将静态资源部署到 CDN，减少网络延迟。配合 HTTP/2 的多路复用，可以并行加载多个资源。

**渐进式加载**：页面内容分层加载，先展示文字和基本框架，图片和复杂组件再异步加载。配合懒加载，让用户更快看到内容。

---

## 54. 图片懒加载方案

图片懒加载的核心思想是：只有当图片进入视口时才加载真实图片，减少初始加载资源。

**Intersection Observer API**：这是现代浏览器推荐的实现方式。创建一个观察器，监听图片元素与视口的交叉状态。当图片进入可视区域时，将 `data-src` 的值赋给 `src` 属性。这种方式的优势是性能好，不需要手动监听滚动事件，由浏览器底层优化。

**滚动监听方案**：作为 Intersection Observer 的降级方案。监听 scroll 事件，通过 `getBoundingClientRect` 判断图片位置。需要注意使用节流函数控制触发频率，避免滚动时频繁计算。

**实现要点**：
- 图片初始时设置占位图或空白，真实地址存储在 `data-src` 属性
- 加载完成后可以添加 loaded 类，用于渐显效果
- 需要考虑图片加载失败的处理
- 对于 SSR 场景，需要保证懒加载逻辑不破坏服务端渲染

**应用场景**：图片列表、长页面内容、无限滚动等。懒加载可以显著减少首屏加载时间，节省用户流量。

---

## 55-56. Canvas 实现图像截取

这两个问题都是关于 Canvas 图像处理，我会从原理到实现进行阐述。

**基本原理**：Canvas 提供了 `drawImage` 方法，可以将原图的部分区域绘制到目标 Canvas 上，实现截取效果。关键在于理解源坐标和目标坐标的对应关系。

**实现步骤**：
1. 使用 `FileReader` 或 Image 对象加载用户选择的图片
2. 在 Canvas 上绘制原图
3. 实现交互选择区域（通常通过鼠标事件获取起始点和结束点）
4. 创建新的 Canvas，使用 `drawImage` 的九参数版本截取选择区域
5. 通过 `toDataURL` 或 `toBlob` 导出截取结果

**交互难点**：
- 坐标转换：Canvas 的实际像素尺寸与 CSS 显示尺寸需要映射，鼠标坐标要转换成 Canvas 坐标
- 拖拽选择：需要处理 mousedown、mousemove、mouseup 事件，实时绘制选择框
- 可调整选择框：高级功能还需支持拖拽调整选区大小和位置

**头像裁剪特殊要求**：头像通常是圆形，需要在导出时进行圆形裁剪。可以通过 `clip` 方法设置圆形裁剪区域，或使用 Canvas 合成技术将图像绘制到圆形路径中。

**性能优化**：大图截取时，可以直接在用户选择后绘制到目标尺寸，避免处理整个原图。使用 `requestAnimationFrame` 优化拖拽时的绘制性能。

---

## 57. Cookie 放到 localStorage 的影响

这是一个安全相关的问题，我会从多个角度说明为什么这样做是危险的。

**安全风险**：Cookie 可以设置 `HttpOnly` 属性，这样 JavaScript 无法读取，有效防止 XSS 攻击窃取 token。而 localStorage 完全暴露在 JS 环境中，任何 XSS 漏洞都能轻易读取存储的数据。一旦 token 泄露，攻击者可以完全接管用户账户。

**自动携带机制**：Cookie 会自动携带在每个同源请求中，服务端通过请求头获取。如果换成 localStorage，需要手动添加到请求头，增加了代码复杂度，也容易遗漏。

**过期管理**：Cookie 有原生的过期机制，可以设置有效期。localStorage 没有过期概念，需要自己实现时间戳管理，容易出现 token 失效后仍然发送的问题。

**跨域限制**：Cookie 可以设置 Domain 实现子域共享，localStorage 受同源策略限制更严格。

**正确做法**：认证 token 必须使用 Cookie + HttpOnly + Secure + SameSite 组合，确保安全性。localStorage 只适合存储非敏感数据，如用户偏好设置、主题等。

---

## 58. 浏览器存储方式（补充）

在之前回答的基础上，我会补充每种存储的技术细节：

**Cookie**：每次 HTTP 请求都会携带，这既是优点也是缺点。优点是服务端自动获取，缺点是增加请求体积。适合存储 session ID 等小数据。现代开发建议使用 HttpOnly 和 SameSite 增强安全性。

**localStorage / sessionStorage**：Web Storage API 的两种实现，提供了 `setItem`、`getItem`、`removeItem` 等简单接口。同步 API，操作简单，但会阻塞主线程。适合存储字符串类型的数据，对象需要 JSON 序列化。

**IndexedDB**：异步 API，支持事务、索引、游标等数据库特性。可以存储大量结构化数据，包括二进制文件（Blob）。适合离线应用、大量数据缓存、本地数据库等场景。使用起来相对复杂，但能力最强。

**CacheStorage**：Service Worker 的核心 API，用于缓存网络请求的响应。可以实现离线可用、自定义缓存策略。配合 fetch 事件拦截，可以实现精细化的资源管理。

**File System Access API**：新兴的本地文件系统 API，允许网页读写用户本地文件，能力更强但兼容性有限。

---

## 59. 重绘和重排

这是浏览器渲染性能的核心概念。

**重排（Reflow）**：当元素的几何属性（宽度、高度、位置、边距等）发生变化时，浏览器需要重新计算元素及其影响元素的布局信息，这个过程叫重排。重排的开销最大，因为它会影响整个文档流。

**重绘（Repaint）**：当元素的外观属性（颜色、背景、阴影、可见性等）发生变化，但布局不变时，浏览器只需要重新绘制这个元素，不涉及布局计算。重绘的开销比重排小。

**触发重排的操作**：
- 添加、删除、修改 DOM 元素
- 修改元素的几何属性
- 改变窗口大小
- 获取某些布局信息（offsetTop、scrollTop、getComputedStyle 等）

**优化策略**：
- 批量修改样式，使用 `classList` 一次性应用多个样式
- 使用 `transform` 代替 `top/left` 做动画，transform 只触发合成
- 离线操作：先克隆元素，修改后再替换
- 使用 `requestAnimationFrame` 批量处理动画帧
- 避免频繁读取布局信息，将读操作和写操作分离

理解重排和重绘的机制对于写出高性能的页面非常重要，尤其是在复杂动画和大数据列表场景中。

---

## 60. 前端三件套基础知识

前端三件套是 HTML、CSS、JavaScript，我会阐述它们的角色和关系：

**HTML（超文本标记语言）**：定义页面结构和内容，是页面的骨架。HTML5 引入了语义化标签（header、article、section 等）、多媒体元素（video、audio）、Canvas 绘图等新特性。HTML 关注的是"有什么"。

**CSS（层叠样式表）**：控制页面的视觉表现，是页面的皮肤。CSS3 带来了 Flexbox、Grid 布局、动画、过渡、响应式设计等能力。CSS 关注的是"长什么样"。

**JavaScript**：实现页面交互和动态逻辑，是页面的灵魂。ES6+ 引入了模块化、Promise、async/await、类等现代特性。JavaScript 关注的是"做什么"。

**三者的关系**：HTML 提供内容结构，CSS 负责美化布局，JS 赋予交互能力。三者分离是前端开发的核心原则，有利于维护和协作。现代前端开发在此基础上引入了组件化、工程化、框架等概念，但本质还是这三者的组合应用。

---

## 61. GPU 加速的应用场景

GPU 加速是指利用图形处理器来渲染某些 CSS 属性，减轻 CPU 负担，提升动画性能。

**哪些情况会启用 GPU 加速**：
- `transform` 属性（translate、scale、rotate 等）
- `opacity` 属性
- `filter` 属性
- `will-change` 属性主动提示浏览器
- 3D 变换（translate3d、translateZ 等）
- 视频播放

**为什么这些属性会启用 GPU**：浏览器会为使用这些属性的元素创建独立的图层，图层的合成工作在 GPU 中完成，不会影响其他图层。而改变 left、top 等属性会导致整个文档流重排，无法利用 GPU。

**性能提升原理**：
- 重排（Reflow）在 CPU 中计算几何信息
- 重绘（Repaint）在 CPU 中填充像素
- 合成（Composite）在 GPU 中合并图层

**使用建议**：动画优先使用 transform 和 opacity，用 `will-change` 提前告知浏览器需要优化，但不要滥用（会增加内存占用）。

---

## 62. 如何进行离线存储

离线存储让网页在没有网络时也能访问，主要通过 Service Worker 实现。

**核心原理**：Service Worker 是一个独立于页面的 JavaScript 工作线程，可以拦截网络请求，从缓存中返回响应，实现离线访问。

**实现步骤**：
1. 注册 Service Worker：在主页面中检查浏览器支持并注册
2. 安装阶段（install 事件）：预缓存关键资源（HTML、CSS、JS、离线页面）
3. 激活阶段（activate 事件）：清理旧缓存
4. 请求拦截（fetch 事件）：实现缓存策略，如 Cache First、Network First 等

**缓存策略**：
- Cache First：优先从缓存返回，失败再请求网络，适合静态资源
- Network First：优先请求网络，失败再使用缓存，适合需要最新数据的 API
- Stale While Revalidate：先返回缓存，同时更新缓存，适合平衡体验和时效性

**注意事项**：
- Service Worker 需要 HTTPS 或 localhost 才能运行
- 作用域控制，只能拦截作用域内的请求
- 更新机制复杂，需要版本管理

**PWA（渐进式网页应用）** 就是基于 Service Worker 的完整解决方案，包括离线可用、推送通知、添加到主屏幕等特性。

---

## 63. DOM 树是如何构建的

DOM 树的构建是浏览器渲染的第一步，我会阐述完整的解析过程。

**字节流处理**：浏览器从网络或本地获取 HTML 字节流（如 `3C 68 74 6D 6C 3E`），根据字符编码（如 UTF-8）解码为字符。

**词法分析（Tokenization）**：将字符流解析为一个个 Token，包括开始标签（`<html>`）、结束标签（`</html>`）、属性（`class="box"`）、文本内容等。

**语法分析**：根据 Token 构建节点对象。遇到开始标签就创建对应的 DOM 节点，遇到文本就创建文本节点。

**树形构建**：使用栈结构维护节点关系。遇到开始标签入栈，遇到结束标签出栈，形成正确的嵌套结构。

**关键点**：
- 解析是渐进式的，边下载边解析
- 遇到普通 script 会暂停解析，等待脚本执行
- CSS 不阻塞解析，但会阻塞渲染
- 图片、字体等资源异步加载，不阻塞解析

**性能优化**：减少 HTML 嵌套深度、避免复杂的文档结构、将 script 放在合适位置，都能加快 DOM 树的构建速度。

---

## 64. HTML 的本质是什么

这是一个开放性问题，可以从多个维度理解。

**从技术角度**：HTML 是一种标记语言，不是编程语言。它通过标签来描述文档的结构和语义，浏览器根据这些标记进行解析和渲染。

**从 Web 角度**：HTML 是万维网的核心基础，定义了网页内容的组织方式。它与其他技术（HTTP、URL、CSS、JS）共同构成了 Web 技术栈。

**从设计角度**：HTML 体现了关注点分离的原则，将内容与样式、行为分离，让不同专业的人员（内容编辑、设计师、开发者）能够协作。

**从语义角度**：HTML 不仅告诉浏览器如何显示内容，更赋予内容意义。语义化标签让机器（搜索引擎、屏幕阅读器）理解内容结构，实现无障碍访问和 SEO。

**从发展角度**：HTML 从最初简单的学术文档格式，发展到今天支持多媒体、图形、离线存储的完整应用平台，见证了 Web 从静态文档到动态应用的演进。

**总结来说**，HTML 是 Web 内容的基石，是连接人类与信息、技术与设计的桥梁，其本质是"结构化信息"的标准化表达。

65.input标签有哪些类型的属性值

66.如何使用js操作dom

67.对html5有哪些了解

68.能详细介绍一下html中的语义化标签吗

69.html获取到如何渲染出来

70.常见的行内元素有哪些？常见的块级元素有哪些？

71.解释一下html语义化

72.Dom树是如何渲染的

73.html5新增了哪些属性和标签

74.如果将script标签不放在body底部，如何确保脚本在页面加载完成后执行

75.script标签放在body前后有什么区别

76.css会阻塞页面加载吗？js呢

77.html的defer和async机制是哪些？domcontentload又是什么？

78.href和url有什么区别？

79.浏览器遇到link标签会发生什么

80.crossorigin=‘anonymous’属性的作用是什么

81.看到一个h5页面，作为一个前端开发，你关注哪些方面

82.meta标签的作用是什么？viewport属性又是什么？device-width属性又是什么？

83.在h5中开发与在pc中开发有什么区别？

84.h5开发要注意哪些问题

85.如何解决浏览器不兼容的问题

86.在浏览器不兼容的情况下，如何给用户一个比较良好的体验？

87.如何给div元素增加class属性

88.script标签会阻塞dom解析吗？css会阻塞吗？

89.async和defer属性的区别是什么？他们会阻塞dom解析吗

90.你了解过link标签的preload属性吗

这是一份全面的 HTML/CSS/浏览器 面试题解答，我将按照你给出的编号逐一回答：

---

## 65. input标签有哪些类型的属性值

**常见 type 属性值：**

| 类型                               | 说明                           |
| ---------------------------------- | ------------------------------ |
| `text`                             | 单行文本输入框（默认）         |
| `password`                         | 密码输入框，内容被掩码         |
| `email`                            | 邮箱地址，移动端会调出对应键盘 |
| `number`                           | 数字输入框，支持步进控制       |
| `tel`                              | 电话号码，移动端调出数字键盘   |
| `url`                              | URL 地址，自带格式校验         |
| `search`                           | 搜索框，部分浏览器有清除按钮   |
| `date` / `time` / `datetime-local` | 日期时间选择器                 |
| `month` / `week`                   | 月份/周选择器                  |
| `color`                            | 颜色选择器                     |
| `range`                            | 滑块控件                       |
| `checkbox`                         | 复选框                         |
| `radio`                            | 单选框                         |
| `file`                             | 文件上传                       |
| `submit`                           | 提交按钮                       |
| `reset`                            | 重置按钮                       |
| `button`                           | 普通按钮                       |
| `image`                            | 图像提交按钮                   |
| `hidden`                           | 隐藏字段                       |

**其他常用属性：** `placeholder`、`required`、`disabled`、`readonly`、`maxlength`、`min`/`max`、`pattern`、`autocomplete`、`autofocus`、`multiple` 等。

---

## 66. 如何使用js操作dom

**DOM 操作常用方法：**

```javascript
// 1. 获取元素
document.getElementById('id')
document.querySelector('.class')
document.querySelectorAll('div')
document.getElementsByClassName('class')
document.getElementsByTagName('div')

// 2. 创建元素
const div = document.createElement('div')
const text = document.createTextNode('文本')

// 3. 添加/插入元素
parent.appendChild(div)
parent.insertBefore(div, referenceNode)
parent.insertAdjacentHTML('beforeend', '<span>html</span>')

// 4. 删除元素
element.remove()
parent.removeChild(child)

// 5. 修改内容/属性
element.innerHTML = '<p>新内容</p>'
element.textContent = '纯文本'
element.setAttribute('class', 'active')
element.classList.add('active')
element.style.color = 'red'

// 6. 事件监听
element.addEventListener('click', handler)
```

---

## 67. 对html5有哪些了解

HTML5 是第五代 HTML 标准，主要新增特性：

1. **语义化标签**：`<header>`、`<nav>`、`<article>`、`<section>`、`<footer>` 等
2. **多媒体支持**：`<video>`、`<audio>` 原生支持音视频
3. **Canvas 和 SVG**：`<canvas>` 绘图，`<svg>` 矢量图形
4. **表单增强**：新的 input 类型（email、date、range 等）和表单属性
5. **本地存储**：`localStorage`、`sessionStorage`
6. **离线应用**：Application Cache、Service Worker
7. **Web Workers**：后台多线程
8. **地理位置**：Geolocation API
9. **拖拽 API**：Drag and Drop
10. **WebSocket**：全双工通信

---

## 68. 能详细介绍一下html中的语义化标签吗

**语义化标签**：用有意义的标签来描述内容，而非单纯用 `<div>` 和 `<span>`。

**常用语义化标签：**

| 标签                        | 含义             |
| --------------------------- | ---------------- |
| `<header>`                  | 页面或区块的头部 |
| `<nav>`                     | 导航区域         |
| `<main>`                    | 页面主要内容     |
| `<article>`                 | 独立的文章/内容  |
| `<section>`                 | 文档中的章节     |
| `<aside>`                   | 侧边栏/附属信息  |
| `<footer>`                  | 页脚             |
| `<h1>-<h6>`                 | 标题层级         |
| `<figure>` + `<figcaption>` | 插图及说明       |
| `<mark>`                    | 高亮文本         |
| `<time>`                    | 时间/日期        |
| `<address>`                 | 联系信息         |

**优点：**
- **SEO 友好**：搜索引擎能更好理解页面结构
- **可访问性**：屏幕阅读器能提供更好的导航
- **代码可读性**：开发者更容易理解结构
- **维护性**：结构清晰，便于维护

---

## 69. html获取到如何渲染出来

浏览器渲染过程：

```
1. 下载 HTML → 解析 HTML → 构建 DOM 树
                    ↓
2. 下载 CSS  → 解析 CSS  → 构建 CSSOM 树
                    ↓
3. DOM + CSSOM = 渲染树（Render Tree）
                    ↓
4. 布局（Layout/Reflow）→ 计算每个节点位置和尺寸
                    ↓
5. 绘制（Paint）→ 填充像素，绘制到屏幕
                    ↓
6. 合成（Composite）→ GPU 合并图层，最终显示
```

**关键点**：
- 遇到 `<script>` 会阻塞解析（除非有 async/defer）
- CSS 不阻塞 DOM 解析，但阻塞渲染和 JS 执行

---

## 70. 常见的行内元素有哪些？常见的块级元素有哪些？

| 类型           | 元素                                                         | 特点                                             |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------ |
| **行内元素**   | `<span>`, `<a>`, `<img>`, `<input>`, `<label>`, `<strong>`, `<em>`, `<br>`, `<button>` | 不换行，宽度由内容决定，不能设置宽高（部分可设） |
| **块级元素**   | `<div>`, `<p>`, `<h1>-<h6>`, `<ul>`, `<li>`, `<header>`, `<footer>`, `<section>`, `<article>`, `<form>` | 独占一行，可设置宽高，默认宽度 100%              |
| **行内块元素** | `<img>`, `<input>`, `<button>`                               | 不换行，但可设置宽高                             |

---

## 71. 解释一下html语义化

**HTML 语义化**：使用恰当的标签来表达内容的含义，而不是仅用 `<div>` 和 `<span>` 包裹所有内容。

**示例对比：**
```html
<!-- 非语义化 -->
<div class="header"></div>
<div class="nav"></div>

<!-- 语义化 -->
<header></header>
<nav></nav>
```

**好处：**
1. **SEO**：搜索引擎爬虫能准确识别内容重要性
2. **无障碍**：辅助技术（屏幕阅读器）能提供更好的导航
3. **可维护**：代码结构清晰，便于团队协作
4. **跨平台**：不同设备能更好地呈现内容

---

## 72. Dom树是如何渲染的

渲染流程详解：

1. **解析 HTML**：浏览器将 HTML 字符串解析成 DOM 树（Document Object Model）
2. **解析 CSS**：同时解析 CSS，构建 CSSOM 树
3. **合并**：DOM + CSSOM = Render Tree（只包含可见元素）
4. **布局（Reflow）**：计算每个节点在视口中的精确位置和大小
5. **绘制（Paint）**：遍历渲染树，调用绘制 API 填充像素
6. **合成（Composite）**：将各层合并，GPU 渲染输出

**优化建议**：
- 避免频繁读写布局属性
- 使用 `transform` 和 `opacity` 触发合成，避免重排
- 减少 DOM 层级

---

## 73. html5新增了哪些属性和标签

**新增语义化标签**：`<header>`、`<nav>`、`<main>`、`<article>`、`<section>`、`<aside>`、`<footer>`

**新增多媒体标签**：`<video>`、`<audio>`、`<source>`、`<track>`

**新增图形标签**：`<canvas>`、`<svg>`

**新增表单标签**：`<input>` 新增 type、`<datalist>`、`<output>`、`<progress>`、`<meter>`

**新增全局属性**：
- `contenteditable`：元素可编辑
- `data-*`：自定义数据属性
- `hidden`：隐藏元素
- `draggable`：可拖拽
- `spellcheck`：拼写检查

---

## 74. 如果将script标签不放在body底部，如何确保脚本在页面加载完成后执行

**方法：**

```html
<!-- 1. 使用 defer 属性 -->
<script src="script.js" defer></script>

<!-- 2. 使用 async 属性（适合独立脚本） -->
<script src="analytics.js" async></script>

<!-- 3. 监听 DOMContentLoaded 事件 -->
<script>
  document.addEventListener('DOMContentLoaded', function() {
    // DOM 加载完成后执行
  });
</script>

<!-- 4. 监听 load 事件（所有资源加载完成后） -->
<script>
  window.addEventListener('load', function() {
    // 所有资源（图片、样式等）加载完成后执行
  });
</script>

<!-- 5. 动态创建脚本 -->
<script>
  const script = document.createElement('script');
  script.src = 'script.js';
  script.onload = function() { /* 执行 */ };
  document.head.appendChild(script);
</script>
```

---

## 75. script标签放在body前后有什么区别

| 位置                  | 特点                    | 影响                                 |
| --------------------- | ----------------------- | ------------------------------------ |
| **放在 `<head>` 中**  | 在 DOM 解析前下载和执行 | 会阻塞页面渲染，可能导致白屏时间延长 |
| **放在 `</body>` 前** | DOM 解析完成后执行      | 页面内容先显示，用户体验更好         |

**现代最佳实践**：
- 关键脚本使用 `defer` 放在 `<head>` 中
- 第三方统计脚本使用 `async`
- 非关键脚本放在 `<body>` 底部

---

## 76. css会阻塞页面加载吗？js呢

**CSS：**
- 不阻塞 DOM 解析
- 阻塞页面渲染（渲染树需要 CSSOM）
- 阻塞 JS 执行（JS 执行前会等待 CSS 加载）

**JS：**
- 默认阻塞 DOM 解析（遇到 `<script>` 停止解析，等待 JS 下载执行）
- 阻塞页面渲染（JS 执行期间不渲染）
- `async`：下载不阻塞，执行时阻塞
- `defer`：下载不阻塞，DOM 解析完成后执行

---

## 77. html的defer和async机制是哪些？domcontentload又是什么？

**async 和 defer 对比：**

| 属性    | 下载时机     | 执行时机                     | 顺序       |
| ------- | ------------ | ---------------------------- | ---------- |
| 无      | 遇到立即下载 | 下载后立即执行，阻塞解析     | 按文档顺序 |
| `async` | 异步下载     | 下载后立即执行，可能阻塞解析 | 不保证顺序 |
| `defer` | 异步下载     | DOM 解析完成后执行           | 按文档顺序 |

**DOMContentLoaded**：
- 当 HTML 完全加载解析完成（DOM 树构建完成）
- 不等待 CSS、图片、iframe 等资源加载
- 触发时机：`defer` 脚本执行后

**对比 load 事件：**
- `DOMContentLoaded`：DOM 就绪
- `load`：所有资源（图片、样式等）都加载完成

---

## 78. href和url有什么区别？

**URL**：统一资源定位符（Uniform Resource Locator），是资源地址的统称。

**href**：超文本引用（Hypertext Reference），是一个**属性**，用于指定资源位置。

**区别**：
- URL 是概念，表示资源地址
- href 是 HTML 属性，值是一个 URL

```html
<!-- href 属性的值是 URL -->
<a href="https://example.com">链接</a>
<link rel="stylesheet" href="style.css">
```

**相关属性**：
- `src`：用于嵌入资源（img、script、iframe），资源是页面的一部分
- `href`：用于建立关联（a、link），资源是引用的关系

---

## 79. 浏览器遇到link标签会发生什么

遇到 `<link rel="stylesheet">` 时：

1. **发起请求**：异步下载 CSS 文件（不阻塞 DOM 解析）
2. **构建 CSSOM**：下载完成后解析 CSS
3. **阻塞渲染**：CSSOM 构建完成前，页面不会渲染
4. **阻塞 JS**：后续 JS 执行前会等待 CSS 加载完成
5. **触发事件**：加载完成后触发 `load` 事件（失败触发 `error`）

**其他 link 用途**：
- `preload`：预加载资源
- `prefetch`：预取下一页面资源
- `icon`：网站图标

---

## 80. crossorigin=‘anonymous’属性的作用是什么

`crossorigin` 属性用于控制跨域资源的请求行为。

**作用**：
1. **启用 CORS 请求**：从其他域获取资源时发送跨域请求
2. **获取错误详情**：配合 `<script>` 可以获取详细的 JavaScript 错误堆栈信息
3. **资源重用**：使资源可以被跨域重用（如 Canvas 中的跨域图片）

**取值**：
- `anonymous`：发送跨域请求，但不携带凭证（cookie）
- `use-credentials`：发送跨域请求，携带凭证

```html
<!-- 获取详细的 JS 错误信息 -->
<script src="https://cdn.example.com/app.js" crossorigin="anonymous"></script>

<!-- 使 canvas 可以读取跨域图片 -->
<img src="https://other-domain.com/img.jpg" crossorigin="anonymous">
```

---

## 81. 看到一个h5页面，作为一个前端开发，你关注哪些方面

**关注维度：**

1. **结构与语义**：HTML 结构是否合理，语义化标签使用是否恰当
2. **性能**：
   - 首屏加载速度
   - 资源大小和数量
   - 图片懒加载
   - 代码分割
3. **兼容性**：
   - 浏览器兼容性
   - 机型适配
4. **响应式/适配**：
   - 移动端适配方案（rem、vw、viewport）
   - 不同屏幕尺寸表现
5. **交互体验**：
   - 动画流畅度
   - 点击区域大小（移动端）
   - 触屏事件处理
6. **SEO**：标题、描述、关键词、语义化标签
7. **可访问性**：ARIA 标签、键盘导航
8. **可维护性**：代码规范、组件化、注释
9. **安全性**：XSS 防护、CSRF 防护
10. **网络优化**：缓存策略、CDN 使用

---

## 82. meta标签的作用是什么？viewport属性又是什么？device-width属性又是什么？

**meta 标签**：提供 HTML 文档的元数据，不显示在页面上，供浏览器和搜索引擎使用。

```html
<meta charset="UTF-8">              <!-- 字符编码 -->
<meta name="description" content=""> <!-- 页面描述，用于 SEO -->
<meta name="keywords" content="">    <!-- 关键词 -->
<meta name="author" content="">      <!-- 作者 -->
```

**viewport**：控制移动端视口的行为

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

| 属性                 | 作用                     |
| -------------------- | ------------------------ |
| `width=device-width` | 视口宽度等于设备屏幕宽度 |
| `initial-scale=1.0`  | 初始缩放比例为 1         |
| `minimum-scale`      | 最小缩放比例             |
| `maximum-scale`      | 最大缩放比例             |
| `user-scalable`      | 是否允许用户缩放         |

**device-width**：设备的物理屏幕宽度（单位是 CSS 像素）

---

## 83. 在h5中开发与在pc中开发有什么区别？

| 方面         | H5（移动端）                                 | PC 端                      |
| ------------ | -------------------------------------------- | -------------------------- |
| **屏幕尺寸** | 多变的尺寸，需要适配各种手机                 | 相对固定，常见宽度 1200px+ |
| **交互方式** | 触摸事件（touch）                            | 鼠标事件（click、hover）   |
| **网络环境** | 移动网络，不稳定，需优化                     | 相对稳定                   |
| **性能**     | 设备性能差异大，需优化                       | 性能较好                   |
| **适配方案** | rem、vw、flex 布局                           | 固定宽度 + 响应式          |
| **浏览器**   | WebView、Safari、Chrome 移动版               | Chrome、Firefox、Edge      |
| **调试**     | 真机调试、vConsole、Chrome DevTools 远程调试 | 浏览器 DevTools            |
| **点击区域** | 至少 44x44 像素                              | 可较小                     |
| **键盘**     | 虚拟键盘会遮挡视图                           | 物理键盘                   |

---

## 84. h5开发要注意哪些问题

1. **视口设置**：正确配置 viewport
2. **适配方案**：选择合适的适配方案（rem、vw、flex）
3. **点击延迟**：使用 `touch-action: manipulation` 或 fastclick 解决 300ms 延迟
4. **1px 边框问题**：使用 transform scale 或 viewport + rem 方案
5. **图片优化**：WebP 格式、懒加载、CDN
6. **性能优化**：减少首屏资源、代码分割、缓存
7. **触摸事件**：处理好 touchstart、touchmove、touchend
8. **软键盘问题**：键盘弹起收起影响布局
9. **安全区域**：适配刘海屏、全面屏（safe-area-inset）
10. **兼容性**：CSS 前缀、API 降级方案
11. **资源加载**：预加载、骨架屏
12. **调试**：真机调试方案

---

## 85. 如何解决浏览器不兼容的问题

1. **CSS 前缀**：使用 autoprefixer 自动添加 `-webkit-`、`-moz-` 等前缀
2. **Polyfill**：为旧浏览器提供缺失的功能
   ```html
   <script src="https://polyfill.io/v3/polyfill.min.js"></script>
   ```
3. **渐进增强**：先保证基础功能，再添加高级特性
4. **优雅降级**：高级功能不支持时，提供替代方案
5. **特性检测**：使用 Modernizr 或手动检测
   ```javascript
   if ('querySelector' in document) {
     // 支持
   }
   ```
6. **CSS Fallback**：提供降级样式
   ```css
   .box {
     display: -webkit-flex;
     display: flex;
   }
   ```
7. **浏览器版本控制**：使用 `.browserslistrc` 配置支持的浏览器
8. **重置样式**：Normalize.css 或 Reset.css

---

## 86. 在浏览器不兼容的情况下，如何给用户一个比较良好的体验？

1. **降级提示**：友好提示，非侵入性
   ```html
   <!--[if lt IE 9]>
     <div class="browser-warning">您的浏览器版本过低，建议升级</div>
   <![endif]-->
   ```

2. **功能降级**：高级功能不可用时，提供基础版本
   - CSS Grid 不支持时用 Flexbox 回退
   - WebP 不支持时使用 JPEG 降级

3. **优雅降级样式**：保证核心内容可读
   ```css
   .container {
     display: flex;        /* 现代浏览器 */
     display: block;       /* 降级方案 */
   }
   ```

4. **性能降级**：低端设备减少动画、简化交互

5. **使用 polyfill**：自动补充缺失 API，但注意性能开销

6. **服务端渲染**：确保核心内容在任何情况下都能显示

7. **确保可访问性**：即使样式加载失败，内容结构依然清晰

---

## 87. 如何给div元素增加class属性

**JavaScript 方式：**

```javascript
// 1. 直接设置 className
const div = document.getElementById('myDiv');
div.className = 'new-class';

// 2. 使用 classList 添加（推荐）
div.classList.add('new-class');
div.classList.add('class1', 'class2'); // 添加多个

// 3. 移除 class
div.classList.remove('old-class');

// 4. 切换 class
div.classList.toggle('active');

// 5. setAttribute
div.setAttribute('class', 'new-class');

// 6. 判断是否存在
if (div.classList.contains('active')) { }
```

**HTML 方式：**
```html
<div class="class1 class2"></div>
```

---

## 88. script标签会阻塞dom解析吗？css会阻塞吗？

**script 标签**：
- 默认**会阻塞** DOM 解析
- 遇到 `<script>` 立即下载并执行，期间停止 HTML 解析
- `async`：下载不阻塞，执行时阻塞
- `defer`：不阻塞，DOM 解析完成后执行

**css 标签**：
- **不直接阻塞** DOM 解析
- **阻塞渲染**（渲染树需要 CSSOM）
- **阻塞 JS 执行**（JS 执行前等待 CSS 加载）
- 当 CSS 在 JS 前面时，CSS 会间接阻塞 DOM 解析

---

## 89. async和defer属性的区别是什么？他们会阻塞dom解析吗

| 对比项               | async                          | defer              |
| -------------------- | ------------------------------ | ------------------ |
| **下载时机**         | 异步下载，不阻塞               | 异步下载，不阻塞   |
| **执行时机**         | 下载完成后立即执行             | DOM 解析完成后执行 |
| **执行顺序**         | 不保证顺序，谁先下载完谁先执行 | 按文档顺序执行     |
| **阻塞 DOM 解析**    | 执行时阻塞                     | 不阻塞             |
| **DOMContentLoaded** | 不等待                         | 等待执行完成后触发 |

**图示流程：**
```
普通 script: [下载]→[执行]→[解析HTML]→...
async script: [解析]→[下载]→[执行]→...
defer script: [解析]→[下载]→[执行]→...
              (解析完成后执行)
```

---

## 90. 你了解过link标签的preload属性吗

**preload**：预加载资源，告诉浏览器当前页面**立即需要**的资源。

```html
<link rel="preload" href="style.css" as="style">
<link rel="preload" href="main.js" as="script">
<link rel="preload" href="font.woff2" as="font" crossorigin>
<link rel="preload" href="image.jpg" as="image">
```

**as 取值**：`script`、`style`、`image`、`font`、`fetch`、`document` 等

**特点：**
- 提高资源加载优先级
- 提前加载关键资源，缩短首屏时间
- 支持 `crossorigin` 属性

**相关属性对比：**

| 属性           | 作用               | 优先级 |
| -------------- | ------------------ | ------ |
| `preload`      | 预加载当前页面资源 | 高     |
| `prefetch`     | 预取下一页面资源   | 低     |
| `preconnect`   | 提前建立连接       | -      |
| `dns-prefetch` | DNS 预解析         | -      |

**使用场景：**
- 字体文件（避免 FOIT）
- 关键 CSS/JS
- 首屏大图
- 动态加载的模块

