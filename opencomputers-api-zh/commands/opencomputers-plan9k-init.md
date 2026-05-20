# init

## 概述

启动 Plan9k 的用户空间、服务、挂载逻辑和登录终端。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\init.lua`

## 语法

```lua
init
```

## 用途

Plan9k 把这个脚本作为首个用户态进程运行。它会初始化核心环境变量、创建 `/root`、读取主机名、启动已配置服务、建立伪终端、拉起 `getty`、`readkey` 和 `sh`，把文件系统挂载到 `/mnt` 下，并持续响应组件热插拔事件。

## 示例

```lua
init
```
启动完整的 Plan9k 用户空间会话管理流程。

## 备注

- 这个脚本设计目标就是系统级 `init` 进程，通常不应该在已经运行中的会话里手工再次启动。
- 新的文件系统组件会自动挂载到 `/mnt/<地址前缀>` 下，而 GPU 或屏幕组件接入后，只要能配成对，就会自动生成新的终端会话。
