# Pixel Place — Firebase + GitHub Pages

Página estática con lienzo compartido en tiempo real. Usa Firebase Realtime Database; no requiere backend propio ni compilación.

## 1. Crear la base de datos

1. Abre https://console.firebase.google.com/ y crea un proyecto.
2. Añade una aplicación web desde la configuración del proyecto.
3. Copia la configuración del SDK web.
4. En **Build → Realtime Database**, crea una base de datos y copia la URL exacta que Firebase muestra.
5. En `index.html`, reemplaza todos los valores `PEGA_AQUI...` del objeto `firebaseConfig`.

## 2. Reglas sencillas para una demostración de clase

En Realtime Database → **Rules**, para una prueba temporal y controlada, se puede usar:

```json
{
  "rules": {
    "pixelPlace": {
      "board": {
        ".read": true,
        "$pixel": {
          ".write": true,
          ".validate": "newData.isString() && newData.val().matches(/^#[0-9a-fA-F]{6}$/)"
        }
      }
    }
  }
}
```

Publicar reglas abiertas permite que cualquier persona en Internet lea y modifique píxeles. El límite de 3 segundos implementado en JavaScript es solo del lado del navegador y se puede saltar; estas reglas **no** hacen cumplir el cooldown en el servidor. Usa esto solo para una demostración temporal con datos no sensibles, vigila el uso y cierra o cambia las reglas cuando termine la clase. Para imponer el límite de forma real hace falta autenticación y validación confiable en backend (por ejemplo, Cloud Functions).

## 3. Publicar en GitHub Pages

1. Sube `index.html`, `README.md` y `.github/workflows/deploy.yml` a la rama `main`.
2. En el repositorio, abre **Settings → Pages**.
3. En **Build and deployment → Source**, selecciona **GitHub Actions**.
4. En **Actions**, espera a que termine el flujo.
5. Comparte la URL pública de GitHub Pages.

## 4. Límite de 3 segundos

La interfaz impide pintar más de un píxel cada 3 segundos en ese navegador. Es una barrera de experiencia de usuario, no una protección segura contra bots, porque se ejecuta en el cliente. Para una actividad de clase básica suele servir si los participantes colaboran.

## Notas
- Firebase `apiKey` en una app web identifica el proyecto, no es una contraseña de administrador. La seguridad depende de las reglas de la base de datos.
- No incluyas nunca credenciales de cuentas de servicio o claves privadas en HTML.
- Si Firebase muestra errores de permisos, revisa las reglas y que `databaseURL` coincida exactamente con la URL de Realtime Database.
