# Web Application Architecture & Components

## Arquitectura de aplicaciones web

La arquitectura de una aplicación web hace referencia a la estructura, organización e interacción de los componentes utilizados para construir una aplicación web.

Define cómo la aplicación:

- Gestiona las solicitudes de los usuarios.
- Procesa la lógica de negocio.
- Almacena y recupera datos.
- Se comunica con servicios externos.
- Entrega respuestas a los usuarios.

Una arquitectura bien diseñada es importante para garantizar:

- Escalabilidad.
- Mantenibilidad.
- Rendimiento.
- Fiabilidad.
- Seguridad.

Antes de realizar una evaluación de seguridad sobre una aplicación web, es fundamental comprender su arquitectura. Este conocimiento permite identificar dónde pueden existir vulnerabilidades, configuraciones incorrectas, flujos de datos inseguros o controles de acceso débiles.

---

## Modelo cliente-servidor

Las aplicaciones web normalmente utilizan el **modelo cliente-servidor**.

La aplicación se divide en dos partes principales:

- **Cliente:** El front-end al que accede el usuario desde el navegador.
- **Servidor:** El back-end que procesa solicitudes, aplica la lógica de negocio, se comunica con bases de datos y devuelve respuestas.

### Arquitectura básica

```text
[ Usuario / Navegador ]
          |
          | Solicitud HTTP / HTTPS
          v
[ Servidor Web ]
          |
          | Archivos estáticos o solicitud reenviada
          v
[ Servidor de Aplicaciones / Backend ]
          |
          | Consultas a BD / Solicitudes API
          v
[ Base de Datos / Servicios Externos ]
          |
          | Respuesta de datos
          v
[ Servidor de Aplicaciones / Backend ]
          |
          | Respuesta HTTP / HTTPS
          v
[ Usuario / Navegador ]
```

---

## Componentes principales de una aplicación web

| Componente | Función |
|---|---|
| Interfaz de usuario (UI) | La parte visual de la aplicación con la que los usuarios interactúan: páginas web, formularios, menús, botones y campos de entrada |
| Tecnologías client-side | Tecnologías que se ejecutan en el navegador del usuario, como HTML, CSS y JavaScript |
| Servidor web | Recibe solicitudes HTTP y sirve contenido estático como HTML, CSS, JavaScript, imágenes y archivos |
| Servidor de aplicaciones | Ejecuta código server-side, procesa lógica de negocio, gestiona solicitudes dinámicas y se comunica con bases de datos |
| Tecnologías server-side | Lenguajes y frameworks usados para procesar solicitudes y generar contenido dinámico, como PHP, Python, Java, Ruby o Node.js |
| Servidor de base de datos | Almacena y gestiona datos de la aplicación, como usuarios, productos, publicaciones, configuraciones y registros |
| Lógica de aplicación | Reglas de negocio que definen cómo funciona la aplicación: autenticación, validación, autorización y flujos de trabajo |
| APIs | Interfaces que permiten a aplicaciones y servicios intercambiar datos de forma programática |

---

## Procesamiento client-side

El procesamiento client-side ocurre en el dispositivo del usuario, normalmente dentro del navegador.

Se encarga de presentar la interfaz y de gestionar tareas interactivas sin necesidad de contactar siempre al servidor.

### Tecnologías principales client-side

```text
HTML          = define la estructura y contenido de una página.
CSS           = define la apariencia, diseño, colores y fuentes.
JavaScript    = añade interactividad y comportamiento dinámico.
Cookies       = guardan pequeñas cantidades de datos en el navegador, normalmente para sesiones.
Local Storage = guarda datos en el navegador que pueden persistir tras cerrarlo.
```

### Tareas comunes del lado cliente

- Mostrar páginas web.
- Gestionar botones, menús y formularios.
- Realizar validaciones básicas de entrada.
- Actualizar elementos de la página dinámicamente.
- Enviar solicitudes asíncronas a APIs.
- Guardar preferencias del usuario.
- Gestionar el estado de la aplicación en el navegador.

