---
tags:
  - Fundamentos
  - Tipos
  - C17
---

# Capítulo 2 — Tipos de Datos, Variables y Operadores

!!! abstract "Objetivos de aprendizaje"
    - Conocer los **tipos fundamentales** y sus modificadores.
    - Usar los **tipos de ancho fijo** de `<stdint.h>` y sus macros de formato.
    - Dominar el **álgebra de operadores**: precedencia, asociatividad y bits.
    - Predecir las **conversiones de tipo** (promoción entera, conversiones
      aritméticas usuales) y evitar pérdidas de precisión.

---

## 2.1 Tipos de datos fundamentales

C ofrece un pequeño conjunto de tipos primitivos cuyos tamaños exactos **dependen
de la implementación** (salvo garantías mínimas):

| Tipo | Significado | Tamaño mínimo garantizado |
|------|-------------|---------------------------|
| `char` | Carácter / byte | 1 byte (≥ 8 bits) |
| `short` | Entero corto | 16 bits |
| `int` | Entero «natural» de la máquina | 16 bits (típicamente 32) |
| `long` | Entero largo | 32 bits |
| `long long` | Entero muy largo (C99) | 64 bits |
| `float` | Real de simple precisión | ~7 dígitos (IEEE-754 32 bits) |
| `double` | Real de doble precisión | ~15 dígitos (IEEE-754 64 bits) |
| `void` | «Sin tipo» / ausencia de valor | — |

Modificadores: `signed` / `unsigned` (signo) y `short` / `long` (tamaño).

```c
unsigned int edad = 30;
long long poblacion = 8000000000LL;
const double PI = 3.14159265358979;
```

### Booleanos

```c
#include <stdbool.h>   // C99: define bool, true, false
bool encendido = true;
```

!!! note "C23"
    En **C23**, `bool`, `true` y `false` son palabras clave del lenguaje; ya no
    hace falta incluir `<stdbool.h>`.

### Tipos de `<stddef.h>`

- `size_t`: entero sin signo para **tamaños** y resultados de `sizeof`.
- `ptrdiff_t`: entero con signo para **diferencias de punteros**.
- `NULL`: constante de puntero nulo.

---

## 2.2 Especificadores de precisión fija (C99+)

Cuando el tamaño exacto importa (protocolos, formatos binarios, embebidos), usa
`<stdint.h>`:

```c
#include <stdint.h>
int8_t   a = -128;       // exactamente 8 bits con signo
uint16_t b = 65535;      // exactamente 16 bits sin signo
int32_t  c = 2000000000;
uint64_t d = 18446744073709551615ULL;
```

Para imprimirlos de forma portable, `<inttypes.h>` define macros:

```c
#include <inttypes.h>
printf("d = %" PRIu64 "\n", d);   // PRIu64 → "llu" o "lu" según la plataforma
```

Rangos y límites: `<limits.h>` (`INT_MAX`, `UCHAR_MAX`…) y `<float.h>`
(`DBL_EPSILON`, `FLT_MAX`…).

---

## 2.3 Variables

```c
int x;          // declaración (valor indeterminado: NO la leas aún)
int y = 10;     // declaración + inicialización
x = y + 5;      // asignación
```

!!! warning "Variables no inicializadas"
    Leer una variable automática no inicializada es **undefined behavior**.
    Inicializa siempre, o deja claro por construcción que se escribe antes de
    leer.

### `const` y `volatile`

- `const`: el valor no debe modificarse a través de ese nombre (contrato de solo
  lectura, ayuda al compilador y al lector).
- `volatile`: prohíbe al compilador optimizar/cachear los accesos; imprescindible
  para registros mapeados en memoria (MMIO) en sistemas embebidos y para
  variables modificadas por manejadores de señal.

```c
const int MAX = 100;
volatile uint32_t *const GPIO = (uint32_t *)0x40020000;  // registro hardware
```

### Ámbito de bloque

Toda `{ ... }` abre un ámbito. Una variable existe desde su declaración hasta el
cierre del bloque.

---

## 2.4 Operadores

### Aritméticos, relacionales y lógicos

```c
int s = a + b, r = a % b;        // aritméticos
bool mayor = (a > b);            // relacional → bool
bool ambos = (a > 0) && (b > 0); // lógico con cortocircuito
```

!!! tip "Cortocircuito"
    `&&` y `||` evalúan de izquierda a derecha y **se detienen en cuanto el
    resultado está decidido**. Esto permite el idiomático
    `if (p != NULL && p->valor > 0)`.

### Operadores a nivel de bits

