---
tags:
  - Maestría
  - Sistemas distribuidos
  - P2P
---

# Capítulo 34 — Sistemas Distribuidos y Redes P2P

!!! abstract "Objetivos de aprendizaje"
    - Comprender los retos de los sistemas distribuidos (CAP, FLP, fallos).
    - Implementar **RPC** y el algoritmo de consenso **Raft**.
    - Conocer sistemas de archivos distribuidos, redes **P2P** y mensajería.

---

## 34.1 Fundamentos

Un sistema distribuido coordina nodos que fallan de forma independiente y se
comunican por una red poco fiable. Resultados teóricos clave:

- **Teorema CAP**: ante una partición de red, eliges entre Consistencia y
  Disponibilidad.
- **Teorema FLP**: no hay consenso determinista garantizado en un sistema
  asíncrono con un fallo — de ahí los algoritmos probabilísticos y con
  *timeouts*.
- Falacias de la computación distribuida (la red **no** es fiable, ni de latencia
  cero, ni de ancho de banda infinito).

---

## 34.2 RPC

La **llamada a procedimiento remoto** abstrae la red como una llamada a función.
Requiere serialización (cap. 35), gestión de fallos y *timeouts*. Implementaciones:
gRPC, Apache Thrift, Cap'n Proto.

---

## 34.3 Raft: consenso

**Raft** es un algoritmo de consenso diseñado para ser *comprensible* (frente a
Paxos). Mantiene un **log replicado** consistente entre nodos mediante:

1. **Elección de líder** (con *terms* y *timeouts* aleatorizados).
2. **Replicación del log** (el líder propaga entradas).
3. **Seguridad** (solo se *commitea* lo replicado en la mayoría).

```c
typedef enum { SEGUIDOR, CANDIDATO, LIDER } Estado;
typedef struct {
    Estado estado;
    uint64_t term_actual;     // término actual
    int voto_para;            // a quién votó en este término
    LogEntry *log; size_t n;  // log replicado
    uint64_t commit_index;
} NodoRaft;
```

---

## 34.4–34.6 Otros consensos, FS distribuidos y P2P

- **Consenso adicional**: Paxos, Zab (ZooKeeper), PBFT (bizantino, *blockchain*).
- **Sistemas de archivos distribuidos**: GFS/HDFS, Ceph.
- **P2P**: tablas hash distribuidas (**DHT**, Kademlia — usada por BitTorrent e
  IPFS), *gossip protocols*.

---

## 34.7–34.8 Mensajería y tolerancia a fallos

Colas y *streaming* (Kafka, NATS), replicación, *sharding*, detección de fallos
(*heartbeats*, *phi accrual*), *quorum*.

---

## Conexión con la actualidad

La infraestructura distribuida que sostiene la nube está construida sobre C/C++:
**etcd** y **Consul** (basados en Raft) coordinan Kubernetes; **ScyllaDB** y
**TiKV** (sobre RocksDB en C++) ofrecen bases de datos distribuidas de alto
rendimiento; **Redis Cluster** y **Kafka** mueven datos a escala planetaria. Raft
se ha convertido en el algoritmo de consenso de facto por su comprensibilidad, y
existen implementaciones en C como **willemt/raft** y la de **Dragonboat**. En
2024–2025, la frontera está en reducir la latencia del consenso (variantes como
**EPaxos**, **Raft con *leases***) y en los sistemas P2P descentralizados
(**IPFS**, **libp2p**). Implementar Raft —aunque sea una versión de juguete— es
uno de los ejercicios más formativos sobre concurrencia (cap. 12), redes (cap. 13)
y razonamiento ante fallos.

---

## Ejercicios

!!! example "Ejercicio 34.1 — RPC propio ★★★"
    Implementa un RPC mínimo sobre TCP con serialización binaria y manejo de
    *timeouts*.

!!! example "Ejercicio 34.2 — Elección de líder Raft ★★★★★"
    Implementa la fase de elección de líder de Raft con 3 nodos y *timeouts*
    aleatorizados.

!!! example "Ejercicio 34.3 — Log replicado ★★★★★"
    Añade replicación del log y *commit* por mayoría; prueba la consistencia ante
    la caída de un nodo.

!!! example "Ejercicio 34.4 — Gossip ★★★★"
    Implementa un protocolo *gossip* de difusión de pertenencia entre nodos.

---

## Referencias

- Ongaro & Ousterhout. *In Search of an Understandable Consensus Algorithm (Raft)*.
- *Designing Data-Intensive Applications* (Martin Kleppmann).
- [The Raft website](https://raft.github.io/) (con visualización interactiva).
- Lamport. *Paxos Made Simple*; *Time, Clocks, and the Ordering of Events*.
