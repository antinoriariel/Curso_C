---
tags:
  - Maestría
  - Parsing
  - Lenguajes
---

# Capítulo 33 — Procesamiento de Texto y Lenguajes

!!! abstract "Objetivos de aprendizaje"
    - Implementar y usar **expresiones regulares** y motores de *parsing*.
    - Parsear **JSON**, **XML**, **YAML/TOML** de forma robusta.
    - Conocer PEG, motores de plantillas y nociones de NLP.

---

## 33.1 Expresiones regulares

POSIX ofrece `<regex.h>`; para más potencia, **PCRE2**. Implementar un motor de
regex (Thompson NFA) es un ejercicio clásico que conecta autómatas y rendimiento:

```c
#include <regex.h>
regex_t re;
regcomp(&re, "^[0-9]+$", REG_EXTENDED);
if (regexec(&re, cadena, 0, NULL, 0) == 0) puts("es número");
regfree(&re);
```

!!! warning "ReDoS"
    Las regex con *backtracking* pueden tener coste exponencial ante entradas
    maliciosas (*Regular expression Denial of Service*, CWE-1333). Los motores
    basados en autómatas (RE2, Thompson NFA) garantizan tiempo lineal.

---

## 33.2 Parsing de JSON

El formato de intercambio dominante. Un parser recursivo (cap. 19) sobre una
*tagged union* (cap. 8):

```c
typedef struct Json {
    enum { J_NULL, J_BOOL, J_NUM, J_STR, J_ARR, J_OBJ } tipo;
    union {
        bool b; double num; char *str;
        struct { struct Json **items; size_t n; } arr;
        struct { char **claves; struct Json **vals; size_t n; } obj;
    } u;
} Json;

Json *json_parse(const char **p);   // descenso recursivo
```

Librerías de referencia: **cJSON** (sencilla), **jansson**, **yyjson** (la más
rápida), **jsmn** (sin asignación).

---

## 33.3–33.5 XML, YAML/TOML y PEG

- **XML**: SAX (eventos, poca memoria) vs DOM (árbol); libxml2, Expat.
- **YAML/TOML**: configuración legible; libyaml, tomlc99.
- **PEG** (*Parsing Expression Grammars*): formalismo alternativo a las gramáticas
  libres de contexto, sin ambigüedad; generadores como `peg/leg`, `re2c`.

---

## 33.6–33.8 Parsers de lenguajes, codegen y plantillas

- Parsers de lenguajes de programación (cap. 19).
- Generación y transformación de código (metaprogramación, transpiladores).
- Motores de **plantillas** (sustitución, secciones, escape) tipo Mustache.

---

## 33.9 NLP

Tokenización, *stemming*, modelos de n-gramas, distancia de edición (Levenshtein).
Base de buscadores (cap. 40-C) y correctores.

---

## Conexión con la actualidad

El *parsing* es una de las superficies de ataque más explotadas: parsers de JSON,
XML y multimedia escritos en C han producido innumerables CVEs (desbordamientos,
*billion laughs* en XML, ReDoS). Por eso 2024–2025 ve dos tendencias claras:
parsers **seguros por diseño** —`yyjson` y `simdjson` (este último usa SIMD,
cap. 21, para parsear gigabytes por segundo)— y el *fuzzing* sistemático de todo
parser que toque datos externos (cap. 30, OSS-Fuzz fuzzea libxml2, jansson, etc.
de forma continua). Además, el procesamiento de texto vive una revolución con los
LLM: los **tokenizadores** modernos (BPE, *SentencePiece*) que alimentan a los
grandes modelos de lenguaje tienen implementaciones críticas en C/C++
(`tokenizers`, `tiktoken`, `llama.cpp`), donde el rendimiento del *tokenizado*
importa tanto como el del modelo. Saber parsear texto de forma robusta y rápida en
C sigue siendo una habilidad central.

---

## Ejercicios

!!! example "Ejercicio 33.1 — Parser JSON ★★★★"
    Implementa un parser JSON completo (con *escapes*, números, anidamiento) que
    pase un conjunto de pruebas de conformidad. Fuzzéalo (cap. 30).

!!! example "Ejercicio 33.2 — Motor de regex ★★★★★"
    Implementa un motor de regex con NFA de Thompson (concatenación, alternancia,
    estrella) en tiempo lineal.

!!! example "Ejercicio 33.3 — Distancia de Levenshtein ★★★"
    Calcula la distancia de edición por programación dinámica y úsala para
    sugerir correcciones.

!!! example "Ejercicio 33.4 — Mini plantillas ★★★"
    Un motor de plantillas que sustituya `{{variable}}` y soporte secciones
    `{{#lista}}...{{/lista}}`.

---

## Referencias

- Russ Cox. *Regular Expression Matching Can Be Simple And Fast* (serie de artículos).
- [json.org](https://www.json.org/), RFC 8259 (JSON).
- [cJSON](https://github.com/DaveGamble/cJSON), [yyjson](https://github.com/ibireme/yyjson), [simdjson](https://simdjson.org/).
- *Speech and Language Processing* (Jurafsky & Martin) — para NLP.
