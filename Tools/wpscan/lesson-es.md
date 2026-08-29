# WPScan Cheat Sheet

## Overview

WPScan es un escáner de seguridad de WordPress utilizado para:

- Enumerar instalaciones de WordPress.
- Identificar plugins y temas vulnerables.
- Detectar credenciales débiles.
- Encontrar problemas de configuración.
- Llevar a cabo evaluaciones de seguridad autorizadas.

WPScan proporciona:

- Base de datos completa de vulnerabilidades de WordPress.
- Enumeración de plugins y temas.
- Capacidades de enumeración de usuarios.
- Brute-force de contraseñas.
- Integración con API para datos de vulnerabilidades.

```text
Usa WPScan únicamente contra sistemas que poseas o tengas autorización explícita para testear.
```

---

# 1. Starting WPScan

## Sintaxis básica

```bash
wpscan [options] -u <URL>
```

## Mostrar ayuda

```bash
wpscan -h
```

## Mostrar versión

```bash
wpscan --version
```

## Actualizar WPScan

```bash
wpscan --update
```

Actualiza la herramienta y la base de datos de vulnerabilidades.

## Actualizar solo la base de datos de vulnerabilidades

```bash
wpscan --update-only
```

---

# 2. Target Specification

## Especificar URL objetivo

```bash
wpscan -u <URL>
```

Ejemplo:

```bash
wpscan -u http://192.168.1.10/
```

## Especificar URL objetivo con puerto

```bash
wpscan -u http://192.168.1.10:8080/
```

## Especificar URL objetivo con ruta

```bash
wpscan -u http://192.168.1.10/wordpress/
```

## Escanear múltiples URLs

```bash
wpscan -u <URL1> -u <URL2> -u <URL3>
```

Ejemplo:

```bash
wpscan -u http://192.168.1.10/ -u http://192.168.1.11/
```

## Escanear desde archivo

```bash
wpscan --url <file>
```

Donde el archivo contiene una URL por línea.

---

# 3. Authentication

## Especificar username

```bash
wpscan -u <URL> --username <username>
```

Ejemplo:

```bash
wpscan -u http://192.168.1.10/ --username admin
```

## Especificar contraseña

```bash
wpscan -u <URL> --password <password>
```

Ejemplo:

```bash
wpscan -u http://192.168.1.10/ --username admin --password password123
```

## Especificar cookie

```bash
wpscan -u <URL> --cookie <cookie>
```

Ejemplo:

```bash
wpscan -u http://192.168.1.10/ --cookie "wordpress_logged_in=abc123"
```

## Especificar User-Agent

```bash
wpscan -u <URL> --user-agent <agent>
```

Ejemplo:

```bash
wpscan -u http://192.168.1.10/ --user-agent "Mozilla/5.0"
```

## Especificar proxy

```bash
wpscan -u <URL> --proxy <proxy>
```

Ejemplo:

```bash
wpscan -u http://192.168.1.10/ --proxy http://127.0.0.1:8080
```

## Especificar autenticación de proxy

```bash
wpscan -u <URL> --proxy <proxy> --proxy-auth <credentials>
```

Ejemplo:

```bash
wpscan -u http://192.168.1.10/ --proxy http://127.0.0.1:8080 --proxy-auth user:pass
```

---

# 4. Enumeration Options

## Enumerar plugins

```bash
wpscan -u <URL> --enumerate p
```

## Enumerar temas

```bash
wpscan -u <URL> --enumerate t
```

## Enumerar usuarios

```bash
wpscan -u <URL> --enumerate u
```

## Enumerar todo

```bash
wpscan -u <URL> --enumerate a
```

Enumera plugins, temas, usuarios y más.

## Enumerar solo plugins vulnerables

```bash
wpscan -u <URL> --enumerate vp
```

## Enumerar solo temas vulnerables

```bash
wpscan -u <URL> --enumerate vt
```

## Enumerar plugins populares

```bash
wpscan -u <URL> --enumerate ap
```

## Enumerar temas populares

```bash
wpscan -u <URL> --enumerate at
```

## Enumerar timthumbs

```bash
wpscan -u <URL> --enumerate tt
```

## Enumerar backups de configuración

```bash
wpscan -u <URL> --enumerate cb
```

