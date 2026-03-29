---
title: msjs
date: 2026-03-26 21:24:37
tags:

---

1.在使用async/await时，如何进行异常捕获？

2.如何处理两个异步请求，其中请求A依赖于请求B的完成

3.是否使用过async/await?他们有返回值吗？

4.Promise对象是否具有直接的返回值？

5.在Jscript对象中，数字键是如何被处理和存储的？

6.分析一段关于promise的代码，判断其最终的输出顺序和结果

7.针对promise、宏任务和微任务，分析一段给定代码的执行顺序和输出结果？

8.关于var、闭包和函数式编程，分析一段给定代码的输出结果？

9.说说js原型链的工作机制和作用？

10.js中object和map有哪些主要区别？在什么场景下会优先选择使用map?

11.js中数组常用的方法有哪些？并阐述他们各自的作用？

12.比较以下两个JavaScript列表的异同点: let list1=[1,2,3,4]和let list2=[]; list2.push(1); list2.push(2);?

13.前端模块化有哪些方式，CommonJS和ESModule的主要区别是什么?

14.#Promise有哪三种状态?当Promise成功和失败时，分别会调用哪些函数？

15.JavaScript 中的作用域和作用域链是怎样
的?

这是一套非常经典的JavaScript面试题，涵盖了异步编程、数据结构、原型链、模块化等多个核心知识点。以下是针对这些问题的详细解答，供你参考：

---

### 1. 在使用async/await时，如何进行异常捕获？

使用 `try/catch` 块进行捕获。

```javascript
async function fetchData() {
  try {
    const res = await fetch('/api/data');
    const data = await res.json();
    return data;
  } catch (error) {
    console.error('请求失败:', error);
    // 可返回默认值或抛出错误
  }
}
```

如果不想用 `try/catch`，也可以 `.catch`：

```javascript
const data = await fetch('/api/data').catch(err => console.log(err));
```

---

### 2. 如何处理两个异步请求，其中请求A依赖于请求B的完成

使用 `await` 按顺序执行：

```javascript
const resultB = await requestB();
const resultA = await requestA(resultB);
```

或者使用 `.then` 链式调用：

```javascript
requestB()
  .then(resultB => requestA(resultB))
  .then(resultA => console.log(resultA));
```

---

### 3. 是否使用过async/await? 他们有返回值吗？

**使用过**。  
`async` 函数总是返回一个 **Promise**。  
如果函数内部返回一个非Promise值，它会被 `Promise.resolve()` 包装；如果抛出错误，返回一个 rejected 的 Promise。

---

### 4. Promise对象是否具有直接的返回值？

**没有直接的返回值**。  
Promise 本身是一个对象，代表一个异步操作的最终完成（或失败）及其结果值。要获取其“返回值”，需要通过 `.then()` 或 `await` 获取。

---

### 5. 在JavaScript对象中，数字键是如何被处理和存储的？

- JavaScript 对象的键（属性名）会被转换为 **字符串**（Symbol 除外）。
- 数字键虽然写法上是数字，但实际存储时转为字符串，如 `obj[1]` 和 `obj['1']` 指向同一属性。
- 但在遍历时，数字键会按升序排列，字符串键按添加顺序排列（ES6 之后）。

---

### 6. 分析一段关于promise的代码，判断其最终的输出顺序和结果

*（你未提供具体代码，我给出一个常见示例）*

```javascript
console.log(1);
Promise.resolve().then(() => console.log(2));
console.log(3);
// 输出: 1, 3, 2
```

Promise 的 `.then` 回调属于微任务，会在当前同步代码执行完后执行。

---

### 7. 针对promise、宏任务和微任务，分析一段给定代码的执行顺序和输出结果

*（你未提供代码，我给出一个典型示例）*

```javascript
console.log('1');
setTimeout(() => console.log('2'), 0);
Promise.resolve().then(() => console.log('3'));
console.log('4');
// 输出: 1, 4, 3, 2
```

**顺序**：同步代码 → 微任务（Promise.then） → 宏任务（setTimeout）。

---

### 8. 关于var、闭包和函数式编程，分析一段给定代码的输出结果

*（未提供代码，常见示例）*

```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}
// 输出: 3, 3, 3
```

因为 `var` 函数作用域，循环结束后 `i` 为 3，闭包引用同一个 `i`。  
若用 `let` 则输出 0, 1, 2。

---

