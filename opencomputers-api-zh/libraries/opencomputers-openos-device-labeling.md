# device_labeling

## 概述

加载、持久化并应用类似 udev 的设备标签规则，用于设备和文件系统代理对象。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\core\device_labeling.lua`

## 用法

```lua
local device_labeling = require("device_labeling")
```

## 接口

### device_labeling.getDeviceLabel(proxy)

- 说明: 返回为某个代理对象显式记录的标签；如果没有规则，则回退到代理自身的 `getLabel` 方法。
- 参数:
  - `proxy`

```lua
device_labeling.getDeviceLabel(nil)
```

### device_labeling.loadRules(root_dir)

- 说明: 从配置的规则目录读取标签规则，并填充内存中的规则表。
- 参数:
  - `root_dir`

```lua
device_labeling.loadRules(nil)
```

### device_labeling.saveRule(rule_set, path)

- 说明: 把一组规则写回指定规则文件。
- 参数:
  - `rule_set`
  - `path`

```lua
device_labeling.saveRule(nil, nil)
```

### device_labeling.saveRules(rules)

- 说明: 把当前加载的全部规则集统一持久化。
- 参数:
  - `rules`

```lua
device_labeling.saveRules(nil)
```

### device_labeling.setDeviceLabel(proxy, label)

- 说明: 为代理对象设置一条标签规则；如果代理本身支持，则可直接调用它自己的 `setLabel`。
- 参数:
  - `proxy`
  - `label`

```lua
device_labeling.setDeviceLabel(nil, "value")
```

## 备注

- 这个辅助模块通常通过 `/lib/core/device_labeling.lua` 在内部被加载，而不是直接给最终用户程序显式 `require`。
