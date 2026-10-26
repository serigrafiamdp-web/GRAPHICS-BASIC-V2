# GRAPHICS BASIC V2
### EPYX BASIC Toolkit Enhanced · Commodore 64 · EasyFlash

**Proyecto de preservación, investigación y modernización de EPYX Programmer's BASIC Toolkit (1985).**

Objetivo: ejecutar EPYX BASIC, BGFONT y SPRITE desde un cartucho **EasyFlash** y dejar la **unidad 8 libre para guardar programas y recursos** en discos D64 estándar. La prioridad es compatibilidad de archivos, estabilidad y recuperación fiel de las herramientas originales; las optimizaciones llegarán después de pruebas reproducibles.

> **Estado al 25 de octubre de 2026:** **TEST21 queda como MASTER funcional**. Confirmados en VICE directorio, cargas, guardado/recarga de pantallas (TEST26), fuentes y recuperación mediante `TEXT LOAD` en EPYX BASIC. Los intentos TEST22–25 de ocultar .SPR fallaron o regresaron a BASIC y no forman parte del MASTER. Algunos .SPR siguen visibles por decisión aceptada. Se preservan TEST18 y TEST21 sin modificaciones.

## Menú de cuatro opciones

1. **EPYX BASIC** — núcleo tomado del ejecutable TOOLKIT BASIC de CSDb 2022, arranque desde ROM sin disco del sistema.
2. **BACKGROUND AND FONT EDITOR** — BGFONT independiente; en TEST18–TEST21 se investiga la compatibilidad de directorios.
3. **SPRITE EDITOR** — ejecutable independiente con salto que omite la portada del crack (TEST13).
4. **C64 BASIC** — retorno al BASIC estándar.

## Documentación

| Documento | Contenido |
| --- | --- |
| [Estado y pruebas](docs/ESTADO_Y_TESTS.md) | Cronología TEST04–TEST26, PASS, FAIL y validación en VICE |
| [Arquitectura](docs/ARQUITECTURA_CARTUCHO.md) | EasyFlash, ROM/RAM, módulos, flujo de ejecución, disco |
| [Mapa de memoria y parches](docs/MAPA_MEMORIA_Y_PARCHES.md) | Direcciones, rutinas, comprobaciones, cautelas |
| [Discos y formatos](docs/DISCOS_Y_FORMATOS.md) | Directorio DOS, $81/$91/$A1/$B1, TX/CH, pruebas A/B/C |
| [Roadmap](docs/ROADMAP.md) | Fases, criterios de aceptación y pruebas de regresión |
| [Ideas y features](docs/FEATURES_E_IDEAS.md) | Propuestas ordenadas y alcance |
| [Fuentes y reproducción](docs/FUENTES_Y_REPRODUCCION.md) | Materiales de referencia, herramientas y reconstrucción |
| [Historial](docs/CHANGELOG.md) | Decisiones y cambios conocidos |
| [TEST26 PASS funcional](docs/TEST26_PASS_25_OCT_2026.md) | Registro definitivo de BGFONT SAVE/LOAD y EPYX BASIC TEXT LOAD |

## Principios

- **No sobrescribir los TEST que dieron PASS en VICE**. Cada modificación crea otra versión.
- Separar *comprobación estructural*, *arranque en emulador* y *prueba real de lectura/escritura*.
- Preservar los G64/D64/PRG originales sin alterarlos.
- No confundir un D64 reconocido por el comando DIR de EPYX BASIC con archivos visibles o cargables desde BGFONT y SPRITE.
- No ejecutar pruebas de escritura sobre discos con proyectos únicos; siempre duplicar el D64 antes.
- Mantener ROM y unidades de disco como responsabilidades separadas: **el G64 guardado como datos en un CRT no se convierte en una 1541 montada**.

**Nota sobre material original:** este repositorio contiene documentación técnica y pruebas propias. No se incluyen aquí imágenes/distribuciones de terceros ni ejecutables originales sin confirmar las autorizaciones correspondientes. Se preservan aparte con sus sumas SHA-256.

## Estado validado y próxima etapa

**TEST21 / TEST26 PASS:** BGFONT funciona con directorios D64, carga y guarda pantallas y fuentes; EPYX BASIC recupera los fondos con `TEXT LOAD`. **El CRT TEST26 es byte a byte el mismo de TEST21.** El filtrado de .SPR se deja pendiente voluntariamente; evitar repetir los JAM de TEST22–25. Próximas pruebas: robustez SAVE/CLOSE, manejo de errores DOS y Sprite Editor. Ver [acta de PASS](docs/TEST26_PASS_25_OCT_2026.md).

---
Repositorio: https://github.com/serigrafiamdp-web/GRAPHICS-BASIC-V2
