[README.md](https://github.com/user-attachments/files/34052383/README.md)

# Agente de noticias XAUUSD para Telegram

Publica en tu grupo de Telegram dos resúmenes al día (lunes a viernes) sobre el oro:

- ☀️ **Mañana (7:00)**: noticias de la noche + calendario USD de alto impacto de **hoy**
- 🌆 **Tarde (16:00)**: noticias del día + calendario USD de alto impacto de **mañana**

Cada resumen incluye titulares con enlace, un resumen en español de cada noticia hecho con IA y un panorama general. Corre gratis en GitHub Actions, sin dejar tu computadora encendida.

---

## Paso 1 — Crear el bot de Telegram

1. En Telegram abre **@BotFather** y envía `/newbot`.
2. Elige un nombre (ej. `Noticias Oro`) y un usuario que termine en `bot` (ej. `noticias_oro_xau_bot`).
3. BotFather te da un **token** (algo como `123456:ABC-xyz...`). Guárdalo en un lugar seguro y **no lo compartas con nadie**.

## Paso 2 — Agregar el bot a tu grupo y obtener el ID del grupo

1. Agrega el bot a tu grupo como miembro. Si el grupo restringe quién puede escribir, hazlo **administrador**.
2. En el grupo envía este comando (cambia el nombre por el de tu bot): `/start@noticias_oro_xau_bot`
3. En tu navegador abre esta dirección, poniendo tu token:
   `https://api.telegram.org/botTU_TOKEN/getUpdates`
4. Busca `"chat":{"id":-100...`. Ese número **con el signo menos** es tu **ID del grupo**.

> Si la página sale vacía (`"result":[]`), envía otra vez el comando en el grupo y recarga.

## Paso 3 — Clave de IA (opcional, para los resúmenes en español)

1. Entra a **console.anthropic.com**, crea una cuenta y carga un poco de saldo.
2. En *API Keys* crea una clave y guárdala.

Con dos resúmenes al día el costo es de centavos al mes. Si no pones esta clave, el bot funciona igual pero publica solo los titulares con enlace.

## Paso 4 — Subir el proyecto a GitHub

1. Crea una cuenta en **github.com** (si no tienes) y crea un repositorio nuevo, por ejemplo `xauusd-bot`. Puede ser **privado**.
2. Pulsa **Add file → Upload files** y sube `bot.py`, `requirements.txt` y `README.md`.
3. Pulsa **Add file → Create new file**, escribe como nombre exactamente
   `.github/workflows/xauusd.yml`
   pega el contenido del archivo `xauusd.yml` y guarda con **Commit changes**.

## Paso 5 — Guardar los secretos

En tu repositorio: **Settings → Secrets and variables → Actions → New repository secret**. Crea estos:

| Nombre | Valor |
|---|---|
| `TELEGRAM_BOT_TOKEN` | el token de BotFather |
| `TELEGRAM_CHAT_ID` | el ID del grupo (con el `-`) |
| `ANTHROPIC_API_KEY` | tu clave de IA (opcional) |

Opcional, en la pestaña **Variables** de la misma página:

| Nombre | Para qué | Por defecto |
|---|---|---|
| `TIMEZONE` | zona horaria del horario y del calendario | `America/Toronto` |
| `MORNING_HOUR` | hora del resumen de la mañana | `7` |
| `EVENING_HOUR` | hora del resumen de la tarde | `16` |

> Si cambias las horas, ajusta también los `cron` del archivo `xauusd.yml` (están en hora UTC).

## Paso 6 — Probarlo

1. Ve a la pestaña **Actions** de tu repositorio.
2. Elige **Noticias XAUUSD a Telegram → Run workflow**, selecciona `am` y pulsa el botón verde.
3. En un minuto debería llegar el mensaje a tu grupo. 🎉

Desde ahí corre solo, de lunes a viernes.

---

## Problemas comunes

- **`chat not found`**: el ID del grupo está mal o el bot no está en el grupo.
- **`Unauthorized`**: el token está mal copiado.
- **Falla el paso "Guardar estado"**: en *Settings → Actions → General → Workflow permissions* elige **Read and write permissions**.
- **Llega un poco tarde**: GitHub a veces retrasa las tareas programadas unos minutos; es normal.

## Cómo funciona por dentro

- Lee noticias de FXStreet (inglés y español) y Google News, y se queda con las que hablan del oro.
- La IA descarta las irrelevantes o repetidas y escribe los resúmenes, sin dar señales de compra o venta.
- El calendario viene del feed semanal de Forex Factory (solo eventos USD de impacto alto).
- `state.json` recuerda lo que ya se publicó para no repetir noticias.
