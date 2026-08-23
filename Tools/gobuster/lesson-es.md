# Hoja de trucos de comandos de Gobuster

## Resumen

Gobuster es una herramienta de fuerza bruta de alto rendimiento escrita en Go, utilizada para descubrir:

- Directorios y archivos ocultos en servidores web.
- Subdominios DNS (con soporte para wildcard).
- Hosts virtuales (VHosts).
- Parámetros y valores mediante fuzzing.
- Buckets de almacenamiento en la nube (S3, GCS).

Solo utilice Gobuster contra sistemas que posea o que estén explícitamente incluidos en una evaluación autorizada.

```text
Reemplace <URL>, <domain> y <wordlist> con los valores autorizados del objetivo.
```

---

# 1. Comandos básicos de Gobuster

## Instalación rápida

```bash
# Usando Go (recomendado)
go install github.com/OJ/gobuster/v3@latest

# En Kali/Debian
sudo apt install gobuster
```

## Ver ayuda general

```bash
gobuster help
```

## Ver ayuda de un modo específico

```bash
gobuster dir help
```

---

# 2. Modo Directory (dir)

El modo más utilizado para enumerar directorios y archivos en servidores web.

## Escaneo básico de directorios

```bash
gobuster dir -u https://example.com -w /usr/share/wordlists/dirb/common.txt
```

## Escaneo con extensiones

Busca cada entrada del wordlist con extensiones adicionales:

```bash
gobuster dir -u https://example.com -w wordlist.txt -x php,html,js,txt,bak
```

## Especificar códigos de estado a mostrar

Muestra solo respuestas con códigos de estado específicos:

```bash
gobuster dir -u https://example.com -w wordlist.txt -s 200,204,301,302,307,401,403
```

## Excluir códigos de estado

Oculta respuestas con códigos de estado específicos:

```bash
gobuster dir -u https://example.com -w wordlist.txt -b 404
```

## Aumentar hilos (threads)

Incrementa el número de hilos concurrentes (default: 10):

```bash
gobuster dir -u https://example.com -w wordlist.txt -t 50
```

## Agregar retraso entre peticiones

Añade un retraso para reducir la carga en el objetivo:

```bash
gobuster dir -u https://example.com -w wordlist.txt --delay 1500ms
```

## Usar proxy HTTP

Envía el tráfico a través de un proxy (ej. Burp Suite):

```bash
gobuster dir -u https://example.com -w wordlist.txt -p http://127.0.0.1:8080
```

## Agregar cabeceras personalizadas

```bash
gobuster dir -u https://example.com -w wordlist.txt -H "Authorization: Bearer TOKEN"
```

## Usar cookies de sesión

```bash
gobuster dir -u https://example.com -w wordlist.txt -c "session=123456;user=admin"
```

## Omitir verificación de certificado TLS

```bash
gobuster dir -u https://example.com -w wordlist.txt -k
```

## Guardar resultados en archivo

```bash
gobuster dir -u https://example.com -w wordlist.txt -o resultados.txt
```

## Modo silencioso (sin banner)

```bash
gobuster dir -u https://example.com -w wordlist.txt -q
```

## Mostrar salida detallada

```bash
gobuster dir -u https://example.com -w wordlist.txt -v
```

## Excluir longitud de respuesta específica

Útil para filtrar respuestas con tamaño constante (ej. páginas de error personalizadas):

```bash
gobuster dir -u https://example.com -w wordlist.txt --exclude-length 1587
```

## Escanear con múltiples extensiones y rutas específicas

```bash
gobuster dir -u https://example.com/admin -w wordlist.txt -x php,html -t 40 -s 200,301,302,403
```

---

# 3. Modo DNS (dns)

Descubre subdominios mediante resolución DNS.

## Escaneo básico de subdominios

```bash
gobuster dns -d example.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```

## Usar resolver DNS personalizado

```bash
gobuster dns -d example.com -w wordlist.txt -r 8.8.8.8
```

## Aumentar hilos

```bash
gobuster dns -d example.com -w wordlist.txt -t 50
```

## Mostrar resultados wildcard

Por defecto, Gobuster oculta resultados wildcard. Usa `-i` para mostrarlos:

```bash
gobuster dns -d example.com -w wordlist.txt -i
```

## Guardar resultados

```bash
gobuster dns -d example.com -w wordlist.txt -o dns-results.txt
```

## Escaneo con dominio y resolver específico

```bash
gobuster dns -d target.com -w subdomains.txt -r 1.1.1.1 -t 100
```

---

# 4. Modo VHost (vhost)

Enumera hosts virtuales mediante fuzzing del encabezado `Host`.

## Escaneo básico de VHosts

```bash
gobuster vhost -u https://example.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt
```

## Usar IP como objetivo base

Útil cuando el servidor usa IP para capturar todo el tráfico:

```bash
gobuster vhost -u http://10.10.10.5 -w vhosts.txt
```

## Agregar User-Agent personalizado

```bash
gobuster vhost -u https://example.com -w wordlist.txt --useragent "PENTEST"
```

## Usar dominio específico

