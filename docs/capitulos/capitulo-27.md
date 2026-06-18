---
tags:
  - Experto
  - Bases de datos
  - Almacenamiento
---

# Capítulo 27 — Bases de Datos y Motores de Almacenamiento

!!! abstract "Objetivos de aprendizaje"
    - Comprender el almacenamiento persistente y la jerarquía disco/memoria.
    - Implementar **B-Tree/B+Tree** y **LSM-Tree**, las dos grandes familias.
    - Conocer índices, procesamiento de consultas, transacciones, **WAL** y
      almacenamiento columnar.

---

## 27.1–27.2 Fundamentos y persistencia

Una base de datos garantiza **ACID** (Atomicidad, Consistencia, Aislamiento,
Durabilidad). El reto central: la memoria es volátil y rápida; el disco es
persistente y lento (aunque los SSD/NVMe estrechan la brecha). El diseño gira en
torno a minimizar las E/S y a sobrevivir a fallos.

```c
// Página: la unidad de E/S (típicamente 4 KB), alineada al bloque del disco
#define PAGE_SIZE 4096
typedef struct { uint8_t datos[PAGE_SIZE]; } Pagina;
```

---

## 27.3 B-Tree y B+Tree

El **B+Tree** es la estructura de índice dominante en bases de datos relacionales
(MySQL/InnoDB, PostgreSQL, SQLite). Árbol equilibrado de **nodos anchos**
(cientos de claves) que minimiza la altura y, por tanto, las lecturas de disco.
Las hojas se enlazan para recorridos por rango eficientes.

| | Búsqueda | Inserción | Rango |
|--|----------|-----------|-------|
| B+Tree | O(log n) | O(log n) | Excelente (hojas enlazadas) |

---

## 27.4 LSM-Tree

El **Log-Structured Merge-Tree** optimiza la **escritura**: acumula en memoria
(*memtable*) y vuelca a disco en archivos ordenados inmutables (**SSTables**),
fusionados periódicamente (*compaction*). Base de RocksDB, LevelDB, Cassandra,
ScyllaDB.

```text
Escritura → WAL + Memtable (RAM) → [flush] → SSTable L0 → [compaction] → L1, L2...
```

Compromiso clásico: B-Tree favorece lecturas; LSM favorece escrituras (mejor para
cargas *write-heavy* y SSD).

---

## 27.5–27.8 Índices, consultas, transacciones y WAL

- **Índices secundarios**: aceleran consultas por columnas no clave.
- **Procesamiento de consultas**: *parsing* (cap. 33), planificación, ejecución
  (*volcano model*).
- **Transacciones y concurrencia**: bloqueo en dos fases (2PL) o **MVCC**
  (*Multi-Version Concurrency Control*, usado por PostgreSQL).
- **Write-Ahead Log (WAL)**: escribe la intención **antes** de modificar los
  datos; permite recuperación tras un *crash* (durabilidad).

```c
// WAL: registrar antes de aplicar
escribir_wal(operacion);   // y fsync()
fsync(wal_fd);             // garantiza que llegó al disco
aplicar(operacion);        // ahora es seguro modificar las páginas
```

---

## 27.9–27.11 Columnar, SQL parser y BD embebidas

- **Almacenamiento columnar** (Apache Arrow, Parquet, DuckDB): guarda por columnas
  para análisis (OLAP), habilitando compresión y SIMD.
- **SQL parser**: lexer + parser (cap. 19/33) que produce un plan.
- **BD embebidas**: SQLite, LMDB (cap. 32).

---

## Conexión con la actualidad

Prácticamente todos los motores de almacenamiento del mundo están escritos en C o
C++: **SQLite** (la pieza de software más desplegada del planeta, en cada móvil y
navegador), **PostgreSQL**, **MySQL**, **Redis**, **RocksDB** y **LMDB**. La gran
tendencia de 2024–2025 es **DuckDB** —un motor analítico columnar embebido,
escrito en C++— que ha popularizado el análisis OLAP «en proceso», y **Apache
Arrow**, que estandariza un formato columnar en memoria para mover datos entre
sistemas sin serializar. En el frente *write-heavy*, los **LSM-Trees** dominan las
bases de datos distribuidas de la era *cloud*. Implementar un B+Tree y un WAL en C
te da la intuición exacta de por qué tu base de datos se comporta como lo hace, y
es la antesala directa del cap. 32 (SQLite internals).

---

## Ejercicios

!!! example "Ejercicio 27.1 — Pager ★★★"
    Implementa un gestor de páginas que lea/escriba páginas de 4 KB en un archivo,
    con una caché LRU en memoria.

!!! example "Ejercicio 27.2 — B+Tree ★★★★"
    Implementa inserción y búsqueda en un B+Tree en memoria; añade recorrido por
    rango.

!!! example "Ejercicio 27.3 — KV con WAL ★★★★"
    Extiende tu KV store del cap. 15-C con un *write-ahead log* y recuperación
    tras un *crash* simulado (mata el proceso a media escritura).

!!! example "Ejercicio 27.4 — LSM mínimo ★★★★"
    Implementa una *memtable* + *flush* a SSTable ordenada y una búsqueda que
    consulte memtable y SSTables en orden.

---

## Referencias

- *Database Internals* (Alex Petrov) — excelente sobre B-Tree/LSM y almacenamiento.
- *Designing Data-Intensive Applications* (Martin Kleppmann).
- Código fuente de SQLite (cap. 32), LevelDB y LMDB.
- *Architecture of a Database System* (Hellerstein, Stonebraker, Hamilton).
