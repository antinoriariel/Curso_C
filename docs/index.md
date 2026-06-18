---
title: Inicio
hide:
  - navigation
---

# Curso Completo de C — De Cero a Experto

!!! abstract "Sobre este curso"
    Un recorrido **académico y 100 % práctico** por el lenguaje de programación
    C, desde el primer `printf` hasta escribir un módulo del kernel de Linux,
    un compilador, un motor de base de datos o una red neuronal entrenada *desde
    cero*. Cubre los estándares modernos **C17 y C23** y conecta cada tema con la
    **actualidad** de la ingeniería de software (2024–2026): la presión
    regulatoria por la *memory safety*, la criptografía post-cuántica, `io_uring`,
    eBPF, WebAssembly y la convivencia de C con Rust y Zig.

<div class="grid cards" markdown>

-   :material-school:{ .lg .middle } **40 capítulos**

    ---

    Más de 200 subcapítulos organizados en cuatro niveles progresivos:
    *Fundamentos*, *Avanzado*, *Experto* y *Maestría*.

-   :material-code-braces:{ .lg .middle } **Código compilable**

    ---

    Cada ejemplo compila con `gcc -std=c17 -Wall -Wextra -pedantic`. Donde se
    usan extensiones (POSIX, GNU, C23) se indica explícitamente.

-   :material-flask:{ .lg .middle } **Parte práctica**

    ---

    Ejercicios progresivos con pistas y soluciones, proyectos acumulativos y un
    portafolio final publicable.

-   :material-newspaper:{ .lg .middle } **Conectado a la actualidad**

    ---

    Cada capítulo incluye una sección *Conexión con la actualidad* que vincula
    la teoría con debates y tecnología vigentes.

</div>

## Mapa del curso

| Nivel | Capítulos | Enfoque |
|-------|-----------|---------|
| **Fundamentos** | 1–10 | Sintaxis, memoria, punteros, E/S, compilación |
| **Avanzado y Sistemas** | 11–20 | Estructuras de datos, concurrencia, redes, criptografía, compiladores, embebidos |
| **Experto** | 21–30 | HPC, juegos, kernel, WebAssembly, verificación formal, ML, bases de datos, seguridad |
| **Maestría** | 31–40 | VMs/JIT, motores de BD, sistemas distribuidos, microservicios, robótica, deep learning, TLPI, portafolio |

## Cómo está estructurado cada capítulo

Todos los capítulos siguen el mismo molde pedagógico:

1. **Objetivos de aprendizaje** — qué sabrás hacer al terminar.
2. **Desarrollo teórico** por subcapítulos, con código compilable y diagramas.
3. **Conexión con la actualidad** — relevancia en la industria 2024–2026.
4. **Ejercicios prácticos** — con pistas y criterios de evaluación.
5. **Referencias** — estándar ISO, libros canónicos y documentación viva.

## Requisitos previos

- Conocimientos básicos de uso de la terminal y archivos.
- Un compilador de C: [GCC](https://gcc.gnu.org/), [Clang](https://clang.llvm.org/) o MSVC.
- Un editor o IDE: VS Code, CLion, Neovim…
- Curiosidad por entender *cómo* funcionan las cosas por dentro.

## Convenciones tipográficas

!!! note "Nota"
    Aclaraciones y contexto adicional.

!!! warning "Cuidado"
    Errores frecuentes y *undefined behavior* (UB).

!!! tip "Buenas prácticas"
    Recomendaciones idiomáticas de C moderno.

```c
// Los bloques de código indican el estándar y los flags recomendados.
// gcc -std=c17 -Wall -Wextra -pedantic ejemplo.c -o ejemplo
#include <stdio.h>

int main(void) {
    puts("¡Bienvenido al curso de C!");
    return 0;
}
```

---

> «C es un lenguaje pequeño y elegante atrapado dentro de un lenguaje grande y
> verboso pidiendo salir.» — *adaptación de una célebre frase sobre C++*

Empieza por la [Guía: cómo estudiar](guia/como-estudiar.md) o salta directo al
[Capítulo 1](capitulos/capitulo-01.md).
