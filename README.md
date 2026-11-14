# Dumatech +mail

Correo, calendario, contactos, archivos .PST, asistente de IA y WhatsApp en una sola aplicación de escritorio.

## Descargar

Descarga la última versión en **[Releases](https://github.com/viDDiv72/dumatech-mail/releases/latest)**:

| Sistema | Archivo |
|---|---|
| Windows 10/11 | `Dumatech-mail-Setup-x.y.z.exe` |
| macOS (Apple Silicon / Intel) | `Dumatech-mail-x.y.z-arm64.dmg` / `-x64.dmg` |
| Linux | `Dumatech-mail-x.y.z-x86_64.AppImage` o `.deb` |

En Windows, la aplicación se actualiza sola. Busca versiones nuevas, las descarga en segundo plano y las instala al cerrar. También puedes actualizar desde **Configuración → Acerca de → Reiniciar y actualizar**.

> El instalador aún no está firmado digitalmente. Windows SmartScreen puede mostrar «Windows protegió su PC»: pulsa **Más información → Ejecutar de todas formas**.

## Cuentas compatibles

- **Microsoft 365**: Empresa Básico, Estándar y Premium, E1/E3/E5. Se conecta con «Iniciar sesión con Microsoft».
- **Outlook.com, Hotmail, Live**: también con «Iniciar sesión con Microsoft».
- **Gmail y Google Workspace**: «Iniciar sesión con Google» o contraseña de aplicación.
- **Yahoo, iCloud y cualquier servidor IMAP/SMTP**.

El proveedor se detecta automáticamente al escribir el correo, también con dominios propios.

## Funciones

- **Correo**: Prioritarios/Otros, conversaciones, búsqueda avanzada, reglas, deshacer envío, firmas y adjuntos.
- **Calendario**: día, semana y mes; eventos periódicos; invitaciones; importar y exportar .ics.
- **Contactos**: importar y exportar .vcf y .csv.
- **Archivos de datos de Outlook (.pst)**: importación de correo, contactos y calendario. Exportación a MBOX/EML.
- **Asistente de IA con tu propia cuenta** (ChatGPT, Claude o DeepSeek): buscar, resumir, redactar, corregir, traducir y crear reglas.
- **WhatsApp integrado**: se vincula con un código QR.

## Novedades

### 1.1.1
- **«Iniciar sesión con Microsoft» listo de serie**: ya no hay que configurar nada para conectar cuentas de Microsoft 365, Outlook.com o Hotmail.

### 1.1.0
- **Iniciar sesión con Microsoft**: Microsoft 365 (incluido Empresa Básico, sin aplicaciones de escritorio) y Outlook.com/Hotmail.
- **Iniciar sesión con Google** para Gmail y Google Workspace.
- **Detección automática del proveedor** por el dominio del correo.
- **Envío por Microsoft Graph**: funciona aunque la empresa tenga desactivado SMTP.
- **Mensajes de error con la solución concreta** (IMAP desactivado, aprobación del administrador, sesión caducada…).
- **Actualizaciones automáticas**.
- **Preparada para macOS y Linux**.
- **WhatsApp**: corregido el arranque y la nueva página de inicio de sesión.

### 1.0.0
- Primera versión.
