---
tags:
  - Fundamentos
  - Control de flujo
---

# Capítulo 3 — Control de Flujo

!!! abstract "Objetivos de aprendizaje"
    - Usar con criterio `if`/`else if`/`else` y `switch`.
    - Elegir el bucle adecuado (`while`, `do-while`, `for`) y controlarlo con
      `break`/`continue`.
    - Comprender los **usos legítimos** de `goto` y sus alternativas.

---

## 3.1 Sentencias condicionales

```c
if (temp > 38.0) {
    puts("Fiebre alta");
} else if (temp > 37.0) {
    puts("Febrícula");
} else {
    puts("Normal");
}
```

!!! tip "Llaves siempre"
    Aunque el cuerpo sea de una línea, usa llaves. El infame *goto fail* de Apple
    (2014), una vulnerabilidad de TLS en iOS/macOS, fue causado por un `if` sin
    llaves seguido de dos sentencias.

### `switch` y *fallthrough*

```c
switch (opcion) {
    case 1:
        puts("Uno");
        break;          // sin break, "cae" al siguiente case
    case 2:
    case 3:             // fallthrough intencional: 2 y 3 comparten cuerpo
        puts("Dos o tres");
        break;
    default:
        puts("Otro");
}
```

!!! note "C23: marcar el fallthrough"
    El atributo `[[fallthrough]];` documenta una caída intencional y silencia el
    aviso `-Wimplicit-fallthrough`.

---

## 3.2 Bucles

```c
// while: comprueba antes de entrar
while (cond) { /* ... */ }

// do-while: ejecuta al menos una vez
do { /* ... */ } while (cond);

// for: el más común; init, condición, paso
for (int i = 0; i < n; i++) { /* ... */ }

// bucle infinito idiomático
for (;;) { if (terminar) break; }
```

Regla práctica: usa `for` cuando conoces (o calculas) el número de iteraciones;
`while` cuando dependes de una condición externa; `do-while` cuando el cuerpo
debe ejecutarse al menos una vez (menús, validación de entrada).

---

## 3.3 Control de bucles

- `break`: abandona el bucle (o `switch`) más interno.
- `continue`: salta al siguiente ciclo del bucle más interno.

```c
for (int i = 0; i < n; i++) {
    if (datos[i] < 0) continue;   // ignora negativos
    if (datos[i] == CENTINELA) break;
    procesar(datos[i]);
}
```

---

## 3.4 La sentencia `goto`

`goto` salta a una etiqueta dentro de la misma función. Aunque tiene mala fama
por *Go To Statement Considered Harmful* (Dijkstra, 1968), tiene **un uso
idiomático y limpio en C**: el patrón de *cleanup* centralizado, omnipresente en
el kernel de Linux.

```c title="Patrón goto-cleanup"
int procesar(void) {
    int rc = -1;
    FILE *f = fopen("datos.bin", "rb");
    if (!f) goto fin;

    char *buf = malloc(1024);
    if (!buf) goto cerrar;

    if (leer(f, buf) < 0) goto liberar;
    rc = 0;   // éxito

liberar:
    free(buf);
cerrar:
    fclose(f);
fin:
    return rc;
}
```

Las alternativas estructuradas (banderas, funciones auxiliares) suelen ser más
verbosas cuando hay varios recursos que liberar en orden inverso. La regla:
**solo saltos hacia adelante, solo para cleanup**.

---

## Conexión con la actualidad

El control de flujo es donde nacen vulnerabilidades de altísimo impacto. El ya
citado **`goto fail`** (CVE-2014-1266) dejó a millones de dispositivos Apple
aceptando certificados TLS inválidos por una línea `goto fail;` duplicada. En
2024–2025, los analizadores estáticos modernos (clang `-Wunreachable-code`,
CodeQL, Coverity — capítulo 25) detectan automáticamente código muerto tras un
salto y *fallthrough* accidentales. El patrón `goto-cleanup`, por su parte, sigue
siendo el estándar de facto en el kernel de Linux, donde conviven millones de
líneas que lo emplean para una gestión de errores predecible.

---

## Ejercicios

!!! example "Ejercicio 3.1 — Adivina el número ★"
    El programa elige un número con `rand()`; el usuario adivina y recibe
    realimentación «mayor/menor» en un bucle hasta acertar.

!!! example "Ejercicio 3.2 — Tablas de multiplicar ★"
    Imprime las tablas del 1 al 10 con bucles `for` anidados, bien alineadas.

!!! example "Ejercicio 3.3 — Menú interactivo ★★"
    Implementa un menú con `do-while` + `switch` que repita hasta elegir
    «Salir». Valida entradas inválidas.

!!! example "Ejercicio 3.4 — Fibonacci ★★"
    Imprime los primeros 30 términos de Fibonacci con un `for`. ¿En qué término
    desbordas `int`? ¿Y `uint64_t`?

!!! example "Ejercicio 3.5 — goto-cleanup ★★★"
    Reescribe una función con tres recursos (archivo, buffer, socket simulado)
    usando el patrón `goto-cleanup`. Verifica con Valgrind que no hay fugas en
    ninguna ruta de error.

---

## Referencias

- E. W. Dijkstra. *Go To Statement Considered Harmful*. CACM, 1968.
- ISO/IEC 9899:2018, §6.8 (sentencias).
- *Linux Kernel Coding Style*, sección sobre `goto`.
- Análisis técnico de CVE-2014-1266 («goto fail»).
