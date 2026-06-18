---
tags:
  - Intermedio
  - Estructuras
---

# Capítulo 8 — Estructuras, Uniones y Enumeraciones

!!! abstract "Objetivos de aprendizaje"
    - Definir y usar `struct`, `union` y `enum`.
    - Entender **alineamiento y *padding*** y optimizar el tamaño de una struct.
    - Crear tipos con `typedef` y estructuras autorreferenciadas.

---

## 8.1 Estructuras (`struct`)

Agrupan datos heterogéneos bajo un nombre:

```c
struct Punto { int x, y; };

struct Punto p = {.x = 3, .y = 4};   // inicialización designada (C99)
p.x = 10;                            // acceso con .
struct Punto *pp = &p;
pp->y = 20;                          // acceso con -> a través de puntero
```

Las structs se copian por valor en asignaciones y llamadas (cuidado con structs
grandes: pasa punteros).

---

## 8.2 Alineamiento y *padding*

El compilador inserta bytes de relleno (*padding*) para respetar el alineamiento
de cada miembro. El orden de los campos **importa**:

```c
struct Mala  { char a; int b; char c; };   // típicamente 12 bytes
struct Buena { int b; char a; char c; };   // típicamente  8 bytes
```

```c
#include <stddef.h>
offsetof(struct Mala, b);   // desplazamiento del miembro b
sizeof(struct Mala);        // incluye el padding
```

!!! tip "Regla práctica"
    Ordena los miembros de **mayor a menor** alineamiento para minimizar el
    *padding*. Importa en estructuras replicadas millones de veces (caché,
    formatos de red, embebidos).

Para formatos binarios exactos puedes usar `#pragma pack` o `__attribute__((packed))`,
pero **a costa** de accesos potencialmente no alineados (cap. 6).

---

## 8.3 Uniones (`union`)

Todos los miembros comparten el mismo almacenamiento; solo uno está «activo» a la
vez. Tamaño = el del mayor miembro.

```c
union Valor { int i; float f; char bytes[4]; };
union Valor v; v.i = 1065353216;
printf("%f\n", v.f);   // reinterpreta los bits (type punning)
```

El idioma de la **unión etiquetada** (*tagged union*) modela un valor que puede
ser de varios tipos — base de intérpretes y serializadores:

```c
struct Json {
    enum { J_NULL, J_NUM, J_STR } tipo;
    union { double num; char *str; } u;
};
```

---

## 8.4 Enumeraciones (`enum`)

Constantes enteras con nombre:

```c
enum Color { ROJO, VERDE = 5, AZUL };   // 0, 5, 6
enum Color c = VERDE;
```

!!! note "C23"
    C23 permite **fijar el tipo subyacente** de un enum: `enum E : uint8_t { ... };`,
    útil para protocolos y ahorro de memoria.

---

## 8.5 `typedef`

Crea alias de tipos para legibilidad:

```c
typedef struct Punto Punto;          // ahora basta "Punto" sin "struct"
typedef unsigned long long u64;
typedef int (*Comparador)(const void *, const void *);
```

---

## 8.6 Estructuras autorreferenciadas

La clave de las estructuras de datos enlazadas: una struct que contiene un
puntero a su propio tipo.

```c
typedef struct Nodo {
    int dato;
    struct Nodo *sig;   // debe ser puntero (un Nodo dentro de Nodo sería infinito)
} Nodo;
```

El *flexible array member* (C99) permite una struct con un array de tamaño
variable al final, en una sola asignación:

```c
struct Cadena { size_t len; char datos[]; };   // datos[] al final
struct Cadena *s = malloc(sizeof *s + n + 1);
s->len = n;
```

---

## Conexión con la actualidad

Las **uniones etiquetadas** son el patrón con el que C modela los tipos suma
(*sum types*) que lenguajes modernos como Rust ofrecen de forma nativa con
`enum`. Aparecen en el corazón de cada parser de **JSON/Protobuf/CBOR** (cap. 33,
35), en el AST de un compilador (cap. 19) y en los valores de una máquina virtual
(cap. 31). El control fino del **layout** de structs es crítico en 2025 para el
rendimiento: la disposición *Struct-of-Arrays* (SoA) frente a *Array-of-Structs*
(AoS) puede multiplicar por varias veces el rendimiento al aprovechar mejor la
caché y la vectorización SIMD (cap. 21). En seguridad, los *type confusion* sobre
uniones sin comprobar la etiqueta son una clase de vulnerabilidad reconocida
(CWE-843), que la verificación formal (cap. 25) ayuda a descartar.

---

## Ejercicios

!!! example "Ejercicio 8.1 — Optimiza la struct ★★"
    Te damos una struct con *padding* excesivo. Reordena sus campos y mide
    `sizeof` antes y después.

!!! example "Ejercicio 8.2 — Valor JSON ★★★"
    Define una *tagged union* para representar `null`, número, cadena, booleano,
    array y objeto. Escribe una función `imprimir(Json)`.

!!! example "Ejercicio 8.3 — Cadena con FAM ★★★"
    Implementa un tipo cadena con *flexible array member* y funciones
    `crear`/`concat`/`destruir`.

!!! example "Ejercicio 8.4 — Registro de hardware ★★"
    Modela un registro de control de 32 bits con una struct de *bitfields* y
    documenta los riesgos de portabilidad de los campos de bits.

---

## Referencias

- ISO/IEC 9899:2018, §6.7.2.1 (struct/union), §6.7.2.2 (enum), §6.7.2.1 §18 (FAM).
- *Computer Systems: A Programmer's Perspective* (Bryant & O'Hallaron), cap. de datos.
- CWE-843: *Access of Resource Using Incompatible Type ('Type Confusion')*.
