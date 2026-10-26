# Discos de trabajo, tipos de archivo y compatibilidad EPYX

## Problema reproducido

EPYX BASIC en el cartucho puede listar archivos mediante **DIR**, mientras que el editor BGFONT independiente **no muestra recursos externos** que sí están en el D64. Las tres variantes de TEST16 demostraron que el filtro de BGFONT depende del tipo codificado en las entradas del directorio Commodore DOS.

No confundir:
- **Formateo/BAM/sectores de un D64 estándar**;
- **tipo y banderas del byte de directorio**;
- **cabecera interna del contenido** (TX, CH, SP);
- **comportamiento de lista/LOAD/SAVE de cada editor**.

## Fe de erratas de Epyx

El addendum original indica:
- `CREATE` no crea un disco de autoarranque. Forma un disco con programa BASIC y soporte de Toolkit, que se carga mediante `LOAD"*",8,1`.
- Los recursos complementarios (sprites y backgrounds) **deben copiarse aparte** al disco creado.
- En pantallas de disco, `init` fue renombrado **`format`** y `new disk` pasó a **`dir`**.
- `CREATE` puede preparar un programa para ejecutarse en otra C64 sin Toolkit instalado, pero **no** se documenta como un paso necesario para reconocer cada D64 de recursos.

Fuente física: `Epyx_Programmers_BASIC_Toolkit_Addendum.pdf`, páginas 1–2.

## Códigos de tipo observados

| Byte DOS de entrada | Recurso en los D64 estudiados | Observación |
| --- | --- | --- |
| **$81** | Archivo SEQ normal | Invisibilizado por BGFONT clásico |
| **$91** | SPRITE EPYX | No debe figurar en el listado filtrado de BGFONT |
| **$A1** | FONT EPYX | Visible en BGFONT |
| **$B1** | SCREEN EPYX | Visible en BGFONT |
| **$82** | PRG normal | No es un recurso de BGFONT |
| **$02** | PRG sin cerrar (*PRG) | No confundir con protección |

Los bits altos/centrales pueden representar información privada de EPYX y banderas DOS. Estos bytes están **observados** en imágenes del usuario y pruebas, no forman parte del estándar universal SEQ.

## Formatos binarios vistos en los proyectos

- **TX / BACK**: cabecera TX, mapa gráfico de 40×25 y atributos por celda. Ejemplos `bb6.back.seq`, `uguis.back.seq`.
- **CH / FONT**: cabecera CH y 256 caracteres de 8×8 bits, con atributos. En el extractor validado de YPF el charset empieza en **offset 5**, no 6.
- **SP / SPRITE**: otra familia de recursos; no debe reconocerse como TX/CH solo por el nombre `.SPR` si se pretende compatibilidad genérica.

**Advertencia:** que el archivo se liste no prueba que BGFONT pueda cargarlo con seguridad. Validar la cabecera antes de activar LOAD es una mejora futura.

## Pruebas empíricas A/B/C

Con contenido idéntico `BB6TEST.SEQ` y `BB6TEST.FONT`, cada disco tiene un nombre de volumen distinto para descartar caché o montaje incorrecto:

| Variante | Pantalla | Fuente | Observación en VICE |
| --- | --- | --- | --- |
| T16-A-EPYX | $B1 | $A1 | Los dos nombres son visibles |
| T16-B-NORMAL | $81 | $81 | Los dos nombres están ocultos |
| T16-C-MIXTO | $81 | $A1 | Solo aparece `BB6TEST.FONT` |

Los recursos **no desaparecen físicamente** cuando no se muestran en la interfaz.

En TEST18 se amplió el filtro: se ven SEQ normales, pero también recursos ajenos. TEST19 y TEST20 **fallaron en VICE**. TEST21 resolvió el filtro de tipos sin rutinas auxiliares y quedó como **MASTER funcional** tras las pruebas de TEST26. Algunos archivos `.SPR` almacenados como SEQ `$81` siguen visibles; se decidió aceptar ese comportamiento y no introducir filtros por nombre tras los JAM de TEST22–25. `TEXT LOAD` permite recuperar en EPYX BASIC las pantallas guardadas con BGFONT.

## Problemas de integridad vistos en un D64 de proyecto

En `d64 ugis.d64` se detectó un `*PRG` con longitud de 0 bloques y referencias cruzadas a otro recurso. Esto concuerda con un archivo sin cerrar, no demuestra una firma de protección. **No ejecutar VALIDATE, FORMAT ni borrar el archivo sobre el único original**, ya que podría afectar contenido compartido.

Los D64 de ensayo iniciales tenían una anomalía de cabecera DOS (ID/tipo desplazado). No utilizarlos como patrón definitivo; emplear imágenes formateadas estándar y pruebas de BAM/directorio/cadena completas.

## Solución objetivo

1. Mostrar únicamente recursos relevantes para cada editor; incluir $81 estándar y $A1/$B1 de EPYX.
2. Leer TX/CH/SP internamente antes de LOAD; mensajes claros para archivos incompatibles o truncados.
3. Soportar recursos copiados desde cualquier D64 DOS 1541 **sin CREATE ni marcadores manuales**, y también los discos EPYX antiguos.
4. REFRESH del directorio al detectar cambio de unidad/medio, con selector 8/9.
5. Guardar y cerrar archivos correctamente, verificar lectura posterior y presentar error de canal 15.
6. Preservar tipo original cuando corresponda; ofrecer importación/conversión de recursos de terceros sin sobrescribir datos.

## Tabla mínima de regresión

Probar discos estándar recién formateados, A/B/C, D64 EPYX antiguo y un D64 con `*PRG` en **copias**. Verificar listados, carga de TX/CH real, guardado, recarga y comparación de bytes antes de anunciar compatibilidad total.