---
tags:
  - Maestría
  - Máquinas virtuales
  - JIT
---

# Capítulo 31 — Máquinas Virtuales y Compilación JIT

!!! abstract "Objetivos de aprendizaje"
    - Diseñar una **VM de pila** y una **VM de registros** con su *bytecode*.
    - Implementar *garbage collection* básico.
    - Comprender la **compilación JIT** (incluido el *tracing JIT*).

---

## 31.1 Fundamentos de máquinas virtuales

Una VM ejecuta *bytecode* portable mediante un bucle de despacho. Es el modelo de
CPython, la JVM, Lua y .NET. Más rápido que un intérprete *tree-walking*
(cap. 19), más portable que el código nativo.

---

## 31.2 VM de pila (*stack-based*)

Los operandos viven en una pila; las instrucciones la manipulan. *Bytecode*
compacto, fácil de generar.

```c
typedef enum { OP_CONST, OP_ADD, OP_MUL, OP_PRINT, OP_HALT } OpCode;

int vm_run(const uint8_t *code, const double *consts) {
    double pila[256]; int sp = 0; const uint8_t *ip = code;
    for (;;) {
        switch (*ip++) {
            case OP_CONST: pila[sp++] = consts[*ip++]; break;
            case OP_ADD:   pila[sp-2] += pila[sp-1]; sp--; break;
            case OP_MUL:   pila[sp-2] *= pila[sp-1]; sp--; break;
            case OP_PRINT: printf("%g\n", pila[sp-1]); break;
            case OP_HALT:  return 0;
        }
    }
}
```

Optimización del bucle: **computed goto** (`&&label`, extensión GNU) en lugar de
`switch` acelera el despacho notablemente (*threaded code*).

---

## 31.3 VM de registros (*register-based*)

Los operandos se referencian por «registros» (índices de un array). *Bytecode*
más grande pero menos instrucciones por operación → suele ser más rápida. Lua 5.x
es el ejemplo canónico.

---

## 31.4 Garbage collection

- **Mark-and-sweep**: marca lo alcanzable desde las raíces, libera el resto.
- **Reference counting**: simple pero no maneja ciclos (CPython combina ambos).
- **Generacional / copia**: optimiza por la hipótesis de que «la mayoría de los
  objetos muere joven».

---

## 31.5–31.7 Compilación JIT

Un **JIT** compila *bytecode* (o *traces*) a código máquina **en tiempo de
ejecución**, combinando portabilidad y velocidad nativa. El **tracing JIT**
(LuaJIT) compila las rutas calientes detectadas dinámicamente. Implementarlo
requiere emitir bytes de máquina en memoria ejecutable:

```c
// Memoria ejecutable para código generado (mmap + mprotect)
void *mem = mmap(NULL, 4096, PROT_READ|PROT_WRITE,
                 MAP_PRIVATE|MAP_ANONYMOUS, -1, 0);
// ... escribir opcodes x86-64 ...
mprotect(mem, 4096, PROT_READ|PROT_EXEC);   // W^X: nunca escribible y ejecutable a la vez
int (*fn)(void) = mem;
fn();
```

---

## 31.8–31.9 Debugging de JIT y VMs notables

Depurar código generado (registro con GDB JIT interface). VMs influyentes en C:
**LuaJIT**, **CPython**, **QuickJS** (Fabrice Bellard), **Wasm3**, la **JVM**
(HotSpot, C++).

---

## Conexión con la actualidad

Las máquinas virtuales y los JIT son tecnología de altísimo impacto, casi siempre
en C/C++: **V8** (el JIT de JavaScript que mueve Chrome y Node.js), **LuaJIT**
(referencia de rendimiento), **CPython** —que en 2024–2025 incorpora un
**intérprete especializado adaptativo** (PEP 659) y un JIT experimental basado en
*copy-and-patch*—, y los *runtimes* de **WebAssembly** (Wasmtime con Cranelift,
Wasm3). El *copy-and-patch compilation* es una técnica reciente que genera JITs
rápidos de escribir y de buen rendimiento. Entender cómo una VM despacha
*bytecode* y cómo un JIT emite código máquina te da una visión profunda de por
qué los lenguajes dinámicos rinden como rinden, y conecta directamente con los
caps. 19 (compiladores) y 24 (WASM).

---

## Ejercicios

!!! example "Ejercicio 31.1 — VM de pila ★★★"
    Implementa una VM de pila con aritmética, comparaciones y saltos
    condicionales; compila la calculadora del cap. 19 a su *bytecode*.

!!! example "Ejercicio 31.2 — Computed goto ★★★"
    Reescribe el bucle de despacho con *computed goto* y mide la mejora frente al
    `switch`.

!!! example "Ejercicio 31.3 — GC mark-and-sweep ★★★★"
    Añade objetos *heap* (pares/strings) a tu VM y un recolector mark-and-sweep.

!!! example "Ejercicio 31.4 — JIT mínimo ★★★★★"
    Genera y ejecuta una función que sume dos enteros emitiendo opcodes x86-64 en
    memoria ejecutable (con `mmap`+`mprotect`).

---

## Referencias

- Robert Nystrom. *Crafting Interpreters*, parte II (VM de bytecode en C) — gratuito.
- Mike Pall. Artículos sobre el diseño de **LuaJIT**.
- *Virtual Machines* (Smith & Nair).
- Xu et al. *Copy-and-Patch Compilation* (OOPSLA 2021).
