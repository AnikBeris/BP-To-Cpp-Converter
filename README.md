<p align="center">
  <strong>-------></strong>
  <a href="/README.md">Русский</a> |
  <a href="/docs/README.en.md">English</a> |
  <a href="/docs/README.es.md">Spanish</a> |
  <a href="/docs/README.zh.md">Chinese</a> |
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
BP To CPP Converter — A Plugin for Seamless Conversion of Blueprint to Readable C++
</h1>

<h3 align="center"> 
The final code includes the complete conversion of functions from Blueprint nodes to C++.
</h3>

<h2 align="center"> 
    ⚠️ Disclaimer ⚠️
</h2> 
<p align="center">
  The author assumes no responsibility for any potential consequences of using this project.
  By using the materials of this repository, you automatically agree to the terms of the associated license agreement.
</p>

<details> 
  <summary align="center">⚠️Full Text⚠️</summary>

1. By using the materials of this repository, you automatically agree to the terms of the associated license agreement.

2. The author provides no warranties, express or implied, as to the accuracy, completeness, or fitness of this material for any specific purposes.

3. The author is not liable for any damages, including but not limited to direct, indirect, incidental, consequential, or special damages, arising from the use or inability to use this material or its accompanying documentation, even if advised of the possibility of such damages.

4. By using this material, you acknowledge and accept all risks associated with its application. Furthermore, you agree that the author cannot be held responsible for any issues or consequences arising from its use.

</details>

* * * * * * * * * * * * * * * * * * 
* * * * * * * * * * * * * * * * * * 

<h1 align="center"> 
Introduction & Warning
</h1>

> ⚠️ **IMPORTANT WARNING**
> 
> This plugin is currently in active development. Using the current version may result in errors in the generated code. Many of these errors are resolved during development, but some stem from fundamental limitations of the Unreal Engine where many elements do not fully support reflection.

<h1 align="center"> 
Plugin Overview
</h1>

<details>
  <summary align="center">📖 Detailed Description</summary>

**BP To CPP Converter** is a specialized plugin for Unreal Engine, designed for the automatic conversion of Blueprint logic into readable C++ code. This plugin addresses the challenge of migrating visual programming into native code, which is especially useful for:

- **Performance Optimization** – Transitioning from Blueprint to C++ for performance-critical areas.
- **Project Refactoring** – Structuring code bases systematically.
- **Learning C++** – Understanding how Blueprint constructions translate into native code.

### Key Features:
- **Seamless Conversion** – Ensures functionality is preserved during conversion.
- **Support for Core Structures** – Blueprint, Interface, Struct, Enum.
- **Flexible Configuration** – Adapts to specific project needs.
- **Editor Integration** – User-friendly UI for managing the process.

</details>


* * * * * * * * * * * * * * * * * * 
* * * * * * * * * * * * * * * * * * 

## 📚 Table of Contents

