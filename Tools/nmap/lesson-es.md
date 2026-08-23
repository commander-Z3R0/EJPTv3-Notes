# Hoja de trucos de comandos de Nmap

## Resumen

Nmap es una herramienta de descubrimiento de red y auditoría de seguridad utilizada para identificar:

- Hosts activos.
- Puertos abiertos, cerrados o filtrados.
- Servicios en ejecución.
- Versiones de servicios.
- Información del sistema operativo.
- Comportamiento de firewalls y filtrado.
- Información relacionada con la seguridad mediante scripts NSE.

Solo escanea sistemas que poseas o que estén explícitamente incluidos en una evaluación autorizada.

```text
Reemplaza <target-IP> con la dirección autorizada del objetivo.
```

---

# 1. Comandos básicos de Nmap

## Escaneo básico

Escanea los puertos TCP más comunes en un objetivo:

```bash
nmap <target-IP>
```

## Salida detallada (verbose)

Muestra información adicional durante el escaneo:

```bash
nmap -v <target-IP>
```

Usa doble verbosidad para una salida más detallada:

```bash
nmap -vv <target-IP>
```

## Desactivar resolución DNS

Evita las búsquedas DNS inversas para hacer los escaneos más rápidos y reducir tráfico innecesario:

```bash
nmap -n <target-IP>
```

## Omitir descubrimiento de host

Trata al objetivo como en línea y escanearlo directamente:

```bash
nmap -Pn <target-IP>
```

Esto es útil cuando ICMP o las sondas de descubrimiento normales están bloqueadas. También puede hacer el escaneo más lento porque Nmap escanea el objetivo incluso si está fuera de línea.

## Listar objetivos sin escanear

Muestra los objetivos que Nmap escanearía sin enviar sondas de escaneo:

```bash
nmap -sL <target-IP>
```

## Solo descubrimiento de host

Descubre si el objetivo está en línea sin realizar un escaneo de puertos:

```bash
nmap -sn <target-IP>
```

Esto se usa comúnmente durante la fase inicial de reconocimiento.

## Mostrar razones del escaneo

Muestra la razón por la cual Nmap asignó un estado particular a un puerto o host:

```bash
nmap --reason <target-IP>
```

## Mostrar solo puertos abiertos

Oculta los puertos que no se reportan como abiertos:

```bash
nmap --open <target-IP>
```

---

# 2. Descubrimiento de host y escaneo de puertos

## Técnicas de descubrimiento de host

### Solicitud de eco ICMP

Usa descubrimiento por eco ICMP:

```bash
sudo nmap -PE <target-IP>
```

### Solicitud de timestamp ICMP

Usa descubrimiento por timestamp ICMP:

```bash
sudo nmap -PP <target-IP>
```

### TCP SYN ping

Envía sondas TCP SYN a puertos específicos:

```bash
sudo nmap -PS80,443 <target-IP>
```

### TCP ACK ping

Envía sondas TCP ACK a puertos específicos:

```bash
sudo nmap -PA80,443 <target-IP>
```

### UDP ping

Envía sondas UDP a puertos seleccionados:

```bash
sudo nmap -PU53,161 <target-IP>
```

### Desactivar ARP ping

Evita que Nmap use descubrimiento ARP en redes locales:

```bash
sudo nmap --disable-arp-ping <target-IP>
```

---

## Escaneo TCP SYN

El escaneo TCP SYN es uno de los tipos de escaneo TCP más utilizados.

```bash
sudo nmap -sS <target-IP>
```

Características:

- Envía paquetes SYN.
- Normalmente requiere privilegios elevados.
- No completa la conexión TCP completa en el caso normal.
- Proporciona una forma rápida de identificar estados de puertos TCP.

Úsalo solo contra objetivos autorizados.

## Escaneo TCP Connect

Usa el método de conexión TCP completo del sistema operativo:

```bash
nmap -sT <target-IP>
```

Esto es útil cuando no se dispone de privilegios de paquetes raw.

Características:

- Completa la conexión TCP.
- Normalmente crea más conexiones visibles en los registros de aplicaciones.
- Normalmente no requiere privilegios de root.

## Escaneo UDP

