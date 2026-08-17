# HTTP/S Protocol Fundamentals

## Protocolo HTTP

**HTTP** significa **Hypertext Transfer Protocol**. Es un protocolo de capa de aplicación utilizado para transferir recursos web, como páginas HTML, imágenes, archivos JavaScript, CSS, datos de APIs y contenido de aplicaciones web.

HTTP funciona sobre **TCP** y fue diseñado para la comunicación entre navegadores web y servidores web.

HTTP utiliza una arquitectura cliente-servidor:

- El **cliente** suele ser un navegador, una aplicación móvil, un script o una herramienta de línea de comandos.
- El **servidor** es el servidor web que recibe solicitudes y devuelve respuestas.

Los recursos se identifican de forma única mediante una **URL** o **URI**.

```text
Cliente / Navegador                         Servidor Web
       |                                        |
       | -------- Solicitud HTTP -------------> |
       |                                        |
       | <------- Respuesta HTTP -------------- |
       |                                        |
```

---

## Sitio web y servidor web

### ¿Qué es un sitio web?

Un sitio web es una colección de páginas web interconectadas y accesibles a través de Internet.

Puede contener:

- Texto.
- Imágenes.
- Vídeos.
- Formularios.
- Enlaces.
- Archivos descargables.
- Contenido interactivo.

### ¿Qué es un servidor web?

Un servidor web es software o hardware que recibe solicitudes HTTP o HTTPS y devuelve recursos web a los clientes.

Ejemplos de software de servidor web:

```text
Apache HTTP Server
Nginx
Microsoft IIS
```

### Tipos de servidores

| Tipo de servidor | Función |
|---|---|
| Servidor HTTP/Web | Gestiona solicitudes HTTP y sirve contenido estático como HTML, CSS, JavaScript, imágenes y archivos |
| Servidor de aplicaciones | Ejecuta aplicaciones, procesa datos, gestiona interacciones y genera contenido dinámico |
| Servidor de base de datos | Almacena y gestiona los datos utilizados por aplicaciones web, como usuarios, sesiones y pedidos |

### Off-Premise Hosting

El **off-premise hosting**, también llamado cloud hosting, consiste en alojar un sitio web o aplicación en infraestructura remota en lugar de usar servidores locales de la organización.

Ejemplos: plataformas cloud, VPS y proveedores de hosting gestionado.

---

## Versiones de HTTP

### HTTP/1.0

HTTP/1.0 es una versión inicial de HTTP.

Permite solicitar recursos a un servidor, pero generalmente necesita una nueva conexión TCP para cada solicitud, por lo que resulta menos eficiente para aplicaciones web modernas.

### HTTP/1.1

HTTP/1.1 mejora HTTP/1.0 al permitir conexiones persistentes.

```text
HTTP/1.0  = normalmente crea una conexión nueva por cada solicitud.
HTTP/1.1  = puede reutilizar la misma conexión para varias solicitudes.
```

HTTP/1.1 utiliza `Connection: keep-alive` para mantener una conexión TCP abierta y enviar varias solicitudes por ella.

### HTTP/2

HTTP/2 mejora el rendimiento al permitir enviar múltiples solicitudes y respuestas simultáneamente a través de una misma conexión.

Mejoras importantes:

- Multiplexing.
- Compresión de headers.
- Menor latencia.
- Carga de recursos más eficiente.

### HTTP/3

HTTP/3 está diseñado para mejorar aún más el rendimiento mediante el protocolo de transporte **QUIC**.

Busca reducir la latencia y acelerar el establecimiento de conexiones, especialmente en redes poco fiables.

---

## HTTP es stateless

HTTP es un protocolo **sin estado** (*stateless*).

Esto significa que cada solicitud es independiente por defecto. El servidor no recuerda automáticamente las solicitudes anteriores del mismo usuario.

Las aplicaciones web utilizan sesiones, cookies y tokens para mantener el estado del usuario.

```text
1. El usuario inicia sesión.
2. El servidor valida las credenciales.
3. El servidor crea una sesión o token.
4. El navegador guarda una cookie o token de sesión.
5. El navegador lo envía con las siguientes solicitudes.
6. El servidor reconoce al usuario autenticado.
```

