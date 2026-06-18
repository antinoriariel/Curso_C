---
tags:
  - Experto
  - HPC
  - SIMD
  - Paralelismo
---

# Capítulo 21 — Computación de Alto Rendimiento (HPC)

!!! abstract "Objetivos de aprendizaje"
    - Optimizar para la jerarquía de memoria (caché) y el *pipeline* de la CPU.
    - Vectorizar con **SIMD** (SSE/AVX/NEON) e intrínsecas.
    - Paralelizar con **OpenMP** (memoria compartida) y **MPI** (distribuida).
    - Acelerar con **GPGPU** (OpenCL/CUDA) desde C.

---

## 21.1 Arquitectura de computadoras modernas

El rendimiento ya no lo limita la aritmética, sino el **acceso a memoria**. La
jerarquía de caché (L1 ~1 ns, L2, L3, RAM ~100 ns) hace que la **localidad** sea
decisiva:

```c
// Recorrido row-major (✔ amigable con la caché)
for (int i = 0; i < N; i++)
    for (int j = 0; j < N; j++) suma += A[i][j];

// Column-major en C (falla la caché en cada acceso): hasta 10x más lento
for (int j = 0; j < N; j++)
    for (int i = 0; i < N; i++) suma += A[i][j];
```

Otros factores: predicción de saltos, *false sharing* entre hilos, alineación.

---

## 21.2–21.3 Optimización y SIMD

**SIMD** (*Single Instruction, Multiple Data*) aplica una operación a varios
datos a la vez (4, 8, 16 *lanes*). Vía intrínsecas:

```c
#include <immintrin.h>   // AVX
void suma_avx(const float *a, const float *b, float *c, int n) {
    for (int i = 0; i < n; i += 8) {              // 8 floats por iteración
        __m256 va = _mm256_loadu_ps(a + i);
        __m256 vb = _mm256_loadu_ps(b + i);
        _mm256_storeu_ps(c + i, _mm256_add_ps(va, vb));
    }
}
```

A menudo basta con dejar **autovectorizar** al compilador (`-O3 -march=native`) y
verificar con `-fopt-info-vec`. NEON es el equivalente en ARM.

---

## 21.4 OpenMP

Paralelismo de memoria compartida con directivas `#pragma`:

```c title="compilar con -fopenmp"
#include <omp.h>
double suma = 0.0;
#pragma omp parallel for reduction(+:suma)
for (int i = 0; i < n; i++) suma += v[i];   // reparte el bucle entre hilos
```

OpenMP cubre `parallel`, `for`, `reduction`, `sections`, `tasks` y `simd`. Es la
forma más rápida de paralelizar código existente.

---

## 21.5 MPI

Para clústeres (memoria **distribuida**), MPI comunica procesos por paso de
mensajes:

```c title="mpicc; mpirun -np 4 ./a.out"
#include <mpi.h>
int rank, size;
MPI_Init(&argc, &argv);
MPI_Comm_rank(MPI_COMM_WORLD, &rank);
MPI_Comm_size(MPI_COMM_WORLD, &size);
MPI_Reduce(&local, &global, 1, MPI_DOUBLE, MPI_SUM, 0, MPI_COMM_WORLD);
MPI_Finalize();
```

---

## 21.6–21.7 GPGPU y programación heterogénea

Las GPU ofrecen miles de núcleos para problemas *data-parallel*. Desde C:
**CUDA** (NVIDIA), **OpenCL** (portable), **HIP** (AMD). El patrón: copiar datos
al *device*, lanzar un *kernel* con miles de hilos, copiar el resultado de vuelta.

---

## 21.8–21.9 Análisis de rendimiento y nube

`perf`, VTune, *roofline model*, y plataformas HPC en la nube (AWS ParallelCluster,
GPUs bajo demanda).

---

## Conexión con la actualidad

HPC es el motor de la revolución de la **IA generativa**: entrenar un LLM consume
semanas de cómputo en miles de GPUs, y todo el *stack* de bajo nivel —cuBLAS,
cuDNN, los *kernels* de PyTorch, las librerías de comunicación NCCL/MPI— está
escrito en C/C++/CUDA. En 2024–2025 dominan tres tendencias: la **precisión
reducida** (FP8, BF16) para multiplicar el rendimiento; la **programación
heterogénea** CPU+GPU+aceleradores (TPUs, NPUs); y SIMD cada vez más ancho
(**AVX-512**, y las extensiones vectoriales escalables **ARM SVE** y **RISC-V
V**). Saber escribir un *kernel* numérico que satura el ancho de banda de memoria
y vectoriza bien es una habilidad escasa y muy valorada. Es, además, la base
directa del capítulo 38 (deep learning desde cero).

---

## Ejercicios

!!! example "Ejercicio 21.1 — El coste de la caché ★★"
    Mide la diferencia de tiempo entre recorrer una matriz por filas y por
    columnas. Explica el resultado con la jerarquía de caché.

!!! example "Ejercicio 21.2 — SIMD a mano ★★★"
    Vectoriza un producto escalar con intrínsecas AVX y compáralo con la versión
    escalar y con la autovectorizada por `-O3`.

!!! example "Ejercicio 21.3 — Mandelbrot con OpenMP ★★★"
    Paraleliza el cálculo del conjunto de Mandelbrot y mide el *speedup* frente
    al número de hilos (ley de Amdahl).

!!! example "Ejercicio 21.4 — Reducción MPI ★★★★"
    Calcula π por Montecarlo distribuido en 4 procesos MPI.

---

## Referencias

- *Computer Architecture: A Quantitative Approach* (Hennessy & Patterson).
- *Optimizing software in C++* (Agner Fog) — aplicable a C; guías de microarquitectura.
- [OpenMP](https://www.openmp.org/), [MPI Forum](https://www.mpi-forum.org/).
- Intel Intrinsics Guide; ARM NEON / SVE documentation.
