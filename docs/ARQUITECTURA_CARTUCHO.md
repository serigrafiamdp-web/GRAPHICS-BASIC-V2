# Arquitectura — EasyFlash / EPYX BASIC Toolkit

## Objetivo operativo

```text
ENCENDIDO / RESET DE LA C64
          |
          v
  EASYFLASH (CRT 1 MiB)
          |
          v
  LAUNCHER (banco 0)
   |     |     |     |
   1     2     3     4
   |     |     |     |
 EPYX  BGFONT SPRITE C64
 BASIC EDITOR EDITOR BASIC
   |     |     |     |
   +-----+-----+-----+
          |
       DOS/IEC
          |
  Unidad 8 / 9: D64 / 1541
  Proyectos BASIC, fondos,
  fuentes y sprites
```

EPYX BASIC y los editores se empaquetan **como PRG en bancos de ROM** y son reconstruidos en RAM por un cargador al seleccionarlos. EasyFlash **no aumenta la RAM convencional** de 64 KiB ni convierte un G64 archivado en una unidad de disco emulada.

## Módulos actuales

| Componente | Fuente en etapa actual | Estado |
| --- | --- | --- |
| Núcleo EPYX BASIC | `TOOLKIT BASIC` extraído de D64 CSDb 2022 | Arranque validado desde CRT |
| BGFONT | `bgfont(1).prg` independiente | Arranque validado; TEST18 amplió listado de SEQ; TEST19 pendiente |
| SPRITE | Ejecutable independiente del D64 aportado | Arranque validado; presentación omitida desde TEST13 |
| C64 BASIC | ROM/KERNAL convencional | Opción 4 |
| Datos G64 históricos | G64 Epyx 1985 original | Fuente histórica, no se ejecutan pistas GCR desde ROM |
| Proyectos | Disquetes D64 normales | Se busca compatibilidad total |

## Organización de bancos (modelo del prototipo)

El CRT EasyFlash contiene **64 bancos lógicos de 16 KiB**, presentados mediante ventanas de ROM de 8 KiB; la distribución exacta debe cotejarse con el manifiesto de cada versión.

- **Banco 0:** bootstrap, selector y copiador ROM→RAM; contiene tabla de integridad.
- **Banco 1:** núcleo BASIC comprimido (slot principal).
- **Bancos 2–4:** BGFONT independiente, en varios segmentos.
- **Banco 5 y siguientes:** SPRITE y material adicional; parte del G64 se archivó como datos en prototipos anteriores.
- **Bancos restantes:** reservados para módulos, datos y evolución.

**No grabar direcciones de banco como API permanente:** la distribución puede cambiar al regenerar cartuchos. Utilizar manifiestos/versiones.

## Ruta de lanzamiento de cada editor

1. Leer la elección del menú y seleccionar los bancos correspondientes.
2. Copiar el cuerpo del PRG original desde las ventanas ROM a la RAM indicada por su dirección de carga.
3. Restaurar registros/estado de carga que esperan los desempaquetadores, especialmente **$AE/$AF** (final de la carga).
4. Deshabilitar la visibilidad del cartucho para que el programa pueda usar RAM en $8000–$9FFF.
5. Entregar el control al punto de entrada apropiado sin suponer que todos los PRG tienen la misma estructura BASIC.
6. El editor administra sus propios recursos; el launcher no debe modificar el DOS de la unidad.

## Independencia de herramientas

Las opciones 2 y 3 proceden de versiones independientes y **no requieren entrar antes a EPYX BASIC**. Es un objetivo importante que trabajen con discos D64 comunes y compartan recursos entre proyectos.

## Proyección técnica, no implementada

- Conmutación y regreso limpio editor ↔ launcher sin reset.
- Gestor de recursos común: reconocimiento por cabecera, carga segura, extensiones y tipos de directorio, duplicación y renombrado.
- Guardado atómico y error-reporting real de DOS 1541.
- Unidades 8/9 con selector y refresco automático de directorios.
- Transferencia acelerada solo después de mantener fallback a DOS estándar.
- Cartucho con programas del usuario y menú de arranque regenerable. Es distinto de escritura directa en flash desde C64, que requerirá un sistema de gestión de sectores de borrado.

## Políticas de integridad

No sobrescribir TEST13 ni los originales. Mantener hashes de CRT/PRG y registrar diferencias entre bancos. No reutilizar pruebas sobre D64 con material irremplazable.