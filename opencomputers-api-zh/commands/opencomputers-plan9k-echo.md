# echo

## 概述

把文本输出到标准输出。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\echo.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\echo`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\echo`

## 语法

```lua
echo [STRING...]
```

## 用途

把给出的字符串写到标准输出。默认情况下 `echo` 会在末尾附加换行；使用 `-n` 时则不会补这个结尾换行。

## 参数

- `STRING...`: 按顺序输出的一个或多个文本片段。

## 选项

- `-n`: 不要输出末尾换行。
- `--help`: 输出内置帮助文本后退出。

## 示例

```lua
echo test
```
输出 `test`，然后换行。

```lua
echo "a   b"
```
按原样输出文本 `a   b`。

```lua
echo -n prompt>
```
输出 `prompt>`，但不换到下一行。
