# Move快速入门

欢迎来到Move语言的世界！Move是Rooch开发的核心编程语言，专为区块链设计，简单、安全又强大。本章将带你快速了解Move的基本概念，并通过一个简单示例让你动手试一试。不用担心复杂细节，我们会从最基础的部分开始！

## 什么是Move？

Move是一种为区块链智能合约设计的编程语言，由Facebook的Libra项目（现为Diem）开发。它有以下特点：
- **安全性**：通过资源管理防止资产重复花费。  
- **简单性**：语法类似Rust，容易上手。  
- **灵活性**：支持Rooch等区块链框架的开发。

在Rooch中，Move用来编写智能合约，帮助你创建去中心化应用（DApp）。本章将让你快速体验它的基本用法。

## Move的核心概念

### 1. 模块（Module）
- 模块是Move代码的基本组织单位，类似一个“文件”或“类”。  
- 每个模块可以定义数据和函数。

### 2. 资源（Resource）
- Move用“资源”表示数字资产，比如代币或NFT。  
- 资源不能被复制或丢弃，只能被转移，确保安全性。

### 3. 函数（Function）
- 函数定义了操作逻辑，例如创建资源或转移资产。  
- 有“公开函数”和“入口函数”，我们会用到后者。

## 第一个Move程序

让我们动手写一个简单的计数器程序！这个程序会在Rooch上存储一个数字，每次调用时增加1。

### 代码示例

> **注意**：以下代码中的中文注释仅为教学查看，实际Move代码不支持中文注释，请在运行时移除这些注释。

```move
module MyCounter::Counter {
    use std::signer;
    use moveos_std::account;

    // 定义一个资源，包含一个计数器值
    struct Counter has key {
        value: u64
    }

    // 初始化计数器（入口函数）
    public entry fun init_counter(account: &signer) {
        // 创建计数器，初始值为0
        let counter = Counter { value: 0 };
        // 将计数器存储到调用者的账户下
        account::move_resource_to(account, counter);
    }

    // 增加计数器（入口函数）
    public entry fun increment(account: &signer) {
        // 获取调用者的计数器
        let counter = account::borrow_mut_resource<Counter>(signer::address_of(account));
        // 值加1
        counter.value = counter.value + 1;
    }
}