### Nota de seguridad importante

Los controles client-side nunca deben considerarse suficientes por sí solos.

Un usuario o atacante puede modificar:

- HTML.
- JavaScript.
- Campos ocultos de formularios.
- Solicitudes del navegador.
- Cookies.
- Valores de Local Storage.

Por esta razón, la validación crítica, la autorización y los controles de seguridad también deben implementarse en el servidor.

---

## Procesamiento server-side

El procesamiento server-side ocurre en el servidor remoto donde se hospeda la aplicación.

El servidor recibe solicitudes de clientes, aplica la lógica de negocio, consulta datos, realiza comprobaciones de seguridad y genera respuestas.

### Tecnologías comunes server-side

```text
PHP                  = lenguaje muy utilizado para aplicaciones web dinámicas.
Python               = usado frecuentemente con frameworks como Django o Flask.
Java                 = usado con frameworks empresariales y servidores de aplicaciones.
Ruby                 = utilizado comúnmente con Ruby on Rails.
JavaScript / Node.js = permite ejecutar JavaScript en el servidor.
```

### Tareas comunes del lado servidor

- Procesar solicitudes de inicio de sesión.
- Validar credenciales.
- Aplicar reglas de autorización.
- Consultar bases de datos.
- Procesar pagos.
- Subir y almacenar archivos.
- Enviar correos electrónicos.
- Llamar APIs externas.
- Generar HTML dinámico o respuestas JSON.
- Registrar actividad y eventos de seguridad.

### Ventaja de seguridad

El procesamiento server-side es más seguro para operaciones sensibles porque los usuarios no pueden modificar directamente el código que se ejecuta en el servidor.

Sin embargo, el código server-side puede contener vulnerabilidades como:

- SQL injection.
- Command injection.
- Server-side request forgery.
- File inclusion.
- Deserialización insegura.
- Broken access control.
- Bypasses de autenticación.

---

## Servidor web vs servidor de aplicaciones

### Servidor web

Un servidor web recibe solicitudes HTTP o HTTPS y sirve contenido estático.

Ejemplos comunes:

```text
Apache HTTP Server
Nginx
Microsoft IIS
```

Un servidor web suele servir:

- Archivos HTML.
- Archivos CSS.
- Archivos JavaScript.
- Imágenes.
- Fuentes.
- Documentos descargables.
- Recursos estáticos.

### Servidor de aplicaciones

Un servidor de aplicaciones ejecuta código server-side y gestiona solicitudes dinámicas.

Normalmente:

- Procesa la lógica de aplicación.
- Gestiona acciones de usuario.
- Se comunica con bases de datos.
- Genera páginas dinámicas.
- Devuelve respuestas API.
- Aplica reglas de negocio.

En aplicaciones pequeñas, el servidor web y el de aplicaciones pueden ejecutarse en la misma máquina. En entornos más grandes, pueden separarse por motivos de rendimiento y seguridad.

---

## Bases de datos

Las bases de datos almacenan y gestionan la información utilizada por las aplicaciones web.

Contenido habitual:

- Cuentas de usuario.
- Hashes de contraseñas.
- Información de productos.
- Registros de clientes.
- Pedidos.
- Publicaciones y comentarios.
- Configuraciones de aplicación.
- Datos de sesión.
- Logs de auditoría.

### Tecnologías comunes

```text
MySQL
MariaDB
PostgreSQL
Microsoft SQL Server
Oracle Database
MongoDB
```

### Importancia para la seguridad

Las bases de datos son objetivos de alto valor porque pueden contener información sensible.

Riesgos comunes:

- SQL injection.
- Credenciales débiles de base de datos.
- Privilegios excesivos.
- Puertos de bases de datos expuestos.
- Datos sensibles sin cifrar.
- Backups expuestos.
- Mensajes de error de base de datos detallados.

