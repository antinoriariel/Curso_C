---
tags:
  - Experto
  - Kernel
  - Linux
---

# Capítulo 23 — Programación de Kernel y Módulos Linux

!!! abstract "Objetivos de aprendizaje"
    - Comprender la arquitectura del kernel y el espacio kernel vs usuario.
    - Escribir, compilar y cargar un **módulo del kernel**.
    - Crear *device drivers* de caracteres, manejar concurrencia y memoria del
      kernel.
    - Conocer **eBPF**, el *network stack* y la seguridad del kernel.

!!! danger "Sin red de seguridad"
    En el kernel no hay `printf`, ni `malloc` estándar, ni protección de memoria
    entre procesos: un fallo cuelga la máquina (*kernel panic*). Trabaja siempre
    en una **máquina virtual** (QEMU/KVM) para experimentar.

---

## 23.1–23.2 Arquitectura y entorno

El kernel monolítico de Linux corre en modo privilegiado. El código de usuario
solicita servicios mediante **llamadas al sistema**. Para desarrollar necesitas
las *kernel headers*, `make` y, preferiblemente, una VM.

---

## 23.3 Tu primer módulo

```c title="hola.c — módulo del kernel"
#include <linux/init.h>
#include <linux/module.h>
#include <linux/kernel.h>

MODULE_LICENSE("GPL");
MODULE_AUTHOR("Kernel");
MODULE_DESCRIPTION("Modulo de ejemplo");

static int __init hola_init(void) {
    printk(KERN_INFO "Hola, kernel\n");   // a dmesg, no a stdout
    return 0;
}
static void __exit hola_exit(void) {
    printk(KERN_INFO "Adios, kernel\n");
}
module_init(hola_init);
module_exit(hola_exit);
```

```makefile title="Makefile"
obj-m += hola.o
all:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules
clean:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
```

```bash
make
sudo insmod hola.ko    # cargar
dmesg | tail           # ver "Hola, kernel"
sudo rmmod hola        # descargar
```

---

## 23.4–23.5 Drivers y llamadas al sistema

Un *character device* expone operaciones (`open`, `read`, `write`, `ioctl`) vía
`struct file_operations` (una tabla de punteros a función, cap. 4):

```c
static struct file_operations fops = {
    .owner = THIS_MODULE,
    .read  = mi_read,
    .write = mi_write,
    .open  = mi_open,
};
```

Para mover datos entre kernel y usuario: `copy_to_user` / `copy_from_user`
(**nunca** desreferencies directamente un puntero de usuario).

---

## 23.6–23.8 Concurrencia, memoria y modelo de dispositivos

- **Sincronización**: *spinlocks* (en contexto atómico), *mutexes*, **RCU**
  (*Read-Copy-Update*, lectura sin bloqueo), variables atómicas.
- **Memoria**: `kmalloc`/`kfree`, `vmalloc`, asignadores *slab*.
- **Modelo de dispositivos**: *sysfs*, *udev*, árbol de dispositivos.

---

## 23.9–23.10 Network stack y eBPF

**eBPF** es la revolución del kernel moderno: permite ejecutar programas
*sandboxed* y verificados **dentro** del kernel, sin escribir un módulo, para
observabilidad, redes y seguridad.

```c
// Programa eBPF (esquema) que cuenta paquetes, cargado con libbpf
SEC("xdp")
int contar(struct xdp_md *ctx) {
    // se ejecuta por cada paquete en el driver de red (XDP)
    return XDP_PASS;
}
```

---

## 23.11 Seguridad del kernel

LSM (SELinux, AppArmor), *seccomp*, *namespaces* y *capabilities* (cap. 39),
mitigaciones contra explotación (KASLR, SMEP/SMAP, *stack canaries*).

---

## Conexión con la actualidad

El kernel de Linux —~30 millones de líneas de C— es el sistema de software más
importante del planeta: corre en la mayoría de servidores, en todos los Android y
en la práctica totalidad de la nube. Dos noticias de primer orden marcan
2024–2025:

- **Rust en el kernel**: desde la versión 6.1 (2022) y consolidándose en 2024–
  2025, Linux admite *drivers* en Rust junto al C. Es el cambio de lenguaje más
  significativo en la historia del proyecto, motivado por la seguridad de
  memoria — pero el **99 %+ del kernel sigue siendo C**, y entenderlo es
  imprescindible.
- **eBPF** se ha convertido en la plataforma de extensibilidad del kernel:
  herramientas como **Cilium**, **Falco** y **bpftrace** lo usan para redes,
  seguridad y observabilidad sin modificar el kernel. Saber C es el prerrequisito
  para escribir y entender programas eBPF.

---

## Ejercicios

!!! example "Ejercicio 23.1 — Hola, kernel ★★"
    Compila, carga y descarga el módulo de ejemplo en una VM; observa `dmesg`.

!!! example "Ejercicio 23.2 — Device de caracteres ★★★"
    Crea `/dev/mibuffer` que almacene lo que se le escriba y lo devuelva al
    leerlo.

!!! example "Ejercicio 23.3 — Parámetros e ioctl ★★★★"
    Añade parámetros de módulo (`module_param`) y un `ioctl` para configurar el
    dispositivo.

!!! example "Ejercicio 23.4 — eBPF observabilidad ★★★★"
    Con `bpftrace`, cuenta cuántas veces se invoca una *syscall* concreta en tu
    sistema durante 10 segundos.

---

## Referencias

- *Linux Device Drivers*, 3.ª ed. (Corbet, Rubini, Kroah-Hartman) — gratuito.
- *Linux Kernel Development* (Robert Love).
- [kernel.org documentation](https://www.kernel.org/doc/html/latest/).
- Brendan Gregg. *BPF Performance Tools*.
