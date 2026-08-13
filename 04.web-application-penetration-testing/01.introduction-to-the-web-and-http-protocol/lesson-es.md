# Introduction to Web App Security Testing

## ¿Qué son las aplicaciones web?

Una **aplicación web** es una aplicación de software que se ejecuta en un servidor web y se accede a ella mediante un navegador.

Las aplicaciones web proporcionan funcionalidades dinámicas e interactivas, permitiendo a los usuarios acceder a información, enviar datos, autenticarse, realizar compras, gestionar cuentas e interactuar con servicios online.

Ejemplos comunes:

- Redes sociales como Facebook y X.
- Servicios de correo web como Gmail y Outlook.
- Sitios de comercio electrónico como Amazon y eBay.
- Plataformas de productividad en la nube como Google Workspace y Microsoft 365.
- Aplicaciones de banca online.
- Portales de clientes.
- Sistemas de gestión de contenido.
- APIs web y plataformas SaaS.

---

## Cómo funcionan las aplicaciones web

Las aplicaciones web normalmente utilizan una **arquitectura cliente-servidor**.

```text
[ Usuario / Navegador ]
          |
          | Solicitud HTTP o HTTPS
          v
[ Servidor web / Aplicación web ]
          |
          | Consulta o solicitud
          v
[ Base de datos / API / Servicios internos ]
          |
          | Respuesta
          v
[ Servidor web / Aplicación web ]
          |
          | Respuesta HTTP o HTTPS
          v
[ Usuario / Navegador ]
```

### Componentes principales

- **Cliente:** El navegador del usuario, como Firefox, Chrome o Edge.
- **Servidor web:** Recibe solicitudes HTTP y devuelve respuestas.
- **Aplicación web:** Contiene la lógica de negocio de la aplicación.
- **Base de datos:** Almacena información como usuarios, contraseñas, pedidos y datos de la aplicación.
- **API:** Permite que sistemas o aplicaciones intercambien datos de forma programática.

### Tecnologías de interfaz

Las interfaces de aplicaciones web suelen construirse con:

- **HTML:** Define la estructura y contenido de una página web.
- **CSS:** Define la apariencia y el diseño de la página.
- **JavaScript:** Añade comportamiento dinámico e interactivo en el navegador.

---

## HTTP y statelessness

Los navegadores se comunican con los servidores web principalmente mediante los protocolos **HTTP** o **HTTPS**.

HTTP es un protocolo **sin estado** (*stateless*). Esto significa que cada solicitud es independiente por defecto y que el servidor no recuerda automáticamente las solicitudes anteriores.

Las aplicaciones web utilizan mecanismos como cookies, IDs de sesión y tokens para mantener el estado del usuario.

### Ejemplo

```text
1. El usuario inicia sesión con un nombre de usuario y contraseña.
2. El servidor valida las credenciales.
3. El servidor crea una sesión o token.
4. El navegador guarda la cookie o token de sesión.
5. El navegador lo envía con las siguientes solicitudes.
6. El servidor reconoce al usuario autenticado.
```

Si la gestión de sesiones es débil, un atacante puede intentar secuestrar sesiones, fijar sesiones o robar tokens.

---

## Seguridad de aplicaciones web

La seguridad de aplicaciones web se centra en proteger las aplicaciones web frente a vulnerabilidades, ataques, accesos no autorizados, filtraciones de datos e interrupciones de servicio.

El objetivo principal es preservar la **tríada CIA**:

- **Confidencialidad:** La información sensible solo es accesible para usuarios autorizados.
- **Integridad:** Los datos no pueden modificarse sin autorización.
- **Disponibilidad:** La aplicación permanece accesible para usuarios legítimos.

Las aplicaciones web son objetivos atractivos porque suelen ser accesibles públicamente y pueden procesar información valiosa, como:

- Usuarios y contraseñas.
- Datos personales.
- Información de pago.
- Datos financieros.
- Documentos internos.
- Datos de negocio.
- Propiedad intelectual.
- Tokens de autenticación.

---

## Por qué importa la seguridad web

La seguridad de aplicaciones web es importante porque una aplicación vulnerable puede provocar:

- Exposición de información sensible.
- Secuestro de cuentas.
- Robo financiero.
- Daño reputacional.
- Sanciones regulatorias.
- Robo de propiedad intelectual.
- Interrupciones del servicio.
- Pérdida de confianza de los clientes.
- Defacement del sitio web.
- Manipulación de datos.

