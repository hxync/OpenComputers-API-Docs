# dd

## 概述

按固定块大小在文件或标准流之间复制原始数据。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\dd.lua`

## 语法

```lua
dd [if=INPUT] [of=OUTPUT] [bs=BYTES] [count=BLOCKS] [wait=SECONDS]
```

## 用途

把每个参数都解析为 `key=value` 形式，然后把数据从输入端复制到输出端。命令既可以从标准输入读取、写到标准输出，也可以限制复制块数、在块之间延时，并在结束时输出传输统计。

## 参数

- `if=INPUT`: 输入文件路径。使用 `-` 或省略时表示从标准输入读取。
- `of=OUTPUT`: 输出文件路径。使用 `-` 或省略时表示写到标准输出。
- `bs=BYTES`: 每次读取和写入的块大小，单位为字节。默认值是 `128`。
- `count=BLOCKS`: 最多复制的块数。默认不限，直到输入结束。
- `wait=SECONDS`: 可选，每复制完一块后额外等待的秒数。

## 示例

```lua
dd if=/tmp/a.bin of=/tmp/b.bin bs=512
```
按 512 字节块把 `/tmp/a.bin` 复制到 `/tmp/b.bin`。

```lua
cat image.bin | dd of=/tmp/image.copy bs=1024
```
从标准输入读取二进制数据，并保存到 `/tmp/image.copy`。

```lua
dd if=/tmp/a.bin of=- count=4 bs=256
```
把 `/tmp/a.bin` 前四个 256 字节块写到标准输出。

## 备注

- 所有参数都必须使用 `key=value` 形式；普通位置参数会被直接判定为非法参数。
- 统计输出按“每次成功写入都写满整块”计算，因此当输入最后一块不足整块时，最终字节数可能会偏大。
