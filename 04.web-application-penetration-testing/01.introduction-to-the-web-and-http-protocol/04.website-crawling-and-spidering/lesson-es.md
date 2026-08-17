# Website Crawling & Spidering

## Visión general

El **website crawling** y el **spidering** son técnicas utilizadas para descubrir y mapear el contenido accesible de una aplicación web.

El objetivo es identificar la superficie de ataque visible de la aplicación, incluyendo:

- Páginas y directorios.
- Archivos y recursos estáticos.
- Rutas URL.
- Parámetros.
- Formularios.
- Endpoints de API.
- Archivos JavaScript.
- Páginas de login.
- Paneles administrativos.
- Funciones de subida de archivos.
- Roles y flujos de usuario.

El crawling es una fase importante al inicio de una evaluación de seguridad web, ya que no puedes probar correctamente una aplicación si no entiendes qué funcionalidades existen.

### Regla importante

Solo debes hacer crawling sobre sitios web y aplicaciones que estén explícitamente dentro del alcance autorizado.

Define el alcance antes de empezar:

- Dominios objetivo.
- Subdominios.
- Direcciones IP.
- Puertos.
- Cuentas de usuario permitidas.
- Funcionalidades excluidas.
- Velocidad y límites del crawling.
- Requisitos de autenticación.

---

## Crawling vs Spidering

Los términos suelen usarse como sinónimos, pero pueden entenderse así:

| Término | Significado |
|---|---|
| Crawling | Descubrir contenido navegando por páginas, siguiendo enlaces, procesando formularios y observando solicitudes |
| Spidering | Seguir automáticamente enlaces y URLs descubiertas para identificar más contenido |
| Crawling pasivo | Mapear contenido a partir del tráfico y referencias sin enviar payloads intrusivos |
| Crawling activo | Solicitar automáticamente páginas y recursos descubiertos; puede generar más tráfico |
| Crawling manual | El pentester navega manualmente mientras un proxy registra las solicitudes |
| AJAX spidering | Recorrer aplicaciones cargadas con JavaScript renderizando páginas e interactuando con elementos dinámicos |

---

## Por qué importa el crawling

El crawling ayuda a crear un mapa de la aplicación objetivo.

Sin un mapa adecuado, es fácil pasar por alto:

- Directorios ocultos.
- Páginas sin enlaces visibles.
- Endpoints de API.
- Parámetros.
- Funciones administrativas.
- Rutas JavaScript.
- Flujos de autenticación.
- Funcionalidad específica para usuarios.
- Subidas de archivos.
- Funciones de recuperación de contraseña.
- Recursos sensibles.

### Ejemplo de mapa de superficie de ataque

```text
https://target.local/
|
+-- /
+-- /login
+-- /register
+-- /forgot-password
+-- /dashboard
+-- /profile
+-- /admin
+-- /uploads
+-- /api/
|   +-- /api/users
|   +-- /api/orders
|   +-- /api/profile
|
+-- /static/
    +-- /static/js/app.js
    +-- /static/css/style.css
```

---

# Crawling pasivo con Burp Suite

## ¿Qué es el crawling pasivo?

El crawling pasivo en Burp Suite mapea el contenido visible de una aplicación a medida que el tráfico del navegador pasa por Burp Proxy.

No requiere explotación activa. Burp observa:

- URLs visitadas en el navegador.
- Enlaces encontrados en respuestas.
- Formularios.
- Scripts.
- Recursos referenciados.
- Parámetros.
- Cookies.
- Métodos HTTP.
- Headers de solicitudes y respuestas.

El contenido descubierto se añade a **Target > Site map**.

---

## Configuración de Burp Suite

### 1. Configurar el alcance

Antes de navegar por el objetivo, añade el objetivo autorizado al scope de Burp.

```text
Target > Scope > Add
```

Añade el dominio, URL, dirección IP o puerto del objetivo.

Ejemplos:

```text
https://target.local
https://*.target.local
```

### Por qué importa el scope

El scope te ayuda a:

- Evitar enviar tráfico a sitios no relacionados.
- Mantener el Site map organizado.
- Enfocar el crawling pasivo en el objetivo.
- Evitar pruebas accidentales contra sistemas fuera de alcance.

---

## 2. Usar Burp Browser

Burp Suite incluye un navegador integrado que ya está configurado para usar Burp Proxy.

```text
Proxy > Intercept > Open Browser
```

Para crawling pasivo, normalmente es más cómodo desactivar la intercepción mientras navegas.

```text
Proxy > Intercept > Intercept is off
```