---

## Lógica de aplicación

La lógica de aplicación representa las reglas y procedimientos que controlan cómo funciona una aplicación.

Ejemplos:

- Registro de usuarios.
- Inicio y cierre de sesión.
- Restablecimiento de contraseñas.
- Control de acceso basado en roles.
- Cálculo de carros de compra.
- Validación de pagos.
- Reglas de subida de archivos.
- Validación de datos.
- Gestión de cuentas.
- Flujos administrativos.

### Importancia para la seguridad

Las vulnerabilidades de lógica de negocio aparecen cuando un atacante abusa de una funcionalidad legítima de una forma no prevista.

Ejemplos:

- Cambiar el precio de un producto en una solicitud.
- Reutilizar un código de descuento.
- Saltarse un paso de pago.
- Acceder a la factura de otro usuario modificando un ID.
- Cambiar roles de cuentas mediante solicitudes modificadas.
- Evitar flujos de aprobación.

---

## Comunicación HTTP y flujo de datos

Las aplicaciones web se comunican por Internet mediante **HTTP** o **HTTPS**.

Cuando un usuario hace clic en un enlace, envía un formulario o carga una página, el navegador envía una solicitud HTTP al servidor.

El servidor procesa la solicitud, puede consultar una base de datos o API externa, y envía una respuesta HTTP de vuelta al navegador.

### Flujo básico de una solicitud

```text
1. El usuario introduce una URL o hace clic en un enlace.
2. El navegador envía una solicitud HTTP/HTTPS.
3. El servidor web recibe la solicitud.
4. El servidor de aplicaciones procesa la solicitud.
5. El backend consulta una base de datos o API si es necesario.
6. El backend genera una respuesta.
7. El servidor envía una respuesta HTTP/HTTPS.
8. El navegador renderiza la respuesta para el usuario.
```

### Ejemplo de solicitud HTTP

```http
GET /profile?id=10 HTTP/1.1
Host: example.com
Cookie: session=abc123
```

### Ejemplo de respuesta HTTP

```http
HTTP/1.1 200 OK
Content-Type: text/html

<html>
  <body>
    <h1>User Profile</h1>
  </body>
</html>
```

---

## Cómo se renderizan las páginas web

Cuando un usuario visita una dirección web, el navegador solicita un recurso al servidor.

### Proceso de renderizado

```text
1. El usuario visita una URL.
2. El navegador solicita la página al servidor.
3. El servidor devuelve un documento HTML.
4. El navegador analiza el HTML.
5. El navegador descarga CSS, JavaScript, imágenes y fuentes enlazadas.
6. El navegador analiza el CSS y genera información de estilo.
7. El navegador ejecuta JavaScript.
8. JavaScript puede modificar la página y solicitar datos adicionales a APIs.
9. El navegador renderiza la página final para el usuario.
```

### Modelo de renderizado del navegador

```text
[ Respuesta HTML ]
       |
       v
[ Parser HTML ] ------> Construye el DOM
       |
       +-----> Descarga archivos CSS
       |              |
       |              v
       |         [ Parser CSS ]
       |
       +-----> Descarga archivos JavaScript
                      |
                      v
             [ Motor JavaScript ]
                      |
                      v
              Modifica el DOM
                      |
                      v
            [ Página web renderizada ]
```

### DOM

El **DOM** (*Document Object Model*) es una representación estructurada de la página web creada por el navegador.

JavaScript puede leer y modificar el DOM dinámicamente.

Por ejemplo, JavaScript puede:

- Cambiar texto de la página.
- Ocultar o mostrar elementos.
- Modificar formularios.
- Añadir botones.
- Cargar datos nuevos.
- Enviar solicitudes API.
- Cambiar el comportamiento del navegador.

---

## Intercambio de datos

El **intercambio de datos** es el proceso de intercambiar información entre aplicaciones, sistemas o servicios.

