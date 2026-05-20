# resolution

## 概述

输出当前屏幕分辨率，或切换到新的分辨率。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\resolution.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\resolution`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\resolution`

## 语法

```lua
resolution
```
```lua
resolution WIDTH HEIGHT
```

## 用途

不带参数时输出当前启用的宽度和高度。带两个数字参数时，会向当前 GPU 请求切换分辨率，并在切换成功后清空终端。

## 参数

- `WIDTH`: 目标屏幕字符宽度。
- `HEIGHT`: 目标屏幕字符高度。

## 示例

```lua
resolution
```
输出当前屏幕分辨率。

```lua
resolution 80 25
```
请求切换到 `80x25` 文本分辨率，并在成功后清屏。

## 备注

- 请求的尺寸必须被当前绑定的 GPU 和屏幕共同支持。
- 如果尺寸非法或不受支持，命令会输出错误，而不会修改显示设置。