### Impacto de negocio

Un ataque exitoso contra una aplicación web puede afectar tanto a la organización como a sus usuarios.

Por ejemplo, una vulnerabilidad SQL injection puede permitir que un atacante acceda a una base de datos que contiene cuentas de usuarios, direcciones de correo, hashes de contraseñas o datos de pago.

---

## Prácticas de seguridad web

### Autenticación y autorización

La autenticación verifica **quién es el usuario**.

La autorización determina **a qué tiene acceso el usuario autenticado**.

```text
Autenticación: “¿Quién eres?”
Autorización: “¿A qué tienes permiso para acceder?”
```

Buenas prácticas:

- Aplicar contraseñas robustas.
- Usar autenticación multifactor.
- Implementar procesos seguros de restablecimiento de contraseña.
- Aplicar control de acceso basado en roles.
- Verificar autorización en cada acción sensible.
- Evitar que los usuarios accedan a recursos de otros usuarios.

---

### Validación de entrada y codificación de salida

Las aplicaciones deben validar toda entrada controlada por el usuario.

Posibles fuentes de entrada:

- Parámetros de URL.
- Campos de formularios.
- Cabeceras HTTP.
- Cookies.
- Solicitudes JSON.
- Archivos subidos.
- Solicitudes API.

La validación de entrada ayuda a prevenir ataques como SQL injection y command injection.

La codificación de salida ayuda a prevenir ataques como cross-site scripting.

---

### Comunicación segura

Las aplicaciones deben usar HTTPS para cifrar el tráfico entre el navegador y el servidor.

```text
HTTP   = comunicación sin cifrar.
HTTPS  = comunicación HTTP cifrada mediante TLS.
```

HTTPS protege datos sensibles en tránsito, como:

- Credenciales de acceso.
- Cookies de sesión.
- Información de pago.
- Datos personales.
- Tokens API.

---

### Desarrollo seguro

Las prácticas de desarrollo seguro reducen la probabilidad de introducir vulnerabilidades durante el desarrollo.

Prácticas importantes:

- Validar toda entrada.
- Usar consultas parametrizadas en bases de datos.
- Evitar funciones peligrosas cuando sea posible.
- Implementar manejo seguro de errores.
- No mostrar stack traces a los usuarios.
- Proteger secretos y API keys.
- Usar configuraciones seguras por defecto.
- Revisar el código antes de desplegarlo.

---

### Actualizaciones regulares

Las aplicaciones web dependen de muchos componentes:

- Servidores web.
- Frameworks.
- Librerías.
- Plataformas CMS.
- Plugins.
- Bases de datos.
- Sistemas operativos.

Todos los componentes deben actualizarse regularmente porque el software desactualizado puede tener vulnerabilidades conocidas.

---

### Principio de mínimo privilegio

El **principio de mínimo privilegio** consiste en asignar a usuarios, aplicaciones, servicios y procesos solo los permisos estrictamente necesarios.

Ejemplos:

- Una cuenta de base de datos no debe tener privilegios de administrador si no los necesita.
- Un usuario normal no debe acceder a páginas administrativas.
- Una aplicación web no debe ejecutarse como root o Administrator.
- Los tokens API deben tener permisos limitados y fechas de expiración.

---

### Web Application Firewall

Un **Web Application Firewall**, o WAF, monitoriza y filtra el tráfico HTTP entre los usuarios y una aplicación web.

Un WAF puede ayudar a detectar o bloquear:

- Intentos comunes de SQL injection.
- Payloads de cross-site scripting.
- Bots maliciosos.
- Patrones de explotación conocidos.
- Solicitudes excesivas.
- Solicitudes HTTP sospechosas.

Un WAF es útil, pero no reemplaza el desarrollo seguro ni las pruebas de seguridad.

---

### Gestión de sesiones

Una gestión segura de sesiones ayuda a prevenir el secuestro de sesiones autenticadas.

Prácticas importantes:

- Usar identificadores de sesión largos y aleatorios.
- Configurar atributos seguros en las cookies.
- Regenerar el ID de sesión después del login.
- Expirar sesiones tras un periodo de inactividad.
- Invalidar sesiones al cerrar sesión.
- Usar HTTPS en todas las páginas autenticadas.
- Evitar exponer tokens de sesión en URLs.

