# 动态更新

Aitne 会在响应式值发生变化的位置**精准更新 DOM**，无需进行 Virtual DOM Diff。

## 响应式文本

在 `{}` 中使用闭包可以创建响应式文本绑定。当 signal 发生变化时，该文本会自动更新：

```mbx
<p>{ () => count.get().to_string() }</p>
```

每当 `count` 发生变化时，只有这个文本节点会更新，而不会重新更新父元素或其兄弟节点。

## 响应式属性

将闭包传递给属性，即可使该属性具有响应性：

```mbx
<div class={() =>
  if selected.get() { "active" } else { "inactive" }
}>
```

当 `selected` 发生变化时，只有 `class` 属性会被更新。

## 响应式 Property

可以通过 `value` 或 `prop` 绑定 DOM Property：

```mbx
<input value={input_val} />
```

## 条件渲染

直接使用标准的 MoonBit 条件语句，无需额外的指令或特殊语法：

```mbx
fn user_greeting(user : User?) -> &View {
  match user {
    Some(u) => <h1>"Welcome, \{u.name}!"</h1>
    None => <h1>"Welcome, Guest!"</h1>
  }
}
```

## 原理

与需要对整个组件树进行 Diff 的 Virtual DOM 框架不同，Aitne 会将模板编译为**精确的 DOM 操作**：

1. 每个响应式表达式都会绑定到特定的 DOM 节点或属性
2. 当 signal 发生变化时，只更新对应的 DOM 节点或属性
3. 不需要 Diff，也不需要 Reconciliation，只执行针对性的更新操作

例如，在包含 **1000 个项目的列表**中更新一个 signal，只会修改对应的**一个文本节点**。
