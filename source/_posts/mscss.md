---
title: mscss
date: 2026-03-26 16:55:05
tags: [css]

---

1.描述一下css中的外边距塌陷问题及其解决方案？

2.给定使用flex属性的css样式，分析其中盒子的最终宽度是什么？

3.flexbox布局模型中，常用的属性及其作用是什么

4.如何使用css实现常见的两栏布局，有哪些方法

5.css选择器的优先级规则是怎样的？内联选择器和id选择器哪个优先级更高？

6.如何实现一个元素的垂直居中

7.css有哪些常用的布局方式

8.css的box-sizing属性有哪些值，他们各自对盒模型计算方式有哪些影响？

9.css中有哪些常见的长度单位，他们各自的使用场景和特点是什么

10.css中position属性有哪些常用的值，他们各自的特点和用法是什么

11.css中的相对定位relative和绝对定位absolute有什么区别，他们各自适用于哪些场景？

12.css中有哪些方法可以隐藏一个div元素，他们之间有什么区别

13.如何使用flexbox布局实现一个div在主轴和侧轴上的居中布局

14.阐述bfc（块级格式化上下文）的概念，并提供几个世纪的应用场景和例子？

15.介绍css的flex布局，主轴和交叉轴的概念是什么？如何使用flex布局实现页面排版？

16.是否使用过css media queries？他们的作用是什么？

17.css中 ：：before和：：after伪类如何使用？他们的应用场景是什么？

18.css中实现元素水平居中有哪些常见方法？

19.使用flexbox布局，如何实现一个居中对齐的盒子？

20.css中，如何实现0.5像素的线条

21.css盒模型包含哪些组成部分？标准和模型与怪异模型有什么区别？

22.解释一下css中的伪类与伪元素有什么区别？has()和is()选择器的作用是什么

23.项目中是否使用过css伪类

24.css选择器有哪些种类，他们的优先级规则是怎样的，是否存在可以打破这些优先级的特殊情况？

25.css盒子模型是什么，如何计算其宽度，是否还存在其他盒子模型？

26.css中哪些情况会出现margin重叠问题，如何避免或解决

27.css中如何清除浮动,有哪些常见方法

28.解释一下justify-content和align-items在flexbox布局的作用和区别？

29.描述一下css中flex属性及其常见用法

30.对css盒模型有哪些理解

31.css伪元素和伪类有什么区别？分别有哪些应用场景？

32.解释cssbfc的概念、创建方式及其实际应用

33.详细说明cssflex布局的工作原理和常见应用场景

34.如何使用css实现多余文字的省略号显示？

35.回流reflow和重绘repaint的概念是什么？他们之间有什么区别？

36.在css中，有哪些方法可以实现元素的缩小效果

37.flexbox常用的属性有哪些？

38.对css的理解是什么？在学习css的过程中有没有遇到什么有意思的经历

39.如何使用flexbox实现一个九宫格布局

40.是否遇到过css外边距重叠问题，如何理解和解决

41.如何利用cssgrid布局创建一个九宫格，并绘制出其分割线

42.如何使用css实现左侧定宽，右侧自适应的两栏布局

43.如何实现响应式布局

44.选用css-in-js技术有哪些优势？他在站点换肤场景中是否使用？相比裸css有何不同？

45.在css中如何确保样式不会相互污染？在vue项目中引入外部样式时，如果造成全局污染，通常有哪些解决方案

46.css实现垂直居中的方案有哪些

47.css定位方式有哪些，例如absolute、relative、sticky分别说明

48.css盒模型的工作原理是什么

49.如何实现组件库的主题切换功能

50.css本身没有model概念，如果提及使用cssmodel ，其实现原理是什么？

51.有哪些方法可以解决css样式冲突问题？

这是一份全面的 CSS 面试题解答，我将按照你给出的编号逐一回答：

---

## 1. 描述一下css中的外边距塌陷问题及其解决方案？

**外边距塌陷（Margin Collapse）**：当两个块级元素的垂直外边距相遇时，它们会合并为一个外边距，取两者中的较大值。

**常见场景：**

1. **相邻兄弟元素**：上下两个元素，margin-bottom 和 margin-top 会合并
2. **父子元素**：父元素与第一个/最后一个子元素的 margin-top/margin-bottom 会合并
3. **空块元素**：自身没有内容的块元素，margin-top 和 margin-bottom 会合并

```html
<!-- 相邻兄弟 -->
<div class="box1" style="margin-bottom: 20px;">Box1</div>
<div class="box2" style="margin-top: 30px;">Box2</div>
<!-- 实际间距 30px，不是 50px -->

<!-- 父子 -->
<div class="parent" style="margin-top: 20px;">
  <div class="child" style="margin-top: 30px;"></div>
</div>
<!-- 父元素实际 margin-top 为 30px -->
```

**解决方案：**

| 场景     | 解决方案                                                     |
| -------- | ------------------------------------------------------------ |
| 相邻兄弟 | 只给其中一个设置 margin，或使用 padding 替代                 |
| 父子     | 父元素设置 `overflow: hidden`、`border`、`padding`，或使用 `display: flex` |
| 通用     | 使用 `padding` 替代 `margin`，或使用 `BFC` 隔离              |

