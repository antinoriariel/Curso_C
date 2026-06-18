---
tags:
  - Intermedio
  - Preprocesador
  - Build
---

# Capítulo 10 — Preprocesador y Compilación

!!! abstract "Objetivos de aprendizaje"
    - Dominar las directivas del preprocesador y las macros (incluidas las
      «con argumentos» y sus trampas).
    - Organizar proyectos en **compilación separada** con *header guards*.
    - Automatizar *builds* con **Make** y **CMake**.
    - Crear y enlazar **bibliotecas estáticas y dinámicas**.

---

## 10.1 Directivas del preprocesador

El preprocesador es un manipulador de texto que actúa **antes** de la compilación:

```c
#include <stdio.h>        // inclusión de archivos
#include "miheader.h"     // "..." busca primero en el directorio local
#define PI 3.14159        // macro objeto
#define MAX(a,b) ((a)>(b)?(a):(b))   // macro función

#ifdef DEBUG
    #define LOG(...) fprintf(stderr, __VA_ARGS__)
#else
    #define LOG(...) ((void)0)
#endif

#if defined(__linux__)
    // código específico de Linux
#endif
```

---

## 10.2 Macros avanzadas

```c
#define STR(x) #x             // stringize: STR(hola) → "hola"
#define CONCAT(a,b) a##b      // token pasting: CONCAT(foo,bar) → foobar
#define LOG(...) printf(__VA_ARGS__)   // macros variádicas (__VA_ARGS__)
```

!!! warning "Las macros son texto, no funciones"
    `#define CUAD(x) x*x` y luego `CUAD(a+b)` se expande a `a+b*a+b` — ¡mal!
    Pon **paréntesis** en cada argumento y en todo el cuerpo:
    `#define CUAD(x) ((x)*(x))`. Aun así, `CUAD(i++)` incrementa `i` dos veces.
    Prefiere funciones `static inline` cuando puedas.

Macros predefinidas útiles: `__FILE__`, `__LINE__`, `__func__`, `__DATE__`,
`__STDC_VERSION__`.

---

## 10.3 y 10.4 Constantes predefinidas, pragmas y plataforma

```c
#pragma once          // alternativa no estándar (pero ubicua) al header guard
#pragma pack(1)       // empaquetado de structs
#error "Mensaje"      // aborta la compilación con un mensaje
```

Detección de plataforma/compilador: `__GNUC__`, `_MSC_VER`, `__clang__`,
`_WIN32`, `__APPLE__`.

---

## 10.5 Compilación separada

Cada módulo se divide en interfaz (`.h`) e implementación (`.c`). El *header
guard* evita inclusiones múltiples:

```c title="mat.h"
#ifndef MAT_H
#define MAT_H
int suma(int a, int b);     // declaración (interfaz)
#endif
```

```c title="mat.c"
#include "mat.h"
int suma(int a, int b) { return a + b; }   // definición
```

```c title="main.c"
#include "mat.h"
#include <stdio.h>
int main(void) { printf("%d\n", suma(2, 3)); }
```

```bash
gcc -c mat.c -o mat.o
gcc -c main.c -o main.o
gcc mat.o main.o -o programa
```

---

## 10.6 Make y CMake

### Make

```makefile title="Makefile"
CC      := gcc
CFLAGS  := -std=c17 -Wall -Wextra -Wpedantic -O2 -g
SRC     := $(wildcard src/*.c)
OBJ     := $(SRC:.c=.o)
BIN     := programa

$(BIN): $(OBJ)
	$(CC) $(CFLAGS) $^ -o $@

%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	$(RM) $(OBJ) $(BIN)

.PHONY: clean
```

### CMake (multiplataforma)

```cmake title="CMakeLists.txt"
cmake_minimum_required(VERSION 3.20)
project(Programa C)
set(CMAKE_C_STANDARD 17)
set(CMAKE_C_STANDARD_REQUIRED ON)
add_executable(programa src/main.c src/mat.c)
target_compile_options(programa PRIVATE -Wall -Wextra -Wpedantic)
```

```bash
cmake -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build
```

---

## 10.7 Bibliotecas estáticas y dinámicas

```bash
# Estática (.a): se copia dentro del ejecutable
gcc -c mat.c -o mat.o
ar rcs libmat.a mat.o
gcc main.c -L. -lmat -o programa

# Dinámica (.so): se enlaza en tiempo de ejecución
gcc -fPIC -c mat.c -o mat.o
gcc -shared mat.o -o libmat.so
gcc main.c -L. -lmat -o programa
LD_LIBRARY_PATH=. ./programa
```

| | Estática (`.a`/`.lib`) | Dinámica (`.so`/`.dll`) |
|--|------------------------|--------------------------|
| Tamaño binario | Mayor | Menor |
| Despliegue | Autocontenido | Requiere la librería |
| Actualizaciones | Recompilar | Reemplazar la `.so` |

---

## Conexión con la actualidad

El preprocesador es a la vez imprescindible y problemático: su naturaleza
puramente textual lo hace propenso a errores sutiles, y por eso C23 incorpora
alternativas más seguras como `constexpr` (constantes con tipo en lugar de
`#define`), `[[attributes]]` estándar y, sobre todo, **`#embed`**, que incrusta
archivos binarios en el ejecutable sin *scripts* externos — una mejora celebrada
para *firmware* y recursos. En el frente de *build systems*, 2024–2025 ha visto
crecer alternativas a Make/CMake como **Meson**, **Ninja** y **Bazel** por su
velocidad y reproducibilidad, y el ecosistema explora gestores de dependencias
(**vcpkg**, **Conan**) que C tradicionalmente no tenía. La seguridad de la
**cadena de suministro** (*supply chain*) — reproducibilidad de builds, SBOM,
firma de artefactos — es hoy una preocupación de primer orden tras incidentes
como la puerta trasera de **xz/liblzma (CVE-2024-3094)** en 2024, que se coló
precisamente a través del sistema de compilación.

---

## Ejercicios

!!! example "Ejercicio 10.1 — Macro segura ★★"
    Escribe `MIN`, `MAX` y `CLAMP` con paréntesis correctos. Demuestra con un
    caso por qué `CLAMP(i++, lo, hi)` es peligroso y reimplementa con `inline`.

!!! example "Ejercicio 10.2 — Proyecto multi-archivo ★★"
    Divide un programa monolítico en `util.h/.c`, `main.c` y un `Makefile` con
    regla `clean` y compilación incremental.

!!! example "Ejercicio 10.3 — Tu primera librería ★★★"
    Compila tu vector dinámico del cap. 7 como `.a` **y** como `.so`, y enlázalo
    desde un programa de prueba.

!!! example "Ejercicio 10.4 — CMake multiplataforma ★★★"
    Migra el proyecto anterior a CMake y compílalo en dos plataformas (o WSL +
    Windows).

---

## Referencias

- ISO/IEC 9899:2018, §6.10 (directivas de preprocesamiento).
- *Managing Projects with GNU Make* (Robert Mecklenburg), O'Reilly.
- [CMake Documentation](https://cmake.org/documentation/).
- Análisis del backdoor xz/liblzma (CVE-2024-3094), 2024.