Permite que sistemas con diferentes lenguajes de programación, estructuras de datos, sistemas operativos o plataformas puedan comunicarse.

Las aplicaciones web suelen intercambiar datos con:

- Bases de datos.
- APIs externas.
- Aplicaciones móviles.
- Pasarelas de pago.
- Proveedores de autenticación.
- Servicios cloud.
- Sistemas internos de empresa.
- Microservicios.

---

## APIs

Una **API** (*Application Programming Interface*) permite que sistemas de software distintos se comuniquen e intercambien datos.

Por ejemplo, una aplicación web puede usar APIs para:

- Procesar pagos.
- Enviar correos.
- Obtener datos meteorológicos.
- Autenticarse con Google o Microsoft.
- Acceder a mapas.
- Obtener información de productos.
- Conectar aplicaciones móviles con el backend.

### Importancia de seguridad de APIs

Las APIs pueden exponer datos o funciones sensibles si no están bien protegidas.

Comprobaciones importantes:

- Autenticación.
- Autorización.
- Validación de tokens.
- Rate limiting.
- Validación de entrada.
- Manejo de errores.
- Exposición de datos.
- Enumeración de endpoints.
- Logging y monitorización.

---

## JSON

**JSON** (*JavaScript Object Notation*) es un formato ligero de intercambio de datos, muy utilizado en aplicaciones web y APIs.

Es fácil de leer para las personas y de procesar para las aplicaciones.

### Ejemplo de JSON

```json
{
  "username": "student",
  "role": "user",
  "email": "student@example.com"
}
```

JSON se utiliza habitualmente cuando un navegador se comunica con una API REST.

---

## XML

**XML** (*eXtensible Markup Language*) es un formato de intercambio de datos que utiliza etiquetas para definir la estructura.

Se utiliza a menudo para:

- Archivos de configuración.
- Sistemas heredados.
- Servicios web SOAP.
- Integraciones empresariales.

### Ejemplo de XML

```xml
<user>
  <username>student</username>
  <role>user</role>
  <email>student@example.com</email>
</user>
```

### Importancia de seguridad

Las aplicaciones que procesan XML pueden ser vulnerables a ataques XML External Entity si los parsers XML tienen una configuración insegura.

---

## REST

**REST** (*Representational State Transfer*) es un estilo arquitectónico usado habitualmente para APIs web.

Las APIs REST suelen usar métodos HTTP estándar:

```text
GET     = recupera datos.
POST    = crea datos.
PUT     = reemplaza o actualiza datos.
PATCH   = actualiza parcialmente datos.
DELETE  = elimina datos.
```

### Ejemplo de endpoints REST

```text
GET    /api/users          # obtiene usuarios.
GET    /api/users/10       # obtiene el usuario 10.
POST   /api/users          # crea un usuario.
PUT    /api/users/10       # actualiza el usuario 10.
DELETE /api/users/10       # elimina el usuario 10.
```

### Importancia de seguridad

Durante las pruebas, verifica que los controles de autorización se apliquen a cada endpoint y a cada método HTTP.

---

## SOAP

**SOAP** (*Simple Object Access Protocol*) es un protocolo para intercambiar información estructurada entre sistemas.

SOAP suele usar XML y proporciona un método estandarizado para la comunicación entre servicios web.

Es habitual encontrarlo en entornos empresariales y aplicaciones heredadas.

---

## Tecnologías de seguridad

### Autenticación y autorización

La autenticación confirma la identidad de un usuario.

La autorización determina qué áreas, acciones y datos de una aplicación puede acceder dicho usuario.

Ejemplos:

- Autenticación con usuario y contraseña.
- Autenticación multifactor.
- Cookies de sesión.
- Tokens JWT.
- Control de acceso basado en roles.
- Listas de control de acceso.

---

### SSL y TLS

**TLS** cifra la comunicación entre el cliente y el servidor.

HTTPS utiliza HTTP sobre TLS.

