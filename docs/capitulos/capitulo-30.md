---
tags:
  - Experto
  - Seguridad
  - Código robusto
---

# Capítulo 30 — Desarrollo Seguro y Código Robusto

!!! abstract "Objetivos de aprendizaje"
    - Aplicar principios de programación segura y los estándares **MISRA-C** y
      **CERT C**.
    - Usar **fuzzing**, SAST y DAST en un flujo de trabajo real.
    - Conocer las mitigaciones (canarios de pila, ASLR, sandboxing) y los patrones
      de codificación segura.

---

## 30.1 Principios de programación segura

- **Validar toda entrada** (longitud, rango, formato) en la frontera de confianza.
- **Mínimo privilegio** y *fail-safe defaults*.
- **Defensa en profundidad**: varias capas, ninguna infalible.
- Tratar tamaños y longitudes como datos de primera clase (cap. 5).

---

## 30.2–30.3 MISRA-C y CERT C

- **MISRA-C**: directrices para C en sistemas críticos (automoción, aviónica).
  Restringe el lenguaje a un subconjunto seguro (sin recursión dinámica, sin
  *malloc* en muchos perfiles, reglas de conversión estrictas).
- **CERT C** (SEI/CMU): reglas y recomendaciones organizadas por tema (INT, MEM,
  STR, FIO…), cada una con ejemplo *conforme* y *no conforme*.

---

## 30.4 Fuzzing

El *fuzzing* alimenta entradas aleatorias/mutadas y guiadas por cobertura hasta
provocar fallos. Es **la** técnica que más bugs reales encuentra en C:

```c title="objetivo de libFuzzer"
#include <stddef.h>
#include <stdint.h>
int LLVMFuzzerTestOneInput(const uint8_t *data, size_t size) {
    parsear(data, size);   // la función bajo prueba
    return 0;
}
```

```bash
clang -fsanitize=fuzzer,address objetivo.c -o fuzz && ./fuzz
```

**AFL++**, **libFuzzer** y **OSS-Fuzz** (que fuzzea continuamente cientos de
proyectos *open-source*) han hallado decenas de miles de vulnerabilidades.

---

## 30.5–30.6 SAST y DAST

- **SAST** (estático, cap. 25): CodeQL, Coverity, clang-tidy en la CI.
- **DAST** (dinámico): ASan/UBSan/MSan, fuzzing, *penetration testing*.

---

## 30.7–30.9 Mitigaciones y vulnerabilidades comunes

| Vulnerabilidad (CWE) | Mitigación |
|----------------------|-----------|
| Buffer overflow (CWE-120) | Límites explícitos; `-fstack-protector-strong`; ASan |
| Use-after-free (CWE-416) | Anular punteros; ASan; MTE |
| Integer overflow (CWE-190) | `<stdckdint.h>`; UBSan |
| Format string (CWE-134) | `printf("%s", x)`; `-Wformat-security` |
| TOCTOU (CWE-367) | Operaciones atómicas; `openat` |
| Inyección de comandos (CWE-78) | Evitar `system()`; `execve` con args |

Mitigaciones del sistema/compilador: **ASLR**, **PIE**, **NX/DEP**, *stack
canaries*, **CFI**, **RELRO**, `_FORTIFY_SOURCE`.

---

## 30.8 Aislamiento y sandboxing

`seccomp-bpf` (restringe *syscalls*), *namespaces* y *capabilities* (cap. 39),
separación de privilegios (modelo de OpenSSH).

---

## 30.10–30.11 Patrones seguros y auditoría

Patrones: *RAII manual* con `goto cleanup` (cap. 3), inicialización a cero,
comprobación sistemática de retornos, *wrappers* seguros (`xmalloc` que aborta si
falla). Auditoría: revisión por pares, *threat modeling*, *checklists* CERT.

---

## Conexión con la actualidad

La seguridad de C es **el** debate de la ingeniería de software actual. Tras los
informes de **CISA/NSA** (2023–2024) y el plan de la Casa Blanca (ONCD) instando
a abandonar los lenguajes *memory-unsafe*, la industria ha respondido endureciendo
el C existente: *hardening flags* por defecto en Fedora/Ubuntu, *fuzzing*
obligatorio (OSS-Fuzz), SAST en cada PR y mitigaciones hardware (**ARM MTE**,
**CHERI**, **CET** de Intel para control de flujo). El incidente **xz/liblzma
(CVE-2024-3094, 2024)** —una puerta trasera introducida en una librería C de
compresión ubicua— recordó además que la seguridad no es solo del código, sino de
toda la **cadena de suministro**. La conclusión práctica del curso: en 2025 se
*puede* escribir C robusto, pero exige disciplina, herramientas (sanitizers,
fuzzing, SAST) y conocer las clases de vulnerabilidad. Eso es justo lo que este
capítulo —y este curso— te enseña.

---

## Ejercicios

!!! example "Ejercicio 30.1 — Encuentra y explota (en laboratorio) ★★★"
    Te damos un programa con un *buffer overflow* de pila. Detéctalo con ASan,
    explica cómo podría explotarse y corrígelo. (Solo en entorno controlado.)

!!! example "Ejercicio 30.2 — Fuzzing ★★★★"
    Escribe un objetivo libFuzzer para tu parser HTTP (cap. 28) o JSON (cap. 33)
    y deja que encuentre *crashes*; corrígelos.

!!! example "Ejercicio 30.3 — Hardening ★★"
    Recompila un proyecto con todas las protecciones (`-D_FORTIFY_SOURCE=2`,
    `-fstack-protector-strong`, PIE, RELRO) y verifica con `checksec`.

!!! example "Ejercicio 30.4 — Sandbox con seccomp ★★★★"
    Restringe un programa para que solo pueda usar `read`/`write`/`exit` mediante
    `seccomp-bpf`.

---

## Referencias

- *Secure Coding in C and C++* (Robert Seacord), 2.ª ed.
- [CERT C Coding Standard](https://wiki.sei.cmu.edu/confluence/display/c) (SEI/CMU).
- MISRA C:2012 (con enmiendas).
- [OSS-Fuzz](https://github.com/google/oss-fuzz), CWE/SANS *Top 25*.
