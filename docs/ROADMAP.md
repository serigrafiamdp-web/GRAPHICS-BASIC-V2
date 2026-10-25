# Roadmap — fases y pruebas de aceptación

## Fase 0 · Preservación y base segura — EN MARCHA
- [x] Capturar G64 1985, G64 repaired, D64 CSDb 2022 y editores independientes con hashes.
- [x] Obtener un CRT de cuatro opciones que arranca EPYX BASIC y editores independientes.
- [x] Evitar portada del Sprite Editor: TEST13.
- [x] Congelar TEST13 como base funcional.
- [ ] Depositar scripts de build propios y manifiestos versionados para builds reproducibles.
- [ ] Definir política de distribución de los binarios originales en repositorio público.

## Fase 1 · Directorio BGFONT universal — TRABAJO ACTUAL
- [x] A/B/C demuestra filtro de tipos de directorio.
- [x] TEST18 demuestra visibilidad de SEQ $81 en BGFONT.
- [x] **TEST19 en VICE: FAIL**; vuelve a BASIC al leer el directorio (sin PASS).
- [x] **TEST20 en VICE: FAIL**; al acceder al directorio vuelve a BASIC.
- [ ] **TEST21 en VICE:** prueba de filtro inline desde TEST18. Confirmar $81/$A1/$B1 visibles y $91/$82 ocultos sin salir a BASIC.
- [ ] Validación posterior por cabeceras TX/CH: TEST21 podría admitir clases especiales $A2/$B2.
- [ ] Cargar sin corrupción BB6TEST.SEQ y BB6TEST.FONT desde D64 estándar.
- [ ] Clasificar archivos estándar por cabeceras TX/CH para no mostrar sprites/otros SEQ.
- [ ] Opcional: modo ALL FILES, claramente separado del filtro seguro.
- [ ] Manejar discos cambiados sin caché persistente y confirmar unidad activa.

**Aceptación:** BGFONT abre los dos BB6 en A/B/C, no lista tipos incompatibles en modo normal y conserva compatibilidad con recursos originales. Ningún cambio de formato/manual es obligatorio.

## Fase 2 · Guardado robusto e interoperabilidad
- [ ] SAVE en D64 nuevo, CLOSE correcto, examinar canal de estado 1541 y reabrir.
- [ ] Determinar por qué aparecen `*PRG` y evitar cadenas de sectores inacabadas.
- [ ] Probar 100 guardados/recargas en copias de disco con comparación de bytes.
- [ ] Nombres, sustitución segura, manejo de disco lleno y errores de escritura.
- [ ] Compatibilidad con niveles/recursos `TX/CH` convertidos a LVLLVL, sin pérdida.

**Aceptación:** sin `*PRG`, sin cambios al archivo al reabrir, errores explicados, proyectos transferibles.

## Fase 3 · SPRITE Editor
- [x] Salto de portada/cracktro a editor (TEST13).
- [ ] Estudiar filtro y rutinas DIR/LOAD/SAVE de sprites $91 y SEQ estándar.
- [ ] Detectar cabecera SP y validar datos antes de mostrar/cargar.
- [ ] Pruebas de sprites entre D64 sin formato especial.

## Fase 4 · Cartucho Enhanced y flujo de usuario
- [ ] Eliminar mensajes de diagnóstico visibles de la versión final (conservar en modo test).
- [ ] Volver de BGFONT/SPRITE al menú sin perder estado.
- [ ] Arranque y configuración estables en PAL/NTSC, VICE x64sc y hardware.
- [ ] Gestor de proyectos común, selector de unidad 8/9, directorios con actualización automática.
- [ ] Incorporar HELP y herramientas adicionales según memoria y licencias.

## Fase 5 · Optimización y extras
- [ ] Medir arranque, E/S, tiempos de dibujo, parser BASIC y operaciones de archivo.
- [ ] Optimizar rutinas 6502 y acceso IEC con compatibilidad DOS estándar.
- [ ] Empaquetador de PRG del usuario al CRT con manifiesto y launcher autogenerado.
- [ ] Evaluar persistencia en flash (mucho más compleja que regenerar un CRT en PC).
- [ ] Recursos compartidos con editor web LVLLVL, con formato documentado.

## Política de pruebas
Cada TEST nuevo documenta: base, archivos, SHA-256, bytes/bancos cambiados, pasos en VICE, resultado, regresiones y decisión PASS/FAIL. Nunca declarar PASS de carga/guardado con solo una prueba de listado.