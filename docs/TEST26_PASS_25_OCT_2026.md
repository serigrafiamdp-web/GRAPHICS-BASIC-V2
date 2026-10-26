# TEST26 — PASS funcional de BGFONT y EPYX BASIC (25-10-2026)

**Versión congelada:** TEST21 inline; TEST26 NO modifica ROM.
**Emulador:** VICE x64sc (C64SC). **Validación:** pruebas funcionales confirmadas por el usuario, no una ejecución automatizada independiente.

## Confirmaciones en VICE
- BGFONT inicia y abre el directorio D64.
- TEST21 deja de mostrar los PRG normales, aunque algunos sprites almacenados como SEQ ($81) siguen visibles. Se acepta ese comportamiento.
- Carga de recursos BGFONT y guardado/recarga de pantallas con extensión elegida por el usuario `.SCREEN`: **PASS**.
- Carga y guardado de fuentes: usuario confirma que funcionan; **PASS reportado**, sin archivo posterior al guardado para comparación binaria.
- Desde EPYX BASIC: `TEXT LOAD "NOMBRE.SCREEN",8` recupera y muestra el fondo guardado. **PASS**.
- El CRT incluido en el paquete TEST26 es **idéntico byte por byte** al CRT TEST21 compacto (435088 bytes; SHA-256 `2e12f8109c11ac9133dc32c969e3ef5ccd00a04973be154cc531391ce6dd3f27`).

## Comandos originales del manual (páginas impresas 56–57 y 107–108)
Pantalla BGFONT:
```basic
TEXT
TEXT LOAD "NOMBRE.SCREEN",8
```
Pantalla con juego de caracteres personalizado:
```basic
COPY UPPERCASE TO RAM
CHAR RAM
CHAR LOAD "NOMBRE.FONT",8
TEXT LOAD "NOMBRE.SCREEN",8
```
`.SCREEN`, `.FONT` son convenciones de nombres del usuario, no requisitos de EPYX.

## Regresión y decisiones
| Versión | Estado | Resultado |
| --- | --- | --- |
| TEST18 | PASS | DIR y cargas, directorio sin filtrar |
| TEST19 | FAIL | Vuelve a BASIC/READY; helper $C7E0 |
| TEST20 | FAIL | Vuelve a BASIC/READY; helper $033C |
| TEST21 | **MASTER funcional** | Filtrado PRG normales; lectura, carga y guardado correctos |
| TEST22 | FAIL | JAM $D3C1 |
| TEST23B | FAIL | JAM $8F7D (desvío diagnóstico) |
| TEST24 | FAIL | Sale de BGFONT y vuelve a BASIC |
| TEST25 | FAIL | JAM $8F7D |
| TEST26 | **PASS** | Validación funcional de TEST21 sin cambios de ROM |

**Decisión:** no seguir corrigiendo .SPR por ahora. No reutilizar la región $8F20 ni ampliar el copiado RAM basándose solo en bytes $FF de relleno. Conservar intactos TEST18 y TEST21. La próxima fase es robustez de SAVE/CLOSE, disco lleno, discos diversos y funciones del Sprite Editor, sin alterar este MASTER.

## Alcance y límites
No se adjuntó el D64 después de los SAVE, por lo que **no** hay cotejo binario TX/CH del resultado. Tampoco se han probado todos los D64, hardware real, disquetera 9, fallos de DOS ni ciclos repetidos de escritura. No declarar compatibilidad universal solo por este PASS.

**Artefactos en respaldo privado de la sesión:** `EPYX_MASTER_TEST21_TEST26_PASS_25_OCT_2026.zip` (CRT, D64 de referencia, copia de trabajo, verificador, informe y manifiesto SHA-256). Los binarios históricos de terceros se conservan fuera de este repositorio público hasta revisar autorización de distribución.
