# Hydra Cheat Sheet

## Overview

Hydra es una herramienta rápida y flexible de cracking de contraseñas online utilizada para:

- Realizar brute-force de credenciales de login.
- Testear políticas de contraseñas.
- Ejecutar ataques de diccionario.
- Validar mecanismos de autenticación.
- Llevar a cabo evaluaciones de seguridad autorizadas.

Hydra soporta:

- Múltiples protocolos (SSH, FTP, HTTP, SMB, MySQL, etc.).
- Conexiones paralelas para mayor velocidad.
- Wordlists y listas de usuarios personalizadas.
- Soporte de proxy.
- Guardar y resumir ataques.

```text
Usa Hydra únicamente contra sistemas que poseas o tengas autorización explícita para testear.
```

---

# 1. Starting Hydra

## Sintaxis básica

```bash
hydra [options] <target> <service>
```

## Mostrar ayuda

```bash
hydra -h
```

## Mostrar versión

```bash
hydra -V
```

## Mostrar protocolos soportados

```bash
hydra -h
```

Busca la lista de servicios soportados al final del output de ayuda.

---

# 2. Target Specification

## Especificar un único objetivo

```bash
hydra <target-IP> <service>
```

Ejemplo:

```bash
hydra 192.168.1.10 ssh
```

## Especificar múltiples objetivos

```bash
hydra -M targets.txt <service>
```

Donde `targets.txt` contiene una IP por línea.

## Especificar un puerto objetivo

```bash
hydra -s <port> <target-IP> <service>
```

Ejemplo:

```bash
hydra -s 2222 192.168.1.10 ssh
```

## Especificar objetivo vía URL

Para servicios HTTP/HTTPS:

```bash
hydra <url> <service>
```

Ejemplo:

```bash
hydra http://192.168.1.10/login http-get-form
```

---

# 3. Username and Password Lists

## Especificar un único username

```bash
hydra -l <username> <target-IP> <service>
```

Ejemplo:

```bash
hydra -l admin 192.168.1.10 ssh
```

## Especificar un único password

```bash
hydra -p <password> <target-IP> <service>
```

Ejemplo:

```bash
hydra -p password123 192.168.1.10 ssh
```

## Especificar una lista de usernames

```bash
hydra -L <userlist.txt> <target-IP> <service>
```

Ejemplo:

```bash
hydra -L users.txt 192.168.1.10 ssh
```

## Especificar una lista de passwords

```bash
hydra -P <passlist.txt> <target-IP> <service>
```

Ejemplo:

```bash
hydra -P passwords.txt 192.168.1.10 ssh
```

## Especificar ambas listas de usuario y password

```bash
hydra -L users.txt -P passwords.txt <target-IP> <service>
```

## Usar una lista combinada (user:pass)

```bash
hydra -C combo.txt <target-IP> <service>
```

Donde `combo.txt` contiene líneas en formato `username:password`.

## Generar usernames dinámicamente

Para algunos servicios, Hydra puede generar usernames:

```bash
hydra -L users.txt -P passwords.txt <target-IP> <service>
```

---

# 4. Connection and Performance Options

## Establecer número de tareas paralelas

```bash
hydra -t <tasks> <target-IP> <service>
```

Ejemplo:

```bash
hydra -t 16 192.168.1.10 ssh
```

El valor por defecto es 16 tareas.

## Establecer número de conexiones paralelas por objetivo

```bash
hydra -c <connections> <target-IP> <service>
```

Ejemplo:

```bash
hydra -c 4 192.168.1.10 ssh
```

## Establecer timeout para conexiones

```bash
hydra -w <seconds> <target-IP> <service>
```

Ejemplo:

```bash
hydra -w 30 192.168.1.10 ssh
```

## Establecer máximo número de reintentos

```bash
hydra -r <retries> <target-IP> <service>
```

Ejemplo:

```bash
hydra -r 3 192.168.1.10 ssh
```

## Tiempo de espera entre intentos

```bash
hydra -d <delay> <target-IP> <service>
```

Ejemplo:

```bash
hydra -d 1 192.168.1.10 ssh
```

Añade un delay de 1 segundo entre intentos.

## Salir después de encontrar la primera credencial válida

```bash
hydra -f <target-IP> <service>
```

Se detiene después del primer login exitoso.

## Salir después de encontrar credenciales válidas por usuario

