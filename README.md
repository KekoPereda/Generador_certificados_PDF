# Generador de Certificados · Fundación ATE SM

Página web (`index.html`) para crear certificados y diplomas en PDF a partir de un Excel con los participantes.
Cada certificado lleva el nombre del participante, el nombre de la formación y su descripción, sobre el diseño oficial de Fundación ATE SM (logo, "certifica que:", "Participó en la capacitación:" y la firma de Francisco Tepper).

Todo funciona dentro del navegador: los datos no se envían a ningún servidor.

## Requisitos

- Google Chrome o Microsoft Edge (recomendado). Safari y Firefox también sirven, salvo el guardado automático del historial en un archivo.
- Conexión a internet al abrir la página, porque las librerías para leer Excel y crear los PDF se descargan al cargarla.

No hay que instalar nada: basta con hacer doble clic en `index.html`.

## Cómo se usa

1. **Participantes.** Arrastra el Excel al recuadro o haz clic para elegirlo. Solo se aceptan archivos Excel (`.xlsx`, `.xls`) o CSV (`.csv`).
   - **Otros archivos:** si subes otra cosa (por ejemplo un `.js`, un PDF o una imagen), aparece un mensaje en rojo y el archivo no se carga.
   - **Extensión cambiada:** también se rechaza un archivo con extensión de Excel cuyo contenido no sea realmente un Excel.
   - La página busca sola la hoja y la columna cuyo encabezado contiene "Nombre". Si no acierta, elígelas en los selectores **Hoja** y **Columna de nombres**.
   - También puedes pegar nombres a mano, uno por línea, y pulsar **Agregar a la lista**.
2. **Datos de la formación.** Escribe el **nombre de la formación** y la **descripción** (colegio, fecha y duración).
   - Si el Excel trae las columnas "Capacitación" y "Descripción", los campos se llenan solos.
   - En la descripción, usa Enter para cortar la línea donde quieras. Se recomiendan hasta 3 líneas.
3. **Lista de participantes.** Revisa la lista:
   - corrige un nombre escribiendo sobre él;
   - desmarca a quien no deba recibir certificado, o quítalo con ✕;
   - haz clic en una fila para ver su certificado en la **Vista previa**;
   - si los nombres vienen en MAYÚSCULAS, activa **Nombre Propio** (MARÍA PÉREZ → María Pérez).
4. **Descargar.**
   - **Descargar ZIP:** un PDF por participante, dentro de un ZIP.
   - **Un solo PDF con todos:** todos los certificados en un archivo, una página por persona.
   - **⬇ en una fila:** solo el certificado de esa persona.

Si un nombre, la formación o la descripción es muy largo, la letra se achica o el texto pasa a otra línea para que no tape la firma.

## Formato del Excel

Sirve la planilla habitual "CERTIFICADOS DIGITALES CAPACITACIONES 2026". Lo importante es que la fila de encabezados incluya:

| Columna | Encabezado que debe contener | Uso |
|---|---|---|
| Nombre | "Nombre" (por ejemplo "Nombre y apellido") | Obligatoria. Un participante por fila. |
| Capacitación | "Capacitación", "Formación", "Curso" o "Taller" | Opcional. Llena el nombre de la formación. |
| Descripción | "Descripción" | Opcional. Llena la descripción. |

Los nombres se imprimen tal como están escritos. Si alguno viene con los apellidos primero, corrígelo en el Excel o en la lista antes de generar.

## Nombres de los archivos

| Descarga | Nombre |
|---|---|
| Cada PDF (en el ZIP o individual) | `CertificadoSM_<Nombre del participante>.pdf` |
| ZIP | `Certificado_SM_AAAA_MM_DD_HH-MM-SS.zip` (fecha y hora de la descarga, así nunca se repite) |
| PDF único | `Certificados_SM_todos.pdf` |

- **Caracteres no permitidos:** se quitan del nombre del archivo (`/ \ : * ? " < > |`).
- **Nombres repetidos:** si dos participantes se llaman igual, el segundo se guarda como `... (2).pdf`.

## Historial de capacitaciones

El botón **🕘 Historial** del encabezado abre una ventana con todas las capacitaciones generadas. El número rojo indica cuántas hay.

