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

## 25 de octubre de 2026 — TEST21 congelado y TEST26 PASS

- **TEST21**: PASS funcional en VICE para directorios, carga/guardado y filtrado de PRG normales. Algunos .SPR almacenados como SEQ $81 permanecen visibles (aceptado).
- **TEST22**: FAIL JAM $D3C1 al incorporar filtro por nombre .SPR.
- **TEST23B**: FAIL JAM $8F7D incluso con salto diagnóstico mínimo.
- **TEST24**: FAIL; abandona BGFONT y vuelve a BASIC.
- **TEST25**: FAIL JAM $8F7D; ampliar la copia hasta $8F55 no es solución segura.
- **TEST26**: **PASS funcional** de SAVE/LOAD de pantallas y recursos sobre el CRT TEST21 **sin alteración de ROM**. El usuario confirma también `TEXT LOAD "NOMBRE.SCREEN",8` desde EPYX BASIC con representación correcta del fondo.
- CRT compacto TEST21 MASTER: **435088 bytes**, SHA-256 `2e12f8109c11ac9133dc32c969e3ef5ccd00a04973be154cc531391ce6dd3f27`.
- Conservación: CRT, disco de prueba y registro detallado en respaldo descargable; binarios de terceros no publicados en el repositorio sin verificación de permisos.

Ver [acta TEST26 PASS](TEST26_PASS_25_OCT_2026.md).