---

# Pruebas de seguridad en aplicaciones web

## ¿Qué son las pruebas de seguridad web?

Las pruebas de seguridad web son el proceso de evaluar una aplicación para identificar vulnerabilidades, debilidades, errores de configuración y riesgos de seguridad.

El objetivo principal es encontrar y corregir fallos antes de que atacantes maliciosos puedan explotarlos.

Las pruebas de seguridad pueden evaluar:

- Autenticación.
- Autorización.
- Validación de entrada.
- Gestión de sesiones.
- Seguridad de APIs.
- Funcionalidad de subida de archivos.
- Configuración del servidor.
- Componentes de terceros.
- Lógica de negocio.
- Protección de datos.

---

## Tipos de pruebas de seguridad

### Vulnerability Scanning

El escaneo de vulnerabilidades utiliza herramientas automatizadas para identificar debilidades conocidas.

Un escáner puede detectar:

- Software desactualizado.
- Cabeceras de seguridad ausentes.
- Configuración TLS débil.
- Indicadores comunes de SQL injection.
- Indicadores comunes de XSS.
- Directorios expuestos.
- Errores de configuración.

El escaneo automatizado es útil para tener una cobertura amplia, pero los resultados deben verificarse manualmente, porque puede generar falsos positivos y falsos negativos.

---

### Penetration Testing

El pentesting de aplicaciones web simula ataques reales de forma controlada y autorizada.

Un pentester intenta:

- Identificar vulnerabilidades.
- Validar si pueden explotarse.
- Determinar el impacto de una explotación exitosa.
- Evaluar los controles de seguridad existentes.
- Identificar rutas hacia datos sensibles o funciones privilegiadas.

El pentesting puede incluir explotación controlada, pero siempre debe mantenerse dentro del alcance y las reglas de compromiso acordadas.

---

### Revisión de código y análisis estático

La revisión de código examina el código fuente de la aplicación para encontrar problemas de seguridad antes del despliegue.

El análisis estático de seguridad, o **SAST**, analiza el código fuente sin ejecutar la aplicación.

Problemas comunes encontrados mediante revisión de código:

- Credenciales hardcodeadas.
- Consultas de base de datos inseguras.
- Criptografía insegura.
- Falta de validación de entrada.
- Manejo inseguro de archivos.
- Uso de funciones peligrosas.
- Controles de autorización débiles.

---

### Análisis dinámico

El análisis dinámico de seguridad, o **DAST**, prueba una aplicación en ejecución desde el exterior.

Las herramientas DAST interactúan con la aplicación mediante solicitudes y respuestas HTTP.

Pueden detectar:

- XSS reflejado.
- Indicadores de SQL injection.
- Cabeceras de seguridad ausentes.
- Cookies inseguras.
- Información expuesta por el servidor.
- Métodos HTTP débiles.
- Redirecciones inseguras.

---

### Análisis interactivo

El análisis interactivo de seguridad, o **IAST**, analiza una aplicación mientras está en ejecución.

IAST combina elementos de análisis estático y dinámico al monitorizar cómo se comporta la aplicación durante las pruebas.

---

### Software Composition Analysis

El análisis de composición de software, o **SCA**, identifica librerías, dependencias y componentes de terceros que contienen vulnerabilidades conocidas.

Esto es importante porque las aplicaciones web modernas dependen mucho de paquetes open source y librerías externas.

---

## Pruebas de autenticación y autorización

Las pruebas de autenticación evalúan si los usuarios pueden demostrar su identidad de forma segura.

Las pruebas de autorización evalúan si los usuarios acceden únicamente a las funciones y datos que tienen permitidos.

Comprobaciones importantes:

- Políticas débiles de contraseña.
- Credenciales por defecto.
- Falta de autenticación multifactor.
- Procesos inseguros de recuperación de contraseñas.
- Enumeración de usuarios.
- Protección contra fuerza bruta.
- Escalada de privilegios.
- IDOR.
- Broken access control.
- Falta de validación de roles.

---

## Pruebas de validación de entrada y salida

Estas pruebas evalúan cómo la aplicación gestiona los datos proporcionados por los usuarios.

El objetivo principal es identificar si la entrada de usuario puede modificar el comportamiento esperado de la aplicación.

Vulnerabilidades comunes:

