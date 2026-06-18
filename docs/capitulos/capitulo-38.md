---
tags:
  - Maestría
  - Deep Learning
  - Tensores
---

# Capítulo 38 — C para Machine Learning: Deep Learning desde Cero

!!! abstract "Objetivos de aprendizaje"
    - Implementar un **tensor** N-dimensional y sus operaciones.
    - Construir un sistema de **autograd** (diferenciación automática reversa).
    - Ensamblar capas, optimizadores y un *training loop* completo en C.
    - Optimizar la **inferencia** (cuantización, *fusion*).

!!! note "El proyecto culminante de IA"
    Este capítulo construye un micro-framework de *deep learning* en C, integrando
    el álgebra lineal (cap. 16), la optimización de rendimiento (cap. 21) y los
    fundamentos del cap. 26.

---

## 38.1 Tensores N-dimensionales

Un tensor es un array multidimensional con *shape* y *strides* (cap. 5, 21):

```c
typedef struct {
    float  *datos;      // buffer contiguo
    int    *shape;      // dimensiones
    int    *strides;    // saltos por dimensión (para vistas sin copia)
    int     ndim;
    size_t  size;       // número total de elementos
} Tensor;

// Indexación genérica: offset = Σ idx[d] * strides[d]
float tensor_get(const Tensor *t, const int *idx);
```

Los *strides* permiten vistas, *transpose* y *broadcasting* sin copiar datos.

---

## 38.2 Álgebra lineal para deep learning

La operación dominante es la **multiplicación de matrices** (matmul), que consume
el grueso del cómputo. Una versión por bloques (*tiling*) amigable con la caché
(cap. 21):

```c
void matmul(const float *A, const float *B, float *C, int M, int N, int K) {
    for (int i = 0; i < M; i++)
        for (int k = 0; k < K; k++) {       // orden i-k-j: mejor localidad
            float a = A[i*K + k];
            for (int j = 0; j < N; j++)
                C[i*N + j] += a * B[k*N + j];
        }
}
```

En producción se delega a BLAS (cap. 16) o a *kernels* SIMD (cap. 21).

---

## 38.3–38.5 Capas, activaciones, pérdidas y optimizadores

- **Capas**: densa (lineal), convolución, *normalization*, *dropout*.
- **Activaciones**: ReLU, GELU, *softmax*; **pérdidas**: MSE, *cross-entropy*.
- **Optimizadores**: SGD, *momentum*, **Adam** (el estándar de facto).

```c
// Paso de Adam para un parámetro
m = b1*m + (1-b1)*g;  v = b2*v + (1-b2)*g*g;
double mh = m/(1-pow(b1,t)), vh = v/(1-pow(b2,t));
param -= lr * mh / (sqrt(vh) + eps);
```

---

## 38.6 Autograd desde cero

El **autograd** construye un grafo de cómputo en el *forward* y propaga gradientes
en el *backward* (*reverse-mode AD*). Cada operación registra cómo derivarse:

```c
typedef struct Valor {
    double dato, grad;
    struct Valor **padres; int n_padres;
    void (*backward)(struct Valor *);   // regla de la cadena local
} Valor;
// Tras forward, se recorre el grafo en orden topológico inverso llamando backward.
```

Este es exactamente el mecanismo que hay detrás de PyTorch y TensorFlow,
reducido a su esencia.

---

## 38.7–38.9 Entrenamiento, serialización e inferencia

- **DataLoader** y bucle de entrenamiento (*epochs*, *batches*, *shuffle*).
- **Serialización** de pesos (formato propio o compatible).
- **Inferencia optimizada**: cuantización INT8/INT4, *operator fusion*, *batching*
  — lo que hace posible ejecutar modelos en CPU/edge.

---

## Conexión con la actualidad

Este capítulo no es un ejercicio teórico: es **exactamente** cómo funcionan los
frameworks que mueven la IA actual. El proyecto **`micrograd`** de Andrej Karpathy
demostró que el autograd cabe en ~100 líneas; **`llm.c`**, también de Karpathy
(2024), entrena GPT-2 en **C/CUDA puro** en unos pocos miles de líneas, sin
PyTorch, con un rendimiento competitivo — una demostración rotunda de que el
*deep learning* es, en el fondo, C bien escrito sobre álgebra lineal. La
biblioteca **`ggml`** (cap. 26) lleva esa filosofía a la inferencia de LLMs en
cualquier dispositivo. Las tendencias de 2024–2025 —**cuantización** extrema
(1.58 bits, *ternary*), **precisión mixta** (FP8) y *kernels* fusionados como
**FlashAttention**— son, todas, problemas de ingeniería de C/CUDA de bajo nivel.
Construir tu propio micro-framework es la mejor forma de entender qué ocurre
realmente cuando entrenas o sirves un modelo.

---

## Ejercicios

!!! example "Ejercicio 38.1 — Tensor con broadcasting ★★★★"
    Implementa el tipo `Tensor` con *strides*, indexación, suma con *broadcasting*
    y `transpose` sin copia.

!!! example "Ejercicio 38.2 — Autograd escalar ★★★★"
    Implementa un autograd al estilo `micrograd` y entrena una neurona para una
    función simple.

!!! example "Ejercicio 38.3 — MLP para MNIST ★★★★★"
    Entrena un MLP de 2 capas sobre un subconjunto de MNIST, implementando
    *forward*, *backward* y Adam en C.

!!! example "Ejercicio 38.4 — Inferencia cuantizada ★★★★★"
    Cuantiza los pesos de tu MLP a INT8 y compara precisión y velocidad frente a
    FP32.

---

## Referencias

- Andrej Karpathy. *micrograd* y *llm.c* (GitHub) — lectura altamente recomendada.
- *Deep Learning* (Goodfellow, Bengio, Courville) — gratuito online.
- [ggml](https://github.com/ggml-org/ggml).
- Capítulos 16, 21 y 26 de este curso.
