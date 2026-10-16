# App de proyectos · Denisse Castro Arquitectura
Especificación al 15/10/2026. Esta es la referencia para seguir mejorando la app.

**App publicada:** https://dcastroarq.github.io (GitHub Pages, repositorio dcastroarq/dcastroarq.github.io; se actualiza sola en 1–2 minutos con cada cambio).
La versión anterior en Claude (https://claude.ai/artifact/SPjYnFrBsaWNuB563RBapQ) ya no se usa.

Archivo fuente: `tablero-estudio.html` (una sola página HTML) + un puente a Firebase (`fb/shim.html`, `fb/build.py`) que genera `index.html`. Datos en Firebase (proyecto `dca-proyectos`, Firestore): `projects`, `assignments`, `activity`, `people`, `presence`, `config/settings`, `config/access` y `private` (presupuestos). Las reglas de Firestore protegen los mismos permisos que la app.

**Ingreso:** botón "Acceder con Google" o correo y clave (con "¿Olvidaste tu clave?"). Solo entran los correos autorizados en el panel **Accesos** (solo administración). Mientras se comprueba la sesión y mientras cargan los proyectos se ve una ruedita girando. Se puede agregar al inicio del celular (ícono con el logo, nombre "DCA Proyectos").

---

## 1. Personas y permisos
- **Administración:** denisse@dcastroarq.com y eprietonogales@diagonal.com.ec (Emilio Prieto), más quien se agregue como administrador en Accesos. El equipo entra con su correo agregado en Accesos.
- **Equipo:** Daniela Molina, Paul Moreira, Camila Coello, Juan José Ochoa, Adrián Cárdenas y Elvira Acosta. Denisse también diseña.
- Cada persona elige su nombre una sola vez. Solo la administración puede liberarlo.
- **Dos responsables por área (opcional, para proyectos grandes):** el responsable 1 es el encargado y el único que puede mover la tarjeta de fase; el responsable 2 la ve en su tablero y en Mis tareas (con la nota "Responsable 2"), y aparece en los filtros. Solo cuando hay 2 en la fase actual la tarjeta muestra "Responsables" con los dos nombres; si hay uno, queda igual.
  - Se elige al crear el proyecto (selector "2º" junto al responsable) o en "Ver proyecto completo".
  - En Correcciones y Planos se hereda del área cuando la tarea la lleva el mismo responsable 1; en el proyecto completo la administración puede poner otro 2º responsable para los planos. Renders sigue siendo solo de Paul.
  - Si el responsable 2 pasa a ser el responsable 1, el 2º se quita.
- Nadie del equipo puede cambiar tareas asignadas a otra persona.
- **Solo la administración** puede:
  - crear proyectos;
  - cambiar fechas y prioridades;
  - mover a columnas con 🔒 o sacar tarjetas de ellas;
  - ver el proyecto completo y el presupuesto.
- **Presupuesto:** es privado. Nadie del equipo puede ver el PDF ni los montos, y el asistente "Preguntar" tampoco los recibe.
- **Daniela Molina** (quien revisa planos) es la única persona fuera de la administración con un permiso extra, y solo en Planos: puede pasar planos de Por revisar a Revisado y enviar el proyecto a Enviados a cliente.

## 2. Crear proyecto (solo administración)
- **Campos:**
  - Proyecto: el nombre del cliente y el del proyecto son un mismo campo.
  - Fecha de inicio: hoy por defecto.
  - Revisión máxima: inicio + 4 semanas.
  - Prioridad: Alta, Media o Baja; se aplica a todas las áreas.
- La fase siempre arranca en Inicio. No hay notas generales.
- **Áreas:** se eligen con chips o se escriben a mano. Las áreas nuevas se recuerdan para la próxima vez.
  - Cada área tiene un responsable y un monto USD opcional. El total se suma solo, se puede editar y es privado.
- Al crear, se cierra el formulario y se abre la pestaña Proyectos.
- **Áreas especiales:**
  - **Distribución:** toda área cuyo nombre empiece por "Distribución", o marcada con el botón "Distribución", que agrega la palabra al inicio.
  - **Fachada:** toda área cuyo nombre empiece por "Fachada" o "Fachadas". Va directo a Renders con Paul y no aparece en Proyectos.

## 3. Pestañas (en orden)
Lista · Proyectos · Renders · Correcciones · Planos · Mis tareas (el asistente ✦ Preguntar no está disponible fuera de Claude)

### Proyectos (diseño)
- **Columnas:** Inicio → En proceso → Por revisar → Revisado 🔒.
- Cada área es una tarjeta independiente.
- **Al llegar a Revisado:**
  - un área normal crea una tarea de Renders para Paul;
  - una distribución pasa directo a Planos.
- Al final de Revisado hay dos listas compactas, con fecha: "Pasaron a Renders" y "Pasaron a Planos".
- **Regresar un área desde Revisado** a una fase anterior (solo administración) la saca de Renders, Correcciones y Planos, y vuelve a ser tarjeta de Proyectos.

### Renders (responsable: Paul)
- **Columnas:** Listo para renders → Renders listos → Revisados cliente 🔒 → Aprobados clientes 🔒.
- Paul solo mueve tarjetas entre las dos primeras columnas.
- Al abrir un área desde Renders, la administración puede cambiar a mano la fecha "Listo para renders desde" (actualiza la línea de fases y el cálculo de "más de 1 semana en Listo para renders").
- **Correcciones:** solo se registran desde Revisados cliente, abriendo la tarjeta desde Renders y si el área todavía no tiene planos. Hay dos botones: Corrección diseño y Corrección renders.
  - **Corrección de diseño:** va a la pestaña Correcciones, con quien diseñó el área.
  - **Corrección de renders:** vuelve directo a Renders con Paul.
  - **Fachadas:** cualquier corrección, sea de diseño o de renders, vuelve directo a Renders con Paul.
- Al pasar a Aprobados clientes se crea la tarea de Planos:
  - normalmente para quien diseñó el área;
  - las fachadas, por defecto para Daniela Molina (se puede cambiar).
- **Fechas en la tarjeta:**
  - "Revisión máx." del proyecto en la primera vuelta, y la de la corrección después de una corrección;
  - sin fecha en Revisados cliente ni en Aprobados clientes.

### Correcciones
- **Columnas:** Por corregir → En proceso → Por revisar → Revisado 🔒.
- **Fechas:** el inicio es el día del registro y la revisión máxima, 2 semanas después. Solo la administración las cambia.
- Incluye 3 correcciones por área. Desde la 4 se marcan como "adicional".
- **Al pasar a Revisado:**
  - una corrección de diseño vuelve a Renders como "Corrección N · diseño";
  - un cambio de distribución vuelve a Planos.
- Al final de Revisado hay dos listas: "Volvieron a Renders" y "Volvieron a Planos".
- El detalle muestra "Enviados al cliente: fecha" solo si esa fecha existe.

### Planos
- **Columnas:** Por hacer → En proceso → Por revisar → Revisado 🔒 → Enviados a cliente 🔒.
- **Fechas:** el inicio es el día de la aprobación y el envío máximo, 2 semanas después. Solo la administración las cambia.
- **Revisado:** una tarjeta por proyecto agrupa todas sus áreas: ✓ las revisadas y, en las que faltan, dónde están. Abajo dice "N de M áreas listas" y el envío máximo (el más tardío de sus áreas).
- **Enviar al cliente:** el proyecto completo pasa a Enviados a cliente solo cuando todas sus áreas están revisadas. No se puede enviar un área sola.
- Enviados a cliente es una lista compacta: proyecto, áreas y fecha.
- Un área cuenta como terminada al estar en Enviados a cliente. Cuando todas lo están, la Lista muestra "Terminado".

### Distribución (paso previo)
- En Proyectos va como cualquier área. Al llegar a Revisado no va a Renders, sino directo a Planos.
- **Cambios del cliente:** al abrir la distribución desde Planos, el botón "Registrar cambio" la manda a Correcciones como "Cambio N · distribución".
  - Se incluyen 3; desde el 4 son adicionales.
  - Mientras está en Correcciones, sale de Planos.
  - Al pasar a Revisado vuelve a Planos, con envío máximo a 2 semanas desde ese día.
- **Las demás áreas del proyecto, incluida la fachada, esperan a la distribución:**
  - Quedan en Inicio (la fachada, en Renders) con la etiqueta "Esperando distribución". Nadie las puede mover y no cuentan como atrasadas.
  - Se liberan solas cuando todas las distribuciones del proyecto llegan a Planos. Su revisión máxima es entonces 2 semanas desde el día en que se liberan.
  - "Empezar sin esperar" (solo administración) libera un área de inmediato y mantiene las fechas del proyecto.
  - Si después el cliente pide un cambio de distribución, las demás áreas siguen trabajando, con la etiqueta "Revisar cambios de distribución" mientras el cambio esté abierto.

### Lista
- **Columnas:** Fecha de inicio | Proyecto | Áreas y fase (categoría · fase de cada área) | Responsables | Status.
- Cada área va en su propia línea, con su responsable (nombre y apellido) en la misma línea, alineado bajo "Responsables". Anchos proporcionales (fecha 8%, proyecto 17%, áreas 35%, responsables 15%, status 25%). En celular cada proyecto se muestra como tarjeta: fecha y proyecto arriba, áreas con su responsable, y el status abajo.
- Se ordena por fecha de inicio, con los más recientes arriba.
- El filtro de responsables incluye "Sin responsable": muestra los proyectos que tienen al menos un área sin asignar (en Proyectos, Correcciones y Planos muestra solo las tarjetas sin responsable).
- **Puntito de prioridad** junto al proyecto y junto a cada área: rojo alta, amarillo media, gris baja. No hay leyenda.
- **Status:**
  - "En cronograma" cuando no hay nada vencido.
  - "Proyecto atrasado (categoría donde está el área ahora)"; debajo, el nombre de cada área atrasada y sus motivos en líneas separadas (sin repetir el nombre), cuando pasa alguna de estas cosas:
    1. no se envió al cliente antes de la revisión máxima del área (inicio + 4 semanas);
    2. más de 2 semanas en Inicio;
    3. más de 1 semana en Listo para renders;
    4. más de 1 semana en Por corregir;
    5. cualquier otra fecha automática vencida: revisión máxima de corrección o envío máximo de planos.
  - "Terminado" cuando todo está en Enviados a cliente.
- Una vez enviado al cliente, la fecha de 4 semanas deja de contar; solo cuentan las de corrección y planos.

### Mis tareas
- Una fila por área, agrupada por proyecto (sin código).
- **Cada fila muestra:** el área, "Categoría · fase" con el número de corrección o cambio, y la fecha de esa fase a la derecha.
- **Selector de fase:** solo aparece si la tarea es tuya en ese momento. Si no, dice "Ahora con X".
- Se ordena por fecha y prioridad.

## 4. Orden, prioridad y fechas
- Cada columna se ordena por la fecha de su categoría y luego por prioridad. Nadie puede subir o bajar tarjetas dentro de una columna: solo se mueven entre columnas.
- **Por área, solo la administración** puede cambiar la prioridad y la revisión máxima, abriendo la tarjeta. El cambio reordena todo automáticamente.
- **Colores de las tarjetas:**
  - Borde rojo: la fecha está vencida (solo por fecha).
  - Fecha en amarillo: por vencer.
  - Franja de color a la izquierda: identifica el proyecto.
  - Etiquetas: "Prioridad alta" en ámbar y "Prioridad baja" en gris.

## 5. Detalle de un área (al tocar una tarjeta)
- **Contenido:**
  - solo esa área;
  - fechas de su categoría: proyecto, corrección o planos;
  - línea de fases de esa categoría;
  - prioridad;
  - notas del área (las editan su responsable o la administración);
  - actividad del área.
- **"Ver proyecto completo"** (solo administración) incluye:
  - todas las áreas, con ✎ para cambiar el nombre (no altera nada más: si era distribución o fachada lo sigue siendo, y conserva su actividad), responsable, botón Distribución, Categoría + Fase (+ número de corrección o cambio) para ubicar el área directamente, y notas;
  - presupuesto privado;
  - historial de fases;
  - actividad;
  - Eliminar proyecto.
- **Al cambiar el responsable** en la vista completa, también cambia el responsable de los planos o de la corrección abierta.
- **Eliminar:** se confirma abajo y luego aparece un indicador de espera. Se borra todo, incluida la actividad.

## 6. Respaldo
- Botón "⤓ Respaldo" arriba, solo administración: descarga un Excel con las hojas Proyectos, Áreas, Actividad, "Datos para restaurar" (copia exacta para volver a cargar todo) e Info. No incluye el presupuesto privado.
- Pendiente: envío automático del respaldo por correo el día 1 de cada mes (requiere conectar Gmail u Outlook).

## 7. Pendientes (cuando Denisse lo indique)
- Lista fija de áreas comunes (ella la pasará).
- Automatización con Fireflies (conectada, pero no activar todavía).
- Reporte diario por correo.
- Integración con la cerradura de asistencia.
