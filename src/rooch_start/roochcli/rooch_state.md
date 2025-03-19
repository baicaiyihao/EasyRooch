# rooch state

`rooch state` 是一个查询工具，用于通过“访问路径”（access-path）获取 Rooch 区块链上的状态数据（states）。这些状态数据可以是链上的对象（object）、资源（resource）、模块（module）或表（table）的内容。

```
rooch state --help
Get states by accessPath

Usage: rooch state [OPTIONS] --access-path <ACCESS_PATH>

Options:
  -a, --access-path <ACCESS_PATH>  /object/$object_id1[,$object_id2] /resource/$account_address/$resource_type1[,$resource_type2] /module/$account_address/$module_name1[,$module_name2] /table/$object_id/$key1[,$key2]
      --password <PASSWORD>        The key store password
      --config-dir <CONFIG_DIR>    rooch config path
      --show-display               Render and return display fields
  -h, --help                       Print help
```

以在rooch move中的hello合约为例：

```
objectId: 0x264967b1b22daa6096810908e5f00db1e0488f71fe0a063c9057c2f3cbfe870cb09a8140a9f24d9ea947c12605da3bc3de312307d29776b4f791a617e45e6d84
    type    : 0x2::object::DynamicField<0x1::string::String, 0x264967b1b22daa6096810908e5f00db1e0488f71fe0a063c9057c2f3cbfe870c::hello::HelloMessage>
```

调用合约的时候创建了一个`HelloMessage` ，下面使用`rooch state` 来进行查询，可以看到其中的内容`"text": "Hello Rooch!"`：

```
#使用rooch state --access-path /object/$object_id1[,$object_id2] 
rooch state --access-path /object/0x264967b1b22daa6096810908e5f00db1e0488f71fe0a063c9057c2f3cbfe870cb09a8140a9f24d9ea947c12605da3bc3de312307d29776b4f791a617e45e6d84
[
  {
    "id": "0x264967b1b22daa6096810908e5f00db1e0488f71fe0a063c9057c2f3cbfe870cb09a8140a9f24d9ea947c12605da3bc3de312307d29776b4f791a617e45e6d84",
    "owner": "rooch1qqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqhxqaen",
    "owner_bitcoin_address": null,
    "flag": 0,
    "state_root": "0x5350415253455f4d45524b4c455f504c414345484f4c4445525f484153480000",
    "size": "0",
    "created_at": "1742296062000",
    "updated_at": "1742296062000",
    "object_type": "0x2::object::DynamicField<0x1::string::String, 0x264967b1b22daa6096810908e5f00db1e0488f71fe0a063c9057c2f3cbfe870c::hello::HelloMessage>",
    "value": "0x573078323634393637623162323264616136303936383130393038653566303064623165303438386637316665306130363363393035376332663363626665383730633a3a68656c6c6f3a3a48656c6c6f4d6573736167650c48656c6c6f20526f6f636821",
    "decoded_value": {
      "abilities": 12,
      "type": "0x2::object::DynamicField<0x1::string::String, 0x264967b1b22daa6096810908e5f00db1e0488f71fe0a063c9057c2f3cbfe870c::hello::HelloMessage>",
      "value": {
        "name": "0x264967b1b22daa6096810908e5f00db1e0488f71fe0a063c9057c2f3cbfe870c::hello::HelloMessage",
        "value": {
          "abilities": 8,
          "type": "0x264967b1b22daa6096810908e5f00db1e0488f71fe0a063c9057c2f3cbfe870c::hello::HelloMessage",
          "value": {
            "text": "Hello Rooch!"
          }
        }
      }
    },
    "display_fields": null
  }
]
```

其他的方式也可以自己进行尝试。