- SQL injection.
- Cross-site scripting.
- Command injection.
- Server-side template injection.
- Path traversal.
- XML external entity injection.
- File inclusion.
- Deserialización insegura.

---

## Pruebas de gestión de sesiones

Las pruebas de gestión de sesiones evalúan cómo la aplicación maneja usuarios autenticados y tokens de sesión.

Problemas frecuentes:

- IDs de sesión predecibles.
- Session fixation.
- Secuestro de sesión.
- Tokens expuestos en URLs.
- Flags de cookies ausentes.
- Sesiones que no expiran.
- Sesiones que permanecen activas después del logout.
- Tokens de recuperación de contraseña reutilizables.

Atributos útiles para cookies:

```text
Secure     = la cookie solo se envía por HTTPS.
HttpOnly   = JavaScript no puede acceder a la cookie.
SameSite   = ayuda a reducir ataques entre sitios.
```

---

## Pruebas de seguridad de APIs

Las APIs permiten que aplicaciones web, móviles y servicios intercambien datos.

Las pruebas de seguridad de APIs deben evaluar:

- Mecanismos de autenticación.
- Comprobaciones de autorización.
- Validación de tokens.
- Rate limiting.
- Validación de entrada.
- Exposición de información sensible.
- Exposición excesiva de datos.
- Endpoints inseguros.
- Versionado de APIs.
- Mensajes de error.
- Logging y monitorización.

---

# Pentesting web vs pruebas de seguridad

| Aspecto | Pruebas de seguridad web | Pentesting de aplicaciones web |
|---|---|---|
| Objetivo | Identificar vulnerabilidades y debilidades | Validar vulnerabilidades mediante explotación controlada |
| Alcance | Amplio: código, configuración, infraestructura, dependencias y comportamiento en ejecución | Centrado en descubrir y explotar rutas de ataque realistas |
| Metodología | Pruebas manuales y automatizadas | Principalmente pruebas manuales apoyadas por herramientas |
| Explotación | Normalmente no explota vulnerabilidades | Usa explotación controlada cuando está autorizada |
| Impacto | Generalmente no intrusivo | Puede ser intrusivo y afectar a la disponibilidad |
| Reporte | Identifica problemas y recomienda correcciones | Documenta explotación exitosa, impacto y mitigación |
| Objetivo principal | Mejorar la postura de seguridad general | Validar defensas y capacidades de respuesta |

---

# Amenazas y riesgos

## Amenaza vs riesgo

Una **amenaza** es una fuente potencial de daño que puede explotar una vulnerabilidad.

Ejemplos de amenazas:

- Ciberdelincuentes.
- Insiders maliciosos.
- Campañas de phishing.
- Malware.
- Ataques de denegación de servicio.
- Desastres naturales.
- Cortes eléctricos.

Un **riesgo** es la posible pérdida o daño que puede producirse si una amenaza explota correctamente una vulnerabilidad.

El riesgo suele evaluarse mediante:

```text
Riesgo = Probabilidad × Impacto
```

- **Probabilidad:** Qué tan probable es que ocurra el evento.
- **Impacto:** Qué tan graves serían las consecuencias.

Una amenaza puede existir sin representar un riesgo alto si los controles de seguridad reducen la probabilidad o el impacto.

---

## Amenazas web comunes

### Cross-Site Scripting

Cross-Site Scripting, o **XSS**, ocurre cuando un atacante inyecta JavaScript malicioso en una página web vista por otros usuarios.

Posible impacto:

- Robo de cookies de sesión.
- Secuestro de cuentas.
- Manipulación del navegador.
- Robo de credenciales.
- Modificación de contenido.
- Redirecciones maliciosas.

Tipos principales de XSS:

- XSS reflejado.
- XSS almacenado.
- XSS basado en DOM.

---

### SQL Injection

SQL injection, o **SQLi**, ocurre cuando un atacante manipula la entrada de una aplicación para inyectar consultas SQL maliciosas en una base de datos.

Posible impacto:

- Acceso no autorizado a datos.
- Filtración de información.
- Modificación de datos.
- Bypass de autenticación.
- Compromiso de la base de datos.
- Eliminación de registros.

Métodos principales de prevención:

- Consultas parametrizadas.
- Prepared statements.
- Validación de entrada.
- Cuentas de base de datos con mínimo privilegio.
- Manejo seguro de errores.

---

### Cross-Site Request Forgery

