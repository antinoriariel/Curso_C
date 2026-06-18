
# 📚 Índice Completo del Curso de C

---

## Capítulo 1 — Introducción al Lenguaje C

### 1.1 Historia y Evolución de C
- Orígenes: Dennis Ritchie y los Bell Labs (1972)
- C经典: K&R C, ANSI C (C89/C90), C99, C11, C17, C23
- Filosofía del lenguaje: confianza en el programador, mínimo runtime, portabilidad

### 1.2 Entorno de Desarrollo
- Instalación de GCC, Clang y MSVC
- Configuración de un editor (VS Code, CLion, Vim/Neovim)
- Hello World: primer programa compilado y ejecutado
- El pipeline de compilación: preprocesador → compilador → ensamblador → linker

### 1.3 Estructura de un Programa en C
- Directivas `#include` y `#define`
- La función `main()`: firmas válidas, valores de retorno
- Declaraciones vs definiciones
- Comentarios: `//` y `/* */`

### 1.4 Compilación desde la Terminal
- `gcc`, `clang`, `cl` (MSVC)
- Flags esenciales: `-std=c17`, `-Wall`, `-Wextra`, `-pedantic`, `-O2`, `-g`
- Archivos objeto y linking manual
- Makefiles básicos

### 1.5 Ejercicios
- Escribir, compilar y ejecutar un Hola Mundo
- Modificar el retorno de `main()` y verificar con `echo $?`
- Introducir errores sintácticos y leer los mensajes del compilador

---

## Capítulo 2 — Tipos de Datos, Variables y Operadores

### 2.1 Tipos de Datos Fundamentales
- `char`, `int`, `float`, `double`, `void`
- Modificadores: `short`, `long`, `signed`, `unsigned`
- El tipo `_Bool` y `<stdbool.h>`
- `size_t`, `intptr_t`, `ptrdiff_t` y `<stddef.h>`

### 2.2 Especificadores de Precisión Fija (C99+)
- `<stdint.h>`: `int8_t`, `uint16_t`, `int32_t`, `uint64_t`
- `<inttypes.h>`: macros `PRIu64`, `SCNd32`, etc.
- Rango y límites: `<limits.h>` y `<float.h>`

### 2.3 Variables
- Declaración, inicialización y asignación
- Ámbito de bloque (`{}`)
- La palabra clave `const`
- `volatile` y su utilidad en sistemas embebidos

### 2.4 Operadores
- Aritméticos: `+`, `-`, `*`, `/`, `%`
- Relacionales: `==`, `!=`, `<`, `>`, `<=`, `>=`
- Lógicos: `&&`, `||`, `!`
- A nivel de bits: `&`, `|`, `^`, `~`, `<<`, `>>`
- Asignación: `=`, `+=`, `-=`, `*=`, etc.
- Incremento/decremento: `++`, `--` (prefijo vs sufijo)
- Operador ternario: `?:`
- `sizeof`: tamaño de tipos y variables
- Precedencia y asociatividad

### 2.5 Conversiones de Tipo
- Conversiones implícitas (promoción entera, `integer promotion`)
- Conversiones explícitas (`cast`)
- Reglas de `usual arithmetic conversions`
- Pérdida de precisión y desbordamientos

### 2.6 Ejercicios
- Calcular área y perímetro de figuras geométricas
- Intercambiar dos variables sin usar una tercera (XOR swap)
- Determinar si un número es potencia de 2 usando operadores de bits
- Imprimir el rango de cada tipo entero usando `<limits.h>`

---

## Capítulo 3 — Control de Flujo

### 3.1 Sentencias Condicionales
- `if`, `else if`, `else`
- Anidamiento y buenas prácticas
- `switch`, `case`, `default`, `break`
- Fallthrough intencional vs accidental

### 3.2 Bucles
- `while`
- `do-while`
- `for` (inicialización, condición, incremento)
- Bucles infinitos intencionales: `for(;;)`

### 3.3 Control de Bucles
- `break`: salida anticipada
- `continue`: saltar a la siguiente iteración
- Bucles anidados y etiquetas (`goto`)

### 3.4 La Sentencia `goto`
- Usos legítimos: salida de buffers anidados, cleanup en kernel
- Controversia y alternativas estructuradas

### 3.5 Ejercicios
- Adivinar un número (con realimentación "mayor/menor")
- Tablas de multiplicar con bucles anidados
- Menú interactivo con `switch`
- Serie de Fibonacci con `for`

---

## Capítulo 4 — Funciones y Ámbito de Variables

### 4.1 Definición y Declaración de Funciones
- Prototipos vs definiciones
- Parámetros formales y reales
- Valor de retorno y `void`
- Funciones sin prototipo (estilo antiguo, evitar)

### 4.2 Paso de Parámetros
- Paso por valor (semántica de C)
- Paso por referencia simulado con punteros
- La palabra clave `inline` (sugerencia al compilador)

### 4.3 Ámbito y Duración
- Variables locales (automáticas)
- Variables estáticas locales: `static`
- Variables globales
- La clase de almacenamiento `extern`
- `register` (obsoleto, sugerencia ignorada por compiladores modernos)

### 4.4 Recursividad
- Recursión directa e indirecta
- Tail recursion y optimización del compilador
- Comparación con iteración (stack overflow, rendimiento)
- Ejemplos: factorial, Fibonacci, Torres de Hanoi, QuickSort recursivo

### 4.5 Punteros a Funciones
- Sintaxis: `tipo (*nombre)(params)`
- Callbacks: `qsort()`, `bsearch()`
- Tablas de dispatch (reemplazo funcional de `switch`)
- Ejemplo: calculadora con tabla de funciones

### 4.6 Funciones Variádicas
- `<stdarg.h>`: `va_list`, `va_start`, `va_arg`, `va_end`
- Implementación de `printf` propia (versión limitada)
- Seguridad y peligros

### 4.7 Ejercicios
- Implementar una función que eleve un número a una potencia (recursiva e iterativa)
- Implementar `qsort` con comparador personalizado
- Crear una calculadora con tabla de punteros a función

---

## Capítulo 5 — Arrays y Cadenas de Caracteres

### 5.1 Arrays Unidimensionales
- Declaración e inicialización
- Acceso mediante índices
- Relación array ↔ puntero
- Arrays de tamaño variable (VLA, opcional en C11)

### 5.2 Arrays Multidimensionales
- Declaración: `int matriz[3][4]`
- Orden row-major
- Inicialización estática
- Paso a funciones (decaimiento a puntero)

### 5.3 Cadenas de Caracteres
- Representación: arrays de `char` terminados en `\0`
- `<string.h>`: `strlen`, `strcpy`, `strncpy`, `strcat`, `strncat`, `strcmp`, `strncmp`, `strchr`, `strrchr`, `strstr`, `strtok`
- Peligros: desbordamiento de buffer, cadenas no terminadas
- Funciones seguras: `strncpy`, `strncat`, `snprintf`
- `strdup` y `strndup` (POSIX, no estándar)

### 5.4 Arrays de Cadenas
- `char *argv[]` y `char **argv`
- Matriz de caracteres vs array de punteros

### 5.5 Algoritmos Clásicos con Arrays
- Búsqueda lineal y binaria
- Ordenación: burbuja, selección, inserción
- Rotación e inversión de arrays

### 5.6 Ejercicios
- Implementar todas las funciones de `<string.h>` manualmente
- Programa que cuente frecuencia de palabras en un texto
- Multiplicación de matrices
- Búsqueda binaria en un array ordenado

---

## Capítulo 6 — Punteros

### 6.1 Conceptos Fundamentales
- Direcciones de memoria y el operador `&`
- El operador de indirección `*`
- Declaración de punteros: `int *p;`
- El puntero nulo (`NULL`) y sus peligros (dereference de null)

### 6.2 Aritmética de Punteros
- `p + n`, `p - n`, `p++`, `p--`
- Diferencia entre punteros: `ptrdiff_t`
- Relación íntima con arrays: `a[i] == *(a + i)`
- Recorrido de arrays con punteros

### 6.3 Punteros a Punteros
- `int **p`: matrices dinámicas, argv
- Paso de punteros a funciones para modificar el puntero original

### 6.4 Punteros a Arrays vs Arrays de Punteros
- `int (*arr)[N]` (puntero a array de N enteros)
- `int *arr[N]` (array de N punteros a entero)
- Diferencia crítica en paso a funciones

### 6.5 Punteros a Funciones (profundización)
- Arrays de punteros a función
- Devolución de punteros a función desde funciones

### 6.6 Punteros `const` y `const` Datos
- `const int *p` (dato constante)
- `int *const p` (puntero constante)
- `const int *const p` (ambos constantes)
- Paso de arrays constantes a funciones

### 6.7 Punteros Genéricos
- `void *`: el puntero genérico
- Aritmética no permitida con `void *`
- Implementación de `qsort` y `bsearch`
- Implementación de genéricos con macros + `void *`

### 6.8 Punteros y Alineamiento
- `_Alignof` y `_Alignas` (C11)
- `aligned_alloc` (C11)
- Consecuencias de violar alineamiento (segfault en ARM, undefined behavior en x86)

### 6.9 Ejercicios
- Implementar una función que invierta una cadena in-place usando punteros
- Implementar `strcat` manual con aritmética de punteros
- Crear un `vector` genérico con `void *`
- Implementar `qsort` genérico desde cero

---

## Capítulo 7 — Gestión Dinámica de Memoria

### 7.1 Memoria en C
- Segmentos: `.text`, `.data`, `.bss`, heap, stack
- Stack: automático, tamaño limitado, rápido
- Heap: dinámico, tamaño controlado por el SO, lento

### 7.2 Funciones de Asignación
- `malloc(size_t size)`: asignar sin inicializar
- `calloc(size_t nmemb, size_t size)`: asignar y zero-inicializar
- `realloc(void *ptr, size_t new_size)`: redimensionar
- `free(void *ptr)`: liberar
- `aligned_alloc` (C11)

### 7.3 Errores Comunes y Buenas Prácticas
- Memory leaks (fuga de memoria)
- Double free
- Use-after-free
- Buffer overflow en heap
- Fragmentation (interna y externa)
- Comprobación del valor de retorno de `malloc`

### 7.4 Implementación de un Asignador Simple
- Pool de memoria fija (arena allocator)
- Free-list con best-fit / first-fit
- Alineamiento manual

### 7.5 Valgrind y AddressSanitizer
- Instalación y uso de Valgrind (memcheck)
- Compilación con `-fsanitize=address`
- Interpretación de informes

### 7.6 Gestión de Memoria para Estructuras de Datos
- Estrategias de ownership
- Patrón: create / destroy
- Copia profunda (deep copy) vs copia superficial (shallow copy)

### 7.7 Ejercicios
- Implementar un vector dinámico (`push_back`, `pop_back`, `reserve`)
- Detectar y corregir memory leaks en un programa dado
- Implementar un arena allocator simple
- Usar Valgrind/ASan en un programa con fugas intencionales

---

## Capítulo 8 — Estructuras, Uniones y Enumeraciones

### 8.1 Estructuras (`struct`)
- Definición y declaración
- Inicialización designada (C99): `struct Point p = {.x = 1, .y = 2};`
- Acceso a miembros: `.` y `->`
- Estructuras anidadas
- Copia y asignación de estructuras
- Paso a funciones por valor y por puntero

### 8.2 Alineamiento y Padding
- Layout en memoria de una estructura
- Reglas de alineamiento natural
- `offsetof` (macro en `<stddef.h>`)
- Reordenación de miembros para minimizar padding
- `__attribute__((packed))` y `#pragma pack`
- Implicaciones en redes y sistemas embebidos

### 8.3 Uniones (`union`)
- Diferencia con structs: superposición en memoria
- Usos: ahorro de memoria, interpretación múltiple de bits
- Type-punning y reglas estrictas de aliasing
- Ejemplo: inspeccionar bytes de un `float`

### 8.4 Enumeraciones (`enum`)
- Definición y valores enteros subyacentes
- Enums como constantes simbólicas
- Enums tipificados en C23: `enum Color : uint8_t { RED, GREEN, BLUE };`

### 8.5 Typedef
- Creación de alias: `typedef struct { ... } Punto;`
- Uso con punteros a función
- Diferencias con `#define`

### 8.6 Estructuras Autoreferenciadas
- Listas enlazadas, árboles
- Forward declaration

### 8.7 Ejercicios
- Definir un `struct Alumno` con nombre, edad y notas (cálculo de promedio)
- Implementar una pila usando unión de struct y array de bytes
- Reordenar miembros de una struct para minimizar su tamaño
- Implementar `htons`/`htonl` manualmente con uniones

---

## Capítulo 9 — Entrada/Salida y Manejo de Archivos

### 9.1 La Biblioteca Estándar `<stdio.h>`
- Flujos estándar: `stdin`, `stdout`, `stderr`
- Redirección desde la terminal

### 9.2 Salida Formateada
- `printf`: especificadores `%d`, `%u`, `%f`, `%e`, `%g`, `%s`, `%p`, `%zu`
- Modificadores: ancho, precisión, banderas (`-`, `+`, `0`, `#`)
- `sprintf`, `snprintf` (seguro), `fprintf`
- `dprintf` (POSIX)

### 9.3 Entrada Formateada
- `scanf`, `fscanf`, `sscanf`
- Problemas comunes: buffer residual con `\n`, desbordamiento
- `fgets` + `sscanf` como alternativa segura

### 9.4 Entrada/Salida de Caracteres y Líneas
- `getchar`, `putchar`
- `gets` (obsoleto, peligroso), `fgets` (seguro)
- `puts`, `fputs`

### 9.5 Archivos
- Modos: `"r"`, `"w"`, `"a"`, `"rb"`, `"wb"`, `"r+"`, `"w+"`
- `fopen`, `fclose`
- `fread`, `fwrite` (E/S binaria)
- `fseek`, `ftell`, `rewind`, `fgetpos`, `fsetpos`
- `feof`, `ferror`
- `remove`, `rename`, `tmpfile`, `tmpnam`

### 9.6 Buffering
- `setvbuf`, `setbuf`, `fflush`
- Buffering completo, de línea, sin buffer

### 9.7 Errores y Finalización
- `perror` y `strerror` (de `<string.h>`)
- `exit`, `atexit`, `_Exit`
- `abort`

### 9.8 Ejercicios
- Leer un archivo CSV y procesarlo línea por línea
- Copiar un archivo binario (imagen/PDF) byte a byte
- Implementar `cat`, `head` y `tail` simplificados
- Serializar y deserializar una estructura a binario

---

## Capítulo 10 — Preprocesador y Compilación

### 10.1 Directivas del Preprocesador
- `#include`: ángulos vs comillas, búsqueda de archivos
- `#define`: macros con y sin parámetros
- `#undef`
- `#if`, `#ifdef`, `#ifndef`, `#else`, `#elif`, `#endif`
- `#error`, `#warning`, `#line`, `#pragma`

### 10.2 Macros Avanzadas
- Operadores: `#` (stringify), `##` (concatenación)
- Macros variádicas: `__VA_ARGS__`, `__VA_OPT__` (C23)
- Macros `do { ... } while(0)` para multi-sentencias
- Macros para aserciones: `assert`, `static_assert` (C11)

### 10.3 Constantes Predefinidas
- `__FILE__`, `__LINE__`, `__DATE__`, `__TIME__`, `__STDC__`, `__STDC_VERSION__`
- `__func__` (identificador de función, C99)

### 10.4 Pragma y Platforma
- `#pragma once` vs include guards
- `#pragma pack`
- `_Pragma()` operator (C99)

### 10.5 Compilación Separada
- Archivos `.h` vs `.c`
- Include guards: `#ifndef HEADER_H` o `#pragma once`
- Variables `extern` y funciones en headers
- El papel del linker: símbolos fuertes y débiles

### 10.6 Make y CMake
- Makefile: targets, dependencias, variables automáticas (`$@`, `$<`, `$^`)
- CMake: `CMakeLists.txt`, targets, bibliotecas
- Compilación condicional con `-D` flags

### 10.7 Bibliotecas Estáticas y Dinámicas
- Creación de `.a` / `.lib` (estáticas)
- Creación de `.so` / `.dll` / `.dylib` (dinámicas)
- Linking estático vs dinámico: ventajas y desventajas
- `dlopen`, `dlsym`, `dlclose` (carga dinámica en runtime)

