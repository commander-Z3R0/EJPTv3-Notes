# File & Directory Enumeration

## Descripción General

El brute-forcing de archivos y directorios (a menudo llamado descubrimiento de contenido o enumeración de directorios) es una técnica utilizada para descubrir archivos, directorios y endpoints ocultos o no enlazados en un servidor web.

Las aplicaciones web a menudo exponen solo una parte de su estructura real a través de enlaces y menús visibles. En segundo plano, puede haber:

- Paneles de administración (`/admin`, `/dashboard`).
- Archivos de copia de seguridad (`.bak`, `.old`, `.zip`).
- Endpoints de desarrollo o prueba (`/test`, `/dev`, `/staging`).
- Rutas de API no referenciadas en el frontend.
- Archivos de configuración o depuración.

El brute-forcing funciona enviando grandes cantidades de solicitudes HTTP para nombres de archivos y directorios comunes (a partir de wordlists predefinidas) y analizando las respuestas del servidor (códigos de estado, tamaño de respuesta, redirecciones, etc.) para deducir qué existe.

---

## Por Qué Se Realiza

El brute-forcing de archivos y directorios se realiza porque lo que no se puede ver puede seguir siendo explotable. Sus objetivos principales son:

- **Ampliar la superficie de ataque**: descubrir funcionalidades que no estaban destinadas a ser públicas.
- **Identificar puntos de entrada sensibles**: portales de administración, herramientas internas, directorios de subida o APIs.
- **Encontrar configuraciones incorrectas**: copias de seguridad expuestas, archivos de código fuente o artefactos de desarrollo olvidados.
- **Habilitar ataques posteriores**: los endpoints descubiertos pueden llevar a:
  - Omisión de autenticación.
  - Vulnerabilidades de subida de archivos.
  - Divulgación de información.
  - Inyección SQL, XSS o fallos lógicos.

En los pentests del mundo real, la enumeración de directorios frecuentemente actúa como un punto de pivote — un endpoint descubierto a menudo desbloquea múltiples rutas de explotación.

---

# Gobuster

**Gobuster** es una herramienta de enumeración rápida por línea de comandos escrita en Go que se utiliza comúnmente durante la fase de reconocimiento y enumeración en pruebas de penetración de aplicaciones web.

Realiza descubrimiento de estilo brute-force usando wordlists y está diseñado para ser eficiente, simple y scriptable.

Gobuster es popular porque es:

- **Rápido** (basado en Go, alto rendimiento).
- **Fiable** (lógica simple, mínimos falsos positivos).
- **Flexible** (funciona con muchas wordlists y configuraciones).
- **Adecuado para automatización** en flujos de trabajo de pentesting.

En la práctica, Gobuster es a menudo una de las primeras herramientas de enumeración activa que se ejecuta contra un objetivo web después del reconocimiento pasivo, ayudando a los testers a identificar rápidamente áreas que merecen una prueba manual más profunda.

---

## Modos de Gobuster

Gobuster soporta múltiples modos de enumeración. Los más relevantes para el pentesting de aplicaciones web son:

| Modo | Descripción |
|---|---|
| `dir` | Enumeración de directorios y archivos en un servidor web |
| `vhost` | Enumeración de hosts virtuales en un dominio objetivo |
| `dns` | Enumeración de subdominios por DNS |
| `fuzz` | Modo de fuzzing para patrones personalizados |
| `gcs` | Enumeración de buckets de Google Cloud Storage |
| `s3` | Enumeración de buckets de AWS S3 |

### Funcionalidad Principal (modo dir)

- Fuerza bruta de directorios y archivos en un servidor web.
- Soporta extensiones de archivo (ej. `.php`, `.js`, `.txt`).
- Filtra resultados basándose en códigos de estado HTTP.
- Ayuda a descubrir rutas y funcionalidades ocultas.

### Enumeración de Hosts Virtuales (modo vhost)

- Descubre hosts virtuales ocultos en un dominio objetivo.
- Útil cuando las aplicaciones se comportan de manera diferente según el nombre de host.
- Cambia la cabecera `Host` para probar hosts virtuales configurados en el objetivo.

### Enumeración de Subdominios por DNS (modo dns)

- Fuerza bruta de subdominios usando consultas DNS.
- Útil para mapear el ecosistema completo de la aplicación.
- Ideal como complemento del modo vhost.

---

# Instalación

Gobuster suele venir preinstalado en Kali Linux.

## Comprobar la Versión Instalada

```bash
gobuster version  # muestra la versión instalada de Gobuster.
```

## Mostrar la Ayuda

```bash
gobuster --help  # muestra las opciones y modos disponibles de Gobuster.
gobuster dir --help  # muestra las opciones específicas del modo dir.
```

## Instalar Gobuster en Sistemas Basados en Debian