---

## Estructura de mensajes HTTP

Durante la comunicación HTTP, el cliente y el servidor intercambian mensajes.

El cliente envía una **solicitud HTTP** y el servidor devuelve una **respuesta HTTP**.

```text
[ Navegador / Cliente ] ---- Solicitud HTTP ----> [ Servidor Web ]
[ Navegador / Cliente ] <--- Respuesta HTTP ----- [ Servidor Web ]
```

Los mensajes HTTP usan:

```text
\r     = Carriage Return; mueve el cursor al inicio de la línea.
\n     = Line Feed; mueve el cursor a la siguiente línea.
\r\n   = Carriage Return + Line Feed; marca el final de una línea.
```

Una línea vacía, representada por `\r\n\r\n`, separa los headers HTTP del cuerpo opcional del mensaje.

---

# Solicitudes HTTP

## Componentes de una solicitud HTTP

Una solicitud HTTP normalmente contiene:

1. Línea de solicitud.
2. Headers de solicitud.
3. Línea vacía.
4. Cuerpo de solicitud opcional.

### Estructura de una solicitud

```http
METHOD /path HTTP/version
Header-Name: Header-Value
Header-Name: Header-Value

Optional request body
```

---

## Request line

La request line es la primera línea de una solicitud HTTP.

Contiene:

- El método HTTP.
- La ruta URL solicitada.
- La versión HTTP.

### Ejemplo

```http
GET / HTTP/1.1
```

```text
GET       = método HTTP.
 /        = ruta solicitada; página raíz.
HTTP/1.1  = versión del protocolo HTTP.
```

---

## Ruta de la solicitud

La ruta indica el recurso al que el cliente quiere acceder.

```http
GET / HTTP/1.1
```

La ruta `/` representa la página principal o directorio raíz del sitio web.

Otros ejemplos:

```http
GET /login HTTP/1.1
GET /downloads/index.php HTTP/1.1
GET /images/logo.png HTTP/1.1
GET /api/users/10 HTTP/1.1
```

El header `Host` y la ruta se combinan para formar la URL completa.

```text
Host: example.com
Path: /login

URL completa: http://example.com/login
```

---

## Headers de solicitudes HTTP

Los headers proporcionan información adicional sobre la solicitud HTTP.

Su formato básico es:

```text
Header-Name: Header-Value
```

Headers comunes:

| Header | Función |
|---|---|
| Host | Especifica el hostname del servidor solicitado |
| User-Agent | Identifica el cliente, navegador, sistema operativo y a veces el idioma |
| Accept | Define los tipos de contenido que el cliente acepta |
| Accept-Encoding | Define formatos de compresión aceptados, como gzip o deflate |
| Authorization | Envía credenciales o tokens de autenticación |
| Cookie | Envía datos almacenados en el cliente, normalmente identificadores de sesión |
| Referer | Indica la página que enlazó al recurso solicitado |
| Origin | Indica el origen de la solicitud, importante para CORS |
| Content-Type | Especifica el formato de los datos enviados en el cuerpo |
| Content-Length | Especifica el tamaño del cuerpo de solicitud en bytes |
| Connection | Controla si la conexión permanece abierta o se cierra |

---

## Host Header

El header `Host` especifica a qué sitio web quiere acceder el cliente.

```http
Host: www.example.com
```

Este header permite que un único servidor web con una IP pueda alojar varios sitios web. Esto se conoce como **virtual hosting**.

```text
Dirección IP: 192.168.1.10

Host: website-a.com
Host: website-b.com
Host: website-c.com
```

El servidor usa el header `Host` para decidir qué configuración de sitio web o virtual host debe gestionar la solicitud.

---

## User-Agent Header

El header `User-Agent` identifica al cliente que realiza la solicitud.

```http
User-Agent: Mozilla/5.0 (X11; Linux x86_64) Firefox/120.0
```

Puede revelar información como:

- Tipo de navegador.
- Versión del navegador.
- Sistema operativo.
- Tipo de dispositivo.
- Idioma o plataforma.

Los servidores web pueden usar esta información para devolver contenido específico para un navegador, aunque también puede exponer información innecesaria sobre el usuario.

---

## Accept Header

