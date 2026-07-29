# Armitage

## Armitage - GUI de MSF

Armitage es una interfaz gráfica de usuario (GUI) para Metasploit Framework. Hace que Metasploit sea más fácil de usar al ofrecer una forma visual de interactuar con objetivos, servicios, sesiones y exploits.

Armitage es especialmente útil para:

- Visualizar hosts y relaciones de red.
- Ejecutar tareas de escaneo y enumeración.
- Lanzar exploits desde una GUI.
- Administrar sesiones activas.
- Apoyar tareas de post-explotación como volcado de hashes, exploración de archivos y pivoting.

>  **Port Scanning & Enumeration With Armitage** — laboratorio de INE.

---

## Configuración De Armitage

### Iniciar Los Servicios Requeridos

Antes de abrir Armitage, Metasploit debe estar conectado a PostgreSQL.

```bash
service postgresql start && msfconsole -q
db_status
```

Si la base de datos está conectada correctamente, Metasploit debería mostrar que está conectado a PostgreSQL.

### Abrir Armitage

Abre Armitage desde una nueva terminal:

```bash
armitage
```

Cuando se te pregunte, responde **YES** para iniciar el servidor RPC.

### Por Qué Importa

Armitage depende de la base de datos de Metasploit y de los servicios RPC para gestionar correctamente hosts, servicios, loot y sesiones.

---

## Escaneo Y Enumeración De Puertos

### Añadir Objetivos

Una vez abierto Armitage, añade el host víctima manualmente:

- Abre **Hosts**.
- Selecciona **Add Hosts**.
- Añade la IP de la víctima.
- Asigna un nombre al host si lo necesitas, por ejemplo `Victim 1`.

### Escanear El Host

Haz clic derecho sobre el objetivo y selecciona **Scan**.  
También puedes ejecutar un escaneo de Nmap desde el menú **Hosts**.

Armitage mostrará los servicios descubiertos y los puertos abiertos en formato visual.

### Flujo Típico

```bash
# Añade el host desde la GUI y después escanéalo
```

### Qué Puedes Ver

Después del escaneo, Armitage puede mostrar:

- Puertos abiertos.
- Servicios en ejecución.
- Versiones detectadas.
- Relaciones entre servicios.

### Por Qué Importa

Armitage facilita la enumeración al presentar los resultados del escaneo de forma visual, en lugar de obligarte a revisarlo todo manualmente en la consola.

---

## Explotación Con Armitage

### Búsqueda De Exploits

Armitage puede buscar módulos relacionados con un servicio y lanzarlos directamente desde la interfaz.

Por ejemplo, si un objetivo ejecuta Rejetto HFS, puedes buscar `rejetto` y lanzar el módulo de exploit correspondiente.

### Ejemplo Típico

Un servicio HFS vulnerable puede explotarse desde Armitage seleccionando el host, eligiendo el servicio detectado y lanzando el módulo de Metasploit correspondiente.

### Por Qué Importa

Este flujo reduce la cantidad de interacción manual con Metasploit y hace que la explotación sea más rápida en laboratorios y evaluaciones.

---

## Post-explotación Con Armitage

### Volcado De Hashes

Armitage puede usarse para ejecutar acciones de post-explotación como el volcado de hashes de Windows.

Un ejemplo es usar el método de registro mediante el módulo `smart_hashdump`:

```bash
post/windows/gather/smart_hashdump
```

Los hashes guardados pueden encontrarse después en el menú **View > Loot**.

### Navegar Por Archivos

Después de comprometer un sistema, Armitage puede ayudarte a explorar archivos del sistema objetivo desde el contexto de la sesión.

### Ver Procesos

También puedes inspeccionar los procesos en ejecución para identificar objetivos útiles para migración o elevación de privilegios.

### Por Qué Importa

Armitage hace más accesibles las tareas comunes de post-explotación y te ayuda a pasar rápidamente del acceso al descubrimiento más profundo del sistema.

---

## Pivoting Con Armitage

### Qué Hace El Pivoting

Pivoting te permite usar un host comprometido para llegar a otros sistemas de la red interna que no son accesibles directamente desde tu máquina atacante.

### Flujo Típico

Después de comprometer `Victim 1`, puedes configurar el pivoting y añadir una ruta hacia la subred interna.

```bash
run autoroute -s <target_network>/24
```

Después puedes escanear `Victim 2` a través del host pivote.

### Port Forwarding

Si se detecta un servicio en el segundo host, puedes redirigir el puerto a través de la máquina comprometida:

```bash
portfwd add -l 1234 -p 80 -r <target_ip>
db_nmap -sV -p 1234 localhost
```

Esto te permite inspeccionar el servicio remoto como si estuviera corriendo localmente en `127.0.0.1:1234`.

### Por Qué Importa

Pivoting es una de las funciones más potentes de Armitage porque te ayuda a extender el acceso más allá del primer host comprometido y explorar más profundamente la red.

---

## Explotando Un Segundo Host

### Flujo De Ejemplo

Después de descubrir servicios en `Victim 2`, puedes buscar exploits compatibles y lanzarlos desde Armitage o Metasploit.

Si el objetivo es vulnerable, también puedes necesitar:

- Migrar a un proceso estable.
- Renombrar la sesión para mayor claridad.
- Usar el tipo de payload correcto para la arquitectura.

### Acciones De Seguimiento Típicas

```bash
sessions -n victim-2 -i 2
```

### Por Qué Importa

Renombrar sesiones y organizar la cadena de compromiso resulta muy útil cuando empiezas a trabajar con varios sistemas al mismo tiempo.

---

## Armitage En Kali Linux

### Instalación

Si Armitage no está disponible, puedes instalarlo en Kali y preparar la base de datos:

```bash
sudo apt install armitage -y
sudo msfdb init
sudo nano /etc/postgresql/15/main/pg_hba.conf
sudo systemctl enable postgresql
sudo systemctl restart postgresql
sudo armitage
```

### Nota Sobre La Autenticación En La Base De Datos

Si Armitage no puede conectarse correctamente, puede que tengas que ajustar la autenticación de PostgreSQL en `pg_hba.conf` para permitir la conexión local.

### Por Qué Importa

Armitage está muy ligado a los servicios backend de Metasploit, así que la base de datos y la configuración RPC deben estar correctas antes de que la GUI funcione bien.

---

## Por Qué Importa

Armitage ofrece una forma visual de trabajar con Metasploit, lo que hace más fácil gestionar escaneo, explotación, sesiones, revisión de loot y pivoting. Es especialmente útil en laboratorios donde quieres moverte rápidamente entre el reconocimiento y la post-explotación.

## Puntos Clave

- **Armitage** es una GUI para Metasploit que simplifica el escaneo, la explotación y la gestión de sesiones.
- Requiere que **PostgreSQL** y Metasploit estén ejecutándose correctamente.
- Puedes añadir hosts, escanearlos y lanzar exploits directamente desde la GUI.
- **Loot**, la exploración de archivos y la visualización de procesos están disponibles después del compromiso.
- El **pivoting** con `autoroute` y `portfwd` te permite alcanzar sistemas internos a través de un host comprometido.
- En Kali, Armitage puede requerir configuración de la base de datos y ajustes de autenticación de PostgreSQL antes de funcionar correctamente.
