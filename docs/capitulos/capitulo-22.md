---
tags:
  - Experto
  - Juegos
  - Gráficos
---

# Capítulo 22 — Desarrollo de Juegos en C

!!! abstract "Objetivos de aprendizaje"
    - Estructurar un juego: *game loop*, gestión de estado y *delta time*.
    - Renderizar 2D con **SDL2** y 3D con **OpenGL** desde C.
    - Implementar física básica, sonido y red para juegos.

---

## 22.1 Arquitectura de un videojuego

El corazón es el **game loop**, desacoplado del *framerate* mediante *delta time*
y, idealmente, con paso de física fijo:

```c
double anterior = ahora();
double acumulador = 0.0;
const double DT = 1.0 / 60.0;   // física a 60 Hz

while (corriendo) {
    double actual = ahora(), frame = actual - anterior;
    anterior = actual;
    acumulador += frame;

    procesar_entrada();
    while (acumulador >= DT) { actualizar_fisica(DT); acumulador -= DT; }
    renderizar(acumulador / DT);   // interpolación
}
```

Patrones útiles: *Entity-Component-System* (ECS), máquinas de estado, *object
pools* (cap. 7).

---

## 22.2 Gráficos 2D con SDL2

```c title="-lSDL2"
#include <SDL2/SDL.h>
SDL_Init(SDL_INIT_VIDEO);
SDL_Window *w = SDL_CreateWindow("Juego", 0, 0, 800, 600, 0);
SDL_Renderer *r = SDL_CreateRenderer(w, -1, SDL_RENDERER_ACCELERATED);

SDL_SetRenderDrawColor(r, 0, 0, 0, 255);
SDL_RenderClear(r);
SDL_Rect jugador = {x, y, 32, 32};
SDL_SetRenderDrawColor(r, 255, 0, 0, 255);
SDL_RenderFillRect(r, &jugador);
SDL_RenderPresent(r);
```

SDL2 gestiona ventana, entrada, audio y texturas de forma multiplataforma.

---

## 22.3 Gráficos 3D con OpenGL

*Pipeline* programable: VBO/VAO, *shaders* (GLSL), matrices de modelo/vista/
proyección. Desde C se usa GLEW/GLAD para cargar funciones y GLM-equivalentes o
matemática propia para las matrices.

---

## 22.4–22.6 Física, sonido y red

- **Física**: integración (Euler semi-implícito, Verlet), detección de colisiones
  (AABB, SAT), respuesta de impulsos.
- **Sonido**: SDL_mixer, OpenAL.
- **Red**: modelo cliente-servidor autoritativo, *client-side prediction*,
  *lag compensation* sobre UDP (cap. 13).

---

## 22.7–22.8 Motores y demoscene

Motores en C: **raylib** (sencillo y didáctico), el motor de **Doom**/**Quake**
(id Tech, código abierto, C canónico). La **demoscene** lleva al límite el C y el
ensamblador para producir gráficos en binarios diminutos (4 KB).

---

## Conexión con la actualidad

Aunque los grandes estudios usan motores en C++ (Unreal) o C# (Unity), **C sigue
muy vivo** en el desarrollo de juegos a través de **raylib** (uno de los
proyectos de gamedev más populares de GitHub) y de la influyente charla
«*Handmade Hero*» de Casey Muratori, que reivindica escribir juegos desde cero en
C para entender y controlar el rendimiento. Los **motores históricos de id
Software** (Doom, Quake, Wolfenstein), liberados como código abierto, son
material de estudio canónico de C de sistemas. En 2024–2025, las APIs gráficas
modernas (**Vulkan**, exponiendo control de bajo nivel) y **WebAssembly**
(cap. 24, para llevar juegos en C al navegador con Emscripten) mantienen a C como
una opción legítima y de alto rendimiento para quien quiere entender la máquina.

---

## Ejercicios

!!! example "Ejercicio 22.1 — Pong ★★"
    Implementa Pong completo en SDL2: dos palas, pelota con rebotes, marcador.

!!! example "Ejercicio 22.2 — Snake ★★"
    El clásico Snake con crecimiento, colisiones y aumento de velocidad.

!!! example "Ejercicio 22.3 — Plataformas con física ★★★"
    Un juego de plataformas con gravedad, salto y colisiones AABB.

!!! example "Ejercicio 22.4 — Cubo 3D ★★★★"
    Renderiza y rota un cubo texturizado con OpenGL y *shaders* GLSL.

---

## Referencias

- *Game Programming Patterns* (Robert Nystrom) — gratuito online.
- *Game Engine Architecture* (Jason Gregory).
- [raylib](https://www.raylib.com/), [SDL](https://www.libsdl.org/).
- Código fuente de id Software (Doom, Quake) en GitHub.
