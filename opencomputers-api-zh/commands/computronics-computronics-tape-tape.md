# tape

## 概述

操作 Computronics 磁带机，查看或修改磁带元数据，并把音频字节写入磁带。

## 可用性

- 仓库：`computronics`
- 系统：`computronics-tape`
- 类型：`command`

## 来源

- `Computronics/src\main\resources\assets\computronics\loot\tape\usr\bin\tape.lua`
- `Computronics/src\main\resources\assets\computronics\doc\opencomputers\computronics\en_US\item\tape.md`
- `Computronics/src\main\resources\assets\computronics\doc\opencomputers\computronics\zh_CN\item\tape.md`

## 语法

```lua
tape play
```

```lua
tape pause
```

```lua
tape stop
```

```lua
tape rewind
```

```lua
tape wipe
```

```lua
tape label [NAME]
```

```lua
tape speed SPEED
```

```lua
tape volume VOLUME
```

```lua
tape write SOURCE
```

## 用途

控制磁带播放状态，回卷和擦除当前插入的磁带，读取或修改磁带标签，调整播放速度与音量，并把本地文件或 HTTP URL 中的字节流写入选定的磁带机。

## 参数

- `NAME`：可选的磁带标签。和 `label` 子命令搭配时，如果省略，则输出当前标签，或者提示这盘磁带还没有标签。
- `SPEED`：播放速度，允许范围是 `0.25` 到 `2.0`。
- `VOLUME`：播放音量，允许范围是 `0.0` 到 `1.0`。
- `SOURCE`：要写入磁带的本地文件路径，或 `http://` / `https://` URL。

## 选项

- `--address=ADDRESS`：使用指定的 `tape_drive` 组件，而不是默认主磁带机。
- `--b=BYTES`：写入时的块大小，默认 `2048` 字节。
- `--t=SECONDS`：从 URL 写入时等待连接建立的最长时间，默认 `5` 秒。
- `-y`：在 `wipe` 和 `write` 这类破坏性操作前跳过确认提示。

## 示例

```lua
tape play
```

如果磁带当前没有播放，就开始播放已插入的磁带。

```lua
tape label DemoTape
```

把磁带标签设为 `DemoTape`。

```lua
tape write /home/music.dfpwm
```

把 `/home/music.dfpwm` 的内容写入磁带。

```lua
tape --address=123 write https://example.com/music.dfpwm
```

从 HTTP 地址下载字节流，并写入指定地址的磁带机。

## 备注

- 机器上必须存在 `tape_drive` 组件，而且所选磁带机里必须已经插入可用磁带。
- `pause` 只暂停播放，不回卷；`stop` 会停止播放并回卷到开头。
- `wipe` 会按 `8192` 字节块把整盘磁带覆盖为 `0xAA`，然后回卷到起点。
- 如果写入源比磁带容量更大，命令只会写到磁带最大容量为止。
- 从 URL 写入时还要求机器具备 `internet` 组件。
