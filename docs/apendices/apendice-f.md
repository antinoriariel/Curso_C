# Apéndice F — Guía Rápida de GDB

Compila siempre con `-g` (y preferiblemente `-O0`) para depurar.

## Arranque

```bash
gdb ./programa             # cargar
gdb --args ./programa a b  # con argumentos
gdb ./programa core        # análisis post-mortem de un core dump
```

## Comandos esenciales

| Comando | Abreviatura | Acción |
|---------|-------------|--------|
| `run` | `r` | Ejecutar el programa |
| `break f` | `b f` | Punto de ruptura en la función `f` |
| `break archivo.c:42` | `b` | Ruptura en una línea |
| `break f if x==0` | | Ruptura **condicional** |
| `next` | `n` | Siguiente línea (sin entrar en funciones) |
| `step` | `s` | Siguiente línea (entrando en funciones) |
| `finish` | | Salir de la función actual |
| `continue` | `c` | Continuar hasta el siguiente *breakpoint* |
| `print expr` | `p` | Evaluar e imprimir |
| `display expr` | | Imprimir tras cada paso |
| `watch x` | | Parar cuando `x` cambie (*watchpoint*) |
| `backtrace` | `bt` | Pila de llamadas |
| `frame n` | `f n` | Seleccionar un marco de la pila |
| `info locals` | | Variables locales |
| `info args` | | Argumentos de la función |
| `list` | `l` | Mostrar código fuente |
| `quit` | `q` | Salir |

## Inspección de memoria

```gdb
p *ptr            # desreferenciar un puntero
p array[0]@10     # imprimir 10 elementos desde array[0]
x/16xb &var       # examinar 16 bytes en hexadecimal
p sizeof(struct T)
ptype variable    # mostrar el tipo
```

## Trucos útiles

```gdb
set var x = 5         # modificar una variable en caliente
tbreak f              # breakpoint temporal (se borra al alcanzarlo)
catch throw           # (C++) parar en excepciones
record / reverse-next # depuración reversa (si está soportada)
```

!!! tip "TUI y diseño"
    Pulsa ++ctrl+x++ ++ctrl+a++ para activar la interfaz TUI (código + comandos).
    Para una experiencia más cómoda, prueba `gef`, `pwndbg` o `cgdb`.

Más en el [Capítulo 29](../capitulos/capitulo-29.md).
