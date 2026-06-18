---
tags:
  - Intermedio
  - Memoria
---

# Capítulo 7 — Gestión Dinámica de Memoria

!!! abstract "Objetivos de aprendizaje"
    - Distinguir las regiones de memoria de un proceso (pila, *heap*, datos).
    - Usar `malloc`, `calloc`, `realloc` y `free` correctamente.
    - Reconocer y evitar fugas, *use-after-free*, doble *free* y desbordamientos.
    - Diagnosticar con **Valgrind** y **AddressSanitizer**.

---

## 7.1 Memoria en C

Un proceso organiza su memoria en regiones:

```text
Direcciones altas
┌─────────────────┐
│   Pila (stack)  │  ← variables locales; crece hacia abajo
├─────────────────┤
│       ↓         │
│       ↑         │
│  Heap (montón)  │  ← malloc/free; crece hacia arriba
├─────────────────┤
│  BSS / Data     │  ← globales y estáticas
├─────────────────┤
│  Texto (código) │  ← instrucciones, solo lectura
└─────────────────┘
Direcciones bajas
```

- **Pila**: rápida, automática, tamaño limitado (típico 1–8 MB), liberada al
  salir de la función.
- **Heap**: grande, gestionada manualmente, vive hasta que llamas a `free`.

---

## 7.2 Funciones de asignación

```c
#include <stdlib.h>

int *a = malloc(n * sizeof *a);          // sin inicializar
int *b = calloc(n, sizeof *b);           // inicializado a 0
int *c = realloc(a, 2 * n * sizeof *a);  // redimensiona
free(b);                                  // libera
```

!!! tip "Idioma seguro de malloc"
    `T *p = malloc(n * sizeof *p);` usa `sizeof *p` (no `sizeof(T)`): si cambias
    el tipo de `p`, el tamaño se ajusta solo. **Comprueba siempre** el retorno:
    ```c
    int *p = malloc(n * sizeof *p);
    if (p == NULL) { /* gestionar fallo */ }
    ```

!!! warning "realloc seguro"
    Nunca escribas `p = realloc(p, ...)`: si falla, devuelve `NULL` y **pierdes**
    el puntero original (fuga). Usa un puntero temporal:
    ```c
    void *tmp = realloc(p, nuevo);
    if (!tmp) { /* p sigue válido */ } else { p = tmp; }
    ```

---

## 7.3 Errores comunes y buenas prácticas

| Error | Síntoma | Defensa |
|-------|---------|---------|
| Fuga (no liberar) | Crecimiento de memoria | Valgrind; un `free` por cada `malloc` |
| *Use-after-free* | Corrupción aleatoria | Pon `p = NULL` tras `free` |
| Doble `free` | Crash / explotable | `free(NULL)` es seguro; anula tras liberar |
| Desbordamiento de heap | Corrupción de metadatos | ASan; longitudes explícitas |
| Leer sin inicializar | Valores basura | `calloc` o inicializa |

Patrón de propiedad: que **cada bloque tenga un dueño claro** responsable de
liberarlo. Documenta en cada función si *transfiere* o *toma prestada* la
propiedad de un puntero.

---

## 7.4 Implementación de un asignador simple

Entender `malloc` por dentro desmitifica el *heap*. Un asignador de tipo *bump*
sobre un búfer estático:

```c title="arena.c — asignador de arena (bump allocator)"
#include <stddef.h>
#include <stdint.h>

static unsigned char arena[1 << 20];   // 1 MiB
static size_t cursor = 0;

void *arena_alloc(size_t n, size_t align) {
    size_t p = (cursor + (align - 1)) & ~(align - 1);  // alinea
    if (p + n > sizeof arena) return NULL;             // sin espacio
    cursor = p + n;
    return &arena[p];
}
void arena_reset(void) { cursor = 0; }   // libera todo de golpe
```

Las **arenas** (o *region allocators*) son hoy una técnica de moda: asignas
rápido y liberas todo de una vez, evitando fugas por construcción. Las usan
compiladores y servidores de alto rendimiento.

---

## 7.5 Valgrind y AddressSanitizer

```bash
# Valgrind: detecta fugas y accesos inválidos (sin recompilar)
valgrind --leak-check=full --show-leak-kinds=all ./programa

# ASan: instrumentación en compilación, mucho más rápido
gcc -std=c17 -g -fsanitize=address,undefined programa.c -o programa
./programa
```

ASan reporta el tipo exacto de error (*heap-buffer-overflow*, *use-after-free*…)
con la pila de la asignación y de la liberación. Es la herramienta de cabecera
del desarrollo en C moderno.

---

## 7.6 Gestión de memoria para estructuras de datos

Toda estructura dinámica (lista, árbol, hash — cap. 11) necesita una pareja
`crear`/`destruir`:

```c
typedef struct Nodo { int dato; struct Nodo *sig; } Nodo;

void lista_destruir(Nodo *cabeza) {
    while (cabeza) {
        Nodo *sig = cabeza->sig;   // guarda antes de liberar
        free(cabeza);
        cabeza = sig;
    }
}
```

---

## Conexión con la actualidad

La gestión manual de memoria es **el** campo de batalla de la seguridad
informática actual. Según Google y Microsoft, en torno al **70 % de las
vulnerabilidades críticas** son errores de seguridad de memoria, mayoritariamente
en el *heap*. Esto ha producido respuestas en todos los frentes:

- **Asignadores endurecidos**: *hardened_malloc* (GrapheneOS), `scudo` (Android),
  y las mitigaciones de glibc (*safe-linking*, comprobación de *tcache*).
- **Hardware**: ARM **MTE** etiqueta cada asignación; un *use-after-free* o un
  desbordamiento producen una excepción.
- **Cuarentena y *zeroing***: el kernel de Linux introdujo *init_on_free* para
  borrar memoria liberada y dificultar *exploits*.

Aprender a usar Valgrind y ASan, y a razonar sobre la **propiedad** de cada
bloque, es lo que separa un programa de C frágil de uno robusto.

---

## Ejercicios

!!! example "Ejercicio 7.1 — Vector dinámico ★★"
    Implementa un *dynamic array* (como `std::vector`) con `realloc`, que duplique
    su capacidad al llenarse. API: `push`, `get`, `len`, `free`.

!!! example "Ejercicio 7.2 — Caza la fuga ★★"
    Te damos un programa con tres fugas y un *use-after-free*. Encuéntralos con
    Valgrind y ASan, y corrígelos.

!!! example "Ejercicio 7.3 — Arena allocator ★★★"
    Extiende el asignador de arena de 7.4 para soportar varias arenas y úsalo en
    el parser de un mini-lenguaje (anticipo del cap. 19).

!!! example "Ejercicio 7.4 — strdup desde cero ★★"
    Implementa `mi_strdup` con `malloc` + `memcpy`. ¿Quién es responsable de
    liberar el resultado?

---

## Referencias

- ISO/IEC 9899:2018, §7.22.3 (gestión de memoria).
- Doug Lea. *A Memory Allocator* (dlmalloc), documento de referencia.
- [Valgrind User Manual](https://valgrind.org/docs/manual/).
- [AddressSanitizer (Clang docs)](https://clang.llvm.org/docs/AddressSanitizer.html).
