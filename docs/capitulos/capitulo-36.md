---
tags:
  - Maestría
  - CLI
  - TUI
---

# Capítulo 36 — Desarrollo de Herramientas CLI y TUI

!!! abstract "Objetivos de aprendizaje"
    - Aplicar la **filosofía Unix** al diseño de herramientas de línea de comandos.
    - Parsear argumentos, leer entrada (readline) y dar salida con color y formato.
    - Construir interfaces de terminal (**TUI**) con ncurses y librerías modernas.

---

## 36.1 Filosofía Unix y CLI

«Haz una cosa y hazla bien»; «escribe programas que cooperen»; «el texto es la
interfaz universal». Una buena CLI: lee de `stdin`, escribe en `stdout`, los
errores van a `stderr`, devuelve un código de salida significativo y es
*pipeable*.

---

## 36.2 Parsing de argumentos

```c
#include <getopt.h>
int main(int argc, char **argv) {
    int verbose = 0; const char *salida = NULL; int opt;
    static struct option largas[] = {
        {"verbose", no_argument,       0, 'v'},
        {"output",  required_argument, 0, 'o'},
        {0, 0, 0, 0}
    };
    while ((opt = getopt_long(argc, argv, "vo:", largas, NULL)) != -1) {
        switch (opt) {
            case 'v': verbose = 1; break;
            case 'o': salida = optarg; break;
            default: /* uso */ return 2;
        }
    }
}
```

`getopt`/`getopt_long` (POSIX/GNU) es el estándar; alternativas: `argp`, `argtable`.

---

## 36.3–36.4 Entrada y salida formateada

- **Readline** (GNU): edición de línea, historial, autocompletado — base de los
  REPL (cap. 19).
- **Salida con color**: secuencias de escape ANSI.

```c
printf("\033[1;31mError:\033[0m algo falló\n");   // negrita roja, luego reset
```

!!! tip "Detecta el terminal"
    Comprueba `isatty(STDOUT_FILENO)` antes de emitir color: si la salida está
    redirigida a un archivo o *pipe*, desactiva los códigos ANSI (o respeta
    `NO_COLOR`).

---

## 36.5–36.6 TUI con ncurses y librerías modernas

**ncurses** controla la pantalla completa del terminal (ventanas, teclado,
colores) — base de `htop`, `vim`, `mc`:

```c
#include <ncurses.h>
initscr(); noecho(); cbreak(); keypad(stdscr, TRUE);
mvprintw(0, 0, "Hola TUI. Pulsa q para salir.");
refresh();
while (getch() != 'q') { /* ... */ }
endwin();
```

Librerías modernas: **notcurses** (gráficos avanzados, *blitting*), **termbox2**.

---

## 36.7–36.10 Tablas/gráficos, señales, plugins y ejemplos

- Salida de **tablas** alineadas y *sparklines*/barras en ASCII.
- **Señales** (`SIGINT`, `SIGTERM`): manejo limpio del ciclo de vida (cap. 39).
- **Plugins**: carga dinámica con `dlopen`/`dlsym`.
- Herramientas C notables: `coreutils`, `git`, `htop`, `tmux`, `fzf` (Go pero
  inspirador), `ripgrep` (Rust).

---

## Conexión con la actualidad

Vivimos un **renacimiento de las herramientas de terminal**. Aunque muchas
estrellas nuevas se escriben en Rust (ripgrep, bat, eza) o Go (fzf, lazygit), el
sustrato sigue siendo C: `git`, `coreutils`, `tmux`, `htop`, `vim`/`neovim` y la
propia `ncurses`/`readline` que muchas dependen. La CLI ha resurgido por la
cultura *developer experience* y la automatización (CI/CD, contenedores), donde
las herramientas *scriptables* y *pipeables* son insustituibles. Bibliotecas
modernas en C como **notcurses** demuestran que el terminal puede mostrar
gráficos, vídeo y animaciones sofisticadas. Y la convención **NO_COLOR** y el
respeto por `isatty`/*pipes* se han vuelto buenas prácticas esperadas. Escribir
una CLI pulida en C —con `--help`, códigos de salida correctos y buen
comportamiento en *pipes*— es una pieza de portafolio que demuestra dominio del
entorno Unix.

---

## Ejercicios

!!! example "Ejercicio 36.1 — Clon de `head`/`tail` ★★"
    Implementa `head -n N` con `getopt`, leyendo de archivo o `stdin`, y
    *pipeable*.

!!! example "Ejercicio 36.2 — REPL con readline ★★★"
    Añade a tu intérprete (cap. 19) historial, edición de línea y autocompletado
    con GNU Readline.

!!! example "Ejercicio 36.3 — Monitor TUI ★★★★"
    Un mini-`htop` con ncurses que muestre procesos y uso de CPU/memoria,
    actualizado en vivo.

!!! example "Ejercicio 36.4 — Color respetuoso ★★"
    Da color a la salida solo cuando `isatty` y `NO_COLOR` no esté definido.

---

## Referencias

- *The Art of Unix Programming* (Eric S. Raymond) — gratuito.
- [Command Line Interface Guidelines](https://clig.dev/).
- [ncurses](https://invisible-island.net/ncurses/), [notcurses](https://notcurses.com/), GNU Readline.
- Código de `coreutils` y `htop` como referencia.
