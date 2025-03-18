# rooch move

`rooch move` 是与 Move 语言开发相关的工具集，提供编译、测试和发布等功能。以下是其帮助信息和常用子命令简介：

- **新建项目 (`new`)**
```
# 使用rooch move new <项目名称>即可创建
$rooch move new hello
Success

#命令执行后在当前目录下会创建和项目名称相同的目录
$ls
test

#test目录结构如下
.
├── Move.toml
└── sources
    └── hello.move
```

- **编译项目 (`build`)**

编辑在new命令中创建的文件
`hello.move`
```
module hello::hello {
    use moveos_std::account;
    use std::string;

    struct HelloMessage has key {
        text: string::String
    }

    public entry fun say_hello(owner: &signer) {
        let hello = HelloMessage { text: string::utf8(b"Hello Rooch!") };
        account::move_resource_to(owner, hello);
    }
}
```
然后可以在hello目录下执行编译的命令，成功后结果如下：
```
$rooch move build
UPDATING GIT DEPENDENCY https://github.com/rooch-network/rooch.git
UPDATING GIT DEPENDENCY https://github.com/rooch-network/rooch.git
UPDATING GIT DEPENDENCY https://github.com/rooch-network/rooch.git
INCLUDING DEPENDENCY MoveStdlib
INCLUDING DEPENDENCY MoveosStdlib
INCLUDING DEPENDENCY RoochFramework
BUILDING hello
Exported package to ./build/hello/package.rpd
Success
```

- **测试项目 (`test`)**

该命令用来测试合约中的功能是否可以正常使用。

编辑`hello.move`文件:

```
module hello::hello {
    use moveos_std::account;
    use std::string;

    struct HelloMessage has key {
        text: string::String
    }

    public entry fun say_hello(owner: &signer) {
        let hello = HelloMessage { text: string::utf8(b"Hello Rooch!") };
        account::move_resource_to(owner, hello);
    }

    #[test(sender=@0x42)]
    fun test_stake(sender: &signer){
        say_hello(sender);
    }
}
```
在执行的时候可能会遇到这种情况：
```
rooch move test
Error: Unable to resolve packages for package 'hello'
While resolving dependency 'MoveosStdlib' in package 'hello'
Failed to fetch to latest Git state for package 'MoveosStdlib', to skip set --skip-fetch-latest-git-deps | Exit status: exit status: 128
```
这时候需要加上`--skip-fetch-latest-git-deps`，成功执行后如下：
```
rooch move test
INCLUDING DEPENDENCY MoveStdlib
INCLUDING DEPENDENCY MoveosStdlib
INCLUDING DEPENDENCY RoochFramework
BUILDING hello
Running Move unit tests
[ PASS    ] 0x264967b1b22daa6096810908e5f00db1e0488f71fe0a063c9057c2f3cbfe870c::hello::test_stake
Test result: OK. Total tests: 1; passed: 1; failed: 0
```

- **部署项目 (`publish`)**

通过build和test之后初步确定合约没有错误后可以进行部署。

如果是首次部署，钱包内没有rgas 会出现下列的报错内容：

```
rooch move publish
UPDATING GIT DEPENDENCY https://github.com/rooch-network/rooch.git
UPDATING GIT DEPENDENCY https://github.com/rooch-network/rooch.git
UPDATING GIT DEPENDENCY https://github.com/rooch-network/rooch.git
INCLUDING DEPENDENCY MoveStdlib
INCLUDING DEPENDENCY MoveosStdlib
INCLUDING DEPENDENCY RoochFramework
BUILDING hello
Publish modules to address: 264967b1b22daa6096810908e5f00db1e0488f71fe0a063c9057c2f3cbfe870c
Transaction error: ErrorObject { code: ServerError(2), message: "status ABORTED of type Execution with sub status 1004", data: None }

```

需要领取rgas之后在进行部署，领取rgas 参考[领取Rgas](../../rooch_start/faucet.md)。

成功部署后如下：