```bash
hydra -F <target-IP> <service>
```

Se detiene después de encontrar una credencial válida por username.

---

# 5. Output and Logging

## Mostrar output verbose

```bash
hydra -v <target-IP> <service>
```

## Mostrar output muy verbose

```bash
hydra -d <target-IP> <service>
```

## Guardar output en un archivo

```bash
hydra -o output.txt <target-IP> <service>
```

## Guardar en un formato específico

```bash
hydra -o output.txt -oN <target-IP> <service>
```

Formatos:

- `-oN` — Texto normal.
- `-oJ` — JSON.
- `-oX` — XML.

Ejemplo:

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 ssh -oJ results.json
```

## Mostrar credenciales encontradas en tiempo real

Hydra muestra las credenciales encontradas automáticamente en modo verbose.

---

# 6. Protocol-Specific Options

## SSH

```bash
hydra -L users.txt -P passwords.txt <target-IP> ssh
```

Ejemplo:

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 ssh
```

## FTP

```bash
hydra -L users.txt -P passwords.txt <target-IP> ftp
```

## HTTP Basic Auth

```bash
hydra -L users.txt -P passwords.txt <target-IP> http-get
```

## HTTP Form-based Auth

```bash
hydra -L users.txt -P passwords.txt <target-IP> http-post-form "<path>:<parameters>:<fail_string>"
```

Ejemplo:

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 http-post-form "/login.php:user=^USER^&pass=^PASS^:Invalid"
```

- `/login.php` — Ruta de la página de login.
- `user=^USER^&pass=^PASS^` — Parámetros POST.
- `Invalid` — String que indica fallo.

## HTTPS

```bash
hydra -L users.txt -P passwords.txt <target-IP> https-get
```

## SMB

```bash
hydra -L users.txt -P passwords.txt <target-IP> smb
```

## MySQL

```bash
hydra -L users.txt -P passwords.txt <target-IP> mysql
```

## PostgreSQL

```bash
hydra -L users.txt -P passwords.txt <target-IP> postgres
```

## RDP

```bash
hydra -L users.txt -P passwords.txt <target-IP> rdp
```

## Telnet

```bash
hydra -L users.txt -P passwords.txt <target-IP> telnet
```

## SMTP

```bash
hydra -L users.txt -P passwords.txt <target-IP> smtp
```

## IMAP

```bash
hydra -L users.txt -P passwords.txt <target-IP> imap
```

## POP3

```bash
hydra -L users.txt -P passwords.txt <target-IP> pop3
```

## LDAP

```bash
hydra -L users.txt -P passwords.txt <target-IP> ldap
```

## VNC

```bash
hydra -L users.txt -P passwords.txt <target-IP> vnc
```

---

# 7. Advanced Options

## Usar un proxy

```bash
hydra -p <password> -P <passlist.txt> -X <proxy> <target-IP> <service>
```

Ejemplo:

```bash
hydra -L users.txt -P passwords.txt -X socks4://127.0.0.1:9050 192.168.1.10 ssh
```

## Usar SSL/TLS

Para servicios que soportan SSL:

```bash
hydra -s <port> -S <target-IP> <service>
```

Ejemplo:

```bash
hydra -s 443 -S 192.168.1.10 https-get
```

## Especificar opciones de módulo

Algunos módulos soportan opciones adicionales:

```bash
hydra -m <options> <target-IP> <service>
```

Ejemplo para HTTP GET:

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 http-get -m /admin
```

## Resumir un ataque previo

```bash
hydra -r <session.restore>
```

Hydra crea automáticamente un archivo de sesión cuando se interrumpe.

## Guardar sesión para resumir después

Hydra guarda la sesión automáticamente al interrumpir (Ctrl+C).

## Ignorar sesión existente

```bash
hydra -I <target-IP> <service>
```

Ignora cualquier archivo de restore existente.

---

# 8. Common Attack Scenarios

## SSH brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 ssh
```

## SSH con un único username

```bash
hydra -l admin -P passwords.txt 192.168.1.10 ssh
```

## SSH con puerto personalizado

```bash
hydra -s 2222 -L users.txt -P passwords.txt 192.168.1.10 ssh
```

## FTP brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 ftp
```

## HTTP Basic Auth brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 http-get
```

## HTTP Form-based login

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 http-post-form "/login.php:user=^USER^&pass=^PASS^:Invalid"
```

