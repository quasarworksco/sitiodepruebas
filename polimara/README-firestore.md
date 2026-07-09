# POLIMARA · Base de datos (Firestore)

La aplicación usa **Cloud Firestore** (proyecto `polimaradvt`) como base de datos
principal, con **respaldo local** en el navegador para no perder ningún dato.

## Cómo funciona el respaldo (sin pérdida de datos)

1. **Cada cambio** (registro de entrada, pago, entrega) se guarda de inmediato en:
   - el **respaldo local** del dispositivo (localStorage), y
   - **Firestore** en la nube.
2. Firestore tiene **persistencia offline** activada: si no hay internet, las
   escrituras se **encolan** y se sincronizan solas al reconectar. La cola
   sobrevive a recargas del navegador.
3. En cada actualización proveniente de la nube, se vuelve a **espejar** al
   respaldo local. Un indicador en la cabecera muestra el estado
   (*Sincronizado* / *Sin conexión · guardado local*).
4. **Ningún registro puede eliminarse** (las reglas prohíben el borrado); las
   correcciones se hacen por actualización y quedan asentadas en las novedades.

El registro se organiza por **día** (`dia`) y los **cortes diarios** resumen el
movimiento de cada jornada.

## Puesta en marcha (una sola vez)

1. En [Firebase Console](https://console.firebase.google.com/) del proyecto
   `polimaradvt`: **Firestore Database → Crear base de datos** (modo producción).
2. **Firestore → Reglas**: pega el contenido de [`../firestore.rules`](../firestore.rules)
   y pulsa **Publicar**. (O con CLI: `firebase deploy --only firestore:rules`.)
3. Listo: la página `polimara/` empezará a leer y escribir en Firestore.

## Recomendado para la comandancia (seguridad)

Las reglas actuales permiten leer/crear/actualizar sin inicio de sesión (pero no
borrar). Para restringir el acceso solo al personal autorizado, conviene
**habilitar Firebase Authentication** y usar la versión de reglas con
`request.auth != null` incluida al final de `firestore.rules`. Se puede añadir
una pantalla de inicio de sesión cuando se indique.
