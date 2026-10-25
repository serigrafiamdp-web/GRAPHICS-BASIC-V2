# Features, principios e ideas pendientes

Separar «funciona hoy» de «queremos construir». Una feature prevista no se registra como implementada.

## Funciones ya demostradas
- Menú de 4 opciones en CRT EasyFlash: EPYX BASIC, BGFONT, SPRITE, BASIC C64.
- EPYX BASIC desde ROM, sin cargar su G64 protegido en unidad 8.
- BGFONT y SPRITE independientes; dependencia de $AE/$AF recuperada durante la carga desde CRT.
- SPRITE sin pantalla de presentación (TEST13).
- En BGFONT TEST18: listado de archivos SEQ normales además de los recursos específicos EPYX.
- Reproducibilidad del problema del directorio con discos TEST16 A/B/C.

## UX y manejo de archivos — alta prioridad
- Filtro **compatibles** que liste pantallas/font originales y SEQ normales verificados por cabecera.
- **ALL FILES** bajo opción explícita, con iconos/indicadores de tipo y desactivación de LOAD sobre archivos incompatibles.
- Cambiar disco/unidad y refrescar directorio automáticamente.
- Mostrar estado de unidad, volumen e ID reales; evitar confundir listado cacheado.
- Separar **OPEN SCREEN**, **OPEN FONT** y **OPEN SPRITE**, con contenido detectado automáticamente.
- Evitar errores silenciosos, escribir mensajes de estado DOS 1541 y ofrecer salida segura.
- Copiar/exportar recursos entre discos sin tener que formatear desde Toolkit ni cambiar bits a mano.

## Integridad — requisito de lanzamiento
- Nunca dejar un archivo abierto (*PRG) al fallar SAVE o interrumpirse la operación.
- Guardado temporal + comprobación + sustitución cuando el DOS/memoria lo permitan.
- Protección ante disco lleno, write-protect, sector defectuoso y desconexión.
- Backup/export antes de operaciones que puedan cambiar BAM y directorio.
- Mantener inalterados los datos gráficos binarios de TX/CH/SP.

## Runtime y cartucho
- Inicio directo opcional en EPYX BASIC en vez de menú, con atajo al launcher.
- Retorno al launcher desde cada editor y reset de entorno limpios.
- Persistencia de algunas preferencias en un área controlada; no confundir ROM con RAM.
- Módulos en bancos EasyFlash cargados bajo demanda.
- Gestor/launcher de programas desarrollados con la suite: importar PRG y **reconstruir CRT en la PC** (etapa inicial); grabar flash desde la C64 queda para una fase posterior.
- Espacio para librerías gráficas, recursos compartidos y herramientas auxiliares.

## Rendimiento
- El cartucho mejora el arranque al eliminar lecturas de ejecutables desde disco, **no** aumenta la velocidad del 6510 por sí mismo.
- Optimizar rutinas ASM de texto, gráficos y copia solo con medición PAL/NTSC.
- Evaluar fastloader IEC opcional, manteniendo carga estándar como respaldo.
- Evitar optimizaciones que alteren tiempos de IRQ/SID o rompan hardware real.

## Compatibilidad y archivo histórico
- Preservar los G64 y D64 como datos históricos independientes del CRT.
- Registrar autor, procedencia y condiciones de redistribución.
- Mantener los formatos originales de la suite para programas BASIC con comandos extendidos.
- Documentar para interoperabilidad LVLLVL el formato 40×25 TX/CH y charset HIRES.

## Ideas a evaluar (no comprometidas)
- Explorador de disco integrado con vista previa rápida de screen/font/sprite.
- Identificación automática de pares BACK/FONT mediante cabeceras TX/CH.
- Emisión de D64 estándar desde la PC con tipos de archivo correctos o normalizados.
- Bancos de ROM para galerías o plantillas, sin ocupar RAM hasta seleccionarlas.
- Pruebas automatizadas en emulador y comparaciones byte a byte tras LOAD/SAVE.