El header `Accept` especifica los tipos de contenido que el cliente acepta.

```http
Accept: text/html,application/xhtml+xml,application/json
```

Tipos de contenido comunes:

```text
text/html          = página HTML.
application/json   = datos JSON de API.
application/xml    = datos XML.
text/plain         = texto plano.
image/png          = imagen PNG.
image/jpeg         = imagen JPEG.
```

---

## Accept-Encoding Header

El header `Accept-Encoding` especifica qué formatos de codificación o compresión acepta el cliente.

```http
Accept-Encoding: gzip, deflate, br
```

Valores comunes:

```text
gzip    = formato común de compresión.
deflate = formato de compresión.
br      = compresión Brotli.
```

La compresión reduce el tamaño de las respuestas y mejora el rendimiento.

---

## Connection Header

El header `Connection` controla cómo se gestiona la conexión de red.

```http
Connection: keep-alive
```

Con HTTP/1.1, `keep-alive` permite al navegador reutilizar una misma conexión TCP para varias solicitudes.

```text
Connection: close       = cierra la conexión después de la respuesta.
Connection: keep-alive  = mantiene la conexión abierta para futuras solicitudes.
```

---

## Authorization Header

El header `Authorization` envía credenciales o tokens de autenticación.

Ejemplo de HTTP Basic Authentication:

```http
Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=
```

El valor después de `Basic` está codificado en Base64 y no debe considerarse cifrado.

Ejemplo de un bearer token:

```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

Durante las pruebas de seguridad, verifica que los tokens:

- No se expongan en URLs.
- Expiren correctamente.
- No puedan reutilizarse después del logout.
- Sean validados por el servidor.
- Estén protegidos mediante HTTPS.

---

## Cookie Header

El header `Cookie` envía cookies guardadas en el navegador al servidor.

```http
Cookie: session=abc123; theme=dark
```

Las cookies se usan habitualmente para:

- Gestión de sesiones.
- Autenticación.
- Preferencias de usuario.
- Selección de idioma.
- Carros de compra.
- Tracking.

### Nota de seguridad

Las cookies de sesión son sensibles porque pueden permitir acceso a cuentas autenticadas.

---

## Request Body

Algunos métodos HTTP incluyen un cuerpo que contiene datos enviados al servidor.

Métodos que utilizan frecuentemente un cuerpo:

- POST.
- PUT.
- PATCH.

### Ejemplo de datos de formulario

```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 31

username=student&password=test123
```

### Ejemplo JSON

```http
POST /api/users HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "username": "student",
  "email": "student@example.com"
}
```

---

# Métodos HTTP

Los métodos HTTP, también llamados verbos HTTP, definen la acción que el cliente quiere realizar sobre un recurso.

| Método | Función | Seguro | Idempotente |
|---|---|---:|---:|
| GET | Recupera datos del servidor | Sí | Sí |
| POST | Envía datos para procesar o crear | No | No |
| PUT | Crea o reemplaza un recurso completo | No | Normalmente sí |
| DELETE | Elimina un recurso | No | Normalmente sí |
| PATCH | Aplica una actualización parcial | No | No siempre |
| HEAD | Recupera solo headers, sin cuerpo de respuesta | Sí | Sí |
| OPTIONS | Recupera opciones de comunicación y métodos admitidos | Sí | Sí |

### Métodos seguros

Un método **seguro** no debería modificar el estado del servidor.

Ejemplos:

```text
GET
HEAD
OPTIONS
```

### Métodos idempotentes

Un método **idempotente** debería dar el mismo resultado aunque se repita varias veces.

Por ejemplo, ejecutar una solicitud `GET` diez veces no debería cambiar los datos del servidor.

---

## GET

`GET` recupera datos del servidor.

```http
GET /products?id=10 HTTP/1.1
Host: example.com
```

GET no debería modificar datos del servidor.

### Nota de seguridad

No se deben enviar datos sensibles mediante parámetros GET porque las URLs pueden guardarse en:

- Historial del navegador.
- Logs del servidor.
- Logs de proxies.
- Marcadores.
- Headers Referer.

Ejemplo incorrecto:

```text
https://example.com/login?username=student&password=password123
```

---

## POST

`POST` envía datos al servidor para procesarlos.

Se usa frecuentemente para:

- Formularios de login.
- Registro de usuarios.
- Subida de archivos.
- Creación de pedidos.
- Creación de comentarios.
- Envío de datos de pago.

```http
POST /register HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

