# install

## 概述

把文件从一个文件系统安装到另一个文件系统，并可选应用元数据驱动的安装行为。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\install.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\install`

## 语法

```lua
install [NAME] [OPTIONS]...
```

## 用途

构建候选源文件系统和目标文件系统列表，并可选按标签或显式路径筛选，然后把源树复制到目标树中。如果源根目录包含 `.prop` 或 `.install`，安装器还可以设置标签、决定启动设备行为、控制重启逻辑，甚至把流程交给一个完全自定义的安装脚本。

## 参数

- `NAME`: 可选的源标签选择器，会对 `.prop` 元数据中的标签进行不区分大小写匹配。

## 选项

- `--from=ADDR`: 通过 UUID 或显式根路径（如 `/tmp`）选择源文件系统。
- `--to=ADDR`: 通过 UUID 或显式根路径选择目标文件系统。
- `--fromDir=PATH`: 把所选源文件系统中的一个相对子目录当作安装根。默认为 `.`。
- `--root=PATH`: 把目标文件系统中的一个相对子目录当作安装目标根。
- `--toDir=PATH`: `--root=PATH` 的别名。
- `-u, --update`: 在修改文件前进行提示，相当于把交互式/更新式标志转发给底层复制步骤。
- `--label=LABEL`: 覆盖原本会从 `.prop` 读取到的标签值。
- `--nosetlabel`: 不要设置目标文件系统标签。
- `--nosetboot`: 不要把目标文件系统设为默认启动设备。
- `--noreboot`: 安装完成后不要自动重启。

## 示例

```lua
install
```
提示选择源文件系统和目标文件系统，然后执行默认安装流程。

```lua
install openos
```
查找 `.prop` 标签匹配 `OpenOS` 的安装源，并引导你选择目标文件系统。

```lua
install --from=/tmp --to=/mnt/disk --root=usr
```
从 `/tmp` 安装到 `/mnt/disk` 的 `usr` 子目录中。

## 备注

- 源 `.prop` 文件可以定义 `label`、`setlabel`、`setboot`、`reboot` 和 `ignore` 等字段。
- 如果源根目录包含 `.install`，安装器会执行该脚本，而不是执行默认的递归复制流程。
- 自定义 `.install` 脚本会通过 `_ENV.install` 获得配置，包括 `from`、`to`、`fromDir`、`root`、`update`、`label`、`setlabel`、`setboot` 和 `reboot`。
