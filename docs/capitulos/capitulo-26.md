---
tags:
  - Experto
  - Machine Learning
  - IA
---

# Capítulo 26 — Machine Learning e Inteligencia Artificial en C

!!! abstract "Objetivos de aprendizaje"
    - Implementar regresión, redes neuronales (MLP, CNN, RNN) en C puro.
    - Entender el **autograd** y la diferenciación automática.
    - Conocer algoritmos clásicos (k-means, árboles, SVM) y la inferencia en
      producción.

!!! note "Relación con el capítulo 38"
    Este capítulo ofrece una panorámica amplia de ML en C; el **capítulo 38**
    profundiza en *deep learning* construyendo un framework con tensores y
    autograd desde cero. Aquí establecemos los cimientos.

---

## 26.1–26.2 Fundamentos y álgebra lineal para ML

ML = ajustar parámetros para minimizar una función de pérdida. El motor es el
**descenso de gradiente**. Todo se reduce a operaciones de álgebra lineal
(cap. 16): productos matriz-vector, que en C se implementan con bucles bien
ordenados (cap. 21).

---

## 26.3 Regresión lineal y logística

```c
// Regresión lineal por descenso de gradiente
void entrenar(double *w, double *b, const double *X, const double *y,
              int n, int d, double lr, int epocas) {
    for (int e = 0; e < epocas; e++) {
        for (int i = 0; i < n; i++) {
            double pred = *b;
            for (int j = 0; j < d; j++) pred += w[j] * X[i*d + j];
            double err = pred - y[i];
            for (int j = 0; j < d; j++) w[j] -= lr * err * X[i*d + j];
            *b -= lr * err;
        }
    }
}
```

La **logística** añade la sigmoide y la entropía cruzada para clasificación
binaria.

---

## 26.4–26.6 Redes neuronales

- **MLP** (perceptrón multicapa): capas densas + activaciones no lineales
  (ReLU, sigmoide); entrenamiento por **retropropagación**.
- **CNN**: convoluciones para visión; *pooling*; aprovechan la localidad espacial.
- **RNN/LSTM**: secuencias (texto, series temporales) con estado recurrente.

```c
double relu(double x)    { return x > 0 ? x : 0; }
double sigmoide(double x){ return 1.0 / (1.0 + exp(-x)); }
```

---

## 26.7 Autograd y diferenciación automática

El corazón del *deep learning*: en lugar de derivar a mano, se construye un
**grafo de cómputo** y se propaga el gradiente hacia atrás (*reverse-mode AD*).
Se desarrolla en detalle en el cap. 38.

---

## 26.8–26.11 Algoritmos clásicos

- **Árboles de decisión / Random Forest**: interpretables, sin necesidad de GPU.
- **k-means**: *clustering* no supervisado.
- **SVM**: clasificación con margen máximo.
- **PCA**: reducción de dimensionalidad (autovalores, cap. 16).

---

## 26.12–26.13 Inferencia y librerías

Para *desplegar* modelos en C: **ONNX Runtime**, **TensorFlow Lite**,
**llama.cpp**, **ggml**, **darknet**. La inferencia en C/C++ con cuantización
(INT8/INT4) es lo que permite ejecutar modelos en dispositivos sin GPU.

---

## Conexión con la actualidad

La explosión de la IA generativa ha puesto a C/C++ en el centro de la
**inferencia eficiente**. El proyecto **`llama.cpp`** de Georgi Gerganov —y su
biblioteca **`ggml`/`gguf`**, escritas en C— permitió por primera vez ejecutar
grandes modelos de lenguaje (LLaMA, Mistral, etc.) en portátiles y móviles
mediante **cuantización** agresiva, y se ha convertido en uno de los proyectos
más influyentes del ecosistema *open-source* de IA (es el motor detrás de Ollama
y de incontables aplicaciones locales). Esto confirma una tesis del curso:
mientras el *entrenamiento* vive en Python/CUDA, la **inferencia en el borde**
—móviles, IoT, *edge*— pertenece a C por su control de memoria y rendimiento. La
*tinyML* (modelos en microcontroladores, cap. 20) es otra frontera donde C es
insustituible.

---

## Ejercicios

!!! example "Ejercicio 26.1 — Regresión lineal ★★"
    Ajusta una recta a datos con ruido por descenso de gradiente; grafica la
    pérdida por época.

!!! example "Ejercicio 26.2 — Clasificador logístico ★★★"
    Clasifica un dataset 2D linealmente separable y dibuja la frontera de
    decisión.

!!! example "Ejercicio 26.3 — MLP para XOR ★★★★"
    Entrena una red de una capa oculta para resolver XOR con retropropagación
    implementada a mano.

!!! example "Ejercicio 26.4 — k-means ★★★"
    Agrupa puntos 2D en k clústeres y visualiza la convergencia.

---

## Referencias

- *Neural Networks from Scratch* (conceptos); Michael Nielsen, *Neural Networks and Deep Learning* (gratuito).
- Andrew Ng, *Machine Learning* (Coursera) — fundamentos.
- [llama.cpp / ggml](https://github.com/ggml-org/llama.cpp).
- *Pattern Recognition and Machine Learning* (Bishop).
