# opencomputers

## 概述

本页是基础 OpenComputers 发行内容的系统级总索引。它不再列出冗长的原始资源路径，而是说明基础模组自带内容的组织方式，以及编写 Lua 程序时应该去哪里查找真正需要的 API 文档。

## 覆盖范围

`opencomputers` 系统主要覆盖基础 OpenComputers 模组自带的内容，包括：

- 核心运行时库，例如 `computer`、`component`、`event`
- 内置组件，例如 GPU、屏幕、文件系统、机器人、红石设备
- OpenOS 命令和自带的用户态程序
- 标准方块、物品、升级和系统机制相关说明

## 如何使用这套文档

在编写 OpenComputers Lua 程序时，通常应优先查找以下目录：

- `core-runtime/`
  这里记录机器级运行时库，例如 `computer`。
- `components/`
  这里记录方块、卡片、升级和设备暴露出的组件接口。
- `commands/`
  这里记录 OpenOS、Plan9k、SecureOS 等系统自带命令。
- `libraries/`
  这里记录操作系统附带的可复用 Lua 模块。
- `systems/`
  这里记录像本页这样的系统总览，用于理解大范围功能的结构。

## 基础模组的主要内容

### 运行时 API

这类接口通常是所有脚本最先依赖的基础能力：

- `computer`：内存、运行时长、能量、用户、架构
- `component`：组件查询和代理访问
- `event`：信号队列与事件处理
- `unicode`：Unicode 安全字符串处理

### 核心组件

这些组件页描述了最常见、最常直接使用的接口：

- 显示与输入：`gpu`、`screen`、`keyboard`
- 存储：`filesystem`、`drive`、`eeprom`
- 机器人与移动：`robot`、`navigation`、`inventory_controller`、`tractor_beam`
- 网络：`modem`、`internet`、链路卡、无线网卡
- 传感器与世界访问：`geolyzer`、`motion_sensor`、`waypoint`
- 自动化：`redstone`、`transposer`、`tank_controller`

### 操作系统与用户态程序

基础模组还附带了大量可直接使用的用户态内容：

- OpenOS 命令，例如 `ls`、`cp`、`find`、`ps`、`free`
- 这些命令以及普通 Lua 程序会调用到的标准库
- 安装脚本与示例程序

## 说明

- 本页是系统总览页，不再保留原始源码资源清单。
- 具体 API 的语法、参数、返回值和示例应查看对应的组件页、命令页或库页。
- 如果你正在寻找某个可调用接口，应优先进入对应的 `components/`、`commands/`、`libraries/` 或 `core-runtime/` 页面。

## 推荐起点

- 刚开始写 OpenComputers 脚本：
  先看 `core-runtime/computer.md`、`core-runtime/events.md`，再看 `gpu`、`screen`、`filesystem` 等基础组件页。
- 正在编写机器人自动化：
  先看 `components/robot.md`、`components/opencomputers-robot-proxy.md`，以及导航、库存、红石相关组件页。
- 正在排查系统行为：
  先查对应组件页，再查相关命令页或库页，看这个行为是由底层组件直接提供，还是通过 OpenOS 工具封装出来的。
