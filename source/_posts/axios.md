---
title: axios
date: 2026-03-23 10:18:48
tags:
---

# JavaScript 核心知识点总结 (Window & DOM 操作)01

本文档基于一个综合的 HTML/JavaScript 示例，总结了浏览器内置对象、DOM 操作、事件处理及 Ajax 请求等核心前端概念。

## 1. Window 对象

`window` 对象是浏览器中的全局对象，代表了浏览器的窗口。

### 1.1 全局变量与函数
在全局作用域中声明的变量和函数，会自动成为 `window` 对象的属性和方法。
```javascript
var fun = function () {
  window.number = 100; // 显式添加到 window 对象
};
fun();
console.log(number); // 输出: 100 (全局可访问)
```

### 1.2 窗口尺寸属性

- `window.innerWidth` / `window.innerHeight`：浏览器窗口的视口（viewport）尺寸（包含滚动条，不包含浏览器边框和工具栏）。
- `window.outerWidth` / `window.outerHeight`：浏览器窗口的整体尺寸（包含边框、工具栏等）。

### 1.3 窗口控制方法

- `window.open(url, name)`：打开一个新窗口。
- `window.close()`：关闭由 `open` 方法打开的窗口。
- a链接的 `target` 属性：`_blank`（新窗口）、`_self`（当前窗口）、`_parent`（父级框架）、`_top`（顶层框架）。

## 2. 其他内置对象

| 对象          | 描述                                | 常用属性/方法                                                |
| ------------- | ----------------------------------- | ------------------------------------------------------------ |
| **location**  | 提供当前 URL 的信息，并可以重定向。 | `href`, `protocol`, `hostname`, `pathname`, `search`, `hash` |
| **history**   | 操作浏览器会话历史。                | `back()`, `forward()`, `go()`                                |
| **navigator** | 提供浏览器的信息。                  | `userAgent`, `language`, `platform`, `cookieEnabled`         |
| **screen**    | 提供用户屏幕的信息。                | `width`, `height`, `colorDepth`                              |

## 3. Document 对象与 DOM 操作

`document` 对象是网页内容（DOM 树）的入口。

### 3.1 获取元素

- `document.querySelector(selectors)`：返回匹配的第一个元素。
- `document.querySelectorAll(selectors)`：返回匹配的所有元素（NodeList）。

### 3.2 事件绑定

通过 `onclick` 等属性绑定事件处理器。

javascript

```
document.querySelector('.btn').onclick = function () {
  // 事件处理逻辑
};
```



常见事件：`click`（单击）、`dblclick`（双击）、`mouseenter`（鼠标进入）、`mouseleave`（鼠标离开）。

### 3.3 样式修改

- **通过 class 控制**：
  - `元素.classList.add('类名')`：添加类。
  - `元素.classList.remove('类名')`：移除类。
  - `元素.classList.toggle('类名')`：切换类。
  - `元素.classList.contains('类名')`：判断是否存在。
- **通过 style 属性**：`元素.style.属性名 = '值'`（属性名使用小驼峰，如 `backgroundColor`）。

### 3.4 创建和添加元素

1. **创建**：`document.createElement('标签名')`
2. **设置内容**：`元素.innerHTML = 'HTML内容'` 或 `元素.innerText = '文本内容'`
3. **添加**：`父元素.appendChild(子元素)`

### 3.5 删除元素

```
父元素.removeChild(子元素)
```

## 4. Ajax 与 JSON

实现异步数据交互，在不重新加载页面的情况下与服务器通信。

### 4.1 原生 XMLHttpRequest (GET 示例)

javascript

```
var xhr = new XMLHttpRequest();
xhr.open('GET', 'http://localhost:3008/list'); // 配置请求
xhr.send();                                      // 发送请求
xhr.onreadystatechange = function () {
  // 判断响应状态和请求完成状态
  if (xhr.status == 200 && xhr.readyState == 4) {
    var data = JSON.parse(xhr.responseText);     // 解析 JSON 字符串
    console.log(data);                           // 处理数据
  }
};
```



### 4.2 原生 XMLHttpRequest (POST 示例)

javascript

```
var xhr = new XMLHttpRequest();
xhr.open('POST', 'http://localhost:3008/list');
// 设置请求头，告知服务器发送的是 JSON 数据
xhr.setRequestHeader('Content-Type', 'application/json');
// 发送 JSON 字符串数据
xhr.send(JSON.stringify({ name: inputvalue }));
xhr.onreadystatechange = function () {
  if (xhr.status >= 200 && xhr.status < 300 && xhr.readyState == 4) {
    var response = JSON.parse(xhr.responseText);
    // 处理响应...
  }
};
```



### 4.3 JSON 方法

- `JSON.stringify(obj)`：将 JavaScript 对象或数组转换为 JSON 字符串。
- `JSON.parse(jsonString)`：将 JSON 字符串转换为 JavaScript 对象或数组。

## 5. CSS 选择器权重简记

样式冲突时，浏览器根据权重决定最终样式。权重值可粗略记忆为：

- 通配符 `*`：1
- 标签/伪元素：10
- 类/伪类/属性：100
- ID：1000
- 行内样式：10000

------

**总结**：此示例涵盖了前端开发中与浏览器交互的核心技术点，掌握这些内容是进行复杂 Web 应用开发的基础。