### 10.8 Ejercicios
- Escribir una macro `MAX(a, b)` que funcione correctamente con cualquier tipo
- Implementar `assert` como macro
- Separar un programa en múltiples archivos con Makefile
- Crear una biblioteca estática y otra dinámica

---

## Capítulo 11 — Estructuras de Datos en C

### 11.1 Listas Enlazadas
- Lista simplemente enlazada
- Lista doblemente enlazada
- Lista circular
- Operaciones: insertar, eliminar, buscar, recorrer, invertir
- Implementación genérica con `void *`

### 11.2 Pilas y Colas
- Pila (LIFO) con array dinámico y con lista enlazada
- Cola (FIFO) con array circular y con lista enlazada
- Cola con prioridad (heap binario)

### 11.3 Árboles
- Árbol binario de búsqueda (BST)
- Recorridos: inorder, preorder, postorder, level-order
- Árbol AVL (balanceado)
- Árbol rojinegro (Red-Black Tree)
- Árbol B (B-Tree)
- Trie (árbol de prefijos)

### 11.4 Tablas Hash
- Funciones hash: djb2, FNV-1a, MurmurHash3, SipHash
- Manejo de colisiones: encadenamiento (chaining), direccionamiento abierto (linear probing, quadratic probing, double hashing)
- Redimensionamiento y factor de carga
- Implementación genérica con `void *` y destructor

### 11.5 Heaps
- Heap binario (max-heap, min-heap)
- Heapify, push, pop
- HeapSort

### 11.6 Grafos
- Representación: matriz de adyacencia, lista de adyacencia
- Recorridos: BFS, DFS
- Algoritmos: Dijkstra, Bellman-Ford, Floyd-Warshall, Kruskal, Prim

### 11.7 Estructuras de Datos Avanzadas
- Union-Find (Disjoint Set Union)
- Fenwick Tree (Binary Indexed Tree)
- Segment Tree
- Skip List

### 11.8 Ejercicios
- Implementar una lista doblemente enlazada genérica
- Implementar un BST con inserción, búsqueda y eliminación recursiva
- Implementar una tabla hash con sondeo lineal
- Implementar Dijkstra sobre un grafo con lista de adyacencia
- Resolver problemas de plataformas (LeetCode, HackerRank) en C

---

## Capítulo 12 — Concurrencia y Paralelismo

### 12.1 Conceptos Fundamentales
- Procesos vs hilos (threads)
- Concurrencia vs paralelismo
- Cambio de contexto (context switch)
- Condiciones de carrera (race conditions)
- Secciones críticas y exclusión mutua

### 12.2 Hilos POSIX (pthreads)
- `pthread_create`, `pthread_join`, `pthread_detach`, `pthread_exit`
- Atributos de hilos: `pthread_attr_t`
- Paso de argumentos a hilos
- Múltiples hilos y datos compartidos

### 12.3 Sincronización con pthreads
- Mutex: `pthread_mutex_t`, `pthread_mutex_lock`, `pthread_mutex_unlock`
- Mutex recursivos y de error-check
- Variables de condición: `pthread_cond_t`, `pthread_cond_wait`, `pthread_cond_signal`, `pthread_cond_broadcast`
- Barreras: `pthread_barrier_t`
- Spinlocks: `pthread_spinlock_t`
- RW Locks: `pthread_rwlock_t`

### 12.4 Semáforos
- `sem_t`, `sem_init`, `sem_wait`, `sem_post`, `sem_destroy`
- Productor-Consumidor con semáforos
- Problemas clásicos: cena de los filósofos, lectores-escritores, barbero dormilón

### 12.5 Variables Atómicas
- `<stdatomic.h>` (C11): `atomic_int`, `atomic_flag`, `atomic_compare_exchange_weak`, `atomic_fetch_add`
- Memory order: `memory_order_relaxed`, `memory_order_acquire`, `memory_order_release`, `memory_order_acq_rel`, `memory_order_seq_cst`
- Diferencia entre atómico y volátil

### 12.6 Thread-Local Storage
- `_Thread_local` (C11) / `thread_local`
- Variables por hilo vs globales

### 12.7 Hilos en C11 (estándar)
- `<threads.h>`: `thrd_t`, `thrd_create`, `mtx_t`, `cnd_t`
- Limitaciones y adopción (pthreads sigue siendo predominante)

### 12.8 Procesos (POSIX)
- `fork`, `execvp`, `waitpid`
- Tuberías (pipes): `pipe`, comunicación padre-hijo
- Señales: `signal`, `sigaction`, `kill`
- Memoria compartida: `shm_open`, `mmap` con `MAP_SHARED`

### 12.9 Patrones de Concurrencia
- Thread pool
- Work-stealing
- Actor model (simplificado)
- Lock-free programming (casos simples)

### 12.10 Problemas Clásicos de Concurrencia
- Productor-Consumidor (bounded buffer)
- Filósofos comensales (prevención de deadlock)
- Lectores-Escritores (prioridad a lectores o escritores)
- Barbero dormilón

### 12.11 Herramientas de Análisis
- Helgrind (Valgrind): detección de race conditions
- ThreadSanitizer: `-fsanitize=thread`
- GDB con hilos: `info threads`, `thread <n>`

### 12.12 Ejercicios
- Implementar una thread pool genérica
- Resolver el problema de los filósofos con mutex y variables de condición
- Implementar un productor-consumidor thread-safe con buffer circular
- Detectar y corregir race conditions con ThreadSanitizer

---

## Capítulo 13 — Programación de Redes con Sockets

### 13.1 Conceptos de Redes
- Modelo TCP/IP: capas (application, transport, internet, link)
- Protocolos: TCP (orientado a conexión) vs UDP (sin conexión)
- Puertos, direcciones IP (IPv4 e IPv6)
- Endianness: network byte order vs host byte order

### 13.2 Sockets BSD (POSIX)
- `socket()`, `bind()`, `listen()`, `accept()`, `connect()`
- Direcciones: `struct sockaddr_in` (IPv4), `struct sockaddr_in6` (IPv6)
- `inet_pton`, `inet_ntop`
- `htons`, `htonl`, `ntohs`, `ntohl`

### 13.3 Servidor TCP Completo
- Creación del socket
- Bind al puerto
- Listen con backlog
- Aceptación de conexiones
- Comunicación con `send` y `recv`
- Manejo de múltiples clientes (fork, select, poll, epoll, kqueue)

### 13.4 Cliente TCP
- Creación del socket
- Connect al servidor
- Envío y recepción de datos
- Cierre ordenado: `shutdown`

### 13.5 Servidor UDP
- Socket UDP (`SOCK_DGRAM`)
- `sendto`, `recvfrom`
- Servidor echo UDP

### 13.6 E/S Multiplexada
- `select()`: monitorización de múltiples descriptores
- `poll()`: alternativa más escalable
- `epoll` (Linux): E/S basada en eventos
- `kqueue` (BSD/macOS): eventos del kernel
- Comparativa de rendimiento: select vs poll vs epoll vs kqueue

### 13.7 Protocolos de Aplicación
- HTTP/1.1 simplificado (servidor web básico)
- Protocolo binario personalizado (serialización con structs)
- Framing: delimitadores vs prefijo de longitud

### 13.8 Sockets Raw (raw sockets)
- Construcción de paquetes IP/TCP manual
- Sniffing de paquetes
- Limitaciones: permisos de root, portabilidad

### 13.9 SSL/TLS con OpenSSL
- Contexto SSL
- Handshake TLS
- Wrappers seguros para sockets

### 13.10 Ejercicios
- Implementar un servidor echo TCP
- Implementar un chat multicliente con `select()` o `epoll`
- Crear un cliente HTTP que descargue una URL
- Implementar un protocolo binario simple para transferencia de archivos

---

## Capítulo 14 — Temas Avanzados y Bajo Nivel

### 14.1 El Modelo de Memoria en C
- Representación de objetos (object representation)
- Valor de representación vs valor de tipo
- Trap representations
- Endianness: little-endian, big-endian, mixed-endian
- Alineamiento natural, `alignof` y `alignas`

