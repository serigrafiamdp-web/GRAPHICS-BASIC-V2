# TEST21 — filtro BGFONT dentro de los 11 bytes originales

Se parte del cartucho **TEST18** (PASS de listado ampliado en VICE), no de TEST19 ni TEST20.

## Parche

En la dirección nominal `$32E6` del PRG BGFONT independiente (load `$0801`), reemplazar el filtro original de TEST18:

```asm
; Antes, TEST18
LDA $32
AND #$07
CMP #$01
BNE +3
JMP $514F
```

por el filtro inline del TEST21:

```asm
; TEST21: 11 bytes, SIN JSR ni memoria auxiliar
LDA $32     ; A5 32
EOR #$56    ; 49 56
CMP #$D7    ; C9 D7
BCC +3      ; 90 03  -> saltar el JMP si no es aceptado
JMP $514F   ; 4C 4F 51
```

Bytes exactos: `A5 32 49 56 C9 D7 90 03 4C 4F 51`.

La estructura y longitud de BGFONT no cambian. El launcher actualiza checksums RAM para el nuevo PRG (XOR **$0F**, suma **$67**). No se altera EPYX BASIC ni SPRITE.

## Tipos que comprobamos

| Entrada DOS | Valor tras XOR #$56 | Decisión |
| --- | --- | --- |
| $81 SEQ | $D7 | Admite |
| $A1 EPYX FONT | $F7 | Admite |
| $B1 EPYX SCREEN | $E7 | Admite |
| $91 EPYX SPRITE | $C7 | Rechaza |
| $82 PRG | $D4 | Rechaza |
| $80 DEL | $D6 | Rechaza |
| $83 USR | $D5 | Rechaza |
| $84 REL | $D2 | Rechaza |
| $01 SEQ abierto | $57 | Rechaza |

**Limitación:** otros tipos especiales no comunes (ej. $A2 o $B2) podrían aparecer. No se inspecciona el contenido TX/CH. No interpretar esto como un filtro completo o una garantía de LOAD seguro.

## Status

**PASS estructural**: sólo seis bytes cambian frente a TEST18, incluyendo las dos constantes checksum del launcher; ambos BB6 aparecen en la simulación de lectura de los discos de referencia A, B y C. **Prueba VICE aún pendiente**.

En VICE: opción 2 BGFONT, disco T16-B-NORMAL, DIR; verificar BB6TEST.SEQ y BB6TEST.FONT, y ausencia de $91; repetir con A y C. **No guardar ni sobrescribir archivos todavía**.

Script de construcción propietario del proyecto: `EPYX_TEST21_build.py`, disponible dentro del ZIP experimental TEST21; no se incluyen ROM/PRG originales de terceros en el repositorio público.