Escanea puertos UDP:

```bash
sudo nmap -sU <target-IP>
```

Los escaneos UDP suelen ser más lentos que los TCP porque los servicios UDP pueden no responder consistentemente.

## Escaneo combinado TCP y UDP

Escanea puertos TCP y UDP seleccionados juntos:

```bash
sudo nmap -sS -sU -p T:22,80,443,U:53,161 <target-IP>
```

La sintaxis separa los protocolos:

```text
T: = puertos TCP
U: = puertos UDP
```

## Escaneo TCP ACK

Usa un escaneo TCP ACK para ayudar a analizar reglas de firewall:

```bash
sudo nmap -sA <target-IP>
```

Este escaneo es principalmente útil para determinar si los puertos están filtrados por un firewall. No está diseñado para identificar puertos abiertos directamente.

## Escaneo TCP FIN

Envía paquetes FIN a puertos TCP:

```bash
sudo nmap -sF <target-IP>
```

## Escaneo TCP NULL

Envía paquetes TCP sin las banderas TCP estándar:

```bash
sudo nmap -sN <target-IP>
```

## Escaneo TCP Xmas

Envía paquetes con las banderas FIN, PSH y URG:

```bash
sudo nmap -sX <target-IP>
```

Estos tipos de escaneo pueden comportarse de manera diferente según el sistema operativo y el firewall del objetivo. Los resultados siempre deben interpretarse con cuidado.

---

## Selección de puertos

### Escanear un solo puerto

```bash
nmap -p 22 <target-IP>
```

### Escanear múltiples puertos

```bash
nmap -p 22,80,443 <target-IP>
```

### Escanear un rango de puertos

```bash
nmap -p 1-1000 <target-IP>
```

### Escanear todos los puertos TCP

```bash
sudo nmap -p- <target-IP>
```

`-p-` significa puertos del 1 al 65535.

### Escanear los puertos más comunes

```bash
nmap --top-ports 100 <target-IP>
```

Escanea los 100 puertos más comunes.

```bash
nmap --top-ports 1000 <target-IP>
```

Escanea los 1.000 puertos más comunes.

### Escanear puertos por protocolo

```bash
sudo nmap -p T:80,443,U:53 <target-IP>
```

### Escanear la lista de puertos predeterminada

```bash
nmap -p- <target-IP>
```

Esto verifica todos los puertos TCP. Para UDP, usa `-sU` explícitamente.

---

# 3. Detección de servicios, SO y NSE

## Detección de servicios y versiones

Identifica la aplicación y versión que se ejecuta en puertos abiertos:

```bash
nmap -sV <target-IP>
```

Nmap envía sondas de servicio a puertos abiertos y compara las respuestas con su base de datos de detección de servicios.

## Detección de servicios en puertos seleccionados

```bash
nmap -sV -p 22,80,443 <target-IP>
```

## Intensidad de detección de versiones

Usa una intensidad menor de detección de versiones:

```bash
nmap -sV --version-intensity 0 <target-IP>
```

Usa una intensidad mayor:

```bash
nmap -sV --version-intensity 9 <target-IP>
```

La intensidad varía de 0 a 9:

- Valores más bajos son más rápidos y menos exhaustivos.
- Valores más altos realizan más sondas.
- Valores más altos pueden generar más tráfico y tardar más.

## Detección del sistema operativo

Intenta identificar el sistema operativo del objetivo:

```bash
sudo nmap -O <target-IP>
```

## Detección de SO con detección de versiones

```bash
sudo nmap -O -sV <target-IP>
```

## Escaneo agresivo

Habilita varias funciones avanzadas de detección:

```bash
sudo nmap -A <target-IP>
```

`-A` habilita varias funciones, incluyendo:

- Detección de SO.
- Detección de servicios y versiones.
- Scripts NSE predeterminados.
- Traceroute.

Debido a que combina varias técnicas de detección, puede generar más tráfico que un escaneo básico.

## Detección de SO con conjetura más agresiva

```bash
sudo nmap -O --osscan-guess <target-IP>
```

Esto le pide a Nmap que haga un esfuerzo máximo para adivinar el sistema operativo cuando los resultados no son concluyentes.

