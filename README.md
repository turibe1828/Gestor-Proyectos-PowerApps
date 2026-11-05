# Gestor de Proyectos Demo · Power Apps

Aplicación **canvas de Power Apps** para dar seguimiento a proyectos y a sus hitos: tablero tipo kanban, lista con filtros, cronograma (diagrama de Gantt), formularios de alta y edición, comentarios por proyecto y solicitudes de cambio de fecha con aprobación.

> **Todos los datos de este repositorio son ficticios.** Personas, clientes, proyectos y correos (`@example.com`) son inventados. La app no se conecta a ningún servicio externo: los datos se generan como colecciones locales al iniciar.

## Pantallas

| Pantalla | Qué hace |
|---|---|
| `scrInicio` | Saludo, indicadores (completados, por vencer, vencidos), tareas y proyectos recientes, y gráfico de proyectos completados por mes. |
| `scrMisProyectos` | Proyectos en vista **Tablero** (columnas por estado) o **Lista** (tabla ordenable), con búsqueda, filtros y filtros rápidos. |
| `scrMisHitos` | Lo mismo para los hitos, con marcado rápido como completado. |
| `scrFormularioProyecto` | Alta y edición del proyecto: avance real, índice SPI, hitos relacionados y panel de comentarios. |
| `scrFormularioHito` | Alta y edición de hitos, con validación de peso y fechas. |
| `scrCronogramaProyectos` | Gantt de proyectos por semanas o meses, con navegación entre periodos. |
| `scrCronogramaHitos` | Gantt de los hitos de un proyecto. |
| `scrAprobaciones` | Solicitud de cambio de fecha de fin, dirigida al supervisor. |
| `scrInformePowerBi` | Espacio reservado para un informe de Power BI (vacío en la demo). |

Componentes reutilizables: menú lateral (`cmpMenuLateral`), encabezado (`cmpEncabezado`), comentarios (`cmpComentarios`) y ventanas emergentes (`cmpPopupAgregar`, `cmpPopupEliminar`, `cmpPopupCargando`, `cmpPopupError`, `cmpPopupExito`).

## Tecnologías

- **Power Apps** (canvas app) con código fuente en formato **`.pa.yaml`**.
- **Power Fx**: fórmulas con nombre (`App.Formulas`), colecciones, carga por lotes en paralelo con `Concurrent`, permisos por rol y cálculo de avance ponderado y SPI.
- Contenedores con *auto-layout* para el diseño responsive.
- Íconos e imágenes en SVG generados con fórmulas, sin archivos binarios.

## Convención de nombres

Todo elemento empieza con un prefijo de 3 letras en minúscula que indica su tipo, seguido de un nombre descriptivo en PascalCase, sin espacios, tildes, guiones ni números de posición. Por ejemplo: `scrInicio`, `lblTituloMisProyectos`, `txtBuscarMisHitos`, `btnGuardarProyecto`, `cntEncabezadoFormHito`.

| Prefijo | Tipo | Prefijo | Tipo |
|---|---|---|---|
| `scr` | Pantalla | `gal` | Galería |
| `lbl` | Etiqueta / texto | `ico` | Ícono |
| `txt` | Entrada de texto | `cmb` | Cuadro combinado |
| `tbl` | Tabla | `fld` | Campo de cuadro combinado |
| `cnt` | Contenedor | `chk` | Casilla |
| `btn` | Botón | `tgl` | Interruptor |
| `img` | Imagen | `dtp` | Selector de fecha |
| `frm` | Formulario | `crd` | Tarjeta de datos |
| `rec` | Rectángulo | `sld` | Control deslizante |
| `rtg` | Calificación (estrella) | `spn` | Indicador de carga |
| `cht` | Gráfico | `pbi` | Mosaico de Power BI |
| `bdg` | Insignia | `cmp` | Componente |

## Diseño responsive

- **Puntos de quiebre:** `App.SizeBreakpoints = [600, 900, 1200]`, que dan celular, tablet y escritorio. Cada pantalla se adapta con `App.ActiveScreenSize`.
- **Estructura de cada pantalla:** `cntPantalla…` (menú + principal) > `cntPrincipal…` (encabezado + contenido), con `LayoutDirection`, `FillPortions`, `LayoutWrap` y `LayoutOverflow`.
- **En celular:**
  - El menú lateral se oculta y se abre con el botón ☰ del encabezado.
  - Los indicadores y las columnas se apilan.
  - Los filtros secundarios se ocultan.
  - Las tablas anchas y el Gantt tienen scroll horizontal propio.
- **Tokens de diseño:** en `App.Formulas` están `Fuente`, `Espacio`, `Colores` y `MyTheme`.

## Estructura del repositorio

```
Src/
├── App.pa.yaml               App.Formulas, App.OnStart (datos ficticios) y puntos de quiebre
├── scr*.pa.yaml              Una pantalla por archivo
├── _EditorState.pa.yaml      Orden de pantallas y componentes
└── Components/cmp*.pa.yaml   Componentes
```

## Cómo usarla

El repositorio contiene **solo el código fuente** `.pa.yaml`, sin el paquete binario (`.msapr`/`.msapp`).

**Para leer y revisar el código:** basta con abrir los archivos de `Src/`.

**Para probarla en Power Apps Studio:**

1. Crea una app canvas en blanco con formato de tableta.
2. Copia en **App** las propiedades `Formulas`, `OnStart` y `SizeBreakpoints` de `Src/App.pa.yaml`.
3. Crea los componentes. Para cada archivo de `Src/Components/`, pega su YAML (clic derecho > *Pegar*, o *Pegar código*).
4. Crea las pantallas: pega el YAML de cada `Src/scr*.pa.yaml`.
5. En **Configuración > Pantalla**, desactiva *Escalar para ajustar* y *Bloquear relación de aspecto*.

**Para empaquetarla con la CLI** ([Power Platform CLI](https://aka.ms/PowerPlatformCLI)), si cuentas con el `.msapr` de una app propia junto a `Src/`:

```powershell
pac canvas pack --sources . --msapp GestorProyectosDemo.msapp --layout SourceCode
```

Después abre el `.msapp` en Power Apps Studio (**Archivo > Abrir > Examinar**).

### Limitaciones del modo demo

- Las listas son colecciones locales, así que los cambios se pierden al cerrar la app.
- La consulta de casos, la creación automática de hitos y la solicitud de aprobación están **simuladas**. Los casos de ejemplo para el botón *Consultar* son `1001` y `1002`.
- Los formularios trabajan sobre colecciones. Al crear registros nuevos con `SubmitForm` puede ser necesario asignar el `ID` a mano, cosa que una fuente de datos real hace sola.
