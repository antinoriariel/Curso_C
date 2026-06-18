# Apéndice D — Diferencias entre C17 y C23

**C23** (ISO/IEC 9899:2024) es la primera revisión con características nuevas
desde C11. Resumen de lo más relevante para el día a día.

## Lenguaje

| Característica | C17 | C23 |
|----------------|-----|-----|
| `bool`, `true`, `false` | Vía `<stdbool.h>` | **Palabras clave** nativas |
| Puntero nulo | `NULL` | `nullptr` (tipo `nullptr_t`) |
| Constantes en compilación | `#define` / `enum` | `constexpr` |
| Atributos | `__attribute__` (no estándar) | `[[nodiscard]]`, `[[maybe_unused]]`, `[[deprecated]]`, `[[fallthrough]]`, `[[noreturn]]` |
| Deducción de tipo | — | `typeof`, `typeof_unqual`, `auto` (con valor) |
| Enteros de ancho arbitrario | — | `_BitInt(N)` |
| Tipo subyacente de `enum` | — | `enum E : tipo { ... }` |
| Funciones `f()` | parámetros desconocidos | equivale a `f(void)` |
| Literales binarios | extensión `0b` | `0b1010` **estándar** |
| Separador de dígitos | — | `1'000'000` |
| `static_assert` | `_Static_assert` | `static_assert` (palabra clave) |
| `#elifdef` / `#elifndef` | — | Sí |
| `#embed` | — | Incrustar archivos binarios |

## Biblioteca

| Cabecera / función | Aporte de C23 |
|--------------------|----------------|
| `<stdckdint.h>` | `ckd_add`, `ckd_sub`, `ckd_mul` (aritmética con comprobación de desbordamiento) |
| `<stdbit.h>` | Operaciones de bits portables (`stdc_count_ones`, `stdc_bit_width`…) |
| `memset_explicit` | Borrado que el compilador no puede optimizar (seguridad) |
| `strdup`, `strndup` | Ahora **estándar** (antes solo POSIX) |
| `%b` en `printf` | Formato binario |

## Garantías del modelo

- Los enteros con signo usan **complemento a dos** (única representación; antes el
  estándar permitía tres).
- Mejor especificación de la *provenance* de punteros (vía TS).

## Cómo activarlo

```bash
gcc -std=c23 programa.c     # GCC 14+ ; usa -std=c2x en versiones algo previas
clang -std=c23 programa.c   # Clang 18+
```

```c
#if __STDC_VERSION__ >= 202311L
    // código específico de C23
#endif
```

Más contexto en el [Capítulo 14](../capitulos/capitulo-14.md).
