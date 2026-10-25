# Mapa de memoria y parches conocidos

Las direcciones siguientes proceden de los archivos y tests analizados; **no son una especificación ABI pública**.

| Zona / dirección | Significado en las versiones analizadas | Observaciones |
| --- | --- | --- |
| $0801 | Dirección de carga de BGFONT y SPRITE independientes | Cuerpo PRG tras cabecera de 2 bytes |
| SYS 2061 ($080D) | Entrada de BGFONT `bgfont(1).prg` | El PRG independiente tiene cabecera especial, no asumir RUN genérico |
| SYS 2084 ($0824) | Entrada de SPRITE independiente | Paquete Nutpressor |
| $AE / $AF | Dirección final (exclusiva) de la última carga, en cero-page | La carga desde cartucho tuvo que restaurarla en TEST12 |
| BGFONT: $8F0A | Final de carga | $AE=$0A y $AF=$8F |
| SPRITE: $75C1 | Final de carga | $AE=$C1 y $AF=$75 |
| $08A8 | Trampolín del cargador SPRITE analizado | `JMP $2753` sustituido por `JMP $2800` |
| $2753 | Presentación/cracktro de SPRITE | Se evita sin alterar el cuerpo principal |
| $2800 | Destino tras la portada; entrada del editor | TEST13 |
| $32E6 | Comprobación del directorio dentro del BGFONT analizado | TEST17–19; código específico de esa versión |
| $0032 | Byte de tipo de archivo leído por el filtro BGFONT | Se observa en desensamblado del parche |
| $C000–$C7FF | Bloque auxiliar copiado a RAM desde ROM del launcher | TEST19 dispone de helper aquí |
| $C7E0 | Rutina auxiliar del filtro TEST19 | 22 bytes; riesgo: verificar supervivencia tras descompresión |

## Evolución del filtro BGFONT

**Original**: reconoce tipos específicos EPYX; las pruebas A/B/C muestran que ignora $81 estándar.

**TEST17**: 3 bytes modificados para ampliar la aceptación SEQ; falla la comprobación RAM porque el launcher esperaba checksums antiguos.

**TEST18**: verifica los checksums actualizados (XOR $A8, SUM $62). En VICE muestra archivos $81, pero también $91 y otros SEQ. No valida contenido TX/CH.

**TEST19**: inserta `JSR $C7E0` desde el control en $32E6. Acepta $81/$A1/$B1 y rechaza $91/$82. Checksum nuevo del BGFONT: XOR **$34** y suma **$16** según su manifiesto de construcción. **Pendiente de PASS en VICE**.

```text
BGFONT (directorio)
      |
      v
leer tipo en $32
      |
      v
TEST19: JSR $C7E0
      |
      +--- tipo SEQ cerrado, sin marca SPRITE --> mostrar
      |
      +--- tipo incompatible -----------------> omitir
```

**Limitación relevante:** el filtro TEST19 no diferencia un SP contenido en SEQ $81 de un TX/CH contenido en SEQ $81. La comparación es solo a nivel de entrada del directorio.

## Control de regresión

- EPYX BASIC y SPRITE: mantener los datos de bancos del TEST13 idénticos durante pruebas BGFONT.
- En TEST19, los bancos modificados respecto de TEST18 son **0 y 2**.
- No probar `BGFONT_TEST19_REQUIERE_CRT.prg` aisladamente: su nuevo `JSR $C7E0` necesita el helper que instala el cartucho.
- Antes de realizar más modificaciones, preservar las copias de TEST13, TEST18 y TEST19 junto con sus manifiestos SHA-256.
- Para diagnóstico en VICE: registrar dirección JAM, estado $01, $AE/$AF, bytes de $32 y $C7E0 si algo falla.

## Trabajos pendientes de ingeniería inversa

Desempaquetar BGFONT para obtener una base relocalizable, seguir el flujo de DIR/LOAD/SAVE, identificar buffers DOS y asegurar que el helper no sea pisado. Investigar si es viable normalizar tipos de archivo exclusivamente en el lector o si se requiere usar lectura de cabeceras por separado.