```
rooch move publish
UPDATING GIT DEPENDENCY https://github.com/rooch-network/rooch.git
UPDATING GIT DEPENDENCY https://github.com/rooch-network/rooch.git
UPDATING GIT DEPENDENCY https://github.com/rooch-network/rooch.git
INCLUDING DEPENDENCY MoveStdlib
INCLUDING DEPENDENCY MoveosStdlib
INCLUDING DEPENDENCY RoochFramework
BUILDING hello
Publish modules to address: 264967b1b22daa6096810908e5f00db1e0488f71fe0a063c9057c2f3cbfe870c
Execution info:
    status: Executed
    gas used: 1318976
    tx hash: 0x29c9a8eaae6f0430d10930522aab28f7f7e479389d22990d12d9a0280e2d9e82
    state root: 0xd9a69167baf27f6a563d4b7fd9073d88496da8632461fe62c08418ee58a87bc2
    event root: 0xee6300870946ec81db8654af859ff685671ac173af63e4a69885769de5db05f4

New modules:
    0x264967b1b22daa6096810908e5f00db1e0488f71fe0a063c9057c2f3cbfe870c::hello

Updated modules:
    None

New objects:
    objectId: 0x14481947570f6c2f50d190f9a13bf549ab2f0c9debc41296cd4d506002379659264967b1b22daa6096810908e5f00db1e0488f71fe0a063c9057c2f3cbfe870c
    type    : 0x2::module_store::Package

    objectId: 0x14481947570f6c2f50d190f9a13bf549ab2f0c9debc41296cd4d506002379659264967b1b22daa6096810908e5f00db1e0488f71fe0a063c9057c2f3cbfe870c2db8971282e9227073eceff88867c864237fa06e529892c5addf335b5e84f13a
    type    : 0x2::object::DynamicField<0x1::string::String, 0x2::move_module::MoveModule>

    objectId: 0x264967b1b22daa6096810908e5f00db1e0488f71fe0a063c9057c2f3cbfe870c
    type    : 0x2::account::Account

    objectId: 0x2ec24e80179f353555d1012f896a708dcf1f441ffb4131413af0cb245510e7216bcf17da2ce4bf1980589f3e8faad8862c8f4723372230cd5112949f869ca885
    type    : 0x2::object::DynamicField<address, 0x3::bitcoin_address::BitcoinAddress>

    objectId: 0x82327808cb5077c8a2f9520286cc90dd748753135ff2b3119df0b5a4bfe0a2b8
    type    : 0x2::module_store::UpgradeCap
            
Modified objects:
    objectId: 0x14481947570f6c2f50d190f9a13bf549ab2f0c9debc41296cd4d506002379659
    type    : 0x2::module_store::ModuleStore

    objectId: 0x29344e813f7dec57c632a72c07c5c0ec653a094d524bfc221f6ab3a32d5de9c5
    type    : 0x3::coin_store::CoinStore<0x3::gas_coin::RGas>

    objectId: 0x2ec24e80179f353555d1012f896a708dcf1f441ffb4131413af0cb245510e721
    type    : 0x3::address_mapping::RoochToBitcoinAddressMapping

    objectId: 0x4e5ceb9b30a672308137d1d636497c805340a7d29a927adb150dd1354f924bf0
    type    : 0x3::coin_store::CoinStore<0x3::gas_coin::RGas>

    objectId: 0x6b271d90caa1dcce1222822a5ded0bf4f3eef88b721dc2303a59f073463255a8
    type    : 0x3::coin_store::CoinStore<0x3::gas_coin::RGas>

    objectId: 0xb6fc47ae8cbd3d22dcb2f6b1c17a0e5ce926ab0efb799fab28e31c5acbe752d1
    type    : 0x3::coin_store::CoinStore<0x3::gas_coin::RGas>

Removed objects:
    None
```

如果遇到有如下的提示：

```
Failed to fetch to latest Git state for package 'MoveosStdlib', to skip set --skip-fetch-latest-git-deps | Exit status: exit status: 128
```

加上`--skip-fetch-latest-git-deps` 再执行即可。

- **调用合约 (`run`)**