**Qué se registra:** cada vez que se generan certificados se guardan la fecha y hora, el nombre de la formación, la descripción, el archivo Excel usado y la lista de participantes.

**Registros repetidos:** si generas la misma capacitación (mismo nombre y descripción) el mismo día, se actualiza el registro en vez de duplicarlo. Las descargas individuales suman a esa persona al registro del día.

**En la ventana del historial:**
- Haz clic en una capacitación para ver su detalle: datos y lista numerada de participantes. Desde ahí puedes **Copiar nombres** o **Usar esta capacitación**.
- **↺** carga el nombre, la descripción y (si lo confirmas) los participantes en el formulario.
- **✕** elimina el registro.

### Dónde se guarda

1. **En el navegador (automático).** Se guarda en el `localStorage` de Chrome, con la clave `historialCapacitacionesSM`. Se mantiene al cerrar el navegador, pero tiene límites:
   - solo existe en ese navegador y ese computador;
   - si mueves o renombras la carpeta, el historial se ve vacío hasta que la devuelvas a su lugar;
   - se pierde si se borran los datos de navegación.
2. **En un archivo JSON (Chrome o Edge).** Con **📎 Guardar automáticamente en archivo JSON** eliges una vez el archivo, por ejemplo `historial_capacitaciones.json` en esta carpeta. Desde entonces se actualiza solo cada vez que generas certificados.
   - Si eliges un archivo que ya existe, su contenido se suma al historial.
   - Chrome puede volver a pedir permiso para modificarlo en otra sesión: acéptalo.
3. **Respaldo manual.** **⬇ Exportar JSON** descarga una copia. **⬆ Importar JSON** la vuelve a cargar, por ejemplo en otro computador; los registros importados se suman sin duplicarse.

Se recomienda vincular el archivo JSON (opción 2) o exportar de vez en cuando, para no depender solo del navegador.

### Formato del JSON

```json
{
  "actualizado": "2026-11-03 11:16",
  "capacitaciones": [
    {
      "id": "mw0rubtf0h3dn",
      "fecha": "2026-11-03 11:16",
      "titulo": "Estrategias para el abordaje y resolución de conflictos en la escuela.",
      "descripcion": "Realizada para la Escuela El Eden,\nJulio y octubre de 2026. Duración: 20 horas.",
      "archivoExcel": "Copia de Copia de DIPLOMAS  EL EDEN_ 4.xlsx",
      "cantidad": 37,
      "participantes": ["Paula Andrea Acevedo Muñoz", "Teresita Valeria Adriazola Jerez"]
    }
  ]
}
```

## Archivos de la carpeta

| Archivo | Descripción |
|---|---|
| `index.html` | El generador. Es un solo archivo: el fondo del certificado y las tipografías Trebuchet MS van dentro, así que se puede copiar solo a otro computador. |
| `CertificadoSM_Anacaren Yanin Carrasco Suárez.pdf` | Certificado modelo del que se tomó el diseño. |
| `Copia de Copia de DIPLOMAS  EL EDEN_ 4.xlsx` | Ejemplo de Excel de participantes. |

## Detalles técnicos

- **Librerías** (desde cdnjs):
  - [SheetJS 0.18.5](https://sheetjs.com/) lee el Excel;
  - [jsPDF 2.5.1](https://github.com/parallax/jsPDF) crea los PDF;
  - [JSZip 3.10.1](https://stuk.github.io/jszip/) arma el ZIP.
- **Página:** A4 apaisado (841,89 × 595,28 pt).
  - El fondo es la imagen JPEG extraída del certificado modelo.
  - **Tipografías:**
    - nombre del participante: Helvetica 34 pt, negro;
    - nombre de la formación: Trebuchet MS Bold 21 pt, rojo `#980000`;
    - descripción: Trebuchet MS 18 pt, gris `#444444`.
- **Posición del texto:** en la función `dibujar()` de `index.html`.
- **Fondo y fuentes:** están incrustados en base64 en las constantes `FONDO`, `TREBUCHET` y `TREBUCHET_BOLD`. Para cambiar el diseño de fondo, reemplaza `FONDO` por otra imagen A4 apaisada en base64 (`data:image/jpeg;base64,...`).
