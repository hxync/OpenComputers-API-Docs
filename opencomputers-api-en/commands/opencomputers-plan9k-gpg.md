# gpg

## Summary

Use data-card cryptography helpers to encrypt, decrypt, sign, verify, and generate keys.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\usr\bin\gpg.lua`

## Syntax

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

## Purpose

Expose several cryptographic workflows implemented on the bundled data-card program: password-based encryption, asymmetric encryption, ECDSA signing, signature verification, and keypair generation.

## Parameters

- `FILE`: Input file to encrypt, decrypt, sign, or verify.
- `KEY`: Key file used for public-key encryption, private-key decryption, signing, or verification.
- `PRIVATE_KEY_FILE`: Destination path for the generated private key.
- `PUBLIC_KEY_FILE`: Destination path for the generated public key.

## Options

- `-ce`: Encrypt a file using a password entered interactively.
- `-cd`: Decrypt a password-encrypted file.
- `-e`: Encrypt a file using the supplied public key.
- `-d`: Decrypt a file using the supplied private key.
- `-g`: Generate a 384-bit keypair and write private/public key files.
- `-s`: Sign a file and write a sibling `.sig` signature file.
- `-v`: Verify a file against its sibling `.sig` signature file.

## Examples

```lua
gpg -g id_rsa id_rsa.pub
```
Generate a private/public keypair.

```lua
gpg -ce secrets.txt
```
Prompt for a password and write `secrets.txt.gpg`.

```lua
gpg -e id_rsa.pub report.txt
```
Encrypt `report.txt` with the public key in `id_rsa.pub`.

```lua
gpg -s id_rsa report.txt
```
Sign `report.txt` with the private key in `id_rsa`.

```lua
gpg -v id_rsa.pub report.txt
```
Verify `report.txt.sig` with the public key in `id_rsa.pub`.

## Notes

- The program requires a data card, and specific workflows require the matching higher-tier methods on that card.
- Generated output files must not already exist; the program refuses to overwrite them.
- Password-mode decryption writes to the original basename when the source ends in `.gpg`, otherwise it appends `.dec`.