## Enumerar exports de base de datos

```bash
wpscan -u <URL> --enumerate db
```

## Limitar resultados de enumeración

```bash
wpscan -u <URL> --enumerate u[1-10]
```

Limita la enumeración de usuarios a los primeros 10 usuarios.

---

# 5. Vulnerability Detection

## Detectar todas las vulnerabilidades

```bash
wpscan -u <URL>
```

Realiza un escaneo completo de vulnerabilidades.

## Detectar plugins vulnerables

```bash
wpscan -u <URL> --enumerate vp
```

## Detectar temas vulnerables

```bash
wpscan -u <URL> --enumerate vt
```

## Detectar plugins y temas vulnerables

```bash
wpscan -u <URL> --enumerate vp,vt
```

## Forzar detección de todas las vulnerabilidades

```bash
wpscan -u <URL> --force
```

## Saltar chequeo de vulnerabilidades

```bash
wpscan -u <URL> --no-vulnerability-check
```

---

# 6. Password Attacks

## Brute-force de contraseñas

```bash
wpscan -u <URL> --passwords <wordlist>
```

Ejemplo:

```bash
wpscan -u http://192.168.1.10/ --passwords /usr/share/wordlists/rockyou.txt
```

## Brute-force de un username específico

```bash
wpscan -u <URL> --username <username> --passwords <wordlist>
```

Ejemplo:

```bash
wpscan -u http://192.168.1.10/ --username admin --passwords passwords.txt
```

## Brute-force de múltiples usernames

```bash
wpscan -u <URL> --usernames <userlist> --passwords <wordlist>
```

Ejemplo:

```bash
wpscan -u http://192.168.1.10/ --usernames users.txt --passwords passwords.txt
```

## Limitar intentos de contraseña

```bash
wpscan -u <URL> --passwords <wordlist> --max-threads <threads>
```

Ejemplo:

```bash
wpscan -u http://192.168.1.10/ --passwords passwords.txt --max-threads 10
```

## Detener después del primer éxito

```bash
wpscan -u <URL> --passwords <wordlist> --stop-on-success
```

---

# 7. API Integration

## Especificar token de API

```bash
wpscan -u <URL> --api-token <token>
```

Ejemplo:

```bash
wpscan -u http://192.168.1.10/ --api-token abc123xyz
```

## Usar token de API para datos de vulnerabilidades

```bash
wpscan -u <URL> --api-token <token> --enumerate vp
```

## Actualizar token de API

```bash
wpscan --update
```

## Comprobar estado de la API

```bash
wpscan --api-token <token>
```

---

# 8. Output and Logging

## Guardar output en archivo

```bash
wpscan -u <URL> -o <output-file>
```

Ejemplo:

```bash
wpscan -u http://192.168.1.10/ -o results.txt
```

## Guardar en formato JSON

```bash
wpscan -u <URL> -f json -o <output-file>
```

Ejemplo:

```bash
wpscan -u http://192.168.1.10/ -f json -o results.json
```

## Guardar en formato CSV

```bash
wpscan -u <URL> -f csv -o <output-file>
```

Ejemplo:

```bash
wpscan -u http://192.168.1.10/ -f csv -o results.csv
```

## Output verbose

```bash
wpscan -u <URL> -v
```

## Output muy verbose

```bash
wpscan -u <URL> -vv
```

## Modo quiet

```bash
wpscan -u <URL> --quiet
```

## Sin output de color

```bash
wpscan -u <URL> --no-color
```

## Loggear todas las peticiones

```bash
wpscan -u <URL> --log <log-file>
```

Ejemplo:

```bash
wpscan -u http://192.168.1.10/ --log scan.log
```

---

# 9. Advanced Options

## Especificar ruta de instalación de WordPress

```bash
wpscan -u <URL> --wp-content-dir <path>
```

## Especificar ruta wp-includes

```bash
wpscan -u <URL> --wp-includes-dir <path>
```

## Especificar ruta de plugins

```bash
wpscan -u <URL> --plugins-dir <path>
```

## Especificar ruta de temas

```bash
wpscan -u <URL> --themes-dir <path>
```

## Forzar detección de WordPress

```bash
wpscan -u <URL> --force
```

## Saltar detección de WordPress

