# Apéndice C — Secuencias de Escape y Caracteres Especiales

## Secuencias de escape estándar

| Secuencia | Significado | Valor (ASCII) |
|-----------|-------------|---------------|
| `\n` | Nueva línea (*line feed*) | 10 |
| `\t` | Tabulación horizontal | 9 |
| `\r` | Retorno de carro | 13 |
| `\0` | Carácter nulo (fin de cadena) | 0 |
| `\\` | Barra invertida | 92 |
| `\'` | Comilla simple | 39 |
| `\"` | Comilla doble | 34 |
| `\a` | Alerta (campana) | 7 |
| `\b` | Retroceso (*backspace*) | 8 |
| `\f` | Avance de página (*form feed*) | 12 |
| `\v` | Tabulación vertical | 11 |
| `\?` | Signo de interrogación | 63 |
| `\nnn` | Carácter en **octal** | — |
| `\xhh` | Carácter en **hexadecimal** | — |
| `\uXXXX` `\UXXXXXXXX` | Punto de código Unicode (C99) | — |

```c
printf("Tabula\taquí\n");
printf("Octal: \101  Hex: \x42\n");   // 'A'  'B'
char campana = '\a';
```

## Sobre codificación de caracteres

- `char` representa un **byte**, no necesariamente un «carácter» visible.
- El texto moderno usa **UTF-8**, donde un carácter puede ocupar varios bytes;
  `strlen` cuenta **bytes**, no caracteres Unicode.
- C11 añadió `char16_t`/`char32_t` (`<uchar.h>`) y los prefijos `u8"..."`,
  `u"..."`, `U"..."`, `L"..."` para literales.

!!! tip "UTF-8 por defecto"
    Trata las cadenas como bytes UTF-8 y evita conversiones innecesarias. Para
    operaciones conscientes de Unicode (longitud en *grapheme clusters*,
    normalización) usa una biblioteca como ICU o utf8proc.

Relacionado: [Capítulo 5](../capitulos/capitulo-05.md) (cadenas).