---

## Nmap Scripting Engine (NSE)

El Nmap Scripting Engine (NSE) permite que los scripts realicen descubrimiento adicional y comprobaciones de seguridad.

Los scripts NSE pueden ayudar con:

- Enumeración de servicios.
- Recopilación de banners.
- Recopilación de información HTTP.
- Comprobaciones de configuración TLS.
- Descubrimiento SMB.
- Comprobaciones relacionadas con autenticación.
- Detección de vulnerabilidades.
- Enumeración de recursos expuestos.

NSE solo debe usarse dentro del alcance autorizado.

## Ejecutar scripts predeterminados

```bash
nmap -sC <target-IP>
```

## Combinar scripts predeterminados con detección de versiones

```bash
nmap -sC -sV <target-IP>
```

Este es un comando de enumeración común para servicios web o de red identificados.

## Ejecutar un script específico

```bash
nmap --script http-title -p 80,443 <target-IP>
```

## Comprobar información de cifrado TLS

```bash
nmap --script ssl-enum-ciphers -p 443 <target-IP>
```

## Enumerar información del sistema operativo SMB

```bash
nmap --script smb-os-discovery -p 445 <target-IP>
```

## Ejecutar scripts seguros

```bash
nmap --script safe <target-IP>
```

La categoría `safe` contiene scripts destinados a ser menos intrusivos, pero aún deben revisarse y usarse solo contra objetivos autorizados.

## Ejecutar scripts predeterminados y seguros

```bash
nmap --script "default,safe" <target-IP>
```

## Ejecutar scripts de vulnerabilidad

```bash
nmap --script vuln <target-IP>
```

Los scripts de vulnerabilidad pueden producir falsos positivos y pueden generar tráfico significativo. Nunca trates su salida como una vulnerabilidad confirmada sin validación manual.

## Ejecutar scripts contra puertos seleccionados

```bash
nmap --script http-title,http-headers -p 80,443 <target-IP>
```

## Mostrar información sobre un script

```bash
nmap --script-help http-title
```

## Mostrar información sobre una categoría de scripts

```bash
nmap --script-help safe
```

## Listar scripts NSE instalados

```bash
ls /usr/share/nmap/scripts/
```

La ubicación exacta puede variar según el sistema operativo y el método de instalación.

---

# 4. Temporización, rendimiento y salida

## Plantillas de temporización

Nmap proporciona plantillas de temporización de `-T0` a `-T5`.

```bash
nmap -T0 <target-IP>
```

Muy lento y diseñado para reducir la velocidad del escaneo.

```bash
nmap -T1 <target-IP>
```

Perfil de temporización lento.

```bash
nmap -T2 <target-IP>
```

Perfil de temporización educado (polite).

```bash
nmap -T3 <target-IP>
```

Perfil de temporización predeterminado.

```bash
nmap -T4 <target-IP>
```

Perfil de temporización más rápido, comúnmente usado en laboratorios controlados o evaluaciones internas autorizadas.

```bash
nmap -T5 <target-IP>
```

Temporización muy agresiva. Puede aumentar la pérdida de paquetes, resultados inexactos e impacto en el servicio.

## Limitar la duración del escaneo

Establece un tiempo máximo para escanear un host:

```bash
nmap --host-timeout 10m <target-IP>
```

## Limitar reintentos

Reduce el número de retransmisiones:

```bash
nmap --max-retries 2 <target-IP>
```

Valores de reintento más bajos pueden hacer los escaneos más rápidos pero pueden reducir la precisión en redes poco fiables.

## Establecer una tasa máxima de paquetes

```bash
sudo nmap --max-rate 100 <target-IP>
```

Esto limita el número máximo de paquetes enviados por segundo.

## Establecer una tasa mínima de paquetes

```bash
sudo nmap --min-rate 50 <target-IP>
```

Usa los controles de tasa con cuidado. El tráfico excesivo puede afectar a los servicios y activar sistemas de monitoreo.

## Añadir un retraso entre sondas

```bash
sudo nmap --scan-delay 100ms <target-IP>
```

Esto puede reducir la velocidad del escaneo y la concentración de tráfico.

