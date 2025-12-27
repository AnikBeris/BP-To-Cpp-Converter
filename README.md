<p align="center">
  <strong>-------></strong>
  <a href="/README.md">Russian</a> |
  <a href="/README.en.md">English</a> |
  <a href="/README.es.md">Spanish</a> |
  <a href="/README.zh.md">Chinese</a> |
  <strong><-------</strong>
</p>

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="./media/logo-dark.png">
    <img width="512" height="auto" alt="Project Logo" src="./media/logo-light.png">
  </picture>
</p>

---

<div align="center">

[![GitHub](https://img.shields.io/badge/GitHub-blue?style=flat&logo=github)](https://github.com/SoulofAO)
[![License](https://img.shields.io/badge/License-purple?style=flat&logo=github)](/LICENSE.md)
[![GitHub Stars](https://img.shields.io/github/stars/SoulofAO?style=flat&logo=github&label=Stars&color=orange)](https://github.com/SoulofAO)

</div>

<h1 align="center"> 
BP To CPP Converter — A Plugin for Seamless Translation of Blueprints to Readable C++
</h1>

<h3 align="center"> 
The final code includes complete conversion of functions from Blueprint nodes to C++.
</h3>

<h2 align="center"> 
    ⚠️ Disclaimer ⚠️
</h2> 
<p align="center">
  The author is not responsible for any consequences of using this project. By using the repository materials, you automatically agree to the associated licensing terms.
</p>

<details> 
  <summary align="center">⚠️Full Text⚠️</summary>

1. By using the repository materials, you automatically agree to the associated licensing terms.

2. The author makes no guarantees, either expressed or implied, regarding the accuracy, completeness, or suitability of this material for any specific purpose. 
3. The author is not responsible for any damages, including direct, indirect, incidental, consequential, or special damages resulting from the use or inability to use this material or its accompanying documentation, even if previously notified of the possibility of such damages.

4. By using this material, you acknowledge and accept all risks associated with its application. Additionally, you agree that the author cannot be held accountable for any issues or consequences arising from its use.

</details>

* * * * * * * * * * * * * * * * * * 
* * * * * * * * * * * * * * * * * * 

<h1 align="center"> 
Introduction and Warning
</h1>

> ⚠️ **Important Warning**
> 
> The plugin is currently under active development. Errors may occur in the generated code when using the current version. Many of these errors are being fixed during development, but some stem from fundamental limitations of the Unreal Engine, where many elements lack full reflection support.

<h1 align="center"> 
Plugin Overview
</h1>

<details>
  <summary align="center">📖 Detailed Description</summary>

**BP To CPP Converter** is a specialized plugin for Unreal Engine designed to automatically convert Blueprint logic into readable C++ code. The plugin addresses the challenges of migrating visual programming into native code, particularly useful for:

- **Performance Optimization** – Transitioning from Blueprint to C++ for performance-critical sections
- **Project Refactoring** – Structuring the code base systematically
- **C++ Learning** – Understanding how Blueprint constructs translate into native code

### Key Features:
- **Seamless Conversion** – Functionality preserved during the conversion process
- **Support for Core Structures** – Blueprint, Interface, Struct, Enum
- **Flexible Customization** – Adaptable to specific project needs
- **Editor Integration** – User-friendly UI for management and control of the process

</details>


* * * * * * * * * * * * * * * * * * 
* * * * * * * * * * * * * * * * * * 

## 📚 Table of Contents

### 🎯 General Information
1. [Introduction and Warning](#introduction-and-warning)
2. [Plugin Overview](#plugin-overview)
3. [EU_NativizationTool - Management Interface](#eu_nativizationtool---management-interface)

### 🏗️ Technical Aspects
4. [Internal Architecture - Operational Principles](#internal-architecture--operational-principles)
5. [Additional Useful Information](#additional-useful-information)

### 🧩 Description
6. [🧩 Plugin Description](#-plugin-description)

### 🚀 Getting Started
7. [Getting Started](#getting-started)
8. [Example Usage](#example-usage)

### ⚙️ Settings and Configuration
9. [Run Nativization Settings](#run-nativization-settings)
10. [Other Actions and Settings](#other-actions-and-settings)
11. [Nativization Settings Configuration](#nativization-settings-configuration)

### 📋 Features and Limitations
12. [Features and Limitations](#additional-useful-information)

### 📜 License and Documentation
13. [📜 License](#-license)

---

## 🔗 Useful Links
- [Unreal Engine Documentation](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-5-7-documentation?application_version=5.7)
- [Blueprint System Overview](https://docs.unrealengine.com/5.0/en-US/blueprint-system-overview-in-unreal-engine/)
- [C++ Programming Guide](https://docs.unrealengine.com/5.0/en-US/cpp-programming-in-unreal-engine/)

* * * * * * * * * * * * * * * * * * 
* * * * * * * * * * * * * * * * * * 


<h1 align="center"> 
Internal Architecture - Operational Principles
</h1>

<details>
  <summary align="center">⚙️ Expand Description</summary>

Here's an overview of how the plugin works.
- First, it searches all dependent assets, which are included in the generation list by default.
- Then, code generation for each asset is performed. Four structures are supported: regular Blueprint (including components and more), Interface, Struct, and Enum. Detailed information primarily pertains to the processing of regular Blueprint.

The core of parsing is registered BaseTranslatorObject inheritors, or more simply, Translators. They periodically modify the algorithm outlined below.

Blueprints initially generate EntryNodes. Instead of using the ready-made Function, the plugin divides nodes into a series partially equivalent to the original Function. More importantly, the final division ensures that no sequence of Entry Nodes is cyclic. Translators modify whether the node should form a cycle, whether temporary Entry Nodes should be generated, or whether they should be generated at all.

Afterwards, Includes are generated separately for CPP and H. They are produced by parsing variables, nodes, parent classes, interfaces, and more. Translators manage nodes and suggest their includes. To avoid cyclic includes, all object references are declared exclusively in the CPP file and preliminarily introduced in the Header file. Includes in Header and CPP exclude each other.

Next comes CS generation. The resulting CS file, as with other module files, only applies changes to the existing file rather than completely replacing it.

Translators also influence the CS.

After that, both Header and CPP are generated separately.

Header generation is relatively straightforward. The process involves iterating through essential elements of the class, specifically its Properties, Functions, Delegates, and declaring them within the class. Constructors and SetupInputComponent declarations are additionally generated, but only if the translator is activated. Functions and variables will have U macros that are closest to Blueprint. Translators can be used to add new variables to the header.

The CPP code is generated after the Header. Supporting functions for Header and CPP, such as constructors and SetupInputComponent, are implemented.

The constructor is generated by iterating through all FProperties within the actor and its components. It identifies non-equivalences, filters primarily variables inaccessible to Blueprint (and consequently, derived from other variables), references the Getter-Setter array, then sequentially initializes differing variables with their modified values. For structures lacking registered constructors in Nativization Settings, a special ManyLineInitialization is used, built on initializing from a default recursively using a copy constructor.

Node traversal follows, including direct traversal forming execution sequence and reverse traversal for all non-exec pins. The generation starts with corresponding EntryNode, checks if no translator exists, proceeds to the next node, adding its result via recursion. Translators, especially those controlling the flow, function similarly, intercepting recursion at their Out Exec pins. Reverse traversal is structurally similar, though slightly more complex, adding adjustments for handling split pins.

Translators handle everything except Event nodes, Function Entry, and Enhance Nodes.

</details>

* * * * * * * * * * * * * * * * * * 

<h1 align="center"> 
Additional Useful Information
</h1>

<details>
  <summary align="center">⚙️ Expand Description</summary>

It is important to note that there are two approaches to generation in Blueprint and how exactly Blueprint initializes itself. During the compilation of Blueprint, its code is simplified into bytecode for optimization. This stage introduces FProperties and UFunctions as reflection objects based on the original Blueprint and its raw data.

Hypothetically, parsing the original Blueprint offers greater flexibility but is more complex. Conversely, parsing the compiled part ensures greater accuracy and resilience to errors. Advanced plugins focused on pure nativization likely use the compiled portion instead of the original.

My plugin employs a combinatorial approach. This is because the compiled part is convenient for parsing but comes with many issues. Initially inclined to parse the compiled portion, I transitioned to parsing raw data later on, resulting in certain systemic contradictions which were intentionally disregarded.

The plugin supports Macros / Composites but excludes cyclic macros not pre-registered in the translator system. This limitation partly stems from how code processes macros or composite nodes. Macros are unpacked during code parsing through stored Generated Bode. Hypothetically, cyclic macros can be parsed but inevitably pollute the code with numerous generated functions already present in the final code. Additionally, most cyclic macros typically have quite simple constructs in C++ code. Therefore, creating a custom translator for specific needs seems more effective than attempting an addition.

Unsupported features include Edit Inline Object, Instance Struct, all non-standard objects, and component swapping in Child Actors.

</details>

* * * * * * * * * * * * * * * * * * 



## 🧩 Plugin Description

<div align="center">
  <img style="width: 80%; height: auto;" alt="EU_NativizationTool" src="./media/Tutorial\Article_1/image6.png"/>
</div>

<details>
  <summary align="center">⚙️ Expand Description</summary>

BP To CPP Converter – a plugin provides a seamless translation of Blueprints into readable C++ code. It allows quick conversion of Blueprint schemas into C++ with a single click. The processing is referred to as nativization, though technically it's transition via translation. The final code encompasses full function conversion from Blueprint nodes into C++.

The project core is the Editor Widget **EU_NativizationTool** which serves as the main control tool:

Three tabs:
1. **Run Nativization** – Main tab for starting nativization.
2. **Apply From Cache Nativization Result** – Main tab for transferring nativized Actor results into functional code.
3. **Other Actions** – Auxiliary utilities for the plugin.

</details>

* * * * * * * * * * * * * * * * * * 

<h2 align="center">
  <a href="#-table-of-contents">⬆️ Back to Top</a> 
</h2>

<h1 align="center"> 
Getting Started
</h1>

<div align="center">
  <img style="width: 80%; height: auto;" alt="Initialize Module" src="./media/Tutorial\Article_1/image9.png"/>
</div>

<details>
  <summary align="center">⚙️ Expand Description</summary>

Before using the plugin, it is strongly recommended to initialize the module **BlueprintNativizationModule**. This module serves as a space for saving all resulting C++ codes, used for transferring existing Blueprint assets onto them. The generated code will be structurally organized. To initialize, go to Other Action, click Initialize Blueprint Initialization Module, and recompile the project. If done correctly, the module status at the top of your toolbar will change. This action should ONLY be performed ONCE BEFORE STARTING WORK.

</details>

* * * * * * * * * * * * * * * * * * 

<h2 align="center">
  <a href="#-table-of-contents">⬆️ Back to Top</a> 
</h2>

<h1 align="center"> 
Example Usage
</h1>

<div align="center">
  <img style="width: 80%; height: auto;" alt="Run Nativization UI" src="./media/Tutorial\Article_1/image5.png"/>
</div>

<details>
  <summary align="center">⚙️ Expand Description</summary>

1. Navigate to the **Run Nativization** tab.
2. Specify any asset in the Blueprints field.
   - Ensure the Blueprint is compiled and saved.
   - You can specify one or multiple assets.
   - All entities specified in Blueprints, as well as those dependent on Blueprints, will be subjected to nativization.
3. Click the **Apply** button.
   - At the bottom of the editor, you'll view the resulting code.

To test the plugin functionality, use your own asset, **TestNativizationActor**, or any item from the Tests folder.

</details>


* * * * * * * * * * * * * * * * * * 

<h2 align="center">
  <a href="#-table-of-contents">⬆️ Back to Top</a> 
</h2>

<h1 align="center"> 
Run Nativization Settings
</h1>

<div align="center">
  <img style="width: 80%; height: auto;" alt="Run Nativization Settings" src="./media/Tutorial\Article_1/image5.png"/>
</div>

<details>
  <summary align="center">⚙️ Expand Description</summary>

---

### Generate Code One Function

Allows generation of code for a selected function only. Check True and specify the function name in the Function Name field. Functions can be generated in series if multiple are selected, e.g., situations where Input Action functions are chosen, or functions subdivided during nativization.

---

### Transform Only One File Code

Generates code only for directly specified Blueprint, ignoring all dependency recursion.

---

### SaveToFile

Enables saving all results to file. This function and subsequent ones won’t work correctly without initializing the BlueprintNativizationModule.

It's generally advised to keep asset references managed by Blueprint.

**Left All Asset Ref In Blueprint** – the flag enabling this functionality. Otherwise, all references will be explicitly written into C++.

---

### Visualization

Flags determining what is displayed below in the editor. You can disable Header or CPP code.

---

### SaveOutputFolder

Determines where to save the resulting C++ code. If empty, the output will be saved in the initialized BlueprintNativizationModule, correctly distributed across folders.

---

### Hot Reload and Replace

An experimental feature for automatically replacing a Blueprint with its generated C++ class without restarting the project. Contains issues and hence mostly Save Cache is utilized.

---

### Save Cache

Saves knowledge about generated objects, allowing fixes and project recompilation while maintaining the ability to replace initial Blueprints with generated C++ between Unreal Engine sessions.

---

### Cache Path

Similar to Save Output Folder, saves cache data into a folder. Otherwise, saves it into the root of the BlueprintNativizationModule.

To use cache, navigate to the **Apply From Cache Nativization Result** tab. Note that cache application requires a restart of Unreal Engine; otherwise, CDO classes won’t initialize.

Leaving Cache Path empty will default the cache to the project’s root.

---

</details>

* * * * * * * * * * * * * * * * * * 

<h2 align="center">
  <a href="#-table-of-contents">⬆️ Back to Top</a> 
</h2>

<h1 align="center"> 
Other Actions and Settings
</h1>

<div align="center">
  <img style="width: 80%; height: auto;" alt="Editor Settings" src="./media/Tutorial\Article_1/image3.png"/>
</div>

<details>
  <summary>⚙️ Expand Description</summary>

Other Actions includes several additional functions; apart from **Initialize Blueprint Initialization Module**, there’s **Reset Names**. The project aims to thoroughly avoid naming conflicts which may occur if nativization is performed in series without transferring Apply results to generated assets. Naming conflicts are resolved via an allocated Unique Name system, and clearing it (although not essential) is often useful, clearing excess caches in the temporary variable system. It’s advisable to conduct nativization step-by-step, transferring generated code objects to BP assets or in one big series. This is partly because the cache exists only within one Unreal Engine session, no longer.

**PrintAllK2Nodes** – ignore this.

Widget settings undergo regular updates based on state preferences. For more permanent configurations, refer to Blueprint Nativization V2 Editor Settings under Editor Settings.

</details>

* * * * * * * * * * * * * * * * * * 

<h2 align="center">
  <a href="#-table-of-contents">⬆️ Back to Top</a> 
</h2>

<h1 align="center"> 
Nativization Settings Configuration
</h1>

<div align="center">
  <img style="width: 60%; height: auto;" alt="Additional UI" src="./media/Tutorial\Article_1/image4.png"/>
</div>

<details>
  <summary>⚙️ Expand Description</summary>

The primary source for translating Blueprint functions into C++ are translators. **Translation** supports one or more types of K2Node and translates them into C++ code.

---

### Global Variable Name

Prevents naming conflicts.

---

### Setup Action Object

Links **EU_NativizationTool** to various auxiliary Widgets on the Blueprint side; it's recommended to avoid modifying this setting.

---

### Enable Generate Value Suffix

Determines whether all generated variables in C++ code will have a GeneratedValue suffix. It's better to disable for cleaner code.

---

### Add BP Prefix To Parent Blueprint

Determines if converted Blueprint objects inheriting C++ classes retain the "BP_" prefix.

---

### Function Redirects

Maps Blueprint Implemented names to the corresponding original C++ function. This resolves metadata insufficiencies related to function calls and overrides.

---

### Construction Descriptors

Constructor definitions for structures where initialization via a constructor is the most appropriate method. Otherwise, direct value setting via `.' is used, e.g., `FLinearColor(0.0,0.66,1.0,1.0)` switches to `FLinearColor LinearColor(); LinearColor.R = 1.0; LinearColor.G = 1.0;` and so forth. Designed due to lacking reflection support. Full constructors are implemented by default for Blueprints-generated structures.

---

### Ignore Class to Ref Generate

Includes classes excluded from nativization. Generally, UI, Widgets, and similar elements. Avoid attempting to generate C++ for such categories, or it may lead to crashes.

---

### Ignore Assets to Ref Generate

Same as above.

---

### Getter And Setter Description

Contains functions for accessing variables to prevent crashes caused by attempting access to private properties. Defines operations like Get Set or complete variable omission.

---

### Code Editor

Information for the visual part of the Text Editor in the main widget, containing directives for substring color highlighting.

---

</details>


* * * * * * * * * * * * * * * * * * 
* * * * * * * * * * * * * * * * * * 


<h1 align="center"> 📜 License</h1>
<h2 align="center">
  <strong>---></strong>
  <strong> This project is distributed under </strong> 
  <a href="./LICENSE">SoulofAO License</a>
  <strong><---</strong>
</h1>

---

<h2 align="center"> 
📚 Documentation
</h2>

<p align="center">
  <strong>---></strong>
  <a href="/README.md">Russian</a> |
  <a href="/README.en.md">English</a> |
  <a href="/README.es.md">Spanish</a> |
  <a href="/README.zh.md">Chinese</a> |
  <strong><---</strong>
</p>
