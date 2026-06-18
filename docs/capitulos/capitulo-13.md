---
tags:
  - Avanzado
  - Redes
  - Sockets
---

# Capítulo 13 — Programación de Redes con Sockets

!!! abstract "Objetivos de aprendizaje"
    - Comprender el modelo TCP/IP y la API de **sockets BSD**.
    - Implementar servidores y clientes **TCP** y **UDP**.
    - Escalar con **E/S multiplexada** (`select`, `poll`, `epoll`).
    - Conocer TLS con OpenSSL y los *raw sockets*.

---

## 13.1 Conceptos de redes

El modelo **TCP/IP** en cuatro capas: enlace, internet (IP), transporte
(TCP/UDP), aplicación (HTTP, DNS…). Un **socket** es el punto final de
comunicación, identificado por (IP, puerto). TCP es fiable, orientado a conexión
y ordenado; UDP es ligero, sin conexión y sin garantías.

!!! note "Orden de bytes de red"
    La red usa *big-endian*. Convierte siempre con `htons`/`htonl` (host→net) y
    `ntohs`/`ntohl` (net→host).

---

## 13.2–13.3 Servidor TCP completo

```c title="servidor_tcp.c"
#include <sys/socket.h>
#include <netinet/in.h>
#include <unistd.h>
#include <string.h>
#include <stdio.h>

int main(void) {
    int s = socket(AF_INET, SOCK_STREAM, 0);          // 1. crear socket
    int opt = 1;
    setsockopt(s, SOL_SOCKET, SO_REUSEADDR, &opt, sizeof opt);

    struct sockaddr_in dir = {0};
    dir.sin_family = AF_INET;
    dir.sin_addr.s_addr = INADDR_ANY;
    dir.sin_port = htons(8080);

    bind(s, (struct sockaddr *)&dir, sizeof dir);     // 2. asociar puerto
    listen(s, 16);                                    // 3. escuchar (backlog)

    for (;;) {
        int c = accept(s, NULL, NULL);                // 4. aceptar conexión
        char buf[1024];
        ssize_t n = recv(c, buf, sizeof buf, 0);      // 5. recibir
        if (n > 0) send(c, buf, n, 0);                // eco
        close(c);
    }
}
```

```bash
gcc servidor_tcp.c -o servidor && ./servidor
# en otra terminal:  nc localhost 8080
```

---

## 13.4–13.5 Cliente TCP y servidor UDP

```c
// Cliente TCP: connect() en lugar de bind/listen/accept
connect(s, (struct sockaddr *)&dir, sizeof dir);
send(s, "hola", 4, 0);

// UDP: SOCK_DGRAM, sin conexión; sendto/recvfrom
int u = socket(AF_INET, SOCK_DGRAM, 0);
recvfrom(u, buf, sizeof buf, 0, (struct sockaddr *)&cli, &len);
```

---

## 13.6 E/S multiplexada

Atender miles de conexiones con un solo hilo: en lugar de un hilo por cliente,
**multiplexa** con `epoll` (Linux), `kqueue` (BSD/macOS) o el portable `poll`.

```c
#include <sys/epoll.h>
int ep = epoll_create1(0);
struct epoll_event ev = {.events = EPOLLIN, .data.fd = s};
epoll_ctl(ep, EPOLL_CTL_ADD, s, &ev);

struct epoll_event eventos[64];
int n = epoll_wait(ep, eventos, 64, -1);   // bloquea hasta actividad
for (int i = 0; i < n; i++) { /* fd listo: aceptar o leer */ }
```

Este es el patrón **reactor** que usan Nginx, Redis y libuv (Node.js).

---

## 13.7–13.9 Protocolos, raw sockets y TLS

- **Protocolos de aplicación**: parsear líneas de petición/respuesta (HTTP,
  cap. 28).
- **Raw sockets** (`SOCK_RAW`, requieren privilegios): construir paquetes a mano,
  *ping*, *sniffers*.
- **TLS con OpenSSL**: envolver el socket en un `SSL *` para cifrado.

```c
SSL_CTX *ctx = SSL_CTX_new(TLS_client_method());
SSL *ssl = SSL_new(ctx);
SSL_set_fd(ssl, s);
SSL_connect(ssl);
SSL_write(ssl, datos, len);
```

---

## Conexión con la actualidad

Casi toda la infraestructura de Internet de alto rendimiento está escrita en C
sobre la API de sockets BSD que aprendes aquí: **Nginx**, **HAProxy**, **Redis**,
**bind**/**unbound** (DNS) y el propio *stack* TCP del kernel de Linux. La
frontera de 2024–2025 es **`io_uring`** (cap. 39), que sustituye el modelo
`epoll` por E/S asíncrona real con anillos compartidos, reduciendo drásticamente
las llamadas al sistema; y **QUIC/HTTP3** (cap. 28), que mueve el transporte
fiable a espacio de usuario sobre UDP para eliminar el *head-of-line blocking* de
TCP. En seguridad de transporte, **TLS 1.3** (cap. 28) y las suites
**post-cuánticas** (ML-KEM/Kyber, estandarizadas por el NIST en 2024) ya se están
desplegando en bibliotecas C como OpenSSL 3.x y BoringSSL.

---

## Ejercicios

!!! example "Ejercicio 13.1 — Servidor de eco ★★"
    Completa el servidor TCP de eco y pruébalo con `nc` y con tu propio cliente.

!!! example "Ejercicio 13.2 — Chat UDP ★★★"
    Implementa un chat multiusuario simple sobre UDP.

!!! example "Ejercicio 13.3 — Servidor concurrente con epoll ★★★★"
    Convierte el servidor de eco en uno que atienda 1000 conexiones simultáneas
    con `epoll` y un solo hilo (patrón reactor).

!!! example "Ejercicio 13.4 — Mini servidor HTTP ★★★★"
    Sirve archivos estáticos respondiendo a peticiones `GET` HTTP/1.1 básicas.
    *Proyecto acumulativo* hacia el cap. 35.

---

## Referencias

- W. Richard Stevens et al. *UNIX Network Programming, Vol. 1: The Sockets API*.
- *Beej's Guide to Network Programming* (gratuito, excelente).
- RFC 793 (TCP), RFC 768 (UDP), RFC 8446 (TLS 1.3).
- [Documentación de epoll](https://man7.org/linux/man-pages/man7/epoll.7.html).