```css
/* 解决父子塌陷 */
.parent {
  overflow: hidden; /* 触发 BFC */
  /* 或 */
  border: 1px solid transparent;
  /* 或 */
  display: flex;
}
```

---

## 2. 给定使用flex属性的css样式，分析其中盒子的最终宽度是什么？

**flex 属性是 `flex-grow`、`flex-shrink`、`flex-basis` 的缩写。**

**计算规则：**

```css
/* 示例 */
.container {
  display: flex;
  width: 600px;
}
.item1 { flex: 1; }      /* flex-grow: 1, flex-shrink: 1, flex-basis: 0% */
.item2 { flex: 2; }      /* flex-grow: 2, flex-shrink: 1, flex-basis: 0% */
.item3 { width: 100px; } /* 无 flex，按内容宽度 */
```

**计算步骤：**
1. 计算剩余空间：`600px - 100px = 500px`
2. 按 flex-grow 比例分配：`500px ÷ (1+2) ≈ 166.67px`
3. 最终宽度：
   - item1: `0 + 166.67px = 166.67px`
   - item2: `0 + 333.33px = 333.33px`
   - item3: `100px`

**注意**：当 `flex-basis` 不为 0 时，需要先减去基础宽度再分配剩余空间。

---

## 3. flexbox布局模型中，常用的属性及其作用是什么

**容器属性：**

| 属性              | 作用                                                         |
| ----------------- | ------------------------------------------------------------ |
| `flex-direction`  | 主轴方向：row、column、row-reverse、column-reverse           |
| `flex-wrap`       | 是否换行：nowrap、wrap、wrap-reverse                         |
| `flex-flow`       | flex-direction 和 flex-wrap 的简写                           |
| `justify-content` | 主轴对齐方式：flex-start、center、flex-end、space-between、space-around、space-evenly |
| `align-items`     | 交叉轴对齐方式：stretch、center、flex-start、flex-end、baseline |
| `align-content`   | 多根轴线对齐方式（多行时）                                   |

**项目属性：**

| 属性          | 作用                       |
| ------------- | -------------------------- |
| `order`       | 排序，数值越小越靠前       |
| `flex-grow`   | 放大比例，默认 0           |
| `flex-shrink` | 缩小比例，默认 1           |
| `flex-basis`  | 分配剩余空间前的基准宽度   |
| `flex`        | grow、shrink、basis 的简写 |
| `align-self`  | 覆盖容器的 align-items     |

---

## 4. 如何使用css实现常见的两栏布局，有哪些方法

**左侧固定，右侧自适应：**

```html
<div class="container">
  <div class="left">固定宽度</div>
  <div class="right">自适应</div>
</div>
```

**方法1：float + BFC**
```css
.left { float: left; width: 200px; }
.right { overflow: hidden; } /* 触发 BFC */
```

**方法2：flex**
```css
.container { display: flex; }
.left { width: 200px; }
.right { flex: 1; }
```

**方法3：position + margin**
```css
.left { position: absolute; width: 200px; }
.right { margin-left: 200px; }
```

**方法4：grid**
```css
.container { display: grid; grid-template-columns: 200px 1fr; }
```

---

## 5. css选择器的优先级规则是怎样的？内联选择器和id选择器哪个优先级更高？

**优先级规则（从高到低）：**

| 优先级 | 类型               | 示例                                |
| ------ | ------------------ | ----------------------------------- |
| 最高   | !important         | `color: red !important;`            |
| 1      | 内联样式           | `style="color: red"`                |
| 2      | ID 选择器          | `#id`                               |
| 3      | 类/伪类/属性选择器 | `.class`、`:hover`、`[type="text"]` |
| 4      | 元素/伪元素选择器  | `div`、`::before`                   |
| 最低   | 通配符/组合器      | `*`、`>`、`+`、`~`                  |

**权重计算**：通常用 (a, b, c, d) 表示
- 内联样式：(1, 0, 0, 0)
- ID：(0, 1, 0, 0)
- 类：(0, 0, 1, 0)
- 元素：(0, 0, 0, 1)

**比较**：内联选择器优先级高于 ID 选择器（除非 ID 使用了 !important）

---

## 6. 如何实现一个元素的垂直居中

**方法1：flex（推荐）**
```css
.parent {
  display: flex;
  align-items: center;
}
```

**方法2：absolute + transform**
```css
.parent { position: relative; }
.child {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
}
```

**方法3：grid**
```css
.parent {
  display: grid;
  align-items: center;
}
```

**方法4：table-cell**
```css
.parent {
  display: table-cell;
  vertical-align: middle;
}
```

**方法5：line-height（单行文本）**
```css
.parent {
  height: 100px;
  line-height: 100px;
}
```

---

## 7. css有哪些常用的布局方式

| 布局方式       | 特点              | 适用场景           |
| -------------- | ----------------- | ------------------ |
| **正常文档流** | 默认块级/行内排列 | 简单页面           |
| **浮动布局**   | float + clear     | 文字环绕、旧版布局 |
| **定位布局**   | position          | 固定元素、弹窗     |
| **Flexbox**    | 一维布局          | 导航栏、卡片、列表 |
| **Grid**       | 二维布局          | 复杂网格、整体页面 |
| **多列布局**   | column-count      | 类似报纸排版       |
| **响应式布局** | media queries     | 多端适配           |

