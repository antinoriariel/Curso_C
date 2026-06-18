# Apéndice G — Cheatsheet de Makefile

## Makefile de referencia (proyecto multi-archivo)

```makefile
# ---- Configuración ----
CC      := gcc
CFLAGS  := -std=c17 -Wall -Wextra -Wpedantic -Wshadow -g
LDFLAGS :=
LDLIBS  :=

SRC_DIR := src
OBJ_DIR := build
BIN     := programa

SRC := $(wildcard $(SRC_DIR)/*.c)
OBJ := $(SRC:$(SRC_DIR)/%.c=$(OBJ_DIR)/%.o)
DEP := $(OBJ:.o=.d)

# ---- Reglas ----
all: $(BIN)

$(BIN): $(OBJ)
	$(CC) $(LDFLAGS) $^ $(LDLIBS) -o $@

$(OBJ_DIR)/%.o: $(SRC_DIR)/%.c | $(OBJ_DIR)
	$(CC) $(CFLAGS) -MMD -MP -c $< -o $@

$(OBJ_DIR):
	mkdir -p $@

# Compilaciones especiales
debug:   CFLAGS += -O0 -fsanitize=address,undefined
debug:   clean all
release: CFLAGS += -O2 -DNDEBUG
release: clean all

test: $(BIN)
	./$(BIN) --test

clean:
	$(RM) -r $(OBJ_DIR) $(BIN)

-include $(DEP)          # dependencias automáticas de cabeceras

.PHONY: all debug release test clean
```

## Conceptos clave

| Elemento | Significado |
|----------|-------------|
| `target: prereqs` | Regla: cómo construir `target` a partir de `prereqs` |
| `$@` | El *target* actual |
| `$<` | El primer prerrequisito |
| `$^` | Todos los prerrequisitos |
| `$*` | El *stem* (parte que casa con `%`) |
| `%` | Comodín en reglas de patrón |
| `:=` | Asignación inmediata (recomendada) |
| `?=` | Asignar solo si no está definida |
| `+=` | Añadir |
| `.PHONY` | *Targets* que no son archivos |

!!! warning "Tabulaciones, no espacios"
    Las recetas de Make **deben** indentarse con un **tabulador real**, no con
    espacios. Es la fuente nº 1 de errores `*** missing separator`.

## Variables automáticas frecuentes

```makefile
$(wildcard *.c)              # lista de archivos que casan
$(patsubst %.c,%.o,$(SRC))   # sustitución de patrón
$(SRC:.c=.o)                 # forma corta de lo anterior
$(shell uname -s)            # salida de un comando
$(addprefix build/,$(OBJ))   # añadir prefijo
```

## Generación automática de dependencias

`-MMD -MP` hace que el compilador emita archivos `.d` con las dependencias de
cabeceras; `-include $(DEP)` las incorpora, de modo que cambiar un `.h` recompila
solo lo necesario.

Más en el [Capítulo 10](../capitulos/capitulo-10.md). Para proyectos
multiplataforma, considera **CMake** (también en el cap. 10).
