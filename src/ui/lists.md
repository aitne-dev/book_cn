# 列表

你可以使用 `<ForEach>` 组件来渲染响应式列表。当列表中的项目被添加、删除或重新排序时，只有受影响的 DOM 节点会发生变化。

## 基本用法

```mbx
<ul>
  <ForEach
    each={() => items.get().iter()}
    key={(item) => item}
    children={(item) => {
      <li>{ () => item }</li>
    }}
  />
</ul>
```

三个属性：

* **each**：返回迭代器的函数
* **key**：将每个项目映射到唯一标识符
* **children**：渲染函数，将每个项目转换为一个视图

## Todo 示例

```mbx
fn todo_app() -> &View {
  let (input_val, set_input_val) = @reactive.create_signal("")
  let (todo, set_todo) = @reactive.create_signal(["Example."])

  let add_item = _ => {
    let text = input_val.get()
    if text != "" {
      set_todo.set(todo.get() + [text])
    }
    set_input_val.set("")
  }

  let remove_item = item => {
    let new_list = todo.get().filter(fn(t) { t != item })
    set_todo.set(new_list)
  }

  <div>
    <h1> Todo List </h1>
    <div>{() => todo.get().length().to_string() }</div>
    <div>
      <input 
        type="text" 
        value={input_val} 
        on:input={ev => set_input_val.set(event_target_value(ev))} 
      />
      <button on:click={add_item}> Add </button>
    </div>
    <ul>
      <ForEach 
        each={()=>todo.get().iter()}
        key={(x)=>{return x}}
        children={(item) => {
          <li>
            {() => item}
            <button on:click={_ => remove_item(item)}> "Delete" </button>
          </li>

        }}
      />
    </ul>
  </div>
}
```

## 基于 Key 的更新

列表项通过 **key** 而不是索引进行跟踪。这意味着：

* 在列表开头添加项目不会重新渲染已有项目
* 删除项目时只会删除对应的 DOM 节点
* 重新排序时只会移动 DOM 节点，而不会重新创建它们

## 工作原理

当数据数组发生变化时，Aitne 会计算新旧 **key 集合**之间的差异。随后，它只将必要的 DOM 变更直接应用到页面上——包括插入、删除和移动，而不会触碰未发生变化的节点。
