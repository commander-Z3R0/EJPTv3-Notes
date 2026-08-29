# Searchsploit Cheat Sheet

## Overview

Searchsploit es una herramienta de búsqueda por línea de comandos para Exploit-DB, utilizada para:

- Buscar exploits, shellcodes y papers.
- Encontrar vulnerabilidades por CVE, EDB-ID o palabra clave.
- Copiar exploits al directorio actual.
- Mostrar información detallada sobre exploits.
- Llevar a cabo evaluaciones de seguridad autorizadas.

Searchsploit proporciona:

- Acceso offline al repositorio de Exploit-DB.
- Capacidades de búsqueda rápidas.
- Integración con Metasploit.
- Soporte para múltiples filtros de búsqueda.
- Capacidad de copiar y ver código fuente de exploits.

```text
Usa Searchsploit únicamente para investigación y contra sistemas que poseas o tengas autorización explícita para testear.
```

---

# 1. Starting Searchsploit

## Sintaxis básica

```bash
searchsploit [options] <search-term>
```

## Mostrar ayuda

```bash
searchsploit -h
```

## Mostrar versión

```bash
searchsploit -V
```

## Mostrar versión verbose

```bash
searchsploit -v
```

## Actualizar la base de datos

```bash
searchsploit -u
```

Actualiza el repositorio local de Exploit-DB.

---

# 2. Basic Search

## Buscar por palabra clave

```bash
searchsploit <keyword>
```

Ejemplo:

```bash
searchsploit apache
```

## Buscar por CVE

```bash
searchsploit <CVE>
```

Ejemplo:

```bash
searchsploit CVE-2017-0144
```

## Buscar por EDB-ID

```bash
searchsploit <EDB-ID>
```

Ejemplo:

```bash
searchsploit 41937
```

## Buscar por autor

```bash
searchsploit --author <author-name>
```

Ejemplo:

```bash
searchsploit --author metasploit
```

## Buscar por tipo

Los tipos incluyen:

- `exploit`
- `shellcode`
- `papers`
- `webapps`
- `platform`
- `local`
- `remote`

```bash
searchsploit --type <type>
```

Ejemplo:

```bash
searchsploit --type exploit apache
```

## Buscar por plataforma

```bash
searchsploit --platform <platform>
```

Ejemplo:

```bash
searchsploit --platform linux
```

## Buscar por puerto

```bash
searchsploit --port <port>
```

Ejemplo:

```bash
searchsploit --port 445
```

## Combinar múltiples filtros

```bash
searchsploit --type exploit --platform windows --port 445 smb
```

---

# 3. Advanced Search Options

## Búsqueda case-insensitive

```bash
searchsploit -i <keyword>
```

Ejemplo:

```bash
searchsploit -i apache
```

## Búsqueda de coincidencia exacta

```bash
searchsploit -e <keyword>
```

Ejemplo:

```bash
searchsploit -e "apache 2.4.49"
```

## Mostrar solo el título

```bash
searchsploit -t <keyword>
```

Ejemplo:

```bash
searchsploit -t apache
```

## Mostrar solo la ruta

```bash
searchsploit -p <keyword>
```

Ejemplo:

```bash
searchsploit -p apache
```

## Mostrar solo el EDB-ID

```bash
searchsploit -n <keyword>
```

Ejemplo:

```bash
searchsploit -n apache
```

## Buscar en la descripción

```bash
searchsploit --search <keyword>
```

Ejemplo:

```bash
searchsploit --search "remote code execution"
```

## Listar todas las plataformas disponibles

```bash
searchsploit --platforms
```

## Listar todos los tipos disponibles

```bash
searchsploit --types
```

---

# 4. Viewing Exploit Details

## Mostrar información detallada

```bash
searchsploit -x <EDB-ID>
```

Ejemplo:

```bash
searchsploit -x 41937
```

## Mostrar información detallada por CVE

```bash
searchsploit -x <CVE>
```

Ejemplo:

```bash
searchsploit -x CVE-2017-0144
```

## Ver código fuente del exploit

```bash
searchsploit -x <EDB-ID>
```

Esto muestra el código fuente completo del exploit.

## Mostrar solo la ruta al exploit

```bash
searchsploit -p <keyword>
```

Ejemplo:

```bash
searchsploit -p eternalblue
```

