---
tags:
  - Experto
  - Depuración
  - Profiling
---

# Capítulo 29 — Depuración y Perfilamiento Avanzado

!!! abstract "Objetivos de aprendizaje"
    - Dominar **GDB** a nivel profundo (puntos de observación, scripting, core dumps).
    - Perfilar CPU, memoria y caché con `perf`, Valgrind y *flame graphs*.
    - Usar *tracing* dinámico (ftrace, eBPF, DTrace) y depurar en remoto/embebido.

---

## 29.1 GDB a nivel profundo

```bash
gcc -g -O0 programa.c -o programa
gdb ./programa
```

```gdb
break main              # punto de ruptura
run arg1 arg2           # ejecutar
next / step             # siguiente línea / entrar en función
print variable          # inspeccionar
watch x                 # parar cuando x CAMBIE (watchpoint)
backtrace               # pila de llamadas
info locals             # variables locales
continue                # seguir
```

Técnicas avanzadas: *conditional breakpoints* (`break f if n==0`), *reverse
debugging* (`record`, `reverse-next`), *pretty printers*, y *scripting* con
Python.

---

## 29.2 Depuración de memoria

```bash
valgrind --leak-check=full --track-origins=yes ./programa
gcc -fsanitize=address,undefined -g programa.c -o p && ./p
```

ASan suele ser 10–50× más rápido que Valgrind y da diagnósticos con la pila de
asignación. Valgrind no necesita recompilar y detecta más casos de memoria no
inicializada (Memcheck).

---

## 29.3 Perfilamiento de CPU

```bash
perf record -g ./programa     # muestrea la pila de llamadas
perf report                   # explora dónde se gasta el tiempo
perf stat ./programa          # ciclos, IPC, fallos de caché, branch misses
```

Visualiza con **flame graphs** (Brendan Gregg): el ancho de cada barra = tiempo
acumulado. Es la forma más rápida de identificar el *hotspot*.

---

## 29.4–29.5 Tracing dinámico

- **ftrace** y **perf**: tracing del kernel y *uprobes*/*kprobes* en funciones.
- **eBPF / bpftrace** (cap. 23): observabilidad programable sin reiniciar nada.
- **DTrace/SystemTap**: tracing de producción.

```bash
# bpftrace: latencia de la syscall read, en un histograma
bpftrace -e 'tracepoint:syscalls:sys_enter_read { @ = count(); }'
```

---

## 29.6–29.8 Caché, concurrencia y heap profiling

- `cachegrind`/`perf` para *cache misses* y *branch mispredictions*.
- **TSan** y Helgrind para carreras (cap. 12).
- **massif** (Valgrind) y **heaptrack** para perfilar el uso de *heap* a lo largo
  del tiempo.

---

## 29.9–29.11 Remoto, core dumps y red

- **Depuración remota**: `gdbserver` (embebido, kernel, contenedores).
- **Core dumps**: analizar un *crash* post-mortem con `gdb programa core`.
- **Rendimiento de red**: `iperf`, `tcpdump`, `ss`.

---

## Conexión con la actualidad

La **observabilidad** es uno de los pilares de la ingeniería de fiabilidad (SRE)
moderna, y su base de bajo nivel es C. **eBPF** ha revolucionado el *tracing* de
producción: herramientas como **bpftrace**, **Pixie**, **Parca** y el *continuous
profiling* permiten perfilar sistemas en vivo, con sobrecarga mínima, sin
reiniciar ni recompilar — y todas se programan, en última instancia, en C. Los
**flame graphs** de Brendan Gregg se han convertido en el lenguaje visual
universal del rendimiento. En depuración, **rr** (Mozilla) populariza el
*record-and-replay* determinista, que convierte los *heisenbugs* (errores que
desaparecen al observarlos) en reproducibles. Saber usar GDB, `perf` y ASan con
soltura es, en la práctica, lo que distingue a un programador de C senior.

---

## Ejercicios

!!! example "Ejercicio 29.1 — Sesión de GDB ★★"
    Depura un programa con un *segfault*: localiza la línea con `backtrace`,
    inspecciona variables y corrige la causa.

!!! example "Ejercicio 29.2 — Watchpoint ★★★"
    Usa un *watchpoint* para descubrir qué parte del código corrompe una variable
    que «cambia sola».

!!! example "Ejercicio 29.3 — Flame graph ★★★"
    Perfila con `perf` un programa con un *hotspot* artificial y genera su flame
    graph; optimiza y vuelve a medir.

!!! example "Ejercicio 29.4 — Core dump ★★★"
    Provoca un *crash*, captura el core (`ulimit -c unlimited`) y analízalo
    post-mortem con GDB.

---

## Referencias

- *The Art of Debugging with GDB, DDD, and Eclipse* (Matloff & Salzman).
- Brendan Gregg. *Systems Performance: Enterprise and the Cloud*, 2.ª ed.
- [Documentación de GDB](https://www.gnu.org/software/gdb/documentation/), [perf wiki](https://perf.wiki.kernel.org/).
- [rr: record and replay](https://rr-project.org/).
