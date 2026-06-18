---
tags:
  - Experto
  - Redes
  - Protocolos
---

# Capítulo 28 — Protocolos de Red Avanzados

!!! abstract "Objetivos de aprendizaje"
    - Implementar y parsear HTTP/1.1, HTTP/2 y conocer **HTTP/3 + QUIC**.
    - Manejar **WebSockets**, **DNS** y **TLS 1.3**.
    - Conocer gRPC/Protobuf, BGP, túneles (VXLAN) y captura de paquetes.

---

## 28.1 HTTP/1.1 y HTTP/2

HTTP/1.1 es texto: línea de petición, cabeceras, cuerpo. Parsearlo correctamente
(y de forma segura frente a *request smuggling*) es un ejercicio clásico:

```text
GET /index.html HTTP/1.1\r\n
Host: ejemplo.com\r\n
\r\n
```

**HTTP/2** lo hace binario y multiplexado (varios *streams* sobre una conexión),
con compresión de cabeceras **HPACK**.

---

## 28.2 HTTP/3 y QUIC

**QUIC** reimplementa el transporte fiable **sobre UDP**, en espacio de usuario,
integrando TLS 1.3 y eliminando el *head-of-line blocking* de TCP. **HTTP/3**
corre sobre QUIC. Implementaciones en C: **quiche** (Cloudflare), **ngtcp2**,
**lsquic**, **msquic**.

---

## 28.3–28.5 WebSockets, DNS y TLS 1.3

- **WebSockets** (RFC 6455): canal bidireccional full-duplex sobre HTTP; *upgrade*
  con `Sec-WebSocket-Key` y tramas con máscara.
- **DNS** (RFC 1035): protocolo binario sobre UDP/TCP; construir y parsear
  consultas/respuestas a mano enseña mucho sobre formatos binarios.
- **TLS 1.3** (RFC 8446): menos *round-trips* (1-RTT, 0-RTT), suites modernas,
  *forward secrecy* obligatorio.

---

## 28.6–28.10 gRPC, BGP, túneles y captura

- **gRPC + Protocol Buffers**: RPC binario y eficiente con IDL y *codegen*.
- **BGP**: el protocolo de enrutamiento de Internet (entre sistemas autónomos).
- **VXLAN/túneles**: superponer redes virtuales (centros de datos, *overlays*).
- **Captura**: `libpcap` (tcpdump/Wireshark) y sockets raw (cap. 13).

```c
// Captura de paquetes con libpcap
pcap_t *h = pcap_open_live("eth0", 65535, 1, 1000, errbuf);
pcap_loop(h, 0, callback, NULL);   // callback por cada paquete
```

---

## Conexión con la actualidad

La pila de protocolos de Internet se está reescribiendo, y C está en el centro.
**HTTP/3 sobre QUIC** ya transporta una fracción enorme del tráfico web (Google,
Cloudflare, Meta lo despliegan a gran escala), y sus implementaciones de
referencia están en C (quiche, ngtcp2, msquic). **TLS 1.3** con criptografía
**post-cuántica híbrida** (X25519+ML-KEM, cap. 18) se está activando en 2024–2025
en los grandes CDNs. En el centro de datos, **eBPF/XDP** (cap. 23) permite
procesar paquetes a velocidad de línea (millones por segundo) en C dentro del
kernel, y proyectos como **Cilium** redefinen el *networking* de Kubernetes.
Entender y parsear estos protocolos a nivel de byte —con cuidado frente a
vulnerabilidades como *request smuggling* o *amplification*— es una competencia
clave en *backend* y seguridad de redes.

---

## Ejercicios

!!! example "Ejercicio 28.1 — Parser HTTP robusto ★★★"
    Parsea una petición HTTP/1.1 manejando cabeceras, `Content-Length` y casos
    malformados sin desbordar búferes.

!!! example "Ejercicio 28.2 — Cliente DNS ★★★★"
    Construye una consulta DNS binaria, envíala por UDP y parsea la respuesta
    (registros A).

!!! example "Ejercicio 28.3 — Servidor WebSocket ★★★★"
    Implementa el *handshake* y el *framing* de WebSocket para un eco
    bidireccional.

!!! example "Ejercicio 28.4 — Sniffer ★★★"
    Con libpcap, captura tráfico y muestra IP origen/destino y protocolo de cada
    paquete.

---

## Referencias

- RFC 9110/9112 (HTTP), RFC 9000 (QUIC), RFC 9114 (HTTP/3), RFC 8446 (TLS 1.3), RFC 6455 (WebSocket), RFC 1035 (DNS).
- *High Performance Browser Networking* (Ilya Grigorik) — gratuito.
- [quiche](https://github.com/cloudflare/quiche), [ngtcp2](https://github.com/ngtcp2/ngtcp2), [libpcap](https://www.tcpdump.org/).
