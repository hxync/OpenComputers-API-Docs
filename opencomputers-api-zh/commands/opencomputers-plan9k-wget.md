# wget

## 概述

使用网络卡通过 HTTP 下载文件。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\wget.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\wget`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\wget`

## 语法

```lua
wget URL [FILE]
```

## 用途

通过 `internet` 库请求目标 URL，并将响应内容流式写入本地文件。如果没有显式给出目标文件名，命令会根据 URL 自动推导文件名。

## 参数

- `URL`: 要下载的 HTTP 地址。
- `FILE`: 可选，本地保存文件名或目标路径。

## 选项

- `-f`: 如果目标文件已存在，则强制覆盖。
- `-q`: 安静模式。隐藏状态输出，但仍显示错误。
- `-Q`: 超级安静模式。状态信息和错误信息都不输出。

## 示例

```lua
wget http://example.com/data.zip
```
下载 `data.zip`，并使用 URL 推导出的文件名保存。

```lua
wget -f http://example.com/data.zip /tmp/archive.zip
```
下载文件，并在 `/tmp/archive.zip` 已存在时强制覆盖。

## 备注

- 命令要求当前机器可用 `internet` 组件。
- 如果在创建输出文件后下载失败，命令会删除未完成的残留文件。
