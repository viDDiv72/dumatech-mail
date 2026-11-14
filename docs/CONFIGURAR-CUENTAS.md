# Configurar cuentas en Dumatech +mail

## Qué cuentas funcionan y cómo

| Cuenta | Cómo se conecta | Qué necesita el usuario |
|---|---|---|
| **Microsoft 365 Empresa Básico** (sin aplicaciones de escritorio) | Iniciar sesión con Microsoft | Solo su correo y contraseña de Microsoft (y su 2FA, si lo tiene) |
| **Microsoft 365 Empresa Estándar / Premium**, E1/E3/E5 | Iniciar sesión con Microsoft | Igual |
| **Outlook.com, Hotmail, Live, MSN** | Iniciar sesión con Microsoft | Igual |
| **Gmail** | Iniciar sesión con Google, o contraseña de aplicación | Verificación en 2 pasos activada si usa contraseña de aplicación |
| **Google Workspace** (Gmail de empresa) | Iniciar sesión con Google, o contraseña de aplicación | Que el administrador permita IMAP |
| Yahoo, iCloud | Contraseña de aplicación | Generarla en la web del proveedor |
| Cualquier otro (IONOS, Zoho, cPanel, Exchange propio…) | IMAP/SMTP con contraseña | Datos del servidor (se detectan solos casi siempre) |

Microsoft 365 Empresa Básico **no trae Outlook de escritorio**, pero sí incluye el buzón de Exchange Online. Dumatech +mail se conecta a ese buzón igual que con Estándar o Premium. La licencia no cambia nada.

Al escribir el correo, Dumatech +mail mira los registros del dominio. Así detecta solo si `tu@tuempresa.com` está en Microsoft 365 o en Google Workspace, y elige el método correcto.

### Por qué Microsoft necesita «Iniciar sesión con Microsoft»

Microsoft desactivó la autenticación con contraseña (IMAP/SMTP «básico»):

- en Microsoft 365 en 2022–2023;
- en Outlook.com/Hotmail en septiembre de 2024.

Ningún programa de correo (Thunderbird, Apple Mail, eM Client…) puede ya entrar con la contraseña. Todos usan OAuth 2.0, igual que Dumatech +mail.

Para usar OAuth, Microsoft y Google exigen que la aplicación esté **registrada**. Se registra **una sola vez para toda la empresa** y es gratis. Después, ningún usuario tiene que hacer nada especial.

---

## 1. Registrar Dumatech +mail en Microsoft (≈ 5 minutos)

Hace falta una cuenta con acceso a <https://portal.azure.com> (o <https://entra.microsoft.com>). Sirve cualquier cuenta de Microsoft 365 de la empresa con permiso para registrar aplicaciones; por defecto lo tienen todos los usuarios.

1. Ve a **Microsoft Entra ID → Registros de aplicaciones → Nuevo registro**.
2. Rellena el registro:
   - **Nombre**: `Dumatech +mail`
   - **Tipos de cuenta compatibles**: *Cuentas de cualquier directorio organizativo y cuentas Microsoft personales*. Esto cubre Microsoft 365 de cualquier empresa y también Outlook.com/Hotmail.
   - **URI de redirección**: elige la plataforma *Cliente público/nativo (móvil y escritorio)* y escribe `http://localhost`.
   - Pulsa **Registrar**.
3. Copia el **Id. de aplicación (cliente)**, que tiene la forma `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`.
4. En **Autenticación**, pon **Permitir flujos de clientes públicos: Sí** y guarda.
5. En **Permisos de API → Agregar un permiso → Microsoft Graph → Permisos delegados**, añade:
   - `offline_access`, `openid`, `profile`, `email`
   - `IMAP.AccessAsUser.All` (para leer el correo)
   - `SMTP.Send` (para enviar por SMTP)
   - `Mail.Send` (para enviar mediante Microsoft Graph, que funciona aunque la empresa tenga SMTP desactivado)
6. Opcional, para tu propia organización: pulsa **Conceder consentimiento de administrador para…**. Así tus usuarios no verán la pantalla de permisos.

No hace falta crear ningún «secreto»: es una aplicación de escritorio (cliente público).

## 2. Registrar Dumatech +mail en Google (opcional, ≈ 10 minutos)

Sin este paso, Gmail funciona igualmente con una **contraseña de aplicación**: <https://myaccount.google.com/apppasswords>, que requiere la verificación en 2 pasos.

1. En <https://console.cloud.google.com>, crea un proyecto llamado `Dumatech +mail`.
2. En **API y servicios → Biblioteca**, habilita **Gmail API**.
3. Configura la **Pantalla de consentimiento de OAuth**:
   - **Tipo de usuario**:
     - *Interno* si solo la usarán cuentas de tu Google Workspace. No requiere revisión de Google.
     - *Externo* para cualquier cuenta de Gmail.
   - **Ámbitos**: añade `https://mail.google.com/`.
   - En modo *Externo* y estado *Prueba*, añade como «usuarios de prueba» las cuentas que vayan a usarlo. En ese modo Google caduca la sesión cada 7 días. Para uso general hay que pedir la verificación de la aplicación a Google.
4. En **Credenciales → Crear credenciales → ID de cliente de OAuth**, elige el tipo **Aplicación de escritorio**.
5. Copia el **ID de cliente** y el **Secreto del cliente**. En las aplicaciones de escritorio este «secreto» no es confidencial.

## 3. Poner los ID en Dumatech +mail

Hay dos formas:

- **En la aplicación**: Configuración → Cuentas → **Configurar inicio de sesión**. Pega los ID y guarda. Se guardan cifrados en ese equipo.
- **Integrados en el instalador**, para que ningún usuario tenga que configurarlos: rellena `server/oauth-apps.json` y genera el instalador.

  ```json
  {
    "microsoft": { "clientId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" },
    "google": { "clientId": "….apps.googleusercontent.com", "clientSecret": "GOCSPX-…" }
  }
  ```

  Después ejecuta `npm run dist` (Windows), `npm run dist:mac` o `npm run dist:linux`.

## 4. Si algo falla con Microsoft 365

| Mensaje | Solución |
|---|---|
| «Tu organización exige que un administrador apruebe…» | Un administrador global abre el enlace **Aprobación del administrador de Microsoft 365** (Configuración → Cuentas → Configurar inicio de sesión) y acepta. Se hace una vez por empresa. |
| «Microsoft rechazó el acceso al buzón» | El administrador activa IMAP para ese buzón: Centro de administración de Microsoft 365 → Usuarios activos → usuario → **Correo → Administrar aplicaciones de correo → IMAP**. Por defecto está activado. |
| El envío falla | Dumatech +mail envía por Microsoft Graph, que no depende de SMTP. Si aun así falla, activa **SMTP autenticado** en el mismo panel. |
| «La sesión ha caducado» | Configuración → Cuentas → Administrar → **Volver a iniciar sesión**. Ocurre si cambias la contraseña o si el administrador revoca las sesiones. |

## 5. Plataformas

Dumatech +mail funciona en **Windows 10/11**, **macOS 12 o posterior** (Intel y Apple Silicon) y **Linux** (Ubuntu, Debian, Fedora…; AppImage o .deb).

- Instaladores:
  - Windows: `npm run dist`
  - macOS: `npm run dist:mac` (ejecutar en un Mac)
  - Linux: `npm run dist:linux` (ejecutar en Linux)
- La forma más cómoda de generar los tres a la vez es GitHub Actions: `.github/workflows/build.yml`.
- WhatsApp necesita Google Chrome, Microsoft Edge o Chromium instalado en el equipo.