---

## 8. css的box-sizing属性有哪些值，他们各自对盒模型计算方式有哪些影响？

| 值                    | 计算方式                        | 示例                                              |
| --------------------- | ------------------------------- | ------------------------------------------------- |
| `content-box`（默认） | width = 内容宽度                | `width: 200px` + `padding: 20px` = 实际宽度 240px |
| `border-box`          | width = 内容 + padding + border | `width: 200px` + `padding: 20px` = 内容宽度 160px |

```css
/* 全局推荐设置 */
* {
  box-sizing: border-box;
}
```

---

## 9. css中有哪些常见的长度单位，他们各自的使用场景和特点是什么

| 单位        | 类型 | 特点                   | 适用场景             |
| ----------- | ---- | ---------------------- | -------------------- |
| `px`        | 绝对 | 固定像素               | 边框、阴影、小尺寸   |
| `rem`       | 相对 | 相对于根元素 font-size | 响应式文字、全局适配 |
| `em`        | 相对 | 相对于父元素 font-size | 局部缩放、组件内     |
| `%`         | 相对 | 相对于父元素           | 宽度、布局           |
| `vw/vh`     | 相对 | 相对于视口宽度/高度    | 全屏、视口比例       |
| `vmin/vmax` | 相对 | 相对于视口较小/较大边  | 保持比例             |
| `ch`        | 相对 | 相对于字符 0 宽度      | 限制文本长度         |

---

## 10. css中position属性有哪些常用的值，他们各自的特点和用法是什么

| 值         | 定位参考             | 是否脱离文档流 | 特点                     |
| ---------- | -------------------- | -------------- | ------------------------ |
| `static`   | 无                   | 否             | 默认值，top/left 无效    |
| `relative` | 自身原位置           | 否             | 保留原占位，相对自身偏移 |
| `absolute` | 最近的非 static 祖先 | 是             | 绝对定位，不占位         |
| `fixed`    | 视口                 | 是             | 固定屏幕位置             |
| `sticky`   | 父容器滚动区域       | 混合           | 滚动到阈值时固定         |

---

## 11. css中的相对定位relative和绝对定位absolute有什么区别，他们各自适用于哪些场景？

| 对比       | relative           | absolute             |
| ---------- | ------------------ | -------------------- |
| **参考点** | 自身原本位置       | 最近的非 static 祖先 |
| **文档流** | 保留原占位         | 脱离文档流           |
| **影响**   | 不影响其他元素位置 | 其他元素会占据其位置 |

**使用场景：**
- **relative**：微调位置、作为 absolute 的参照物
- **absolute**：弹窗、下拉菜单、图标定位、悬浮层

---

## 12. css中有哪些方法可以隐藏一个div元素，他们之间有什么区别

| 方法                                    | 是否占位 | 是否响应事件 | 是否渲染     | 适用场景   |
| --------------------------------------- | -------- | ------------ | ------------ | ---------- |
| `display: none`                         | 否       | 否           | 不渲染       | 完全隐藏   |
| `visibility: hidden`                    | 是       | 否           | 渲染但不可见 | 保留占位   |
| `opacity: 0`                            | 是       | 是           | 渲染透明     | 动画过渡   |
| `position: absolute; left: -9999px`     | 否       | 否           | 移出屏幕     | 屏幕外隐藏 |
| `width: 0; height: 0; overflow: hidden` | 否       | 否           | 尺寸为 0     | 特殊场景   |

---

## 13. 如何使用flexbox布局实现一个div在主轴和侧轴上的居中布局

```css
.container {
  display: flex;
  justify-content: center; /* 主轴居中 */
  align-items: center;     /* 交叉轴居中 */
  height: 100vh;
}
```

**或使用 margin: auto：**
```css
.container {
  display: flex;
}
.item {
  margin: auto;
}
```

---

## 14. 阐述bfc（块级格式化上下文）的概念，并提供几个世纪的应用场景和例子？

**BFC（Block Formatting Context）**：块级格式化上下文，是 CSS 渲染中独立的布局环境，内部元素不会影响外部。

**触发 BFC 的方式：**
- `overflow: hidden/auto/scroll`
- `float: left/right`
- `position: absolute/fixed`
- `display: inline-block/flex/grid/table`

**应用场景：**

1. **清除浮动**
```css
.clearfix {
  overflow: hidden; /* 触发 BFC，包含浮动元素 */
}
```

2. **防止外边距塌陷**
```css
.parent {
  overflow: hidden; /* 隔离父子 margin */
}
```

3. **自适应两栏布局**
```css
.left { float: left; width: 200px; }
.right { overflow: hidden; } /* 触发 BFC，不环绕浮动 */
```

---

## 15. 介绍css的flex布局，主轴和交叉轴的概念是什么？如何使用flex布局实现页面排版？

**主轴（Main Axis）**：由 `flex-direction` 决定的方向，项目沿主轴排列。

**交叉轴（Cross Axis）**：与主轴垂直的方向。

```css
.container {
  display: flex;
  flex-direction: row; /* 主轴：水平，交叉轴：垂直 */
  /* flex-direction: column; 主轴：垂直，交叉轴：水平 */
}
```

