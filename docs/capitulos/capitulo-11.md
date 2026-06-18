---
tags:
  - Avanzado
  - Estructuras de datos
  - Algoritmos
---

# Capítulo 11 — Estructuras de Datos en C

!!! abstract "Objetivos de aprendizaje"
    - Implementar listas, pilas, colas, árboles, tablas hash, *heaps* y grafos.
    - Razonar sobre **complejidad** (notación Big-O) y *trade-offs* de memoria.
    - Aplicar la gestión de memoria del cap. 7 a estructuras dinámicas reales.

---

## 11.1 Listas enlazadas

```c
typedef struct Nodo { int dato; struct Nodo *sig; } Nodo;

Nodo *insertar_inicio(Nodo *cabeza, int v) {
    Nodo *n = malloc(sizeof *n);
    if (!n) return cabeza;          // o gestionar el fallo
    n->dato = v; n->sig = cabeza;
    return n;                        // nueva cabeza
}
```

| Operación | Lista enlazada | Array |
|-----------|----------------|-------|
| Acceso por índice | O(n) | O(1) |
| Inserción al inicio | O(1) | O(n) |
| Localidad de caché | Mala | Excelente |

Variantes: doblemente enlazada, circular, con nodo centinela. En la práctica, un
*dynamic array* (cap. 7) suele ganar a una lista enlazada por la **localidad de
caché**, salvo cuando dominan las inserciones/borrados en el interior.

---

## 11.2 Pilas y colas

- **Pila (LIFO)**: `push`/`pop` en un extremo. Base de la recursión, *undo*,
  evaluación de expresiones.
- **Cola (FIFO)**: `enqueue`/`dequeue`. Planificadores, BFS, *buffers*.

Una cola circular sobre array fijo es la implementación más eficiente para
*ring buffers* (audio, redes):

```c
typedef struct { int buf[N]; size_t head, tail, count; } Cola;
bool encolar(Cola *c, int v) {
    if (c->count == N) return false;
    c->buf[c->tail] = v; c->tail = (c->tail + 1) % N; c->count++;
    return true;
}
```

---

## 11.3 Árboles

Árbol binario de búsqueda (BST): inserción/búsqueda O(log n) si está equilibrado,
O(n) en el peor caso.

```c
typedef struct Arbol { int v; struct Arbol *izq, *der; } Arbol;

Arbol *insertar(Arbol *r, int v) {
    if (!r) { Arbol *n = malloc(sizeof *n); *n = (Arbol){v, NULL, NULL}; return n; }
    if (v < r->v) r->izq = insertar(r->izq, v);
    else if (v > r->v) r->der = insertar(r->der, v);
    return r;
}
```

Árboles autoequilibrados: **AVL** y **rojo-negro** (este último usado en el
planificador CFS del kernel de Linux y en muchos `std::map`). Recorridos:
inorden (ordenado), preorden, postorden, por niveles (BFS).

---

## 11.4 Tablas hash

Acceso medio O(1). Necesitan una **función hash** y una estrategia de colisiones
(encadenamiento o direccionamiento abierto):

```c
size_t fnv1a(const char *s) {              // hash FNV-1a, simple y bueno
    size_t h = 1469598103934665603ULL;
    for (; *s; s++) { h ^= (unsigned char)*s; h *= 1099511628211ULL; }
    return h;
}
```

El **factor de carga** (elementos/cubetas) determina cuándo redimensionar
(*rehash*), típicamente al superar 0.7.

!!! warning "Hash flooding"
    Una función hash predecible permite a un atacante provocar colisiones masivas
    y degradar la tabla a O(n) (DoS algorítmico, CWE-407). Servidores y lenguajes
    modernos usan hashing con clave secreta (**SipHash**).

---

## 11.5 Heaps

Un *binary heap* sobre array implementa una **cola de prioridad** con
inserción/extracción O(log n). Base de Dijkstra, *Huffman* (cap. 17) y
planificadores.

```c
// hijo izq de i: 2i+1; hijo der: 2i+2; padre: (i-1)/2
```

---

## 11.6 Grafos

Representaciones: **lista de adyacencia** (dispersos) vs **matriz de adyacencia**
(densos). Algoritmos fundamentales: BFS, DFS, Dijkstra, Kruskal/Prim (MST),
ordenación topológica.

---

## 11.7 Estructuras avanzadas

- **Trie**: búsqueda por prefijos (autocompletado, IP routing).
- **Skip list**: alternativa probabilística a los árboles equilibrados (Redis la
  usa en los *sorted sets*).
- **Filtro de Bloom**: pertenencia probabilística con poca memoria.
- **Union-Find** (DSU): componentes conexas, casi O(1) amortizado.

---

## Conexión con la actualidad

Las estructuras de datos en C no son ejercicios académicos: **son** la
infraestructura del software que usamos a diario. Redis implementa *skip lists*,
tablas hash y *quicklists* en C puro; el planificador del kernel de Linux pasó de
un árbol rojo-negro (CFS) al planificador **EEVDF** en 2023–2024; SQLite (cap. 32)
es un B-Tree gigantesco. La elección de estructura determina si un servicio
escala a millones de peticiones o se cae. En 2025, con conjuntos de datos
enormes, ganan protagonismo las **estructuras *cache-friendly*** (B-Trees de
nodos anchos, *Swiss tables*) y las **probabilísticas** (Bloom/Cuckoo filters,
HyperLogLog), que cambian exactitud por una fracción de la memoria — vitales en
*big data* y bases de datos columnares (cap. 27).

---

## Ejercicios

!!! example "Ejercicio 11.1 — Lista genérica ★★★"
    Implementa una lista doblemente enlazada **genérica** (`void *dato`) con API
    completa y pruébala con Valgrind (cero fugas).

!!! example "Ejercicio 11.2 — Tabla hash ★★★"
    Construye una tabla hash *string → int* con encadenamiento y *rehash*
    automático. Úsala para el contador de palabras del cap. 5.

!!! example "Ejercicio 11.3 — Cola de prioridad ★★★"
    Implementa un *min-heap* y resuélvelo con él el camino más corto (Dijkstra)
    en un grafo pequeño.

!!! example "Ejercicio 11.4 — Filtro de Bloom ★★★★"
    Implementa un filtro de Bloom con *k* funciones hash y mide su tasa de falsos
    positivos frente a la teórica.

---

## Referencias

- Cormen, Leiserson, Rivest, Stein. *Introduction to Algorithms* (CLRS), 4.ª ed.
- Sedgewick & Wayne. *Algorithms*, 4.ª ed.
- Código fuente de Redis (`src/dict.c`, `src/t_zset.c`) — estructuras en C real.
- Aumasson & Bernstein. *SipHash: a fast short-input PRF*.
