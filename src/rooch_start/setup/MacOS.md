## Binary install

建议使用二进制安装方法，因为它简单快捷。通过使用预编译的二进制文件，您可以在系统上快速设置 Rooch，而无需从源代码构建，从而确保与 MacOS 版本的兼容性并最大限度地缩短设置时间。

### MacOS Arm

#### Download

```shell
wget https://github.com/rooch-network/rooch/releases/latest/download/rooch-macos-latest.zip
```

#### Decompress

```shell
unzip rooch-macos-latest.zip
```

#### Install

```shell
sudo cp rooch /usr/local/bin
```

## 

## Compile and install

### macOS

#### Homebrew

Use the following command to install [Homebrew](https://brew.sh/):

```sh
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

#### Install dependencies

```shell
brew install curl cmake libpq git
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