---

# 5. Copying Exploits

## Copiar exploit al directorio actual

```bash
searchsploit -m <EDB-ID>
```

Ejemplo:

```bash
searchsploit -m 41937
```

## Copiar exploit a un directorio específico

```bash
searchsploit -m <EDB-ID> -o /path/to/output
```

Ejemplo:

```bash
searchsploit -m 41937 -o /tmp/exploits
```

## Copiar múltiples exploits

```bash
searchsploit -m <EDB-ID-1> <EDB-ID-2> <EDB-ID-3>
```

Ejemplo:

```bash
searchsploit -m 41937 42000 42050
```

## Copiar todos los exploits de una búsqueda

```bash
searchsploit -m <search-term>
```

Ejemplo:

```bash
searchsploit -m eternalblue
```

Copia todos los exploits coincidentes al directorio actual.

---

# 6. Metasploit Integration

## Buscar módulos de Metasploit

```bash
searchsploit --nmap <nmap-output.xml>
```

Analiza el output de Nmap y sugiere módulos de Metasploit.

## Buscar por nombre de módulo de Metasploit

```bash
searchsploit <module-name>
```

Ejemplo:

```bash
searchsploit ms17_010_eternalblue
```

## Mostrar ruta del módulo de Metasploit

```bash
searchsploit -p <module-name>
```

Ejemplo:

```bash
searchsploit -p ms17_010_eternalblue
```

## Comprobar si el exploit está disponible en Metasploit

Searchsploit indica si un exploit está disponible en Metasploit en la vista detallada.

---

# 7. Common Search Scenarios

## Buscar exploits de Apache

```bash
searchsploit apache
```

## Buscar exploits del kernel de Linux

```bash
searchsploit linux kernel
```

## Buscar exploits de Windows SMB

```bash
searchsploit windows smb
```

## Buscar EternalBlue

```bash
searchsploit eternalblue
```

## Buscar por CVE-2017-0144

```bash
searchsploit CVE-2017-0144
```

## Buscar shellcode

```bash
searchsploit --type shellcode
```

## Buscar exploits de aplicaciones web

```bash
searchsploit --type webapps
```

## Buscar escalada de privilegios local

```bash
searchsploit --type local
```

## Buscar exploits remotos

```bash
searchsploit --type remote
```

## Buscar exploits en el puerto 445

```bash
searchsploit --port 445
```

## Buscar exploits de WordPress

```bash
searchsploit wordpress
```

## Buscar exploits de SSH

```bash
searchsploit ssh
```

## Buscar exploits de FTP

```bash
searchsploit ftp
```

## Buscar exploits de MySQL

```bash
searchsploit mysql
```

---

# 8. Practical Workflows

## Flujo de trabajo básico de investigación de vulnerabilidades

```text
1. Identificar el servicio o software objetivo.
2. Buscar exploits usando Searchsploit.
3. Revisar detalles y requisitos del exploit.
4. Copiar el exploit a tu directorio de trabajo.
5. Analizar y modificar el exploit si es necesario.
6. Testear en un entorno de laboratorio controlado.
7. Documentar hallazgos.
```

## Ejemplo: Investigación de vulnerabilidad Apache

```bash
# Buscar exploits de Apache
searchsploit apache

# Ver información detallada
searchsploit -x 41937

# Copiar exploit al directorio actual
searchsploit -m 41937
```

## Ejemplo: Investigación basada en CVE

```bash
# Buscar por CVE
searchsploit CVE-2017-0144

# Ver detalles
searchsploit -x CVE-2017-0144

# Copiar exploit
searchsploit -m 41937
```

## Ejemplo: Búsqueda específica de plataforma

```bash
# Buscar exploits de Linux
searchsploit --platform linux

# Buscar exploits de Windows
searchsploit --platform windows
```

## Ejemplo: Búsqueda específica de tipo

```bash
# Buscar shellcode
searchsploit --type shellcode

# Buscar webapps
searchsploit --type webapps
```

## Ejemplo: Integración con Nmap

```bash
# Analizar output de Nmap
searchsploit --nmap scan.xml
```

Esto sugiere exploits relevantes basados en los servicios descubiertos.

---

# 9. Common Commands Reference

