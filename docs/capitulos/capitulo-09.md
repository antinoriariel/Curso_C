---
tags:
  - Intermedio
  - E/S
  - Archivos
---

# Capítulo 9 — Entrada/Salida y Manejo de Archivos

!!! abstract "Objetivos de aprendizaje"
    - Usar `<stdio.h>` para E/S formateada y de archivos.
    - Leer líneas de forma **segura** (`fgets`, `getline`).
    - Comprender el *buffering* y el tratamiento de errores (`errno`, `ferror`).

---

## 9.1 La biblioteca `<stdio.h>`

C ofrece tres flujos estándar abiertos automáticamente:

| Flujo | Descriptor | Uso |
|-------|-----------|-----|
| `stdin` | 0 | Entrada |
| `stdout` | 1 | Salida (con buffer) |
| `stderr` | 2 | Errores (sin buffer) |

---

## 9.2 Salida formateada

```c
printf("%d %5.2f %s %c %x\n", 42, 3.14159, "hola", 'A', 255);
//      entero  ancho.prec  cadena  char  hex     → "42  3.14 hola A ff"
int n = snprintf(buf, sizeof buf, "%d-%d", a, b);  // a búfer, seguro
fprintf(stderr, "error: %s\n", msg);               // a un flujo concreto
```

La tabla completa de especificadores está en el
[Apéndice B](../apendices/apendice-b.md).

---

## 9.3 Entrada formateada

```c
int x;
if (scanf("%d", &x) != 1) { /* entrada inválida */ }
```

!!! warning "Los peligros de scanf"
    `scanf("%s", buf)` **no limita** la longitud y desborda el búfer. Usa una
    anchura: `scanf("%63s", buf)` para un `buf[64]`. En general, para entrada de
    usuario es más robusto leer una **línea completa** y luego parsearla.

---

## 9.4 E/S de caracteres y líneas

```c
int c;
while ((c = getchar()) != EOF) putchar(c);   // copia stdin→stdout

char linea[256];
while (fgets(linea, sizeof linea, stdin)) {   // lee una línea con límite
    // fgets conserva el '\n'; recórtalo si hace falta
}
```

POSIX ofrece `getline`, que asigna el búfer dinámicamente (sin límite fijo):

```c
char *linea = NULL; size_t cap = 0;
ssize_t len;
while ((len = getline(&linea, &cap, stdin)) != -1) { /* ... */ }
free(linea);
```

---

## 9.5 Archivos

```c
FILE *f = fopen("datos.txt", "r");   // modos: r w a r+ w+ a+, "b" para binario
if (!f) { perror("fopen"); return 1; }

char linea[256];
while (fgets(linea, sizeof linea, f)) fputs(linea, stdout);
fclose(f);
```

Acceso aleatorio y E/S binaria:

```c
fseek(f, 0, SEEK_END);    // ir al final
long tam = ftell(f);      // posición = tamaño
rewind(f);                // volver al principio

struct Registro r;
fread(&r, sizeof r, 1, f);    // lee un registro binario
fwrite(&r, sizeof r, 1, f);   // escribe
```

---

## 9.6 Buffering

`stdout` a terminal es *line-buffered* (se vuelca al `\n`); redirigido a archivo
es *fully buffered*. `stderr` no tiene búfer. Fuerza el volcado con `fflush`:

```c
printf("Procesando...");   // puede no aparecer aún
fflush(stdout);            // ahora sí
```

Controla el modo con `setvbuf`. Esto explica por qué los mensajes de depuración
con `printf` a veces «desaparecen» tras un crash: estaban en el búfer. Usa
`stderr` o `fflush` para logs críticos.

---

## 9.7 Errores y finalización

```c
#include <errno.h>
FILE *f = fopen(ruta, "r");
if (!f) {
    fprintf(stderr, "No se pudo abrir %s: %s\n", ruta, strerror(errno));
}
if (ferror(f)) { /* error de flujo */ }
if (feof(f))   { /* fin de archivo */ }
```

`atexit` registra funciones de limpieza que se ejecutan al terminar el programa.

---

## Conexión con la actualidad

La E/S con buffer de `<stdio.h>` es portable pero **no es la más rápida ni la más
escalable**. Los servidores de alto rendimiento de 2024–2025 abandonan `FILE *`
por las *system calls* directas (`read`/`write`, cap. 13 y 39) y, sobre todo, por
**`io_uring`** (cap. 39), la interfaz de E/S asíncrona de Linux que elimina la
sobrecarga de las llamadas al sistema mediante anillos compartidos entre kernel y
espacio de usuario. Aun así, dominar `<stdio.h>` sigue siendo imprescindible: es
la base de la mayoría de herramientas CLI (cap. 36) y la forma idiomática de leer
configuración, *logs* y datos. En seguridad, los *format string bugs* (pasar
datos del usuario como cadena de formato de `printf`) siguen apareciendo en
auditorías; la regla es inmutable: **nunca** `printf(entrada_usuario)`, siempre
`printf("%s", entrada_usuario)`.

---

## Ejercicios

!!! example "Ejercicio 9.1 — cat minimalista ★"
    Implementa un `cat` que reciba nombres de archivo por `argv` y vuelque su
    contenido a `stdout`.

!!! example "Ejercicio 9.2 — wc ★★"
    Cuenta líneas, palabras y bytes de un archivo (clon de `wc`).

!!! example "Ejercicio 9.3 — Copia binaria robusta ★★"
    Copia un archivo binario en bloques con `fread`/`fwrite`, comprobando errores
    en cada operación.

!!! example "Ejercicio 9.4 — Mini-printf, parte 2 ★★★★"
    Retoma tu `mi_printf` del cap. 4 y añade soporte de `%f`, anchura y la
    variante `mi_snprintf` que respeta el tamaño del búfer.

---

## Referencias

- ISO/IEC 9899:2018, §7.21 (`<stdio.h>`).
- *Advanced Programming in the UNIX Environment* (Stevens & Rago), capítulos de E/S.
- The Open Group: `getline`, `fgets`, `fread` (POSIX.1-2017).