## Mostrar estadísticas periódicas

```bash
nmap --stats-every 10s <target-IP>
```

Esto muestra el progreso del escaneo a intervalos regulares.

---

## Guardar salida como texto normal

```bash
nmap -oN nmap-result.txt <target-IP>
```

## Guardar salida como XML

```bash
nmap -oX nmap-result.xml <target-IP>
```

XML es útil para importar resultados a otras herramientas.

## Guardar salida en formato grepable

```bash
nmap -oG nmap-result.gnmap <target-IP>
```

Este formato es útil para procesamiento de texto simple.

## Guardar salida en todos los formatos principales

```bash
nmap -oA nmap-result <target-IP>
```

Esto crea archivos como:

```text
nmap-result.nmap
nmap-result.xml
nmap-result.gnmap
```

## Añadir salida a un archivo existente

```bash
nmap --append-output -oN nmap-result.txt <target-IP>
```

## Retomar un escaneo interrumpido

```bash
nmap --resume nmap-result.nmap
```

Esto requiere un archivo de salida en formato normal del escaneo interrumpido.

## Aumentar el detalle de la salida

```bash
nmap -v <target-IP>
```

## Habilitar depuración

```bash
nmap -d <target-IP>
```

Usa niveles de depuración más altos solo cuando resuelvas problemas de comportamiento del escaneo:

```bash
nmap -dd <target-IP>
```

---

# 5. Flujos de trabajo prácticos e interpretación

## Flujo de trabajo de reconocimiento básico

### Paso 1: Comprobar si el host está en línea

```bash
nmap -sn -n <target-IP>
```

### Paso 2: Escanear puertos TCP comunes

```bash
nmap -n --top-ports 1000 <target-IP>
```

### Paso 3: Escanear todos los puertos TCP

```bash
sudo nmap -n -sS -p- <target-IP>
```

### Paso 4: Detectar servicios y versiones

Usa los puertos encontrados en el escaneo anterior:

```bash
nmap -n -sV -p 22,80,443 <target-IP>
```

### Paso 5: Ejecutar scripts predeterminados

```bash
nmap -n -sC -sV -p 22,80,443 <target-IP>
```

### Paso 6: Intentar detección de SO

```bash
sudo nmap -n -O -p 22,80,443 <target-IP>
```

### Paso 7: Guardar los resultados finales

```bash
sudo nmap -n -sC -sV -O -p 22,80,443 -oA nmap-final <target-IP>
```

---

## Flujo de trabajo de enumeración de servicios web

Escanea puertos web comunes y detecta versiones:

```bash
nmap -sV -p 80,443,8080,8443 <target-IP>
```

Recopila el título HTTP:

```bash
nmap --script http-title -p 80,443,8080,8443 <target-IP>
```

Recopila cabeceras HTTP:

```bash
nmap --script http-headers -p 80,443,8080,8443 <target-IP>
```

Ejecuta scripts de descubrimiento HTTP seleccionados:

```bash
nmap --script http-title,http-headers -p 80,443 <target-IP>
```

Siempre valida manualmente los resultados interesantes con un navegador autorizado, proxy o cliente HTTP.

---

## Flujo de trabajo de enumeración de servicios TLS

Identifica servicios en puertos TLS comunes:

```bash
nmap -sV -p 443,465,636,8443 <target-IP>
```

Enumera los cifrados TLS soportados:

```bash
nmap --script ssl-enum-ciphers -p 443 <target-IP>
```

Obtén información del certificado:

```bash
nmap --script ssl-cert -p 443 <target-IP>
```

Ejecuta ambos scripts:

```bash
nmap --script ssl-cert,ssl-enum-ciphers -p 443 <target-IP>
```

---

## Flujo de trabajo de enumeración UDP

Escanea puertos UDP comunes:

```bash
sudo nmap -sU --top-ports 100 <target-IP>
```

Escanea puertos UDP seleccionados:

```bash
sudo nmap -sU -p 53,67,68,123,161 <target-IP>
```

Combina puertos TCP y UDP seleccionados:

```bash
sudo nmap -sS -sU -p T:22,80,443,U:53,123,161 <target-IP>
```

