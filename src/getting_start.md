# 快速开始

本章将指导你安装 Aitne CLI 工具链、初始化第一个项目，并启动本地开发服务器。

在开始之前，请确保已经安装 [MoonBit 工具链](https://www.moonbitlang.com/)。

## 安装

Aitne 提供了一个轻量级 CLI，用于管理项目生命周期、代码生成以及编译任务。

### macOS 和 Linux

你可以在终端中运行以下命令自动安装 Aitne CLI：

```bash
curl -fsSL https://raw.githubusercontent.com/Arcelyth/aitne/main/scripts/install.sh | bash
```

> ⚠️ **Windows 支持说明**
>
> `aitne` CLI 依赖的 MoonBit async 库目前尚不原生支持 Windows。
> 如果你使用 Windows，强烈建议通过 **WSL2（Windows Subsystem for Linux）** 运行 Aitne 工具链。

## 创建新项目

安装完成后，可以使用 `init` 子命令创建一个新的 Aitne 项目。该命令会生成标准的项目目录结构，并完成 MoonBit 与 MBX 开发所需的基础配置。

```bash
# 初始化一个名为 "your_project" 的新项目
aitne init your_project

cd your_project
```

## 构建项目

执行以下命令构建项目：

```bash
aitne build
```

在内部，Aitne 会扫描项目中的 `.mbx` 模板，生成对应的 MoonBit 实现文件，然后调用 MoonBit 后端编译器，生成优化后的 JavaScript/WebAssembly 构建产物。

## 运行开发服务器

要查看响应式 Web 应用的运行效果，可以启动内置的高性能 Web 服务器：

```bash
aitne run
```

默认情况下，该命令会启动本地服务器托管你的应用。打开浏览器并访问：

```text
http://localhost:8000
```

### 通过 `eirene.toml` 配置

`aitne run` 的运行行为，例如监听端口、路由规则以及静态资源目录等，由项目根目录下生成的 `eirene.toml` 文件控制。

你可以修改该文件，以自定义本地开发环境的相关配置。