**实现页面排版示例：**
```css
/* 典型页面布局 */
.page {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}
.header { height: 60px; }
.main { flex: 1; display: flex; } /* 中间区域 flex 填充 */
.sidebar { width: 200px; }
.content { flex: 1; }
.footer { height: 80px; }
```

---

## 16. 是否使用过css media queries？他们的作用是什么？

**Media Queries**：响应式设计的核心，根据设备特性（宽度、高度、分辨率等）应用不同样式。

```css
/* 语法 */
@media (max-width: 768px) {
  .container {
    flex-direction: column;
  }
}

/* 常用断点 */
/* 手机 */ @media (max-width: 640px) { }
/* 平板 */ @media (min-width: 641px) and (max-width: 1024px) { }
/* 桌面 */ @media (min-width: 1025px) { }

/* 其他特性 */
@media (prefers-color-scheme: dark) { } /* 深色模式 */
@media (hover: hover) { } /* 支持 hover */
```

---

## 17. css中 ::before 和 ::after 伪类如何使用？他们的应用场景是什么？

**使用方式：**
```css
.element::before {
  content: "前缀"; /* 必须要有 content */
  display: inline-block;
  /* 其他样式 */
}
.element::after {
  content: "";
  display: block;
}
```

**应用场景：**
- 添加装饰性图标
- 清除浮动：`.clearfix::after { content: ""; clear: both; display: table; }`
- 添加提示文本
- 实现边框特效
- 实现计数器

---

## 18. css中实现元素水平居中有哪些常见方法？

```css
/* 1. 行内/行内块元素 */
.parent { text-align: center; }

/* 2. 块级元素（定宽） */
.child { margin: 0 auto; }

/* 3. flex */
.parent { display: flex; justify-content: center; }

/* 4. grid */
.parent { display: grid; place-items: center; }

/* 5. absolute + transform */
.parent { position: relative; }
.child {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
}
```

---

## 19. 使用flexbox布局，如何实现一个居中对齐的盒子？

```css
/* 方法1：容器控制 */
.parent {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

/* 方法2：子项 margin */
.parent {
  display: flex;
}
.child {
  margin: auto;
}

/* 方法3：grid + place-items */
.parent {
  display: grid;
  place-items: center;
  height: 100vh;
}
```

---

## 20. css中，如何实现0.5像素的线条

**方法1：transform scale**
```css
.line {
  height: 1px;
  background: #000;
  transform: scaleY(0.5);
  transform-origin: 0 0;
}
```

**方法2：伪元素 + transform**
```css
.line {
  position: relative;
}
.line::after {
  content: "";
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 1px;
  background: #000;
  transform: scaleY(0.5);
  transform-origin: 0 0;
}
```

**方法3：box-shadow**
```css
.line {
  box-shadow: 0 0.5px 0 0 #000;
}
```

**方法4：使用 SVG**

---

## 21. css盒模型包含哪些组成部分？标准和模型与怪异模型有什么区别？

**盒模型组成部分：**
- **内容（Content）**：实际内容区域
- **内边距（Padding）**：内容与边框之间的空间
- **边框（Border）**：边框线
- **外边距（Margin）**：盒子之间的空间

**标准盒模型 vs 怪异盒模型：**

| 盒模型     | box-sizing    | width 计算                      |
| ---------- | ------------- | ------------------------------- |
| 标准       | `content-box` | width = 内容宽度                |
| 怪异（IE） | `border-box`  | width = 内容 + padding + border |

```css
/* 标准盒模型 */
.box {
  width: 200px;
  padding: 20px;
  border: 1px solid;
  box-sizing: content-box; /* 实际宽度 242px */
}

/* 怪异盒模型 */
.box {
  width: 200px;
  padding: 20px;
  border: 1px solid;
  box-sizing: border-box; /* 实际宽度 200px，内容宽度 158px */
}
```

---

## 22. 解释一下css中的伪类与伪元素有什么区别？has()和is()选择器的作用是什么

**伪类 vs 伪元素：**

| 对比 | 伪类                   | 伪元素                   |
| ---- | ---------------------- | ------------------------ |
| 语法 | 单冒号 `:`             | 双冒号 `::`（CSS3 规范） |
| 作用 | 选择特定状态的元素     | 创建虚拟元素             |
| 示例 | `:hover`、`:nth-child` | `::before`、`::after`    |

**`:is()` 选择器：**
```css
/* 简化复杂选择器 */
:is(header, main, footer) p { color: red; }
/* 等同于 */
header p, main p, footer p { color: red; }
```

**`:has()` 选择器：**
```css
/* 选择包含子元素的父元素 */
div:has(p) { background: yellow; } /* 包含 p 的 div */
form:has(:invalid) { border: 1px solid red; } /* 有无效字段的表单 */
```

---

## 23. 项目中是否使用过css伪类

**常用伪类示例：**

```css
/* 1. 状态伪类 */
.button:hover { background: blue; }
.input:focus { outline: none; }
.link:active { color: red; }

/* 2. 结构伪类 */
li:first-child { margin-top: 0; }
li:last-child { margin-bottom: 0; }
li:nth-child(odd) { background: #f5f5f5; }
li:not(:first-child) { border-top: 1px solid #eee; }

/* 3. 表单伪类 */
input:checked + label { color: blue; }
input:disabled { opacity: 0.5; }
input:required { border-color: red; }
```

