---
tags:
  - Maestría
  - Microservicios
  - APIs
---

# Capítulo 35 — Microservicios y APIs en C

!!! abstract "Objetivos de aprendizaje"
    - Construir servidores HTTP y APIs REST en C.
    - Serializar con Protobuf, MessagePack, CBOR y FlatBuffers.
    - Añadir autenticación, procesamiento asíncrono, contenerización y
      monitorización.

---

## 35.1 Fundamentos de microservicios

Arquitectura de servicios pequeños, independientes y comunicados por red (HTTP/
gRPC). C es una opción legítima cuando importan la latencia, el consumo de memoria
o el control fino — *sidecars*, *proxies*, servicios de datos.

---

## 35.2–35.3 Servidores HTTP y REST

```c title="REST con libmicrohttpd"
#include <microhttpd.h>
static enum MHD_Result handler(void *cls, struct MHD_Connection *c,
        const char *url, const char *method, const char *version,
        const char *upload, size_t *upload_size, void **con_cls) {
    const char *json = "{\"status\":\"ok\"}";
    struct MHD_Response *r = MHD_create_response_from_buffer(
        strlen(json), (void *)json, MHD_RESPMEM_PERSISTENT);
    MHD_add_response_header(r, "Content-Type", "application/json");
    enum MHD_Result ret = MHD_queue_response(c, MHD_HTTP_OK, r);
    MHD_destroy_response(r);
    return ret;
}
// MHD_start_daemon(MHD_USE_AUTO | MHD_USE_INTERNAL_POLLING_THREAD, 8080, ...)
```

Frameworks en C: **libmicrohttpd**, **Mongoose**, **Kore**, **Onion**, **facil.io**.

---

## 35.4 Serialización

| Formato | Tipo | Notas |
|---------|------|-------|
| JSON | Texto | Legible, ubicuo (cap. 33) |
| **Protobuf** | Binario + IDL | Compacto, con esquema y *codegen* |
| **MessagePack** | Binario | «JSON binario», sin esquema |
| **CBOR** (RFC 8949) | Binario | Estándar IETF, usado en COSE/IoT |
| **FlatBuffers** | Binario *zero-copy* | Acceso sin deserializar |

---

## 35.5–35.7 Auth, async y frameworks

- **Auth**: API keys, **JWT** (verificación de firma, cap. 18), OAuth2.
- **Asíncrono**: *event loop* (libuv, libev), colas de trabajo, *workers*.
- Integración con bases de datos (caps. 27, 32) y caché (Redis).

---

## 35.8–35.10 Despliegue, monitorización y API gateway

- **Contenerización**: imágenes Docker mínimas (`FROM scratch` con binario
  estático) — C produce imágenes de pocos MB.
- **Monitorización**: métricas Prometheus, *health checks*, *tracing*
  distribuido (OpenTelemetry).
- **API gateway**: enrutado, *rate limiting*, autenticación centralizada.

```dockerfile
FROM scratch
COPY servicio /servicio
ENTRYPOINT ["/servicio"]
# Imagen resultante: pocos MB, arranque en milisegundos
```

---

## Conexión con la actualidad

Aunque los microservicios suelen escribirse en Go, Java o Rust, **C domina la
infraestructura que los sostiene**: **Nginx** y **Envoy** (proxies/*service
mesh*), **HAProxy** (balanceo), **Redis** (caché/colas) y **Kafka** (*streaming*).
La tendencia de 2024–2025 hacia el *edge computing* y el *serverless* favorece
binarios diminutos y de arranque instantáneo —exactamente lo que C produce— y
WebAssembly/WASI (cap. 24) se perfila como el nuevo formato de despliegue de
funciones. En serialización, **Protobuf** y **gRPC** son el estándar de facto de
la comunicación entre servicios, y **CBOR** se impone en IoT y en los *passkeys*/
WebAuthn. Saber construir un servicio en C con un servidor HTTP eficiente, métricas
y una imagen de contenedor mínima es una habilidad diferencial en sistemas de
alto rendimiento.

---

## Ejercicios

!!! example "Ejercicio 35.1 — API REST ★★★"
    Construye una API CRUD (JSON) sobre tu KV store (cap. 15-C) con
    libmicrohttpd o Mongoose.

!!! example "Ejercicio 35.2 — JWT ★★★★"
    Implementa la verificación de un JWT firmado con HMAC-SHA256 (cap. 18) para
    proteger rutas.

!!! example "Ejercicio 35.3 — Protobuf ★★★"
    Define un esquema `.proto`, genera el código C (`protobuf-c`) y serializa/
    deserializa mensajes.

!!! example "Ejercicio 35.4 — Contenerización ★★★"
    Empaqueta tu servicio como binario estático en una imagen `FROM scratch` y
    expón métricas Prometheus básicas.

---

## Referencias

- [libmicrohttpd](https://www.gnu.org/software/libmicrohttpd/), [Mongoose](https://mongoose.ws/), [facil.io](https://facil.io/).
- *Building Microservices* (Sam Newman).
- [Protocol Buffers](https://protobuf.dev/), RFC 8949 (CBOR).
- [Envoy](https://www.envoyproxy.io/) y [Nginx](https://nginx.org/) como referencias de C de red.
