# edit

## 概述

打开一个简单的全屏文本编辑器。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\edit.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\edit`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\edit`

## 语法

```lua
edit FILE
```

## 用途

对单个文件启动内置编辑器。如果打开的是可写文件系统中一个尚不存在的路径，那么在保存时会创建这个新文件。

## 参数

- `FILE`: 要编辑或创建的文件路径。

## 选项

- `-r`: 以只读模式打开文件。

## 示例

```lua
edit /tmp/test.txt
```
打开 `/tmp/test.txt` 进行编辑；如果需要，会在保存时创建它。

```lua
edit -r /bin/ls.lua
```
以只读模式打开 `/bin/ls.lua`。
