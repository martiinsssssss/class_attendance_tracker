# Control de faltas de clase

Una web estática y de un solo archivo para llevar la cuenta de las horas que faltas a cada seminario, sin pasarte del límite permitido.

**Versión publicada:** https://martiinsssssss.github.io/class_attendance_tracker/

Pensada para compartir con toda la clase: cada persona la abre en su propio navegador y apunta sus propias faltas. No hay servidor ni base de datos: cada quien ve únicamente sus propios datos.

## Cómo se usa

1. Abre la web (en el móvil o el ordenador).
2. En **Seminarios** verás un anillo por seminario. El número del centro son las horas que aún puedes faltar; el anillo se va cerrando a medida que faltas y cambia de color según cómo vas:
   - morado: vas bien,
   - amarillo: cerca del límite (has gastado el 60 % del margen o más),
   - naranja: sin margen (justo en el límite),
   - rojo: límite superado (el centro dice cuántas horas te has pasado).
3. Pulsa el botón **+ Apuntar falta**. Elige cuándo (hoy, ayer u otro día), el seminario, las horas (1, 2, 3, 4 o escribe otra cantidad como `1,5`) y, si quieres, un motivo. Antes de guardar verás cómo te quedaría el seminario.
4. Toca un seminario para ver solo sus faltas, o ve a **Historial** para verlas todas. Toca cualquier falta para editarla o borrarla (al borrar tienes unos segundos para pulsar "Deshacer").
5. Los botones de arriba (M1, M2, M3) filtran los seminarios por módulo.

### El seminario "Fonaments de la professió infermera"

Este seminario tiene 6 horas teoricopràctiques de asistencia obligatoria: no cuentan dentro del 20 % de margen y no se puede faltar a ellas. Al apuntar una falta de Fonaments puedes elegir "Clase normal" o "Teoricopràctica". Las faltas a teoricopràctiques se guardan aparte, no gastan el margen y se marcan siempre en rojo.

## Tus datos son solo tuyos

Todo se guarda en el `localStorage` de tu propio navegador: nadie más puede ver tus faltas, y en este repositorio no se almacena ningún dato personal de nadie.

Como las faltas solo viven en tu navegador:

- Si cambias de navegador o de dispositivo, no verás las faltas que apuntaste antes.
- Si borras los datos de navegación, perderás el historial.
- En iPhone, Safari puede borrar los datos de una web si pasas muchos días sin abrirla. Para evitarlo, añade esta web a la pantalla de inicio (botón Compartir → "Añadir a pantalla de inicio") y ábrela desde ese icono.

Por eso conviene hacer copias de seguridad de vez en cuando, desde **Historial → Copia de seguridad**:

- **Descargar copia**: guarda un archivo `.json` con todas tus faltas.
- **Restaurar copia**: vuelve a cargar ese archivo (combina por id, sin duplicar).
- **Descargar para Excel (CSV)**: pensado para abrir directamente en Excel en español (separador `;`, decimales con coma).
- La web te avisa si tienes faltas y han pasado más de 14 días desde tu última copia (o nunca has hecho una).

## Cómo cambiar los seminarios o el porcentaje

Toda la configuración está al principio de la etiqueta `<script>` dentro de `index.html`, en el objeto `CONFIG`:

```js
const CONFIG = {
  generalLimitPct: 0.20,                 // % máximo de horas al que se puede faltar
  modules: {                             // etiqueta corta y nombre de cada módulo
    M1: { tag: "M1", name: "La professió infermera" },
    // ...
  },
  seminars: [                            // un objeto por seminario
    { id: "anatomia", name: "Anatomia i fisiologia humana", module: "M2", hoursTotal: 63 },
    // ...
  ],
  moduleNotes: { M3: "..." },            // nota opcional debajo de un módulo
  reasons: ["Enfermedad", "Cita médica", ...]  // motivos rápidos del formulario
};
```

- Para cambiar el límite general, edita `generalLimitPct` (por ejemplo, `0.25` para un 25 %).
- Para añadir, quitar o editar un seminario, modifica la lista `seminars`. Cada uno necesita `id` (único), `name`, `module` (`M1`, `M2` o `M3`) y `hoursTotal`. **No cambies el `id` de un seminario que ya existe**: las faltas guardadas se enlazan con él.
- Si un seminario tiene horas de asistencia obligatoria que no deben contar en el 20 % (como Fonaments), añade `obligatoryHours` con ese número de horas.
- El máximo de horas a las que se puede faltar se calcula solo, como `(horas totales − horas obligatorias) × límite general`, redondeado a 2 decimales.

No hace falta tocar nada más: los anillos, el resumen y el formulario se generan automáticamente a partir de esta configuración.

## Publicar los cambios (GitHub Pages)

Si todavía no está activado:

1. En el repositorio de GitHub, ve a **Settings → Pages**.
2. En "Build and deployment", elige **Deploy from a branch**.
3. Selecciona la rama `main` y la carpeta `/ (root)`.
4. Guarda. En uno o dos minutos la web estará disponible en `https://martiinsssssss.github.io/class_attendance_tracker/`.

## Tecnología

Un único archivo `index.html` con HTML, CSS y JavaScript vanilla. Sin frameworks, sin paso de build y sin backend. La única petición externa es la fuente Nunito de Google Fonts; si no carga, se usa la fuente del sistema. Se adapta solo al modo claro u oscuro del dispositivo.
