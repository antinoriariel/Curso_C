---
tags:
  - Experto
  - WebAssembly
  - Emscripten
---

# Capítulo 24 — WebAssembly y Emscripten

!!! abstract "Objetivos de aprendizaje"
    - Entender qué es **WebAssembly (WASM)** y su modelo de ejecución.
    - Compilar C a WASM con **Emscripten** y conectarlo con JavaScript.
    - Gestionar la memoria lineal de WASM y optimizar el tamaño/rendimiento.
    - Conocer **WASI** para ejecutar C fuera del navegador.

---

## 24.1 Introducción a WebAssembly

WASM es un formato binario portable, de **bajo nivel y casi nativo**, pensado
como objetivo de compilación para C/C++/Rust. Se ejecuta en una máquina virtual
de pila *sandboxed* dentro del navegador (o en *runtimes* autónomos). Modelo:
**memoria lineal** (un gran `ArrayBuffer`), tipos numéricos básicos, sin acceso
directo al sistema (todo pasa por imports).

---

## 24.2 Compilar C a WASM con Emscripten

```c title="suma.c"
#include <emscripten.h>

EMSCRIPTEN_KEEPALIVE        // evita que el optimizador la elimine
int suma(int a, int b) { return a + b; }
```

```bash
emcc suma.c -O3 -o suma.js \
     -sEXPORTED_FUNCTIONS=_suma \
     -sEXPORTED_RUNTIME_METHODS=ccall,cwrap
```

```javascript
// En el navegador / Node:
const suma = Module.cwrap('suma', 'number', ['number', 'number']);
console.log(suma(2, 3));   // 5
```

Emscripten también puede generar una página HTML completa (`-o index.html`) y
emular SDL2, OpenGL (vía WebGL) y un sistema de archivos virtual.

---

## 24.3–24.5 Interoperabilidad, memoria y optimización

- **C ↔ JS**: `ccall`/`cwrap`, `EM_JS`/`EM_ASM` para incrustar JS, y paso de
  *buffers* a través de la memoria lineal (`HEAPU8`, `_malloc`/`_free`).
- **Memoria**: punteros = *offsets* en la memoria lineal; cuidado con el
  *ownership* al cruzar la frontera.
- **Optimización**: `-O3`, `-Os`/`-Oz` para tamaño, `-sALLOW_MEMORY_GROWTH`,
  `wasm-opt` (de Binaryen).

---

## 24.6 WASI

**WASI** (*WebAssembly System Interface*) define una API tipo POSIX para que el
código WASM acceda a archivos, red y reloj **fuera** del navegador, de forma
*capability-based* (seguridad por capacidades).

```bash
# Compilar con el SDK de WASI y ejecutar en wasmtime/wasmer
clang --target=wasm32-wasi hola.c -o hola.wasm
wasmtime hola.wasm
```

---

## 24.7–24.9 Aplicaciones, benchmarks y WAT

Casos reales: portar bibliotecas C (FFmpeg, SQLite, OpenCV) al navegador,
videojuegos, herramientas científicas. WASM alcanza típicamente el **50–90 % del
rendimiento nativo**. El formato textual **WAT** permite leer/escribir WASM a
mano.

---

## Conexión con la actualidad

WebAssembly es una de las tecnologías de sistemas más relevantes de la década, y
C es uno de sus lenguajes fuente por excelencia. En el navegador, proyectos como
**SQL.js** (SQLite compilado a WASM), **FFmpeg.wasm** y **Photoshop web** (Adobe
portó su núcleo C++ a WASM) demuestran su potencia. Pero la verdadera frontera de
2024–2025 está **fuera** del navegador: el **WASI Preview 2** y el **Component
Model** están convirtiendo WASM en un formato universal de *plugins* y funciones
*serverless* —adoptado por Fastly, Cloudflare Workers, Docker (`docker run
--runtime=wasm`) y Kubernetes (runwasi)— por su arranque en microsegundos y su
*sandboxing* por capacidades, más seguro que los contenedores. Compilar tu C a
WASM con WASI es hoy una vía directa a *edge computing* y *plugins* portables.

---

## Ejercicios

!!! example "Ejercicio 24.1 — Hola WASM ★★"
    Compila una función C a WASM y llámala desde una página HTML.

!!! example "Ejercicio 24.2 — Procesado de imagen en el navegador ★★★"
    Porta tu filtro de imagen del cap. 17 a WASM y aplícalo a un `<canvas>`.

!!! example "Ejercicio 24.3 — Paso de buffers ★★★"
    Pasa un array grande de JS a C a través de la memoria lineal, procésalo y
    devuélvelo, gestionando `_malloc`/`_free`.

!!! example "Ejercicio 24.4 — WASI CLI ★★★★"
    Compila una herramienta CLI en C a WASI y ejecútala con `wasmtime`, leyendo
    un archivo del *host*.

---

## Referencias

- [Documentación de Emscripten](https://emscripten.org/).
- [WebAssembly.org](https://webassembly.org/) y la especificación.
- [WASI](https://wasi.dev/), [Wasmtime](https://wasmtime.dev/).
- *Programming WebAssembly with Rust* (conceptos aplicables a C).
