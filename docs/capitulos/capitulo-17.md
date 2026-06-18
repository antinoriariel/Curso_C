---
tags:
  - Avanzado
  - DSP
  - Multimedia
---

# Capítulo 17 — Procesamiento de Señales y Multimedia

!!! abstract "Objetivos de aprendizaje"
    - Comprender el muestreo, los filtros digitales (FIR/IIR) y la FFT aplicada.
    - Procesar **audio**, **imagen** y **vídeo** a bajo nivel.
    - Implementar compresión (RLE, Huffman) y conocer las librerías del ecosistema.

---

## 17.1 Fundamentos de DSP

Una señal continua se **muestrea** a una frecuencia `fs`. El teorema de
**Nyquist-Shannon** exige `fs > 2·f_max` para reconstruir sin *aliasing*. Una
señal digital es, en C, un array de muestras:

```c
// Genera un tono senoidal (PCM 16-bit)
for (int n = 0; n < N; n++)
    muestras[n] = (int16_t)(32767 * sin(2 * M_PI * freq * n / fs));
```

---

## 17.2 Filtros digitales

- **FIR** (respuesta finita): convolución con un *kernel*; siempre estable.
- **IIR** (respuesta infinita): usa retroalimentación; más eficiente, puede ser
  inestable.

```c
// FIR: y[n] = Σ h[k]·x[n-k]
double fir(const double *x, const double *h, int M, int n) {
    double y = 0.0;
    for (int k = 0; k < M; k++) if (n - k >= 0) y += h[k] * x[n - k];
    return y;
}
```

---

## 17.3 Procesamiento de audio

Formato **WAV** (cabecera RIFF + PCM). Leer, aplicar ganancia/eco/filtro y
escribir es un ejercicio canónico:

```c
struct WavHeader {            // empaquetada; 44 bytes
    char riff[4]; uint32_t size; char wave[4];
    char fmt[4];  uint32_t fmt_size; uint16_t audio_fmt, channels;
    uint32_t sample_rate, byte_rate; uint16_t block_align, bits;
    char data[4]; uint32_t data_size;
};
```

Efectos: ganancia, *delay*/eco, normalización, ecualización por FFT.

---

## 17.4–17.5 Imagen y vídeo

- **Imagen**: formatos PPM/BMP (sin compresión, ideales para aprender), luego
  PNG/JPEG vía libpng/libjpeg. Operaciones: escala de grises, convolución
  (desenfoque, *sharpen*, Sobel para bordes), umbralización.

```c
// Conversión a escala de grises (luminancia perceptual)
uint8_t gris = (uint8_t)(0.299*r + 0.587*g + 0.114*b);
```

- **Vídeo**: secuencia de fotogramas; conceptos de *codec*, *keyframes* e
  *inter-frame*. En la práctica se usa **FFmpeg/libav** (escrito en C).

---

## 17.6 Compresión de datos

- **RLE** (*run-length encoding*): básica, para datos repetitivos.
- **Huffman**: códigos de longitud variable óptimos (usa un *min-heap*, cap. 11).
- **LZ77/DEFLATE**: base de gzip/zlib y PNG.

```c
// Huffman: construir el árbol con una cola de prioridad de frecuencias
```

---

## 17.7–17.8 Señales biomédicas y librerías

Señales ECG/EEG: filtrado de ruido, detección de picos (QRS). Ecosistema C:
**FFmpeg**, **libsndfile**, **PortAudio**, **OpenCV** (C/C++), **libpng**,
**zlib**.

---

## Conexión con la actualidad

Todo el *streaming* moderno se apoya en C: **FFmpeg** —escrito en C— procesa el
vídeo de YouTube, Netflix y prácticamente toda la web; los *codecs* de última
generación **AV1**, **VVC/H.266** y el audio **Opus** tienen sus
implementaciones de referencia en C, exprimiendo SIMD (cap. 21) para alcanzar
tiempo real. En 2024–2025, el procesamiento de señales converge con el *machine
learning*: *super-resolución*, reducción de ruido y *codecs* neuronales combinan
DSP clásico (FFT, filtros) con redes entrenadas (caps. 26 y 38). Y en audio,
bibliotecas en C como **RNNoise** (supresión de ruido con una RNN) demuestran esa
fusión. Entender el procesamiento de señales a nivel de muestra es lo que permite
escribir o depurar estos sistemas.

---

## Ejercicios

!!! example "Ejercicio 17.1 — Generador de tonos WAV ★★"
    Escribe un WAV con un acorde (suma de senoides) y reprodúcelo.

!!! example "Ejercicio 17.2 — Filtro de imagen ★★★"
    Lee un PPM, aplica un desenfoque gaussiano y detección de bordes Sobel, y
    guarda el resultado.

!!! example "Ejercicio 17.3 — Eco de audio ★★★"
    Aplica un efecto de eco a un WAV mono variando *delay* y realimentación.

!!! example "Ejercicio 17.4 — Compresor Huffman ★★★★"
    Implementa codificación y decodificación Huffman de un archivo y mide el
    ratio de compresión.

---

## Referencias

- Steven W. Smith. *The Scientist and Engineer's Guide to Digital Signal Processing* (gratuito).
- *Introduction to Data Compression* (Khalid Sayood).
- [FFmpeg](https://ffmpeg.org/), [libsndfile](https://libsndfile.github.io/libsndfile/), [zlib](https://zlib.net/).
- Especificaciones de Opus (RFC 6716) y AV1.