Cross-Site Request Forgery, o **CSRF**, engaña a un usuario autenticado para que realice una acción no deseada mediante su sesión activa.

Posible impacto:

- Cambiar datos de cuenta.
- Cambiar direcciones de correo.
- Iniciar transacciones.
- Modificar contraseñas.
- Realizar acciones privilegiadas.

Protecciones comunes:

- Tokens anti-CSRF.
- Cookies SameSite.
- Reautenticación para acciones sensibles.
- Validación de Origin y Referer.

---

### Errores de configuración

Los errores de configuración ocurren cuando servidores, aplicaciones, bases de datos, servicios cloud o frameworks se configuran de forma insegura.

Ejemplos:

- Credenciales por defecto.
- Modo debug habilitado.
- Directory listing habilitado.
- Paneles administrativos expuestos.
- Servicios innecesarios.
- Páginas de error por defecto.
- Mensajes de error detallados.
- Cabeceras de seguridad ausentes.
- Permisos excesivos.

---

### Exposición de datos sensibles

La exposición de datos sensibles ocurre cuando la información confidencial no está protegida adecuadamente.

Ejemplos:

- Contraseñas almacenadas en texto claro.
- Cifrado débil.
- Tráfico HTTP sin cifrar.
- Datos sensibles en logs.
- Backups expuestos.
- APIs que devuelven datos excesivos.
- Credenciales subidas a repositorios de código.

---

### Fuerza bruta y credential stuffing

Un ataque de fuerza bruta prueba muchas contraseñas posibles contra una cuenta.

El credential stuffing utiliza combinaciones de usuario y contraseña filtradas previamente contra otros servicios, aprovechando la reutilización de contraseñas.

Defensas:

- Autenticación multifactor.
- Rate limiting.
- Políticas de bloqueo de cuentas.
- CAPTCHA cuando sea apropiado.
- Monitorizar logins fallidos.
- Gestores de contraseñas.
- Detección de actividad de login anómala.

---

### Vulnerabilidades de subida de archivos

Las subidas de archivos inseguras pueden permitir que un atacante suba archivos maliciosos o peligrosos.

Posible impacto:

- Ejecución remota de código.
- Distribución de malware.
- Compromiso del servidor.
- XSS almacenado.
- Path traversal.
- Denegación de servicio.

Prácticas seguras:

- Validar el tipo y contenido del archivo.
- Renombrar los archivos subidos.
- Guardar archivos fuera del web root.
- Restringir permisos de ejecución.
- Aplicar límites de tamaño.
- Analizar archivos subidos.
- Usar allowlists en lugar de blocklists.

---

### DoS y DDoS

Un ataque de denegación de servicio, o **DoS**, intenta volver una aplicación inaccesible agotando recursos.

Un ataque distribuido de denegación de servicio, o **DDoS**, utiliza muchos sistemas para saturar el objetivo.

Posible impacto:

- Caída de la aplicación.
- Pérdida de ingresos.
- Pérdida de productividad.
- Daño reputacional.
- Interrupción del servicio.

Defensas:

- Rate limiting.
- CDN.
- Balanceo de carga.
- Servicios de protección DDoS.
- Monitorización.
- Planificación de capacidad.

---

### Server-Side Request Forgery

Server-Side Request Forgery, o **SSRF**, permite que un atacante haga que el servidor envíe solicitudes a recursos internos o externos.

Posible impacto:

- Acceso a servicios internos.
- Exposición de metadatos cloud.
- Escaneo de red interno.
- Robo de información.
- Bypass de restricciones de red.

Defensas:

- Allowlists estrictas para solicitudes salientes.
- Bloquear acceso a rangos IP privados cuando no sea necesario.
- Validar URLs y protocolos.
- Segmentar redes internas.
- Restringir permisos de red del servidor web.

---

### Broken Access Control

Broken access control ocurre cuando los usuarios pueden acceder a funciones o datos fuera de sus permisos autorizados.

Ejemplos:

- Acceder al perfil de otro usuario cambiando un ID en una URL.
- Ver páginas administrativas como usuario normal.
- Descargar documentos de otra cuenta.
- Acceder a APIs sin comprobaciones de autorización.
- Modificar objetos pertenecientes a otros usuarios.

Esta es una de las áreas más importantes que deben probarse durante un pentesting web.

---

### Componentes vulnerables