Esto permite que las solicitudes pasen por Burp sin detenerse una por una.

---

## 3. Navegar manualmente por la aplicación

Utiliza Burp Browser para explorar la aplicación web como lo haría un usuario normal.

Navega por:

- La página principal.
- Páginas de login y logout.
- Páginas de registro.
- Funciones de recuperación de contraseña.
- Perfiles de usuario.
- Formularios de búsqueda.
- Menús de navegación.
- Páginas de configuración.
- Formularios de subida de archivos.
- Enlaces de descarga.
- Páginas administrativas, si están autorizadas.
- APIs utilizadas por la aplicación.
- Páginas con contenido generado por usuarios.

### Flujo de crawling manual

```text
1. Abre el objetivo en Burp Browser.
2. Navega por todos los menús visibles.
3. Haz clic en enlaces relevantes.
4. Envía formularios no destructivos.
5. Inicia sesión con cuentas de prueba autorizadas.
6. Repite el proceso con otros roles de usuario.
7. Revisa el Site map.
```

Burp registra el tráfico y rellena el Site map con el contenido visitado. También puede inferir ubicaciones adicionales a partir de enlaces y formularios presentes en las respuestas. [122][126]

---

## 4. Revisar Target Site Map

Abre:

```text
Target > Site map
```

El Site map muestra la estructura descubierta de la aplicación.

Busca:

- Directorios interesantes.
- Endpoints de API.
- Archivos JavaScript.
- Parámetros.
- Métodos HTTP.
- Códigos de respuesta.
- Cookies.
- Redirecciones.
- Rutas ocultas o atenuadas.
- Endpoints administrativos.
- Ubicaciones de subida de archivos.
- Contenido diferente para distintos roles.

### Elementos útiles del Site Map

```text
/login
/logout
/register
/admin
/api
/api/v1/users
/uploads
/download
/profile?id=10
/search?q=test
/robots.txt
/sitemap.xml
```

Las entradas atenuadas pueden ser rutas inferidas desde las respuestas, pero que Burp todavía no ha solicitado. Puedes abrirlas manualmente en Burp Browser si están dentro del alcance.

---

## 5. Filtrar el Site Map

Usa los filtros del Site map para reducir ruido.

Filtros útiles:

- Mostrar solo elementos dentro del scope.
- Ocultar imágenes.
- Ocultar archivos CSS.
- Ocultar fuentes.
- Ocultar librerías JavaScript estáticas.
- Mostrar solo contenido dinámico.
- Mostrar solo elementos solicitados.
- Mostrar códigos HTTP específicos.

Esto permite enfocarte en endpoints que pueden contener funcionalidad relevante de la aplicación.

---

## Checklist de crawling con Burp Suite

- Añadir el objetivo al scope.
- Abrir Burp Browser.
- Desactivar la intercepción durante la navegación normal.
- Navegar por todas las páginas visibles.
- Seguir enlaces y menús.
- Enviar formularios seguros.
- Iniciar sesión con cuentas autorizadas.
- Probar distintos roles si están disponibles.
- Revisar `Target > Site map`.
- Identificar APIs y parámetros.
- Inspeccionar archivos JavaScript.
- Revisar endpoints ocultos o inferidos.
- Documentar la funcionalidad relevante.

---

# Crawling con Burp Suite Professional

Burp Suite Professional incluye funcionalidades de crawling automatizado.

### Iniciar un crawling automatizado

```text
Target > Site map
Clic derecho en la raíz del objetivo
Scan
Seleccionar Crawl
Start Scan
```

Puedes configurar credenciales de login si la aplicación requiere autenticación.

### Nota importante

Burp Suite Community Edition no incluye las mismas capacidades de crawling automatizado que Burp Suite Professional.

En Burp Community Edition, utiliza navegación manual mediante Burp Proxy para rellenar el Site map.

---

# Crawling pasivo con OWASP ZAP

## ¿Qué es OWASP ZAP?

OWASP ZAP, también conocido como **Zed Attack Proxy**, es una herramienta open source para pruebas de seguridad de aplicaciones web.

ZAP puede ayudar con:

- Interceptar tráfico HTTP/S.
- Passive scanning.
- Spidering tradicional.
- AJAX spidering.
- Exploración manual.
- Mapeo de sitios.
- Inspección de headers.
- Análisis de cookies.
- Descubrimiento de APIs.

---

## Passive Scanning en ZAP

El passive scanner de ZAP analiza mensajes HTTP y WebSocket que pasan por ZAP sin modificarlos.

