---
tags:
  - Avanzado
  - Compiladores
  - Intérpretes
---

# Capítulo 19 — Desarrollo de Compiladores e Intérpretes

!!! abstract "Objetivos de aprendizaje"
    - Conocer la arquitectura de un compilador: *front-end*, *middle-end*,
      *back-end*.
    - Implementar **lexer**, **parser** de descenso recursivo y **AST**.
    - Realizar análisis semántico, generar IR y código.
    - Construir intérpretes *tree-walking* y de *bytecode*, y entender el JIT.

---

## 19.1 Arquitectura de un compilador

```mermaid
graph LR
    SRC["Código fuente"] --> LEX["Lexer<br/>tokens"]
    LEX --> PAR["Parser<br/>AST"]
    PAR --> SEM["Análisis<br/>semántico"]
    SEM --> IR["IR"]
    IR --> OPT["Optimizador"]
    OPT --> GEN["Codegen"]
    GEN --> ASM["Código máquina"]
```

---

## 19.2 Análisis léxico (lexer)

Convierte texto en una secuencia de **tokens**:

```c
typedef enum { T_NUM, T_PLUS, T_STAR, T_LPAREN, T_RPAREN, T_EOF } TipoTok;
typedef struct { TipoTok tipo; double valor; } Token;

Token siguiente_token(const char **p) {
    while (**p == ' ') (*p)++;
    if (isdigit((unsigned char)**p)) {
        return (Token){T_NUM, strtod(*p, (char **)p)};
    }
    switch (*(*p)++) {
        case '+': return (Token){T_PLUS};
        case '*': return (Token){T_STAR};
        case '(': return (Token){T_LPAREN};
        case ')': return (Token){T_RPAREN};
        case '\0': return (Token){T_EOF};
    }
    /* error léxico */
}
```

---

## 19.3–19.4 Parser y AST

El **descenso recursivo** traduce directamente una gramática a funciones,
respetando precedencia. Para una calculadora:

```text
expr   → term (('+' | '-') term)*
term   → factor (('*' | '/') factor)*
factor → NUM | '(' expr ')'
```

El **AST** es una *tagged union* (cap. 8):

```c
typedef struct Nodo {
    enum { N_NUM, N_BINOP } tipo;
    union {
        double num;
        struct { char op; struct Nodo *izq, *der; } binop;
    } u;
} Nodo;
```

Técnicas alternativas: *Pratt parsing* (precedencia por *binding power*), tablas
LL(1)/LR, y generadores (yacc/bison, ANTLR).

---

## 19.5–19.6 Análisis semántico e IR

- **Semántico**: tabla de símbolos, comprobación de tipos, ámbitos, resolución de
  nombres.
- **IR** (representación intermedia): forma independiente de la máquina. La más
  influyente es **LLVM IR**; el estilo **SSA** (*Static Single Assignment*)
  facilita las optimizaciones.

---

## 19.7–19.9 Optimización, codegen y enlazado

- Optimizaciones: *constant folding*, *dead code elimination*, *inlining*,
  asignación de registros (coloreado de grafos).
- **Codegen**: emitir ensamblador/máquina para la ISA destino.
- **Linker/loader**: resolución de símbolos, reubicaciones, ELF/PE.

---

## 19.10–19.12 Intérpretes y JIT

- **Tree-walking**: recorre el AST evaluando. Simple, lento. (Tu cap. 15-A.)
- **Bytecode**: compila a instrucciones de una VM y ejecútalas en un bucle
  *dispatch* (cap. 31). Mucho más rápido (CPython, Lua, Ruby).
- **JIT**: compila *bytecode* a código máquina en tiempo de ejecución (cap. 31).

```c
// Evaluador tree-walking del AST anterior
double eval(const Nodo *n) {
    if (n->tipo == N_NUM) return n->u.num;
    double a = eval(n->u.binop.izq), b = eval(n->u.binop.der);
    switch (n->u.binop.op) {
        case '+': return a + b;  case '-': return a - b;
        case '*': return a * b;  case '/': return a / b;
    }
    return 0;
}
```

---

## Conexión con la actualidad

Los compiladores en C/C++ son la columna vertebral del software: **GCC**, **LLVM/
Clang**, **V8** (JavaScript), **CPython**, **LuaJIT** y los *runtimes* de WASM
(cap. 24) están escritos en C o C++. En 2024–2025, **LLVM** se ha consolidado
como la infraestructura común sobre la que se construyen casi todos los lenguajes
nuevos (Rust, Swift, Zig, Julia), y su IR es el «ensamblador portable» de la
industria. Hay un renacimiento de **compiladores de C minúsculos y auditables**
—*tcc*, *chibicc*, *cproc/QBE*— en el contexto del *bootstrapping* reproducible
(proyecto **GNU Mes**, que busca arrancar todo un sistema desde un binario
mínimo y verificable, una respuesta al ataque «*Trusting Trust*» de Ken
Thompson). Escribir un compilador, aunque sea de juguete, es uno de los
ejercicios que más profundiza tu comprensión de C y de las máquinas.

---

## Ejercicios

!!! example "Ejercicio 19.1 — Calculadora ★★"
    Lexer + parser de descenso recursivo + evaluador para expresiones aritméticas
    con precedencia y paréntesis.

!!! example "Ejercicio 19.2 — Variables y REPL ★★★"
    Añade variables (`x = 3 * 4`) y un bucle REPL con tabla de símbolos.

!!! example "Ejercicio 19.3 — Pratt parser ★★★★"
    Reescribe el parser con la técnica de Pratt y soporta operadores unarios y
    exponenciación asociativa por la derecha.

!!! example "Ejercicio 19.4 — A bytecode ★★★★"
    Compila el AST a *bytecode* de una VM de pila y ejecútalo (puente al cap. 31).

---

## Referencias

- Aho, Lam, Sethi, Ullman. *Compilers: Principles, Techniques, and Tools* (el «Dragon Book»).
- Robert Nystrom. *Crafting Interpreters* (gratuito online) — altamente recomendado.
- Rui Ueyama. *chibicc* — un compilador de C pedagógico en C.
- [LLVM Kaleidoscope Tutorial](https://llvm.org/docs/tutorial/).
