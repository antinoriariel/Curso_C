---
tags:
  - Intermedio
  - Arrays
  - Cadenas
---

# Capítulo 5 — Arrays y Cadenas de Caracteres

!!! abstract "Objetivos de aprendizaje"
    - Declarar y recorrer arrays uni y multidimensionales.
    - Comprender el **decaimiento** array → puntero y sus consecuencias.
    - Manejar cadenas C (`'\0'`-terminadas) y `<string.h>` **con seguridad**.
    - Implementar algoritmos clásicos: búsqueda y ordenación.

---

## 5.1 Arrays unidimensionales

```c
int v[5] = {10, 20, 30, 40, 50};
v[0];            // primer elemento
v[4];            // último (índice n-1)
size_t n = sizeof v / sizeof v[0];   // número de elementos = 5
```

!!! warning "Sin comprobación de límites"
    C **no verifica** los índices. `v[5]` o `v[-1]` son *undefined behavior* y la
    causa nº 1 de vulnerabilidades de corrupción de memoria. Usa
    `-fsanitize=address` durante el desarrollo.

### Relación array ↔ puntero

En casi todos los contextos, el nombre de un array **decae** a un puntero a su
primer elemento. Por eso `v[i]` es exactamente `*(v + i)`. La excepción notable:
`sizeof v` y `&v` operan sobre el array completo.

### VLA (C99)

```c
void f(size_t n) {
    int tmp[n];   // array de longitud variable, en la pila
}
```

Los VLA son **opcionales en C11/C17** y arriesgados (pueden agotar la pila con
`n` grande o atacante-controlado). Evítalos en código de producción; usa
`malloc` (cap. 7).

---

## 5.2 Arrays multidimensionales

```c
int m[3][4];                 // 3 filas, 4 columnas; almacenamiento row-major
int id[2][2] = {{1,0},{0,1}};
```

*Row-major* significa que las filas están contiguas en memoria:
`m[i][j]` está en `base + (i*4 + j)`. Al pasar a una función hay que conservar
todas las dimensiones salvo la primera:

```c
void imprimir(int filas, int cols, int mat[][cols]) { /* ... */ }
```

---

## 5.3 Cadenas de caracteres

Una cadena C es un **array de `char` terminado en el byte nulo `'\0'`**:

```c
char saludo[] = "Hola";   // {'H','o','l','a','\0'} → 5 bytes
```

`<string.h>` aporta las operaciones fundamentales:

| Función | Acción | Riesgo |
|---------|--------|--------|
| `strlen` | Longitud (sin contar `\0`) | UB si no está terminada |
| `strcpy` | Copia | **Desbordamiento** si destino pequeño |
| `strncpy` | Copia acotada | Puede **no** terminar en `\0` |
| `strcat` | Concatena | Desbordamiento |
| `strcmp` | Compara | — |
| `strchr`/`strstr` | Busca carácter/subcadena | — |
| `strtok` | Tokeniza | No reentrante (usa estado interno) |

```c
char destino[16];
snprintf(destino, sizeof destino, "%s!", saludo);  // forma SEGURA y preferida
```

!!! tip "Preferir snprintf"
    `snprintf` siempre termina en `'\0'` (salvo tamaño 0) y nunca escribe más allá
    del búfer. Es la herramienta por defecto para construir cadenas con seguridad.

`strdup`/`strndup` (POSIX, estándar en C23) duplican una cadena reservando
memoria con `malloc` — recuerda `free`.

---

## 5.4 Arrays de cadenas

```c
const char *dias[] = {"Lun", "Mar", "Mié"};   // array de punteros a cadena
// argv es exactamente esto: char *argv[] ≡ char **argv
```

Distingue una matriz `char m[3][4]` (memoria contigua, cadenas de tamaño fijo)
de un array de punteros `char *p[3]` (punteros a cadenas de tamaños distintos).

---

## 5.5 Algoritmos clásicos

```c title="Búsqueda binaria (array ordenado)"
int busqueda_binaria(const int *v, int n, int objetivo) {
    int lo = 0, hi = n - 1;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;   // evita desbordar (lo+hi)
        if (v[mid] == objetivo) return mid;
        if (v[mid] < objetivo) lo = mid + 1;
        else                   hi = mid - 1;
    }
    return -1;   // no encontrado
}
```

Ordenación por inserción (estable, O(n²), eficiente para arrays casi ordenados):

```c
void insercion(int *v, int n) {
    for (int i = 1; i < n; i++) {
        int clave = v[i], j = i - 1;
        while (j >= 0 && v[j] > clave) { v[j + 1] = v[j]; j--; }
        v[j + 1] = clave;
    }
}
```

---

## Conexión con la actualidad

Las cadenas terminadas en `'\0'` y la ausencia de límites en los arrays son el
**pecado original** de C en materia de seguridad. Funciones como `strcpy`,
`strcat` y `gets` (esta última **eliminada del estándar en C11**) están detrás de
décadas de *buffer overflows*. La respuesta de la industria en 2024–2025 es doble:

- **Detección**: AddressSanitizer y *fuzzing* (cap. 30) encuentran estos fallos
  antes de producción; el proyecto **OSS-Fuzz** de Google ha hallado decenas de
  miles de bugs así.
- **Mitigación hardware**: **ARM MTE** (Memory Tagging Extension) y la
  arquitectura experimental **CHERI** (adoptada en el prototipo *Morello* de
  ARM) detectan accesos fuera de límites a nivel de CPU, marcando punteros y
  regiones de memoria.

Las *Annex K* (`strcpy_s`, `strncpy_s`) buscaron estandarizar variantes seguras,
con adopción desigual. La recomendación práctica del curso: **trata el tamaño del
búfer como un dato de primera clase** y usa `snprintf`/`memcpy` con longitudes
explícitas.

---

## Ejercicios

!!! example "Ejercicio 5.1 — Reimplementa string.h ★★"
    Escribe tus propias `mi_strlen`, `mi_strcpy`, `mi_strcmp` y `mi_strcat`.
    Compáralas con las de la libc.

!!! example "Ejercicio 5.2 — Frecuencia de palabras ★★★"
    Lee un texto y cuenta cuántas veces aparece cada palabra. (Más adelante lo
    mejorarás con una tabla hash, cap. 11.)

!!! example "Ejercicio 5.3 — Multiplicación de matrices ★★★"
    Multiplica dos matrices NxN. Mide el tiempo y compáralo recorriéndolas en
    orden row-major vs column-major (anticipo del cap. 21, *cache locality*).

!!! example "Ejercicio 5.4 — Búsqueda binaria robusta ★★"
    Verifica la búsqueda binaria con casos límite: array vacío, un elemento,
    objetivo menor que el mínimo y mayor que el máximo.

---

## Referencias

- ISO/IEC 9899:2018, §6.7.6 (declaradores de array), §7.24 (`<string.h>`).
- *The Practice of Programming* (Kernighan & Pike), capítulo sobre cadenas.
- [OSS-Fuzz](https://github.com/google/oss-fuzz) — fuzzing continuo de software C/C++.
- CHERI / ARM Morello — documentación del proyecto.
