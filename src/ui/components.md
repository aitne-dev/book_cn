# 组件

## 在 MBX 中编写标记

MBX 是一种类似 JSX、但专为 MoonBit 设计的标记语法。MBX 文件使用 `.mbx` 扩展名。

你可以在 `{}` 代码块中嵌入 MoonBit 代码。

```mbx
fn app() -> &View {
  let content = "Let's try something."
  <div class="container">
    <h1> Welcome To Aitne </h1>
    <div> {content} </div>
  </div>
}
```

## 创建组件

组件是一个可复用的 UI 单元。在 Aitne 中，组件本质上就是一个返回 `&View` 的函数。

下面是一个简单的例子：

```moonbit
using @dom { trait View }

fn hello_world() -> &View {
  <h1>"Hello, World!"</h1>
}
```

组件的使用方式与 HTML 元素类似。标签既可以使用函数的原始名称，也可以使用 `PascalCase` 形式。

```mbx
fn app() -> &View {
  <hello_world />
  <HelloWorld />
}
```

组件之间也可以进行嵌套：

```mbx
fn app() -> &View {
  <div> 
    <HelloWorld />
  </div>
}
```

## 组件命名

在 MBX 中，组件标签会在编译过程中从 `PascalCase` 转换为 `snake_case`：

* `<MyComponent />` 会调用 `my_component(...)`
* `<HelloWorld />` 会调用 `hello_world(...)`

这样，MoonBit 中采用 `snake_case` 命名的函数就可以通过类似 JSX 的 `PascalCase` 标签来调用。