| Operador | Significado | Ejemplo (`a=0b1100`, `b=0b1010`) |
|----------|-------------|----------------------------------|
| `&` | AND | `a & b == 0b1000` |
| `\|` | OR | `a \| b == 0b1110` |
| `^` | XOR | `a ^ b == 0b0110` |
| `~` | NOT (complemento) | `~a` |
| `<<` | Desplazamiento izq. | `a << 1 == 0b11000` |
| `>>` | Desplazamiento der. | `a >> 1 == 0b0110` |

Patrones idiomáticos:

```c
x |=  (1u << n);   // poner a 1 el bit n
x &= ~(1u << n);   // poner a 0 el bit n
x ^=  (1u << n);   // invertir el bit n
bool on = (x >> n) & 1u;  // leer el bit n
```

### `sizeof`, ternario, incremento

```c
size_t t = sizeof(int);        // operador en tiempo de compilación
int max = (a > b) ? a : b;     // ternario
int post = i++;                // usa i, luego incrementa
int pre  = ++j;                // incrementa, luego usa j
```

### Precedencia y asociatividad

La precedencia decide quién «agarra» antes; la asociatividad rompe empates entre
operadores de igual precedencia. La tabla completa está en el
[Apéndice A](../apendices/apendice-a.md).

!!! warning "Ante la duda, paréntesis"
    `a & b == c` se interpreta como `a & (b == c)` porque `==` tiene **más**
    precedencia que `&`. Casi nunca es lo que quieres: escribe `(a & b) == c`.

---

## 2.5 Conversiones de tipo

### Promoción entera y conversiones aritméticas usuales

En una operación aritmética, los tipos «pequeños» (`char`, `short`) se
**promocionan** a `int`, y luego ambos operandos se llevan a un tipo común. Esto
provoca sorpresas clásicas:

```c
unsigned int u = 1;
int i = -1;
if (i < u) puts("menor"); else puts("¡mayor!");  // imprime "¡mayor!"
```

`i` se convierte a `unsigned`, y `-1` pasa a ser `UINT_MAX`. Este es uno de los
bugs más frecuentes y peligrosos de C.

### Conversiones explícitas (cast)

```c
double media = (double)suma / n;   // fuerza división en punto flotante
```

!!! warning "Desbordamiento con signo = UB"
    El desbordamiento de enteros **con signo** es *undefined behavior*. El de los
    **sin signo** está definido (aritmética módulo 2ⁿ). Detéctalo con
    `-fsanitize=undefined`.

---

## Conexión con la actualidad

Los errores de conversión y desbordamiento de enteros siguen siendo una fuente
viva de **CVEs**: desde desbordamientos en cálculos de tamaño que preceden a un
*heap overflow*, hasta confusiones signed/unsigned en *parsers* de red. Por eso
C23 introdujo `<stdckdint.h>` con **aritmética con comprobación de
desbordamiento** (`ckd_add`, `ckd_mul`…), y el estándar **fijó por fin el
complemento a dos** como única representación de enteros con signo (antes
permitía tres representaciones). Las directrices **CERT-C** (capítulo 30)
dedican una sección entera (INT) a estos peligros.

```c
#include <stdckdint.h>   // C23
size_t total;
if (ckd_mul(&total, n, size)) { /* desbordamiento: aborta */ }
```

---

## Ejercicios

!!! example "Ejercicio 2.1 — Geometría ★"
    Lee el radio y calcula área y perímetro de un círculo usando `const double
    PI`. Cuida la conversión entera/flotante.

!!! example "Ejercicio 2.2 — XOR swap ★★"
    Intercambia dos enteros **sin** variable auxiliar usando `^=`. Explica por
    qué falla si las dos variables son el mismo objeto.

!!! example "Ejercicio 2.3 — ¿Potencia de dos? ★★"
    Determina si `n` es potencia de 2 con una sola expresión de bits.

    ??? note "Pista"
        `n > 0 && (n & (n - 1)) == 0`.

!!! example "Ejercicio 2.4 — Rangos ★★"
    Imprime `INT_MAX`, `UINT_MAX`, `LLONG_MIN`… usando `<limits.h>` y las macros
    de `<inttypes.h>`. Comprueba `sizeof` de cada tipo en tu máquina.

!!! example "Ejercicio 2.5 — La trampa unsigned ★★★"
    Reproduce el bug `i < u` de la sección 2.5, luego corrígelo de dos formas
    distintas (cast explícito y rediseño de tipos). Compila con `-Wconversion`.

---

## Referencias

- ISO/IEC 9899:2018, §6.2.5 (tipos), §6.3 (conversiones), §6.5 (operadores).
- *CERT C Coding Standard*, secciones INT y FLP. SEI/CMU.
- [cppreference: Arithmetic operators](https://en.cppreference.com/w/c/language/operator_arithmetic).
- [cppreference: stdckdint.h (C23)](https://en.cppreference.com/w/c/numeric).
