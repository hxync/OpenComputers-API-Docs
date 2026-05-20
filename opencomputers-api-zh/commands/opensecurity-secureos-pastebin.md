# pastebin

## 概述

从 Pastebin 下载程序、上传程序，或直接运行远程贴子内容。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\pastebin.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\pastebin`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\pastebin`

## 语法

```lua
pastebin get PASTE_ID FILE
```
```lua
pastebin put FILE
```
```lua
pastebin run PASTE_ID [ARGUMENT...]
```

## 用途

支持三种工作流：`get` 把贴子下载到本地文件，`put` 把本地文件上传为新的 Pastebin 贴子，`run` 则先把贴子下载到临时文件，再通过 shell 执行。

## 参数

- `PASTE_ID`: 下载或执行时使用的 Pastebin 贴子标识。
- `FILE`: 上传时的本地源文件，或下载时保存内容的目标文件。
- `ARGUMENT...`: 使用 `run` 时转发给已下载程序的额外参数。

## 选项

- `-f`: 在执行 `get` 时允许覆盖已存在的目标文件。
- `-k`: 保持下载内容原始换行符，不把 Windows 的 CRLF 规范化为 Unix 的 LF。

## 示例

```lua
pastebin get AbCdEfGh /home/test.lua
```
下载贴子 `AbCdEfGh`，并保存为 `/home/test.lua`。

```lua
pastebin put prog.lua
```
上传 `prog.lua`，并输出生成出的 Pastebin 地址和贴子标识。

```lua
pastebin run AbCdEfGh foo bar
```
下载贴子 `AbCdEfGh` 并立即执行，同时把 `foo bar` 作为位置参数传入。

## 备注

- 三个子命令都要求机器可用 `internet` 组件。
- 使用 `run` 时，下载内容会先写入临时路径，并在执行结束后删除。
- 上传时会读取 `/etc/pastebin.conf` 中的 `key`；如果没有配置，则回退到程序内置的默认开发者密钥。
