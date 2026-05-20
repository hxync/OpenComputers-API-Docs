# cgroups

## 概述

Plan9k 用来限制组件可见性和模块加载状态的 cgroup 构造辅助函数。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\lib\modules\base\19_cgroups.lua`

## 用法

```lua
-- internal cgroup constructors exposed to Plan9k userspace helpers
```

## 接口

### cgroups.component(wl, bl)

- 说明: 创建一个继承当前线程组件可见性的组件组，并额外施加白名单或黑名单过滤。
- 参数:
  - `wl`: 可选，按组件地址建索引的白名单表。
  - `bl`: 可选，按组件地址建索引的黑名单表。

```lua
local group = cgroups.component(whitelist, blacklist)
```

### cgroups.module()

- 说明: 创建一个新的模块加载组，拥有独立的 preload 表、loaded 集、loading 集和 searcher 列表。

```lua
local group = cgroups.module()
```

## 备注

- 本页只列出模块导出的公开函数。
