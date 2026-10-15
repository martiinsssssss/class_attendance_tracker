# Control de faltas de clase

Una web estática y de un solo archivo para llevar la cuenta de las horas que faltas a cada seminario, sin pasarte del límite permitido.

**Versión publicada:** https://martiinsssssss.github.io/class_attendance_tracker/

Pensada para compartir con toda la clase: cada persona la abre en su propio navegador y apunta sus propias faltas. No hay servidor ni base de datos: cada quien ve únicamente sus propios datos.

## Cómo se usa

1. Abre la web (en el móvil o el ordenador).
2. En "Apuntar una falta", elige la fecha, el seminario, las horas (puedes escribir `1,5` o `1.5`, o usar los botones rápidos de 1, 2, 3 o 4 h) y, si quieres, un motivo.
3. Antes de guardar verás una vista previa que te dice cuántas horas llevarías y cuántas te quedarían (o si te pasarías del límite).
4. Pulsa "Guardar falta". El seminario correspondiente actualizará su barra de progreso al momento.
5. En "Historial de faltas" puedes filtrar por seminario, editar o borrar cualquier falta (al borrar tienes unos segundos para pulsar "Deshacer").
6. En "Gestión de datos" puedes exportar una copia en JSON o en CSV (para abrir en Excel), importar una copia JSON anterior, o borrar todo.

### El seminario "Fonaments de la professió infermera"

Este seminario tiene 6 horas teoricopràctiques de asistencia obligatoria: no cuentan dentro del 20 % de margen y no se puede faltar a ellas. Si registras una falta de este tipo, se marca aparte, en rojo, con un aviso de que era obligatoria.

## Tus datos son solo tuyos

Todo se guarda en el `localStorage` de tu propio navegador: nadie más puede ver tus faltas, y en este repositorio no se almacena ningún dato personal de nadie.

Como las faltas solo viven en tu navegador:

- Si cambias de navegador o de dispositivo, no verás las faltas que apuntaste antes.
- Si borras los datos de navegación, perderás el historial.
- En iPhone, Safari puede borrar los datos de una web si pasas muchos días sin abrirla. Para evitarlo, añade esta web a la pantalla de inicio (botón Compartir → "Añadir a pantalla de inicio") y ábrela desde ese icono.

Por eso conviene hacer copias de seguridad de vez en cuando:

- **Exportar JSON**: guarda una copia completa que luego puedes volver a importar (combina por id, sin duplicar).
- **Exportar CSV**: pensado para abrir directamente en Excel en español (separador `;`, decimales con coma).
- La web te avisa si han pasado más de 14 días desde tu última copia exportada.

## Cómo cambiar los seminarios o el porcentaje

Toda la configuración está al principio de la etiqueta `<script>` dentro de `index.html`, en el objeto `CONFIG`:

```js
const CONFIG = {
  generalLimitPct: 0.20, // % máximo de horas al que se puede faltar
  modules: { ... },      // nombres de los módulos
  seminars: [             // un objeto por seminario
    { id: "anatomia", name: "Anatomia i fisiologia humana", module: "M2", hoursTotal: 63 },
    // ...
  ]
};
```

- Para cambiar el límite general, edita `generalLimitPct` (por ejemplo, `0.25` para un 25 %).
- Para añadir, quitar o editar un seminario, modifica la lista `seminars`. Cada uno necesita `id` (único), `name`, `module` (`M1`, `M2` o `M3`) y `hoursTotal`.
- Si un seminario tiene horas de asistencia obligatoria que no deben contar en el 20 % (como Fonaments), añade `obligatoryHours` con ese número de horas.
- El máximo de horas a las que se puede faltar se calcula solo, como `(horas totales − horas obligatorias) × límite general`, redondeado a 2 decimales.

No hace falta tocar nada más: el formulario, las barras de progreso y el resumen se generan automáticamente a partir de esta configuración.

## Publicar los cambios (GitHub Pages)

Si todavía no está activado:

1. En el repositorio de GitHub, ve a **Settings → Pages**.
2. En "Build and deployment", elige **Deploy from a branch**.
3. Selecciona la rama `main` y la carpeta `/ (root)`.
4. Guarda. En uno o dos minutos la web estará disponible en `https://martiinsssssss.github.io/class_attendance_tracker/`.

## Tecnología

Un único archivo `index.html` con HTML, CSS y JavaScript vanilla. Sin frameworks, sin dependencias, sin paso de build y sin backend.
