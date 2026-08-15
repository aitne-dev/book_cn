# 事件

在 MBX 中，可以使用 `on:` 命名空间处理用户交互事件。

## 基本事件处理器

```mbx
<button on:click={fn(ev) {
  println("Button clicked!")
}}>
  "Click me"
</button>
```

## 读取输入值

可以使用事件辅助函数读取表单数据：

```mbx
let (value, set_value) = @reactive.create_signal("")

<input
  value={value}
  on:input={fn(ev) {
    set_value.set(@ffi.event_target_value(ev))
  }}
/>
```

## 事件类型

| MBX 事件       | 触发时机    |
| ------------ | ------- |
| `on:click`   | 元素被点击   |
| `on:input`   | 输入值发生变化 |
| `on:change`  | 选择项发生变化 |
| `on:submit`  | 表单提交    |
| `on:keydown` | 按键按下    |
| `on:keyup`   | 按键释放    |

## Window 事件

可以订阅 `window` 事件，并由响应式系统自动处理事件清理：

```mbt
@reactive.create_effect(fn(_) {
  let cleanup = @dom.on_window_event("resize", fn(ev) {
    ...
  })
  @reactive.on_cleanup(cleanup)
})
```

当 effect 被清理时，对应的 `window` 事件监听器也会自动移除。

## 阻止默认行为

可以在事件处理器中调用 `event_prevent_default` 来阻止浏览器的默认行为：

```mbx
<form on:submit={fn(ev) {
  @ffi.event_prevent_default(ev)
  ...
}}>
```
