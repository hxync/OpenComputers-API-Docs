# hostname

## 概述

显示、设置或刷新机器主机名。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\hostname.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\hostname`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\hostname`

## 语法

```lua
hostname [NEW NAME] [--update]
```

## 用途

从 `/etc/hostname` 读取主机名、把新值写回该文件，或者在使用 `--update` 时，把磁盘上的值重新同步到 `HOSTNAME` 相关环境变量中。

## 参数

- `NEW NAME`: 可选，要写入 `/etc/hostname` 的主机名。

## 选项

- `--update`: 从 `/etc/hostname` 刷新 `HOSTNAME` 相关环境变量，并且不向标准输出打印结果。

## 示例

```lua
hostname
```
输出当前配置的主机名。

```lua
hostname test
```
把机器主机名设置为 `test`。

```lua
hostname --update
```
从 `/etc/hostname` 重新加载与主机名相关的环境变量。

## 备注

- 如果当前没有配置主机名，同时也没有提供新值，命令会报告错误。
- 设置新主机名时会先写文件，只有启用 `--update` 时才会同步更新环境变量。
