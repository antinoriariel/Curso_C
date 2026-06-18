---
tags:
  - Avanzado
  - Concurrencia
  - Paralelismo
---

# Capítulo 12 — Concurrencia y Paralelismo

!!! abstract "Objetivos de aprendizaje"
    - Distinguir concurrencia de paralelismo, y procesos de hilos.
    - Crear y sincronizar hilos con **pthreads** y con `<threads.h>` (C11).
    - Usar *mutexes*, variables de condición, semáforos y **atómicos**.
    - Reconocer condiciones de carrera, *deadlocks* y resolver problemas clásicos.

---

## 12.1 Conceptos fundamentales

- **Concurrencia**: estructurar el programa como tareas que progresan de forma
  solapada (no necesariamente a la vez).
- **Paralelismo**: ejecutar realmente varias tareas a la vez (varios núcleos).
- **Proceso**: espacio de memoria propio. **Hilo**: flujo de ejecución que
  comparte la memoria de su proceso → comunicación rápida pero peligrosa.

---

## 12.2 Hilos POSIX (pthreads)

```c title="hilos.c — compilar con -pthread"
#include <pthread.h>
#include <stdio.h>

void *trabajo(void *arg) {
    int id = *(int *)arg;
    printf("Hilo %d\n", id);
    return NULL;
}

int main(void) {
    pthread_t h[4]; int ids[4];
    for (int i = 0; i < 4; i++) {
        ids[i] = i;
        pthread_create(&h[i], NULL, trabajo, &ids[i]);
    }
    for (int i = 0; i < 4; i++) pthread_join(h[i], NULL);
}
```

---

## 12.3 Sincronización con pthreads

La condición de carrera (*data race*) — dos hilos acceden a la misma memoria y al
menos uno escribe, sin sincronización — es **undefined behavior**. El *mutex* la
evita:

```c
pthread_mutex_t m = PTHREAD_MUTEX_INITIALIZER;
long contador = 0;

void *sumar(void *_) {
    for (int i = 0; i < 1000000; i++) {
        pthread_mutex_lock(&m);
        contador++;             // sección crítica
        pthread_mutex_unlock(&m);
    }
    return NULL;
}
```

La **variable de condición** permite esperar a que se cumpla un predicado sin
*busy-waiting* — base del patrón productor/consumidor.

---

## 12.4–12.6 Semáforos, atómicos y TLS

```c
#include <semaphore.h>
sem_t s; sem_init(&s, 0, N);   // N permisos
sem_wait(&s); /* ... */ sem_post(&s);

#include <stdatomic.h>          // C11: operaciones atómicas sin mutex
atomic_long c = 0;
atomic_fetch_add(&c, 1);        // incremento atómico, lock-free

_Thread_local int errno_local;  // TLS: una copia por hilo
```

Los atómicos exponen el **modelo de memoria** de C11 (`memory_order_relaxed`,
`acquire`, `release`, `seq_cst`): la base de las estructuras *lock-free*.

---

## 12.7 Hilos en C11 (`<threads.h>`)

La alternativa estándar y portable a pthreads (soporte aún desigual):

```c
#include <threads.h>
int tarea(void *arg) { return 0; }
thrd_t t; thrd_create(&t, tarea, NULL); thrd_join(t, NULL);
```

---

## 12.8 Procesos (POSIX)

```c
pid_t pid = fork();             // duplica el proceso
if (pid == 0)      execlp("ls", "ls", "-l", NULL);   // hijo: reemplaza imagen
else if (pid > 0)  wait(NULL);                        // padre: espera
```

Comunicación entre procesos (IPC): *pipes*, memoria compartida, colas de mensajes
(detalle en cap. 39).

---

## 12.9–12.10 Patrones y problemas clásicos

- **Thread pool**: N hilos consumen tareas de una cola.
- **Productor/consumidor**: búfer acotado + condición.
- **Lectores/escritores**, **cena de los filósofos** (prevención de *deadlock*
  por orden de adquisición de *locks*).

!!! warning "Las cuatro condiciones de Coffman para el deadlock"
    Exclusión mutua, retención y espera, no apropiación, y espera circular. Rompe
    cualquiera (p. ej., adquiere siempre los *locks* en un orden global fijo) y
    eliminas el *deadlock*.

---

## 12.11 Herramientas de análisis

```bash
gcc -fsanitize=thread programa.c -pthread -o p   # ThreadSanitizer (TSan)
valgrind --tool=helgrind ./programa              # detector de carreras
```

ThreadSanitizer detecta *data races* que casi nunca se reproducen a mano.

---

## Conexión con la actualidad

El **fin del *free lunch***: las CPUs ya no aumentan su frecuencia, sino su número
de núcleos (los servidores de 2025 superan los 100–200 núcleos). El rendimiento
**solo** crece si el software es paralelo, y C — con pthreads, OpenMP (cap. 21) y
los atómicos de C11 — es la base sobre la que se construyen esos sistemas. Las
estructuras de datos **lock-free** y **wait-free**, basadas en `stdatomic.h` y en
el modelo de memoria, son un área de investigación e ingeniería muy activa
(colas MPMC, *hazard pointers*, RCU — esta última, omnipresente en el kernel de
Linux). En seguridad, las condiciones de carrera **TOCTOU** (*time-of-check to
time-of-use*) siguen produciendo vulnerabilidades de escalada de privilegios, y
ThreadSanitizer se ha vuelto estándar en la CI de proyectos serios.

---

## Ejercicios

!!! example "Ejercicio 12.1 — La carrera ★★"
    Ejecuta el contador **sin** mutex con 4 hilos y observa que el resultado es
    erróneo e inestable. Corrígelo con mutex y con atómicos; compara rendimiento.

!!! example "Ejercicio 12.2 — Productor/consumidor ★★★"
    Implementa un búfer acotado con mutex + variable de condición.

!!! example "Ejercicio 12.3 — Thread pool ★★★★"
    Construye un *pool* de N hilos que procese una cola de tareas (`void (*)(void*)`).

!!! example "Ejercicio 12.4 — Filósofos sin deadlock ★★★★"
    Resuelve la cena de los filósofos garantizando ausencia de *deadlock*.
    Verifica con Helgrind/TSan.

---

## Referencias

- ISO/IEC 9899:2018, §7.17 (`<stdatomic.h>`), §7.26 (`<threads.h>`).
- *Programming with POSIX Threads* (David Butenhof).
- *C++ Concurrency in Action* (Anthony Williams) — el modelo de memoria, aplicable a C11.
- Paul McKenney. *Is Parallel Programming Hard, And, If So, What Can You Do About It?*