username=student&password=securepassword
```

POST puede cambiar el estado del servidor y no suele ser idempotente.

---

## PUT

`PUT` crea o reemplaza un recurso en una URL concreta.

```http
PUT /api/users/10 HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "username": "student",
  "role": "user"
}
```

Si el recurso existe, PUT normalmente lo reemplaza por completo. Si no existe, puede crearlo.

---

## DELETE

`DELETE` solicita eliminar un recurso.

```http
DELETE /api/users/10 HTTP/1.1
Host: example.com
```

Las solicitudes DELETE deben estar correctamente autenticadas y autorizadas.

Si la autorización es débil, un atacante podría eliminar datos de otros usuarios.

---

## PATCH

`PATCH` aplica modificaciones parciales sobre un recurso.

```http
PATCH /api/users/10 HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "email": "new-email@example.com"
}
```

A diferencia de PUT, PATCH suele modificar solamente los campos seleccionados.

---

## HEAD

`HEAD` es similar a GET, pero devuelve solo los headers de la respuesta y no el cuerpo.

```http
HEAD /backup.zip HTTP/1.1
Host: example.com
```

HEAD puede ser útil para comprobar:

- Si existe un recurso.
- Los headers de respuesta.
- El tipo de contenido.
- El tamaño del contenido.
- La fecha de modificación.

---

## OPTIONS

`OPTIONS` recupera las opciones de comunicación disponibles para un recurso.

```http
OPTIONS /api/users HTTP/1.1
Host: example.com
```

El servidor puede devolver métodos permitidos mediante el header `Allow`.

```http
Allow: GET, POST, OPTIONS
```

Durante una evaluación de seguridad, OPTIONS puede ayudar a identificar métodos habilitados como PUT, DELETE o PATCH.

---

# Respuestas HTTP

## Componentes de una respuesta HTTP

Una respuesta HTTP normalmente contiene:

1. Línea de estado.
2. Headers de respuesta.
3. Línea vacía.
4. Cuerpo de respuesta opcional.

### Estructura de respuesta

```http
HTTP/version status-code status-message
Header-Name: Header-Value
Header-Name: Header-Value

Response body
```

### Ejemplo

```http
HTTP/1.1 200 OK
Date: Fri, 13 Mar 2015 11:26:05 GMT
Cache-Control: private, max-age=0
Content-Type: text/html; charset=UTF-8
Content-Encoding: gzip
Server: nginx
Content-Length: 258

<html>
  <body>
    <h1>Hello World</h1>
  </body>