### 9. 说说js原型链的工作机制和作用？

- **机制**：每个对象都有一个内部 `[[Prototype]]` 指针（通过 `__proto__` 或 `Object.getPrototypeOf` 访问），指向其原型对象。当访问对象属性时，若自身不存在，则沿原型链向上查找，直到 `null`。
- **作用**：实现**继承**和**属性共享**。所有实例共享原型上的方法，节省内存。

---

### 10. js中object和map有哪些主要区别？在什么场景下会优先选择使用map?

| 特性   | Object                         | Map                          |
| ------ | ------------------------------ | ---------------------------- |
| 键类型 | 字符串或 Symbol                | 任意类型（对象、函数等）     |
| 顺序   | 数字键按升序，字符串按插入顺序 | 严格按插入顺序               |
| 大小   | 手动计算 `Object.keys()`       | `size` 属性直接获取          |
| 性能   | 适合少量键值对                 | 频繁增删时性能更好           |
| 迭代   | 需 `Object.entries`            | 直接 `for...of` 或 `forEach` |

**优先使用 Map 的场景**：
- 键为对象或非字符串类型
- 需要频繁增删键值对
- 需要保持插入顺序
- 需要快速获取长度

---

### 11. js中数组常用的方法有哪些？并阐述他们各自的作用？

| 方法                 | 作用                        |
| -------------------- | --------------------------- |
| `push` / `pop`       | 末尾添加 / 删除             |
| `unshift` / `shift`  | 开头添加 / 删除             |
| `map`                | 映射，返回新数组            |
| `filter`             | 过滤，返回新数组            |
| `reduce`             | 累积计算                    |
| `forEach`            | 遍历                        |
| `find` / `findIndex` | 查找元素 / 索引             |
| `some` / `every`     | 判断是否至少一个 / 全部满足 |
| `includes`           | 是否包含某值                |
| `sort`               | 排序（原地）                |
| `slice` / `splice`   | 截取 / 删除并插入           |
| `concat`             | 合并数组                    |

---

### 12. 比较以下两个JavaScript列表的异同点: let list1=[1,2,3,4]和let list2=[]; list2.push(1); list2.push(2);?

**相同点**：最终内容都是 `[1, 2]`。

**不同点**：
- `list1` 是**字面量创建**，一次性定义。
- `list2` 是**动态创建**，通过 `push` 逐步添加。
- 性能上，字面量更快。
- `list1` 有初始 `length` 为 4，但后文未体现；实际上若只比较最终值，两者元素相同。

---

### 13. 前端模块化有哪些方式，CommonJS和ESModule的主要区别是什么?

**模块化方式**：
- IIFE（立即执行函数）
- AMD / CMD
- CommonJS
- ES Module

**CommonJS vs ESModule**：

| 特性     | CommonJS                     | ES Module                   |
| -------- | ---------------------------- | --------------------------- |
| 环境     | Node.js（默认）              | 浏览器 + Node（需配置）     |
| 加载时机 | 运行时同步加载               | 编译时静态分析              |
| 导出     | `module.exports` / `exports` | `export` / `export default` |
| 导入     | `require()`                  | `import`                    |
| 动态导入 | 不支持原生动态               | `import()` 函数             |
| 值的绑定 | 导出值的拷贝                 | 导出值的实时绑定（只读）    |

---

### 14. Promise有哪三种状态?当Promise成功和失败时，分别会调用哪些函数？

**三种状态**：
- `pending`（进行中）
- `fulfilled`（已成功）
- `rejected`（已失败）

**成功时**：调用 `then` 的第一个回调函数。  
**失败时**：调用 `catch` 或 `then` 的第二个回调函数。

---

### 15. JavaScript中的作用域和作用域链是怎样的？

- **作用域**：变量和函数的可访问范围。JS 中有**全局作用域、函数作用域、块级作用域**（`let` / `const`）。
- **作用域链**：当在某个作用域中访问变量时，会先从当前作用域查找，若找不到则逐级向上层作用域查找，直到全局作用域，若仍找不到则报错（未声明变量）或返回 `undefined`（属性访问）。  
  这种嵌套作用域的链式查找机制称为**作用域链**，由函数定义时的位置决定（静态作用域/词法作用域）。

---

如果需要我针对第6、7、8题中你未给出的具体代码进行分析，可以把代码贴出来，我可以为你详细拆解执行顺序和输出结果。

16.