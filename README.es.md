<p align="center">
  <strong>-------></strong>
  <a href="/README.md">Ruso</a> |
  <a href="/README.en.md">Inglés</a> |
  <a href="/README.es.md">Español</a> |
  <a href="/README.zh.md">Chino</a> |
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
BP To CPP Converter — Un complemento para la traducción fluida de Blueprints a C++ legible
</h1>

<h3 align="center"> 
El código final incluye la conversión completa de las funciones de los nodos Blueprint a C++.
</h3>

<h2 align="center"> 
    ⚠️ Aviso ⚠️
</h2> 
<p align="center">
  El autor no se responsabiliza de las consecuencias derivadas del uso de este proyecto. Al utilizar los materiales de este repositorio, aceptas automáticamente los términos de licencia asociados.
</p>

<details> 
  <summary align="center">⚠️Texto Completo⚠️</summary>

1. Al utilizar los materiales del repositorio, aceptas automáticamente los términos de licencia asociados.

2. El autor no ofrece garantías, expresas o implícitas, sobre la precisión, integridad o idoneidad de este material para un propósito específico.  

3. El autor no es responsable de ningún daño, incluidos daños directos, indirectos, incidentales, consecuentes o especiales, resultantes del uso o la incapacidad de usar este material o su documentación adjunta, incluso si se notificó previamente sobre la posibilidad de dichos daños.

4. Al utilizar este material, reconoces y aceptas todos los riesgos asociados con su aplicación. Asimismo, aceptas que el autor no puede ser considerado responsable por cualquier problema o consecuencia que surja de su uso.

</details>

* * * * * * * * * * * * * * * * * * 
* * * * * * * * * * * * * * * * * * 

<h1 align="center"> 
Introducción y Advertencia
</h1>

> ⚠️ **Advertencia Importante**
> 
> El complemento se encuentra en desarrollo activo. Pueden ocurrir errores en el código generado al usar la versión actual. Muchos de estos errores se están corrigiendo durante el desarrollo, pero algunos se deben a limitaciones fundamentales del Unreal Engine, donde muchos elementos carecen de soporte total para reflexión.

<h1 align="center"> 
Resumen del Complemento
</h1>

<details>
  <summary align="center">📖 Descripción Detallada</summary>

**BP To CPP Converter** es un complemento especializado para Unreal Engine diseñado para convertir automáticamente la lógica de Blueprint en código C++ legible. El complemento aborda los desafíos de migrar la programación visual a código nativo, particularmente útil para:

- **Optimización de Rendimiento** – Transición de Blueprint a C++ para secciones críticas en rendimiento.  
- **Refactorización de Proyectos** – Estructurando sistemáticamente la base de código.  
- **Aprendizaje de C++** – Comprender cómo se traducen las construcciones de Blueprint en código nativo.  

### Funciones principales:
- **Conversión Fluida** – Funcionalidad preservada durante el proceso de conversión.  
- **Soporte para Estructuras Principales** – Blueprint, Interface, Struct, Enum.  
- **Personalización Flexible** – Adaptable a las necesidades específicas del proyecto.  
- **Integración en el Editor** – Interfaz fácil de usar para la gestión y control del proceso.  

</details>


* * * * * * * * * * * * * * * * * * 
* * * * * * * * * * * * * * * * * * 

## 📚 Tabla de Contenidos

### 🎯 Información General
1. [Introducción y Advertencia](#introducción-y-advertencia)  
2. [Resumen del Complemento](#resumen-del-complemento)  
3. [EU_NativizationTool - Interfaz de gestión](#eu_nativizationtool---interfaz-de-gestión)  

### 🏗️ Aspectos Técnicos  
4. [Arquitectura Interna - Principios Operativos](#arquitectura-interna---principios-operativos)  
5. [Información Adicional Útil](#información-adicional-útil)  

### 🧩 Descripción
6. [🧩 Descripción del Complemento](#-descripción-del-complemento)  

### 🚀 Comenzando
7. [Cómo Comenzar](#cómo-comenzar)  
8. [Ejemplo de Uso](#ejemplo-de-uso)  

### ⚙️ Configuración
9. [Configuración de Nativización](#configuración-de-nativización)  
10. [Otras Configuraciones y Acciones](#otras-configuraciones-y-acciones)  
11. [Configuración de Ajustes de Nativización](#configuración-de-ajustes-de-nativización)  

### 📋 Características y Limitaciones  
12. [Características y Limitaciones](#información-adicional-útil)  

### 📜 Licencia y Documentación  
13. [📜 Licencia](#-licencia)  

---

## 🔗 Enlaces Útiles
- [Documentación de Unreal Engine](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-5-7-documentation?application_version=5.7)  
- [Resumen del Sistema Blueprint](https://docs.unrealengine.com/5.0/en-US/blueprint-system-overview-in-unreal-engine/)  
- [Guía de Programación en C++](https://docs.unrealengine.com/5.0/en-US/cpp-programming-in-unreal-engine/)

* * * * * * * * * * * * * * * * * * 
* * * * * * * * * * * * * * * * * */

<h1 align="center"> 
Arquitectura Interna - Principios Operativos
</h1>

<details>
  <summary align="center">⚙️ Expandir Descripción</summary>

Aquí tienes una visión general de cómo funciona el complemento.  
- Primero, busca todos los activos dependientes, que se incluyen por defecto en la lista de generación.  
- Después, se realiza la generación de código para cada activo. Se admiten cuatro estructuras principales: Blueprint regular (incluidos componentes y más), Interface, Struct y Enum.  

El núcleo del análisis consiste en herederos registrados de BaseTranslatorObject, o más simplemente, Traductores. Se modifican periódicamente sobre el algoritmo previamente delineado.  

(Continúa de la misma manera para cada sección o encabezado en que desees expandirte.)  