</html>
```

---

## Línea de estado

La primera línea de una respuesta HTTP se llama **status line**.

```http
HTTP/1.1 200 OK
```

Contiene:

```text
HTTP/1.1 = versión del protocolo.
200      = código de estado HTTP.
OK       = mensaje de estado legible.
```

---

## Códigos HTTP comunes

| Código | Significado |
|---|---|
| 200 OK | La solicitud tuvo éxito y el servidor devolvió el recurso solicitado |
| 201 Created | Se creó un recurso correctamente |
| 204 No Content | La solicitud tuvo éxito, pero no existe cuerpo de respuesta |
| 301 Moved Permanently | El recurso se ha movido permanentemente a otra URL |
| 302 Found | El recurso está temporalmente disponible en otra URL |
| 303 See Other | El cliente debe recuperar otra URL mediante GET |
| 307 Temporary Redirect | Redirección temporal; debe conservarse el método HTTP |
| 400 Bad Request | El servidor no puede procesar la solicitud porque está mal formada |
| 401 Unauthorized | Se requiere autenticación o las credenciales no son válidas |
| 403 Forbidden | El servidor entendió la solicitud, pero rechaza el acceso |
| 404 Not Found | El recurso solicitado no existe |
| 405 Method Not Allowed | El método HTTP no está permitido para ese recurso |
| 429 Too Many Requests | El cliente envió demasiadas solicitudes; suele indicar rate limiting |
| 500 Internal Server Error | El servidor encontró un error inesperado |
| 502 Bad Gateway | Un gateway o proxy recibió una respuesta no válida de un servidor upstream |
| 503 Service Unavailable | El servicio no está disponible temporalmente o está sobrecargado |

---

## Headers de respuesta

Los headers de respuesta ofrecen información sobre la respuesta, el servidor, el caching, las cookies y el contenido.

| Header | Función |
|---|---|
| Date | Muestra cuándo el servidor generó la respuesta |
| Content-Type | Especifica el tipo de contenido de la respuesta |
| Content-Length | Indica el tamaño del cuerpo de respuesta en bytes |
| Content-Encoding | Especifica la compresión aplicada, como gzip |
| Server | Identifica el software o banner del servidor web |
| Set-Cookie | Indica al navegador que guarde una cookie |
| Cache-Control | Define reglas de caché |
| Location | Especifica el destino de una redirección |
| Strict-Transport-Security | Indica al navegador que debe usar HTTPS en el futuro |
| Access-Control-Allow-Origin | Define qué orígenes pueden acceder a un recurso mediante CORS |

---

## Date Header

El header `Date` indica cuándo el servidor generó la respuesta.

```http
Date: Fri, 13 Mar 2015 11:26:05 GMT
```

Ayuda a clientes y sistemas intermediarios a evaluar la frescura de una respuesta y sincronizar información relacionada con el tiempo.

---

## Content-Type Header

El header `Content-Type` especifica el tipo de datos devueltos por el servidor.

```http
Content-Type: text/html; charset=UTF-8
```

Ejemplos:

```text
text/html                 = página HTML.
application/json          = datos JSON.
application/xml           = datos XML.
text/plain                = texto plano.
image/png                 = imagen PNG.
application/pdf           = documento PDF.
application/javascript    = contenido JavaScript.
```

Los navegadores usan este header para decidir cómo procesar y mostrar la respuesta.

---

## Content-Length Header

El header `Content-Length` indica el tamaño del cuerpo de la respuesta en bytes.

```http
Content-Length: 258
```

Ayuda al navegador a saber cuánto contenido debe esperar.

---

## Content-Encoding Header

El header `Content-Encoding` especifica la compresión aplicada al cuerpo de la respuesta.

```http
Content-Encoding: gzip
```

El navegador utiliza este header para descomprimir correctamente la respuesta.

Valores comunes:

```text
gzip
deflate
br
```

---

## Server Header

El header `Server` puede revelar el software o banner del servidor web.

```http
Server: Apache/2.4.57
```

Otros ejemplos:

```text
Server: nginx
Server: Microsoft-IIS/10.0
Server: gws
```

### Nota de seguridad

Los banners detallados pueden ayudar a un atacante a identificar versiones y vulnerabilidades conocidas.

---

## Location Header

El header `Location` se utiliza normalmente con redirecciones.

```http
HTTP/1.1 302 Found
Location: https://example.com/login
```

El navegador sigue la ubicación y solicita la nueva URL.

---

## Set-Cookie Header

El header `Set-Cookie` indica al navegador que guarde una cookie.

```http
Set-Cookie: session=abc123; Secure; HttpOnly; SameSite=Lax
```

Se utiliza habitualmente para la gestión de sesiones después del login.

---

# Cache-Control

El header `Cache-Control` define cómo el navegador y las cachés intermedias pueden guardar y reutilizar una respuesta.

```http
Cache-Control: private, max-age=0
```

Directivas comunes:

| Directiva | Significado |
|---|---|
| `public` | La respuesta puede guardarse en caché por navegadores y cachés compartidas |
| `private` | La respuesta está destinada a un usuario y no debe guardarse en proxies compartidos |
| `no-cache` | La respuesta puede almacenarse, pero debe validarse con el servidor antes de reutilizarse |
| `no-store` | La respuesta no debe almacenarse en navegadores ni intermediarios |
| `max-age=<seconds>` | Define durante cuánto tiempo la respuesta puede permanecer en caché |

### Nota de seguridad

Las páginas sensibles, como perfiles de usuario o páginas de pago, deben utilizar reglas restrictivas de caché.

```http
Cache-Control: no-store
```

---

# Cookies y sesiones

## ¿Qué es una sesión?

Una sesión permite que un sitio web mantenga un estado temporal entre un usuario y el servidor.

Las sesiones permiten al servidor recordar información específica del usuario mientras navega por distintas páginas.

Se utilizan frecuentemente para:

- Autenticación.
- Carros de compra.
- Preferencias de usuario.
- Formularios de varios pasos.
- Estado de acceso temporal.

## ¿Qué es una cookie?

Una cookie es una pequeña pieza de información enviada por un sitio web al navegador.

El navegador la guarda y la envía nuevamente al servidor en solicitudes posteriores.

```http
Set-Cookie: session=abc123
```

Más tarde, el navegador puede enviar:

```http
Cookie: session=abc123
```

---

## Atributos de cookies

Los atributos de las cookies definen su alcance, duración y comportamiento de seguridad.

| Atributo | Función |
|---|---|
| Name | Identificador único de la cookie |
| Value | Datos almacenados en la cookie |
| Domain | Define qué dominio puede recibir la cookie |
| Path | Define qué ruta URL puede recibir la cookie |
| Expires / Max-Age | Define la duración de la cookie |
| Secure | Envía la cookie únicamente por HTTPS |
| HttpOnly | Impide que JavaScript acceda a la cookie |
| SameSite | Controla el comportamiento de cookies en solicitudes cross-site |
| Priority | Influye en la prioridad de eliminación de cookies por el navegador |
| Size | Tamaño máximo del nombre, valor y metadatos de la cookie |

### Ejemplo de cookie segura

```http
Set-Cookie: session=abc123; Secure; HttpOnly; SameSite=Strict; Path=/
```

```text
Secure          = la cookie se envía solamente por HTTPS.
HttpOnly        = JavaScript no puede leer la cookie.
SameSite=Strict = el navegador restringe el envío cross-site de la cookie.
Path=/          = la cookie está disponible en todo el sitio.
```

---

# Security Headers

Los security headers ayudan a los navegadores a aplicar protecciones de seguridad adicionales.

| Header | Propósito |
|---|---|
| Content-Security-Policy | Restringe fuentes de contenido permitidas y ayuda a mitigar inyección de scripts |
| Strict-Transport-Security | Fuerza el uso de HTTPS en futuras conexiones |
| X-Frame-Options | Controla si una página puede mostrarse dentro de un frame o iframe; ayuda a prevenir clickjacking |
| Referrer-Policy | Controla cuánta información de URL se envía mediante Referer |
| X-Content-Type-Options | Impide MIME-type sniffing |
| Permissions-Policy | Controla acceso a funcionalidades del navegador, como cámara, micrófono y geolocalización |

### Content-Security-Policy

```http
Content-Security-Policy: default-src 'self'
```

Esto restringe la carga de contenido al mismo origen, salvo que se permitan explícitamente otras fuentes.

### Strict-Transport-Security

```http
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