### 14.2 Comportamiento Definido por la Implementación
- Sizeof de tipos, signed representation (two's complement desde C23)
- Orden de evaluación de argumentos
- Bit-fields: orden de bits, signo

### 14.3 Comportamiento Indefinido (Undefined Behavior)
- Fuentes comunes: buffer overflow, signed overflow, uso después de free, double free, violación de strict aliasing, división por cero
- UB != crash: nasal demons, optimizaciones agresivas del compilador
- Herramientas: UBSan (`-fsanitize=undefined`), análisis estático (Clang Static Analyzer, Coverity, Cppcheck)

### 14.4 Strict Aliasing
- Regla estricta de aliasing (C99 §6.5)
- Excepciones: `char *`, `void *`, unions legales
- Escribir código que evite violaciones
- `-fno-strict-aliasing` (parche, no solución)

### 14.5 Atributos y Extensiones del Compilador
- GCC/Clang: `__attribute__((constructor))`, `__attribute__((destructor))`, `__attribute__((unused))`, `__attribute__((noreturn))`
- `__attribute__((format(printf,...)))` para validación de tipos
- `__attribute__((cleanup))` (RAII simulado en C)
- C23: atributos estándar `[[nodiscard]]`, `[[maybe_unused]]`, `[[deprecated]]`, `[[fallthrough]]`, `[[noreturn]]`

### 14.6 Inline Assembly (ASM embebido)
- Sintaxis AT&T vs Intel
- `asm volatile("...")` con GCC
- Restricciones: entrada, salida, clobber
- Ejemplo: punto de medición de ciclos CPU, manipulación de registros de control

### 14.7 Optimización de Código
- Perfilamiento con `perf`, `gprof`
- Optimizaciones del compilador: `-O1`, `-O2`, `-O3`, `-Ofast`, `-Os`
- LTO (Link-Time Optimization)
- Loop unrolling, vectorización automática
- Uso de `restrict` (C99) y `const` para ayudar al compilador
- Cache-friendly code: localidad espacial y temporal
- Branch prediction: `__builtin_expect` (GCC/Clang) → `[[likely]]`/`[[unlikely]]` (C23)

### 14.8 Programación de Sistemas
- Señales POSIX: `signal()`, `sigaction()`, manejadores reentrantes
- `setjmp` / `longjmp`: saltos no locales, co-rutinas simplificadas
- `mmap`, `munmap`, `mprotect`: mapeo de memoria
- Archivos mapeados en memoria (memory-mapped files)
- Creación de daemons y servicios

### 14.9 Interoperabilidad con Otros Lenguajes
- Llamar a C desde Python (ctypes, cffi)
- Llamar a C desde Lua (LuaJIT FFI, C API de Lua)
- Exportar funciones C para otros lenguajes (extern "C" en C++)
- FFI y ABI: calling convention (cdecl, stdcall, fastcall)

### 14.10 C23: Novedades del Estándar
- `#elifdef` y `#elifndef`
- `constexpr` para variables
- `auto` para inferencia de tipos (no confundir con C++)
- `typeof` y `typeof_unqual`
- `bool` como tipo nativo (sin `<stdbool.h>`)
- Atributos estándar `$` en identificadores
- `nullptr` y `nullptr_t`
- `memccpy`, `strdup`, `strndup` ahora estándar
- `_BitInt(N)` para enteros de ancho arbitrario

### 14.11 Seguridad en C
- Buffer overflow: ataques y defensas
- Stack canaries (`-fstack-protector`)
- ASLR y DEP/NX
- Format string vulnerabilities
- Integer overflow y wraparound
- Tooling: `-Wformat=2`, `-Wconversion`, `-Wsign-conversion`

### 14.12 Pruebas y Documentación
- Pruebas unitarias con CTest, CMocka, Unity
- Cobertura: `gcov`, `lcov`
- Documentación: Doxygen (comentarios `/** */`)
- Integración continua (CI) con GitHub Actions o GitLab CI

### 14.13 Ejercicios
- Escribir un detector de fugas de memoria simple (interceptando `malloc`/`free` con macros)
- Implementar una macro `container_of` (como en el kernel Linux)
- Escribir un minuto de medición de rendimiento con inline assembly (RDTSC)
- Encontrar y corregir UB en un código dado usando UBSan
- Implementar un logger thread-safe con atributos de formato printf

---

## Capítulo 15 — Proyecto Final Integrador

### 15.1 Opción A: Intérprete de un Lenguaje Minimalista
- Lexer (tokenización)
- Parser (recursive descent)
- AST (Abstract Syntax Tree)
- Evaluador tree-walking
- REPL interactivo
- Extensiones: variables, funciones, closures

### 15.2 Opción B: Servidor Web Asíncrono
- Parser HTTP/1.1
- Pool de hilos o epoll/kqueue
- Servicio de archivos estáticos
- Soporte para CGI
- Benchmarking y optimización

### 15.3 Opción C: Base de Datos KV (Key-Value Store)
- Almacenamiento en archivo binario
- Índice hash persistente
- Soporte para transacciones simples (WAL - Write-Ahead Log)
- Protocolo de red (TCP)
- Cliente CLI interactivo

### 15.4 Opción D: Simulador de Sistema Operativo Minimalista
- Planificador round-robin
- Gestión de memoria con paginación
- Sistema de archivos simple (FAT-like)
- Llamadas al sistema (syscalls)

### 15.5 Opción E: Compilador de C a C (transpilador)
- Análisis léxico manual
- Parser LL(1) o recursive descent
- Generación de código C optimizado
- Soporte para un subconjunto significativo de C

### 15.6 Entrega y Evaluación
- Código fuente completo con Makefile/CMake
- Documentación interna (Doxygen)
- Pruebas unitarias con cobertura > 80%
- Sin fugas de memoria (Valgrind/ASan clean)
- Sin undefined behavior (UBSan clean)
- README con instrucciones y arquitectura

---

## Capítulo 16 — Cómputo Numérico y Álgebra Lineal

### 16.1 Fundamentos del Cómputo Numérico
- Aritmética de punto flotante: IEEE 754, precisión simple/doble/quad
- Error de redondeo, cancelación catastrófica, condicionamiento
- Épsilon de máquina y algoritmos numéricamente estables
- `<math.h>`: `sin`, `cos`, `tan`, `exp`, `log`, `pow`, `sqrt`, `fma`
- `<tgmath.h>`: macros genéricas de tipo (C99)

### 16.2 Álgebra Lineal con BLAS y LAPACK
- Introducción a BLAS: niveles 1, 2 y 3
- Multiplicación de matrices (GEMM): optimización y cache blocking
- LAPACK: factorización LU, Cholesky, QR, SVD
- Bibliotecas: OpenBLAS, BLIS, Intel MKL, Apple Accelerate
- Implementación manual de `dgemm` optimizado con blocking

### 16.3 Vectores y Matrices en C
- Implementación de struct Vector y Matrix
- Operaciones: suma, producto punto, producto cruz, norma
- Multiplicación matriz-matriz y matriz-vector
- Algoritmo de Strassen (multiplicación de matrices subcúbica)

### 16.4 Resolución de Sistemas Lineales
- Eliminación gaussiana con pivoteo parcial
- Factorización LU
- Eliminación de Gauss-Jordan
- Sistemas tridiagonales (algoritmo de Thomas)
- Métodos iterativos: Jacobi, Gauss-Seidel, SOR

### 16.5 Autovalores y Autovectores
- Método de la potencia y potencia inversa
- Algoritmo QR (sin desplazamiento y con desplazamiento)
- Descomposición en valores singulares (SVD)
- Aplicaciones: PCA, compresión de imágenes

### 16.6 Interpolación y Aproximación
- Interpolación polinomial de Lagrange y Newton
- Splines cúbicos
- Mínimos cuadrados lineales y no lineales
- Aproximación de Chebyshev

### 16.7 Integración y Diferenciación Numérica
- Reglas del rectángulo, trapecio, Simpson
- Cuadratura adaptativa
- Diferenciación numérica: diferencias finitas
- Método de Runge-Kutta para EDOs (RK4)

### 16.8 Transformada Rápida de Fourier (FFT)
- DFT y su implementación ingenua O(n²)
- Algoritmo Cooley-Tukey FFT O(n log n)
- Implementación manual de FFT in-place
- Aplicaciones: convolución, filtrado, correlación

### 16.9 Números Aleatorios y Simulación
- Generadores congruenciales lineales (LCG)
- Mersenne Twister (MT19937)
- `<stdlib.h>`: `rand`, `srand`, límites y sesgos
- Implementación de Xoshiro256** (estado del arte)
- Distribuciones: uniforme, normal (Box-Muller), exponencial

### 16.10 Precisión Arbitraria (BigNum)
- Representación de enteros grandes (arrays de `uint32_t`/`uint64_t`)
- Suma, resta, multiplicación (Karatsuba, Toom-Cook, FFT)
- División y módulo (algoritmo de D. Knuth)
- Exponenciación modular (exponentiation by squaring)
- Algoritmo de Montgomery para multiplicación modular
- Implementación de `mpz_t` simplificado (como GMP)

### 16.11 GNU MPFR y GMP
- Uso de GMP para enteros y racionales de precisión arbitraria
- MPFR para punto flotante con redondeo controlado
- Comparación con implementación manual

### 16.12 Ejercicios
- Implementar multiplicación de matrices con y sin cache blocking (comparar rendimiento)
- Escribir una función que resuelva un sistema 3x3 con eliminación gaussiana
- Implementar la FFT de Cooley-Tukey desde cero
- Calcular π con precisión arbitraria usando la serie de Machin
- Implementar un generador Xoshiro256** y pasar tests estadísticos

---

## Capítulo 17 — Procesamiento de Señales y Multimedia

### 17.1 Fundamentos de Procesamiento Digital de Señales
- Muestreo y teorema de Nyquist-Shannon
- Aliasing y antialiasing
- Cuantización y relación señal/ruido (SNR)
- Señales en tiempo discreto: convolución, correlación

### 17.2 Filtros Digitales
- FIR (Finite Impulse Response): ventanas, implementación directa
- IIR (Infinite Impulse Response): Butterworth, Chebyshev, elíptico
- Transformada Z y función de transferencia
- Implementación en C: estructuras directas, cascada, paralelo

### 17.3 Procesamiento de Audio
- Formato WAV: cabecera, muestras PCM
- Lectura/escritura de archivos WAV en C
- Efectos: eco, reverberación, chorus, flanger, wah-wah
- Ecualizador paramétrico con filtros IIR
- Síntesis: osciladores (seno, diente de sierra, cuadrado), envolventes ADSR
- Librerías: libsndfile, PortAudio, SDL_audio

### 17.4 Procesamiento de Imágenes
- Formatos: PPM/PGM (simple), BMP, PNG (libpng), JPEG (libjpeg)
- Representación: matrices de píxeles, canales RGB/YUV
- Operaciones puntuales: brillo, contraste, gamma, ecualización de histograma
- Convolución 2D: desenfoque gaussiano, detección de bordes (Sobel, Canny)
- Filtros morfológicos: erosión, dilatación, opening, closing
- Transformaciones geométricas: escalado (bilineal, bicúbico), rotación, affine
- Compresión JPEG por bloques DCT

### 17.5 Procesamiento de Video
- Formatos y códecs: contenedores AVI/MKV, códecs H.264/H.265
- FFmpeg como biblioteca (libavcodec, libavformat)
- Captura de video con V4L2 (Linux)
- Motion estimation y compensación de movimiento
- Algoritmos de tracking: Mean-Shift, CamShift, Optical Flow (Lucas-Kanade)

### 17.6 Compresión de Datos
- Algoritmos sin pérdida: Run-Length Encoding (RLE), Huffman, LZ77, LZ78
- Algoritmo LZW: compresión de texto e imágenes GIF
- Deflate (ZIP/gzip): LZ77 + Huffman
- Implementación completa de Huffman: tabla de frecuencias, árbol, codificación
- Compresión con pérdida: DCT + cuantización (JPEG simplificado)
- Bibliotecas: zlib, bzip2, lz4, zstd (Zstandard)

### 17.7 Señales Biomédicas
- ECG: detección de picos R, filtrado de línea base
- EEG: eliminación de artefactos, análisis espectral
- Algoritmo de Pan-Tompkins para detección de QRS

### 17.8 Librerías de Audio/Video en C
- libsndfile (audio), PortAudio (streaming), SDL2 (multimedia)
- GStreamer: pipelines multimedia, plugins en C
- OpenAL: audio posicional 3D

### 17.9 Ejercicios
- Implementar un filtro FIR paso bajo y aplicarlo a una señal WAV
- Escribir un ecualizador gráfico de 10 bandas con filtros IIR
- Implementar la detección de bordes Sobel en una imagen PGM
- Crear un compresor/decompresor de Huffman para archivos de texto
- Leer y reproducir un archivo WAV con PortAudio

---

## Capítulo 18 — Criptografía y Seguridad Informática

### 18.1 Fundamentos Criptográficos
- Criptografía simétrica vs asimétrica vs hashing
- Confusión, difusión y principio de Kerckhoffs
- Números primos, aritmética modular y logaritmo discreto
- Curvas elípticas: conceptos básicos, suma de puntos, multiplicación escalar

### 18.2 Cifrado Simétrico
- XOR como cifrado base (one-time pad)
- AES: estructura, S-box, ShiftRows, MixColumns, AddRoundKey
- Implementación de AES-128 desde cero (ECB, CBC, CTR, GCM)
- ChaCha20: cifrado de flujo moderno (seguro, rápido, sin hardware acceleration)
- Modos de operación: ECB (inseguro), CBC, CFB, OFB, CTR, GCM, XTS

### 18.3 Cifrado Asimétrico
- RSA: generación de claves, cifrado, descifrado, firma
- Diffie-Hellman: intercambio de claves
- Curvas Elípticas: ECDH, ECDSA, Ed25519
- Cifrado híbrido: ECIES, RSA-OAEP + AES-GCM

### 18.4 Funciones Hash
- SHA-1, SHA-256, SHA-3: estructura y rondas
- MD5 (roto), SHA-1 (debilitado), SHA-2 (seguro), SHA-3 (estándar)
- BLAKE2 y BLAKE3: hashing ultrarrápido
- HMAC: hash-based message authentication code
- Ataques: colisiones (birthday attack), length extension

### 18.5 Criptografía de Curva Elíptica
- Curvas: secp256k1 (Bitcoin), Curve25519, P-256
- Firma EdDSA con Ed25519
- Acuerdo de claves X25519
- Implementación de operaciones de curva elíptica en C

### 18.6 Autenticación y Certificados
- X.509: certificados, CA, chain of trust
- TLS 1.3: handshake, key exchange, cipher suites
- OpenSSL: EVP (encriptación de alto nivel), BIO, SSL_*

### 18.7 Código Constante en Tiempo (Constant-Time)
- Side-channel attacks: timing, cache, power analysis
- Operaciones constant-time: comparación, selección, intercambio
- Implementación de memcmp constante-tiempo
- Evitar branching con datos secretos
- `ctgrind` y herramientas de verificación constant-time

### 18.8 Generación Segura de Números Aleatorios
- `/dev/urandom`, `getrandom()` (Linux), `BCryptGenRandom` (Windows)
- Diferencias con `rand()`: impredecibilidad, propósitos criptográficos
- Implementación de DRBG (Deterministic Random Bit Generator) basado en hash

### 18.9 Ataques y Defensas
- Buffer overflow: shellcode, NOP sled, ASLR, stack canaries, NX
- Format string vulnerability: leer/escribir memoria arbitraria
- Integer overflow: wraparound, promoción, conversión de signo
- Use-after-free: heap exploitation técnicas (tcache poisoning, fastbin attack)
- Backdoor detection: análisis de código, hashing de binarios
- Fuzzing: AFL, libFuzzer, Honggfuzz

### 18.10 Aplicaciones Criptográficas
- Cifrado de archivos (openssl enc simplificado)
- Firma digital y verificación
- Protocolo de autenticación desafío-respuesta
- Password hashing: bcrypt, scrypt, argon2 (comparativa e implementación)

### 18.11 Librerías Criptográficas
- OpenSSL / LibreSSL
- libsodium (recomendada: fácil, segura, moderna)
- mbedTLS (embebido)
- Comparativa: API, seguridad, rendimiento, licencia

### 18.12 Ejercicios
- Implementar AES-128 en modo CBC desde cero
- Escribir una función de comparación constant-time
- Implementar HMAC-SHA256
- Crear una herramienta de cifrado/descifrado de archivos con libsodium
- Detectar y explotar (en sandbox) una vulnerabilidad format string

---

## Capítulo 19 — Desarrollo de Compiladores e Intérpretes

### 19.1 Arquitectura de un Compilador
- Fases: análisis léxico → sintáctico → semántico → IR → optimización → codegen
- Frontend vs backend vs middle-end
- Herramientas: lex/flex, yacc/bison (vs implementación manual)

### 19.2 Análisis Léxico (Lexer)
- Expresiones regulares y autómatas finitos deterministas (DFA)
- Implementación manual de un lexer: tabla de transiciones, tokenización
- Manejo de palabras reservadas, identificadores, literales, operadores
- Flex: especificación de patrones y generación de lexer

### 19.3 Análisis Sintáctico (Parser)
- Gramáticas libres de contexto (CFG) y BNF
- Recursive descent: fácil, manual, expresivo (para LL gramáticas)
- LALR(1) con yacc/bison: parsing table, shift/reduce, reduce/reduce
- Manejo de errores sintácticos: panic mode, error recovery
- Implementación manual de un parser LL(1) con tabla de parsing

### 19.4 Árbol de Sintaxis Abstracta (AST)
- Diseño de nodos AST con structs y uniones etiquetadas
- Construcción del AST durante el parsing
- Pretty-printer para el AST

### 19.5 Análisis Semántico
- Tabla de símbolos: implementación con tabla hash o árbol
- Scope: anidamiento, resolución de nombres
- Type checking: inferencia básica, verificación de tipos
- Detección de errores semánticos: variables no declaradas, tipos incompatibles

### 19.6 Generación de Código Intermedio (IR)
- Three-address code (TAC): struct con operación, src1, src2, dst
- SSA (Static Single Assignment): dominadores, φ-functions
- Representación linear (IR lineal) vs tree-based
- De AST a IR: traducción de expresiones, sentencias, funciones

### 19.7 Optimización de Código
- Constant folding y propagation
- Dead code elimination
- Common subexpression elimination (CSE)
- Loop-invariant code motion (LICM)
- Strength reduction
- Inlining y tail-call optimization
- Dataflow analysis: reaching definitions, live variables, available expressions

### 19.8 Generación de Código (Codegen)
- Generación de código C (transpilador): C → C (útil como backend portable)
- Generación de código x86-64: selección de instrucciones, asignación de registros
- Algoritmo de coloración de grafos para register allocation
- Calling convention: x86-64 System V ABI, Windows x64

### 19.9 Linker y Cargador
- Relocalización, resolución de símbolos
- Formato ELF: secciones, segmentos, tabla de símbolos
- Linker manual vs GNU ld

### 19.10 Intérpretes Tree-Walking
- AST Walk: evaluar nodo por nodo
- Entorno de ejecución: tabla de símbolos en runtime
- Ámbito léxico (closures) vs ámbito dinámico
- REPL: read-eval-print loop

### 19.11 Intérpretes de Bytecode
- Diseño de bytecode: conjunto de instrucciones, operandos
- Compilación a bytecode (del AST)
- Virtual machine: dispatcher (switch-based, threaded code)
- Stack-based VM vs register-based VM

### 19.12 Compilación Just-In-Time (JIT)
- Diferencias entre JIT y compilación anticipada (AOT)
- Tiny JIT: generar código máquina en memoria ejecutable (`mmap` + `PROT_EXEC`)
- DynASM (LuaJIT): generación de código máquina con macros de ensamblador
- Tree de LuaJIT: tracing JIT, SSA, IR, codegen

### 19.13 Herramientas de Generación
- Lex/Flex + Yacc/Bison
- ANTLR (multi-lenguaje, LL(*))
- Tree-sitter: parser incremental para IDEs

### 19.14 Ejercicios
- Implementar un lexer completo para un subconjunto de C
- Construir un parser recursive descent para expresiones aritméticas
- Escribir un intérprete tree-walking para Brainfuck
- Crear un pequeño compilador de C a x86-64 (solo expresiones enteras)

---

## Capítulo 20 — Sistemas Embebidos y Microcontroladores

### 20.1 Fundamentos de Sistemas Embebidos
- Diferencias: PC vs microcontrolador (MCU)
- Arquitecturas: ARM Cortex-M, AVR, RISC-V, ESP32
- Memoria: flash (código), SRAM (datos), EEPROM, registros
- Periféricos: GPIO, UART, SPI, I2C, TIM, ADC, DAC, DMA
- Herramientas: compilador cruzado (cross-compiler), linker script, debugger (JTAG/SWD)

### 20.2 Entorno de Desarrollo Embebido
- ARM GCC: `arm-none-eabi-gcc`, `arm-none-eabi-gdb`
- CMake para embebido: toolchain file, target triple
- OpenOCD: debug interface y flashing
- IDEs: STM32CubeIDE, PlatformIO, VS Code + Cortex-Debug

### 20.3 GPIO (General Purpose Input/Output)
- Configuración de pines: input, output, pull-up, pull-down, open-drain
- Registros de control: MODER, ODR, IDR, BSRR, PUPDR
- Blinking LED: el "Hello World" embebido
- Debounce de botones: software (temporización) y hardware (RC filter)

### 20.4 Interrupciones (Interrupts)
- NVIC (Nested Vectored Interrupt Controller): prioridad, anidamiento
- Vector de interrupciones: tabla en flash
- ISR (Interrupt Service Routine): escritura, restricciones
- Interrupciones externas (EXTI)
- Factores críticos: latencia, atomicidad, `volatile` en variables compartidas

### 20.5 Timers y PWM
- Timers básicos: modo contador, modo de captura/compare
- PWM (Pulse Width Modulation): control de motores, LEDs, servos
- Medición de frecuencia y periodo con input capture
- Watchdog timer: prevención de hangs (IWDG, WWDG)

### 20.6 Comunicación Serial
- UART/USART: transmisión asíncrona, baud rate, frame
- Protocolo: start bit, data bits, parity, stop bit
- Implementación de driver UART con interrupciones y DMA
- I2C/TWI: protocolo, direccionamiento, multi-maestro, clock stretching
- SPI: full-duplex, modos de polaridad/fase, encadenamiento de dispositivos
- Diferencias: UART vs I2C vs SPI (velocidad, pines, topología)

### 20.7 ADC y DAC
- ADC: resolución, referencia, tiempo de conversión, modos (single, continuous, injected)
- Conversión analógico-digital con disparo por timer
- DAC: generación de señales analógicas, forma de onda arbitraria
- Filtrado suavizado: media móvil, filtro de Kalman simple

### 20.8 DMA (Direct Memory Access)
- Copia de datos sin intervención de la CPU
- Configuración: fuente, destino, tamaño, modo circular, incremento
- Uso típico: ADC → RAM, RAM → UART, SPI → RAM
- Interrupciones de DMA: transferencia completa, half-transfer, error

### 20.9 RTOS (Real-Time Operating Systems)
- Conceptos: tareas, scheduling (prioridad, Round-Robin), ticks
- FreeRTOS: `xTaskCreate`, `vTaskDelay`, `xQueueSend`, `xSemaphoreTake`
- Sincronización: queues, semáforos binarios y contadores, mutex, event groups
- Problemas: priority inversion, starvation, deadlock
- Medir tiempo real: `xTaskGetTickCount`, `configTICK_RATE_HZ`
- Alternativas: Zephyr, ThreadX, embOS, Mbed OS

### 20.10 Bajo Consumo de Energía
- Modos de sleep: sleep, stop, standby
- Wakeup: interrupción externa, RTC, WDT
- Medición de consumo y optimización
- Aplicaciones: IoT con batería, sensores remotos

### 20.11 Firmware Seguro
- Secure boot, CRCs, firmado de firmware
- Protección de memoria: MPU (Memory Protection Unit)
- Actualización OTA (Over-The-Air): bootloader dual bank
- Anti-rollback, enclave seguro (TrustZone)

### 20.12 RISC-V: Arquitectura Abierta
- ISA base: RV32I, RV64I, extensiones M/A/F/D/C
- Comparativa con ARM Cortex-M
- Herramientas: GCC para RISC-V, QEMU (simulación)
- Ejemplo: SiFive HiFive1, ESP32-C3, CH32V

### 20.13 Ejercicios
- Configurar GPIO y timer para hacer un LED PWM (atenuación suave)
- Leer un sensor de temperatura I2C (ej. LM75, DHT22) y mostrar por UART
- Implementar un scheduler cooperativo sin RTOS
- Crear un shell minimalista por UART (CLI embebido)
- Portar FreeRTOS para un microcontrolador y crear 3 tareas concurrentes

---

## Capítulo 21 — Computación de Alto Rendimiento (HPC)

### 21.1 Arquitectura de Computadoras Modernas
- Memoria: jerarquía (L1/L2/L3 cache, DRAM, NUMA)
- Procesamiento vectorial: SIMD, AVX-512, SVE
- Procesamiento paralelo: multicore, many-core, GPGPU, FPGA
- Medición de rendimiento: FLOPS, throughput, latencia, bandwidth

### 21.2 Optimización de Código para HPC
- Memory layout: AoS vs SoA
- Cache blocking (tiling) para bucles anidados
- Prefetching: `__builtin_prefetch` (GCC) y hints de hardware
- Loop unrolling y pragmas de vectorización
- Alineamiento de datos y allocación alineada (`posix_memalign`, `aligned_alloc`)
- `restrict` para desambiguación de punteros
- Profile-guided optimization (PGO): `-fprofile-generate`, `-fprofile-use`

### 21.3 SIMD (Single Instruction, Multiple Data)
- Intrinsics de SSE/AVX/AVX-512 (x86) y NEON (ARM)
- Carga/almacenamiento: `_mm_load_ps`, `_mm_store_ps`
- Aritmética vectorial: `_mm_add_ps`, `_mm_mul_ps`, `_mm_fmadd_ps`
- Reducción horizontal: `_mm_hadd_ps`, `_mm_sum_ps`
- Ejemplo: normalización de vectores, producto de matrices con SIMD
- Auto-vectorización: ayudar al compilador con pragmas y patrones

### 21.4 OpenMP
- Directivas: `#pragma omp parallel`, `#pragma omp for`, `#pragma omp parallel for`
- Scheduling: `static`, `dynamic`, `guided`, `auto`, `runtime`
- Reducciones: `#pragma omp parallel for reduction(+:sum)`
- Secciones y tasks: `#pragma omp sections`, `#pragma omp task`
- Sincronización: `critical`, `atomic`, `barrier`, `lock`
- Control de afinidad: `OMP_PLACES`, `OMP_PROC_BIND`
- Ejemplo: multiplicación de matrices paralelizada, integración numérica

### 21.5 MPI (Message Passing Interface)
- Conceptos: communicator, rank, size
- Punto a punto: `MPI_Send`, `MPI_Recv`, `MPI_Isend`, `MPI_Irecv`
- Colectivas: `MPI_Bcast`, `MPI_Reduce`, `MPI_Allreduce`, `MPI_Scatter`, `MPI_Gather`
- Tipos de datos derivados: `MPI_Type_create_struct`
- Comunicadores y topologías virtuales: `MPI_Cart_create`
- Patrones: maestro-esclavo, pipeline, descomposición de dominio
- Ejemplo: multiplicación de matrices paralela con MPI (distribuida)

### 21.6 GPGPU con OpenCL y CUDA desde C
- Modelo de ejecución: host + device, kernels, work-items, work-groups
- OpenCL: plataforma, contexto, cola de comandos, kernel
- CUDA (desde C con llamadas al driver): `cuModuleLoad`, `cuLaunchKernel`
- Memoria: global, compartida, local, constante, registros
- Optimización: coalescing de memoria, occupancy, bank conflicts
- Ejemplo: SAXPY, reducción paralela, multiplicación de matrices en GPU

### 21.7 Programación Heterogénea
- SYCL (Khronos): programación heterogénea C++-like
- HIP (AMD): traducción de CUDA a GPU AMD
- OneAPI (Intel): DPC++ para CPU/GPU/FPGA
- OpenCL + MPI: paralelismo distribuido con aceleración GPU

### 21.8 Análisis de Rendimiento
- `perf` (Linux): contadores de hardware, profiling por muestreo
- `gprof`: profiling por instrumentación
- Valgrind --tool=callgrind: simulación de caché, profiling de instrucciones
- Intel VTune, AMD uProf, ARM MAP
- Roofline model: análisis de bottleneck computacional vs de memoria

### 21.9 HPC en la Nube
- Slurm: job scheduling, scripts de envío
- Singularity/Apptainer: contenedores para HPC
- AWS ParallelCluster, Azure CycleCloud, GCP Batch

### 21.10 Ejercicios
- Optimizar multiplicación de matrices con cache blocking y SIMD
- Paralelizar con OpenMP un solver de ecuaciones diferenciales
- Escribir un programa MPI para sumar un array gigante en paralelo
- Implementar un kernel de reducción en OpenCL
- Usar `perf` para identificar bottlenecks y mejorar rendimiento

---

## Capítulo 22 — Desarrollo de Juegos en C

### 22.1 Arquitectura de un Videojuego
- Game loop: update (fijo) + render (variable), fixed timestep
- Entrada: teclado, ratón, gamepad (SDL_GameController)
- Física: gravedad, colisiones AABB, circle-circle, raycasting
- ECS (Entity-Component-System): diseño de datos orientado a caché
- Serialización de estado del juego (guardado/carga)

### 22.2 Gráficos 2D con SDL2
- Inicialización: `SDL_Init`, `SDL_CreateWindow`, `SDL_CreateRenderer`
- Sprites: `SDL_Texture`, `SDL_Surface`, carga de BMP/PNG (SDL_image)
- Animación: sprite sheets, frame timing
- Tile maps: editor de niveles, renderizado por tiles, culling
- Cámaras: scroll, follow, parallax
- Partículas: sistemas de partículas, emisores, físicas simples
- Texto: SDL_ttf (TrueType Fonts)

### 22.3 Gráficos 3D con OpenGL desde C
- Pipeline gráfico moderno: shaders, VAO, VBO, EBO
- GLAD/GLFW: inicialización de OpenGL en C
- Shaders GLSL: vertex, fragment, geometry, compute
- Transformaciones: matrices de modelo, vista, proyección (perspectiva, ortográfica)
- Iluminación: Phong, Blinn-Phong, PBR (Physically Based Rendering)
- Mapeado: texturas, normal maps, specular maps, shadow maps
- Modelos: carga de OBJ/glTF, jerarquía de nodos, animación esquelética
- Optimización: frustum culling, LOD, instancing, occlusion queries

### 22.4 Física del Juego
- Colisiones: SAT (Separating Axis Theorem), GJK, EPA
- Dinámica de cuerpos rígidos: fuerzas, torque, resolución de restricciones
- Motores de física: integración con Bullet Physics (C API)
- Detección de colisiones espacial: grid, quad-tree, BVH

### 22.5 Sonido y Música
- SDL_mixer: carga y reproducción de WAV/OGG/MP3, canales, mezcla
- 3D audio: atenuación por distancia, paneo estéreo
- Streaming de audio: buffers circulares, callback-based

### 22.6 Redes para Juegos
- Cliente-servidor vs peer-to-peer
- Serialización de estado del juego (snapshots)
- Interpolación, predicción, reconciliación
- Lag compensation: rewinding, extrapolación
- Conexión: TCP vs UDP (ENet, RakNet)

### 22.7 Game Engines en C
- Raylib: minimalista, ideal para aprendizaje y prototipado
- Godot: scripting en C mediante GDNative/GDExtension
- Custom engine: arquitectura de componentes, editor, scripting

### 22.8 Retro y Demoscene
- Limitaciones: 64KB, 4KB, 1KB intros, bytebeat
- Mode 13h (VGA 320x200x256): gráficos en modo real-02
- Efectos clásicos: rotozoom, fire effect, plasma, tunnel, starfield
- Sincronización con música: tracker music (MOD/S3M/XM/IT)

### 22.9 Ejercicios
- Crear un Pong completo con SDL2 y físicas simples
- Implementar un tile map renderer con cámara de scroll
- Escribir un shader GLSL simple y usarlo desde C con OpenGL
- Desarrollar un juego tipo Snake con ECS
- Hacer un pequeño juego multiplayer (2 jugadores) sobre UDP con ENet

---

## Capítulo 23 — Programación de Kernel y Módulos Linux

### 23.1 Arquitectura del Kernel Linux
- Monolítico modular: kernel space vs user space
- System calls: interface, implementación, tabla de syscalls
- VFS (Virtual File System): inodes, dentries, superblocks
- Process scheduler: CFS (Completely Fair Scheduler), prioridades, grupos
- Gestión de memoria del kernel: slab allocator, vmalloc, kmalloc, page allocator

### 23.2 Configuración del Entorno
- Compilación del kernel desde fuente: `make menuconfig`, `make -j$(nproc)`
- Módulos: `modprobe`, `insmod`, `rmmod`, `lsmod`, `modinfo`
- QEMU + Busybox: kernel en máquina virtual para desarrollo seguro
- Depuración: `printk`, `kgdb`, QEMU + GDB, `kprobes`, `tracepoints`

### 23.3 Escritura de Módulos del Kernel
- Estructura: `module_init`, `module_exit`, licencia (`MODULE_LICENSE`)
- Makefile para módulos: `obj-m += modulo.o`, `KERNELDIR`
- Parámetros del módulo: `module_param`, `module_param_array`
- Archivos en `/proc` y `/sys`: `proc_create`, `kobject_create`
- Comunicación con userspace: `copy_to_user`, `copy_from_user`

### 23.4 Drivers de Dispositivo
- Dispositivos de caracter: `struct file_operations`, `cdev_add`
- Dispositivos de bloque: `struct block_device_operations`, request queue
- Dispositivos de bus: platform driver, device tree, PCI, USB
- Interrupciones en drivers: `request_irq`, `tasklet`, `workqueue`, threaded IRQ
- DMA en drivers: `dma_alloc_coherent`, streaming DMA mapping

### 23.5 System Calls
- Implementación de una syscall personalizada
- `SYSCALL_DEFINE`, tabla de syscalls (x86: `arch/x86/entry/syscalls/syscall_64.tbl`)
- Verificación de parámetros desde kernel: `access_ok`, copia segura
- Buenas prácticas: no bloquear, no dormir en spinlocks, manejar señales

### 23.6 Sincronización en el Kernel
- Spinlocks: `spin_lock`, `spin_lock_irqsave`, `spin_lock_bh`
- Mutex del kernel: `struct mutex`, `mutex_lock_interruptible`
- Semáforos: `struct semaphore`
- Read-Copy-Update (RCU): `rcu_read_lock`, `synchronize_rcu`, `call_rcu`
- Wait queues: `wait_event_interruptible`, `wake_up_interruptible`
- Completions: `struct completion`

### 23.7 Gestión de Memoria del Kernel
- Asignación: `kmalloc`, `kzalloc`, `vmalloc`, `__get_free_pages`
- Slab allocator: `kmem_cache_create`, `kmem_cache_alloc`
- Memory pools: `mempool_create`
- I/O memory: `ioremap`, `readl/writel`, `devm_ioremap_resource`

### 23.8 El Modelo de Dispositivos Linux
- `struct device`, `struct device_driver`, `struct bus_type`
- Device tree: `.dts`, `compatible`, reg, interrupts
- Power management: runtime PM, system suspend/resume
- Regmap: abstracción de registro para buses (I2C, SPI, MMIO)

### 23.9 Network Stack en el Kernel
- Socket kernel: `struct sock`, `struct sk_buff`
- Netfilter: hooks, `nf_register_hook`, NAT, firewall
- Módulos de red: implementación de un protocolo de transporte simple
- eXpress Data Path (XDP): procesamiento de paquetes en la NIC

### 23.10 eBPF (Extended Berkeley Packet Filter)
- Concepto: VM en el kernel, verificación de seguridad, JIT
- Programas eBPF: `BPF_PROG_TYPE_KPROBE`, `BPF_PROG_TYPE_TRACEPOINT`
- Maps: hash, array, LRU, ring buffer
- BCC y bpftrace: herramientas de tracing dinámico
- XDP con eBPF: DDoS mitigation, load balancing, packet filtering

### 23.11 Seguridad del Kernel
- SELinux, AppArmor, Smack: LSM (Linux Security Modules)
- KASLR (Kernel Address Space Layout Randomization)
- Stack canaries, `CONFIG_STACKPROTECTOR`
- Control Flow Integrity (CFI) y Shadow Call Stack
- Kernel Self-Protection: `CONFIG_HARDENED_USERCOPY`

### 23.12 Ejercicios
- Escribir un módulo "Hello World" y cargarlo/descargarlo
- Implementar un driver de carácter que exponga un buffer circular
- Añadir una syscall personalizada que devuelva el tiempo de actividad
- Crear un módulo de netfilter que registre paquetes ICMP
- Escribir un programa eBPF que cuente syscalls por proceso

---

## Capítulo 24 — WebAssembly y Emscripten

### 24.1 Introducción a WebAssembly
- WASM: formato binario de instrucciones, máquina virtual basada en stack
- Diferencias con asm.js, JavaScript, Native Client (NaCl)
- Casos de uso: juegos, procesamiento multimedia, computación científica, CDN edge
- Herramientas: Emscripten, WASI SDK, wabt (WAT2WASM), wasmtime

### 24.2 Compilación de C a WASM con Emscripten
- Instalación de Emscripten SDK: `emsdk install`, `emsdk activate`
- Compilación básica: `emcc hola.c -o hola.html`
- Flags: `-O3`, `-s WASM=1`, `-s EXPORTED_FUNCTIONS`, `-s ALLOW_MEMORY_GROWTH`
- Salidas: `.wasm` (binario), `.js` (glue code), `.html` (página demo)

### 24.3 Interoperabilidad C ↔ JavaScript
- Llamar funciones C desde JS: `Module._funcion()`, `ccall`, `cwrap`
- Pasar arrays: `Module.HEAPU8`, `Module.HEAPF32`, punteros
- Llamar funciones JS desde C: `EMSCRIPTEN_KEEPALIVE`, `EM_JS`, `EM_ASM`
- Callbacks: `addFunction`, `removeFunction`
- Promesas y async: `EM_PROMISE`, `Asyncify`

### 24.4 Manipulación de Memoria WASM
- Layout de memoria: stack, heap, data segments
- Punteros: `malloc`, `free` desde WASM
- Pasar buffers grandes: pre-allocar, compartir vista de `ArrayBuffer`
- Memory growth y límites: `-s INITIAL_MEMORY`, `-s MAXIMUM_MEMORY`

### 24.5 Optimización para WebAssembly
- Tamaño del binario: `-Os`, `-Oz`, `wasm-opt` (Binaryen), tree shaking
- Velocidad: WASM es generalmente 1.2-2× más lento que nativo (sin SIMD)
- WASM SIMD: `-msimd128`, intrinsics portátiles (wasm_simd128.h)
- Multithreading: SharedArrayBuffer, `-s pthread`, Web Workers
- File system virtual: `--preload-file`, MEMFS, NODEFS, IDBFS

### 24.6 WASI (WebAssembly System Interface)
- Interfaz estándar para WASM fuera del navegador
- `wasmtime`, `wasmer`, `wasm3` como runtimes
- Compilar con `wasm32-wasi` target: `clang --target=wasm32-wasi`
- Acceso a sistema de archivos, args, entorno, clocks

### 24.7 Aplicaciones Prácticas
- Portar un emulador (ej. Doom, FFmpeg, SQLite, Lua) al navegador
- Procesamiento de imágenes/video en el cliente con WASM
- WebAssembly en edge computing: Cloudflare Workers, Fastly Compute@Edge
- Plugins WASM: extensiones seguras para aplicaciones

### 24.8 Rendimiento y Benchmarks
- Comparativa WASM vs JS nativo vs asm.js
- PolyBench/C, SPEC, CoreMark en WASM
- Perfilamiento en navegador: Chrome DevTools, Firefox Profiler

### 24.9 WAT: Formato Textual de WebAssembly
- Sintaxis S-expression: `(module (func ...) (export ...))`
- Herramientas: `wat2wasm`, `wasm2wat`, `wasm-objdump`
- Escribir módulos WAT a mano para máximo control

### 24.10 Ejercicios
- Compilar un programa C simple a WASM y llamarlo desde una página HTML
- Portar un algoritmo de procesamiento de imágenes (ej. filtro sepia) y comparar velocidad contra JS puro
- Crear un módulo WASM que exponga funciones para multiplicar matrices
- Ejecutar un programa WASI con wasmtime desde la terminal

---

## Capítulo 25 — Análisis Estático y Verificación Formal

### 25.1 Introducción al Análisis Estático
- Análisis estático vs dinámico: ventajas y limitaciones
- Tipos: linting, dataflow analysis, symbolic execution, abstract interpretation
- Falsos positivos vs falsos negativos
- Soundness y completitud (trade-off)

### 25.2 Linting y Análisis de Código
- GCC/Clang: `-Wall -Wextra -Wpedantic -Wconversion -Wsign-conversion -Wformat=2`
- Clang-Tidy: `clang-tidy`, checks, configuración con `.clang-tidy`
- Cppcheck: análisis de fuga de memoria, desbordamiento, estilo
- PVS-Studio: análisis comercial, detección de bugs sutiles
- SonarQube: plataforma de calidad, integración CI, reglas para C

### 25.3 Análisis de Flujo de Datos (Dataflow Analysis)
- Reaching definitions, live variables, available expressions
- Monotone framework: lattice, transfer functions, fixed-point iteration
- Implementación de un dataflow analyzer para un IR simple
- Aplicaciones: detección de variables no inicializadas, dead code

### 25.4 Symbolic Execution
- Modelo: ejecución con símbolos en lugar de valores concretos
- Path explosion y soluciones: pruning, merging, heuristics
- KLEE: symbolic execution engine para C y LLVM bitcode
- Angr: framework de análisis binario con symbolic execution
- Aplicaciones: generación automática de test cases, detección de bugs

### 25.5 Frama-C y ACSL
- ACSL (ANSI/ISO C Specification Language): contratos para C
  - `requires`, `ensures`, `assigns`, `behavior`
  - `assert`, `loop invariant`, `variant`
  - `ghost` variables, axiomatics, lemmas
- Frama-C: plataforma de análisis de código C
  - WP (Weakest Precondition): verificación deductiva
  - E-ACSL: runtime assertion checking compilado
  - AoraL: análisis de concurrencia
  - Eva (ex Value): abstract interpretation para valores posibles
- Ejemplo: probar que `memcpy` no produce buffer overflow

### 25.6 Abstract Interpretation
- Teoría: lattice, concretización, abstracción, widening
- Polyhedral model: análisis de bucles y acceso a arrays
- Ejemplos: Astrée (Airbus, sin falsos positivos para código embarcado crítico)
- Aplicación: detección de overflow, división por cero, out-of-bounds

### 25.7 Model Checking
- SPIN: verificación de sistemas concurrentes con Promela
- CBMC (C Bounded Model Checker): verificación de programas C
  - Unwinding de bucles, SAT/SMT solving
  - Verificación de aserciones, array bounds, pointer safety
- SMACK: verificación de programas C (traducción a Boogie)
- Herramientas: NuSMV, CADP, TLA+ (para especificación)

### 25.8 Verificación Deductiva
- Why3: plataforma de verificación con múltiples backends (Alt-Ergo, Z3, CVC4/5)
- Creación de contratos para funciones C en Why3
- Prueba de programas con y sin bucles (invariants)
- Aplicación: verificación de algoritmos de ordenación, búsqueda

### 25.9 Análisis Dinámico Asistido
- AddressSanitizer (ASan): detección de errores de memoria
- UndefinedBehaviorSanitizer (UBSan): UB en runtime
- MemorySanitizer (MSan): uso de memoria no inicializada
- LeakSanitizer (LSan): detección de memory leaks
- ThreadSanitizer (TSan): data races

### 25.10 Análisis de Programas Concurrentes
- Model checking de concurrencia: detección de deadlocks y race conditions
- SPIN + Promela: verificación de protocolos de sincronización
- Happens-before analysis y vectores de reloj
- Herramientas: Helgrind (Valgrind), ThreadSanitizer, RELAY

### 25.11 Certificación de Código Crítico
- DO-178C (aviónica): niveles DAL A-E, objetivos de verificación
- IEC 61508 (industrial): SIL 1-4, cobertura y pruebas
- ISO 26262 (automoción): ASIL A-D, freedom from interference
- Misra C: guidelines para código seguro en sistemas críticos
- CERT C Coding Standard: reglas para seguridad y robustez

### 25.12 Ejercicios
- Configurar Clang-Tidy con un conjunto de checks estrictos en un proyecto
- Escribir un contrato ACSL para una función binaria (búsqueda, ordenación)
- Usar Frama-C (Eva) para probar que `strcpy` nunca desborda un buffer
- Detectar un bug sutil en un código dado con CBMC
- Analizar un programa concurrente con SPIN para verificar ausencia de deadlock

---

## Capítulo 26 — Machine Learning e Inteligencia Artificial en C

### 26.1 Fundamentos de Machine Learning
- Paradigmas: supervisado, no supervisado, semi-supervisado, refuerzo
- Componentes: modelo, función de pérdida, optimizador, datos
- El problema de la implementación en C: velocidad, control de memoria, portabilidad
- Comparativa: C vs Python (NumPy, PyTorch) para ML

### 26.2 Álgebra Lineal para ML
- Tensores: generalización de escalares, vectores, matrices
- Operaciones: broadcasting, einsum, reductions, slicing
- Implementación de un tensor N-dimensional en C (strides, shape, data)
- Backend BLAS para operaciones aceleradas

### 26.3 Regresión Lineal y Logística
- Regresión lineal: OLS, gradiente descendente, ecuación normal
- Regresión logística: sigmoide, entropía cruzada, gradiente
- Implementación desde cero con optimización por lotes (batch) y estocástica (SGD)
- Ridge, Lasso, ElasticNet: regularización L1/L2

### 26.4 Redes Neuronales Feedforward (MLP)
- Neurona artificial: peso, bias, activación (ReLU, sigmoide, tanh, softmax)
- Capas fully connected: forward pass, backward pass
- Backpropagation: regla de la cadena, gradientes locales, actualización de pesos
- Implementación: MLP con N capas ocultas en C puro
- Optimizadores: SGD, Momentum, Adam, RMSprop

### 26.5 Redes Neuronales Convolucionales (CNN)
- Capa convolucional: kernel/filter, stride, padding, canales
- Capa de pooling: max pooling, average pooling
- Implementación de convolución directa e im2col + GEMM
- Arquitecturas: LeNet, AlexNet, ResNet (bloques residuales)

### 26.6 Redes Neuronales Recurrentes (RNN)
- RNN vanilla: celda, estado oculto, backpropagation through time (BPTT)
- LSTM (Long Short-Term Memory): forget gate, input gate, output gate, cell state
- GRU (Gated Recurrent Unit): simplificación de LSTM
- Implementación de una celda LSTM en C
- Aplicaciones: generación de texto, predicción de series temporales

### 26.7 Autograd y Diferenciación Automática
- Forward mode vs reverse mode (backpropagation)
- Tape-based autograd: registro de operaciones en un grafo computacional
- Implementación de autograd en C: nodos de operación, gradientes, topo-sort
- Comparativa: gradientes numéricos vs analíticos vs automáticos

### 26.8 Árboles de Decisión y Random Forest
- Árbol de decisión: Gini impurity, entropía, information gain
- Construcción del árbol: CART (Classification and Regression Trees)
- Random Forest: bagging, random subspace, votación
- Implementación: entrenamiento y predicción en C

### 26.9 k-Means y Algoritmos de Clustering
- k-Means: inicialización (k-means++), asignación, actualización de centroides
- DBSCAN: clustering basado en densidad, ruido
- Implementación de k-Means con optimización vectorizada

### 26.10 Máquinas de Soporte Vectorial (SVM)
- SVM lineal: margen máximo, hinge loss, gradiente
- Kernel trick: RBF, polinomial, sigmoide
- SMO (Sequential Minimal Optimization): algoritmo de entrenamiento
- Implementación de SVM binaria y multiclase (one-vs-one, one-vs-rest)

### 26.11 Reducción de Dimensionalidad
- PCA (Principal Component Analysis): matriz de covarianza, autovalores, proyección
- t-SNE y UMAP: visualización de alta dimensionalidad (implementación simplificada)

### 26.12 Inferencia en Producción
- Exportar pesos entrenados desde Python (NumPy, PyTorch, TensorFlow)
- Formato: cabecera binaria con pesos, normalización, arquitectura
- Forward pass optimizado: SIMD, quantización INT8, pruning
- ONNX Runtime C: ejecución de modelos ONNX desde C

### 26.13 Librerías de ML en C
- tinygrad: implementación educativa de autograd/tensor (inspiración)
- Darknet (YOLO): redes neuronales en C, detección de objetos
- CCV (libccv): visión por computadora en C
- scikit-learn en C: implementaciones propias

### 26.14 Ejercicios
- Implementar una regresión lineal con gradiente descendente y comparar con scikit-learn
- Construir un MLP para clasificar dígitos MNIST desde cero en C
- Implementar autograd y entrenar una red de 2 capas
- Escribir un kernel de convolución 3×3 optimizado con SIMD
- Cargar pesos pre-entrenados de PyTorch y hacer inferencia en C

---

## Capítulo 27 — Bases de Datos y Motores de Almacenamiento

### 27.1 Fundamentos de Bases de Datos
- Modelo relacional: tablas, filas, columnas, índices, constraints
- SQL: SELECT, JOIN, GROUP BY, subqueries, window functions
- Transacciones: ACID (Atomicity, Consistency, Isolation, Durability)
- Implementación de una tabla en memoria con índices

### 27.2 Almacenamiento Persistente
- Páginas: size fijo (4KB, 8KB, 16KB), slot array, layout
- Heap files: organización desordenada, inserción append-only
- Slotted pages: administración de espacio variable dentro de la página
- Almacenamiento en disco: `pread`/`pwrite`, O_DIRECT, fsync, journaling
- Serialización de tuplas a binario (fixlen vs varlen)

### 27.3 B-Tree y B+Tree
- Estructura: nodos internos + hojas, branching factor, altura logarítmica
- Operaciones: búsqueda, inserción (split), eliminación (merge/redistribute)
- B+Tree: solo datos en hojas, hojas enlazadas para scan secuencial
- Implementación completa de B+Tree en C con almacenamiento en archivo
- Optimizaciones: buffers de páginas, búfer de inserción, fuse (árboles fractales)

### 27.4 LSM-Tree (Log-Structured Merge-Tree)
- Componentes: memtable (in-memory, sorted, skip list o árbol rojinegro)
- SSTables: archivos ordenados inmutables en disco
- Compaction: leveling (LevelDB/RocksDB) vs tiering (Cassandra)
- Bloom filters: probabilistic membership test, reducción de I/O
- Implementación: LSM-tree simple con merge iterativo
- Comparativa: B+Tree vs LSM-Tree (escritura vs lectura, amplificación)

### 27.5 Índices Secundarios
- Índices clustered vs non-clustered
- Índices compuestos, covering index, partial index
- Bitmap indexes: para columnas con baja cardinalidad
- Índices hash: lookup O(1), no soporta range scans
- Índices espaciales: R-Tree, Quadtree, Grid

### 27.6 Procesamiento de Consultas
- Plan de ejecución: operadores (scan, filter, join, aggregate, sort)
- Join algorithms: nested-loop, hash join, merge join, sort-merge join
- Cost-based optimization: cardinalidad, selectividad, histogramas
- Volcano model: operadores que producen tuplas una a una
- Vectorized execution: batches de tuplas para mejor localidad de caché

### 27.7 Transacciones y Concurrencia
- ARIES: algoritmo de recovery con WAL (Write-Ahead Log)
- Lock manager: shared/exclusive/intention locks, 2PL (Two-Phase Locking)
- MVCC (Multi-Version Concurrency Control): snapshots, version chains
- Isolation levels: serializable, repeatable read, read committed, read uncommitted
- Deadlock detection y prevention: wait-die, wound-wait, timeout

### 27.8 Write-Ahead Log (WAL)
- Estructura del log: LSN (Log Sequence Number), records, checkpoints
- Logging: físico, lógico, fisiológico
- Recovery: analysis, redo, undo phases
- Group commit y batching para rendimiento
- Implementación de WAL simple con checksums

### 27.9 Almacenamiento Columnar
- Row store vs column store: trade-offs (OLTP vs OLAP)
- Parquet: formato columnar con compresión por páginas
- Run-length encoding, dictionary encoding, delta encoding
- Predicate pushdown y late materialization

### 27.10 SQL Parser
- Lexer SQL: tokens (SELECT, FROM, WHERE, JOIN, etc.)
- Parser: AST para SQL (SELECT, INSERT, CREATE TABLE)
- Planner: de AST a plan lógico (álgebra relacional)
- Optimizer: reescritura de consultas, pushdown de predicados
- Executor: de plan físico a resultados

### 27.11 Bases de Datos Embebidas
- SQLite: arquitectura, VDBE (Virtual Database Engine), B-tree, pager
- DuckDB: OLAP embebido, vectorizado, columnar
- Implementación de una base de datos OLTP simple (SQL subset) en C
- Benchmarks: SQLite vs RocksDB vs LMDB vs implementación propia

### 27.12 Ejercicios
- Implementar un archivo de páginas slotted con lectura/escritura directa
- Construir un B+Tree funcional con operaciones de insert, search, range scan
- Implementar un LSM-tree con bloom filter y compaction por niveles
- Escribir un parser SQL mínimo que soporte SELECT y WHERE
- Crear una mini-base de datos transaccional con WAL y rollback

---

## Capítulo 28 — Protocolos de Red Avanzados

### 28.1 HTTP/1.1 y HTTP/2
- HTTP/1.1: persistencia, chunked transfer, pipeline, pipelining issues
- HTTP/2: multiplexación, HPACK (compresión de headers), server push, prioridades
- Framing: frames, streams, settings, flow control
- Implementación de un cliente HTTP/2 en C: HPACK decoder, frame parsing
- h2load y benchmarks: HTTP/1.1 vs HTTP/2

### 28.2 HTTP/3 y QUIC
- QUIC: transporte sobre UDP, 0-RTT, conexión migración, TLS 1.3 integrado
- HTTP/3: adaptación de HTTP/2 sobre QUIC
- librerías: lsquic, quiche, msquic, ngtcp2
- Implementación de un cliente QUIC básico

### 28.3 WebSockets (RFC 6455)
- Handshake HTTP/1.1 upgrade
- Framing: opcode, payload length, masking
- Implementación de servidor WebSocket en C desde cero
- Extensiones: permessage-deflate (compresión)

### 28.4 DNS (Domain Name System)
- Formato de mensajes DNS: header, question, answer, authority, additional
- Tipos de registros: A, AAAA, CNAME, MX, NS, TXT, SRV
- Resolución recursiva vs iterativa
- Implementación de un resolvedor DNS minimalista desde cero
- DNSSEC: extensión de seguridad, RRSIG, DNSKEY, DS

### 28.5 TLS 1.3
- Handshake: ClientHello, ServerHello, key exchange, Finished
- Cipher suites: AEAD (AES-GCM, ChaCha20-Poly1305), key derivation (HKDF)
- 0-RTT: early data, riesgos de replay
- Implementación simplificada de TLS 1.3 con libtls (LibreSSL)

### 28.6 gRPC y Protocol Buffers
- Protocol Buffers: `.proto`, codificación wire format (varint, zigzag, packed)
- gRPC: HTTP/2 streaming (unary, server streaming, client streaming, bidirectional)
- Implementar un stub gRPC en C con nanopb + implementación HTTP/2 propia
- Comparativa: gRPC vs REST vs Thrift

### 28.7 DHCP e IPMI
- DHCP: discover, offer, request, acknowledge
- Implementación de un cliente DHCP en C raw socket

### 28.8 BGP (Border Gateway Protocol)
- Ruteo entre AS: path vector, atributos (AS_PATH, NEXT_HOP, LOCAL_PREF)
- Mensajes BGP: OPEN, UPDATE, NOTIFICATION, KEEPALIVE
- Implementación rudimentaria de un peer BGP (lectura educativa)
- Herramientas: FRRouting, Bird, GoBGP

### 28.9 VXLAN y Túneles
- VXLAN: encapsulación L2 sobre UDP, VNI
- Geneve, GRE, GUE: alternativas de túneles
- Virtual networking: bridge, Open vSwitch, netlink

### 28.10 Análisis y Captura de Paquetes
- libpcap (tcpdump): captura raw, filtros BPF
- Implementación de un sniffer TCP en C con libpcap
- Reconstrucción de flujos TCP (seguimiento de conexiones)

### 28.11 Ejercicios
- Implementar un servidor WebSocket que transmita datos en tiempo real
- Escribir un resolver DNS que consulte un servidor raíz y siga las delegaciones
- Capturar tráfico HTTP/2 con tcpdump y decodificar los frames manualmente
- Construir un servicio gRPC simple con servidor y cliente en C usando nanopb
- Implementar un cliente QUIC básico con quiche que realice una solicitud HTTP/3

---

## Capítulo 29 — Depuración y Perfilamiento Avanzado

### 29.1 GDB a Nivel Profundo
- Breakpoints: hardware, software, conditional, watchpoints
- Debugging de optimización: `-Og`, variables optimizadas, `volatile` temporal
- Hilos: `info threads`, `thread apply all bt`, scheduler-locking
- Core dumps: `ulimit -c unlimited`, `gdb binario core`, análisis post-mortem
- Reverse debugging: `record`, `reverse-step`, `reverse-continue` (GDB + RR)
- GDB scripting: Python API, pretty-printers, breakpoint commands

### 29.2 Debugging de Memoria
- Valgrind avanzado: Memcheck, Cachegrind, Massif, DHAT, Helgrind, DRD
- AddressSanitizer: `-fsanitize=address`, LeakSanitizer, detectar OOM
- MemorySanitizer: `-fsanitize=memory` (sin Valgrind)
- Profiling de heap: Massif (Valgrind), heaptrack, `malloc_hook`, `mtrace`
- Detección de double free/use-after-free: ASan, Valgrind, GWP-ASan

### 29.3 Perfilamiento de CPU
- `perf` (Linux) avanzado: `perf record -F 99 -g`, `perf report`, `perf annotate`
- Flame graphs: `perf script | stackcollapse-perf.pl | flamegraph.pl`
- Off-CPU analysis: `perf sched`, wakeup tracking, I/O profiling
- Eventos de hardware: cycles, cache-misses, branch-misses, stalled-cycles
- `perf stat -d`: estadísticas detalladas de ejecución
- Perfilamiento en producción: `perf` sin parar el proceso, `-p PID`

### 29.4 Tracing Dinámico
- `strace`: syscalls, filtros, seguimiento de procesos hijos
- `ltrace`: llamadas a bibliotecas compartidas
- `lsof`: archivos abiertos por proceso
- eBPF con `bpftrace`: one-liners, scripts de tracing
  - `bpftrace -e 'kprobe:do_sys_open { printf("%s\n", str(arg1)); }'`
  - `tracepoint:syscalls:sys_enter_*`
- `perf-tools` de Brendan Gregg: `execsnoop`, `opensnoop`, `biolatency`

### 29.5 SystemTap y DTrace
- SystemTap: scripts de instrumentación del kernel Linux
- DTrace (BSD/macOS): probes, providers, actions, agregaciones
- Ejemplo: medir latencia de syscall `read` por proceso

### 29.6 Profiling de Memoria y Cache
- Cachegrind (Valgrind): simulación de caché L1/L2, branch predictor
- `perf c2c`: análisis de false sharing y cache line contention
- Intel Advisor: roofline analysis, memory access pattern
- Uso de `perf mem`: muestreo de accesos a memoria

### 29.7 Profiling de Concurrencia
- ThreadSanitizer (TSan): `-fsanitize=thread`, detección de data races
- Helgrind (Valgrind): análisis de sincronización con pthreads
- RR (Mozilla): grabación y reproducción determinista de programas multi-hilo
- Intel Inspector: detección de deadlocks, memory errors

### 29.8 Heap Profiling y Análisis de Fugas
- Massif (Valgrind): gráficos de consumo de heap en el tiempo
- Heaptrack (KDE): snapshots, backtraces de asignación
- `malloc` hooking: `__malloc_hook`, `__free_hook` (glibc), `jemalloc` profiling
- Implementación de un profiler de heap mínimo con `LD_PRELOAD`

### 29.9 Depuración Remota y Embebida
- GDB remoto: `target remote :1234`, OpenOCD + GDB
- QEMU + GDB: debugging de kernel y firmware
- JTAG/SWD: debugging físico de microcontroladores
- Logging remoto: rsyslog, netconsole, serial console

### 29.10 Análisis de Core Dumps
- Configuración del sistema: `/proc/sys/kernel/core_pattern`
- Extraer información: `gdb`, `objdump`, `readelf`, `strings`
- Análisis de stack traces, registros, variables locales
- Post-mortem analysis en producción: systemd-coredump, apport

### 29.11 Análisis de Rendimiento de Red
- `tcpdump` + Wireshark: captura y análisis de tráfico
- `ss` y `netstat`: sockets, estadísticas TCP
- `iperf3`: medición de throughput
- `tc` (traffic control): simulación de latencia, pérdida, ancho de banda
- `netdata`, `bmon`, `nload`: monitoreo en tiempo real

### 29.12 Ejercicios
- Usar `perf` para generar un flame graph de una aplicación CPU-bound
- Analizar un programa con fuga de memoria usando Massif y corregirla
- Usar RR para grabar y reproducir un bug intermitente multi-hilo
- Escribir un script de bpftrace que rastree todas las aperturas de archivos
- Realizar un core dump post-mortem de un proceso en producción y diagnosticar la falla

---

## Capítulo 30 — Desarrollo Seguro y Código Robusto

### 30.1 Principios de Programación Segura
- Principio de mínimo privilegio
- Defense in depth (defensa en profundidad)
- Fail safe / fail secure
- Complete mediation
- Secure by design vs security by obscurity
- Security vs usability trade-offs

### 30.2 MISRA-C: Directrices para C Seguro
- MISRA-C:2012 (y 2023): reglas obligatorias, requeridas, advisory
- Reglas clave: sin asignación en condiciones, sin `goto` (excepto patrones), sin `#undef`
- Restricciones de tipado: casts explícitos, evitar `union` type-punning, `bool` estricto
- Análisis estático: herramienta MISRA Checker, PC-Lint, Coverity
- Uso en automoción (ISO 26262) y aviónica (DO-178C)

### 30.3 CERT C Coding Standard
- Estrategias: prevenir, detectar, mitigar
- Categorías: preprocessor (PRE), declarations (DEC), expressions (EXP), integers (INT)
  - INT30-C: asegurar que las operaciones con enteros no desborden
  - ARR30-C: no realizar aritmética de punteros fuera de límites
  - STR31-C: garantizar terminación de cadenas
  - MEM30-C: no acceder a memoria liberada
  - FIO30-C: excluir nombres de archivos definidos por el usuario
  - ERR30-C: manejar errores de `setjmp`/`longjmp`
- Ejemplos de reglas por categoría: INT32-C, MSC30-C, CON40-C

### 30.4 Fuzzing (Pruebas de Robustez)
- Tipos de fuzzing: black-box, white-box, grey-box, coverage-guided
- AFL (American Fuzzy Lop): mutación, instrumentación, mapa de cobertura
- libFuzzer: fuzzing in-process, coverage-guided, integración con sanitizers
- Honggfuzz: fuzzing con hardware feedback (Intel PT)
- Fuzzing de APIs: escribir harnesses de entrada para funciones específicas
- Fuzzing de redes: Preeny, AFLNet, Fuzzotron
- Corpora: semillas iniciales, minimización de corpus
- Integración CI: fuzzing continuo (ClusterFuzz, OSS-Fuzz)

### 30.5 SAST (Static Application Security Testing)
- Semgrep: reglas personalizables, modo pro, patrones de código inseguro
- CodeQL (GitHub): consultas para vulnerabilidades en C/C++
- Flawfinder: análisis simple de C/C++ para funciones peligrosas
- RATS (Rough Auditing Tool for Security)
- Análisis de flujo de datos: taint tracking, source-to-sink

### 30.6 DAST (Dynamic Application Security Testing)
- Pruebas de penetración automatizadas
- Inyección de entradas maliciosas: format strings, buffer overflows
- Verificación de comportamientos: time-of-check time-of-use (TOCTOU)
- Herramientas: OWASP ZAP (para web), BeStorm, Cenzic

### 30.7 Control de Flujo y Protección de Pila
- Stack canaries: `-fstack-protector`, `-fstack-protector-strong`, `-fstack-protector-all`
- Shadow stacks: hardware (Intel CET) y software
- FORTIFY_SOURCE: `-D_FORTIFY_SOURCE=2`, detección en tiempo de compilación
- _FORTIFY_SOURCE=3: detección de desbordamiento dinámico (glibc 2.33+)
- Control Flow Integrity (CFI): `-fsanitize=cfi` (Clang)
- SafeStack: `-fsanitize=safe-stack`

### 30.8 Aislamiento y Sandboxing
- `seccomp` (Linux): filtro de syscalls, `seccomp-bpf`
- `landlock`: sandboxing de archivos desde Linux 5.13
- `namespaces` y `cgroups`: contenedores ligeros
- `pledge` y `unveil` (OpenBSD): promesas de capacidad
- `capsicum` (FreeBSD): capability-based sandboxing

### 30.9 Mitigación de Vulnerabilidades Comunes
- Integer overflow: `__builtin_add_overflow`, `__builtin_mul_overflow`
- Buffer overflow: `strncpy` → `strlcpy` / `memcpy_s`, `snprintf`
- Format string: nunca pasar entrada de usuario como formato
- Command injection: no usar `system()` con input externo
- Race conditions: `O_CREAT | O_EXCL`, `flock`, `mkstemp`
- TOCTOU: minimizar ventana entre verificación y uso

### 30.10 Secure Coding Patterns
- RAII simulado: `defer` macros, `__attribute__((cleanup))`
- Resource acquisition is initialization: archivos, locks, memoria
- Safe memory management: reference counting, arena allocators
- Error handling consistente: goto cleanup pattern (kernel style)
- Logging seguro: no loguear secretos, sanitizar entradas

### 30.11 Auditoría de Código
- Checklist de revisión manual: desbordamientos, uso de memoria, concurrencia
- Revisión de cambios (diff): seguridad en nuevas funcionalidades
- Threat modeling: STRIDE, DREAD, attack trees
- Security champions y bug bounty

### 30.12 Ejercicios
- Identificar y corregir 10 vulnerabilidades de OWASP en código C dado
- Escribir un harness de libFuzzer para una función de parsing
- Configurar seccomp para restringir un proceso a solo `read`, `write`, `exit`
- Implementar una función de copia segura que verifique límites en compilación y runtime
- Realizar un threat model de una aplicación de red usando STRIDE

---

## Capítulo 31 — Máquinas Virtuales y Compilación JIT

### 31.1 Fundamentos de Máquinas Virtuales
- VM de sistema vs VM de proceso
- Stack-based vs register-based: trade-offs (bytecode size, instruction count)
- Ejemplos: JVM (stack), LuaJIT (register), CPython (stack), Dalvik (register)
- Diseño de ISA: word size, operandos, instrucciones típicas

### 31.2 Diseño de una VM Stack-based
- Pila de operandos, pila de llamadas, frame pointer
- Sistema de tipos: tagged integers, boxing/unboxing
- Conjunto de instrucciones: push, pop, add, sub, call, ret, jmp, load, store
- Dispatcher: switch-based, threaded code (label pointers, GCC computed goto)
- Implementación completa: VM Brainfuck o VM para un lenguaje minimalista

### 31.3 Diseño de una VM Register-based
- Conjunto de registros virtuales (infinitos o finitos)
- Instrucciones: add r1, r2, r3 (3-address); load r1, [r2]
- SSA como IR intermedio para la VM
- Comparativa: número de instrucciones vs stack VM

### 31.4 Garbage Collection para VMs
- Mark-and-Sweep: fases de marcado y barrido, fragmentación
- Copying GC (Cheney): semispace, scavenger, compactación
- Generational GC: young/old generation, write barrier, promotion
- Reference counting: ciclo de conteo, weak references
- Implementación de un GC simple (mark-and-sweep) en C para una VM

### 31.5 Compilación JIT (Just-In-Time)
- JIT vs AOT vs intérprete: latencia inicial vs rendimiento sostenido
- Métodos: method JIT, tracing JIT, JIT de grado de optimización
- Generación de código en memoria: `mmap` + `PROT_EXEC`, `VirtualAlloc`
- DynASM (de LuaJIT): generación de código máquina con macros
- Implementación de un JIT minimalista: bytecode → x86-64

### 31.6 Tracing JIT (LuaJIT)
- Tracing: registro de trazas durante ejecución, hot paths
- Side exits, guard conditions, loop detection
- SSA IR, optimizaciones por traza (const folding, alias analysis, DCE)
- Code generation: de IR a código máquina
- Diferencias: LuaJIT vs PyPy (tracing) vs V8 (method JIT)

### 31.7 JIT para un Subconjunto de C
- Compilar a un IR (TAC o SSA)
- Aplicar optimizaciones del capítulo 19 (constant folding, DCE, CSE)
- Generar código x86-64: asignación de registros naive (todos a stack)
- Ejecutar el código generado directamente desde un buffer ejecutable

### 31.8 GDB de JIT: Debugging de Código Generado
- `__jit_debug_register_code`: interfaz GDB JIT
- Símbolos para código generado (DWARF simplificado)
- Breakpoints en código JIT

### 31.9 VMs Notables Implementadas en C
- Lua (PUC-Rio): register-based VM, 30K+ líneas
- CPython: stack-based VM, co-rutinas (generadores)
- Dalvik (Android): register-based, dex format
- LuaJIT: tracing JIT, uno de los JIT más eficientes
- Wasm3: intérprete WASM rápido en C

### 31.10 Ejercicios
- Implementar una VM stack-based completa para Brainfuck con JIT
- Diseñar un bytecode para un lenguaje simple y escribir un intérprete
- Implementar un mark-and-sweep GC simple para la VM
- Usar DynASM para generar una función que sume dos números en código máquina
- Extender la VM con soporte para closures y upvalues (como Lua)

---

## Capítulo 32 — Motores de Bases de Datos Embebidas

### 32.1 Introducción a Bases de Datos Embebidas
- SQLite: la base de datos embebida más utilizada del mundo
- Arquitectura: frontend (tokenizer, parser, code generator) + backend (B-tree, pager, OS interface)
- Casos de uso: aplicaciones móviles, navegadores, IoT, sistemas críticos
- Comparativa: SQLite vs RocksDB vs LMDB vs Berkeley DB

### 32.2 SQLite Internals: El Pager
- Pager: gestión de páginas, caché, journal (rollback y WAL)
- Page cache: buffer pool, page replacement (LRU)
- Locking: estados UNLOCK, SHARED, RESERVED, PENDING, EXCLUSIVE
- WAL mode: concurrent reads + single writer, checkpointing

### 32.3 SQLite Internals: B-Tree
- B-tree de SQLite: diferencias con B+Tree, páginas internas vs hojas
- Cell format: payload, overflow pages, pointer map
- Table B-tree vs Index B-tree
- Operaciones: balance, split, coalesce, rebalance

### 32.4 SQLite Internals: VDBE (Virtual Database Engine)
- Programas VDBE: bytecode generado por el compilador SQL
- Instrucciones: OpenRead, OpenWrite, SeekGE, Column, ResultRow, Next
- SQL → bytecode: ejemplo de SELECT, INSERT, JOIN
- Análisis de programas VDBE: `EXPLAIN` en SQLite

### 32.5 LMDB (Lightning Memory-Mapped Database)
- Arquitectura: B+Tree sobre archivos mapeados en memoria (`mmap`)
- MVCC: snapshot isolation, lectores sin locks
- Transacciones: readonly (sin locks) vs readwrite (exclusive)
- Single writer + múltiples lectores concurrentes
- Zero-copy: lectores acceden directamente a la memoria mapeada
- Limitaciones: tamaño fijo del mapa de memoria, sin SQL

### 32.6 RocksDB y LSM Avanzado
- RocksDB: fork de LevelDB, optimizado para SSD y flash storage
- Componentes: memtable (skip list), WAL, SSTables (bloque), Bloom filters
- Compaction: leveled, universal, FIFO
- Merge operators: read-modify-write atómico en LSM
- Column families: datos agrupados con diferentes configuraciones
- RocksDB desde C: interfaz C API (rocksdb/c.h)

### 32.7 Bases de Datos en Memoria
- Redis: estructura de datos en memoria, protocolo RESP, persistencia RDB/AOF
- Implementación de un key-value store en memoria con índices hash
- Expiración de claves: lazy deletion, time wheel
- Pub/Sub: canales de mensajería

### 32.8 Implementación de un Motor SQL Simple
- SQL Parser con Lemon (parser generator usado por SQLite)
- Query planner: selección de índices, join ordering
- Query executor: iteradores, filtros, proyecciones
- Transacciones: BEGIN/COMMIT/ROLLBACK con WAL simple

### 32.9 Ejercicios
- Leer el código fuente de SQLite y explicar el flujo de una consulta SELECT
- Implementar un key-value store estilo LMDB usando `mmap` y B+Tree
- Analizar un programa VDBE generado por `EXPLAIN` para consultas SQL complejas
- Escribir un benchmark comparando SQLite vs LMDB vs RocksDB en tu hardware
- Agregar una función SQL personalizada (UDF) a SQLite en C

---

## Capítulo 33 — Procesamiento de Texto y Lenguajes

### 33.1 Expresiones Regulares
- Teoría: autómatas finitos deterministas (DFA) y no deterministas (NFA)
- Thompson NFA: construcción de NFA a partir de regex
- Subset construction: NFA → DFA, minimización de DFA
- Implementación de un motor de regex simple desde cero
- Backtracking vs NFA/DFA: ventajas y desventajas
- PCRE, RE2 (Google), TRE: bibliotecas de regex en C
- Uso de `<regex.h>` (POSIX) y PCRE2 con API C

### 33.2 Parsing de JSON
- Gramática: object, array, string, number, boolean, null
- Parser recursive descent para JSON
- DOM builder: árbol de nodos JSON en memoria
- SAX-style parser: callbacks para eventos (begin_object, key, value, etc.)
- Implementación de un serializador JSON (struct → string)
- jansson, cJSON, yyjson (ultrarrápido): comparativa de librerías

### 33.3 Parsing de XML
- Gramática XML: elementos, atributos, entidades, namespaces
- SAX parser: expat (C), callbacks para inicio/fin de elemento
- DOM parser: libxml2, árbol de documentos
- XPath: expresión de rutas de navegación
- XML vs JSON vs YAML vs TOML: cuándo usar cada uno

### 33.4 Parsing de YAML y TOML
- YAML: indentación, anchors, aliases, tipos complejos
- TOML: tablas, arrays de tablas, fechas, tipos inline
- Implementación de un parser TOML minimalista
- libyaml, libtoml, tomlc99: implementaciones en C

### 33.5 PEG Parsing (Parsing Expression Grammar)
- PEG vs CFG: ordered choice vs unordered, no ambigüedad
- Packrat parsing: memoización para parsing lineal
- PEG parsers: LPeg (Lua), peg/leg (C), peggy
- Implementación de un parser PEG en C

### 33.6 Parsers de Lenguajes de Programación
- Parser de Lua: API C para analizar código Lua
- Parser de Python: CPython parser (Grammar/Grammar)
- Parser de JavaScript: Duktape, QuickJS (incluyen parser completo)
- Uso de Tree-sitter: parsing incremental, AST concreto, queries

### 33.7 Generación y Transformación de Código
- Code generation: from AST to source (pretty printer)
- Autómatas de transformación: tree matching, reescritura
- Herramientas: Coccinelle (transformación semántica de código C)
- Formatting: clang-format, GNU indent

### 33.8 Motores de Plantillas
- mustache: template logic-less, implementación en C (mustach)
- Implementación de un template engine simple: reemplazo de variables, bucles

### 33.9 Procesamiento de Lenguaje Natural (NLP)
- Tokenización, stemming (Porter stemmer en C)
- n-gramas, TF-IDF, cosine similarity
- Implementación de un corrector ortográfico simple (edit distance, Norvig)

### 33.10 Ejercicios
- Implementar un motor de regex simple (Thompson NFA) que soporte `*`, `+`, `?`, `[]`
- Escribir un parser JSON completo con DOM builder
- Implementar un parser PEG para expresiones matemáticas
- Crear un minificador/pretty-printer de código C usando Tree-sitter
- Construir un template engine simple para generar HTML desde C

---

## Capítulo 34 — Sistemas Distribuidos y Redes P2P

### 34.1 Fundamentos de Sistemas Distribuidos
- Características: concurrencia, falta de reloj global, fallos independientes
- Modelos: cliente-servidor, peer-to-peer, híbrido
- Tiempo y orden: relojes lógicos (Lamport), relojes vectoriales
- CAP theorem: consistencia, disponibilidad, tolerancia al particionamiento
- Fallos: crash, byzantine, particiones de red

### 34.2 RPC (Remote Procedure Call)
- Principio: llamar a una función remota como si fuera local
- Stubs: client stub, server stub, marshalling/unmarshalling
- Serialización: Sun RPC (XDR), ONC RPC
- Implementación de un RPC en C con sockets + serialización propia
- gRPC: HTTP/2 + protobuf (usando C API)

### 34.3 Raft: Algoritmo de Consenso
- Estados: follower, candidate, leader
- Elección de líder: términos, votos, timeouts aleatorizados
- Log replication: entradas, commit, matching index
- Safety: election safety, log matching, leader completeness
- Persistencia: guardar estado en disco para recuperación
- Implementación de Raft en C: core algorithm, RPCs, state machine

### 34.4 Protocolos de Consenso Adicionales
- Paxos: Classic Paxos, Multi-Paxos, Fast Paxos
- Zab (ZooKeeper): atomic broadcast, leader election
- Gossip protocols: diseminación de información, detección de fallos, SWIM
- Implementación de SWIM (Scalable Weakly-consistent Infection-style) en C

### 34.5 Sistemas de Archivos Distribuidos
- NFS v3/v4: protocolo RPC, mount, operaciones de archivos
- MooseFS: chunkservers, metaserver, replicación
- Implementación de un DFS simple: metadata server + storage nodes

### 34.6 Redes Peer-to-Peer
- DHT (Distributed Hash Table): Chord, Kademlia
- Kademlia: XOR metric, buckets, find_node, find_value
- Implementación de Kademlia DHT en C
- BitTorrent: piezas, trackerless (DHT), PEX
- IPFS: content-addressed, DHT, bitswap

### 34.7 Mensajería y Streaming
- Apache Kafka: productor, consumidor, tópicos, particiones
- RabbitMQ: AMQP, exchanges, queues, bindings
- ZeroMQ: sockets inteligentes, patrones (pub/sub, req/rep, push/pull)
- Nanomsg: sucesor de ZeroMQ en C (nng)
- Implementación de un message broker simple con publish/subscribe

### 34.8 Tolerancia a Fallos y Replicación
- Failover: activo-pasivo, activo-activo
- State machine replication: logs replicados, aplicación determinista
- Leaderless replication: Dynamo-style, read repair, hinted handoff
- Snapshot isolation y consistent snapshot

### 34.9 Sistemas Distribuidos Reales
- etcd: key-value store, Raft consensus, usado por Kubernetes
- ZooKeeper: coordinación distribuida, znodes, watches (C API)
- Consul: service discovery, health checking, KV store
- Redis Cluster: sharding, replicación, failover

### 34.10 Ejercicios
- Implementar un RPC simple para un servicio de clave-valor en C
- Construir un cluster Raft mínimo con 3 nodos (elección + replicación de logs)
- Implementar una DHT estilo Kademlia con find_node y store_value
- Crear un sistema pub/sub con ZeroMQ (nng) en C
- Escribir un cliente etcd en C que lea y escriba claves

---

## Capítulo 35 — Microservicios y APIs en C

### 35.1 Fundamentos de Microservicios
- Principios: servicios pequeños, autónomos, desplegables independientemente
- Comunicación: REST, gRPC, mensajería asíncrona
- Descubrimiento de servicios: DNS, Consul, Etcd
- API Gateway: enrutamiento, autenticación, rate limiting
- Comparativa: monolito vs microservicios (trade-offs)

### 35.2 Servidores HTTP en C
- Implementación de un servidor HTTP/1.1 desde cero
- Parsing de requests, routing, handlers (como express.js pero en C)
- librerías: libmicrohttpd (GNU), libonion, kore (async), facio
- TLS: integración de libtls (LibreSSL) o OpenSSL

### 35.3 RESTful APIs con libmicrohttpd
- Configuración: puerto, threads, TLS
- Handlers: GET, POST, PUT, DELETE, PATCH
- Content negotiation: JSON vs XML vs CBOR
- Middleware: logging, autenticación básica, rate limiting
- OpenAPI/Swagger: documentación de la API

### 35.4 Serialización: Protobuf, MessagePack, CBOR, FlatBuffers
- Protocol Buffers: .proto, compilación con protoc para C (nanopb)
- MessagePack: formato binario compacto, implementación msgpack-c
- CBOR: RFC 7049, estándar IETF, similar a JSON binario, libcbor
- FlatBuffers (Google): acceso directo sin parsing, ideal para móvil/juegos
- Comparativa: tamaño, velocidad de parse, facilidad de uso

### 35.5 Frameworks Web en C
- Kore: framework HTTP/2 asíncrono, TLS, WebSocket, Python scripting
  - Routers, parámetros de URL, body parsing, plantillas
  - Módulos: Kore module system, páginas estáticas, WebSocket handlers
- libonion: servidor HTTP con handlers, post, sesiones, websockets
- facio: framework minimalista, single-header, fácil de integrar

### 35.6 Autenticación y Autorización
- HTTP Basic Auth y Digest Auth
- JWT (JSON Web Tokens): implementación de firma HMAC/RSA en C
- OAuth 2.0: flujo de authorization code, client credentials
- API keys: generación, validación, rotación
- Rate limiting: sliding window, token bucket, leaky bucket

### 35.7 Procesamiento Asíncrono
- Colas de trabajo: implementación con productor-consumidor
- Background jobs: procesamiento diferido, retry, dead letter queue
- Event-driven: libevent, libuv, libev (integración con el servidor HTTP)

### 35.8 Contenerización y Despliegue
- Dockerfile para aplicaciones C: multi-stage builds, imágenes slim (Alpine, distroless)
- Compilación estática: `-static`, sin dependencias de glibc (musl)
- Docker Compose: API + base de datos + caching
- Health checks: endpoints `/healthz`, `/readyz`

### 35.9 Monitorización de APIs
- Métricas: Prometheus client library para C
  - Contadores, gauges, histogramas, summaries
  - Endpoint `/metrics` en formato Prometheus
- Logging estructurado: JSON logs, correlation IDs
- Distributed tracing: OpenTelemetry (C SDK)
- Health checks y readiness probes

### 35.10 API Gateway en C
- Enrutamiento basado en paths y hosts
- Rate limiting por IP/usuario/API key
- Autenticación centralizada (JWT validation)
- Agregación de respuestas (GraphQL-like)
- Caching de respuestas (in-memory, Redis)

### 35.11 Ejercicios
- Crear una API REST CRUD para una entidad "Usuario" con libmicrohttpd + JSON
- Implementar un middleware de rate limiting (token bucket)
- Escribir un servidor HTTP asíncrono con Kore que sirva contenido dinámico
- Desplegar la API con Docker y docker-compose
- Agregar métricas Prometheus y construir un dashboard simple

---

## Capítulo 36 — Desarrollo de Herramientas CLI y TUI

### 36.1 Filosofía Unix y CLI
- Principios: hace una cosa bien, entrada/salida de texto, composición
- Tipos de CLI: argumentos, stdin/stdout, interactivas
- Streams: stdin, stdout, stderr, redirección, pipes

### 36.2 Parsing de Argumentos
- `getopt()` y `getopt_long()`: opciones cortas y largas
- Argumentos posicionales vs flags
- Subcomandos: `git commit`, `docker run` (implementación manual)
- librerías: argparse (header-only), argtable3, gengetopt
- Implementación de un parser de argumentos desde cero

### 36.3 Entrada de Usuario y Readline
- GNU Readline: `readline()`, `rl_history`, `rl_completion`, `rl_bind_key`
- Historia: `add_history()`, guardar/cargar archivo `.hist`
- Completado por TAB: funciones de callback, completado personalizado
- Alternativas: linenoise (ligera), editline (BSD), `getline()` simple

### 36.4 Salida Coloreada y Formateada
- Códigos de escape ANSI: colores, negrita, subrayado, cursor, limpiar pantalla
- Terminfo / termcap: bases de datos de capacidades de terminal
- `isatty()`, `fileno()`: detectar si la salida es una terminal
- Barras de progreso: implementación con `\r` y `fflush(stdout)`
- Spinners: animación de espera

### 36.5 TUI (Terminal User Interface) con ncurses
- Inicialización: `initscr()`, `cbreak()`, `noecho()`, `keypad()`
- Ventanas y paneles: `newwin()`, `subwin()`, `delwin()`
- Dibujo: `mvwprintw()`, `wborder()`, `wattron()`, `wattroff()`
- Entrada: `getch()`, menús, formularios
- Colores: `start_color()`, `init_pair()`, `COLOR_PAIR()`
- Paneles: `new_panel()`, `top_panel()`, `update_panels()`
- Ejemplo: editor de texto minimalista, gestor de archivos TUI

### 36.6 Librerías Modernas de TUI en C
- Notcurses: TUI de alto rendimiento, visuales complejas, true color
  - Plot, widgets, reflow, multimedia (imágenes, video en terminal)
- FTXUI (C++ pero con C API): widgets, DOM, eventos
- Termbox: minimalista, simple de integrar, sin ncurses

### 36.7 Salida de Datos: Tablas y Gráficos
- Tablas formateadas: `column -t` (implementación), alineación, bordes
- Gráficos de barras y sparklines en terminal
- Plotting: gnuplot como subproceso, pipe de datos
- Notcurses: `ncplot` para gráficos en tiempo real

### 36.8 Señales y Ciclo de Vida
- Manejo de SIGINT/SIGTERM: limpieza de terminal antes de salir
- `atexit()` y cleanup handlers
- Restaurar modo terminal: `endwin()` (ncurses), `tcsetattr()`
- Daemonización de procesos CLI

### 36.9 Plugins y Extensibilidad en CLI
- Carga de plugins con `dlopen()`/`dlsym()`
- Interfaz de plugin: struct con punteros a función
- Ejemplo: sistema de comandos extensible

### 36.10 Ejemplos de Herramientas CLI Notables en C
- `git`: el mejor ejemplo de CLI en C (subcomandos, flags, pipes)
- `curl`: tool + library, soporta múltiples protocolos
- `htop`: TUI interactiva con colores, barras, mouse
- `tmux`: multiplexor de terminal, paneles, ventanas
- `nano`: editor de texto TUI completo

### 36.11 Ejercicios
- Crear una herramienta que liste procesos (como `ps`) con formato de tabla
- Escribir un REPL con readline, historia y completado por TAB
- Implementar un editor de texto TUI minimalista con ncurses
- Construir un monitor de sistema TUI estilo htop
- Crear una herramienta con subcomandos (como `git`) usando getopt_long

---

## Capítulo 37 — Robótica y Sistemas de Control

### 37.1 Fundamentos de Robótica
- Componentes: sensores, actuadores, controlador, comunicación
- Cinemática directa e inversa (para brazos robóticos)
- Odometría: integración de encoder, dead reckoning
- Sistemas de coordenadas: mundo, robot, sensor

### 37.2 Control PID
- Componentes: proporcional, integral, derivativo
- Sintonización: Ziegler-Nichols, Cohen-Coon, auto-tuning
- Implementación en C: anti-windup integral, derivada en la medición
- Ejemplo: control de temperatura, velocidad de motor, balance de robot
- PID discreto vs continuo: tiempo de muestreo, discretización (Tustin, ZOH)

### 37.3 Filtros para Robótica
- Filtro complementario: fusión IMU (acelerómetro + giroscopio)
- Filtro de Kalman: predicción, actualización, matrices de covarianza
- Filtro de Kalman Extendido (EKF): linealización, Jacobianos
- Aplicación: localización, seguimiento de objetos, fusión de sensores
- Filtro de partículas (Monte Carlo Localization): implementación en C

### 37.4 ROS (Robot Operating System) desde C
- ROS 2: arquitectura DDS, nodos, tópicos, servicios, acciones
- Cliente C para ROS 2: `rclc` (embebido) y `rcl` (estándar)
- Publicación y suscripción a tópicos desde C
- Servicios y acciones: llamada remota de procedimientos
- ROS 2 en microcontroladores: micro-ROS

### 37.5 Control de Motores
- Motores DC: PWM + puente H (L298N, DRV8833), control de velocidad
- Motores paso a paso: secuencias de bobinas, microstepping, aceleración trapezoidal
- Servomotores: señal PWM, ángulo, calibración
- Encoders: cuadratura, lectura de posición y velocidad, dirección

### 37.6 Sensores y Fusión
- IMU: acelerómetro, giroscopio, magnetómetro (MPU6050, ICM-20948)
- Sensor de distancia: ultrasonido (HC-SR04), LiDAR, IR
- Cámara: captura + procesamiento embebido (OpenCV en C)
- Fusión sensorial: EKF, filtro complementario

### 37.7 Planificación de Movimiento
- Potential fields: atractivo + repulsivo, mínimos locales
- A* (A-star): grid-based, heurística, pathfinding
- Dijkstra sobre grafos de configuración
- RRT (Rapidly-exploring Random Tree): motion planning para espacios continuos

### 37.8 Control Embebido en Tiempo Real
- RTOS para robótica: FreeRTOS, Zephyr, NuttX
- Tareas periódicas: control loop a 1kHz, lectura de sensores, actuación
- Comunicación: CAN bus (para robots industriales), EtherCAT

### 37.9 Ejercicios
- Implementar un controlador PID para un motor DC virtual (simulado con EDO)
- Escribir un filtro de Kalman 1D para estimar distancia con un sensor ultrasónico
- Implementar A* para navegación en un grid y visualizar el camino
- Crear un nodo ROS 2 en C que publique velocidades de un robot diferencial
- Construir un balancebot simulado con IMU, filtro complementario y PID

---

## Capítulo 38 — C para Machine Learning: Deep Learning desde Cero

### 38.1 Tensores: Implementación N-Dimensional
- Estructura: shape, strides, data pointer, layout (row-major)
- Operaciones: reshape, transpose, permute, slice, concatenate, pad
- Broadcasting: expansión automática de dimensiones
- Contiguity: contiguo en memoria vs no contiguo (strides irregulares)
- Operaciones en el tensor: fill, copy, apply (unary, binary)

### 38.2 Operaciones de Álgebra Lineal para Deep Learning
- Producto matriz-matriz (GEMM) optimizado
- Convolución 2D: directa, im2col + GEMM, Winograd
- Pooling: max, average, global average
- Batch normalization: forward y backward, fused kernels
- Dropout: máscara binaria, escalado en inferencia

### 38.3 Capas y Modelos
- Capa Dense (Linear): forward pass, backward pass
- Capa Convolutional 2D: kernels, padding, stride, dilation
- Capa Recurrente: RNN, LSTM, GRU (forward + backward)
- Capa de Embedding: lookup table, sparse gradients
- Capa de atención (Attention): scaled dot-product, multi-head attention
- Modelo: Sequential API vs Functional API

### 38.4 Funciones de Activación y Pérdida
- Activaciones: ReLU, LeakyReLU, ELU, GELU, Swish/SiLU, sigmoid, tanh
- Pérdidas: MSE, MAE, Huber, Cross-Entropy, Binary Cross-Entropy, KL Divergence
- Derivadas analíticas de cada función

### 38.5 Optimizadores
- SGD (con y sin momentum, Nesterov)
- Adam, AdamW, Adamax
- RMSprop, AdaGrad, AdaDelta
- Learning rate scheduling: step decay, cosine annealing, warmup
- Gradient clipping: norm clipping, value clipping

### 38.6 Autograd desde Cero
- Tape: registro de operaciones en ordered_map/list
- Nodo: operación, inputs, gradientes, forward function, backward function
- Topological sort: orden de ejecución del backward
- Backward pass: recorrer el grafo en reversa acumulando gradientes
- Ejemplo: autograd para una red de 2 capas

### 38.7 Entrenamiento: DataLoader y Loop
- Dataset: carga de datos (MNIST, CIFAR-10), normalización, augmentación
- DataLoader: batching, shuffle, workers (hilos)
- Training loop: forward, loss, backward, optimizer.step
- Validation loop: evaluación sin gradientes
- Metrics: accuracy, precision, recall, F1, confusion matrix
- Early stopping, model checkpointing

### 38.8 Serialización de Modelos
- Guardar pesos: formato binario (cabecera + arrays de floats)
- Cargar pesos pre-entrenados desde Python (NumPy .npy, PyTorch .pt)
- ONNX: formato estándar de intercambio, runtime ONNX C

### 38.9 Inferencia Optimizada
- Cuantización: FP32 → INT8, calibración, quantized GEMM
- Pruning: eliminar pesos cercanos a cero, fine-tuning
- Kernel fusion: conv + batchnorm + relu en un solo kernel
- SIMD: vectorización de convolución y GEMM
- TFLite Micro: inferencia en microcontroladores

### 38.10 Ejercicios
- Implementar un tensor N-dimensional con broadcasting y operaciones básicas
- Construir una red fully-connected (2 capas ocultas) para clasificar MNIST
- Implementar autograd y entrenar un modelo de regresión lineal
- Escribir una convolución 2D optimizada con im2col + GEMM
- Cargar pesos de PyTorch y hacer inferencia en C para una CNN

---

## Capítulo 39 — TLPI: Programación Avanzada de Sistemas Linux

### 39.1 Llamadas al Sistema (System Calls)
- `syscall()` directa, wrapping glibc, instrucción `syscall` (x86-64)
- Traducción de errores a `errno`, `perror()`, `strerror()`
- Performance cost de las syscalls: contexto, cambio de modo
- Alternativas: vDSO (virtual dynamic shared object), `gettimeofday` sin syscall

### 39.2 Gestión de Procesos
- `fork()`, `vfork()`, `clone()`: diferencias y usos
- `execve()`, `execvp()`, `execl()`: reemplazo de la imagen del proceso
- `wait()`, `waitpid()`, `waitid()`: recolección de hijos zombi
- Procesos huérfanos y zombis
- Grupos de procesos, sesiones, `setsid()`
- Daemonización: protocolo de doble fork

### 39.3 Señales en Profundidad
- Mascar de señales: `sigprocmask()`, `pthread_sigmask()`
- Señales en tiempo real: `SIGRTMIN` a `SIGRTMAX`, cola de señales
- `sigwaitinfo()`, `sigtimedwait()`: recepción sincrónica de señales
- `signalfd()`: señales como descriptores de archivo
- Señales asíncronas seguras: lista de funciones async-signal-safe

### 39.4 Timers y Relojes
- `timer_create()`, `timer_settime()`, `timer_getoverrun()`
- `timerfd_create()`: timers como descriptores de archivo (integración con poll/epoll)
- `clock_gettime()`, `clock_nanosleep()`: nanosegundos
- High-resolution timers del kernel

### 39.5 Memoria Avanzada
- `mmap()`: archivos mapeados, memoria anónima, `MAP_SHARED`, `MAP_PRIVATE`
- `mprotect()`, `madvise()`, `mlockall()`: control de páginas
- `brk()`/`sbrk()`: ajuste del program break (heap)
- `alloca()`: asignación en stack (peligrosa, no portátil)
- `memfd_create()`: archivos anónimos en memoria
- Huge pages: `MAP_HUGETLB`, `mmap` con páginas de 2MB/1GB, Transparent Huge Pages

### 39.6 E/S Avanzada
- E/S sin bloqueo: `O_NONBLOCK`, `EAGAIN`/`EWOULDBLOCK`
- E/S con vectores: `readv()`, `writev()`, `preadv()`, `pwritev()`
- `splice()`, `tee()`, `sendfile()`: transferencia de datos en el kernel
- `fallocate()`, `posix_fallocate()`: preasignación de espacio
- `O_DIRECT`: E/S directa a disco sin caché de página
- `io_uring`: interfaz de E/S asíncrona moderna (Linux 5.1+)

### 39.7 io_uring en Profundidad
- Setup: `io_uring_setup()`, ring buffers (SQ y CQ)
- Submission: `io_uring_get_sqe()`, `io_uring_prep_read/write/accept/connect`
- Completion: `io_uring_wait_cqe()`, `io_uring_peek_cqe()`
- Modos: interrupt-driven vs polling
- Ejemplos: servidor TCP con io_uring, copia de archivos
- liburing: biblioteca de alto nivel para io_uring

### 39.8 Namespaces y Contenedores
- Tipos: PID, NET, MNT, UTS, IPC, USER, CGROUP
- `clone()` con flags de namespace
- `unshare()`, `setns()`: mover procesos entre namespaces
- Creación de un contenedor minimalista: namespace + cgroups + pivot_root

### 39.9 Control Groups (cgroups)
- v1 vs v2: jerarquía unificada
- Control de CPU: `cpu.max`, `cpu.weight`
- Control de memoria: `memory.max`, `memory.high`
- Control de I/O: `io.max`, `io.weight`
- Control de PID: `pids.max`
- Implementación de un resource limiter para un proceso

### 39.10 Capabilities y Privilegios
- `capabilities(7)`: lista completa, bounding set, ambient set
- `cap_from_text()`, `cap_set_proc()`, `cap_get_proc()`
- Eliminación de privilegios: `cap_set_proc()` + `prctl(PR_CAP_AMBIENT)`
- Mejores prácticas: mínimo privilegio, heredabilidad

### 39.11 Ejercicios
- Escribir un servidor echo TCP con io_uring (Linux)
- Crear una herramienta tipo `strace` minimalista usando `ptrace()`
- Implementar un daemon que se fork y ejecute en segundo plano
- Construir un contenedor Linux mínimo con namespaces + cgroups
- Escribir un programa que use `signalfd` para manejar señales con poll

---

## Capítulo 40 — Proyectos de Síntesis y Portafolio

### 40.1 Proyecto A: Navegador Web Minimalista
- Parser HTML (tag, atributos, texto, anidamiento)
- Parser CSS (selectores, propiedades, cascade, especificidad)
- Layout engine (box model, block/inline, positioning, flexbox básico)
- Render engine (pintado de texto, cajas, colores, fondo)
- Networking: HTTP/1.1, conexiones, TLS
- Interactividad: scroll, clics, formularios básicos
- Objetivo: renderizar una página web simple correctamente

### 40.2 Proyecto B: Emulador de Consola Retro (Chip-8, Game Boy)
- CPU: instrucciones, registros, flags, ciclo de fetch-decode-execute
- Memoria: ROM, RAM, stack, registros de I/O
- GPU: framebuffer, sprites, tiles, scroll, interrupciones por línea
- Audio: generación de tonos simples
- Input: mapeo de teclado a controles de la consola
- Frontend: SDL2 para video y audio
- Objetivo: emular y jugar un ROM comercial

### 40.3 Proyecto C: Motor de Búsqueda de Texto Completo
- Crawler: descarga de páginas web, extracción de texto
- Indexador: inverted index, tokenización, stemming, stop words
- Rankeado: TF-IDF, BM25, PageRank simplificado
- Query engine: parsing de consultas, boolean model, ranked retrieval
- Almacenamiento: índice en disco (B-tree o mmap)
- Frontend: servidor HTTP con interfaz de búsqueda
- Objetivo: indexar un conjunto de documentos y responder consultas

### 40.4 Proyecto D: Editor de Audio Digital (DAW simplificado)
- Carga/guardado de archivos WAV, MP3 (con libmpg123)
- Visualización de forma de onda (SDL2, OpenGL)
- Edición: cortar, copiar, pegar, silenciar, fade in/out
- Efectos: EQ paramétrico, compresor, reverb (convolución o Schroeder)
- Mezcla multicanal: faders, panner, envíos a efectos
- Reproducción en tiempo real: PortAudio, buffer circular
- Objetivo: cargar pistas de audio, aplicar efectos y exportar

### 40.5 Proyecto E: Sistema de Archivos en Espacio de Usuario (FUSE)
- FUSE (Filesystem in Userspace): `fuse_main()`, `fuse_operations`
- Operaciones: getattr, readdir, open, read, write, create, unlink, mkdir, rmdir
- Almacenamiento: archivo de respaldo en disco o en memoria
- Directorios: árbol, búsqueda, creación de rutas
- Metadatos: permisos, timestamps, tamaño
- Extensiones: compresión, cifrado, journaling
- Objetivo: montar y usar el sistema de archivos como un directorio real

### 40.6 Proyecto F: Lenguaje de Programación desde Cero
- Nombre del lenguaje: propuesta del estudiante
- Lexer: tokens completos (identificadores, literales, operadores, keywords)
- Parser: recursive descent, AST completo
- Semantic analysis: type checking, scope resolution
- IR: bytecode stack-based o tres direcciones
- VM: intérprete de bytecode o JIT
- Standard library: I/O, string, math, file, collections
- Objetivo: escribir y ejecutar un programa no trivial en el lenguaje creado

### 40.7 Proyecto G: Framework de Machine Learning Embebido
- Modelos pequeños: MLP, CNN pequeña, MobileNet
- Cuantización INT8: calibración, quantized ops (conv, gemm, pooling)
- Runtime sin dependencias: puro C, sin malloc (arena allocator)
- Microcontroladores: deploy en STM32 / ESP32 / Raspberry Pi Pico
- Demo: clasificación de dígitos, detección de palabras clave (keyword spotting)
- Objetivo: inferir un modelo ML en un microcontrolador a < 100ms

### 40.8 Proyecto H: Cliente/Servidor de Streaming de Video
- Captura: V4L2 (cámara) o archivo de video
- Codificación: libx264 (H.264) en tiempo real
- Protocolo: RTMP, HLS, o protocolo propio sobre UDP
- Servidor: pool de hilos, sesiones, bitrate adaptation
- Cliente: reproducción con SDL2 + decodificación con FFmpeg
- Objetivo: transmitir video en vivo de una cámara a un reproductor

### 40.9 Proyecto I: Base de Datos SQL Relacional Completa
- SQL parser: SELECT, INSERT, CREATE TABLE, WHERE, JOIN, GROUP BY, ORDER BY, LIMIT
- Query planner: índices, join ordering (nested loop, hash join)
- Executor: volcano model, operadores, evaluación de expresiones
- Storage engine: B+Tree persistente, páginas, buffer pool
- Transacciones: WAL, COMMIT, ROLLBACK, MVCC simplificado
- CLI: interfaz de línea de comandos estilo sqlite3
- Objetivo: ejecutar consultas SQL complejas sobre datos persistentes

### 40.10 Proyecto J: Implementación de un Protocolo de Consenso (Raft)
- Raft completo: leader election, log replication, safety, membership changes
- Cluster: 3 o 5 nodos, comunicación por UDP/TCP
- State machine: key-value store replicada
- Persistencia: logs y snapshot en disco
- Cliente: set, get, watch (cambios en tiempo real)
- Objetivo: cluster tolerante a fallos que sobreviva la caída de un nodo

### 40.11 Guía de Portafolio y Publicación
- README profesional: descripción, capturas, instalación, uso, ejemplos
- Código limpio: estilo consistente, sin warnings, documentado
- Tests: unitarios + integración, cobertura > 80%
- CI/CD: GitHub Actions (compilar, testear, sanitizers, valgrind)
- Empaquetado: Makefile, CMake, Homebrew formula, Docker
- Publicación: GitHub, Reddit (r/C_Programming), Hacker News
- Objetivo: proyecto que demuestre dominio de C a nivel de ingeniería de software

---



## Apéndices

### A — Tabla de Operadores en C (precedencia y asociatividad)
### B — Tabla de Especificadores de Formato `printf` / `scanf`
### C — Secuencias de Escape y Caracteres Especiales
### D — Diferencias entre C17 y C23
### E — Lecturas Recomendadas y Referencias

| Recurso | Autor | Tipo |
|---------|-------|------|
| *The C Programming Language (2nd ed.)* | Kernighan & Ritchie | Libro |
| *C: A Reference Manual* | Harbison & Steele | Libro |
| *Modern C* | Jens Gustedt | Libro |
| *21st Century C* | Ben Klemens | Libro |
| *Expert C Programming: Deep C Secrets* | Peter van der Linden | Libro |
| *C Traps and Pitfalls* | Andrew Koenig | Libro |
| | | |
| cppreference.com | — | Web |
| ISO/IEC 9899:2024 (C23) | ISO | Estándar |
| Linux man pages | — | Documentación |

### F — Guía Rápida de GDB
### G — Cheatsheet de Makefile
### H — Plantilla de Proyecto en C
