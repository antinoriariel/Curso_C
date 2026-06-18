---
tags:
  - Experto
  - Bajo nivel
  - UB
  - C23
---

# Capítulo 14 — Temas Avanzados y Bajo Nivel

!!! abstract "Objetivos de aprendizaje"
    - Comprender el **modelo de memoria** y los tres tipos de comportamiento:
      definido, definido por la implementación e **indefinido (UB)**.
    - Dominar *strict aliasing*, atributos del compilador y ASM embebido.
    - Conocer las técnicas de **optimización**, la interoperabilidad con otros
      lenguajes y las novedades de **C23**.

---

## 14.1–14.3 Modelo de memoria y comportamientos

El estándar clasifica el comportamiento de un programa en tres categorías que
debes interiorizar:

| Categoría | Significado | Ejemplo |
|-----------|-------------|---------|
| **Definido** | El estándar garantiza el resultado | `2 + 2 == 4` |
| **Definido por la implementación** | Cada compilador documenta su elección | tamaño de `int`, `>>` con signo |
| **No especificado** | El compilador elige, sin documentar | orden de evaluación de argumentos |
| **Indefinido (UB)** | *Todo* puede pasar | desbordamiento con signo, desreferenciar `NULL`, *data race* |

!!! danger "El UB no es un error que «a veces funciona»"
    Ante UB, el compilador asume que **nunca ocurre** y optimiza en consecuencia.
    Esto produce los famosos «*the compiler deleted my security check*»: un
    chequeo de desbordamiento eliminado porque el desbordamiento era UB. La
    defensa: `-fsanitize=undefined`, `-Wall -Wextra`, y conocer la lista de UB.

---

## 14.4 Strict aliasing

El compilador asume que punteros de **tipos incompatibles** no apuntan al mismo
objeto, y optimiza sobre esa base. Violarlo es UB:

```c
float f = 1.0f;
int i = *(int *)&f;        // ❌ viola strict aliasing (UB)
int j; memcpy(&j, &f, sizeof j);   // ✔ forma correcta de "type punning"
```

`char *` y `unsigned char *` pueden aliasar cualquier cosa (excepción del
estándar). Compila con `-fstrict-aliasing -Wstrict-aliasing` para auditarlo.

---

## 14.5–14.6 Atributos y ASM embebido

```c
[[noreturn]] void fatal(void);          // C23: la función no retorna
__attribute__((always_inline)) static inline int f(void);   // GNU
__attribute__((aligned(64))) int v[16];

// ASM embebido (GNU), para intrínsecas de bajo nivel:
uint64_t rdtsc(void) {
    uint32_t lo, hi;
    __asm__ volatile ("rdtsc" : "=a"(lo), "=d"(hi));
    return ((uint64_t)hi << 32) | lo;
}
```

---

## 14.7 Optimización de código

- Niveles `-O2`/`-O3`/`-Os`; **LTO** (*link-time optimization*, `-flto`).
- **Profile-Guided Optimization** (PGO): compila, perfila con datos reales,
  recompila.
- `restrict` (C99): promete que un puntero es el único acceso a su objeto,
  habilitando optimizaciones potentes.
- Branch hints: `__builtin_expect` / `[[likely]]` (C23).

!!! tip "Mide, no adivines"
    «*Premature optimization is the root of all evil*» (Knuth). Perfila (cap. 29)
    antes de optimizar; el compilador moderno suele ser más listo que tú en lo
    micro.

---

## 14.8–14.9 Programación de sistemas e interoperabilidad

- **Llamadas al sistema** directas (`syscall`), memoria mapeada (`mmap`).
- **FFI**: C es el *lingua franca* de la interoperabilidad. Python (ctypes,
  CPython API), Rust (`extern "C"`), Go (cgo), Java (JNI/Panama) llaman a C. La
  clave es la **ABI** estable y el `extern "C"`.

---

## 14.10 C23: novedades del estándar

| Característica | Descripción |
|----------------|-------------|
| `true`/`false`/`bool` | Palabras clave nativas |
| `nullptr` | Constante de puntero nulo con tipo propio |
| `constexpr` | Constantes evaluadas en compilación |
| `typeof`/`typeof_unqual` | Deducción de tipo |
| `[[attributes]]` | Sintaxis estándar de atributos |
| `_BitInt(N)` | Enteros de anchura arbitraria |
| `#embed` | Incrustar archivos binarios |
| `<stdckdint.h>` | Aritmética con comprobación de desbordamiento |
| Funciones `()` = `(void)` | Fin de los prototipos vacíos K&R |

Detalle completo en el [Apéndice D](../apendices/apendice-d.md).

---

## 14.11–14.12 Seguridad, pruebas y documentación

- **Hardening**: `-D_FORTIFY_SOURCE=2`, `-fstack-protector-strong`, ASLR, PIE.
- **Pruebas**: marcos como Unity, Check, criterion; *fuzzing* (cap. 30).
- **Documentación**: Doxygen genera referencia a partir de comentarios `/** */`.

---

## Conexión con la actualidad

El *undefined behavior* es el tema de bajo nivel más relevante de la C moderna:
es la raíz tanto de optimizaciones agresivas como de vulnerabilidades. La
comunidad debate activamente «domesticar» el UB — proyectos como **«Friendly
C»**, los modos `-fwrapv`/`-ftrapv`, y la propuesta de un **C *provenance model***
para punteros (incorporada en parte a C23 mediante un *Technical Specification*).
La **ABI** estable de C es lo que hace posible el ecosistema políglota de 2025:
el FFI de **Rust**, el nuevo **Project Panama** de Java y el **WASM Component
Model** se apoyan, en última instancia, en la interfaz de C. Y C23, con
`constexpr`, `nullptr` y `#embed`, demuestra que el comité ISO sigue modernizando
el lenguaje sin romper su modelo de bajo nivel.

---

## Ejercicios

!!! example "Ejercicio 14.1 — Caza el UB ★★★"
    Te damos 5 fragmentos con UB sutil (desbordamiento con signo, *aliasing*,
    lectura sin inicializar, índice fuera de rango, orden de evaluación).
    Identifícalos y confírmalos con UBSan.

!!! example "Ejercicio 14.2 — type punning correcto ★★"
    Convierte un `float` a sus bits con `memcpy` y con `union`. Razona por qué el
    cast de puntero es UB.

!!! example "Ejercicio 14.3 — restrict ★★★"
    Escribe `void saxpy(int n, float a, float *restrict x, float *restrict y)` y
    compara el ensamblador generado con y sin `restrict`.

!!! example "Ejercicio 14.4 — Moderniza a C23 ★★★"
    Toma un módulo de capítulos previos y moderniza `NULL`→`nullptr`,
    `#define` constantes → `constexpr`, y añade `[[nodiscard]]` donde proceda.

---

## Referencias

- ISO/IEC 9899:2024 (C23), borrador **N3220**.
- *What Every C Programmer Should Know About Undefined Behavior* (LLVM Project blog, 3 partes).
- Chris Lattner et al., serie sobre UB y optimización.
- *Computer Systems: A Programmer's Perspective* (Bryant & O'Hallaron).