---

## 24. css选择器有哪些种类，他们的优先级规则是怎样的，是否存在可以打破这些优先级的特殊情况？

**选择器种类：**

| 种类         | 示例                   |
| ------------ | ---------------------- |
| 通配符       | `*`                    |
| 元素选择器   | `div`、`p`             |
| 类选择器     | `.class`               |
| ID 选择器    | `#id`                  |
| 属性选择器   | `[type="text"]`        |
| 伪类选择器   | `:hover`、`:nth-child` |
| 伪元素选择器 | `::before`             |
| 组合器       | `>`、`+`、`~`、空格    |

**优先级规则：**

内联样式 > ID > 类/属性/伪类 > 元素/伪元素 > 通配符

**打破优先级的方式：**
1. **`!important`**：最高优先级，但应谨慎使用
2. **选择器重复**：`.class.class` 比 `.class` 优先级高
3. **内联样式**：优先级高于任何内部/外部样式

```css
/* !important 会覆盖所有 */
.element {
  color: red !important;
}
```

---

## 25. css盒子模型是什么，如何计算其宽度，是否还存在其他盒子模型？

**盒子模型**：HTML 元素在页面中占据空间的矩形区域模型。

**宽度计算：**

```css
/* content-box */
总宽度 = width + padding-left + padding-right + border-left + border-right

/* border-box */
总宽度 = width（已包含 padding 和 border）
内容宽度 = width - padding - border
```

**其他盒子模型概念：**
- **弹性盒子（Flexbox）**：一维布局模型
- **网格盒子（Grid）**：二维布局模型
- **多列盒子（Multi-column）**：类似报纸排版

---

## 26. css中哪些情况会出现margin重叠问题，如何避免或解决

**出现情况：**

1. **相邻兄弟元素**：上下 margin 合并
2. **父子元素**：父元素与第一个/最后一个子元素 margin 合并
3. **空块元素**：自身上下 margin 合并

**解决方案：**

| 场景     | 解决方式                                           |
| -------- | -------------------------------------------------- |
| 相邻兄弟 | 只设置一个 margin，或用 padding 替代               |
| 父子     | 父元素设置 `overflow: hidden`、`border`、`padding` |
| 通用     | 使用 `display: flex` 或 `display: grid`            |
| 空块     | 设置 `min-height`、`border`、`padding`             |

---

## 27. css中如何清除浮动,有哪些常见方法

**方法1：clearfix（推荐）**
```css
.clearfix::after {
  content: "";
  display: table;
  clear: both;
}
```

**方法2：overflow**
```css
.parent {
  overflow: hidden; /* 触发 BFC */
}
```

**方法3：空 div**
```html
<div style="clear: both;"></div>
```

**方法4：使用 flex/grid 替代浮动**

---

## 28. 解释一下justify-content和align-items在flexbox布局的作用和区别？

| 属性              | 作用方向   | 取值                                                         | 说明                     |
| ----------------- | ---------- | ------------------------------------------------------------ | ------------------------ |
| `justify-content` | **主轴**   | flex-start、center、flex-end、space-between、space-around、space-evenly | 控制项目在主轴上的对齐   |
| `align-items`     | **交叉轴** | stretch、center、flex-start、flex-end、baseline              | 控制项目在交叉轴上的对齐 |

```css
.container {
  display: flex;
  flex-direction: row; /* 主轴水平 */
  justify-content: center; /* 水平居中 */
  align-items: center; /* 垂直居中 */
}
```

---

## 29. 描述一下css中flex属性及其常见用法

**flex 是 `flex-grow`、`flex-shrink`、`flex-basis` 的缩写。**

**常见用法：**

```css
/* 默认值 */
.item { flex: 0 1 auto; }

/* 可伸缩，占满剩余空间 */
.item { flex: 1; }      /* 1 1 0% */

/* 不可伸缩，根据内容决定 */
.item { flex: none; }   /* 0 0 auto */

/* 固定宽度，不伸缩 */
.item { flex: 0 0 100px; }

/* 均分父容器宽度 */
.container > .item { flex: 1; }
```

**应用场景：**
- 导航栏：`logo { flex: 0 0 auto; } .nav { flex: 1; }`
- 两栏布局：`.left { flex: 0 0 200px; } .right { flex: 1; }`
- 等分布局：`.item { flex: 1; }`

---

## 30. 对css盒模型有哪些理解

**核心理解：**

1. **组成部分**：content、padding、border、margin
2. **两种模型**：content-box（标准）、border-box（IE/怪异）
3. **计算方式**：box-sizing 决定 width/height 包含的范围
4. **外边距塌陷**：垂直 margin 会合并
5. **内边距影响**：padding 会撑大盒子（border-box 下除外）

**最佳实践**：
```css
* {
  box-sizing: border-box; /* 推荐全局设置 */
}
```

---

## 31. css伪元素和伪类有什么区别？分别有哪些应用场景？

| 对比     | 伪类               | 伪元素                           |
| -------- | ------------------ | -------------------------------- |
| **含义** | 选择元素的特定状态 | 创建虚拟元素                     |
| **语法** | `:`（单冒号）      | `::`（双冒号）                   |
| **数量** | 多个可同时使用     | 每个元素最多两个（before/after） |

