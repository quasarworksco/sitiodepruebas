# POLIMARA · Base de datos (Firestore) e inicio de sesión

La aplicación usa **Cloud Firestore** (proyecto `polimaradvt`) como base de datos,
con **inicio de sesión** (Firebase Authentication) y **respaldo local** para no
perder ningún dato.

## Puesta en marcha (una sola vez, en la consola de Firebase)

1. **Firestore Database → Crear base de datos** (modo producción, ubicación más
   cercana).
2. **Firestore → Reglas**: pega el contenido de [`../firestore.rules`](../firestore.rules)
   y pulsa **Publicar**.  (O con CLI: `firebase deploy --only firestore:rules`.)
3. **Authentication → Sign-in method →** habilita **Correo/Contraseña**.
4. Crea la **primera cuenta**: desde la app, pestaña **“Crear cuenta”**, usando el
   **código de registro**; o en **Authentication → Users → Add user**.

> Sin estos pasos, la app mostrará *“Reconectando…”* y trabajará solo con el
> respaldo local hasta que Firestore y el inicio de sesión estén configurados.

## Inicio de sesión

- Pantalla de acceso con **iniciar sesión** y **crear cuenta**.
- **Crear cuenta requiere un código de registro** (solo el personal autorizado),
  definido en `polimara/index.html`:

  ```js
  const REG_CODE = 'POLIMARA-2026';   // cámbielo por el que defina el comando
  ```

- El resto es correo + contraseña. La sesión queda recordada en el dispositivo.

## Respaldo sin pérdida de datos

1. **Cada cambio** se guarda de inmediato en el **respaldo local** (localStorage)
   y en **Firestore**.
2. Firestore tiene **persistencia offline**: si no hay internet, las escrituras se
   **encolan** y se sincronizan al reconectar; la cola sobrevive a recargas.
3. En cada actualización desde la nube se **espeja** al respaldo local.
4. Un indicador en la cabecera muestra el estado: **En línea** / **Reconectando…**
5. **Ningún registro puede eliminarse** (las reglas prohíben el borrado).

El registro se organiza por **día** (`dia`) y los **cortes diarios** y las
**métricas** resumen el movimiento de cada jornada.