| Command | Description |
|---|---|
| `searchsploit -h` | Mostrar ayuda |
| `searchsploit -V` | Mostrar versión |
| `searchsploit -v` | Mostrar versión verbose |
| `searchsploit -u` | Actualizar base de datos |
| `searchsploit <keyword>` | Buscar por palabra clave |
| `searchsploit -i <keyword>` | Búsqueda case-insensitive |
| `searchsploit -e <keyword>` | Búsqueda de coincidencia exacta |
| `searchsploit -t <keyword>` | Mostrar solo títulos |
| `searchsploit -p <keyword>` | Mostrar solo rutas |
| `searchsploit -n <keyword>` | Mostrar solo EDB-IDs |
| `searchsploit -x <EDB-ID>` | Mostrar información detallada |
| `searchsploit -m <EDB-ID>` | Copiar exploit al directorio actual |
| `searchsploit --author <name>` | Buscar por autor |
| `searchsploit --type <type>` | Buscar por tipo |
| `searchsploit --platform <platform>` | Buscar por plataforma |
| `searchsploit --port <port>` | Buscar por puerto |
| `searchsploit --nmap <file>` | Analizar output de Nmap |
| `searchsploit --platforms` | Listar todas las plataformas |
| `searchsploit --types` | Listar todos los tipos |
| `searchsploit --search <term>` | Buscar en descripción |

---

# 10. Advanced Usage

## Buscar con múltiples filtros

```bash
searchsploit --type exploit --platform linux --port 22 ssh
```

## Buscar papers y documentación

```bash
searchsploit --type papers
```

## Buscar solo exploits locales

```bash
searchsploit --type local
```

## Buscar solo exploits remotos

```bash
searchsploit --type remote
```

## Buscar por versión específica de software

```bash
searchsploit "apache 2.4.49"
```

## Buscar exploits por año

Incluye el año en el término de búsqueda:

```bash
searchsploit "2021"
```

## Buscar investigación de 0-day

```bash
searchsploit --type exploit --platform linux
```

Revisa exploits recientes para patrones potenciales de 0-day.

## Combinar búsqueda con grep

```bash
searchsploit apache | grep "2.4"
```

## Exportar resultados de búsqueda

```bash
searchsploit apache > results.txt
```

---

# 11. Exploit-DB Integration

## Actualizar base de datos de Exploit-DB

```bash
searchsploit -u
```

Esto descarga los últimos exploits de Exploit-DB.

## Comprobar estado de la base de datos

```bash
searchsploit -v
```

Muestra la versión de la base de datos y última actualización.

## Actualizar manualmente Exploit-DB

```bash
cd /usr/share/exploitdb
git pull
```

## Buscar en Exploit-DB online

Visita:

```text
https://www.exploit-db.com/
```

Para búsqueda web y características adicionales.

---

# 12. Common Exploits and Scenarios

## MS17-010 EternalBlue

```bash
searchsploit eternalblue
searchsploit -x 41937
searchsploit -m 41937
```

## Apache Struts RCE

```bash
searchsploit apache struts
searchsploit -x <EDB-ID>
searchsploit -m <EDB-ID>
```

## Escalada de privilegios del kernel de Linux

```bash
searchsploit linux kernel privilege escalation
searchsploit --type local --platform linux
```

## Exploits de plugins de WordPress

```bash
searchsploit wordpress plugin
searchsploit --type webapps wordpress
```

## Vulnerabilidades de SSH

```bash
searchsploit ssh
searchsploit --port 22 ssh
```

## Vulnerabilidades de FTP

```bash
searchsploit ftp
searchsploit --port 21 ftp
```

## Vulnerabilidades de SMB

```bash
searchsploit smb
searchsploit --port 445 smb
```

## Vulnerabilidades de MySQL

```bash
searchsploit mysql
searchsploit --port 3306 mysql
```

## Vulnerabilidades de RDP

```bash
searchsploit rdp
searchsploit --port 3389 rdp
```

## Exploits de aplicaciones web

```bash
searchsploit --type webapps
searchsploit "sql injection"
searchsploit "xss"
```

---

# 13. Troubleshooting

## No se encuentran resultados

- Prueba diferentes palabras clave.
- Usa búsqueda case-insensitive: `-i`
- Busca directamente por CVE o EDB-ID.
- Actualiza la base de datos: `searchsploit -u`

## El exploit no funciona