```bash
wpscan -u <URL> --no-wp-content-dir-check
```

## Deshabilitar verificación TLS

```bash
wpscan -u <URL> --disable-tls-checks
```

## Especificar timeout

```bash
wpscan -u <URL> --timeout <seconds>
```

Ejemplo:

```bash
wpscan -u http://192.168.1.10/ --timeout 30
```

## Especificar máximo de threads

```bash
wpscan -u <URL> --max-threads <threads>
```

Ejemplo:

```bash
wpscan -u http://192.168.1.10/ --max-threads 20
```

## Delay entre peticiones

```bash
wpscan -u <URL> --throttle <milliseconds>
```

Ejemplo:

```bash
wpscan -u http://192.168.1.10/ --throttle 1000
```

---

# 10. Common Attack Scenarios

## Escaneo básico de WordPress

```bash
wpscan -u http://192.168.1.10/
```

## Enumerar plugins

```bash
wpscan -u http://192.168.1.10/ --enumerate p
```

## Enumerar temas

```bash
wpscan -u http://192.168.1.10/ --enumerate t
```

## Enumerar usuarios

```bash
wpscan -u http://192.168.1.10/ --enumerate u
```

## Enumerar todo

```bash
wpscan -u http://192.168.1.10/ --enumerate a
```

## Detectar plugins vulnerables

```bash
wpscan -u http://192.168.1.10/ --enumerate vp
```

## Brute-force de contraseña de admin

```bash
wpscan -u http://192.168.1.10/ --username admin --passwords passwords.txt
```

## Escanear con token de API

```bash
wpscan -u http://192.168.1.10/ --api-token abc123xyz
```

## Guardar resultados en JSON

```bash
wpscan -u http://192.168.1.10/ -f json -o results.json
```

## Escanear con proxy

```bash
wpscan -u http://192.168.1.10/ --proxy http://127.0.0.1:8080
```

## Escanear múltiples objetivos

```bash
wpscan -u http://192.168.1.10/ -u http://192.168.1.11/
```

## Escaneo verbose

```bash
wpscan -u http://192.168.1.10/ -v
```

---

# 11. Practical Workflows

## Flujo de trabajo básico de escaneo de seguridad de WordPress

```text
1. Identificar instalación de WordPress.
2. Ejecutar WPScan contra el objetivo.
3. Enumerar plugins y temas.
4. Comprobar vulnerabilidades.
5. Enumerar usuarios.
6. Testear credenciales débiles.
7. Documentar hallazgos.
```

## Ejemplo: Enumeración completa

```bash
# Escaneo básico
wpscan -u http://192.168.1.10/

# Enumerar plugins
wpscan -u http://192.168.1.10/ --enumerate p

# Enumerar temas
wpscan -u http://192.168.1.10/ --enumerate t

# Enumerar usuarios
wpscan -u http://192.168.1.10/ --enumerate u

# Enumerar todo
wpscan -u http://192.168.1.10/ --enumerate a
```

## Ejemplo: Detección de vulnerabilidades

```bash
# Detectar plugins vulnerables
wpscan -u http://192.168.1.10/ --enumerate vp

# Detectar temas vulnerables
wpscan -u http://192.168.1.10/ --enumerate vt

# Escaneo completo de vulnerabilidades
wpscan -u http://192.168.1.10/
```

## Ejemplo: Brute-force de contraseñas

```bash
# Brute-force de admin
wpscan -u http://192.168.1.10/ --username admin --passwords passwords.txt

# Brute-force de múltiples usuarios
wpscan -u http://192.168.1.10/ --usernames users.txt --passwords passwords.txt

# Detener en éxito
wpscan -u http://192.168.1.10/ --username admin --passwords passwords.txt --stop-on-success
```

## Ejemplo: Integración con API

```bash
# Escanear con token de API
wpscan -u http://192.168.1.10/ --api-token abc123xyz

# Enumerar plugins vulnerables con API
wpscan -u http://192.168.1.10/ --api-token abc123xyz --enumerate vp
```

## Ejemplo: Output y logging

