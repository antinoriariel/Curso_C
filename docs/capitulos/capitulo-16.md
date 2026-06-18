---
tags:
  - Avanzado
  - Numérico
  - Álgebra lineal
---

# Capítulo 16 — Cómputo Numérico y Álgebra Lineal

!!! abstract "Objetivos de aprendizaje"
    - Entender la **aritmética de punto flotante IEEE-754** y sus peligros.
    - Operar con vectores y matrices, y resolver sistemas lineales.
    - Usar **BLAS/LAPACK** y técnicas: interpolación, integración, **FFT**.
    - Conocer la precisión arbitraria (GMP/MPFR).

---

## 16.1 Fundamentos del cómputo numérico

El punto flotante IEEE-754 representa reales con precisión finita. Consecuencias
que **debes** interiorizar:

```c
if (0.1 + 0.2 == 0.3) puts("igual");   // ¡NO se imprime!
// 0.1 + 0.2 == 0.30000000000000004
```

```c
#include <math.h>
bool casi_igual(double a, double b, double eps) {
    return fabs(a - b) <= eps * fmax(fabs(a), fabs(b));  // comparación relativa
}
```

Conceptos clave: *epsilon* de la máquina (`DBL_EPSILON`), cancelación
catastrófica, `NaN`/`Inf`, y la pérdida de precisión al restar números cercanos.

---

## 16.2–16.3 Álgebra lineal, vectores y matrices

BLAS (*Basic Linear Algebra Subprograms*) define una API estándar en tres niveles
(vector-vector, matriz-vector, matriz-matriz); LAPACK construye encima
(factorizaciones, sistemas, autovalores). Implementaciones de alto rendimiento:
OpenBLAS, Intel MKL, BLIS.

```c
// Producto escalar (BLAS nivel 1 "a mano")
double dot(int n, const double *x, const double *y) {
    double s = 0.0;
    for (int i = 0; i < n; i++) s += x[i] * y[i];
    return s;
}
```

Almacenamiento *row-major* (C) vs *column-major* (Fortran/BLAS): cuidado al
interoperar.

---

## 16.4–16.5 Sistemas lineales y autovalores

- **Eliminación gaussiana** con pivoteo parcial; factorización **LU**.
- Sistemas simétricos definidos positivos: **Cholesky**.
- **QR** y métodos iterativos (Jacobi, Gauss-Seidel, gradiente conjugado).
- Autovalores/autovectores: método de la potencia, QR iterativo.

---

## 16.6–16.8 Interpolación, integración y FFT

```c
// Regla del trapecio compuesta
double trapecio(double (*f)(double), double a, double b, int n) {
    double h = (b - a) / n, s = (f(a) + f(b)) / 2.0;
    for (int i = 1; i < n; i++) s += f(a + i * h);
    return s * h;
}
```

La **Transformada Rápida de Fourier (FFT)** reduce la DFT de O(n²) a O(n log n).
Es la piedra angular del procesamiento de señales (cap. 17):

```c
// Cooley-Tukey radix-2 (esquema). Usa FFTW en producción.
void fft(complex double *x, int n);
```

---

## 16.9–16.11 Aleatoriedad, BigNum y MPFR/GMP

- **PRNGs**: evita `rand()` para simulación seria; usa PCG, xoshiro o
  `<random>`-equivalentes. Para criptografía, ver cap. 18.
- **Precisión arbitraria**: **GMP** (enteros/racionales grandes) y **MPFR**
  (flotantes de precisión arbitraria) para criptografía, teoría de números y
  cálculos exactos.

---

## Conexión con la actualidad

El cómputo numérico en C es la **base oculta de la era de la IA**: NumPy, SciPy,
PyTorch y TensorFlow delegan sus operaciones pesadas en bibliotecas BLAS/LAPACK
escritas en C/Fortran/ensamblador (OpenBLAS, MKL, cuBLAS). Cuando entrenas una
red neuronal en Python, el 99 % del tiempo de CPU/GPU se gasta en código C
vectorizado. En 2024–2025, la frontera está en la **precisión mixta** (FP16,
BF16, FP8) para acelerar el *deep learning* (cap. 38), en los formatos numéricos
nuevos como **Posit** y en la reproducibilidad numérica en hardware heterogéneo.
Dominar IEEE-754 y BLAS es lo que te permite escribir el *kernel* numérico que
está debajo de todo ese ecosistema (caps. 21 y 38).

---

## Ejercicios

!!! example "Ejercicio 16.1 — El test 0.1+0.2 ★"
    Demuestra el problema y escribe una función de comparación con tolerancia
    relativa y absoluta.

!!! example "Ejercicio 16.2 — Resolutor lineal ★★★"
    Implementa eliminación gaussiana con pivoteo parcial y resuelve un sistema
    3×3. Compara con LAPACK (`dgesv`).

!!! example "Ejercicio 16.3 — Integración numérica ★★"
    Integra una función con trapecio y Simpson; estudia el error frente a `n`.

!!! example "Ejercicio 16.4 — FFT ★★★★"
    Implementa una FFT radix-2 y verifica con FFTW. Úsala para detectar la
    frecuencia dominante de una señal sintética.

---

## Referencias

- *Numerical Recipes in C* (Press et al.) — clásico (con reservas sobre licencia).
- David Goldberg. *What Every Computer Scientist Should Know About Floating-Point Arithmetic*.
- [Netlib BLAS/LAPACK](https://www.netlib.org/lapack/), [OpenBLAS](https://www.openblas.net/), [FFTW](https://www.fftw.org/).
- [GMP](https://gmplib.org/) y [MPFR](https://www.mpfr.org/).
