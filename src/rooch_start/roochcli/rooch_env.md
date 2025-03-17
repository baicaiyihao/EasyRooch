# rooch env
- **查看已有环境 (`list`)**

下列是当前默认的一些环境配置，默认应用的是test。
```
rooch env list
╭───────────┬─────────────────────────────────┬───────────────┬────────────╮
│ Env Alias │ RPC URL                         │ Websocket URL │ Active Env │
├───────────┼─────────────────────────────────┼───────────────┼────────────┤
│ local     │ http://127.0.0.1:6767           │ Null          │            │
│ dev       │ https://dev-seed.rooch.network  │ Null          │ True       │
│ test      │ https://test-seed.rooch.network │ Null          │            │
│ main      │ https://main-seed.rooch.network │ Null          │            │
╰───────────┴─────────────────────────────────┴───────────────┴────────────╯
```
- **切换环境 (`switch`)**

```
rooch env switch
error: the following required arguments were not provided:
  --alias <ALIAS>

Usage: rooch env switch --alias <ALIAS>

For more information, try '--help'.

# 这里可以使用别名来进行切换
rooch env switch --alias dev
The active environment was successfully switched to `dev`
```

- **添加环境 (`add`)**

添加别名以及rpc的url即可。
```
rooch env add --alias test-rooch --rpc https://test-seed.rooch.network
Environment `test-rooch was successfully added
```
- **移除环境 (`remove`)**
```
rooch env remove
error: the following required arguments were not provided:
  --env <ENV>

Usage: rooch env remove --env <ENV>

For more information, try '--help'.

#这里也是根据别名即可移除

rooch env remove --env test-rooch
Environment `test-rooch` was successfully removed
```
