# assembler

## 概述

`assembler` 组件是装配机方块的 Lua 控制接口。它不会把所有配方规则逐条暴露给脚本，而是提供当前布局是否可装配的判断，以及在物料齐全时启动装配任务的能力。

## 可用性

- 仓库：`opencomputers`
- Lua 组件名：`assembler`
- 常见宿主：装配机方块

## 作用

- 用于组装机器人、无人机、平板、微控制器等高级 OpenComputers 设备。
- 根据当前放入的底座和部件，校验所选模板是否成立。
- 在消耗足够能量后，把输入材料转换为一个成品设备。

## 装配流程

1. 先把目标设备的底座物品放入装配机的第一个槽位。
2. 再把所需部件放入模板允许的其余槽位。
3. 调用 `status()` 检查当前布局是否已经满足装配条件。
4. 调用 `start()` 启动装配。
5. 等待能量消耗完成，成品会放回第一个槽位。

装配开始后，装配机会先把结果暂存在内部，然后随着时间持续消耗能量，直到整台设备完成。

## API

### status

- 语法：`assembler.status()`
- 返回值：
  - 正在装配时：`"busy", progressPercent`
  - 空闲且当前布局有效时：`"idle", true`
  - 空闲但当前布局无效时：`"idle", false`
- 用途：查看装配机当前是否正在工作，或者当前槽位布局是否已经可启动。

`progressPercent` 是 `0` 到 `100` 之间的数字百分比。

### start

- 语法：`assembler.start()`
- 返回值：`boolean`
- 用途：启动当前已经配置好的装配任务。

返回值含义：

- `true`：任务已成功开始。
- `false`：装配机正在忙、内部已有待输出结果，或者当前槽位布局不合法。

## 使用说明

- 装配机同一时间只能处理一个任务。
- 任务进行中时，不能再启动新的装配。
- 如果当前配方缺件或部件不匹配，`status()` 会返回 `"idle", false`，`start()` 也会返回 `false`。
- 装配速度取决于供电情况，因为机器会逐步扣除所需总能量。
- 成品完成后，会替换第一个槽位中的原始底座物品。

## 示例

```lua
local component = require("component")
local assembler = component.assembler

local state, value = assembler.status()
if state == "idle" and value == true then
  assert(assembler.start(), "failed to start assembly")
else
  error("assembler recipe is not ready")
end

repeat
  os.sleep(0.5)
  state, value = assembler.status()
  if state == "busy" then
    print(("Assembly progress: %.1f%%"):format(value))
  end
until state == "idle"

print("装配完成，请从第一个槽位取出成品。")
```

## 相关内容

- `disassembler`
- `robot`
- `drone`
- `tablet`
- `microcontroller`
