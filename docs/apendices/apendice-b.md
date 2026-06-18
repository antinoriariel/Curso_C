# Apéndice B — Especificadores de Formato `printf` / `scanf`

## Conversiones de `printf`

| Especificador | Tipo | Ejemplo de salida |
|---------------|------|-------------------|
| `%d` / `%i` | `int` con signo | `-42` |
| `%u` | `unsigned int` | `42` |
| `%ld` `%lld` | `long` / `long long` | `9000000000` |
| `%zu` | `size_t` | `1024` |
| `%f` | `double` (decimal) | `3.141593` |
| `%e` / `%g` | Notación científica / la más corta | `3.14e+00` / `3.14159` |
| `%x` / `%X` | Hexadecimal min/may | `ff` / `FF` |
| `%o` | Octal | `17` |
| `%c` | Carácter | `A` |
| `%s` | Cadena (`char *`) | `hola` |
| `%p` | Puntero | `0x7ffe...` |
| `%%` | Literal `%` | `%` |

### Banderas, anchura y precisión

```text
%[banderas][anchura][.precisión][longitud]conversión
```

- **Banderas**: `-` (izquierda), `0` (rellenar con ceros), `+` (signo), ` `
  (espacio), `#` (forma alternativa).
- **Anchura**: mínimo de caracteres (`%5d` → `   42`).
- **Precisión**: decimales en `%f`, máximo de caracteres en `%s` (`%.3f`, `%.10s`).
- `*` toma el valor como argumento: `printf("%*d", ancho, x)`.

```c
printf("|%-10s|%08.2f|%+d|\n", "hola", 3.14159, 5);
// |hola      |00003.14|+5|
```

## Conversiones de `scanf`

| Especificador | Lee en |
|---------------|--------|
| `%d` | `int *` |
| `%u` `%ld` `%lf` | `unsigned*`, `long*`, `double*` |
| `%f` | `float *` (¡distinto de `printf`!) |
| `%s` | cadena (¡usa anchura: `%63s`!) |
| `%c` | un carácter |
| `%[...]` | conjunto de caracteres (*scanset*) |

!!! warning "Seguridad y tipos correctos"
    - En `scanf`, los `double` usan `%lf`; en `printf`, `%f` (un `float` se
      promociona a `double`).
    - `%s` sin anchura desborda el búfer. Usa siempre `%63s` para `char buf[64]`.
    - Para enteros de ancho fijo usa `<inttypes.h>`: `printf("%" PRId64, x)`,
      `scanf("%" SCNd64, &x)`.

Detalles en el [Capítulo 9](../capitulos/capitulo-09.md).
