---
tags:
  - Maestría
  - Robótica
  - Control
---

# Capítulo 37 — Robótica y Sistemas de Control

!!! abstract "Objetivos de aprendizaje"
    - Comprender el bucle percepción-decisión-actuación y el control en tiempo real.
    - Implementar un **controlador PID** y filtros (media móvil, Kalman).
    - Conocer **ROS**, el control de motores y la fusión de sensores.

---

## 37.1 Fundamentos de robótica

Un robot cierra el bucle: **sensar → decidir → actuar**, con restricciones de
tiempo real (cap. 20). C domina el control de bajo nivel por su determinismo y
acceso al hardware.

---

## 37.2 Control PID

El controlador **PID** (Proporcional-Integral-Derivativo) es el caballo de
batalla del control: corrige el error entre el valor deseado (*setpoint*) y el
medido.

```c
typedef struct { double kp, ki, kd, integral, prev_err; } PID;

double pid_update(PID *c, double setpoint, double medido, double dt) {
    double err = setpoint - medido;
    c->integral += err * dt;
    double deriv = (err - c->prev_err) / dt;
    c->prev_err = err;
    return c->kp*err + c->ki*c->integral + c->kd*deriv;
}
```

!!! tip "Detalles que importan"
    En producción, añade *anti-windup* (limitar la integral), saturación de la
    salida y filtrado del término derivativo. El **ajuste** (Ziegler-Nichols,
    manual) es tan importante como la fórmula.

---

## 37.3 Filtros para robótica

- **Media móvil** y filtro paso-bajo: suavizan ruido de sensores.
- **Filtro de Kalman**: estimación óptima fusionando predicción y medición con
  incertidumbre — base de la navegación y el *tracking*.
- **Filtro complementario**: fusión barata de giroscopio + acelerómetro (IMU).

---

## 37.4 ROS desde C

**ROS** (*Robot Operating System*) estructura el software robótico en **nodos**
que se comunican por **tópicos** (publicador/suscriptor) y **servicios**. ROS 2
usa **DDS** y su capa C es **rcl**:

```c
// ROS 2 (rcl/rclc): publicar en un tópico
rcl_publish(&publisher, &mensaje, NULL);
```

---

## 37.5–37.8 Motores, sensores, planificación y tiempo real

- **Motores**: PWM (cap. 20), control de servos, *steppers*, lazo de velocidad/
  posición.
- **Sensores y fusión**: IMU, encoders, LIDAR, *odometry*.
- **Planificación de movimiento**: A\*, RRT, campos potenciales.
- **Tiempo real**: RTOS (cap. 20), Linux con `PREEMPT_RT`, latencias acotadas.

---

## Conexión con la actualidad

La robótica vive un momento explosivo —robots humanoides, drones autónomos,
vehículos, automatización industrial— y su capa de control en tiempo real es,
casi universalmente, C/C++. **ROS 2** se ha consolidado como el estándar de la
industria y la investigación, con su núcleo en C/C++ y un creciente uso de
**`PREEMPT_RT`** (mainline en el kernel de Linux desde 2024) para control
determinista sobre Linux. La gran tendencia de 2024–2025 es la fusión de control
clásico (PID, Kalman) con **aprendizaje por refuerzo** y modelos de visión: la
*policy* puede entrenarse en Python/GPU, pero su **inferencia y el lazo de
control** se ejecutan en C en el robot, por latencia y fiabilidad. El control PID
y el filtro de Kalman, con casi un siglo y medio siglo respectivamente, siguen
siendo el primer recurso en cualquier sistema que se mueve en el mundo físico.

---

## Ejercicios

!!! example "Ejercicio 37.1 — PID en simulación ★★★"
    Controla un sistema simulado (p. ej., temperatura o posición de un carro) con
    tu PID; ajusta las ganancias y grafica la respuesta.

!!! example "Ejercicio 37.2 — Filtro complementario ★★★"
    Fusiona datos simulados de acelerómetro y giroscopio para estimar el ángulo
    de inclinación.

!!! example "Ejercicio 37.3 — Filtro de Kalman 1D ★★★★"
    Implementa un filtro de Kalman unidimensional para estimar la posición a
    partir de mediciones ruidosas.

!!! example "Ejercicio 37.4 — Planificación A\\* ★★★★"
    Planifica una ruta en una rejilla con obstáculos usando A\*.

---

## Referencias

- *Modern Robotics* (Lynch & Park) — gratuito.
- *Probabilistic Robotics* (Thrun, Burgard, Fox) — para Kalman y fusión.
- [ROS 2 Documentation](https://docs.ros.org/).
- Åström & Murray. *Feedback Systems* (gratuito) — teoría de control.
