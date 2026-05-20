# grep

## 概述

在文件、标准输入或递归目录扫描中查找匹配 Lua 模式的文本行。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\grep.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\grep`

## 语法

```lua
grep [OPTIONS] PATTERN [FILE...]
```

## 用途

使用一个或多个模式检查每一行输入，并按需输出匹配结果、文件名、行号、颜色、高亮、计数或递归扫描结果。和 POSIX grep 不同，这个实现默认使用 Lua 模式，而不是 POSIX 正则，除非显式切换到固定字符串模式。

## 参数

- `PATTERN`: 主搜索模式。默认情况下它会被当作 Lua 模式来解释。
- `FILE...`: 可选，要搜索的文件。使用 `-` 表示标准输入。省略时默认搜索 `-`，若启用 `-r` 则默认从 `.` 开始。

## 选项

- `-F, --fixed-strings`: 把模式当作普通字面字符串处理。
- `-e, --lua-regexp`: 即使原本会选中固定字符串模式，也强制继续使用 Lua 模式匹配。
- `--file=PATTERN_FILE`: 从 `PATTERN_FILE` 中按每行一个模式读取搜索条件。
- `-w, --word-regexp`: 要求匹配结果在实现的 `%a_` 词边界判断下构成完整单词。
- `-x, --line-regexp`: 要求整行完整匹配该模式。
- `-i, --ignore-case`: 把每个给定模式改写成大小写不敏感的 Lua 模式。
- `--label=NAME`: 当输入来自标准输入时，用 `NAME` 作为显示标签。
- `-s, --no-messages`: 抑制文件读取诊断信息。
- `-v, --invert-match`: 选择不匹配的行，而不是匹配的行。
- `--max-count=N`: 在当前输入流中命中 `N` 次后停止。
- `-n, --line-number`: 输出行号。
- `-r, --recursive`: 递归收集目录参数下的全部文件。
- `-l, --files-with-matches`: 只输出至少命中过一次的文件名。
- `-L, --files-without-match`: 只输出完全没有命中的文件名。
- `-H, --with-filename`: 始终在输出中显示文件名。
- `-h, --no-filename`: 在输出中隐藏文件名。
- `-o, --only-matching`: 只输出匹配到的片段，而不是整行。
- `-q, --quiet, --silent`: 抑制常规输出，仅通过退出码表示是否找到匹配。
- `-c, --count`: 输出每个输入源的命中次数。
- `--color, --colour`: 在输出到终端时使用颜色高亮匹配结果。
- `-t, --trim`: 在打印的匹配片段或行内容周围裁掉首尾空白。
- `-V, --version, --help`: 输出简短用法说明。

## 示例

```lua
grep hello notes.txt
```
在 `notes.txt` 中查找匹配 Lua 模式 `hello` 的行。

```lua
grep -i -n 'error' /var/log.txt
```
执行大小写不敏感搜索，并输出命中的行号。

```lua
grep -r -l 'TODO' /home/project
```
递归扫描 `/home/project`，并只输出至少命中过一次 `TODO` 的文件。

## 备注

- 递归模式只会收集文件，不会把目录本身当作一条可搜索记录。
- 退出码在“找到任意匹配”时为 `0`，在“完全没有匹配”时为 `1`，在用法或文件错误时为 `2`。
