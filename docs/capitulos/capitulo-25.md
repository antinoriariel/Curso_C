---
tags:
  - Experto
  - Análisis estático
  - Verificación formal
---

# Capítulo 25 — Análisis Estático y Verificación Formal

!!! abstract "Objetivos de aprendizaje"
    - Usar *linters* y analizadores estáticos (clang-tidy, cppcheck, Coverity).
    - Comprender el análisis de flujo de datos, la ejecución simbólica y la
      interpretación abstracta.
    - Verificar formalmente con **Frama-C/ACSL** y *model checking*.

---

## 25.1 Introducción

El análisis **estático** examina el código **sin ejecutarlo** para hallar bugs y
demostrar propiedades. Frente al **dinámico** (ASan, fuzzing), no necesita casos
de prueba y puede cubrir *todas* las rutas, pero produce **falsos positivos**.

---

## 25.2 Linting y análisis

```bash
clang-tidy programa.c -- -std=c17     # chequeos de estilo, bugs, modernización
cppcheck --enable=all programa.c       # análisis ligero, pocos falsos positivos
scan-build gcc programa.c              # Clang Static Analyzer
```

Detectan: punteros nulos, fugas, *use-after-free*, índices fuera de rango,
desbordamientos, variables sin usar.

---

## 25.3–25.4 Flujo de datos y ejecución simbólica

- **Dataflow analysis**: propaga información (¿está inicializada esta variable?
  ¿puede ser nula?) por el grafo de control.
- **Symbolic execution**: ejecuta el programa con valores *simbólicos* en lugar
  de concretos, explorando rutas y generando entradas que las disparan
  (**KLEE** es la herramienta de referencia para C, sobre LLVM).

---

## 25.5 Frama-C y ACSL

**Frama-C** verifica C anotado con **ACSL** (*ANSI/ISO C Specification
Language*), un lenguaje de contratos (pre/postcondiciones, invariantes):

```c
/*@ requires n >= 0;
  @ requires \valid(a + (0..n-1));
  @ ensures \result >= 0;
  @ ensures \forall integer i; 0 <= i < n ==> a[i] <= \result;
  @*/
int maximo(const int *a, int n);
```

El plugin **WP** (*weakest precondition*) descarga las obligaciones de prueba a
demostradores automáticos (Alt-Ergo, Z3). Si todo se prueba, la función cumple su
contrato **para toda entrada**.

---

## 25.6–25.8 Interpretación abstracta, model checking y verificación deductiva

- **Interpretación abstracta**: razona sobre conjuntos de estados (intervalos,
  octágonos). **Astrée** demostró ausencia de errores en tiempo de ejecución en
  el software de vuelo del Airbus A380.
- **Model checking**: explora exhaustivamente el espacio de estados (CBMC para C).
- **Verificación deductiva**: pruebas asistidas (Frama-C/WP, VST sobre Coq).

---

## 25.9–25.11 Análisis dinámico asistido, concurrencia y certificación

Sanitizers (ASan/TSan/UBSan), análisis de concurrencia, y certificación de
software crítico (DO-178C en aviónica, ISO 26262 en automoción, IEC 62304 en
dispositivos médicos).

---

## Conexión con la actualidad

El análisis estático y la verificación formal han pasado de nicho académico a
**requisito industrial**. La presión regulatoria por la *memory safety*
(CISA/NSA, 2023–2024) impulsa la adopción de SAST en la CI de todo proyecto serio:
**CodeQL** (GitHub), **Coverity** y **Semgrep** escanean automáticamente cada
*pull request*. En software crítico, la verificación formal es ya obligatoria o
fuertemente recomendada: el microkernel **seL4** está **probado formalmente**
correcto (de la especificación al binario), y empresas como AWS usan métodos
formales (proyecto **s2n-tls**, verificado) en su criptografía en C. El
*fuzzing* guiado por cobertura (**libFuzzer**, **AFL++**, **OSS-Fuzz**, cap. 30)
combina lo dinámico y lo estático y encuentra miles de bugs en software C real.
Dominar estas herramientas es lo que diferencia el «funciona en mi máquina» de la
ingeniería de software demostrablemente correcta.

---

## Ejercicios

!!! example "Ejercicio 25.1 — Caza de bugs con clang-tidy ★★"
    Pasa clang-tidy y cppcheck a un módulo de capítulos previos; corrige todo lo
    señalado y razona los falsos positivos.

!!! example "Ejercicio 25.2 — Contrato ACSL ★★★★"
    Anota con ACSL y verifica con Frama-C/WP una función de búsqueda binaria
    (demuestra que no accede fuera de límites).

!!! example "Ejercicio 25.3 — Ejecución simbólica ★★★★"
    Usa KLEE para generar automáticamente entradas que cubran todas las ramas de
    una función pequeña.

!!! example "Ejercicio 25.4 — CI con SAST ★★★"
    Configura GitHub Actions con CodeQL o Semgrep para tu repositorio del cap. 15.

---

## Referencias

- [Frama-C](https://frama-c.com/) y el manual de ACSL.
- *Principles of Program Analysis* (Nielson, Nielson, Hankin).
- [KLEE](https://klee.github.io/), [CBMC](https://www.cprover.org/cbmc/).
- seL4: *Comprehensive Formal Verification of an OS Microkernel* (Klein et al.).
