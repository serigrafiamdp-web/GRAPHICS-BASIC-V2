# Estado de pruebas — 25-10-2026 (actualizado tras TEST26 PASS)

Este historial diferencia **simulación/estructura** de **PASS funcional en VICE comunicado por el usuario**. No asumir que un menú que aparece implica lectura o guardado correcto.

## Matriz cronológica

| Test | Objeto | Resultado observable | Estado |
| --- | --- | --- | --- |
| TEST01–03 | Investigación original G64, Vorpal y diseño EasyFlash | Relevamiento y arquitectura; sin CRT funcional | Investigación |
| TEST04 | Primer CRT con launcher y PRG independientes | VICE JAM en $20E6 al seleccionar editor | FAIL |
| TEST06 | Menú separado en cuatro opciones | EPYX BASIC todavía dependía de LOAD en unidad 8 | Descartado para opción 1 |
| TEST08 | EPYX BASIC CSDb 2022 desde ROM; dos editores independientes | Usuario confirma arranque de opción 1; BGFONT JAM en $206E | **Opción 1 PASS**; otras FAIL |
| TEST09 | Intento de iniciar editores mediante RUN | ?UNDEF'D STATEMENT ERROR | FAIL |
| TEST10 | Reintento mediante SYS | JAM en $206E | FAIL |
| TEST11 | Diagnóstico de RAM previo al editor | RAM VERIFIED y después JAM al iniciar | Diagnóstico positivo, arranque FAIL |
| TEST12 | Restaurar $AE/$AF con final de carga real | Usuario informa «bueno funciona» | **PASS funcional de arranque** |
| TEST13 | Omitir portada del crack de SPRITE e iniciar directo | Usuario confirma prototipo de cartucho conseguido | **Base funcional / conservar** |
| TEST14 | Comparación D64 «reconocido» y discos nuevos | Hipótesis sobre formato/directorio; no evidencia de requisito de protección del disco | Investigación |
| TEST15 | UGUIS con tipos de archivo EPYX y estándar | Disco A visible y carga de SEQ/FONT reportada por usuario | PASS de referencia de recursos |
| TEST16-A | BB6 en D64 con $B1 SCREEN y $A1 FONT | Ambos aparecen en BGFONT | **PASS del listado** |
| TEST16-B | BB6 con $81 / $81 SEQ normal | Ninguno aparece en BGFONT | **PASS del diagnóstico** |
| TEST16-C | BB6 con $81 / $A1 | Aparece FONT, no SCREEN; archivo ausente del listado pero permanece en el D64 | **PASS del diagnóstico** |
| TEST17 | Abrir filtro BGFONT para SEQ | RAM CHECK FAILED: checksum de BGFONT no actualizado | FAIL de verificación |
| TEST18 | Actualizar XOR/SUM y conservar filtro amplio | Ambos BB6 aparecen incluso como $81; también salen sprites/otros SEQ | **PASS de listado ampliado**, NO de carga general |
| TEST19 | Filtro por $81 / $A1 / $B1 con helper en $C7E0 | Tras acceder a disquetera desde DIR, abandona BGFONT y vuelve a BASIC (captura 25/10) | **FAIL funcional en VICE** | 
| TEST20 | Filtro reubicado a $033C; instalado antes de arrancar BGFONT, base TEST18 | El editor activa la disquetera y regresa a READY durante DIR (captura 25/10) | **FAIL funcional en VICE** |
| TEST21 | Filtro inline de 11 bytes en $32E6, sin helpers | En VICE oculta PRG normales y conserva DIR, LOAD y SAVE; algunos sprites SEQ siguen visibles | **MASTER funcional** |

| TEST22 | Filtro .SPR mediante código adicional | JAM en VICE $D3C1 | **FAIL** |
| TEST23B | Salto mínimo de diagnóstico a $8F20 | JAM en VICE $8F7D | **FAIL** |
| TEST24 | Carga extendida / salto diagnóstico | Regresa a BASIC | **FAIL** |
| TEST25 | Intento solo .SPR, ampliando la región de copia | JAM en VICE $8F7D | **FAIL** |
| TEST26 | Prueba funcional sobre CRT TEST21 sin modificaciones | Guarda/recarga .SCREEN, recursos y muestra pantalla mediante TEXT LOAD en EPYX BASIC | **PASS funcional reportado** |

## PASS que debemos preservar
- **TEST13**: cartucho de cuatro opciones con EPYX BASIC desde ROM y SPRITE sin portada. No sustituirlo por tests experimentales.
- **TEST16**: tres D64 de clasificación controlada con volúmenes diferentes; útiles para regresión.
- **TEST18**: primera ampliación de BGFONT que demuestra visibilidad de SEQ normales. Mantener como referencia si TEST19 falla.

## Pruebas históricas de TEST19/20 y criterios de TEST21 (ya ejecutados)
1. Reiniciar VICE completamente con el CRT de TEST21.
2. Opción 1 debe seguir iniciando EPYX BASIC (regresión).
3. Opción 2 debe iniciar BGFONT sin RAM CHECK FAILED ni JAM.
4. Con **T16-B-NORMAL**, DIR debe mostrar BB6TEST.SEQ y BB6TEST.FONT.
5. Con un disco que contenga sprites EPYX tipo $91, el listado de BGFONT no debe mostrar estos sprites.
6. Repetir con **T16-A-EPYX** para comprobar compatibilidad hacia atrás.
7. **No escribir ni sobrescribir** archivos hasta validar carga/guardado y cabeceras.
8. Guardar capturas y el primer mensaje de error o dirección JAM si la prueba falla.
9. Si vuelve a BASIC durante DIR, anotar la dirección o comportamiento; TEST21 no utiliza código en $033C ni $C7E0.
10. El filtro inline es intencionalmente limitado: algunas clases especiales $A2/$B2 podrían mostrarse; NO confundir esto con validación de cabecera TX/CH.

## Límites aún no demostrados
- El TEST26 PASS es una validación funcional en VICE comunicada por el usuario: no se recibió el D64 posterior a los guardados para comprobar el contenido byte por byte.
- No se ha probado compatibilidad universal con **todo D64**, protección de escritura, disco lleno ni ciclos de 100 guardados.
- Los archivos .SPR que comparten tipo SEQ $81 pueden seguir apareciendo; comportamiento aceptado.
- No se ha validado aún el retorno limpio de editores al menú del cartucho ni todas las funciones de SPRITE.
- Para evitar nuevos JAM, no reutilizar la zona $8F20 de las pruebas TEST22–25 sin análisis de RAM ejecutable.

**Acta definitiva:** [TEST26 PASS](TEST26_PASS_25_OCT_2026.md). **Regla:** «PASS de listado» != «PASS de carga» != «PASS de guardado»; la sesión confirmó las tres operaciones, con las limitaciones arriba.
