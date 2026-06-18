---
tags:
  - Avanzado
  - Embebidos
  - Microcontroladores
---

# Capítulo 20 — Sistemas Embebidos y Microcontroladores

!!! abstract "Objetivos de aprendizaje"
    - Programar microcontroladores *bare-metal*: GPIO, interrupciones, timers.
    - Manejar comunicación serial (UART, SPI, I²C), ADC/DAC y DMA.
    - Conocer los RTOS, el bajo consumo, el *firmware* seguro y **RISC-V**.

---

## 20.1–20.2 Fundamentos y entorno

Un microcontrolador (MCU) integra CPU, memoria (Flash + SRAM) y periféricos en un
chip. Restricciones: poca RAM (KB), sin sistema operativo (a menudo), tiempo real.
Herramientas: `arm-none-eabi-gcc`, OpenOCD, *linker scripts*, depuración por
JTAG/SWD.

```c
// Acceso a un registro mapeado en memoria (MMIO): volatile es OBLIGATORIO
#define GPIOA_ODR (*(volatile uint32_t *)0x48000014)
GPIOA_ODR |= (1u << 5);   // poner el pin 5 a alto
```

---

## 20.3–20.4 GPIO e interrupciones

```c
// Manejador de interrupción (ISR): corto, sin bloqueos, sin malloc
void EXTI0_IRQHandler(void) {
    if (EXTI->PR & 1) {       // ¿pendiente?
        EXTI->PR = 1;         // limpiar el flag (escribir 1)
        bandera_evento = 1;   // señalar al bucle principal (volatile)
    }
}
```

!!! warning "Reglas de oro de las ISR"
    Una ISR debe ser **breve**, no llamar a funciones no reentrantes (`printf`,
    `malloc`), y comunicarse con el código principal mediante variables
    `volatile` o colas *lock-free*. Las variables compartidas con una ISR deben
    ser `volatile` (y atómicas si superan el tamaño de palabra).

---

## 20.5–20.8 Timers, serial, ADC/DAC y DMA

- **Timers/PWM**: generar señales periódicas, controlar motores y brillo.
- **UART**: serie asíncrona; **SPI** e **I²C**: buses para sensores y memorias.
- **ADC/DAC**: convertir entre el mundo analógico y digital.
- **DMA**: mover datos entre periféricos y memoria **sin** intervención de la CPU
  — esencial para audio, redes y muestreo continuo.

---

## 20.9 RTOS

Un sistema operativo de tiempo real (**FreeRTOS**, **Zephyr**) ofrece tareas,
planificación por prioridades, *mutexes* y colas con **garantías temporales**
(determinismo). Diferencia clave frente a Linux: el *tiempo de respuesta* está
acotado.

```c
// FreeRTOS: crear una tarea
xTaskCreate(tarea_led, "LED", 128, NULL, 1, NULL);
vTaskStartScheduler();
```

---

## 20.10–20.12 Bajo consumo, firmware seguro y RISC-V

- **Bajo consumo**: modos *sleep*/*stop*, despertar por interrupción — crítico en
  IoT con batería.
- **Firmware seguro**: *secure boot*, arranque verificado, actualizaciones OTA
  firmadas, *flash* cifrada.
- **RISC-V**: ISA **abierta y libre de royalties** que está transformando el
  sector.

---

## Conexión con la actualidad

C es, sin discusión, el **rey indiscutible de los sistemas embebidos**: miles de
millones de microcontroladores (automoción, electrodomésticos, IoT, dispositivos
médicos) ejecutan firmware en C. Tres corrientes definen 2024–2025:

- **RISC-V** ha pasado de promesa a realidad industrial: fabricantes como
  SiFive, Espressif (ESP32-C) y un ecosistema chino enorme adoptan esta ISA
  abierta, y su *toolchain* en C/GCC es de primera clase.
- **Seguridad de firmware**: regulaciones como la **Cyber Resilience Act** de la
  UE (2024) obligan a *secure boot*, actualizaciones firmadas y SBOM en
  dispositivos conectados, elevando el listón del C embebido.
- **La competencia de Rust**: Rust embebido (`embassy`, `embedded-hal`) gana
  terreno por su seguridad de memoria, pero C sigue dominando por madurez de
  *toolchains*, soporte de silicio y base instalada. Saber C embebido sigue
  siendo imprescindible y muy demandado.

---

## Ejercicios

!!! example "Ejercicio 20.1 — Blink ★"
    Haz parpadear un LED en una placa real (STM32, ESP32, RP2040) o en el
    simulador **Wokwi**/QEMU. El «Hola, mundo» del embebido.

!!! example "Ejercicio 20.2 — Botón con interrupción ★★"
    Enciende/apaga un LED desde una ISR de botón, con *debounce*.

!!! example "Ejercicio 20.3 — Sensor por I²C ★★★"
    Lee un sensor de temperatura por I²C y envía las lecturas por UART.

!!! example "Ejercicio 20.4 — Tarea en FreeRTOS ★★★★"
    Implementa dos tareas (parpadeo + lectura de sensor) comunicadas por una
    cola, con prioridades distintas.

---

## Referencias

- *Making Embedded Systems* (Elecia White), O'Reilly.
- *The Definitive Guide to ARM Cortex-M* (Joseph Yiu).
- [FreeRTOS](https://www.freertos.org/), [Zephyr Project](https://www.zephyrproject.org/).
- [Wokwi](https://wokwi.com/) — simulador de microcontroladores en el navegador.