```bash
# Guardar en archivo de texto
wpscan -u http://192.168.1.10/ -o results.txt

# Guardar en JSON
wpscan -u http://192.168.1.10/ -f json -o results.json

# Guardar en CSV
wpscan -u http://192.168.1.10/ -f csv -o results.csv

# Output verbose
wpscan -u http://192.168.1.10/ -v -o results.txt
```

---

# 12. Common Commands Reference

| Command | Description |
|---|---|
| `wpscan -h` | Mostrar ayuda |
| `wpscan --version` | Mostrar versión |
| `wpscan -u <URL>` | Especificar URL objetivo |
| `wpscan --update` | Actualizar WPScan |
| `wpscan --enumerate p` | Enumerar plugins |
| `wpscan --enumerate t` | Enumerar temas |
| `wpscan --enumerate u` | Enumerar usuarios |
| `wpscan --enumerate a` | Enumerar todo |
| `wpscan --enumerate vp` | Enumerar plugins vulnerables |
| `wpscan --enumerate vt` | Enumerar temas vulnerables |
| `wpscan --passwords <file>` | Especificar wordlist de contraseñas |
| `wpscan --username <user>` | Especificar username |
| `wpscan --usernames <file>` | Especificar lista de usernames |
| `wpscan --api-token <token>` | Especificar token de API |
| `wpscan -o <file>` | Guardar output en archivo |
| `wpscan -f json` | Output en formato JSON |
| `wpscan -f csv` | Output en formato CSV |
| `wpscan -v` | Output verbose |
| `wpscan --proxy <proxy>` | Usar proxy |
| `wpscan --cookie <cookie>` | Especificar cookie |
| `wpscan --user-agent <agent>` | Especificar User-Agent |
| `wpscan --timeout <seconds>` | Establecer timeout |
| `wpscan --max-threads <threads>` | Establecer máximo de threads |
| `wpscan --force` | Forzar detección de WordPress |
| `wpscan --quiet` | Modo quiet |
| `wpscan --no-color` | Sin output de color |

---

# 13. Plugin Enumeration

## Listar todos los plugins

```bash
wpscan -u <URL> --enumerate p
```

## Listar plugins vulnerables

```bash
wpscan -u <URL> --enumerate vp
```

## Listar plugins populares

```bash
wpscan -u <URL> --enumerate ap
```

## Listar plugins con rango específico

```bash
wpscan -u <URL> --enumerate p[1-50]
```

## Detectar versiones de plugins

```bash
wpscan -u <URL> --enumerate p
```

WPScan detecta automáticamente las versiones de plugins.

## Comprobar vulnerabilidades de plugins

```bash
wpscan -u <URL> --enumerate vp
```

Usa la base de datos de vulnerabilidades de WPScan.

---

# 14. Theme Enumeration

## Listar todos los temas

```bash
wpscan -u <URL> --enumerate t
```

## Listar temas vulnerables

```bash
wpscan -u <URL> --enumerate vt
```

## Listar temas populares

```bash
wpscan -u <URL> --enumerate at
```

## Listar temas con rango específico

```bash
wpscan -u <URL> --enumerate t[1-20]
```

## Detectar versiones de temas

```bash
wpscan -u <URL> --enumerate t
```

WPScan detecta automáticamente las versiones de temas.

## Comprobar vulnerabilidades de temas

```bash
wpscan -u <URL> --enumerate vt
```

Usa la base de datos de vulnerabilidades de WPScan.

---

# 15. User Enumeration

## Listar todos los usuarios

```bash
wpscan -u <URL> --enumerate u
```

## Listar usuarios con rango específico

```bash
wpscan -u <URL> --enumerate u[1-10]
```

## Listar solo usernames

```bash
wpscan -u <URL> --enumerate u
```

## Detectar roles de usuario

```bash
wpscan -u <URL> --enumerate u
```

WPScan detecta automáticamente los roles de usuario.

## Comprobar contraseñas débiles

```bash
wpscan -u <URL> --username admin --passwords passwords.txt
```

## Brute-force de múltiples usuarios

```bash
wpscan -u <URL> --usernames users.txt --passwords passwords.txt
```

---

# 16. Troubleshooting

## WordPress no detectado

- Verifica la instalación de WordPress.
- Usa `--force` para forzar la detección.
- Comprueba si el sitio es realmente WordPress.
- Verifica que la URL es correcta.

