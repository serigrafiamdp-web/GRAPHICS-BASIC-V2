# Fuentes, artefactos y reproducción

## Materiales de referencia recibidos y analizados

| Recurso | Uso | Nota |
| --- | --- | --- |
| `Epyx-Toolkit-Basic-1985-Epyx.G64` | Imagen histórica del disco original | 333744 bytes; original sin modificar |
| `epyx_toolkit_basic[epyx_1985]_repaired - copia(1).g64` | Comparación de pistas | No es desprotección demostrada |
| `Epyx Toolkit Basic csdb 2022.d64` | Núcleo BASIC para el CRT | Imagen de 174848 bytes, contiene TOOLKIT BASIC, BGFONT, SPRITE, HELP |
| `bgfont(1).prg` | BGFONT independiente | 34571 bytes; SYS 2061 |
| `epyx sprite editor.d64` | Editor SPRITE independiente | Contiene ejecutable empaquetado y nota histórica |
| `d64 ugis.d64` | Caso real de directorio y archivos *PRG | No reparar automáticamente ni borrar sin backup |
| `epyx disk.d64` | Disco de usuario reconocido por EPYX BASIC | Base de comparación; no es prueba de CREATE obligatorio |
| `bb6.back.seq` / `bb6.font.seq` | Recursos gráficos de regresión | TX/CH |
| `Epyx_Programmers_BASIC_Toolkit_Addendum.pdf` | Fe de erratas oficial | CREATE / FORMAT / DIR / sprites |

**Referencias externas:**
- Manual Epyx Programmer's BASIC Toolkit: https://archive.org/details/Epyx_Programmers_BASIC_Toolkit
- EasyFlash programmer's reference: https://skoe.de/easyflash/files/devdocs/EasyFlash-ProgRef.pdf
- EasyFlash introduction: https://www.skoe.de/easyflash/efintro/
- CSDb (edición estudiada): https://csdb.dk/release/?id=225614

## Artefactos de la sesión — no publicados aún como binarios en GitHub

- `EPYX_CARTUCHO_TEST13_SPRITE_DIRECTO/EPYX_TOOLKIT_TEST13_SPRITE_DIRECT.crt`: cartucho base, imagen EasyFlash 1050688 bytes.
- `EPYX_TEST18_RAM_CHECK_FIX/EPYX_TOOLKIT_TEST18_BGFONT_RAMCHECK.crt`: amplía visibilidad de SEQ normales.
- `EPYX_TEST19_BGFONT_FILTRO/EPYX_TOOLKIT_TEST19_BGFONT_FILTRO.crt`: filtro selectivo en evaluación.
- `EPYX_TEST16_BB6_CONTROLADO/EPYX_BB6_TEST16_A_EPYX.d64`.
- `EPYX_TEST16_BB6_CONTROLADO/EPYX_BB6_TEST16_B_NORMAL.d64`.
- `EPYX_TEST16_BB6_CONTROLADO/EPYX_BB6_TEST16_C_MIXTO.d64`.
- Scripts de construcción propios: `build_cart_test13.py`, `epyx_patch_test17.py`, `EPYX_TEST19_build.py` y manifiestos asociados (pendiente importación organizada, auditando dependencias).

**Importante:** estas rutas son nombres de artefactos en la sesión de investigación, **no URLs ni archivos ya presentes en el repositorio**. No se han subido ejecutables de terceros ni G64/D64 protegidos a este repositorio público.

## Huellas para comprobar procedencia

- `bgfont(1).prg`: SHA-256 `30f5a48abb268d3d89fe2928bf26acc6d354fe4e0c5a11f3f38cccbdc7577b9c`
- SPRITE PRG original independiente: SHA-256 `0adad3b15514e492f45f7edb9f4df884eba6f030c9464c81201e20057a46cfb7`
- SPRITE PRG modificado TEST13: SHA-256 `806e7f601df6a8d8d6161fb83cf7d903f7a8b94aca9f5f20ef1ea11dbf932359`
- G64 1985: SHA-256 `dcfbded88c6c24d517bff4dca092fd7823a5f476d9abe2b1143b2c9e2efa2214`
- CRT TEST19: SHA-256 `64e23d522c5493a1035d587418eae4969fb542161857db20973ec4b5d0ac6eb9`
- BGFONT TEST19: SHA-256 `8cbe8d93e69bbb2e25c2d50ff0a6d9e9e61dfe24d2c6ba72a34e3de9a4e128dc`

## Reproducción y reporte de pruebas

1. Duplicar D64 de trabajo; no usar copia única para escrituras.
2. Abrir **VICE x64sc (C64SC)** y realizar *hard reset* antes de cambiar CRT.
3. Cargar el CRT del TEST objetivo. Para TEST19: opción 2 BGFONT.
4. Si el ejecutable diagnóstico indica RAM VERIFIED, continuar con RETURN.
5. Insertar la variante D64; comprobar nombre e ID de volumen (A, B o C) después de DIR.
6. Registrar qué archivos aparecen y cuáles cargan; guardar capturas distintas para LIST, LOAD y SAVE.
7. Ante fallo: registrar dirección JAM o código DOS y preservar instantánea VSF si es posible.
8. Comparar huellas SHA-256 y manifiestos para descartar haber probado otro test.

No comprometer código o binarios de terceros a repositorios públicos sin revisar permisos. Las fuentes propias del launcher y del filtro pueden incorporarse separadamente tras limpieza y revisión de dependencias.