**伪类应用场景：**
- `:hover`：鼠标悬停效果
- `:nth-child()`：表格斑马纹
- `:checked`：自定义 checkbox
- `:focus`：表单焦点状态
- `:not()`：排除特定元素

**伪元素应用场景：**
- `::before`/`::after`：装饰图标、清除浮动、提示文字
- `::first-letter`：首字下沉
- `::first-line`：首行特殊样式
- `::placeholder`：占位符样式
- `::selection`：选中文本样式

---

## 32. 解释css bfc的概念、创建方式及其实际应用

**BFC（块级格式化上下文）**：独立的渲染区域，内部元素不会影响外部。

**创建方式：**
```css
/* 常用触发方式 */
.bfc {
  overflow: hidden;      /* 最常用 */
  float: left;
  position: absolute;
  display: inline-block;
  display: flex;
  display: grid;
}
```

**实际应用：**

1. **清除浮动**：父元素触发 BFC 包含浮动子元素
2. **防止 margin 塌陷**：BFC 隔离父子 margin
3. **自适应布局**：触发 BFC 的元素不与浮动元素重叠
4. **多列布局**：BFC 内部独立布局

---

## 33. 详细说明css flex布局的工作原理和常见应用场景

**工作原理：**

1. **主轴概念**：通过 `flex-direction` 定义主轴方向
2. **空间分配**：`flex-grow` 分配剩余空间，`flex-shrink` 处理溢出
3. **对齐方式**：`justify-content` 主轴对齐，`align-items` 交叉轴对齐
4. **换行控制**：`flex-wrap` 决定是否换行

**应用场景：**

| 场景         | 实现方式                                        |
| ------------ | ----------------------------------------------- |
| **导航栏**   | `display: flex; justify-content: space-between` |
| **等分布局** | `.item { flex: 1 }`                             |
| **垂直居中** | `align-items: center`                           |
| **圣杯布局** | 三栏：左右固定，中间 flex: 1                    |
| **瀑布流**   | `flex-wrap: wrap` + 百分比宽度                  |
| **卡片列表** | `flex-wrap: wrap` + 固定宽度                    |

---

## 34. 如何使用css实现多余文字的省略号显示？

**单行省略：**
```css
.ellipsis {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
```

**多行省略（webkit 内核）：**
```css
.ellipsis-multi {
  display: -webkit-box;
  -webkit-line-clamp: 3;  /* 显示行数 */
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-overflow: ellipsis;
}
```

**多行省略（通用方案）：**
```css
.ellipsis-multi {
  position: relative;
  max-height: 4.5em;  /* 3行 × 1.5行高 */
  overflow: hidden;
}
.ellipsis-multi::after {
  content: "...";
  position: absolute;
  bottom: 0;
  right: 0;
  background: white;
  padding-left: 20px;
}
```

---

## 35. 回流reflow和重绘repaint的概念是什么？他们之间有什么区别？

| 对比         | 回流（Reflow）                     | 重绘（Repaint）                |
| ------------ | ---------------------------------- | ------------------------------ |
| **定义**     | 重新计算元素几何属性（位置、尺寸） | 重新绘制元素外观（颜色、背景） |
| **触发**     | 布局变化                           | 样式变化不改变布局             |
| **开销**     | 大，影响性能                       | 相对较小                       |
| **是否必然** | 回流必然导致重绘                   | 重绘不一定导致回流             |

**触发回流的操作：**
- 添加/删除 DOM 元素
- 改变尺寸（width、height、padding、margin）
- 改变位置（top、left）
- 改变字体大小
- 窗口 resize
- 获取 offsetWidth 等属性

**优化建议：**
- 使用 `transform` 替代 top/left
- 使用 `visibility: hidden` 替代 `display: none`
- 批量修改样式（class 或 cssText）
- 使用 `requestAnimationFrame`

---

## 36. 在css中，有哪些方法可以实现元素的缩小效果

```css
/* 1. transform scale */
.element {
  transition: transform 0.3s;
}
.element:hover {
  transform: scale(0.9);
}

/* 2. 宽度/高度缩小 */
.element {
  transition: all 0.3s;
}
.element:hover {
  width: 90%;
  height: 90%;
}

/* 3. clip-path */
.element:hover {
  clip-path: inset(5%);
}

/* 4. zoom（非标准） */
.element:hover {
  zoom: 0.9;
}
```

---

## 37. flexbox常用的属性有哪些？

**容器属性（6个）：**
- `flex-direction`
- `flex-wrap`
- `flex-flow`
- `justify-content`
- `align-items`
- `align-content`

**项目属性（6个）：**
- `order`
- `flex-grow`
- `flex-shrink`
- `flex-basis`
- `flex`
- `align-self`

---

## 38. 对css的理解是什么？在学习css的过程中有没有遇到什么有意思的经历

**CSS 理解：**
- CSS 是描述文档呈现方式的样式表语言
- 核心是盒模型、布局（flex/grid）、响应式
- 难点在于兼容性、优先级、层叠上下文、BFC

**有意思的经历（示例）：**
- 发现 `margin: auto` 在 flex 中可以实现双轴居中
- 理解 BFC 后解决了多年的浮动和 margin 塌陷问题
- 使用 `:has()` 选择器实现父元素根据子元素状态改变样式
- 实现纯 CSS 的开关、轮播图、下拉菜单

