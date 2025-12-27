<p align="center">
  <strong>-------></strong>
  <a href="/README.md">Русский</a> |
  <a href="/docs/README.en.md">英语</a> |
  <a href="/docs/README.es.md">西班牙语</a> |
  <a href="/docs/README.zh.md">中文</a> |
  <strong><-------</strong>
</p>

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="./media/logo-dark.png">
    <img width="512" height="auto" alt="项目Logo" src="./media/logo-light.png">
  </picture>
</p>

---

<div align="center">

[![GitHub](https://img.shields.io/badge/GitHub-blue?style=flat&logo=github)](https://github.com/SoulofAO)
[![License](https://img.shields.io/badge/License-purple?style=flat&logo=github)](/LICENSE.md)
[![GitHub Stars](https://img.shields.io/github/stars/SoulofAO?style=flat&logo=github&label=Stars&color=orange)](https://github.com/SoulofAO)

</div>

<h1 align="center">
BP To CPP Converter —— 一款用于将Blueprint无缝转换为可读C++的插件
</h1>

<h3 align="center">
生成的最终代码包含从Blueprint节点转换为C++的完整函数。
</h3>

<h2 align="center">
    ⚠️ 免责声明 ⚠️
</h2>
<p align="center">
  作者对使用本项目可能导致的任何后果概不负责。
  使用本代码库的资源即表示您已同意相关许可协议的条款。
</p>

<details>
  <summary align="center">⚠️ 完整文本 ⚠️</summary>

1. 使用本代码库资料即表示您自动同意相关许可协议的条款。

2. 作者对本资料在准确性、完整性或适用于特定用途方面的任何明示或默示担保概不负责。

3. 作者不对因使用或无法使用本资料及其随附文档而引起的任何损害负责，包括但不限于直接的、间接的、附带的、后果性或特殊的损害，即使已被告知可能会发生此类损害。

4. 使用本资料即表明您已知并接受与其应用相关的所有风险。此外，您同意作者对使用本资料所导致的任何问题或后果不承担责任。

</details>

* * * * * * * * * * * * * * * * * * 
* * * * * * * * * * * * * * * * * * 

<h1 align="center">
简介与警告
</h1>

> ⚠️ **重要警告**
> 
> 该插件目前处于活跃开发阶段。使用当前版本可能会导致生成的代码出现错误。其中一些错误会在开发过程中解决，但有些问题是由于Unreal Engine固有的限制而导致的，其中许多元素未完全支持反射功能。

<h1 align="center">
插件概述
</h1>

<details>
  <summary align="center">📖 详细描述</summary>

**BP To CPP Converter** 是一款为Unreal Engine设计的专用插件，用于将Blueprint逻辑自动转换为可读的C++代码。此插件旨在解决将可视化编程迁移到原生代码的挑战，这对于以下场景特别有用：

- **性能优化** —— 将性能关键区域从Blueprint过渡到C++。
- **项目重构** —— 系统化地构建代码基础。
- **学习C++** —— 了解如何将Blueprint结构转化为原生代码。

### 主要功能：
- **无缝转换** —— 在转换过程中确保功能的完整性。
- **支持核心结构** —— Blueprint、Interface、Struct、Enum。
- **灵活的配置** —— 可根据特定项目需要进行适配。
- **编辑器集成** —— 用户友好的操作界面。

</details>

* * * * * * * * * * * * * * * * * * 
* * * * * * * * * * * * * * * * * * 

## 📚 目录

### 🎯 通用信息
1. [简介与警告](#简介与警告)
2. [插件概述](#插件概述)
3. [EU_NativizationTool - 管理接口](#eu_nativizationtool---管理接口)

### 🏗️ 技术方面
4. [内部架构 – 工作原理](#内部架构--工作原理)
5. [其他实用信息](#其他实用信息)

### 🧩 描述
6. [🧩 插件描述](#-插件描述)

### 🚀 快速入门
7. [准备工作](#准备工作)
8. [使用示例](#使用示例)

### ⚙️ 设置及配置
9. [运行本地化设置](#运行本地化设置)
10. [其他操作及设置](#其他操作及设置)
11. [本地化设置](#本地化设置)

### 📋 功能与限制
12. [功能与限制](#其他实用信息)

### 📜 许可证及文档
13. [📜 许可协议](#-许可协议)

---

## 🔗 实用链接
- [Unreal Engine 文档](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-5-7-documentation?application_version=5.7)
- [Blueprint 系统总览](https://docs.unrealengine.com/5.0/en-US/blueprint-system-overview-in-unreal-engine/)
- [C++ 编程指南](https://docs.unrealengine.com/5.0/en-US/cpp-programming-in-unreal-engine/)

* * * * * * * * * * * * * * * * * * 
* * * * * * * * * * * * * * * * * * 


<h1 align="center">
内部架构 – 工作原理
</h1>

<details>
  <summary align="center">⚙️ 展开描述</summary>

总体来说，插件的工作原理如下：
- 首先，它会识别所有依赖的Assets，并自动将其包含在代码生成列表中。
- 然后，为每个Asset生成代码。支持四种主要结构：常规Blueprint（包括组件等）、Interface、Struct和Enum。直接生成标准Blueprint值得详细讨论。

核心的解析通过插件设置中注册的BaseTranslatorObject派生类处理，称为Translators。以下依赖算法描述了它们如何周期性地进行修改或使用。

接下来的详细信息请... 

</details>
  
... 