### 🎯 General Information
1. [Introduction & Warning](#introduction--warning)
2. [Plugin Overview](#plugin-overview)
3. [EU_NativizationTool - Management Interface](#eu_nativizationtool---management-interface)

### 🏗️ Technical Aspects
4. [Internal Architecture – Working Principles](#internal-architecture--working-principles)
5. [Additional Useful Information](#additional-useful-information)

### 🧩 Description
6. [🧩 Plugin Description](#-plugin-description)

### 🚀 Getting Started
7. [Preparation](#preparation)
8. [Usage Example](#usage-example)

### ⚙️ Settings & Configuration
9. [Run Nativization Settings](#run-nativization-settings)
10. [Other Actions & Settings](#other-actions--settings)
11. [Nativization Settings](#nativization-settings)

### 📋 Features & Limitations
12. [Features & Limitations](#additional-useful-information)

### 📜 License & Documentation
13. [📜 License](#-license)

---

## 🔗 Useful Links
- [Unreal Engine Documentation](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-5-7-documentation?application_version=5.7)
- [Blueprint System Overview](https://docs.unrealengine.com/5.0/en-US/blueprint-system-overview-in-unreal-engine/)
- [C++ Programming Guide](https://docs.unrealengine.com/5.0/en-US/cpp-programming-in-unreal-engine/)

* * * * * * * * * * * * * * * * * * 
* * * * * * * * * * * * * * * * * * 


<h1 align="center"> 
Internal Architecture – Working Principles
</h1>

<details>
  <summary align="center">⚙️ Expand Description</summary>

Generally, the plugin works as follows:
- First, it identifies all dependent assets. These are automatically included in the list for code generation.
- Then, code is generated for each asset. Four main structures are supported: regular Blueprint (including components and more), Interface, Struct, and Enum. It's worth discussing the generation of standard Blueprints in more detail.

The core parsing is handled by registered BaseTranslatorObject derivatives, or simply Translators, set in the plugin settings. These periodically modify or use the algorithm described below.

Blueprints first generate EntryNodes. Instead of using ready-made Functions, the plugin breaks them into a series of nodes that are only partially equivalent to the original Functions. Most importantly, the final breakdown ensures that no sequence of Entry Nodes is cyclic. Translators adjust whether to loop back a node, whether temporary Entry Nodes should be generated, and whether they should be generated at all.

Next, Includes are generated separately for CPP and H files. Includes are generated by parsing variables, nodes, parent classes, interfaces, etc. Translators process nodes and propose their specific includes. To avoid cyclic includes, all object references are declared solely in the CPP file and only their forward declaration is included in the Header file. Header and CPP includes are mutually exclusive.

Next, the CS (C++ Script) is generated.
The final CS, along with other modifications to the existing files in the module, applies only changes to the file rather than replacing the entire file.

Translators also affect the CS.

Then, separate generation of Header and CPP follows.

Header generation is quite straightforward. The main class elements are traversed, especially its Property, Function, and Delegate, which are then declared within the class. Additionally, constructor and SetupInputComponent are declared, but only if the relevant translator is activated. Larger functions and variables are marked with the closest Blueprint-matching U-macros. Translators can also add new variables to the Header.

The CPP code is generated after the Header. Supporting functions for Header and CPP, such as the constructor and SetupInputComponent, are implemented.
The constructor iterates over all FProperty contained within the Actor and its components, identifies inconsistencies, filters mostly those variables inaccessible to Blueprint (and thus derived from other variables), cross-references with the Getter-Setter array, and subsequently initializes all differing variables with their modified values. For structures with no constructor registered in Nativization Settings, a special ManyLineInitialization is used, constructed on the principle of default initialization via cloning, recursively, using the copy constructor.

The process then continues with a pass through the Node. The traversal may be direct (forming the main execution sequence) or reverse, primarily for all non-Exec pins. Generation starts from the corresponding EntryNode, checks for the absence of a translator, and proceeds to the next node, appending its result to the current one via recursion. Translators, particularly those controlling the flow, function similarly by intercepting recursion through their Out Exec pins. The reverse path structurally resembles the direct path but is slightly more complex. In general, the process is similar but includes adjustments for how split pins are processed.

Translators process everything except Event nodes, Function Entry, and Enhance Nodes.

</details>

* * * * * * * * * * * * * * * * * * 

<h1 align="center"> 
Additional Useful Information
</h1>

<details>
  <summary align="center">⚙️ Expand Description</summary>

It should be noted that there are two approaches to generation in Blueprint and, in general, how exactly a Blueprint is initialized.
During the compilation of a Blueprint, its code is simplified into bytecode. Most data is simplified for optimization purposes. During the compilation phase, components such as FProperty and UFunction appear within Blueprint as reflection objects. All of this is derived from the original Blueprint and its raw data.
Hypothetically, parsing the original Blueprint is much freer but also more complex.
Parsing the compiled part of the Blueprint, however, is much more precise and less susceptible to errors. Highly sophisticated plugins for pure nativization likely rely on the compiled part rather than the raw data.

My plugin employs a combined approach. This is because the compiled part is generally convenient for parsing but comes with several issues. Initially, I leaned toward parsing the compiled part and later transitioned to parsing raw data. This created a certain systemic contradiction, which I chose to put aside for now.

The plugin supports macros/Composite but not cyclic macros, which are not pre-defined in the translator system. This is partly due to the plugin's design ideology, where code processes macros or composite nodes by unfolding them during parsing. This is implemented thanks to a stored Generated Code. Hypothetically, cyclic macros are not particularly difficult to parse, but they inevitably lead to code pollution with a massive number of generated functions, which are already abundant in the final code. Furthermore, most cyclic macros generally have simple structures in C++ code, so creating a specific translator for your needs is likely a better solution than attempting inclusion.

Edit InLine Object, Instance Struct, all non-standard objects, replacement of components in Child Actors are not supported.

</details>

* * * * * * * * * * * * * * * * * * 




## 🧩 Plugin Description

<div align="center">
  <img style="width: 80%; height: auto;" alt="EU_NativizationTool" src="./media/Tutorial\Article_1/image6.png"/>
</div>

<details>
  <summary align="center">⚙️ Expand Description</summary>

BP To CPP Converter is a plugin that provides seamless translation of Blueprints into readable C++. With a single click, it allows the conversion of Blueprint schematics into C++ code. The transformation process is called nativization, which, strictly speaking, is a misnomer. The final code includes the complete transformation of functions from Blueprint nodes into C++ code.

The core of the project is the Editor Widget **EU_NativizationTool**, which is the key to control. Let's break it down further:

Three tabs:
1. **Run Nativization** – Main tab to start the nativization process.
2. **Apply From Cache Nativization Result** – Main tab to transfer nativized Actor results into actual code.
3. **Other Actions** – Auxiliary utilities for the plugin.

</details>

* * * * * * * * * * * * * * * * * * 

<h2 align="center">
  <a href="#-table-of-contents">⬆️ Back to Top</a> 
</h2>

<h1 align="center"> 
Preparation
</h1>

<div align="center">
  <img style="width: 80%; height: auto;" alt="Initialize Module" src="./media/Tutorial\Article_1/image9.png"/>
</div>

<details>
  <summary align="center">⚙️ Expand Description</summary>

When first using the plugin, I strongly recommend initializing the **BlueprintNativizationModule**. This module serves as the space where you save all C++ codes for subsequently transferring existing Blueprint Assets to them. The resulting code will be structured. To initialize, go to Other Action, click the **Initialize Blueprint Initialization Module** button, and recompile the project. If everything is done correctly, the module status at the top of your toolbar will change. This action needs to be performed ONLY ONCE BEFORE STARTING WORK.

</details>

* * * * * * * * * * * * * * * * * * 

<h2 align="center">
  <a href="#-table-of-contents">⬆️ Back to Top</a> 
</h2>

<h1 align="center"> 
Usage Example
</h1>

<div align="center">
  <img style="width: 80%; height: auto;" alt="Run Nativization UI" src="./media/Tutorial\Article_1/image5.png"/>
</div>

<details>
  <summary align="center">⚙️ Expand Description</summary>

1. Go to the **Run Nativization** tab.
2. Specify in Blueprints any of your assets.
   - Ensure the Blueprint is compiled and saved.
   - You can specify one or multiple assets.
   - All entities specified in Blueprints, as well as those dependent on them, will also be subject to nativization.
3. Press the **Apply** button.
   - Below, in the editor, you'll see the final code.

To test the plugin, you may use your own or try **TestNativizationActor** or any other from the **Tests** folder.

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

Allows code generation for only one selected function. Check **True**, and select the function name in the **Function Name** field. Functions can be generated as a series if multiple functions exist. For example, this occurs when an input action function or a function is broken into sub-functions during nativization.

---

### Transform Only One File Code

Generates code only for the currently specified Blueprint, ignoring all other dependency recursion.

---

### SaveToFile

Allows saving the entire result to a file. This feature and many others will not function correctly if you have not initialized the BlueprintNativizationModule.

It is often considered good practice to leave all references to assets on the Blueprint side.

**Left All Asset Ref In Blueprint** – A flag to achieve this functionality. Otherwise, all references will be hardcoded in C++.

---

### Visualization

Flags that determine what is displayed in the editor below. You can disable Header or CPP code display.

---

### SaveOutputFolder

Determines where the generated C++ code should be saved. If left empty, it will save files in the initialized BlueprintNativizationModule with proper folder distribution.

---

### Hot Reload and Replace

An experimental feature that allows automatically replacing Blueprints with the resulting C++ class without restarting the project. As it might cause issues, **Save Cache** is often used instead.

---

### Save Cache

Retains information about which objects were generated from what. This helps to fix errors and recompile the project while preserving the ability to replace Blueprints with the generated C++ code between UE sessions.

---

### Cache Path

Similar to Save Output Folder, it saves cache data into a folder. If left empty, it will save to the root of the BlueprintNativizationModule.

To use the cache, navigate to the **Apply From Cache Nativization Result** tab. Note that applying cache assumes restarting the Unreal Engine project; otherwise, CDO classes won't initialize.

Leaving the **Cache Path** empty allows the cache to be used from the project's root.

---

</details>

* * * * * * * * * * * * * * * * * * 

<h2 align="center">
  <a href="#-table-of-contents">⬆️ Back to Top</a> 
</h2>

<h1 align="center"> 
Other Actions & Settings
</h1>

<div align="center">
  <img style="width: 80%; height: auto;" alt="Editor Settings" src="./media/Tutorial\Article_1/image3.png"/>
</div>

<details>
  <summary>⚙️ Expand Description</summary>

**Other Actions** contain additional useful functions, but aside from **Initialize Blueprint Initialization Module**, there's **Reset Names**. The project aims to carefully avoid name conflicts that may arise if you perform nativization in series without transferring the **Apply** results of generated code. Name conflicts are resolved using a specially designated Unique Name system. Although not mandatory, clearing unused cache in the system for temporarily allocated variables can often be beneficial with **Reset Names**. However, I recommend sequential steps during nativization with object transfers to BP or performing one large series. Partial caching applies only within a single Unreal Engine session.

**PrintAllK2Nodes** – Ignore.

The widget settings are presumed to change from their state continuously. Permanent settings are located under **Blueprint Nativization V2 Editor Settings** in **Editor Settings**.

</details>

* * * * * * * * * * * * * * * * * * 

<h2 align="center">
  <a href="#-table-of-contents">⬆️ Back to Top</a> 
</h2>

<h1 align="center"> 
Nativization Settings
</h1>

<div align="center">
  <img style="width: 60%; height: auto;" alt="Additional UI" src="./media/Tutorial\Article_1/image4.png"/>
</div>

<details>
  <summary>⚙️ Expand Description</summary>

The primary mechanism for translating Blueprint functions into C++ lies in Translators. A **Translator** handles one or more types of K2Node, converting them into C++ code.

---

### Global Variable Name

Used to avoid name conflicts.

---

### Setup Action Object

Represents the object that links **EU_NativizationTool** with various auxiliary widgets on the Blueprint side. It should remain unchanged.

---

### Enable Generate Value Suffix

Determines whether all generated variables in C++ code will have a **GeneratedValue** suffix. Disabling it results in cleaner code.

---

### Add BP Prefix To Parent Blueprint

Controls whether existing Blueprint classes rebuilt with C++ inheritance gain the **"BP_"** prefix.

---

### Function Redirects

A list of Blueprint-implemented functions. Such functions lack sufficient metadata to pinpoint which function invokes them elsewhere or where to override them in C++. This array maps the Blueprint-implemented function name and the original C++ function.

---

### Construction Descriptors

Specifies constructors for structures where using a constructor is the most appropriate choice for initialization. In other cases, direct initialization through `.` is used. For example, instead of `FLinearColor(0.0,0.66,1.0,1.0)`, it will generate `FLinearColor LinearColor(); LinearColor.R = 1.0; LinearColor.G = 1.0;` etc. This is implemented to overcome the lack of reflection for such cases. For structures generated in Blueprints, a complete constructor is implemented by default.

---

### Ignore Class to Ref Generate

Lists all classes that should not undergo nativization. Typically, this includes UI, Widget, etc. Avoid altering this widget or attempting to generate C++ for UI as it may lead to crashes.

---

### Ignore Assets to Ref Generate

The equivalent setting for ignoring assets.

---

### Getter And Setter Description

**FProperty** lacks information about the accessibility (private or public) of variables, which can cause a crash when attempting to access upper-level objects. For this purpose, the list allows mapping variables to operations such as **Get**, **Set**, or ignoring the variable.

---

### Code Editor

Handles information about the visual presentation in the **Text Editor** part of the main widget. Primarily contains information about which substrings to highlight in specific colors.

---

</details>


* * * * * * * * * * * * * * * * * * 
* * * * * * * * * * * * * * * * * * 


<h1 align="center"> 📜 License</h1>
<h2 align="center">
  <strong>---></strong>
  <strong> This project is distributed under the </strong> 
  <a href="./LICENSE">SoulofAO License</a>
  <strong><---</strong>
</h1>

---

<h2 align="center"> 
📚 Check Out the Documentation 
</h2>

<p align="center">
  <strong>---></strong>
  <a href="/README.md">Русский</a> |
  <a href="/docs/README.en.md">English</a> |
  <a href="/docs/README.es.md">Spanish</a> |
  <a href="/docs/README.zh.md">Chinese</a> |
  <strong><---</strong>
</p>