Los resultados UDP pueden requerir validación adicional porque muchos servicios UDP no responden consistentemente.

---

## Comando completo de evaluación autorizada

Un escaneo detallado que guarda resultados en múltiples formatos:

```bash
sudo nmap -n -sS -sV -sC -O -p- -T3 -oA nmap-assessment <target-IP>
```

Este comando realiza:

- Sin resolución DNS.
- Escaneo TCP SYN.
- Detección de servicios y versiones.
- Scripts NSE predeterminados.
- Detección de SO.
- Escaneo de todos los puertos TCP.
- Temporización moderada.
- Salida guardada en formatos normal, XML y grepable.

Usa esto solo cuando el alcance permita el tráfico asociado y las técnicas de detección.

---

## Estados comunes de puertos

| Estado | Significado |
|---|---|
| `open` | Una aplicación está aceptando activamente conexiones en el puerto |
| `closed` | El puerto es alcanzable, pero ninguna aplicación está escuchando |
| `filtered` | Un firewall o dispositivo de filtrado impide que Nmap determine el estado |
| `open\|filtered` | Nmap no puede determinar si el puerto está abierto o filtrado |
| `closed\|filtered` | Nmap no puede determinar si el puerto está cerrado o filtrado |

El estado del puerto no es automáticamente una vulnerabilidad. Es una observación que requiere análisis adicional.

---

## Opciones comunes de Nmap

| Opción | Descripción |
|---|---|
| `-sS` | Escaneo TCP SYN |
| `-sT` | Escaneo TCP connect |
| `-sU` | Escaneo UDP |
| `-sA` | Escaneo TCP ACK |
| `-sV` | Detección de servicios y versiones |
| `-O` | Detección del sistema operativo |
| `-A` | Funciones de escaneo agresivo |
| `-sC` | Ejecutar scripts NSE predeterminados |
| `--script` | Ejecutar scripts o categorías NSE seleccionadas |
| `-p` | Especificar puertos |
| `-p-` | Escanear todos los puertos TCP |
| `--top-ports` | Escanear los puertos más comunes |
| `-Pn` | Omitir descubrimiento de host |
| `-sn` | Descubrimiento de host sin escaneo de puertos |
| `-n` | Desactivar resolución DNS |
| `-T0` a `-T5` | Plantillas de temporización |
| `-v` | Salida detallada (verbose) |
| `--reason` | Mostrar razones para estados de puertos |
| `--open` | Mostrar solo puertos abiertos o potencialmente abiertos |
| `-oN` | Guardar salida normal |
| `-oX` | Guardar salida XML |
| `-oG` | Guardar salida grepable |
| `-oA` | Guardar salida en múltiples formatos |
| `--resume` | Retomar un escaneo interrumpido |
| `--host-timeout` | Limitar tiempo de escaneo para un host |
| `--max-retries` | Limitar retransmisiones |
| `--stats-every` | Mostrar información periódica de progreso |

---

## Flujo de trabajo recomendado

```text
1. Confirmar autorización y alcance.
2. Realizar descubrimiento de host.
3. Escanear puertos comunes.
4. Escanear todos los puertos requeridos.
5. Detectar servicios y versiones.
6. Ejecutar scripts NSE cuidadosamente seleccionados.
7. Guardar la salida.
8. Validar manualmente resultados interesantes.
9. Eliminar falsos positivos.
10. Documentar solo hallazgos confirmados.
```

## Recordatorios importantes

- Un puerto abierto no es automáticamente una vulnerabilidad.
- Un banner de servicio puede ser inexacto o modificado intencionalmente.
- La detección de versiones no prueba que exista una vulnerabilidad.
- Los resultados de NSE pueden contener falsos positivos.
- La temporización agresiva puede reducir la precisión del escaneo.
- El escaneo UDP suele ser más lento y más difícil de interpretar.
- Los escaneos grandes pueden generar tráfico significativo.
- Los escaneos pueden activar alertas de firewall, IDS, IPS o SIEM.
- Mantén la salida original del escaneo como evidencia.
- Valida resultados importantes con herramientas adicionales y pruebas manuales.
- Nunca escanees sistemas fuera del alcance autorizado.