El uso de componentes con vulnerabilidades conocidas introduce riesgos en una aplicación web.

Los componentes afectados pueden incluir:

- Frameworks.
- CMS.
- Plugins.
- Librerías JavaScript.
- Software de servidor.
- Dependencias API.
- Imágenes de contenedores.

La mitigación incluye mantener un inventario de dependencias, monitorizar avisos de seguridad y aplicar parches rápidamente.

---

# Metodología de pentesting web

## 1. Definir alcance y reglas

Antes de probar, confirma:

- Dominios e IPs objetivo.
- Cuentas de prueba permitidas.
- Fechas y franjas horarias.
- Técnicas permitidas.
- Sistemas fuera de alcance.
- Funciones sensibles que no deben interrumpirse.
- Requisitos de gestión de datos.
- Contactos de emergencia.
- Requisitos de reporte.

---

## 2. Recopilación de información

Recopila información sobre la aplicación antes de probarla.

Identifica:

- Dominios y subdominios.
- Tecnologías web.
- Frameworks.
- Cabeceras del servidor.
- Directorios y endpoints.
- Páginas de login.
- APIs.
- Archivos JavaScript.
- Parámetros.
- Formularios.
- Cookies.
- Roles de usuario.
- Integraciones de terceros.

---

## 3. Mapeo de superficie de ataque

Mapea toda la funcionalidad accesible.

Presta atención a:

- Páginas de autenticación.
- Páginas de registro.
- Restablecimiento de contraseña.
- Ajustes de cuenta.
- Subida de archivos.
- Formularios de búsqueda.
- Carritos de compra.
- Funciones administrativas.
- Endpoints API.
- Flujos de pago.
- Contenido generado por usuarios.
- Métodos HTTP.
- Parámetros ocultos.

---

## 4. Pruebas de vulnerabilidades

Prueba la superficie de ataque identificada buscando debilidades comunes.

Categorías habituales:

- Fallos de autenticación.
- Fallos de autorización.
- Problemas de gestión de sesiones.
- Problemas de validación de entrada.
- Problemas de subida de archivos.
- Debilidades en APIs.
- Errores de configuración.
- Fallos de lógica de negocio.
- Exposición de datos sensibles.
- Componentes vulnerables.

---

## 5. Explotación controlada

Cuando las reglas de compromiso lo permitan, valida vulnerabilidades importantes mediante explotación controlada.

El objetivo es demostrar impacto minimizando el riesgo.

Ejemplos de validación:

- Confirmar acceso a datos de otro usuario de prueba.
- Demostrar un bypass de autorización limitado.
- Probar que se exponen datos sensibles.
- Mostrar que una restricción de subida de archivos puede evitarse en un laboratorio seguro.
- Confirmar una versión vulnerable de un componente.

Evita acciones destructivas salvo autorización específica.

---

## 6. Reporte

Un buen reporte de pentesting web debe incluir:

- Resumen ejecutivo.
- Alcance y metodología.
- Vulnerabilidades identificadas.
- Severidad e impacto de negocio.
- Evidencias de los hallazgos.
- URLs, endpoints o componentes afectados.
- Pasos de reproducción.
- Recomendaciones de mitigación.
- Clasificación de riesgo.
- Resultados de retesting, si están disponibles.

---

## 7. Remediación y retesting

Después de corregir las vulnerabilidades, realiza pruebas de nuevo para verificar que:

- El problema se resolvió.
- La corrección no introdujo nuevas vulnerabilidades.
- Los controles de seguridad funcionan correctamente.
- La prueba de concepto original ya no funciona.

---

# Puntos Clave

- Las aplicaciones web son aplicaciones cliente-servidor accesibles mediante navegadores.
- La seguridad web protege la confidencialidad, integridad y disponibilidad.
- HTTP es sin estado, por lo que las aplicaciones deben gestionar las sesiones de forma segura.
- Las pruebas de seguridad identifican debilidades, mientras que el pentesting valida vulnerabilidades mediante explotación controlada.
- Las amenazas comunes incluyen XSS, SQLi, CSRF, SSRF, broken access control, subidas de archivos inseguras, errores de configuración y DDoS.
- Desarrollo seguro, autenticación robusta, validación de entrada, sesiones seguras, parcheado, mínimo privilegio y pruebas continuas son esenciales.
- La seguridad de aplicaciones web es un proceso continuo, no una actividad puntual.
