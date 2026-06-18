---
tags:
  - Avanzado
  - Criptografía
  - Seguridad
---

# Capítulo 18 — Criptografía y Seguridad Informática

!!! abstract "Objetivos de aprendizaje"
    - Distinguir cifrado simétrico/asimétrico, *hashing* y firmas.
    - Implementar y, sobre todo, **usar correctamente** primitivas criptográficas.
    - Escribir **código de tiempo constante** y generar aleatoriedad segura.
    - Conocer ataques (canal lateral) y defensas.

!!! danger "Regla de oro de la criptografía"
    **No implementes tu propia criptografía para producción.** Estos ejercicios
    son para *entender* las primitivas; en sistemas reales usa libsodium, OpenSSL
    o BoringSSL, auditadas por expertos. Un error sutil = sistema roto.

---

## 18.1 Fundamentos

Objetivos de seguridad (CIA): **Confidencialidad**, **Integridad**,
**Autenticidad**. El principio de **Kerckhoffs**: la seguridad debe residir en la
clave, no en el secreto del algoritmo.

---

## 18.2 Cifrado simétrico

Misma clave para cifrar y descifrar. Estándar actual: **AES** (128/256 bits) en
modos de cifrado autenticado (**AES-GCM**, ChaCha20-Poly1305).

```c
// XOR cipher: SOLO didáctico. Inseguro, pero ilustra el concepto de cifrado en flujo.
void xor_cipher(uint8_t *datos, size_t n, const uint8_t *clave, size_t k) {
    for (size_t i = 0; i < n; i++) datos[i] ^= clave[i % k];
}
```

!!! warning "Modos y nonces"
    Nunca uses ECB (filtra patrones). Usa AEAD (GCM/Poly1305) y **nunca** repitas
    el *nonce* con la misma clave.

---

## 18.3 Cifrado asimétrico

Clave pública/privada. **RSA** (basado en factorización) y **curvas elípticas**
(ECC, más eficiente). Permiten intercambio de claves (Diffie-Hellman) y firma
digital.

---

## 18.4 Funciones hash

Resumen de tamaño fijo, unidireccional y resistente a colisiones. **SHA-256**,
**SHA-3**, **BLAKE2/3**. Para contraseñas, hashes **lentos y con sal**:
**Argon2**, scrypt, bcrypt — nunca SHA crudo.

```c
// HMAC: autenticación de mensajes con clave (esquema)
// HMAC(K, m) = H((K ⊕ opad) || H((K ⊕ ipad) || m))
```

---

## 18.5–18.6 ECC, autenticación y certificados

Curvas modernas: **Curve25519** (X25519 para intercambio, Ed25519 para firma).
Certificados X.509 y PKI: cadena de confianza que respalda TLS (cap. 13, 28).

---

## 18.7 Código de tiempo constante

Comparar secretos con `memcmp` filtra información por **canal lateral de tiempo**
(termina antes en el primer byte distinto). Compara en tiempo constante:

```c
int ct_memcmp(const void *a, const void *b, size_t n) {
    const uint8_t *x = a, *y = b; uint8_t d = 0;
    for (size_t i = 0; i < n; i++) d |= x[i] ^ y[i];  // sin retorno anticipado
    return d;   // 0 si iguales
}
```

---

## 18.8 Aleatoriedad segura

```c
// NO uses rand() para claves. En Linux/POSIX moderno:
#include <sys/random.h>
uint8_t clave[32];
getrandom(clave, sizeof clave, 0);   // o getentropy()
```

---

## 18.9–18.11 Ataques, aplicaciones y librerías

- **Ataques**: fuerza bruta, canal lateral (tiempo, caché, potencia), *padding
  oracle*, repetición.
- **Aplicaciones**: gestores de contraseñas, cifrado de archivos, firma de
  software.
- **Librerías**: **libsodium** (la recomendada por su API a prueba de errores),
  OpenSSL, BoringSSL, mbedTLS (embebidos).

---

## Conexión con la actualidad

La criptografía vive su mayor transición en décadas: la **criptografía
post-cuántica (PQC)**. En agosto de 2024 el NIST estandarizó los primeros
algoritmos resistentes a computadores cuánticos — **ML-KEM (Kyber)**, **ML-DSA
(Dilithium)** y **SLH-DSA (SPHINCS+)** — y las grandes implementaciones en C
(OpenSSL 3.x, BoringSSL, liboqs) ya los incorporan; navegadores y Cloudflare han
desplegado X25519+Kyber híbrido en TLS. Paralelamente, los **ataques de canal
lateral** siguen siendo críticos: Spectre/Meltdown y sus sucesores demostraron
que incluso código criptográficamente correcto puede filtrar secretos por la
microarquitectura, lo que ha hecho del **código de tiempo constante** una
disciplina obligatoria. Para auditar fugas de tiempo existen herramientas como
`dudect` y `ctgrind`. La lección del curso: usa bibliotecas auditadas, pero
entiende lo suficiente para usarlas **bien**.

---

## Ejercicios

!!! example "Ejercicio 18.1 — Cifrado César y Vigenère ★"
    Implementa y rompe ambos por análisis de frecuencias (didáctico).

!!! example "Ejercicio 18.2 — SHA-256 con librería ★★"
    Calcula el hash de un archivo usando libsodium/OpenSSL y verifícalo con
    `sha256sum`.

!!! example "Ejercicio 18.3 — Comparación de tiempo constante ★★★"
    Mide experimentalmente la diferencia de tiempo entre `memcmp` y `ct_memcmp`
    al comparar tokens secretos.

!!! example "Ejercicio 18.4 — Cifrado de archivos con libsodium ★★★★"
    Cifra y descifra un archivo con `crypto_secretstream` (AEAD), gestionando
    clave y *nonce* correctamente.

---

## Referencias

- *Serious Cryptography* (Jean-Philippe Aumasson), 2.ª ed.
- *Cryptography Engineering* (Ferguson, Schneier, Kohno).
- [libsodium](https://doc.libsodium.org/) — la API recomendada.
- NIST FIPS 203/204/205 (ML-KEM, ML-DSA, SLH-DSA), 2024.
