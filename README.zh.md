<p align="center">
  <strong>-------></strong>
  <a href="/README.md">俄语</a> |
  <a href="/README.en.md">英语</a> |
  <a href="/README.es.md">西班牙语</a> |
  <a href="/README.zh.md">中文</a> |
  <strong><-------</strong>
</p>

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="./media/logo-dark.png">
    <img width="512" height="auto" alt="项目标志" src="./media/logo-light.png">
  </picture>
</p>

---

<div align="center">

[![GitHub](https://img.shields.io/badge/GitHub-blue?style=flat&logo=github)](https://github.com/SoulofAO)
[![License](https://img.shields.io/badge/License-purple?style=flat&logo=github)](/LICENSE.md)
[![GitHub Stars](https://img.shields.io/github/stars/SoulofAO?style=flat&logo=github&label=Stars&color=orange)](https://github.com/SoulofAO)

</div>

<h1 align="center"> 
BP To CPP Converter — 用于无缝翻译蓝图到可读C++的插件
</h1>

<h3 align="center"> 
最终代码包括从蓝图节点到C++的函数完全转换。
</h3>

<h2 align="center"> 
    ⚠️ 免责声明 ⚠️
</h2> 
<p align="center">
  作者对使用本项目所带来的任何结果概不负责。使用本仓库的材料即表示您自动同意相关许可条款。
</p>

<details> 
  <summary align="center">⚠️完整内容⚠️</summary>

1. 使用本仓库的材料即表示您自动同意相关许可条款。

2. 作者对该材料的准确性、完整性或适用性不作任何明示或暗示的担保。

3. 作者对因使用或无法使用本材料及其随附文档而可能导致的任何损害，包括直接、间接、附带、后续或特殊损害，概不承担责任，即使事先已被告知可能会发生此类损害。

4. 使用本材料即表示您承认并接受与其应用相关的所有风险。此外，您同意作者不应为其使用所导致的任何问题或后果承担责任。

</details>

* * * * * * * * * * * * * * * * * * 
* * * * * * * * * * * * * * * * * * 

<h1 align="center"> 
介绍和警告
</h1>

> ⚠️ **重要警告**
>
> 插件目前处于积极开发中。在使用当前版本时，生成的代码可能会出现错误。这些错误中的许多正在开发过程中修复中，但有些错误源于虚幻引擎的基本限制，其中许多元素缺乏完整的反射支持。

<h1 align="center"> 
插件概览
</h1>

<details>
  <summary align="center">📖 详细描述</summary>

**BP To CPP Converter** 是一个专门为虚幻引擎设计的插件，可自动将蓝图逻辑转换为可读的C++代码。该插件解决了将可视化编程迁移到本地代码的挑战，特别适用于以下方面：

- **性能优化** – 将蓝图转换为C++以优化性能关键部分
- **项目重构** – 系统地构建代码库
- **学习C++** – 了解蓝图结构如何转换为本地代码

### 主要功能：
- **无缝转换** – 过程中保持功能完整性
- **支持核心结构** – 蓝图、接口、结构体、枚举
- **灵活定制** – 可适应特定项目需求
- **编辑器整合** – 管理与控制流程的用户友好性界面

</details>

* * * * * * * * * * * * * * * * * * 
* * * * * * * * * * * * * * * * * * 

## 📚 目录

### 🎯 基本信息
1. [介绍和警告](#介绍和警告)
2. [插件概览](#插件概览)
3. [EU_NativizationTool - 管理界面](#eu_nativizationtool---管理界面)

### 🏗️ 技术面
4. [内部架构 - 操作原理](#内部架构--操作原理)
5. [其他有用信息](#其他有用信息)

### 🧩 描述
6. [🧩 插件描述](#-插件描述)

### 🚀 快速入门
7. [快速入门](#快速入门)
8. [使用示例](#使用示例)

### ⚙️ 设置与配置
9. [运行本地化设置](#运行本地化设置)
10. [其他操作与设置](#其他操作与设置)
11. [本地化设置配置](#本地化设置配置)

### 📋 功能与限制
12. [功能与限制](#功能与限制)

### 📜 许可证与文档
13. [📜 许可证](#-许可证)

---

## 🔗 有用链接
- [虚幻引擎文档](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-5-7-documentation?application_version=5.7)
- [蓝图系统概述](https://docs.unrealengine.com/5.0/en-US/blueprint-system-overview-in-unreal-engine/)
- [C++编程指南](https://docs.unrealengine.com/5.0/en-US/cpp-programming-in-unreal-engine/)

* * * * * * * * * * * * * * * * * * 
* * * * * * * * * * * * * * * * * * 

<h1 align="center"> 
内部架构 - 操作原理
</h1>

<details>
  <summary align="center">⚙️ 展开描述</summary>

以下是插件工作原理的概述：
- 首先查找所有依赖资源，这些资源默认包含在生成列表中。
- 然后为每个资源执行代码生成，支持四种结构：常规蓝图（包括组件等）、接口、结构体和枚举。详细信息主要与处理常规蓝图有关。

解析的核心是被注册的 BaseTranslatorObject 子类，或更简单地说是译码器（Translators）。它们会周期性地修改以下概述的算法。

蓝图最初生成 EntryNodes，不使用现成的 Function，插件会将节点分为部分等价于原始 Function 的连续系列。更重要的是，最终分割确保最终划分的 EntryNodes 不具有环的结构。译码器会对节点是否构成循环，是否生成临时的 EntryNodes，或者是否彻底生成进行修改。

接下来，单独为 CPP 和 H 生成 Includes，通过解析变量、节点、父类、接口等生成。译码器管理节点并建议其 Includes。避免循环的 Includes，所有对象引用仅在 CPP 文件中声明，并之前在 Header 文件中预先声明。Header 和 CPP 中的 Includes 互不包含。

之后是 CS 代码生成。生成的 CS 文件与其他模块文件一样，仅用于修改现有文件，而不会完全替换。

译码器也会影响CS生成。

然后分别生成 Header 和 CPP。

Header 生成相对简单，过程涉及遍历类的基本元素，特别是其属性、函数、委托，并在类中声明它们。Constructor 和 SetupInputComponent 声明在此额外生成，但仅当激活了译码器时才会生成。

在 Header 之后生成 CPP代码。Header 和 CPP 的支持功能，例如构造函数和 SetupInputComponent，也在此实现。

构造函数通过遍历 actor 及其组件中的所有 FProperties 来生成，识别差异值，过滤掉主要无法被蓝图访问的变量（因此从其他变量派生），参考 Getter-Setter 数组，然后按顺序初始化更改后的变量值。对于在本地化设置中未注册构造函数的结构体，使用一种特殊的 ManyLineInitialization 初始Создать，被设计为基于递归的默认复制构造函数自动实现。

节点遍历，然后通过直接遍历形成执行顺序和逆向遍历所有非执行引脚生成程序。

译码器负责管理除了事件节点、函数入口和增强节点之外的所有内容。

</details>

* * * * * * * * * * * * * * * * * * 

<h1 align="center"> 
其他有用信息
</h1>

<details>
  <summary align="center">⚙️ 展开描述</summary>

值得注意的是，蓝图生成有两种方法，以及蓝图是如何初始化的。在蓝图的编译过程中，其代码被简化为字节码以优化性能。在此阶段，反射对象 FProperties 和 UFunctions 基于原始蓝图及其原始数据生成。

假设解析原始蓝图可以提供更大的灵活性，但更加复杂。相反，解析编译部分则确保更高的准确性和对错误的韧性。专注于纯本地化的高级插件可能更倾向于使用编译部分，而不是原始部分。

我的插件采用了组合方法。因为编译部分方便解析，但存在诸多问题。起初选择解析编译部分，后来转向解析原始数据，导致某些系统性矛盾被故意忽略。

插件支持宏/复合但不包含未在译码器系统中预注册的循环宏。这部分限制源于代码如何处理宏或复合节点。

未支持功能包括 Edit Inline Object、Instance Struct、所有非标准对象及子演员中的组件交换。

</details>

* * * * * * * * * * * * * * * * * * 

## 🧩 插件描述

<div align="center">
  <img style="width: 80%; height: auto;" alt="EU_NativizationTool" src="./media/Tutorial\Article_1/image6.png"/>
</div>

<details>
  <summary align="center">⚙️ 展开描述</summary>

BP To CPP Converter – 这是一个插件，可以无缝地将蓝图转换为可读的 C++ 代码。它允许用户通过单击一次按钮，将蓝图逻辑快速转换为 C++。这一过程被称为本地化技术，尽管从技术上讲是通过翻译进行的过渡。最终代码包括蓝图节点到 C++ 的完整函数转换。

项目的核心是 Editor Widget **EU_NativizationTool**，作为主要控制工具:

三个标签页：
1. **运行本地化** - 用于启动本地化的主要标签。
2. **从缓存结果中应用本地化** - 用于将本地化结果转移到功能性代码的主要标签页。
3. **其他操作** - 提供插件的辅助工具。

</details>

* * * * * * * * * * * * * * * * * * 

<h2 align="center">
  <a href="#-table-of-contents">⬆️ 返回顶部</a> 
</h2>

<h1 align="center"> 
快速入门
</h1>

<div align="center">
  <img style="width: 80%; height: auto;" alt="Initialize Module" src="./media/Tutorial\Article_1/image9.png"/>
</div>

<details>
  <summary align="center">⚙️ 展开描述</summary>

在使用插件之前，强烈建议初始化模块 **BlueprintNativizationModule**。此模块作为保存所有生成的 C++ 代码之用，负责将现有蓝图资源转移至生成的代码当中。生成的代码会被结构性地组织起来。

欲初始化，请进入 Other Action，点击 Initialize Blueprint Initialization Module 按钮并重新编译项目。如果正确完成，工具栏顶部的模块状态会更改。 **该操作仅需在工作开始前执行一次。**

</details>

* * * * * * * * * * * * * * * * * * 
* * * * * * * * * * * * * * * * * * 
