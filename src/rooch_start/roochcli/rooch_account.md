# rooch account
- **创建新账户 (`create`)**

会基于init时创建钱包的助记词继续创建新的账户。

```
rooch account create

Generated new keypair for address with key pair type [rooch1yeyk0vdj9k4xp95ppyywtuqdk8sy3rm3lc9qv0ys2lp08jl7suxqh8aryh]
Secret Recovery Phrase : [mountain weapon whip denial tray damp weekend coast picnic toddler endorse call]
```

- **查看账户 (`list`)**

查看Cli 中的所有账户，Active 为True时代表你当前使用的是哪个账户。

```
rooch account list
╭──────────────────┬───────────────────────────────────────────────────────────────────────────┬────────────╮
│ Field            │ Value                                                                     │ Active     │
├──────────────────┼───────────────────────────────────────────────────────────────────────────┼────────────┤
│ Address          │ rooch1yeyk0vdj9k4xp95ppyywtuqdk8sy3rm3lc9qv0ys2lp08jl7suxqh8aryh          │ False      │
│ Hex Address      │ 0x264967b1b22daa6096810908e5f00db1e0488f71fe0a063c9057c2f3cbfe870c        │            │
│ Bitcoin Address  │ bcrt1puq36h99xwjxgq005p8kx97uvnmu3x7hzarm7n3hxsxxh0f8jfn3sqdjvyl          │            │
│ Public Key       │ 0x028912a48902bb1ff0f66366c40d0e1d2904603964d561399363334f4f03239d34      │            │
│ Nostr Public Key │ npub13yf2fzgzhv0lpanrvmzq6rsa9yzxqwty64snnymrxd857qern56qmef756           │            │
│ ──────────────── │ ───────────────────────────────────────────────────────────────────────── │ ────────── │
│ Address          │ rooch1s5y679r9j8m02szf0fr6ffcrhdasvhhzka4v59fdqc6ndp7xj3wsu7pvny          │ True       │
│ Hex Address      │ 0x8509af146591f6f540497a47a4a703bb7b065ee2b76aca152d06353687c6945d        │            │
│ Bitcoin Address  │ bcrt1pg6er56jr394flxy73l7lr229ch5xme4f63zd0yqt77zng9kt7tsqm2x34d          │            │
│ Public Key       │ 0x03e35e180b0242b341f988f9b7f7bc8df6db6ba91f11c6f387bfc9f6e579573624      │            │
│ Nostr Public Key │ npub1ud0pszczg2e5r7vglxml00yd7mdkh2glz8r08pale8mw272hxcjqgzy0gc           │            │
│ ──────────────── │ ───────────────────────────────────────────────────────────────────────── │ ────────── │
╰──────────────────┴───────────────────────────────────────────────────────────────────────────┴────────────╯
```
- **查看账户余额 (`balance`)**

可以查看当前账户的余额。
```
rooch account balance
╭───────────┬────────┬──────────┬─────────╮
│ Coin Type │ Symbol │ Decimals │ Balance │
├───────────┼────────┼──────────┼─────────┤
│ Bitcoin   │ BTC    │ 8        │ 0       │
╰───────────┴────────┴──────────┴─────────╯
```

- **切换账户 (`switch`)**

用来切换当前使用的账户。

```
rooch account switch
error: the following required arguments were not provided:
  --address <ADDRESS>

Usage: rooch account switch --address <ADDRESS>

For more information, try '--help'.

#可以根据list中查询出来的address来进行切换

rooch account switch --address rooch1yeyk0vdj9k4xp95ppyywtuqdk8sy3rm3lc9qv0ys2lp08jl7suxqh8aryh
The active account was successfully switched to `rooch1yeyk0vdj9k4xp95ppyywtuqdk8sy3rm3lc9qv0ys2lp08jl7suxqh8aryh`
```

- **导出账户 (`export`)**

会导出当前使用账户的助记词。
```
rooch account export
Export succeeded with the mnemonic phrase [mountain weapon whip denial tray damp weekend coast picnic toddler endorse call]
```

- **导入账户 (`import`)**

这里需要传入钱包账户的hex格式的私钥进行导入。
```
rooch account import --secretkey 1d6701355158a08a2ee734f47b0951138a813ecf9f6c3341ddfa1cec30e97018
Import succeeded with address [rooch1s5y679r9j8m02szf0fr6ffcrhdasvhhzka4v59fdqc6ndp7xj3wsu7pvny] and the secret key
```