## No se encontraron plugins

- Los plugins pueden estar ocultos.
- Comprueba el directorio wp-content/plugins.
- Usa `--enumerate p` explícitamente.
- Verifica los permisos.

## Errores de API

- Comprueba la validez del token de API.
- Verifica la conectividad a internet.
- Actualiza la base de datos de WPScan.
- Comprueba los límites de tasa de la API.

## Rendimiento lento

- Reduce los threads máximos: `--max-threads 10`
- Añade throttle: `--throttle 1000`
- Usa proxy para anonimato.
- Limita el rango de enumeración.

## Falsos positivos

- Verifica las vulnerabilidades manualmente.
- Comprueba las versiones de plugins/temas.
- Revisa el output de WPScan cuidadosamente.
- Cruza información con otras fuentes.

---

# 17. Security Best Practices

## Verifica siempre los hallazgos

- Comprueba las vulnerabilidades manualmente.
- Verifica las versiones de plugins/temas.
- Testea en entorno controlado.
- Documenta todos los hallazgos.

## Respeta los límites legales

- Testea solo sistemas que poseas.
- Obtén autorización explícita.
- Sigue la divulgación responsable.
- Documenta todas las actividades.

## Minimiza el impacto

- Usa configuraciones de throttle apropiadas.
- Evita escaneos agresivos.
- Testea durante ventanas de mantenimiento.
- Monitoriza el sistema objetivo.

## Mantén las herramientas actualizadas

- Actualiza WPScan regularmente.
- Actualiza la base de datos de vulnerabilidades.
- Mantente informado sobre nuevas vulnerabilidades.
- Sigue los advisories de seguridad.

## Usa la API para resultados precisos

- Regístrate para la API de WPScan.
- Usa token de API para escaneos.
- Obtén los últimos datos de vulnerabilidades.
- Mejora la precisión del escaneo.

---

# 18. Important Reminders

- Obtén siempre autorización explícita antes de usar WPScan.
- Testea primero en un entorno de laboratorio controlado.
- No todas las vulnerabilidades detectadas son explotables.
- Algunos escaneos pueden impactar el rendimiento del sitio web.
- Mantén WPScan actualizado regularmente.
- Valida los hallazgos manualmente; no confíes únicamente en resultados automatizados.
- Documenta todas las acciones, comandos y resultados.
- Preserva la evidencia original y los logs.
- Respeta el scope y las reglas del engagement.
- Entiende las implicaciones legales y éticas de tus acciones.

---

# 19. Quick Reference Examples

## Escaneo básico

```bash
wpscan -u http://192.168.1.10/
```

## Enumerar plugins

```bash
wpscan -u http://192.168.1.10/ --enumerate p
```

## Enumerar temas

```bash
wpscan -u http://192.168.1.10/ --enumerate t
```

## Enumerar usuarios

```bash
wpscan -u http://192.168.1.10/ --enumerate u
```

## Enumerar todo

```bash
wpscan -u http://192.168.1.10/ --enumerate a
```

## Detectar plugins vulnerables

```bash
wpscan -u http://192.168.1.10/ --enumerate vp
```

## Brute-force de contraseña

```bash
wpscan -u http://192.168.1.10/ --username admin --passwords passwords.txt
```

## Usar token de API

```bash
wpscan -u http://192.168.1.10/ --api-token abc123xyz
```

## Guardar en JSON

```bash
wpscan -u http://192.168.1.10/ -f json -o results.json
```

## Escaneo verbose

```bash
wpscan -u http://192.168.1.10/ -v
```

## Usar proxy

```bash
wpscan -u http://192.168.1.10/ --proxy http://127.0.0.1:8080
```

## Actualizar WPScan

```bash
wpscan --update
```

---

# 20. Additional Resources

## Sitio web oficial de WPScan

```text
https://wpscan.com/
```

## Repositorio GitHub de WPScan

```text
https://github.com/wpscanteam/wpscan
```

## Base de datos de vulnerabilidades de WPScan

```text
https://wpscan.com/vulnerability-db
```

## Seguridad de WordPress

```text
https://wordpress.org/support/article/hardening-wordpress/
```

## OWASP WordPress Security

```text
https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/04-Enumerate_Applications_on_Web_Server
```
