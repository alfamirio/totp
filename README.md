# 🔑 Panel TOTP de 24 Horas

Herramienta web (100% del lado del cliente, sin servidor) que genera y valida códigos de seguridad de 6 dígitos a partir de una frase secreta. A diferencia de un TOTP estándar (que cambia cada 30 segundos), aquí el código cambia **una vez al día, a las 00:00 UTC**, lo que lo hace útil para escenarios donde necesitas un código memorable que se mantenga estable durante toda la jornada.

## Características

- **Generador diario**: crea un código de 6 dígitos válido para la fecha seleccionada.
- **Validador**: verifica si un código introducido corresponde a la clave y fecha indicadas.
- **Exportación anual**: genera y descarga un CSV con los 365/366 códigos de un año completo, con vista previa paginada y búsqueda.
- Funciona completamente en el navegador: no se envía ninguna clave a un servidor externo.

## Cómo funciona

1. La **clave secreta** que escribes (cualquier palabra o frase en ASCII) se procesa con SHA-256 para obtener un valor de longitud fija.
2. Ese valor se codifica en Base32, formato requerido por el algoritmo HOTP/TOTP.
3. Se genera un código de 6 dígitos usando el número de día (no la hora) como contador, de modo que el código se mantiene igual durante las 24 horas de ese día calendario.

> **Nota técnica:** este esquema no es un TOTP estándar (RFC 6238), sino una variante de HOTP con contador diario. Si necesitas compatibilidad con apps de autenticación como Google Authenticator, este sistema no es intercambiable con ellas.

## ⚠️ Aviso de seguridad y responsabilidad

**Este proyecto es una herramienta experimental/educativa y NO debe usarse para proteger nada serio o sensible** (cuentas, sistemas de producción, datos personales, acceso financiero, infraestructura crítica, etc.). No está diseñado ni auditado como un mecanismo de autenticación de nivel productivo.

El código se proporciona **"tal cual" (as-is), sin garantía de ningún tipo**, explícita o implícita. El autor no se hace responsable de pérdidas de acceso, brechas de seguridad, filtraciones de datos, daños directos o indirectos, ni de ninguna otra consecuencia derivada del uso, mal uso o fallo de este código. Su uso es bajo el propio riesgo y responsabilidad de quien lo implemente.

## Requisitos

- Un navegador web moderno con conexión a internet (el panel carga Bootstrap, DataTables y otplib desde un CDN).
- No requiere instalación, backend ni dependencias adicionales.

## Cómo usar

### 1. Generar un código para hoy (o cualquier fecha)

1. Abre `index.html` en tu navegador.
2. En la pestaña **Hoy / Fecha**, panel **Generador TOTP**:
   - Escribe tu frase en **Clave Secreta (ASCII)**.
   - Selecciona la fecha deseada (por defecto, la de hoy).
3. El código de 6 dígitos aparecerá dividido en dos bloques de 3.

### 2. Validar un código

1. En el panel **Validador TOTP**, ingresa la misma clave secreta usada para generar el código.
2. Selecciona la fecha correspondiente.
3. Escribe el código de 6 dígitos a verificar.
4. El sistema indicará si el código es **válido** o **incorrecto** en tiempo real.

> La clave secreta se sincroniza automáticamente entre el generador y el validador, y se guarda localmente en tu navegador para que no tengas que volver a escribirla.

### 3. Generar códigos para todo un año (CSV)

1. Ve a la pestaña **Año Completo**.
2. Escribe tu clave secreta y el año deseado (2020–2100).
3. Haz clic en **Generar CSV** para ver la vista previa de todos los códigos del año.
4. Haz clic en **Descargar CSV Completo** para guardar el archivo `TOPT_<año>.csv` con las columnas `Fecha` y `Codigo_TOPT`.

## Consideraciones de seguridad

- La clave secreta se guarda en el `localStorage` del navegador **en texto plano**. No la uses en equipos compartidos o públicos.
- Como la derivación de la clave usa un único SHA-256 (sin factor de costo como PBKDF2 o Argon2), frases secretas cortas o comunes pueden ser vulnerables a ataques de fuerza bruta o diccionario.
- Esta herramienta está pensada **únicamente** para usos personales, lúdicos o de muy baja sensibilidad (por ejemplo, un recordatorio diario o un juego), **nunca** como sustituto de un sistema de autenticación de dos factores real ni para proteger nada que importe de verdad.
- Recuerda: úsala bajo tu propia responsabilidad. Ver el aviso completo al inicio de este documento.

## Estructura del proyecto

```
.
├── index.html   # Aplicación completa (HTML + CSS + JS)
└── README.md    # Este archivo
```