```bash
sudo apt update  # actualiza la lista local de paquetes.
sudo apt install gobuster -y  # instala Gobuster.
```

Alternativamente, Gobuster puede instalarse desde sus releases de GitHub o compilarse desde el código fuente usando Go:

```bash
go install github.com/OJ/gobuster/v3@latest  # instala Gobuster usando Go.
```

---

# Flags y Opciones Básicas

| Flag | Descripción | Ejemplo |
|---|---|---|
| `-u` | URL objetivo | `-u https://example.com` |
| `-w` | Wordlist para brute-force | `-w /path/to/wordlist.txt` |
| `-k` | Ignora errores de certificado SSL/TLS (HTTPS) | `-k` |
| `-t` | Número de hilos para acelerar el escaneo | `-t 20` |
| `-o` | Guarda resultados en un archivo | `-o results.txt` |
| `-x` | Extensiones de archivo a buscar | `-x php,html,txt` |
| `-r` | Activa el modo recursivo | `-r` |
| `-s` | Filtra por códigos de estado HTTP | `-s 200,204,301` |
| `-z` | Ignora la longitud de respuesta (sin barra de progreso por resultado) | `-z` |
| `-X` | Usa métodos HTTP específicos | `-X GET,POST` |
| `-P` | Prefijo de ruta URL para cada solicitud | `-P /app/` |
| `--append-domain` | En modo vhost, agrega el dominio base a cada palabra (recomendado) | `--append-domain` |
| `-q` | Modo silencioso, suprime el banner y la salida extra | `-q` |
| `-e` | Salida expandida, muestra URLs completas | `-e` |
| `-n` | No muestra códigos de estado en la salida | `-n` |
| `--no-error` | No muestra errores | `--no-error` |
| `-c` | Especifica cookies HTTP para autenticación | `-c "session=abc123"` |
| `-H` | Especifica cabeceras HTTP personalizadas | `-H "Authorization: Bearer token"` |
| `-b` | Lista negra de códigos de estado específicos | `-b 403,404` |
| `-l` | Muestra la longitud de respuesta en la salida | `-l` |

---

# Ejemplos de Enumeración de Directorios

## Enumeración Básica de Directorios

Enumera directorios usando una wordlist común:

```bash
gobuster dir -u https://www.example.com -w common.txt  # brute-force básico de directorios.
```

## Wordlist Personalizada y Extensiones

Usa una wordlist personalizada y especifica extensiones de archivo a buscar:

```bash
gobuster dir -u https://www.example.com -w custom.txt -x php,html  # busca archivos .php y .html.
```

## Enumeración Recursiva de Directorios

Activa el modo recursivo para explorar subdirectorios:

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -r  # enumera recursivamente los directorios descubiertos.
```

## Enumeración de Directorios desde una Ruta URL Específica

Enumera directorios comenzando desde una ruta URL específica:

```bash
gobuster dir -u https://www.example.com/subdir/ -w common.txt  # fuerza bruta de rutas bajo /subdir/.
```

## Filtrar por Códigos de Estado HTTP

Especifica qué códigos de estado HTTP considerar como encontrados:

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -x php,html -s 200,204  # solo muestra resultados con estado 200 y 204.
```

## Usar Diferentes Métodos HTTP

Usa diferentes métodos HTTP durante la enumeración de directorios:

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -x php -X GET,POST  # envía solicitudes GET y POST.
```

## Prefijo de Ruta URL

Añade un prefijo de ruta URL a cada solicitud:

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -P /app/  # prefija cada palabra con /app/.
```

## Ignorar la Longitud de Respuesta

Ignora la longitud de respuesta para identificar rápidamente rutas existentes:

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -z  # suprime la columna de longitud de respuesta.
```

## Guardar Resultados en un Archivo

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -o results.txt  # guarda los resultados en un archivo.
```

## Escanear con Cookies Personalizadas (Enumeración Autenticada)

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -c "session=abc123; role=admin"  # escanea con cookies de autenticación.
```

## Escanear con Cabeceras Personalizadas

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -H "Authorization: Bearer token123" -H "X-Custom-Header: test"  # envía cabeceras personalizadas con cada solicitud.
```

## Lista Negra de Códigos de Estado

Excluye códigos de estado específicos de los resultados:

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -b 403,404  # oculta respuestas 403 y 404.
```

## Salida Expandida con URLs Completas

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -e -o full-urls.txt  # muestra URLs completas en la salida y las guarda.
```

