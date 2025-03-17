## Binary install

建议使用二进制安装方法，因为它简单快捷。通过使用预编译的二进制文件，您可以在系统上快速设置 Rooch，而无需从源代码构建，从而确保与 Ubuntu 版本的兼容性并最大限度地缩短设置时间。

### Ubuntu 24.04

#### Download
Download the latest Rooch binary for Ubuntu 24.04:

```shell
wget https://github.com/rooch-network/rooch/releases/latest/download/rooch-ubuntu-latest.zip
```

#### Decompress

```shell
unzip rooch-ubuntu-latest.zip
```

#### Install

```shell
sudo cd rooch-artifacts/ && cp rooch /usr/local/bin
```


### Ubuntu 22.04
#### Download
Download the latest Rooch binary for Ubuntu 22.04:

```shell
wget https://github.com/rooch-network/rooch/releases/latest/download/rooch-ubuntu-22.04.zip
```

#### Decompress

```shell
unzip rooch-ubuntu-22.04.zip
```

#### Install

```shell
sudo cd rooch-artifacts/ && cp rooch /usr/local/bin
```

## Compile and install

### Ubuntu

#### Install dependencies

```shell
sudo apt install git curl cmake make gcc lld pkg-config libssl-dev libclang-dev libsqlite3-dev g++ protobuf-compiler
```

#### Install Rust

```shell
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

#### Clone source code

```shell
git clone https://github.com/rooch-network/rooch.git
```

#### Compile and install Rooch

```shell
cd rooch && cargo build && cp target/debug/rooch ~/.cargo/bin/
```