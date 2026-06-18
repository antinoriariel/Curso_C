---
tags:
  - Maestría
  - Proyectos
  - Portafolio
---

# Capítulo 40 — Proyectos de Síntesis y Portafolio

!!! abstract "Objetivos del capítulo"
    Reunir todo el curso en **proyectos ambiciosos** dignos de un portafolio
    profesional, y aprender a **publicarlos** con la calidad de ingeniería que
    espera la industria: pruebas, CI, *sanitizers*, documentación y empaquetado.

!!! tip "Elige según tu itinerario"
    No hagas los diez. Elige **uno o dos** alineados con el área donde quieres
    trabajar (sistemas, datos, IA, redes, *gamedev*…) y llévalos a un nivel de
    excelencia.

---

## Los diez proyectos

Cada proyecto integra varios capítulos del curso:

| # | Proyecto | Integra capítulos |
|---|----------|-------------------|
| **A** | Navegador web minimalista (HTTP + parser HTML + render) | 13, 28, 33 |
| **B** | Emulador retro (Chip-8 / Game Boy) | 6, 8, 22, 31 |
| **C** | Motor de búsqueda de texto completo (índice invertido) | 11, 33, 27 |
| **D** | Editor de audio digital (DAW simplificado) | 17, 36 |
| **E** | Sistema de archivos en espacio de usuario (FUSE) | 9, 39 |
| **F** | Lenguaje de programación desde cero | 19, 31 |
| **G** | Framework de ML embebido | 26, 38, 20 |
| **H** | Cliente/servidor de *streaming* de vídeo | 13, 17, 28 |
| **I** | Base de datos SQL relacional completa | 19, 27, 32 |
| **J** | Implementación de Raft | 12, 13, 34 |

### 40.1 A — Navegador minimalista

Cliente HTTP/HTTPS (cap. 13, 28), parser de un subconjunto de HTML/CSS (cap. 33),
modelo de documento (árbol, cap. 11) y un *render* a terminal o a ventana
(cap. 22/36). Un proyecto que toca redes, *parsing* y estructuras de datos.

### 40.2 B — Emulador retro

El **Chip-8** es el «Hola, mundo» de la emulación: ~35 opcodes, ideal para
empezar (cap. 31). La **Game Boy** es el siguiente nivel (CPU LR35902, PPU,
*timing*). Enseña como nada el modelo de máquina, los bits (cap. 2, 6) y el
*timing* preciso.

### 40.3 C — Motor de búsqueda

Construye un **índice invertido** (token → lista de documentos, cap. 11, 33),
*ranking* TF-IDF/BM25 y una CLI de consulta (cap. 36). Núcleo de cualquier
buscador.

### 40.4–40.10 (resto)

DAW (cap. 17), FUSE (cap. 9, 39), un lenguaje completo (cap. 19, 31), ML embebido
(cap. 38), *streaming* de vídeo (cap. 17, 28), una base de datos SQL (cap. 27, 32)
y Raft (cap. 34). Cada uno es una pieza de portafolio por sí solo.

---

## 40.11 Guía de portafolio y publicación

Lo que convierte un proyecto en una **carta de presentación**:

=== "README profesional"

    - Descripción clara en una frase + capturas/GIF.
    - Instalación, uso y ejemplos copy-paste.
    - Arquitectura (un diagrama) y decisiones de diseño.
    - *Badges* de CI, licencia y cobertura.

=== "Calidad de código"

    - Estilo consistente (`clang-format`), **cero warnings** con `-Wall -Wextra`.
    - Documentación Doxygen.
    - Limpio bajo ASan/UBSan/Valgrind (caps. 7, 29) y SAST (cap. 25).

=== "Pruebas y CI"

    - Unitarias + integración, cobertura > 80 %.
    - GitHub Actions: compilar en varias plataformas, ejecutar pruebas,
      sanitizers, *fuzzing* (cap. 30).

=== "Empaquetado y difusión"

    - `Makefile`/`CMake`, imagen Docker, *release* con binarios.
    - Publica en GitHub; comparte en `r/C_Programming`, Hacker News, Lobsters.

```yaml title=".github/workflows/ci.yml (esqueleto)"
name: CI
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: sudo apt-get install -y valgrind
      - run: make CFLAGS="-std=c17 -Wall -Wextra -Werror -fsanitize=address,undefined"
      - run: make test
      - run: valgrind --error-exitcode=1 --leak-check=full ./build/tests
```

---

## Conexión con la actualidad

En un mercado donde la presión por la *memory safety* (caps. 6, 30) pone a C bajo
escrutinio, la mejor forma de demostrar tu valía no es afirmar que «sabes C», sino
**enseñar un proyecto** en C que sea verificablemente robusto: limpio bajo
sanitizers, *fuzzeado*, con CI verde y pruebas. Eso es exactamente lo que buscan
los equipos de sistemas, *embedded*, bases de datos, seguridad e infraestructura
de IA, donde C sigue siendo insustituible. Proyectos *open-source* recientes como
`llm.c`, `chibicc`, `ggml` o `wasm3` se convirtieron en cartas de presentación de
sus autores precisamente por su calidad de ingeniería. Tu portafolio en C, pulido
con la disciplina de este curso, es tu activo más convincente.

---

## Ejercicios / Entregable final

!!! example "Proyecto Capstone ★★★★★"
    Elige un proyecto de la tabla (o propón el tuyo), planifícalo (`DISEÑO.md`),
    impleméntalo en hitos, y publícalo cumpliendo **toda** la guía de portafolio:
    README, pruebas >80 %, CI con sanitizers, Doxygen y *release*. Este es el
    cierre del curso.

!!! example "Revisión por pares ★★★"
    Intercambia tu proyecto con otra persona y realizad una revisión de código
    mutua usando una *checklist* CERT-C (cap. 30).

---

## Referencias

- *Build Your Own X* (repositorio comunitario) — guías de muchos de estos proyectos.
- *The Pragmatic Programmer* (Hunt & Thomas) — oficio del ingeniero.
- Robert Nystrom. *Crafting Interpreters* (proyecto F).
- [Awesome C](https://github.com/oz123/awesome-c) — librerías y recursos de C.
