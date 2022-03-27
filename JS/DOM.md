# DOM

## 常见DOM操作

### 1.  DOM的获取

```js
getElementById // 按照 id 查询
getElementsByTagName // 按照标签名查询
getElementsByClassName // 按照类名查询
querySelectorAll // 按照 css 选择器查询
```

### 2.DOM的创建

```js
// 设置 span 节点的内容
targetSpan.innerHTML = 'hello world'
// 把新创建的元素塞进父节点里去
container.appendChild(targetSpan)
```

### 3. DOM的删除

```js
container.removeChild(targetNode)
```

### 4. DOM的修改

```js
// 交换两个元素，把 content 置于 title 前面
container.insertBefore(content, title)
```