Indica al navegador que debe usar HTTPS para ese dominio durante el periodo indicado.

### X-Frame-Options

```http
X-Frame-Options: DENY
```

Impide que la página se muestre dentro de frames o iframes.

### Referrer-Policy

```http
Referrer-Policy: strict-origin-when-cross-origin
```

Reduce la exposición innecesaria de información de URL cuando un usuario navega a otros orígenes.

---

# HTTPS

## ¿Qué es HTTPS?

**HTTPS** significa **Hypertext Transfer Protocol Secure**.

HTTPS es la versión segura de HTTP y proporciona comunicación cifrada entre un cliente y un servidor web.

HTTPS ejecuta HTTP sobre **SSL/TLS**.

```text
Solicitud / Respuesta HTTP
          |
          v
Capa de cifrado SSL / TLS
          |
          v
TCP
```

### Arquitectura HTTPS

```text
[ Navegador ]
     |
     | Datos HTTP protegidos por TLS
     v
[ Internet ]
     |
     | Comunicación cifrada
     v
[ Servidor Web ]
```

---

## Por qué HTTP es inseguro

Por defecto, el tráfico HTTP se envía en texto claro.

Un atacante capaz de interceptar tráfico HTTP podría leer o modificar:

- Nombres de usuario.
- Contraseñas.
- Cookies de sesión.
- Datos de formularios.
- Tokens API.
- Información personal.
- Contenido de páginas web.