当成功部署合约之后就可以通过run来调用已部署的合约，在publish中已经部署了合约，这里直接调用。
```
rooch move run --function 0x264967b1b22daa6096810908e5f00db1e0488f71fe0a063c9057c2f3cbfe870c::hello::say_hello
Execution info:
    status: Executed
    gas used: 327056
    tx hash: 0xa833f1dbd225b822948598fe8547c0a21d8cf3c115ca6c71cb8c328668d8ce50
    state root: 0x5f29c924812b508a4db0c0dfe78712421bde717edef41e8c6ad42290681dee88
    event root: 0x345d02121ff1b018fc7979492280f0d06a1bfd4d6b033553d87d1830fe8a271a

New objects:
    objectId: 0x264967b1b22daa6096810908e5f00db1e0488f71fe0a063c9057c2f3cbfe870cb09a8140a9f24d9ea947c12605da3bc3de312307d29776b4f791a617e45e6d84
    type    : 0x2::object::DynamicField<0x1::string::String, 0x264967b1b22daa6096810908e5f00db1e0488f71fe0a063c9057c2f3cbfe870c::hello::HelloMessage>

    objectId: 0x5900af94221897cbb995f35c2aeb1ee24a56174a321e6956036b33accc5e8deb
    type    : 0x3::coin_store::CoinStore<0x3::gas_coin::RGas>

    objectId: 0xd66d0c2e9b47f75f2a6dca80584208333c3327cf89326b905d3850b9970f13f36bcf17da2ce4bf1980589f3e8faad8862c8f4723372230cd5112949f869ca885
    type    : 0x2::object::DynamicField<address, 0x2::object::Object<0x3::coin_store::CoinStore<0x3::gas_coin::RGas>>>
            
Modified objects:
    objectId: 0x264967b1b22daa6096810908e5f00db1e0488f71fe0a063c9057c2f3cbfe870c
    type    : 0x2::account::Account

    objectId: 0x29344e813f7dec57c632a72c07c5c0ec653a094d524bfc221f6ab3a32d5de9c5
    type    : 0x3::coin_store::CoinStore<0x3::gas_coin::RGas>

    objectId: 0x4e5ceb9b30a672308137d1d636497c805340a7d29a927adb150dd1354f924bf0
    type    : 0x3::coin_store::CoinStore<0x3::gas_coin::RGas>

    objectId: 0x6b271d90caa1dcce1222822a5ded0bf4f3eef88b721dc2303a59f073463255a8
    type    : 0x3::coin_store::CoinStore<0x3::gas_coin::RGas>

    objectId: 0xb6fc47ae8cbd3d22dcb2f6b1c17a0e5ce926ab0efb799fab28e31c5acbe752d1
    type    : 0x3::coin_store::CoinStore<0x3::gas_coin::RGas>

    objectId: 0xd66d0c2e9b47f75f2a6dca80584208333c3327cf89326b905d3850b9970f13f3
    type    : 0x3::transaction_fee::TransactionFeePool

Removed objects:
    None

Events:
    event handle id: 0x18e4e7d4ead4c9a9be261bbe4c140e06c12a6e9bd7cad46a9f44caa4acc4174c
    event seq      : 5552143
    event type     : 0x3::coin_store::WithdrawEvent

    event handle id: 0xb853393e53406be49dfd6325e65de25eae00c348cbe0b2cec8ea56513fa2c26a
    event seq      : 11128202
    event type     : 0x3::coin_store::DepositEvent

    event handle id: 0x18e4e7d4ead4c9a9be261bbe4c140e06c12a6e9bd7cad46a9f44caa4acc4174c
    event seq      : 5552144
    event type     : 0x3::coin_store::WithdrawEvent

    event handle id: 0xb853393e53406be49dfd6325e65de25eae00c348cbe0b2cec8ea56513fa2c26a
    event seq      : 11128203
    event type     : 0x3::coin_store::DepositEvent

    event handle id: 0x3797fc63218944e37225cf62545e1bfa77cbd404214017e4ce5effa6e2e62e53
    event seq      : 1195
    event type     : 0x3::coin_store::CreateEvent

    event handle id: 0xb853393e53406be49dfd6325e65de25eae00c348cbe0b2cec8ea56513fa2c26a
    event seq      : 11128204
    event type     : 0x3::coin_store::DepositEvent

    event handle id: 0xb853393e53406be49dfd6325e65de25eae00c348cbe0b2cec8ea56513fa2c26a
    event seq      : 11128205
    event type     : 0x3::coin_store::DepositEvent

    event handle id: 0xb853393e53406be49dfd6325e65de25eae00c348cbe0b2cec8ea56513fa2c26a
    event seq      : 11128206
    event type     : 0x3::coin_store::DepositEvent
```

这里面还有一些传入参数方法,可以通过help进行查看。
```
rooch move run --help
Run a Move function

Usage: rooch move run [OPTIONS] --function <FUNCTION>

Options:
      --function <FUNCTION>
          Function name as `<ADDRESS>::<MODULE_ID>::<FUNCTION_NAME>` Example: `0x42::message::set_message`, `rooch_framework::empty::empty`

      --type-args <TYPE_ARGS>
          TypeTag arguments separated by spaces.
          
          Example: `0x1::M::T1 0x1::M::T2 rooch_framework::empty::Empty`

      --args <ARGS>
          If there are multiple parameters, multiple `--args` modifications are required.
          
          Example: rooch move run --function 0x3::MODULE::FUNCTION --args u64:15 --args @0x42
          
          Supported types [u8, u16, u32, u64, u128, u256, bool, object_id, string, address, vector<inner_type>]
          
          Example: `address:0x1 bool:true u8:0 u256:1234 'vector<u32>:a,b,c,d'` address and uint can be written in short form like `@0x1 1u8 4123u256`.

      --password <PASSWORD>
          The key store password

      --config-dir <CONFIG_DIR>
          rooch config path

      --sender <SENDER>
          Sender account address
          
          [default: default]

      --sequence-number <SEQUENCE_NUMBER>
          Custom account's sequence number

      --max-gas-amount <MAX_GAS_AMOUNT>
          Custom the transaction's gas limit. [default: 1_000_000_000] [alias: "gas-limit"]

      --authenticator <AUTHENTICATOR>
          Custom the transaction's authenticator format: `auth_validator_id:payload`, auth validator id is u64, payload is hex string example: 123:0x2abc

      --session-key <SESSION_KEY>
          Sign the transaction via session key This option conflicts with `authenticator`

      --json
          Return command outputs in json format

      --gas-profile
          Run the gas profiler and output html report

      --dry-run
          Run the DryRun for this transaction

  -h, --help
          Print help (see a summary with '-h')
```
          