Puede identificar problemas como:

- Security headers ausentes.
- Cookies sin atributos de seguridad.
- Divulgación de información.
- Datos sensibles en URLs.
- Exposición de banners de servidor.
- Configuración débil de Cache-Control.
- Falta de controles anti-clickjacking.
- Problemas de configuración CORS.

### Diferencia importante

```text
Passive scan = analiza tráfico existente sin enviar payloads de ataque.
Active scan  = envía payloads de ataque y puede afectar al objetivo.
```

No ejecutes active scans salvo que estén explícitamente autorizados.

---

## Configuración de ZAP

### 1. Crear una sesión

Inicia OWASP ZAP y crea o abre una sesión.

```text
File > New Session
```

Una sesión guarda:

- URLs descubiertas.
- Historial HTTP.
- Alertas.
- Resultados de spiders.
- Configuración de contextos.
- Configuración de autenticación.

---

## 2. Definir Context y Scope

Crea un contexto para la aplicación objetivo.

```text
Sites > Clic derecho en el objetivo
Include in Context > New Context
```

Añade el dominio o URL objetivo.

Ejemplos:

```text
https://target.local
https://.*\.target\.local.*
```

Después añade el objetivo al scope:

```text
Sites > Clic derecho en el objetivo
Include in Scope
```

### Por qué utilizar un Context

Un Context de ZAP permite definir:

- URLs dentro del alcance.
- Reglas de autenticación.
- Reglas de gestión de sesiones.
- Cuentas de usuario.
- Rutas excluidas.
- Restricciones de spidering.

---

## 3. Usar Manual Explore

Usa el navegador integrado o configura tu propio navegador para pasar el tráfico a través de ZAP.

```text
Quick Start > Manual Explore
```

Introduce la URL objetivo y abre el navegador.

Navega normalmente por la aplicación:

- Abre páginas.
- Sigue enlaces de navegación.
- Envía formularios no destructivos.
- Inicia sesión con cuentas autorizadas.
- Explora perfiles y configuraciones.
- Utiliza roles de usuario disponibles.
- Activa llamadas API.

ZAP registra las solicitudes en:

```text
History
Sites
```

La estructura de la aplicación se mostrará en el árbol **Sites**.

---

# Traditional Spider en OWASP ZAP

## ¿Qué es el Traditional Spider?

El Traditional Spider de ZAP solicita páginas automáticamente y analiza el HTML devuelto para descubrir:

- Enlaces.
- Formularios.
- Recursos.
- Parámetros.
- Rutas referenciadas.

Normalmente es rápido y funciona bien contra sitios HTML tradicionales.

### Iniciar Traditional Spider

```text
Sites > Clic derecho sobre el objetivo
Attack > Spider
```

También puedes usar:

```text
Tools > Spider
```

Configura:

- URL inicial.
- Context.
- Profundidad máxima.
- Número máximo de elementos hijos.
- Número de hilos.
- URLs excluidas.
- Configuración de recursividad.

Después, inicia el spider.

### Monitorizar resultados

Revisa los resultados en:

```text
Spider tab
Sites tree
History tab
```

---

## Limitaciones del Traditional Spider

El Traditional Spider analiza principalmente HTML devuelto por el servidor.

Puede tener dificultades para descubrir contenido de aplicaciones que dependen de:

- JavaScript.
- Frameworks de single-page applications.
- Cambios dinámicos del DOM.
- Client-side routing.
- Solicitudes AJAX.
- Botones que no usan enlaces convencionales.

Para aplicaciones que usan mucho JavaScript, utiliza AJAX Spider además del Traditional Spider.

---

# AJAX Spider en OWASP ZAP

## ¿Qué es AJAX Spider?

El **AJAX Spider** está diseñado para aplicaciones web modernas que dependen de JavaScript, AJAX, elementos dinámicos y renderizado client-side.

Utiliza el crawler Crawljax para renderizar páginas e interactuar con aplicaciones ricas en AJAX.

AJAX Spider puede descubrir páginas y estados que un spider HTML tradicional podría no encontrar.

### Cuándo utilizarlo

Usa AJAX Spider cuando la aplicación utilice:

- React.
- Angular.
- Vue.js.
- Single-page applications.
- Menús dinámicos.
- Botones JavaScript.
- Solicitudes AJAX.
- Client-side routing.
- Contenido cargado después de la respuesta inicial.

### Iniciar AJAX Spider

```text
Sites > Clic derecho en el objetivo
Attack > AJAX Spider
```

También puedes usar:

```text
Tools > AJAX Spider
```

Configura la URL objetivo y el Context, luego inicia el crawling.

### Nota importante

AJAX Spider es más lento que Traditional Spider porque renderiza páginas, ejecuta JavaScript e interactúa con contenido dinámico.

Para tener mayor cobertura, usa ambos:

```text
1. Navegación manual.
2. Traditional Spider.
3. AJAX Spider.
4. Passive scanning.
```

---

# Checklist de crawling con ZAP

- Crear una sesión nueva de ZAP.
- Definir el Context del objetivo.
- Añadir el objetivo al scope.
- Usar Manual Explore.
- Navegar por la aplicación con cuentas autorizadas.
- Revisar el árbol Sites.
- Ejecutar Traditional Spider.
- Ejecutar AJAX Spider para aplicaciones con JavaScript.
- Esperar a que termine el passive scanning.
- Revisar las alertas pasivas.
- Exportar o documentar endpoints descubiertos.
- No ejecutar active scans sin autorización.

---

# Burp Suite vs OWASP ZAP

| Característica | Burp Suite | OWASP ZAP |
|---|---|---|
| Licencia | Community Edition es gratuita; Professional Edition es comercial | Open source y gratuito |
| Crawling pasivo | Usa tráfico de Proxy y Site map | Usa tráfico proxied, árbol Sites y passive scanner |
| Navegación manual | Burp Browser y Proxy | Manual Explore y navegador con proxy |
| Crawling automatizado | Disponible en Burp Suite Professional | Traditional Spider y AJAX Spider disponibles |
| Crawling JavaScript | Disponible mediante funcionalidades de crawler en Burp Professional | AJAX Spider disponible |
| Passive scanning | Análisis pasivo básico y Site map | Passive scanner integrado y reglas pasivas |
| Active scanning | Función de Professional | Disponible, pero debe estar explícitamente autorizada |
| Vista principal de mapeo | Target > Site map | Árbol Sites e History |

---

# Objetivos útiles para descubrir

Durante crawling y spidering pasivo, busca:

```text
/login
/logout
/register
/forgot-password
/reset-password
/profile
/settings
/admin
/dashboard
/api
/api/v1/
/uploads
/downloads
/backups
/config
/robots.txt
/sitemap.xml
/.git
/.env
```

También identifica:

- Parámetros URL.
- Campos de formularios.
- Cookies.
- Tokens de sesión.
- Solicitudes API.
- Endpoints JavaScript.
- Roles de usuario.
- Métodos HTTP.
- Redirecciones.
- Subidas de archivos.
- Páginas de error.
- Integraciones de terceros.

---

# Flujo práctico

## Burp Suite Community Edition

```text
1. Abre Burp Suite.
2. Añade el objetivo en Target > Scope.
3. Abre Burp Browser.
4. Desactiva la intercepción para navegar normalmente.
5. Navega por toda la funcionalidad accesible.
6. Inicia sesión con cuentas autorizadas.
7. Revisa Target > Site map.
8. Investiga rutas y parámetros interesantes.
9. Inspecciona Proxy > HTTP history.
10. Documenta los endpoints descubiertos.
```

## OWASP ZAP

```text
1. Abre OWASP ZAP.
2. Crea una sesión nueva.
3. Crea un Context para el objetivo.
4. Añade el objetivo al scope.
5. Inicia Manual Explore.
6. Navega normalmente por la aplicación.
7. Revisa el árbol Sites y History.
8. Ejecuta Traditional Spider.
9. Ejecuta AJAX Spider si la aplicación utiliza mucho JavaScript.
10. Espera los resultados del passive scan.
11. Revisa las alertas y documenta los hallazgos.
```

---

# Puntos clave

- Crawling y spidering ayudan a mapear la superficie de ataque visible de una aplicación web.
- El crawling pasivo observa tráfico y contenido de respuestas sin utilizar payloads de ataque.
- Burp Suite rellena `Target > Site map` mientras el tráfico pasa por Burp Proxy.
- Burp Suite Community Edition depende principalmente de la navegación manual para el crawling.
- OWASP ZAP proporciona Manual Explore, Traditional Spider, AJAX Spider y passive scanning.
- Traditional Spider funciona bien contra aplicaciones HTML clásicas.
- AJAX Spider es útil para aplicaciones con mucho JavaScript y single-page applications.
- El passive scanning puede identificar problemas de seguridad sin atacar activamente al objetivo.
- Define siempre el scope, utiliza cuentas autorizadas y evita active scans salvo autorización explícita.