HTTP no proporciona cifrado fuerte, protección de integridad ni autenticación robusta entre navegador y servidor.

---

## SSL y TLS

SSL (*Secure Sockets Layer*) y TLS (*Transport Layer Security*) son protocolos criptográficos utilizados para proporcionar comunicación segura a través de una red.

TLS es el protocolo moderno. SSL es el término antiguo que se usa a menudo de manera informal al hablar de certificados HTTPS y tráfico web cifrado.

HTTPS proporciona:

- **Confidencialidad:** Un atacante no puede leer fácilmente el tráfico cifrado.
- **Integridad:** Un atacante no puede modificar datos en tránsito sin que se detecte.
- **Autenticación:** Los certificados ayudan al navegador a verificar la identidad del servidor.

---

## Ventajas de HTTPS

### Cifrado de datos en tránsito

HTTPS cifra los datos transmitidos entre el navegador y el servidor.

Aunque un atacante intercepte el tráfico, no debería poder leer el contenido cifrado.

### Protección contra eavesdropping

HTTPS ayuda a proteger datos sensibles contra interceptación de red, como:

- Credenciales de acceso.
- Información de tarjetas de crédito.
- Datos personales.
- Tokens de sesión.
- Mensajes privados.
- API keys.

### Protección contra manipulación

HTTPS reduce el riesgo de que un atacante modifique tráfico HTTP durante el tránsito.

Por ejemplo, ayuda a impedir que un atacante en la red inyecte JavaScript malicioso en una respuesta HTTP sin cifrar.

---

## HTTPS no corrige vulnerabilidades web

HTTPS es esencial, pero no protege contra errores dentro de la aplicación web.

Las siguientes vulnerabilidades pueden existir incluso usando HTTPS:

- SQL injection.
- Cross-site scripting.
- Broken access control.
- CSRF.
- SSRF.
- Vulnerabilidades de subida de archivos.
- Autenticación débil.
- Gestión insegura de sesiones.
- APIs inseguras.
- Fallos de lógica de negocio.

```text
HTTPS protege los datos mientras viajan entre el cliente y el servidor.

HTTPS no protege automáticamente una aplicación frente a código inseguro,
controles de acceso débiles, malas configuraciones o componentes vulnerables.
```

---

# Enumeración de métodos HTTP

Durante una evaluación de seguridad autorizada, puede ser útil identificar los métodos HTTP que acepta un servidor o recurso.

## OPTIONS con cURL

```bash
curl -X OPTIONS -i http://target.local/  # envía una solicitud OPTIONS y muestra los headers completos de la respuesta.
```

Busca el header `Allow`:

```http
Allow: GET, POST, OPTIONS
```

## Script de Nmap para métodos HTTP

```bash
nmap -p 80,443 --script http-methods <TARGET_IP>  # comprueba métodos HTTP compatibles en los puertos 80 y 443.
```

```text
-p 80,443              = escanea puertos HTTP y HTTPS comunes.
--script http-methods  = usa el script de Nmap para enumerar métodos HTTP.
<TARGET_IP>            = IP o hostname objetivo.
```

Prueba únicamente sistemas dentro del alcance autorizado.

---

# Comandos útiles de cURL

`curl` es una herramienta de línea de comandos para enviar solicitudes HTTP y ver respuestas del servidor.

```bash
curl http://example.com  # recupera el contenido de la página.
curl -I http://example.com  # recupera solo los headers de respuesta.
curl -i http://example.com  # recupera headers y cuerpo de respuesta.
curl -L http://example.com  # sigue redirecciones HTTP.
curl -A "Custom User Agent" http://example.com  # envía un User-Agent personalizado.
```

## Enviar una solicitud POST

```bash
curl -X POST -d "param1=value1&param2=value2" http://example.com/api  # envía datos de formulario mediante una solicitud HTTP POST.
```

```text
-X POST = especifica el método HTTP POST.
-d       = envía datos en el cuerpo de la solicitud.
```

## Enviar datos JSON

