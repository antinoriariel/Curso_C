---
tags:
  - Maestría
  - Linux
  - TLPI
  - Sistemas
---

# Capítulo 39 — TLPI: Programación Avanzada de Sistemas Linux

!!! abstract "Objetivos de aprendizaje"
    - Dominar las **llamadas al sistema**: procesos, señales, timers, E/S avanzada.
    - Usar **`io_uring`**, *namespaces*, **cgroups** y *capabilities*.
    - Sentar las bases de la contenerización moderna desde C.

!!! note "Sobre el título"
    TLPI = *The Linux Programming Interface*, el tratado de referencia de Michael
    Kerrisk. Este capítulo recorre sus temas avanzados con foco en lo vigente.

---

## 39.1 Llamadas al sistema

La frontera entre tu programa y el kernel. `errno` reporta el error; comprueba
**siempre** el retorno:

```c
#include <unistd.h>
ssize_t n = write(STDOUT_FILENO, "hola\n", 5);
if (n == -1) perror("write");
```

`strace` muestra todas las *syscalls* que hace un proceso — herramienta
imprescindible.

---

## 39.2 Gestión de procesos

```c
pid_t pid = fork();                 // crea un proceso hijo
if (pid == 0) {                     // hijo
    execvp("ls", (char*[]){"ls","-l",NULL});
    _exit(127);                     // solo si exec falla
}
int status; waitpid(pid, &status, 0);   // padre espera
```

Procesos zombis y huérfanos, `setsid` (demonios), grupos de procesos y sesiones.

---

## 39.3 Señales en profundidad

```c
#include <signal.h>
void handler(int sig) { /* async-signal-safe ONLY */ }
struct sigaction sa = { .sa_handler = handler };
sigaction(SIGINT, &sa, NULL);       // preferible a signal()
```

!!! warning "Async-signal-safety"
    Dentro de un manejador de señal solo puedes llamar a funciones
    *async-signal-safe* (`write`, `_exit`…). `printf`/`malloc` **no** lo son. El
    patrón moderno: el manejador solo escribe en un *self-pipe* o un
    `volatile sig_atomic_t`, y el bucle principal reacciona. Alternativa elegante:
    `signalfd`.

---

## 39.4–39.5 Timers y memoria avanzada

- **Timers**: `timerfd`, `setitimer`, `clock_gettime` (relojes monotónicos).
- **Memoria**: `mmap` (archivos y anónima), memoria compartida POSIX, `mlock`,
  `madvise`.

---

## 39.6 E/S avanzada

E/S no bloqueante (`O_NONBLOCK`), multiplexada (`select`/`poll`/`epoll`, cap. 13),
*scatter-gather* (`readv`/`writev`), `sendfile` (copia *zero-copy* kernel-a-kernel).

---

## 39.7 io_uring en profundidad

**`io_uring`** (Linux 5.1+) es la mayor innovación en E/S de la última década:
dos anillos compartidos entre usuario y kernel (*submission* y *completion*)
permiten E/S asíncrona **sin** una llamada al sistema por operación, eliminando la
sobrecarga de `epoll`.

```c
// Esquema con liburing
struct io_uring ring;
io_uring_queue_init(256, &ring, 0);
struct io_uring_sqe *sqe = io_uring_get_sqe(&ring);
io_uring_prep_read(sqe, fd, buf, len, 0);     // encola lectura asíncrona
io_uring_submit(&ring);
struct io_uring_cqe *cqe;
io_uring_wait_cqe(&ring, &cqe);               // espera la finalización
```

---

## 39.8–39.10 Namespaces, cgroups y capabilities

Los **bloques de construcción de los contenedores**:

- **Namespaces** (`clone` con `CLONE_NEW*`): aíslan PID, red, montajes, usuarios,
  UTS, IPC.
- **cgroups v2**: limitan y contabilizan recursos (CPU, memoria, E/S).
- **Capabilities**: dividen el «todopoderoso» root en privilegios granulares
  (`CAP_NET_ADMIN`, `CAP_SYS_ADMIN`…), reduciendo la superficie de ataque.

```c
// Un "contenedor" mínimo: nuevo namespace de PID y montajes
int flags = CLONE_NEWPID | CLONE_NEWNS | CLONE_NEWUTS | SIGCHLD;
pid_t pid = clone(child_fn, stack_top, flags, NULL);
```

---

## Conexión con la actualidad

Este capítulo explica **cómo funciona la nube por dentro**. Docker, Podman,
containerd y Kubernetes no son magia: son *namespaces* + *cgroups* +
*capabilities* + *seccomp*, todo expuesto por el kernel de Linux y consumido vía
C. Saber esto te permite depurar, asegurar y optimizar contenedores a un nivel que
pocos alcanzan. **`io_uring`** es la frontera del rendimiento de E/S en 2024–2025:
bases de datos, *proxies* y *runtimes* lo están adoptando para exprimir los NVMe y
las redes de alta velocidad, aunque su superficie de ataque también ha generado
CVEs y restricciones en entornos endurecidos. Y la gestión de **señales** y
procesos sigue siendo donde se decide la robustez de cualquier demonio o servicio
de larga vida. Es, junto con el cap. 23, la cumbre del C de sistemas.

---

## Ejercicios

!!! example "Ejercicio 39.1 — Mini-shell ★★★"
    Implementa una shell que ejecute comandos con `fork`/`exec`/`wait`, soporte
    *pipes* (`|`) y redirección (`>`, `<`).

!!! example "Ejercicio 39.2 — Señales bien hechas ★★★"
    Maneja `SIGINT`/`SIGTERM` para un apagado limpio usando `sigaction` y el
    patrón *self-pipe* o `signalfd`.

!!! example "Ejercicio 39.3 — Servidor con io_uring ★★★★★"
    Reescribe el servidor de eco (cap. 13) con `liburing` y compáralo con la
    versión `epoll`.

!!! example "Ejercicio 39.4 — Contenedor mínimo ★★★★★"
    Crea un «contenedor» que ejecute un proceso en nuevos *namespaces* de PID y
    montajes, con `chroot`/`pivot_root` y un *cgroup* que limite la memoria.

---

## Referencias

- Michael Kerrisk. *The Linux Programming Interface* (TLPI) — la referencia.
- W. R. Stevens, S. Rago. *Advanced Programming in the UNIX Environment* (APUE).
- Jens Axboe. *Efficient IO with io_uring* (documento de diseño); [liburing](https://github.com/axboe/liburing).
- `man 7 namespaces`, `man 7 cgroups`, `man 7 capabilities`.
