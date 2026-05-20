# components

## 概述

列出可见组件，并可选地查看它们的可调用成员。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\components.lua`

## 语法

```lua
components [FILTER...]
```
```lua
components -l [FILTER...]
```
```lua
components --limit=COUNT [FILTER...]
```

## 用途

枚举 `component.list()` 返回的可见组件。可选过滤参数可以限制组件类型；`-l` 会展开显示每个组件的成员和文档；`--limit` 则会在达到指定数量后停止。

## 参数

- `FILTER...`: 可选，组件类型过滤条件，例如 `gpu`、`filesystem` 或 `modem`。

## 选项

- `-l`: 显示组件成员，以及每个成员对应的 `component.doc(address, member)` 结果。
- `--limit=COUNT`: 在输出至多 `COUNT` 个匹配组件地址后停止。

## 示例

```lua
components
```
列出当前所有可见组件的地址和类型。

```lua
components filesystem gpu
```
只列出文件系统组件和 GPU 组件。

```lua
components -l --limit=1 filesystem
```
显示一个文件系统组件，并展开列出其可调用成员和内联文档。

## 备注

- 如果多个过滤条件命中了同一个组件，命令会按地址去重后再输出。
- 成员展开依赖 `component.proxy`，因此只会显示运行时代理对象上可见的成员。