```bash
curl -X POST http://example.com/api \
  -H "Content-Type: application/json" \
  -d '{"username":"student","role":"user"}'  # envía datos JSON en una solicitud POST.
```

## Usar Basic Authentication

```bash
curl -u username:password http://api.example.com/data  # envía credenciales mediante HTTP Basic Authentication.
```

## Descargar un archivo

```bash
curl -O http://example.com/file.txt  # descarga file.txt usando su nombre remoto.
```

## Subir un archivo

```bash
curl --upload-file test.txt http://example.com/upload/test.txt  # sube test.txt usando una solicitud HTTP PUT si el servidor lo admite.
```

---

# Herramientas básicas de evaluación web

## Nmap

Nmap ayuda a identificar puertos abiertos, servicios, versiones e información relacionada con HTTP.

```bash
nmap -sV -p 80,443 <TARGET_IP>  # detecta versiones de servicios en puertos HTTP y HTTPS.
```

```bash
nmap -p 80,443 --script http-title,http-headers <TARGET_IP>  # obtiene títulos de páginas y headers HTTP.
```

## DIRB

DIRB es una herramienta de enumeración de directorios y archivos.

Utiliza wordlists para buscar directorios y archivos potencialmente ocultos en un servidor web.

```bash
dirb http://target.local  # inicia enumeración de directorios y archivos contra el sitio web objetivo.
```

Ejemplos de resultados interesantes:

```text
/admin
/login
/uploads
/backups
/config
/robots.txt
/.git
```

## Burp Suite

Burp Suite es una plataforma de pruebas de seguridad para aplicaciones web.

Puede utilizarse para:

- Interceptar tráfico HTTP/S.
- Inspeccionar solicitudes y respuestas.
- Modificar solicitudes manualmente.
- Reenviar solicitudes.
- Probar parámetros.
- Analizar cookies y headers.
- Mapear la superficie de ataque.

Componentes útiles de Burp Suite:

```text
Proxy      = intercepta tráfico del navegador.
Repeater   = modifica y reenvía solicitudes manualmente.
Intruder   = automatiza variaciones controladas de solicitudes.
Decoder    = codifica y decodifica datos.
Comparer   = compara solicitudes y respuestas.
```

---

# Flujo práctico HTTP/S

Un flujo básico de evaluación HTTP/S sería:

1. Identificar el servidor web y los puertos abiertos.
2. Navegar por la aplicación y mapear sus funcionalidades.
3. Inspeccionar solicitudes y respuestas con herramientas del navegador o Burp Suite.
4. Revisar headers HTTP, cookies, redirecciones y códigos de estado.
5. Enumerar métodos HTTP compatibles.
6. Revisar security headers.
7. Verificar si HTTPS está habilitado y forzado.
8. Comprobar los atributos de seguridad de cookies de sesión.
9. Identificar directorios, archivos, APIs y parámetros.
10. Probar solamente dentro del alcance aprobado.

---

# Puntos Clave

- HTTP es un protocolo de capa de aplicación sin estado que funciona sobre TCP.
- HTTP utiliza arquitectura cliente-servidor: los clientes envían solicitudes y los servidores devuelven respuestas.
- Las solicitudes HTTP contienen una request line, headers y un cuerpo opcional.
- Las respuestas HTTP contienen una status line, headers y un cuerpo opcional.
- Headers importantes de solicitud incluyen `Host`, `User-Agent`, `Accept`, `Authorization` y `Cookie`.
- Headers importantes de respuesta incluyen `Content-Type`, `Content-Length`, `Set-Cookie`, `Cache-Control` y `Server`.
- Los métodos HTTP definen cómo los clientes interactúan con recursos; los principales son GET, POST, PUT, DELETE, PATCH, HEAD y OPTIONS.
- Las cookies y sesiones permiten a las aplicaciones web mantener el estado de usuario.
- HTTPS utiliza TLS para proporcionar confidencialidad, integridad y autenticación del servidor.
- HTTPS protege datos en tránsito, pero no evita vulnerabilidades web como XSS, SQL injection o broken access control.
- Herramientas como cURL, Nmap, DIRB y Burp Suite son útiles para comprender y evaluar la comunicación HTTP/S.
