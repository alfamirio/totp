# 🔑 Panel TOPT 24 Horas

Herramienta web (100% del lado del cliente, sin servidor) que genera y valida códigos de seguridad a partir de una frase secreta. A diferencia de un TOTP estándar (que cambia cada 30 segundos), aquí el código cambia **una vez al día**, lo que lo hace útil para escenarios donde necesitas un código memorable que se mantenga estable durante toda la jornada.

## Características

- **Dos modos de código**:
  - **TOTP**: código numérico de 6 dígitos, mostrado en 2 bloques de 3.
  - **Pass**: contraseña alfanumérica de 12 caracteres (mayúsculas, minúsculas y dígitos), mostrada en 3 bloques de 4.
- **Generador diario**: crea el código correspondiente a la fecha seleccionada.
- **Validador**: verifica si un código introducido corresponde a la clave y fecha indicadas.
- **Dos botones de copia**:
  - 🟠 Copia el código **con espacios** entre bloques (más legible).
  - 🟣 Copia el código **sin espacios** (listo para pegar directamente).
- **Historial de claves**: guarda hasta 5 claves secretas usadas recientemente, accesibles desde un desplegable con búsqueda (Select2).
- **Exportación anual**: genera una tabla con todos los códigos del año seleccionado, con vista previa paginada, búsqueda y exportación a **CSV**, **Excel** y **PDF**.
- Funciona completamente en el navegador: no se envía ninguna clave a un servidor externo.

## Cómo funciona

1. La **clave secreta** que escribes (cualquier palabra o frase en ASCII) se procesa con SHA-256 para obtener un valor de longitud fija.
2. **Modo TOTP**: el valor se codifica en Base32 y se genera un código de 6 dígitos usando el número de día como contador (variante HOTP con contador diario).
3. **Modo Pass**: el valor hash se combina con el número de día para derivar de forma determinista una contraseña alfanumérica de 12 caracteres. La misma clave y fecha siempre producen el mismo resultado.

> **Nota técnica:** el modo TOTP no es un TOTP estándar (RFC 6238), sino una variante de HOTP con contador diario. No es compatible con apps de autenticación como Google Authenticator.

## ⚠️ Aviso de seguridad y responsabilidad

**Este proyecto es una herramienta experimental/educativa y NO debe usarse para proteger nada serio o sensible** (cuentas, sistemas de producción, datos personales, acceso financiero, infraestructura crítica, etc.). No está diseñado ni auditado como un mecanismo de autenticación de nivel productivo.

El código se proporciona **"tal cual" (as-is), sin garantía de ningún tipo**, explícita o implícita. El autor no se hace responsable de pérdidas de acceso, brechas de seguridad, filtraciones de datos, daños directos o indirectos, ni de ninguna otra consecuencia derivada del uso, mal uso o fallo de este código. Su uso es bajo el propio riesgo y responsabilidad de quien lo implemente.

## Requisitos

- Un navegador web moderno con conexión a internet (el panel carga Bootstrap, DataTables, Select2 y otplib desde un CDN).
- No requiere instalación, backend ni dependencias adicionales.

## Cómo usar

### Barra de navegación

Todos los controles globales están en la barra superior:

- **Clave Secreta**: desplegable con historial de hasta 5 claves. Puedes escribir una nueva directamente para crearla.
- **Fecha**: selector de fecha usado por el generador y el validador.
- **Método**: elige entre `TOTP (6 dígitos)` y `Pass (12 alfanuméricos)`.
- **Hoy / Fecha** y **Año Completo**: navegación entre las dos páginas. El logo 🔑 también navega a la página principal.

### 1. Generar un código

1. Abre `index.html` en tu navegador.
2. En la barra superior, introduce tu **Clave Secreta**, selecciona la **Fecha** y el **Método**.
3. El código aparecerá automáticamente en el panel **Generador**.
4. Usa el botón 🟠 para copiar el código con espacios, o el botón 🟣 para copiarlo sin espacios.

### 2. Validar un código

1. En el panel **Validador**, asegúrate de tener la misma clave y fecha configuradas en la barra superior.
2. Escribe el código a verificar en el campo de entrada.
3. El sistema indicará en tiempo real si el código es **válido** o **incorrecto**.

> La clave secreta y la fecha se sincronizan automáticamente entre el generador y el validador.

### 3. Generar códigos para todo un año

1. Ve a la pestaña **Año Completo**.
2. Selecciona el año deseado (2020–2100).
3. Haz clic en **Generar CSV** para ver la tabla con todos los códigos del año.
4. Exporta el resultado usando los botones **CSV**, **Excel** o **PDF**.

## Consideraciones de seguridad

- La clave secreta se guarda en el `localStorage` del navegador **en texto plano**. No la uses en equipos compartidos o públicos.
- La derivación de clave usa un único SHA-256 (sin factor de costo como PBKDF2 o Argon2), por lo que frases cortas o comunes pueden ser vulnerables a ataques de fuerza bruta o diccionario.
- Esta herramienta está pensada **únicamente** para usos personales, lúdicos o de muy baja sensibilidad. **Nunca** como sustituto de un sistema de autenticación de dos factores real ni para proteger nada que importe de verdad.

## Estructura del proyecto

```
.
├── index.html   # Aplicación completa (HTML + CSS + JS)
└── README.md    # Este archivo
```