- Comprueba los requisitos y la plataforma.
- Verifica que la versión objetivo coincide.
- Revisa el código fuente del exploit.
- Testea en un entorno de laboratorio controlado.

## Base de datos desactualizada

```bash
searchsploit -u
```

## Permission denied

Ejecuta con permisos apropiados:

```bash
sudo searchsploit -m <EDB-ID>
```

## Ruta del exploit no encontrada

Verifica que el exploit existe:

```bash
searchsploit -p <keyword>
```

---

# 14. Security Best Practices

## Verifica siempre el código del exploit

- Revisa el código fuente antes de ejecutar.
- Comprueba si hay payloads maliciosos.
- Entiende qué hace el exploit.
- Testea en un entorno aislado.

## Respeta los límites legales

- Usa exploits solo en sistemas que poseas.
- Obtén autorización explícita para testear.
- Sigue prácticas de divulgación responsable.
- Documenta todas las actividades de investigación.

## Mantén un entorno de laboratorio

- Usa máquinas virtuales para testear.
- Aísla las redes de test.
- Mantén snapshots de estados limpios.
- Documenta configuraciones y resultados.

## Mantén las herramientas actualizadas

- Actualiza regularmente Searchsploit.
- Actualiza la base de datos de Exploit-DB.
- Mantente informado sobre nuevas vulnerabilidades.
- Sigue noticias y advisories de seguridad.

---

# 15. Important Reminders

- Obtén siempre autorización explícita antes de usar exploits.
- Testea exploits primero en un entorno de laboratorio controlado.
- No todos los exploits son fiables; verifica antes de usar.
- Algunos exploits pueden crashear servicios o sistemas.
- Mantén Searchsploit y Exploit-DB actualizados regularmente.
- Valida los hallazgos manualmente; no confíes únicamente en resultados automatizados.
- Documenta todas las acciones, comandos y resultados.
- Preserva la evidencia original y los logs.
- Respeta el scope y las reglas del engagement.
- Entiende las implicaciones legales y éticas de tus acciones.

---

# 16. Quick Reference Examples

## Búsqueda básica

```bash
searchsploit apache
```

## Buscar por CVE

```bash
searchsploit CVE-2017-0144
```

## Ver detalles del exploit

```bash
searchsploit -x 41937
```

## Copiar exploit

```bash
searchsploit -m 41937
```

## Buscar exploits de Linux

```bash
searchsploit --platform linux
```

## Buscar shellcode

```bash
searchsploit --type shellcode
```

## Actualizar base de datos

```bash
searchsploit -u
```

## Búsqueda case-insensitive

```bash
searchsploit -i apache
```

## Búsqueda de coincidencia exacta

```bash
searchsploit -e "apache 2.4.49"
```

## Buscar por autor

```bash
searchsploit --author metasploit
```

## Analizar output de Nmap

```bash
searchsploit --nmap scan.xml
```

## Buscar con múltiples filtros

```bash
searchsploit --type exploit --platform windows --port 445 smb
```

---

# 17. Supported Search Filters

## Por tipo

- `exploit`
- `shellcode`
- `papers`
- `webapps`
- `platform`
- `local`
- `remote`

## Por plataforma

- `linux`
- `windows`
- `macos`
- `bsd`
- `android`
- `ios`
- `hardware`
- `php`
- `python`
- `ruby`
- `java`
- Y muchas más...

## Por puerto

Cualquier número de puerto válido:

- `21` (FTP)
- `22` (SSH)
- `80` (HTTP)
- `443` (HTTPS)
- `445` (SMB)
- `3306` (MySQL)
- `3389` (RDP)
- Y muchos más...

## Por CVE

Cualquier identificador CVE válido:

- `CVE-2017-0144`
- `CVE-2021-44228`
- `CVE-2020-1472`

## Por EDB-ID

Cualquier ID de Exploit-DB válido:

- `41937`
- `42000`
- `50000`

---

# 18. Additional Resources

## Exploit-DB

```text
https://www.exploit-db.com/
```

## Offensive Security

```text
https://www.offensive-security.com/
```

## Metasploit Framework

```text
https://www.metasploit.com/
```

## CVE Database

```text
https://cve.mitre.org/
```

## National Vulnerability Database

```text
https://nvd.nist.gov/
```