---

## 39. 如何使用flexbox实现一个九宫格布局

```html
<div class="grid">
  <div class="item">1</div>
  <div class="item">2</div>
  <!-- ... 共9个 -->
</div>
```

```css
.grid {
  display: flex;
  flex-wrap: wrap;
  width: 300px;
}
.item {
  flex: 0 0 33.333%; /* 或 width: 33.333% */
  height: 100px;
  box-sizing: border-box;
  border: 1px solid #ccc;
}
```

**使用 gap 属性：**
```css
.grid {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  width: 320px; /* 100px * 3 + 10px * 2 */
}
.item {
  flex: 0 0 100px;
  height: 100px;
}
```

---

## 40. 是否遇到过css外边距重叠问题，如何理解和解决

**遇到过，典型场景：**
- 多个相邻卡片之间的 margin 合并
- 父元素与子元素的 margin-top 合并

**理解和解决：**
1. 理解原理：只有块级元素的垂直 margin 会合并
2. 解决方案：
   - 使用 padding 替代 margin
   - 父元素触发 BFC（overflow: hidden）
   - 使用 flex/grid 布局
   - 只给一个方向设置 margin

---

## 41. 如何利用css grid布局创建一个九宫格，并绘制出其分割线

```html
<div class="grid">
  <div class="item">1</div>
  <!-- ... 共9个 -->
</div>
```

```css
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 100px);
  gap: 2px;
  background: #000; /* 通过 gap 显示分割线 */
}

/* 或者用 border */
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 100px);
}
.item {
  border: 1px solid #ccc;
  margin: -1px 0 0 -1px; /* 合并边框 */
}

/* 使用 outline */
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  outline: 1px solid #ccc;
}
.item {
  outline: 1px solid #ccc;
}
```

---

## 42. 如何使用css实现左侧定宽，右侧自适应的两栏布局

```css
/* 方法1：flex */
.container {
  display: flex;
}
.left { width: 200px; }
.right { flex: 1; }

/* 方法2：grid */
.container {
  display: grid;
  grid-template-columns: 200px 1fr;
}

/* 方法3：float + BFC */
.left { float: left; width: 200px; }
.right { overflow: hidden; }

/* 方法4：position */
.container { position: relative; }
.left { position: absolute; width: 200px; }
.right { margin-left: 200px; }
```

---

## 43. 如何实现响应式布局

**响应式布局方法：**

1. **Media Queries**：根据屏幕尺寸应用不同样式
```css
@media (max-width: 768px) {
  .container { flex-direction: column; }
}
```

2. **弹性布局**：flex、grid 自动适配
```css
.container { display: flex; flex-wrap: wrap; }
.item { flex: 1; min-width: 250px; }
```

3. **相对单位**：rem、%、vw、vh
```css
html { font-size: 16px; }
@media (max-width: 768px) {
  html { font-size: 14px; }
}
```

4. **视口设置**：
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

5. **图片响应式**：
```css
img { max-width: 100%; height: auto; }
<picture>
  <source srcset="large.jpg" media="(min-width: 800px)">
  <img src="small.jpg">
</picture>
```

---

## 44. 选用css-in-js技术有哪些优势？他在站点换肤场景中是否使用？相比裸css有何不同？

**CSS-in-JS 优势：**
- **作用域隔离**：自动生成唯一类名，避免冲突
- **动态样式**：基于 props/state 动态计算样式
- **按需加载**：只加载使用的样式
- **类型安全**：TypeScript 支持
- **易于维护**：样式与组件同文件

**换肤场景：**
```javascript
// styled-components 换肤
const Button = styled.button`
  background: ${props => props.theme.primary};
  color: ${props => props.theme.text};
`;

// ThemeProvider 切换主题
<ThemeProvider theme={darkTheme}>
  <App />
</ThemeProvider>
```

**与裸 CSS 对比：**

| 对比     | CSS-in-JS         | 裸 CSS                  |
| -------- | ----------------- | ----------------------- |
| 作用域   | 自动隔离          | 需要命名规范（BEM）     |
| 动态样式 | 原生支持          | 需要 CSS 变量或 JS 修改 |
| 学习成本 | 需学习框架 API    | 标准 CSS                |
| 性能     | 运行时开销        | 无运行时开销            |
| 调试     | DevTools 支持有限 | 成熟的调试工具          |

---

## 45. 在css中如何确保样式不会相互污染？在vue项目中引入外部样式时，如果造成全局污染，通常有哪些解决方案

**确保样式不污染的方法：**

1. **CSS Modules**：自动生成唯一类名
```css
/* button.module.css */
.button { color: red; }
```
```js
import styles from './button.module.css'
<button className={styles.button}>
```

2. **BEM 命名规范**：Block__Element--Modifier
```css
.card { }
.card__title { }
.card--large { }
```

3. **Scoped CSS（Vue）**：添加 `scoped` 属性
```vue
<style scoped>
.button { color: red; } /* 自动添加 data-v 属性 */
</style>
```

4. **CSS-in-JS**：styled-components、emotion

**Vue 中解决全局污染：**

1. **使用 scoped**：最常用
```vue
<style scoped>
/* 样式只作用于当前组件 */
</style>
```

