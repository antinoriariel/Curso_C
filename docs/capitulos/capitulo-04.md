---
tags:
  - Fundamentos
  - Funciones
---

# Capítulo 4 — Funciones y Ámbito de Variables

!!! abstract "Objetivos de aprendizaje"
    - Declarar y definir funciones con **prototipos** correctos.
    - Entender el **paso por valor** de C y simular paso por referencia con
      punteros.
    - Manejar las **clases de almacenamiento** (`static`, `extern`) y la
      recursividad.
    - Usar **punteros a función** para *callbacks* y tablas de despacho.
    - Escribir funciones **variádicas** con `<stdarg.h>`.

---

## 4.1 Definición y declaración

```c
double area_circulo(double r);              // prototipo (declaración)

double area_circulo(double r) {             // definición
    return 3.14159265358979 * r * r;
}
```

!!! warning "Funciones sin prototipo"
    El estilo K&R antiguo `int f();` (lista de parámetros vacía con significado
    «desconocido») es peligroso y, en **C23**, `()` pasa por fin a significar
    `(void)`. Declara siempre `(void)` para «sin parámetros».

---

## 4.2 Paso de parámetros

C pasa **siempre por valor**: la función recibe una *copia*. Para que una función
modifique una variable del llamador, se pasa su **dirección**:

```c
void incrementar(int *p) { (*p)++; }   // recibe la dirección

int n = 5;
incrementar(&n);   // n vale ahora 6
```

Este «paso por referencia simulado» con punteros es la base de funciones como
`scanf` (`&variable`) y del intercambio:

```c
void swap(int *a, int *b) { int t = *a; *a = *b; *b = t; }
```

La palabra clave `inline` (C99) sugiere al compilador insertar el cuerpo en el
punto de llamada; es una *sugerencia*, no una orden.

---

## 4.3 Ámbito y duración

| Clase | Palabra clave | Duración | Visibilidad |
|-------|---------------|----------|-------------|
| Local automática | (ninguna) | bloque | bloque |
| Local estática | `static` | todo el programa | bloque |
| Global | (ninguna, fuera de funciones) | todo el programa | unidad de traducción / `extern` |
| Global con enlace interno | `static` (en fichero) | programa | solo ese `.c` |

```c
int contador(void) {
    static int n = 0;   // persiste entre llamadas; se inicializa una vez
    return ++n;         // 1, 2, 3, ...
}
```

`extern` **declara** una variable o función definida en otra unidad de
traducción. `register` es un vestigio histórico ignorado por los compiladores
modernos.

---

## 4.4 Recursividad

Una función recursiva se llama a sí misma. Necesita un **caso base** que detenga
la recursión:

```c
unsigned long factorial(unsigned n) {
    if (n <= 1) return 1;           // caso base
    return n * factorial(n - 1);    // caso recursivo
}
```

!!! warning "Profundidad y pila"
    Cada llamada consume espacio de pila. Una recursión sin caso base o
    demasiado profunda provoca **stack overflow**. Algunos compiladores aplican
    *tail-call optimization* (TCO) cuando la llamada recursiva es lo último que
    se hace, convirtiéndola en un bucle — pero C **no lo garantiza**.

Ejemplos clásicos: factorial, Fibonacci, Torres de Hanói, QuickSort.

---

## 4.5 Punteros a función

Una función tiene una dirección, y podemos guardarla en una variable:

```c
int suma(int a, int b)  { return a + b; }
int resta(int a, int b) { return a - b; }

int (*op)(int, int) = suma;   // op apunta a suma
printf("%d\n", op(3, 4));     // 7
op = resta;
```

### Callbacks: `qsort`

```c
#include <stdlib.h>
int cmp_int(const void *a, const void *b) {
    int x = *(const int *)a, y = *(const int *)b;
    return (x > y) - (x < y);   // evita el bug del desbordamiento de x - y
}
qsort(v, n, sizeof v[0], cmp_int);
```

### Tablas de despacho

Sustituyen un `switch` por una tabla de punteros — útil en intérpretes (cap. 19,
31):

```c
int (*tabla[])(int, int) = { suma, resta };
int r = tabla[codigo](a, b);
```

---

## 4.6 Funciones variádicas

Reciben un número variable de argumentos vía `<stdarg.h>`:

```c title="suma_variadica.c"
#include <stdarg.h>

int suma_todos(int cuenta, ...) {
    va_list ap;
    va_start(ap, cuenta);
    int total = 0;
    for (int i = 0; i < cuenta; i++)
        total += va_arg(ap, int);
    va_end(ap);
    return total;
}
// suma_todos(3, 10, 20, 30) == 60
```

!!! warning "Sin red de seguridad"
    Las funciones variádicas **no conocen** el número ni el tipo real de sus
    argumentos: dependen de una convención (un contador, un centinela o una
    cadena de formato). Pasar el tipo equivocado a `va_arg` es *undefined
    behavior*. Es la raíz de las vulnerabilidades de *format string*.

---

## Conexión con la actualidad

Los **punteros a función** son el mecanismo que hace posibles los *callbacks*
asíncronos de `libuv` (el corazón de Node.js), los manejadores de eventos de
`epoll`/`io_uring` (cap. 39) y las tablas de operaciones (`struct file_operations`)
de los drivers de Linux (cap. 23). Su contrapartida de seguridad es que un
puntero a función corrompido es un objetivo predilecto de los exploits; por eso
los compiladores modernos ofrecen **Control-Flow Integrity (CFI)** (`-fsanitize=cfi`
en Clang), que verifica en tiempo de ejecución que cada llamada indirecta apunta
a una función del tipo esperado. Las funciones variádicas, por su parte, motivan
el aviso `-Wformat-security`, hoy obligatorio en las *hardening guidelines* de
distribuciones como Fedora y Ubuntu.

---

## Ejercicios

!!! example "Ejercicio 4.1 — Potencia ★"
    Implementa `potencia(base, exp)` en versión iterativa y recursiva. Compara.

!!! example "Ejercicio 4.2 — qsort propio ★★"
    Ordena un array de `struct persona` por edad y luego por nombre usando
    `qsort` con dos comparadores.

!!! example "Ejercicio 4.3 — Calculadora con tabla ★★★"
    Construye una calculadora que mapee los caracteres `+ - * /` a funciones vía
    una tabla de punteros a función.

!!! example "Ejercicio 4.4 — mini-printf ★★★★"
    Escribe `mi_printf(const char *fmt, ...)` que soporte `%d`, `%s` y `%c` usando
    `<stdarg.h>` y `putchar`. *Proyecto acumulativo*: lo ampliarás en el cap. 9.

---

## Referencias

- ISO/IEC 9899:2018, §6.9 (definiciones externas), §7.16 (`<stdarg.h>`).
- *21st Century C* (Ben Klemens), capítulos sobre punteros a función.
- [Clang CFI documentation](https://clang.llvm.org/docs/ControlFlowIntegrity.html).
