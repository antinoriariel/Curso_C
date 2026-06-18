# Apéndice A — Precedencia y Asociatividad de Operadores

De **mayor** a **menor** precedencia. Ante la duda, usa paréntesis.

| Nivel | Operadores | Descripción | Asociatividad |
|-------|-----------|-------------|---------------|
| 1 | `()` `[]` `.` `->` `++` `--` (sufijo) `(tipo){...}` | Postfijos, acceso, literal compuesto | Izq → Der |
| 2 | `++` `--` (prefijo) `+` `-` (unarios) `!` `~` `*` `&` `sizeof` `_Alignof` `(tipo)` | Unarios, *cast* | Der → Izq |
| 3 | `*` `/` `%` | Multiplicativos | Izq → Der |
| 4 | `+` `-` | Aditivos | Izq → Der |
| 5 | `<<` `>>` | Desplazamiento de bits | Izq → Der |
| 6 | `<` `<=` `>` `>=` | Relacionales | Izq → Der |
| 7 | `==` `!=` | Igualdad | Izq → Der |
| 8 | `&` | AND a nivel de bits | Izq → Der |
| 9 | `^` | XOR a nivel de bits | Izq → Der |
| 10 | `\|` | OR a nivel de bits | Izq → Der |
| 11 | `&&` | AND lógico (cortocircuito) | Izq → Der |
| 12 | `\|\|` | OR lógico (cortocircuito) | Izq → Der |
| 13 | `?:` | Condicional ternario | Der → Izq |
| 14 | `=` `+=` `-=` `*=` `/=` `%=` `&=` `^=` `\|=` `<<=` `>>=` | Asignación | Der → Izq |
| 15 | `,` | Operador coma | Izq → Der |

!!! warning "Trampas frecuentes"
    - `&` y `|` tienen **menos** precedencia que `==`: `a & b == c` ≡ `a & (b == c)`.
    - El desplazamiento tiene menos precedencia que `+`: `x << 1 + 2` ≡ `x << (1 + 2)`.
    - `*p++` ≡ `*(p++)` (postfijo antes que la indirección unaria).

Detalles en el [Capítulo 2](../capitulos/capitulo-02.md).
