<p align="center">
  <strong>-------></strong>
  <a href="/README.md">Русский</a> |
  <a href="/docs/README.en.md">Inglés</a> |
  <a href="/docs/README.es.md">Español</a> |
  <a href="/docs/README.zh.md">Chino</a> |
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
BP To CPP Converter — Un Plugin para la Conversión Fluida de Blueprint a Código C++ Legible
</h1>

<h3 align="center"> 
El código final incluye la conversión completa de funciones de nodos Blueprint a C++.
</h3>

<h2 align="center"> 
    ⚠️ Descargo de Responsabilidad ⚠️
</h2> 
<p align="center">
  El autor no asume ninguna responsabilidad por posibles consecuencias del uso de este proyecto.
  Al utilizar los materiales de este repositorio, aceptas automáticamente los términos del acuerdo de licencia asociado.
</p>

<details> 
  <summary align="center">⚠️Texto Completo⚠️</summary>

1. Al utilizar los materiales de este repositorio, aceptas automáticamente los términos del acuerdo de licencia asociado.

2. El autor no otorga ninguna garantía, expresa o implícita, sobre la exactitud, integridad o idoneidad de este material para propósitos específicos.

3. El autor no será responsable de ningún daño, incluidos, entre otros, daños directos, indirectos, incidentales, consecuentes o especiales, derivados del uso o la imposibilidad de usar este material o su documentación acompañante, incluso si se advierte de la posibilidad de dichos daños.

4. Al utilizar este material, reconoces y aceptas todos los riesgos asociados con su aplicación. Además, aceptas que el autor no será responsable de ningún problema o consecuencia derivada de su uso.

</details>

* * * * * * * * * * * * * * * * * * 
* * * * * * * * * * * * * * * * * * 

<h1 align="center"> 
Introducción y Advertencia
</h1>

> ⚠️ **ADVERTENCIA IMPORTANTE**
> 
> Este plugin está actualmente en desarrollo activo. El uso de la versión actual puede resultar en errores en el código generado. Muchos de estos errores se resuelven durante el desarrollo, pero algunos se deben a limitaciones fundamentales del Unreal Engine, donde muchos elementos no admiten completamente la reflexión.

<h1 align="center"> 
Descripción del Plugin
</h1>

<details>
  <summary align="center">📖 Descripción Detallada</summary>

**BP To CPP Converter** es un plugin especializado para Unreal Engine, diseñado para la conversión automática de la lógica de Blueprint a código C++ legible. Este plugin aborda el desafío de migrar la programación visual a código nativo, lo cual es especialmente útil para:

- **Optimización del Rendimiento** – Transición de Blueprint a C++ en áreas críticas de rendimiento.
- **Refactorización de Proyectos** – Organización sistemática de códigos base.
- **Aprendizaje de C++** – Comprender cómo las construcciones de Blueprint se traducen a código nativo.

### Características Clave:
- **Conversión Fluida** – Garantiza que la funcionalidad se preserve durante la conversión.
- **Soporte para Estructuras Fundamentales** – Blueprint, Interface, Struct, Enum.
- **Configuración Flexible** – Se adapta a necesidades específicas del proyecto.
- **Integración en el Editor** – Interfaz amigable para administrar el proceso.

</details>


* * * * * * * * * * * * * * * * * * 
* * * * * * * * * * * * * * * * * * 

## 📚 Tabla de Contenidos

### 🎯 Información General
1. [Introducción y Advertencia](#introducción-y-advertencia)
2. [Descripción del Plugin](#descripción-del-plugin)
3. [EU_NativizationTool - Interfaz de Gestión](#eu_nativizationtool---interfaz-de-gestión)

### 🏗️ Aspectos Técnicos
4. [Arquitectura Interna – Principios de Funcionamiento](#arquitectura-interna--principios-de-funcionamiento)
5. [Información Adicional Útil](#información-adicional-útil)

### 🧩 Descripción
6. [🧩 Descripción del Plugin](#-descripción-del-plugin)

### 🚀 Comenzando
7. [Preparación](#preparación)
8. [Ejemplo de Uso](#ejemplo-de-uso)

### ⚙️ Configuración y Ajustes
9. [Ejecutar Configuración de Nativización](#ejecutar-configuración-de-nativización)
10. [Otras Acciones y Configuraciones](#otras-acciones-y-configuraciones)
11. [Configuración de Nativización](#configuración-de-nativización)

### 📋 Características y Limitaciones
12. [Características y Limitaciones](#información-adicional-útil)

### 📜 Licencia y Documentación
13. [📜 Licencia](#-licencia)

---

## 🔗 Enlaces Útiles
- [Documentación de Unreal Engine](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-5-7-documentation?application_version=5.7)
- [Descripción General del Sistema de Blueprint](https://docs.unrealengine.com/5.0/en-US/blueprint-system-overview-in-unreal-engine/)
- [Guía de Programación C++](https://docs.unrealengine.com/5.0/en-US/cpp-programming-in-unreal-engine/)

* * * * * * * * * * * * * * * * * * 
* * * * * * * * * * * * * * * * * * 


<h1 align="center"> 
Arquitectura Interna – Principios de Funcionamiento
</h1>

<details>
  <summary align="center">⚙️ Expandir Descripción</summary>

En general, el plugin funciona de la siguiente manera:
- Primero, identifica todos los activos dependientes. Estos se incluyen automáticamente en la lista para la generación de código.
- Luego, se genera código para cada activo. Se admiten cuatro estructuras principales: Blueprint regular (incluyendo componentes y más), Interface, Struct y Enum. Vale la pena discutir más a fondo la generación de Blueprints estándar.

El análisis principal lo manejan derivados registrados de BaseTranslatorObject, o simplemente Translations, configurados en los ajustes del plugin. Estos modifican periódicamente o usan el algoritmo descrito a continuación.

Los Blueprints primero generan EntryNodes. En lugar de usar Functions listas, el plugin las descompone en una serie de nodos solo parcialmente equivalentes a las Functions originales. Lo más importante es que la descomposición final asegura que ninguna secuencia de Entry Nodes sea cíclica. Los Translators ajustan si deben retroceder a un nodo, si deben generarse Entry Nodes temporales, y si deben generarse en absoluto.

A continuación, los Includes se generan por separado para archivos CPP y H. Se generan inspeccionando variables, nodos, clases padre, interfaces, etc. 

</details>

* * * * * * * * * * * * * * * * * * 

<h1 align="center"> 
Información Adicional Útil
</h1>

<details>
  <summary align="center">⚙️ Expandir Descripción</summary>

Existen dos enfoques en la generación en Blueprint, y generalmente en cómo se inicializa un Blueprint.
....
(cont.).