```bash
gobuster vhost -u https://example.com -w wordlist.txt --domain example.com
```

## Omitir verificación TLS

```bash
gobuster vhost -u https://example.com -w wordlist.txt -k
```

---

# 5. Modo Fuzz (fuzz)

Reemplaza la palabra clave `FUZZ` en cualquier parte de la URL para fuzzing flexible.

## Fuzzing de parámetro

Reemplaza `FUZZ` con cada entrada del wordlist en el valor del parámetro:

```bash
gobuster fuzz -u "https://example.com/page.php?id=FUZZ" -w wordlist.txt
```

## Fuzzing de nombre de parámetro

```bash
gobuster fuzz -u "https://example.com/page.php?FUZZ=value" -w wordlist.txt
```

## Fuzzing en la ruta

```bash
gobuster fuzz -u "https://example.com/FUZZ/admin" -w wordlist.txt
```

## Fuzzing en el encabezado Host

```bash
gobuster fuzz -u "https://example.com/" -w wordlist.txt -H "Host: FUZZ.example.com"
```

## Múltiples posiciones FUZZ

```bash
gobuster fuzz -u "https://FUZZ.example.com/api/FUZZ" -w wordlist.txt
```

## Fuzzing con método HTTP personalizado

```bash
gobuster fuzz -u "https://example.com/api" -w wordlist.txt -m POST
```

---

# 6. Modo S3 (s3)

Enumera buckets de Amazon S3.

## Escaneo básico de buckets S3

```bash
gobuster s3 -w bucket-names.txt
```

## Aumentar hilos

```bash
gobuster s3 -w bucket-names.txt -t 50
```

---

# 7. Modo GCS (gcs)

Enumera buckets de Google Cloud Storage.

## Escaneo básico de buckets GCS

```bash
gobuster gcs -w bucket-names.txt
```

---

# 8. Opciones globales

Estas opciones funcionan en todos los modos.

## Número de hilos

```bash
-t 50
```

## Wordlist

```bash
-w /ruta/a/wordlist.txt
```

## Archivo de salida

```bash
-o resultados.txt
```

## Modo silencioso

```bash
-q
```

## Sin colores

```bash
--no-color
```

## Sin barra de progreso

```bash
-z
```

## Sin mostrar errores

```bash
--no-error
```

## Verbose (mostrar errores)

```bash
-v
```

## Proxy HTTP

```bash
-p http://127.0.0.1:8080
```

## Omitir verificación TLS

```bash
-k
```

## Agregar cabecera personalizada

```bash
-H "Nombre-Cabecera: valor"
```

## Cookies

```bash
-c "cookie1=valor1;cookie2=valor2"
```

## Retraso entre peticiones

```bash
--delay 1500ms
```

---

# 9. Flujos de trabajo prácticos

## Flujo de trabajo básico de enumeración web

### Paso 1: Escaneo inicial de directorios

```bash
gobuster dir -u https://example.com -w /usr/share/wordlists/dirb/common.txt -t 30
```

### Paso 2: Escaneo con extensiones comunes

```bash
gobuster dir -u https://example.com -w wordlist.txt -x php,html,js,txt,bak,zip,conf -t 40
```

### Paso 3: Filtrar por códigos de estado relevantes

```bash
gobuster dir -u https://example.com -w wordlist.txt -x php,html -s 200,301,302,401,403 -t 50
```

### Paso 4: Guardar resultados

```bash
gobuster dir -u https://example.com -w wordlist.txt -x php,html,js -o dir-results.txt -t 50
```

---

## Flujo de trabajo de descubrimiento de subdominios

### Paso 1: Escaneo DNS básico

```bash
gobuster dns -d example.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -t 50
```

### Paso 2: Usar resolver personalizado

```bash
gobuster dns -d example.com -w subdomains.txt -r 8.8.8.8 -o dns-results.txt
```

### Paso 3: Enumerar VHosts

```bash
gobuster vhost -u https://example.com -w subdomains.txt -t 50
```

---

## Flujo de trabajo de fuzzing de parámetros

### Fuzzing de ID numérico

```bash
gobuster fuzz -u "https://example.com/user.php?id=FUZZ" -w /usr/share/seclists/Fuzzing/numbers.txt
```

### Fuzzing de nombres de parámetro

```bash
gobuster fuzz -u "https://example.com/login.php" -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt -H "FUZZ: test"
```

---

## Escaneo completo autorizado

Un escaneo detallado que guarda resultados:

```bash
gobuster dir -u https://example.com \
  -w /usr/share/seclists/Discovery/Web-Content/common.txt \
  -x php,html,txt,js,bak,zip,conf \
  -t 50 \
  -s 200,204,301,302,307,401,403 \
  -o gobuster-full.txt \
  -k
```

Este comando realiza:

- Escaneo de directorios y archivos.
- Prueba con extensiones comunes.
- 50 hilos concurrentes.
- Filtra por códigos de estado relevantes.
- Guarda resultados en archivo.
- Omite verificación TLS.

Use esto solo cuando el alcance autorice el tráfico asociado.

---