## SMB brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 smb
```

## MySQL brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 mysql
```

## RDP brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 rdp
```

## Telnet brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 telnet
```

## SMTP brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 smtp
```

## VNC brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 vnc
```

---

# 9. HTTP Form Attacks

## HTTP POST form con parámetros personalizados

```bash
hydra -L users.txt -P passwords.txt <target-IP> http-post-form "<path>:<parameters>:<fail_string>"
```

Ejemplo:

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 http-post-form "/login.php:username=^USER^&password=^PASS^:Login failed"
```

## HTTP POST form con cookies

```bash
hydra -L users.txt -P passwords.txt <target-IP> http-post-form "<path>:<parameters>:<fail_string>:<headers>"
```

Ejemplo:

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 http-post-form "/login.php:user=^USER^&pass=^PASS^:Invalid:Cookie: session=abc123"
```

## HTTP GET form

```bash
hydra -L users.txt -P passwords.txt <target-IP> http-get-form "<path>:<parameters>:<fail_string>"
```

Ejemplo:

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 http-get-form "/login.php?user=^USER^&pass=^PASS^:Invalid"
```

## HTTP form con string de éxito

Usa `F=` para string de fallo o `S=` para string de éxito:

```bash
hydra -L users.txt -P passwords.txt <target-IP> http-post-form "/login.php:user=^USER^&pass=^PASS^:S=Welcome"
```

## HTTP form con múltiples condiciones de fallo

```bash
hydra -L users.txt -P passwords.txt <target-IP> http-post-form "/login.php:user=^USER^&pass=^PASS^:F=Invalid:F=Error"
```

---

# 10. Practical Workflows

## Flujo de trabajo básico de SSH brute force

```text
1. Preparar lista de usernames (users.txt).
2. Preparar lista de passwords (passwords.txt).
3. Ejecutar Hydra contra SSH.
4. Revisar el output para credenciales válidas.
5. Validar credenciales manualmente.
6. Documentar hallazgos.
```

## Ejemplo: Ataque SSH completo

```bash
hydra -L users.txt -P passwords.txt -vV 192.168.1.10 ssh
```

- `-vV` — Output muy verbose.

## Ejemplo: Ataque HTTP form

```bash
hydra -L users.txt -P passwords.txt -vV 192.168.1.10 http-post-form "/login.php:user=^USER^&pass=^PASS^:Invalid"
```

## Ejemplo: Múltiples objetivos

```bash
hydra -M targets.txt -L users.txt -P passwords.txt ssh
```

Donde `targets.txt` contiene:

```text
192.168.1.10
192.168.1.11
192.168.1.12
```

## Ejemplo: Detener después del primer éxito

```bash
hydra -L users.txt -P passwords.txt -f 192.168.1.10 ssh
```

## Ejemplo: Puerto personalizado y timeout

```bash
hydra -s 2222 -w 30 -L users.txt -P passwords.txt 192.168.1.10 ssh
```

---

# 11. Common Commands Reference

| Command | Description |
|---|---|
| `hydra -h` | Mostrar ayuda |
| `hydra -V` | Mostrar versión |
| `hydra -l <user>` | Especificar un único username |
| `hydra -L <file>` | Especificar lista de usernames |
| `hydra -p <pass>` | Especificar un único password |
| `hydra -P <file>` | Especificar lista de passwords |
| `hydra -C <file>` | Especificar archivo combo (user:pass) |
| `hydra -t <tasks>` | Establecer número de tareas paralelas |
| `hydra -s <port>` | Especificar puerto objetivo |
| `hydra -M <file>` | Especificar lista de objetivos |
| `hydra -o <file>` | Guardar output en archivo |
| `hydra -v` | Output verbose |
| `hydra -V` | Output muy verbose |
| `hydra -f` | Salir después de la primera credencial válida |
| `hydra -F` | Salir después de credencial válida por usuario |
| `hydra -w <seconds>` | Establecer timeout de conexión |
| `hydra -r <retries>` | Establecer máximo número de reintentos |
| `hydra -d <delay>` | Establecer delay entre intentos |
| `hydra -I` | Ignorar sesión existente |
| `hydra -X <proxy>` | Usar proxy |
| `hydra -S` | Usar SSL |
| `hydra -m <options>` | Opciones específicas del módulo |

---

# 12. Wordlist Tips

## Ubicaciones comunes de wordlists

- `/usr/share/wordlists/`
- `/usr/share/seclists/`
- `rockyou.txt`
- `common-passwords.txt`

## Crear una wordlist personalizada

```bash
echo "password123" >> passwords.txt
echo "admin123" >> passwords.txt
```

## Usar múltiples wordlists

```bash
cat list1.txt list2.txt > combined.txt
```

## Eliminar duplicados

```bash
sort passwords.txt | uniq > passwords_unique.txt
```

## Generar variaciones

Usa herramientas como `crunch` para generar wordlists personalizadas:

```bash
crunch 8 12 -t password@@@ -o generated.txt
```

---

# 13. Common Exploits and Scenarios

## SSH con credenciales por defecto

```bash
hydra -l root -P passwords.txt 192.168.1.10 ssh
```

## FTP login anónimo

```bash
hydra -l anonymous -p anonymous 192.168.1.10 ftp
```

## HTTP admin panel

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 http-post-form "/admin/login.php:user=^USER^&pass=^PASS^:Invalid"
```