2. **使用 CSS Modules**：
```vue
<style module>
.button { }
</style>
```

3. **深度选择器**：需要影响子组件时
```css
:deep(.child-class) { }
::v-deep .child-class { }
>>> .child-class { }
```

4. **避免在全局样式中使用元素选择器**：使用类选择器

---

## 46. css实现垂直居中的方案有哪些

```css
/* 1. flex（推荐） */
.parent {
  display: flex;
  align-items: center;
}

/* 2. grid */
.parent {
  display: grid;
  align-items: center;
}
/* 或 place-items: center; */

/* 3. absolute + transform */
.parent { position: relative; }
.child {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
}

/* 4. table-cell */
.parent {
  display: table-cell;
  vertical-align: middle;
}

/* 5. line-height（单行文本） */
.parent {
  height: 100px;
  line-height: 100px;
}

/* 6. margin: auto（flex 子项） */
.parent { display: flex; }
.child { margin: auto 0; }

/* 7. writing-mode */
.parent {
  writing-mode: vertical-lr;
  text-align: center;
}
```

---

## 47. css定位方式有哪些，例如absolute、relative、sticky分别说明

| 定位       | 参考系               | 是否脱离文档流 | 说明                       |
| ---------- | -------------------- | -------------- | -------------------------- |
| `static`   | 无                   | 否             | 默认值，top/left 无效      |
| `relative` | 自身原本位置         | 否             | 保留原占位，相对偏移       |
| `absolute` | 最近的非 static 祖先 | 是             | 完全脱离，不占位           |
| `fixed`    | 视口                 | 是             | 固定屏幕位置，滚动时不移动 |
| `sticky`   | 父容器滚动区域       | 混合           | 滚动到阈值时表现为 fixed   |

**sticky 特性：**
```css
.header {
  position: sticky;
  top: 0; /* 滚动到顶部时固定 */
}
/* 需要设置父容器高度，且不能有 overflow: hidden */
```

---

## 48. css盒模型的工作原理是什么

**工作原理：**

1. **元素渲染为矩形盒子**：每个元素生成一个或多个矩形框
2. **盒子尺寸计算**：由 `box-sizing` 决定 width/height 含义
3. **盒子在文档流中排列**：块级垂直排列，行内水平排列
4. **盒子相互作用**：margin 塌陷、浮动环绕、定位层叠

**层叠上下文**：当元素有定位、透明度、transform 等属性时，会创建新的层叠上下文，影响 z-index 效果。

---

## 49. 如何实现组件库的主题切换功能

**方法1：CSS 变量（推荐）**
```css
/* 定义主题变量 */
:root {
  --primary-color: #1890ff;
  --bg-color: #fff;
  --text-color: #333;
}

[data-theme="dark"] {
  --primary-color: #3a6ea5;
  --bg-color: #1a1a1a;
  --text-color: #e0e0e0;
}

.component {
  background: var(--bg-color);
  color: var(--text-color);
}
```
```js
// 切换主题
document.documentElement.setAttribute('data-theme', 'dark')
```

**方法2：动态加载 CSS 文件**
```js
const link = document.createElement('link')
link.rel = 'stylesheet'
link.href = `theme-${theme}.css`
document.head.appendChild(link)
```

**方法3：CSS-in-JS 主题提供者**
```jsx
<ThemeProvider theme={lightTheme}>
  <App />
</ThemeProvider>
```

**方法4：LESS/SASS 变量 + 编译多主题**
- 预编译多个主题 CSS 文件
- 运行时动态切换

---

## 50. css本身没有model概念，如果提及使用css model，其实现原理是什么？

**可能指的是：**
1. **CSS 模块化（CSS Modules）**
2. **CSS 对象模型（CSSOM）**

**CSS Modules 原理：**
- 构建工具（webpack）在编译时解析 CSS
- 将类名转换为唯一哈希值
- 生成映射对象供 JS 使用

```js
// 编译前
.button { color: red; }

// 编译后
._2a3b4c { color: red; }

// 映射
{ button: "_2a3b4c" }
```

**CSSOM（CSS Object Model）原理：**
- 浏览器解析 CSS 生成的树形结构
- 可以通过 JS 操作样式规则

```js
// 操作 CSSOM
const styles = document.styleSheets[0]
styles.insertRule('.new-class { color: red }', 0)
```

---

## 51. 有哪些方法可以解决css样式冲突问题？

| 方法                | 说明                               | 适用场景       |
| ------------------- | ---------------------------------- | -------------- |
| **CSS Modules**     | 自动生成唯一类名                   | 组件化项目     |
| **Scoped CSS**      | Vue 特有，添加 data 属性           | Vue 项目       |
| **BEM 命名**        | 命名规范：Block__Element--Modifier | 任何项目       |
| **CSS-in-JS**       | 样式与组件绑定                     | React 项目     |
| **命名空间**        | 添加前缀 `.myapp-`                 | 全局样式隔离   |
| **Shadow DOM**      | 完全隔离的样式作用域               | Web Components |
| **!important 避免** | 不使用或谨慎使用                   | 通用规范       |
| **CSS 重置**        | 统一浏览器默认样式                 | 基础样式       |

**优先级：** CSS Modules > Scoped CSS > 命名空间 > BEM



