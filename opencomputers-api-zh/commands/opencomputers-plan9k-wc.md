# wc

## 概述

统计标准输入中的字节数、字符数、行数或单词数。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\wc.lua`

## 语法

```lua
wc
```
```lua
wc -c
```
```lua
wc -m
```
```lua
wc -l
```

## 用途

从当前标准输入流读取数据，并输出一种统计结果。默认输出单词数；使用选项后可以改为输出字节数、Unicode 字符数或行数。

## 选项

- `-c, --bytes`: 输出字节数。
- `-m, --chars`: 输出 Unicode 字符数。
- `-l, --lines`: 输出行数。

## 示例

```lua
echo hello | wc
```
对管道输入输出默认的单词统计值。

```lua
cat /etc/motd | wc -l
```
统计输入流产生了多少行。

## 备注

- 这个 Plan9k 实现只从标准输入读取数据，不直接接受文件路径参数。
