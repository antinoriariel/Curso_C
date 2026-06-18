---
tags:
  - Proyecto
  - Integrador
---

# Capítulo 15 — Proyecto Final Integrador

!!! abstract "Objetivos del proyecto"
    Consolidar **todo** lo aprendido en los capítulos 1–14 en un proyecto de
    tamaño medio, con arquitectura limpia, pruebas, *build system* y cero
    errores de memoria. Elige **una** de las cinco opciones (o propón la tuya).

!!! tip "Este capítulo es práctico de principio a fin"
    No hay teoría nueva: aplicas la que ya tienes. El entregable es un repositorio
    publicable que sirva de pieza de portafolio (ver cap. 40).

---

## Reglas comunes a todas las opciones

- **Build**: `Makefile` o `CMakeLists.txt` que compile con
  `-std=c17 -Wall -Wextra -Wpedantic`.
- **Calidad**: limpio bajo Valgrind/ASan (memoria) y UBSan (UB).
- **Pruebas**: cobertura > 80 % con un marco mínimo (Unity, Check o uno propio).
- **Documentación**: Doxygen + `README.md` con arquitectura e instrucciones.
- **Git**: historia de commits coherente.

---

## 15.1 Opción A — Intérprete de un lenguaje minimalista

Construye un intérprete para un lenguaje tipo Lisp o una calculadora con
variables. Es el camino directo al cap. 19 (compiladores) y 31 (VMs).

**Componentes**: *lexer* (tokenización) → *parser* de descenso recursivo → AST
(*tagged union*, cap. 8) → evaluador *tree-walking* → REPL. Extensiones:
variables, funciones, *closures*.

```c
// Núcleo del evaluador tree-walking
typedef struct Expr {
    enum { NUM, VAR, OP, CALL } tipo;
    union { double num; char *var; struct { char op; struct Expr *a, *b; } bin; } u;
} Expr;

double evaluar(const Expr *e, Entorno *env);
```

---

## 15.2 Opción B — Servidor web asíncrono

Un servidor HTTP/1.1 que sirva archivos estáticos, escalable a miles de
conexiones. Aplica los caps. 9, 12 y 13.

**Componentes**: parser HTTP/1.1, *event loop* con `epoll`/`kqueue` (o *thread
pool*), servicio de estáticos con *MIME types*, *keep-alive*, *benchmarking* con
`wrk`/`ab`. Es la antesala directa del cap. 35.

---

## 15.3 Opción C — Almacén clave-valor (KV store)

Un Redis minimalista persistente. Aplica caps. 7, 11, 13.

**Componentes**: almacenamiento en archivo binario, índice **hash persistente**,
transacciones simples con **Write-Ahead Log** (WAL, cap. 27), protocolo de red
TCP y cliente CLI. Antesala de los caps. 27 y 32.

---

## 15.4 Opción D — Simulador de SO minimalista

Modela los conceptos de un sistema operativo en espacio de usuario.

**Componentes**: planificador *round-robin*, gestión de memoria con paginación
simulada, sistema de archivos tipo FAT y una capa de *syscalls* simuladas.
Antesala de los caps. 23 y 39.

---

## 15.5 Opción E — Transpilador C→C

Un compilador *source-to-source* para un subconjunto de C.

**Componentes**: análisis léxico y sintáctico manuales (descenso recursivo o
LL(1)), AST, y generación de C reescrito/optimizado. Antesala directa del cap. 19.

---

## 15.6 Entrega y evaluación

Rúbrica sugerida (100 puntos):

| Criterio | Puntos |
|----------|--------|
| Funcionalidad completa | 35 |
| Limpio bajo ASan/UBSan/Valgrind | 20 |
| Pruebas (>80 % cobertura) | 15 |
| Arquitectura y legibilidad | 15 |
| Documentación (README + Doxygen) | 10 |
| *Build system* y CI | 5 |

---

## Conexión con la actualidad

Cada una de las cinco opciones es un **arquetipo de software real** que sigue
escribiéndose en C en 2025: los intérpretes (Lua, CPython, mruby), los servidores
web (Nginx, lighttpd), los KV stores (Redis, Memcached, LMDB), los emuladores/
sandboxes y los compiladores (tcc, chibicc, cproc). Completar uno de extremo a
extremo — con CI, *fuzzing* y *sanitizers* — es exactamente el tipo de evidencia
que buscan los equipos de sistemas, *embedded* y seguridad al contratar. La
diferencia entre un proyecto de juguete y uno profesional no está en las líneas
de código, sino en la **disciplina de calidad**: pruebas, ausencia de UB y
documentación.

---

## Ejercicios / Hitos

!!! example "Hito 1 — Diseño ★★"
    Redacta un `DISEÑO.md` con la arquitectura, los módulos y sus interfaces
    (`.h`) antes de escribir lógica.

!!! example "Hito 2 — Núcleo funcional ★★★"
    Implementa el camino feliz mínimo end-to-end.

!!! example "Hito 3 — Robustez ★★★★"
    Maneja todos los errores, pasa ASan/UBSan/Valgrind y añade *fuzzing* de
    entradas.

!!! example "Hito 4 — Pulido ★★★"
    Pruebas >80 %, Doxygen, README y un *benchmark* o demo.

---

## Referencias

- *Crafting Interpreters* (Robert Nystrom) — para la opción A (gratuito online).
- *Build Your Own Redis / Database / Web Server* (serie *Build Your Own X*).
- Bob Nystrom, *Crafting Interpreters*; Thorsten Ball, *Writing an Interpreter*.
- Capítulos 7, 11, 12, 13 y 14 de este curso.
