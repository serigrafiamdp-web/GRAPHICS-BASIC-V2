# Historial del proyecto

**25 de octubre de 2026 — Consolidación inicial del repositorio GRAPHICS-BASIC-V2.**

### Cartucho / runtime
- TEST04: primer launcher EasyFlash; errores JAM.
- TEST08: EPYX BASIC recuperado de CSDb 2022 puede arrancar desde ROM.
- TEST09–11: problemas RUN/SYS y diagnóstico de copia a RAM; JAM $206E.
- TEST12: restauración de $AE/$AF tras copia de PRG desde ROM; arranque comunicado como funcional.
- TEST13: SPRITE arranca sin la presentación del crack; cartucho de cuatro opciones conservado como **base funcional**.

### Discos y formatos
- Estudio de discos G64/D64 originales, reparados y de CSDb.
- Fe de erratas: CREATE forma programa + soporte; no autoarranque; FORMAT/DIR reemplazan nombres previos.
- Descubrimiento práctico del filtro de tipos en BGFONT, probado con A/B/C en TEST16.
- Diferenciación de SEQ estándar $81, SPRITE $91, FONT $A1, SCREEN $B1, PRG $82.
- Investigación de archivo `*PRG` sin cerrar en proyecto UGUIS; **no se considera una marca de protección demostrada**.

### Compatibilidad de BGFONT
- TEST17: primer parche de filtro; falla control de integridad RAM por checksum no actualizado.
- TEST18: checksums corregidos; BGFONT muestra BB6 estándar, pero incluye también otros SEQ.
- TEST19: **FAIL en VICE**; durante DIR el editor accede al disco y vuelve a BASIC. El filtro JSR $C7E0 depende de RAM del cargador que no se garantiza durante BGFONT.
- TEST20: **FAIL en VICE** al ejecutar DIR: activa la disquetera y retorna al prompt READY. Conservado para diagnóstico, no aprobado.
- TEST21: vuelve a TEST18; reemplaza únicamente 11 bytes del filtro de directorio por `LDA $32 / EOR #$56 / CMP #$D7 / BCC skip / JMP $514F`. Sin helper externo ni JSR. Permite tipos EPYX y SEQ normal habituales, rechaza SPRITE $91 y PRG $82, pero puede aceptar variantes especiales infrecuentes. **PASS estructural; pendiente VICE**.

### Decisiones
- No usar un G64 montado en la disquetera para arrancar EPYX BASIC en el CRT.
- Nunca pisar un TEST que dio PASS.
- No alterar el SAVE ni el contenido binario de recursos durante experimentos del directorio.
- Entregar futuro soporte de cualquier D64 1541 sin requerir CREATE/FORMAT especial.
- Guardar mapas, decisiones, límites y pruebas en el repo, y distribuir binarios originales únicamente cuando corresponda legalmente.