# os_rot13

## 概述

对标准输入或文件内容执行 ROT13 变换。

## 可用性

- 仓库: `opensecurity`
- 系统: `datablock`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\datablock\bin\os_rot13.lua`

## 语法

```lua
os_rot13 [FILE...]
```

## 用途

完整读取标准输入或一个或多个文件，执行 `datablock.rot13`，并写出变换后的文本。

## 参数

- `FILE...`: 可选，要转换的文件。省略时从标准输入读取。

## 示例

```lua
os_rot13 secret.txt
```
对 `secret.txt` 执行 ROT13，并输出转换结果和文件名。
