# primary

## 概述

查询或切换某种组件类型的主组件。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\primary.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\primary`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\primary`

## 语法

```lua
primary TYPE
```
```lua
primary TYPE ADDRESS
```

## 用途

只提供组件类型时，命令会输出该类型当前主组件的地址。额外提供地址时，命令会先切换主组件，再输出新的主组件地址。

## 参数

- `TYPE`: 组件类型，例如 `gpu` 或 `filesystem`。
- `ADDRESS`: 可选，完整或缩写形式的组件地址，用于设为主组件。

## 示例

```lua
primary gpu
```
输出当前主 GPU 的地址。

```lua
primary gpu 24a
```
把地址以 `24a` 开头的 GPU 设为新的主 GPU。

## 备注

- 只要能够唯一匹配，组件地址可以使用缩写前缀。
- 切换主组件后脚本会短暂休眠，给组件信号处理留出时间。
- 注意：地址可以使用缩写形式，只要能够唯一匹配。
