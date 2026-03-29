---
title: Ajax异步编程实战：从待办事项看前端数据交互与事件绑定
date: 2026-03-23 10:26:07
tags:

---

# 02前端异步编程与Ajax交互实战总结

本文档基于一个完整的待办事项列表示例，总结了Ajax请求、异步操作、DOM动态渲染及常见陷阱。

## 1. 异步操作的核心概念

### 1.1 同步 vs 异步
- **同步阻塞**：代码按顺序执行，前一个任务未完成，后续任务必须等待。
- **异步非阻塞**：任务（如Ajax、`setTimeout`）被挂起，待主线程空闲后再执行回调。

### 1.2 经典示例：`setTimeout` 与循环
```javascript
for (var i = 0; i < 5; i++) {
  setTimeout(function () {
    console.log(i); // 输出5个5
  }, 0);
}
```

**原因**：`setTimeout` 是异步任务，循环执行完毕后 `i` 已变为5，此时才执行回调。

**解决方案**：使用 `let` 创建块级作用域或使用立即执行函数（IIFE）。

## 2. 封装 HTTP 请求函数

代码中使用了自定义的 `httpRequest` 函数来统一处理请求，这是良好的实践。

javascript

```
// 假设 httpRequest 定义在外部 ajax.js 中
// 参数：method, url, data, callback
httpRequest('GET', 'http://localhost:3008/list', null, function (response) {
  // 处理响应
});
```



### 2.1 GET 请求

用于初始化加载数据列表。

javascript

```
httpRequest('GET', 'http://localhost:3008/list', null, function (response) {
  var data = JSON.parse(response);
  // 渲染列表...
});
```



### 2.2 POST 请求

用于添加新数据。

javascript

```
httpRequest('POST', 'http://localhost:3008/list', { name: inputvalue }, function (response) {
  var newData = JSON.parse(response);
  // 动态添加新项到DOM...
});
```



### 2.3 DELETE 请求

用于删除数据，URL 中通常包含资源 ID。

javascript

```
httpRequest('delete', `http://localhost:3008/list/${id}`, null, function (res) {
  item.parentNode.remove(); // 从DOM中移除
});
```



## 3. DOM 动态渲染与事件绑定

### 3.1 初始化渲染

在页面加载时，通过 GET 请求获取数据并渲染列表。

javascript

```
httpRequest('GET', 'http://localhost:3008/list', null, function (response) {
  var data = JSON.parse(response);
  for (var i = 0; i < data.length; i++) {
    var newListItem = document.createElement('li');
    // 关键：将数据ID存储在自定义属性中
    newListItem.innerHTML = `${data[i].name}<button class="del" data-id=${data[i].id}>删除</button>`;
    document.querySelector('.list').appendChild(newListItem);
  }
});
```



### 3.2 动态添加项

添加新项时，直接为新创建的删除按钮绑定事件。

javascript

```
document.querySelector('.addList').onclick = function () {
  var inputvalue = document.querySelector('.add_input').value;
  if (inputvalue === '') return;

  httpRequest('POST', 'http://localhost:3008/list', { name: inputvalue }, function (response) {
    var data = JSON.parse(response);
    var newListItem = document.createElement('li');
    newListItem.innerHTML = `${data.name}<button class="del">删除</button>`;
    
    // 重要：为新按钮单独绑定事件
    newListItem.querySelector('.del').onclick = function () {
      // 执行删除逻辑（通常需要对应的DELETE请求）
      console.log('删除成功');
    };
    
    document.querySelector('.list').appendChild(newListItem);
  });
};
```



## 4. 常见问题与最佳实践

### 4.1 问题：删除按钮事件无效（异步时序）

**错误示例**：在渲染循环中直接为所有 `.del` 按钮绑定事件，但此时DOM还未生成。

javascript

```
// ❌ 错误写法
for (...) {
  // 渲染DOM...
}
var delBtns = document.querySelectorAll('.del');
delBtns.forEach(...); // 如果渲染是异步的，这里可能获取不到元素
```



**解决方案**：

1. 在 **回调内部**（数据返回后）完成DOM渲染和事件绑定。
2. 使用**事件委托**，将事件绑定到父容器上。

### 4.2 最佳实践：事件委托

将事件监听器附加到稳定的父元素（如 `ul.list`）上，通过判断点击目标来执行逻辑。

javascript

```
document.querySelector('.list').addEventListener('click', function (e) {
  if (e.target && e.target.classList.contains('del')) {
    var id = e.target.getAttribute('data-id');
    // 发送 DELETE 请求
    httpRequest('DELETE', `http://localhost:3008/list/${id}`, null, function () {
      e.target.parentNode.remove(); // 删除对应的li
    });
  }
});
```



### 4.3 数据ID的传递

在动态生成的删除按钮上，使用 `data-id` 属性存储数据的主键ID，便于删除时使用。

html

```
<button class="del" data-id="1">删除</button>
```



## 5. 整体代码执行流程

1. **页面加载** → 执行 GET 请求获取已有数据 → 渲染列表。
2. **点击“添加”按钮** → 获取输入值 → 发送 POST 请求 → 服务器返回新数据 → 将新项追加到列表末尾。
3. **点击“删除”按钮** → 获取对应 ID → 发送 DELETE 请求 → 请求成功后从 DOM 中移除该项。

## 6. 总结

| 知识点         | 说明                                  |
| -------------- | ------------------------------------- |
| **Ajax 封装**  | 统一处理请求，简化代码，提高复用性    |
| **异步处理**   | 所有DOM更新必须在异步请求的回调中进行 |
| **事件委托**   | 解决动态元素的事件绑定问题，提升性能  |
| **自定义属性** | 使用 `data-*` 存储数据ID，便于操作    |
| **JSON 解析**  | `JSON.parse()` 将响应字符串转为对象   |

------

**延伸思考**：现代前端开发中，通常使用 `fetch` API 或 `axios` 库替代原生 `XMLHttpRequest`，并配合 `async/await` 语法更优雅地处理异步流程。