# 10. Tablas de referencia

## Modos de Gobuster

| Modo | Nombre completo | Descripción |
|---|---|---|
| `dir` | Directory/file | Fuerza bruta de directorios y archivos en servidores web. El más utilizado. |
| `dns` | DNS subdomain | Descubre subdominios mediante fuerza bruta de entradas DNS. |
| `vhost` | Virtual Host | Enumera hosts virtuales mediante fuzzing del encabezado Host. |
| `fuzz` | Fuzzing general | Reemplaza la palabra clave `FUZZ` en cualquier parte de la URL. |
| `s3` | AWS S3 bucket | Enumera buckets de Amazon S3. |
| `gcs` | Google Cloud Storage | Enumera buckets de Google Cloud Storage. |

## Opciones comunes de Gobuster

| Opción | Descripción |
|---|---|
| `-u` | URL objetivo (modos dir, vhost, fuzz) |
| `-d` | Dominio objetivo (modo dns) |
| `-w` | Ruta al wordlist |
| `-x` | Extensiones a agregar (modo dir) |
| `-s` | Mostrar solo estos códigos de estado |
| `-b` | Excluir estos códigos de estado |
| `-t` | Número de hilos concurrentes (default: 10) |
| `-o` | Archivo de salida |
| `-p` | Proxy HTTP |
| `-H` | Cabecera personalizada |
| `-c` | Cookies |
| `-k` | Omitir verificación TLS |
| `-q` | Modo silencioso (sin banner) |
| `-v` | Salida verbose |
| `-z` | Sin barra de progreso |
| `--delay` | Retraso entre peticiones |
| `--exclude-length` | Excluir respuestas con esta longitud |
| `-r` | Resolver DNS personalizado (modo dns) |
| `-i` | Mostrar resultados wildcard (modo dns) |
| `--useragent` | User-Agent personalizado (modo vhost) |
| `-m` | Método HTTP personalizado (modo fuzz) |

---

# 11. Wordlists recomendadas

## Directorios y archivos

```bash
/usr/share/wordlists/dirb/common.txt
/usr/share/seclists/Discovery/Web-Content/common.txt
/usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
```

## Subdominios

```bash
/usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
/usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt
/usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt
```

## Fuzzing de parámetros

```bash
/usr/share/seclists/Fuzzing/fuzz-Bo0oM.txt
/usr/share/seclists/Fuzzing/numbers.txt
/usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt
```

## Buckets S3/GCS

```bash
/usr/share/seclists/Discovery/Cloud-Storage/bucket-names.txt
```

---

# 12. Recordatorios importantes

- Un directorio encontrado no es automáticamente una vulnerabilidad.
- Los resultados pueden incluir falsos positivos. Valide manualmente.
- El fuzzing agresivo puede generar tráfico significativo.
- Los escaneos pueden activar alertas de firewall, WAF, IDS, IPS o SIEM.
- Use `-k` solo en entornos de prueba; en producción, valide certificados.
- Ajuste los hilos (`-t`) según la capacidad del objetivo.
- Use `--delay` para reducir la carga en sistemas sensibles.
- Guarde siempre la salida original como evidencia.
- Valide resultados interesantes con navegador, proxy o cliente HTTP.
- Nunca escanee sistemas fuera del alcance autorizado.
- Gobuster es ideal para un primer barrido rápido; use herramientas como ffuf para fuzzing más avanzado.

---

# 13. Ejemplos de comandos rápidos

| Tarea | Comando |
|---|---|
| Escaneo básico de directorios | `gobuster dir -u https://target.com -w wordlist.txt` |
| Escaneo con extensiones | `gobuster dir -u https://target.com -w wordlist.txt -x php,html,js` |
| Mostrar excepto 404 | `gobuster dir -u https://target.com -w wordlist.txt -b 404` |
| Escaneo DNS de subdominios | `gobuster dns -d target.com -w subdomains.txt` |
| Escaneo VHost | `gobuster vhost -u https://target.com -w vhosts.txt` |
| Fuzzing de parámetro | `gobuster fuzz -u "https://target.com?id=FUZZ" -w wordlist.txt` |
| Guardar resultados | `gobuster dir -u https://target.com -w wordlist.txt -o results.txt` |
| Usar proxy | `gobuster dir -u https://target.com -w wordlist.txt -p http://127.0.0.1:8080` |
| 50 hilos | `gobuster dir -u https://target.com -w wordlist.txt -t 50` |
| Retraso 1.5s | `gobuster dir -u https://target.com -w wordlist.txt --delay 1500ms` |

---

## Flujo de trabajo recomendado

```text
1. Confirmar autorización y alcance.
2. Realizar escaneo inicial de directorios (modo dir).
3. Escanear con extensiones comunes.
4. Enumerar subdominios (modo dns).
5. Enumerar VHosts (modo vhost).
6. Realizar fuzzing de parámetros (modo fuzz).
7. Guardar todos los resultados.
8. Validar manualmente hallazgos interesantes.
9. Eliminar falsos positivos.
10. Documentar solo hallazgos confirmados.
```