## Aumentar Hilos para un Escaneo Más Rápido

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -t 50  # usa 50 hilos para una enumeración más rápida.
```

## Ignorar Errores de Certificado SSL/TLS (HTTPS)

```bash
gobuster dir -u https://www.example.com -w wordlist.txt -k  # ignora errores de validación de certificados.
```

---

# Enumeración de Hosts Virtuales (modo vhost)

Un servidor web puede alojar múltiples sitios web (hosts virtuales) en la misma dirección IP. El modo vhost de Gobuster fuerza bruta los nombres de hosts virtuales cambiando la cabecera `Host` de cada solicitud.

## Enumeración Básica de vhost

```bash
gobuster vhost -u https://nunchucks.htb -k -w subdomains.txt --append-domain  # fuerza bruta de hosts virtuales, agregando el dominio base.
```

### Nota Importante Sobre `--append-domain`

`--append-domain` es crítico para que el modo vhost funcione correctamente. Sin él, Gobuster solo envía la palabra cruda de la wordlist como cabecera `Host` (ej. `admin` en lugar de `admin.nunchucks.htb`). Con `--append-domain`, cada palabra se combina con el dominio base, produciendo nombres de hosts virtuales válidos.

## Enumeración vhost con Hilos y Salida

```bash
gobuster vhost -u https://nunchucks.htb -k -w subdomains.txt --append-domain -t 30 -o vhost_results.txt  # escanea con 30 hilos y guarda los resultados.
```

El modo vhost es útil cuando:

- Las aplicaciones se comportan de manera diferente según la cabecera `Host`.
- El DNS no está disponible o no resuelve subdominios.
- Desea descubrir hosts virtuales configurados en el servidor pero no expuestos públicamente.

---

# Enumeración de Subdominios por DNS (modo dns)

El modo dns de Gobuster fuerza bruta subdominios enviando consultas DNS. No envía solicitudes HTTP; solo comprueba si existen registros DNS para cada subdominio candidato.

## Enumeración DNS Básica

```bash
gobuster dns -d nunchucks.htb -w subdomains.txt -t 30 -o dns_result.txt  # fuerza bruta de subdominios vía consultas DNS con 30 hilos.
```

### Cuándo Usar el Modo dns

- Para mapear el ecosistema completo de la aplicación.
- Como complemento del modo vhost (DNS encuentra subdominios resolubles; vhost encuentra hosts virtuales configurados en el servidor incluso sin DNS).
- Cuando desea confirmar si los subdominios descubiertos se resuelven a una dirección IP.

### Diferencias Entre el Modo vhost y el Modo dns

| Aspecto | Modo vhost | Modo dns |
|---|---|---|
| Lo que prueba | Hosts virtuales cambiando la cabecera `Host` | Subdominios vía resolución DNS |
| Requiere DNS | No | Sí |
| Encuentra | Hosts configurados en el servidor | Subdominios que se resuelven en DNS |
| Mejor usado para | Encontrar apps ocultas en una IP conocida | Mapear la huella DNS del dominio |

Usar ambos modos juntos proporciona una imagen más completa del panorama de subdominios y hosts virtuales del objetivo.

---

# Wordlists Comunes

Gobuster requiere una wordlist para realizar el brute-force. Ubicaciones comunes de wordlists en Kali Linux:

```text
/usr/share/wordlists/dirb/common.txt
/usr/share/wordlists/dirb/big.txt
/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
/usr/share/wordlists/dirbuster/directory-list-2.3-small.txt
/usr/share/wordlists/seclists/Discovery/Web-Content/
```

[SecLists](https://github.com/danielmiessler/SecLists) es una colección completa de wordlists para pruebas de seguridad. Puede instalarse en Kali Linux:

```bash
sudo apt install seclists -y  # instala la colección SecLists.
```

Elija una wordlist apropiada para el objetivo:

- `common.txt` para escaneos rápidos y amplios.
- `directory-list-2.3-medium.txt` para una enumeración más exhaustiva.
- Wordlists personalizadas adaptadas a la pila tecnológica del objetivo.

---

# Entender los Códigos de Estado HTTP en los Resultados

Gobuster reporta resultados basándose en los códigos de estado HTTP devueltos por el servidor. Entender estos códigos ayuda a interpretar los hallazgos:

| Código de Estado | Significado | Interpretación |
|---|---|---|
| `200` | OK | El recurso existe y es accesible |
| `204` | No Content | El recurso existe pero no devuelve cuerpo |
| `301` | Moved Permanently | El recurso existe pero ha sido redirigido |
| `302` | Found | El recurso existe pero está temporalmente redirigido |
| `401` | Unauthorized | El recurso existe pero requiere autenticación |
| `403` | Forbidden | El recurso existe pero el acceso está denegado |
| `404` | Not Found | El recurso no existe |
| `500` | Internal Server Error | El recurso existe pero causó un error del servidor |

Los códigos de estado `401` y `403` son particularmente interesantes durante la enumeración — confirman que un recurso existe aunque el acceso esté restringido, lo que puede guiar ataques posteriores (omisión de autenticación, escalada de privilegios, etc.).

---

# Comandos Prácticos Comunes

## Escaneo Rápido de Directorios con Wordlist Común

```bash
gobuster dir -u http://target.local -w /usr/share/wordlists/dirb/common.txt -t 30 -o gobuster-dir.txt  # escaneo rápido de directorios, 30 hilos, guarda la salida.
```

## Escaneo de Directorios con Extensiones y HTTPS

```bash
gobuster dir -u https://target.local -w /usr/share/wordlists/dirb/common.txt -x php,html,txt -k -t 30 -o gobuster-ext.txt  # escanea archivos con extensiones por HTTPS ignorando errores de certificado.
```

## Escaneo Recursivo con Extensiones

```bash
gobuster dir -u http://target.local -w /usr/share/wordlists/dirb/common.txt -x php,bak,old -r -t 30 -o gobuster-recursive.txt  # escanea recursivamente con extensiones de archivos de copia de seguridad.
```

## Escaneo de Directorios Autenticado

```bash
gobuster dir -u http://target.local -w /usr/share/wordlists/dirb/common.txt -c "session=abc123" -o gobuster-auth.txt  # escanea con una cookie de sesión.
```

## Enumeración de vhost

```bash
gobuster vhost -u https://target.local -k -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain -t 30 -o gobuster-vhost.txt  # fuerza bruta de hosts virtuales con dominio agregado.
```

## Enumeración de Subdominios por DNS

```bash
gobuster dns -d target.local -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt -t 30 -o gobuster-dns.txt  # fuerza bruta de subdominios vía DNS.
```

## Escaneo con Filtro de Código de Estado y Lista Negra

```bash
gobuster dir -u http://target.local -w /usr/share/wordlists/dirb/common.txt -s 200,301,401,403 -b 404 -o gobuster-filtered.txt  # muestra solo códigos de estado interesantes y oculta los 404.
```

---

# Modo Fuzzing (fuzz)

Gobuster también incluye un modo `fuzz` que permite fuerza bruta de patrones de URL personalizados. Esto es útil cuando se conoce parte de la estructura de una URL y se quiere forzar un segmento específico.

## Ejemplo Básico de Fuzzing

```bash
gobuster fuzz -u https://www.example.com/FUZZ?param=value -w wordlist.txt  # fuerza bruta del marcador FUZZ en la URL.
```

La palabra clave `FUZZ` se reemplaza por cada palabra de la wordlist. Este modo es flexible y soporta cualquier patrón de URL donde se quiera enumerar un segmento variable.

---

# Lista de Comprobación para una Enumeración Segura

Antes de ejecutar Gobuster:

- Confirme que el objetivo está dentro del alcance.
- Confirme la URL, el puerto y el protocolo del objetivo.
- Identifique si se usa HTTPS (use `-k` si es necesario).
- Elija una wordlist apropiada para el objetivo.
- Decida si se requiere autenticación (use `-c` o `-H`).
- Establezca un número de hilos razonable para evitar saturar el objetivo.
- Considere si se necesita escaneo recursivo.
- Informe al equipo correspondiente si el escaneo podría afectar a sistemas de monitorización o producción.

Después de ejecutar Gobuster:

- Guarde los resultados.
- Revise cada ruta y archivo descubierto.
- Verifique manualmente los hallazgos interesantes con un navegador, `curl` o Burp Suite.
- Compruebe los códigos de estado `401` y `403` para problemas de autenticación o control de acceso.
- Investigue los archivos de copia de seguridad (`.bak`, `.old`, `.zip`, `.tar.gz`).
- Pruebe los paneles de administración y directorios de subida descubiertos.
- Documente evidencia como URLs, respuestas HTTP y capturas de pantalla.

---

# Puntos Clave

- El brute-forcing de archivos y directorios descubre contenido oculto o no enlazado en un servidor web.
- Gobuster es una herramienta de enumeración rápida basada en Go con múltiples modos.
- Use el modo `dir` para enumeración de directorios y archivos.
- Use el modo `vhost` para enumeración de hosts virtuales (use siempre `--append-domain`).
- Use el modo `dns` para enumeración de subdominios por DNS.
- Use el modo `fuzz` para fuerza bruta de patrones de URL personalizados.
- Use `-u` para definir la URL objetivo.
- Use `-w` para definir la wordlist.
- Use `-x` para especificar extensiones de archivo.
- Use `-r` para enumeración recursiva.
- Use `-s` para filtrar por códigos de estado y `-b` para lista negra.
- Use `-k` para ignorar errores de certificado SSL/TLS.
- Use `-t` para controlar el número de hilos.
- Use `-o` para guardar resultados en un archivo.
- Use `-c` o `-H` para escaneos autenticados.
- Los códigos de estado `401` y `403` confirman que un recurso existe aunque el acceso esté restringido.
- Valide siempre los hallazgos manualmente antes de reportarlos como vulnerabilidades.
- Elija wordlists apropiadas a la pila tecnológica del objetivo.
