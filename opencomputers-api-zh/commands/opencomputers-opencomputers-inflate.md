# inflate

## 概述

使用数据卡的 inflate 例程解压数据。

## 可用性

- 仓库: `opencomputers`
- 系统: `opencomputers`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\data\usr\bin\inflate.lua`

## 语法

```lua
inflate
```
```lua
inflate FILE
```

## 用途

把标准输入或指定文件一次性完整读入内存，然后将 inflate 解压后的结果写到标准输出。

## 参数

- `FILE`: 可选，要解压的文件。省略时从标准输入读取。

## 示例

```lua
inflate document.deflated > document.txt
```
解压 `document.deflated`，并把明文结果重定向到 `document.txt`。

```lua
cat document.deflated | inflate
```
解压管道输入的数据流。

## 备注

- 这个实现会先把完整输入加载到内存里，再执行解压。
