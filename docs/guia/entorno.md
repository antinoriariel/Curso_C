# Entorno y herramientas

Antes de escribir una línea de C necesitas un **toolchain** funcional. Esta
guía cubre los tres sistemas operativos principales y el conjunto mínimo de
herramientas que usaremos a lo largo del curso.

## Instalación del compilador

=== "Linux (Debian/Ubuntu)"

    ```bash
    sudo apt update
    sudo apt install build-essential clang gdb valgrind make cmake
    gcc --version
    ```

=== "macOS"

    ```bash
    xcode-select --install          # Clang + herramientas
    brew install gcc gdb make cmake  # GCC real (no el alias de Clang)
    clang --version
    ```

=== "Windows"

    Opción recomendada: **WSL2** (Ubuntu) para máxima fidelidad con entornos
    POSIX. Alternativas nativas:

    ```powershell
    winget install GnuWin32.Make
    # MSYS2 para un GCC nativo:
    winget install MSYS2.MSYS2
    # Dentro de MSYS2:  pacman -S mingw-w64-ucrt-x86_64-gcc gdb
    ```

    MSVC se instala con *Visual Studio Build Tools*; se invoca con `cl`.

## Verificación

```c title="hola.c"
#include <stdio.h>

int main(void) {
    printf("Toolchain operativo. C estándar: %ld\n", __STDC_VERSION__);
    return 0;
}
```

```bash
gcc -std=c17 -Wall -Wextra -pedantic hola.c -o hola
./hola
# Toolchain operativo. C estándar: 201710
```

El valor de `__STDC_VERSION__` te dice qué estándar aplica el compilador:

| Valor | Estándar |
|-------|----------|
| `199901L` | C99 |
| `201112L` | C11 |
| `201710L` | C17 |
| `202311L` | C23 |

## Herramientas que usaremos

| Herramienta | Para qué | Capítulo donde aparece |
|-------------|----------|------------------------|
| `gcc` / `clang` | Compilación | 1 |
| `gdb` / `lldb` | Depuración | 1, 29 |
| `valgrind` | Fugas y errores de memoria | 7, 29 |
| AddressSanitizer / UBSan | Detección en runtime | 7, 30 |
| `make` / `cmake` | Automatización de builds | 10 |
| `clang-tidy` / `cppcheck` | Análisis estático | 25, 30 |
| `perf` | Perfilamiento de CPU | 21, 29 |
| `git` | Control de versiones | 40 |
| Doxygen | Documentación | 14, 40 |

## Construir el sitio del curso (MkDocs)

Este material es en sí mismo un proyecto MkDocs. Para servirlo localmente:

```bash
python -m pip install -r requirements.txt
mkdocs serve          # http://127.0.0.1:8000
mkdocs build          # genera el sitio estático en ./site
```

!!! tip "Editor recomendado"
    VS Code con las extensiones *C/C++* (Microsoft), *clangd*, *CodeLLDB* y
    *Markdown Preview Enhanced* cubre todo el ciclo: editar, compilar, depurar y
    previsualizar la documentación.

## Plantilla de proyecto

A lo largo del curso usaremos esta estructura mínima por proyecto:

```text
proyecto/
├── src/          # .c
├── include/      # .h
├── tests/        # pruebas
├── Makefile
└── README.md
```

En el [Apéndice G](../apendices/apendice-g.md) tienes un `Makefile` de
referencia listo para copiar.
