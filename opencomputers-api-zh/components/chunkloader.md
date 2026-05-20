# chunkloader

## 概述

`chunkloader` 组件是区块加载升级的控制接口。启用后，它会保持宿主所在区块及其周围八个区块持续加载，让机器人、微控制器等设备即使位于无人区域也能继续运行。

## 可用性

- 仓库：`opencomputers`
- Lua 组件名：`chunkloader`
- 常见宿主：机器人、微控制器，以及其他可以安装区块加载升级的设备

## 作用

- 持续加载以宿主当前位置为中心的 `3 x 3` 区块区域。
- 宿主移动时，加载区域会跟着一起移动。
- 计算机启动时会自动启用，计算机停止时会自动关闭。
- 如果设备无法持续支付区块加载的耗能，它会自动停用。

## 启用规则

- 如果当前维度被 OpenComputers 的区块加载白名单或黑名单限制，就不能启用该升级。
- 当 Lua 调用 `setActive(true)` 且当前维度被禁止时，调用会直接抛出错误。
- 如果启用后电力不足，升级会释放区块票据，`isActive()` 也会变成 `false`。

## API

### isActive

- 语法：`chunkloader.isActive()`
- 返回值：`boolean`
- 用途：检查当前是否持有有效的 Forge 区块加载票据。

### setActive

- 语法：`chunkloader.setActive(enabled)`
- 参数：
  - `enabled:boolean`：`true` 表示请求开始加载，`false` 表示释放加载状态。
- 返回值：`boolean`
- 用途：手动启用或禁用区块加载升级。

返回值含义：

- `true`：活动状态发生了变化。
- `false`：请求的状态原本就已经生效。

可能失败：

- 如果在被限制的维度中请求启用，会抛出错误消息 `this dimension is blacklisted`。

## 使用说明

- 对机器人这类可移动宿主来说，加载区会随着宿主一起迁移。
- 升级会保存自己的区块票据，世界重载后如果宿主正常重新连接，票据可以恢复。
- 无人机这类实体宿主不会依赖移动事件，而是通过周期更新刷新加载范围；对 Lua 使用者来说，表现效果是一致的。

## 示例

```lua
local component = require("component")
local loader = component.chunkloader

if not loader.isActive() then
  local ok, changed = pcall(loader.setActive, true)
  if not ok then
    error("chunkloader could not be enabled: " .. tostring(changed))
  end
  print("State changed:", changed)
end

print("Chunkloader active:", loader.isActive())
```

## 相关内容

- `robot`
- `microcontroller`
- `computer`