## MySQL acceso root

```bash
hydra -l root -P passwords.txt 192.168.1.10 mysql
```

## SMB acceso guest

```bash
hydra -l guest -P passwords.txt 192.168.1.10 smb
```

## RDP administrator

```bash
hydra -l Administrator -P passwords.txt 192.168.1.10 rdp
```

## Telnet root

```bash
hydra -l root -P passwords.txt 192.168.1.10 telnet
```

## SMTP authentication

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 smtp
```

---

# 14. Troubleshooting

## Connection refused

- Comprueba si el servicio está en ejecución.
- Verifica el número de puerto.
- Asegúrate de que el firewall permite conexiones.

## Too many errors

- Reduce tareas paralelas: `-t 4`
- Aumenta timeout: `-w 30`
- Añade delay: `-d 1`

## No valid credentials found

- Prueba una wordlist diferente.
- Comprueba si el servicio requiere parámetros especiales.
- Verifica el string de fallo para HTTP forms.

## Service not supported

- Comprueba protocolos soportados: `hydra -h`
- Algunos servicios pueden requerir módulos específicos.

## Session restore issues

- Ignora sesión existente: `hydra -I`
- Elimina archivo de restore manualmente.

---

# 15. Important Reminders

- Obtén siempre autorización explícita antes de usar Hydra.
- Testea primero en un entorno de laboratorio controlado.
- El brute-forcing puede bloquear cuentas o triggerar alertas.
- Respeta los límites de tasa y políticas de bloqueo de cuentas.
- Algunos servicios pueden detectar y bloquear intentos de brute-force.
- Documenta todas las acciones, comandos y resultados.
- Valida los hallazgos manualmente; no confíes únicamente en resultados automatizados.
- Preserva la evidencia original y los logs.
- Respeta el scope y las reglas del engagement.
- Entiende las implicaciones legales y éticas de tus acciones.

---

# 16. Quick Reference Examples

## SSH brute force

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 ssh
```

## HTTP form attack

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 http-post-form "/login.php:user=^USER^&pass=^PASS^:Invalid"
```

## FTP con un único usuario

```bash
hydra -l admin -P passwords.txt 192.168.1.10 ftp
```

## Múltiples objetivos

```bash
hydra -M targets.txt -L users.txt -P passwords.txt ssh
```

## Detener después del primer éxito

```bash
hydra -L users.txt -P passwords.txt -f 192.168.1.10 ssh
```

## Puerto personalizado y verbose

```bash
hydra -s 2222 -vV -L users.txt -P passwords.txt 192.168.1.10 ssh
```

## Guardar resultados en JSON

```bash
hydra -L users.txt -P passwords.txt 192.168.1.10 ssh -oJ results.json
```

## Usar proxy

```bash
hydra -L users.txt -P passwords.txt -X socks4://127.0.0.1:9050 192.168.1.10 ssh
```

---

# 17. Supported Protocols (Partial List)

- `ssh`
- `ftp`
- `http-get`
- `http-post-form`
- `https-get`
- `https-post-form`
- `smb`
- `mysql`
- `postgres`
- `rdp`
- `telnet`
- `smtp`
- `imap`
- `pop3`
- `ldap`
- `vnc`
- `snmp`
- `cisco`
- `oracle`
- `mssql`

Para una lista completa:

```bash
hydra -h
```
