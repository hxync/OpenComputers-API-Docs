# ping

## 概述

通过内置 MCNET 软件网络栈发送类似 ICMP 的探测包。

## 可用性

- 仓库：`opencomputers`
- 系统：`network`
- 类型：`command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\network\data\bin\ping.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\network\data\usr\man\ping`

## 语法

```lua
ping ADDRESS [--count=N] [--size=BYTES] [--interval=SECONDS] [--droptime=SECONDS] [-v]
```

## 用途

生成一段随机负载，通过 `network.icmp.ping` 发往目标主机，等待匹配的 `ping_reply` 事件，然后输出往返耗时以及最终丢包统计。

这个实现支持自定义探测次数、负载大小、发包间隔、丢包超时以及详细日志输出。

## 参数

- `ADDRESS`：要探测的远端 MCNET 主机地址。

## 选项

- `--c=N`, `--count=N`：发送的探测次数。
- `--s=BYTES`, `--size=BYTES`：每个探测包的负载字节数，默认 `56`。
- `--i=SECONDS`, `--interval=SECONDS`：两次探测之间的等待时间，默认 `1` 秒。
- `--t=SECONDS`, `--droptime=SECONDS`：单个探测在多少秒后判定为丢失，默认 `8` 秒。
- `-v`, `--verbose`：输出更详细的发送、丢失和异常回包信息。
- `-h`, `--help`：显示内置帮助文本。

## 示例

```lua
ping localhost
```

通过软件网络栈对本机执行 ping。

```lua
ping somehost --count=3 --size=128
```

向 `somehost` 发送 3 个探测包，并把负载大小设为 128 字节。

```lua
ping relay-01 -v --droptime=2
```

以详细模式探测 `relay-01`，并把单包超时缩短到 2 秒。

## 备注

- 当没有显式指定次数时，命令默认发送 `8` 个探测包。
- 负载内容是随机字节流；如果生成出 `:` 字符，会自动替换为 `_`，避免干扰回包匹配逻辑。
- 如果收到的回包内容和发出的负载不一致，命令会把它统计为 `malformed` 异常回包。
