# gpg

## 概述

使用数据卡的密码学能力进行加密、解密、签名、验签和密钥生成。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\usr\bin\gpg.lua`

## 语法

```lua
gpg -ce FILE
```
```lua
gpg -cd FILE
```
```lua
gpg -e KEY FILE
```
```lua
gpg -d KEY FILE
```
```lua
gpg -g PRIVATE_KEY_FILE PUBLIC_KEY_FILE
```
```lua
gpg -s KEY FILE
```
```lua
gpg -v KEY FILE
```

## 用途

封装内置数据卡程序提供的多种密码学工作流：基于密码的加密、非对称加密、ECDSA 签名、签名校验以及密钥对生成。

## 参数

- `FILE`: 要加密、解密、签名或验签的输入文件。
- `KEY`: 用于公钥加密、私钥解密、签名或验签的密钥文件。
- `PRIVATE_KEY_FILE`: 生成出的私钥保存路径。
- `PUBLIC_KEY_FILE`: 生成出的公钥保存路径。

## 选项

- `-ce`: 通过交互输入密码来加密文件。
- `-cd`: 解密使用密码加密的文件。
- `-e`: 使用给定公钥加密文件。
- `-d`: 使用给定私钥解密文件。
- `-g`: 生成一组 384 位密钥对，并写出私钥/公钥文件。
- `-s`: 对文件签名，并写出同名 `.sig` 签名文件。
- `-v`: 使用同名 `.sig` 签名文件校验目标文件。

## 示例

```lua
gpg -g id_rsa id_rsa.pub
```
生成一组私钥/公钥文件。

```lua
gpg -ce secrets.txt
```
提示输入密码，并生成 `secrets.txt.gpg`。

```lua
gpg -e id_rsa.pub report.txt
```
使用 `id_rsa.pub` 中的公钥加密 `report.txt`。

```lua
gpg -s id_rsa report.txt
```
使用 `id_rsa` 中的私钥为 `report.txt` 签名。

```lua
gpg -v id_rsa.pub report.txt
```
使用 `id_rsa.pub` 中的公钥校验 `report.txt.sig`。

## 备注

- 程序要求机器具备数据卡，而且不同工作流还要求数据卡提供对应的更高阶方法。
- 生成出的目标文件如果已经存在，程序会拒绝覆盖。
- 密码模式解密时，如果源文件名以 `.gpg` 结尾，会恢复到原始基名；否则会在输出名后追加 `.dec`。