```text
HTTP  = los datos pueden viajar sin cifrar.
HTTPS = los datos se cifran durante la transmisión.
```

TLS ayuda a proteger contra:

- Interceptación de credenciales.
- Robo de cookies de sesión en la red.
- Ataques Man-in-the-Middle.
- Exposición de información sensible en tránsito.

---

## Tecnologías externas

### Content Delivery Networks

Una **Content Delivery Network**, o CDN, distribuye contenido estático entre múltiples servidores ubicados en distintas regiones geográficas.

Las CDNs suelen distribuir:

- Imágenes.
- Archivos CSS.
- Librerías JavaScript.
- Fuentes.
- Vídeos.
- Recursos descargables.

Beneficios:

- Carga de páginas más rápida.
- Menor carga sobre el servidor de origen.
- Mejor disponibilidad.
- Mayor fiabilidad.
- Cierta protección frente a grandes volúmenes de tráfico.

---

### Librerías y frameworks de terceros

Las aplicaciones web suelen utilizar librerías y frameworks de terceros para acelerar el desarrollo y añadir funcionalidades avanzadas.

Ejemplos:

```text
Frameworks frontend: React, Angular, Vue.js
Frameworks backend: Django, Flask, Laravel, Spring
Librerías JavaScript: jQuery, Lodash
CMS: WordPress, Drupal, Joomla
```

### Importancia de seguridad

Los componentes de terceros pueden introducir vulnerabilidades si están desactualizados o son inseguros.

Durante una evaluación de seguridad, identifica:

- Versiones de frameworks.
- Tecnologías de servidor.
- Librerías JavaScript.
- Plugins de CMS.
- Dependencias.
- Componentes con vulnerabilidades conocidas.

---

## Visión de seguridad de una aplicación web

Durante una evaluación de seguridad, es importante saber qué componente procesa cada acción.

```text
[ Navegador ]
     |
     | Client-side: HTML, CSS, JavaScript, Cookies
     v
[ Servidor Web ]
     |
     | Apache, Nginx, IIS
     v
[ Servidor de Aplicaciones ]
     |
     | PHP, Python, Java, Ruby, Node.js
     v
[ Base de Datos ]
     |
     | MySQL, PostgreSQL, MSSQL, Oracle
     v
[ APIs / Servicios Externos ]
```

Esto ayuda a identificar posibles superficies de ataque:

| Componente | Áreas de seguridad |
|---|---|
| Navegador / Cliente | XSS, manipulación del DOM, Local Storage inseguro, tokens expuestos |
| Servidor web | Errores de configuración, archivos expuestos, TLS débil, directory listing |
| Servidor de aplicaciones | Fallos de autenticación, problemas de control de acceso, inyecciones, errores de lógica de negocio |
| Base de datos | SQL injection, permisos débiles, exposición de datos, backups inseguros |
| APIs | Broken object authorization, problemas de tokens, exposición excesiva de datos, falta de rate limiting |
| Componentes de terceros | Librerías desactualizadas, plugins vulnerables, dependencias inseguras |

---

## Puntos Clave

- La arquitectura de aplicaciones web define cómo interactúan los componentes para procesar solicitudes y gestionar datos.
- Las aplicaciones web normalmente utilizan un modelo cliente-servidor.
- Las tecnologías client-side incluyen HTML, CSS, JavaScript, cookies y Local Storage.
- Las tecnologías server-side incluyen servidores web, servidores de aplicaciones, bases de datos y lenguajes server-side.
- La validación sensible, autorización y lógica de negocio deben aplicarse siempre en el lado servidor.
- HTTP y HTTPS se utilizan para la comunicación entre clientes y servidores.
- Las APIs suelen intercambiar datos usando JSON o XML.
- Las APIs REST usan métodos como GET, POST, PUT, PATCH y DELETE.
- Comprender la arquitectura permite al pentester identificar superficies de ataque, vulnerabilidades y errores de configuración.
