# Enum4Linux Cheat Sheet

## Overview

Enum4Linux es una herramienta para enumerar información de sistemas Windows y Samba. Se usa para:

- Enumerar usuarios y grupos.
- Listar shares y permisos.
- Obtener información del sistema.
- Identificar malas configuraciones de seguridad.
- Llevar a cabo evaluaciones de seguridad autorizadas.

Enum4Linux proporciona:

- Enumeración automatizada de servicios SMB/CIFS.
- Enumeración de usuarios y grupos.
- Enumeración de shares y testeo de acceso.
- Extracción de políticas de contraseñas.
- Integración con otras herramientas de pentesting.

```text
Usa Enum4Linux únicamente contra sistemas que poseas o tengas autorización explícita para testear.
```

---

# 1. Starting Enum4Linux

## Sintaxis básica

```bash
enum4linux [options] <target>
```

## Mostrar ayuda

```bash
enum4linux -h
```

## Mostrar versión

```bash
enum4linux -V
```

## Actualizar Enum4Linux

```bash
git clone https://github.com/CiscoCXSecurity/enum4linux.git
cd enum4linux
```

---

# 2. Basic Enumeration

## Enumerar toda la información

```bash
enum4linux <target>
```

Ejemplo:

```bash
enum4linux 192.168.1.10
```

## Enumerar con output verbose

```bash
enum4linux -v <target>
```

Ejemplo:

```bash
enum4linux -v 192.168.1.10
```

## Enumerar con output legible por máquina

```bash
enum4linux -M <target>
```

Output en un formato adecuado para parsing.

## Enumerar objetivo específico

```bash
enum4linux 192.168.1.10
```

## Enumerar con hostname

```bash
enum4linux hostname.local
```

---

# 3. User Enumeration

## Enumerar todos los usuarios

```bash
enum4linux -U <target>
```

Ejemplo:

```bash
enum4linux -U 192.168.1.10
```

## Enumerar usuarios con RID cycling

```bash
enum4linux -U -r <target>
```

RID cycling intenta enumerar usuarios iterando a través de RIDs.

## Enumerar usuarios con rango RID específico

```bash
enum4linux -U -r <start-end> <target>
```

Ejemplo:

```bash
enum4linux -U -r 500-550 192.168.1.10
```

## Listar usuarios con descripciones

```bash
enum4linux -U <target>
```

Incluye descripciones de usuario cuando están disponibles.

## Enumerar usuarios con autenticación

```bash
enum4linux -U -u <username> -p <password> <target>
```

Ejemplo:

```bash
enum4linux -U -u guest -p '' 192.168.1.10
```

---

# 4. Group Enumeration

## Enumerar todos los grupos

```bash
enum4linux -G <target>
```

Ejemplo:

```bash
enum4linux -G 192.168.1.10
```

## Enumerar grupos con miembros

```bash
enum4linux -G <target>
```

Incluye información de membresía de grupos.

## Enumerar grupo específico

```bash
enum4linux -G -g <groupname> <target>
```

Ejemplo:

```bash
enum4linux -G -g "Domain Admins" 192.168.1.10
```

## Listar SIDs de grupos

```bash
enum4linux -G <target>
```

Muestra Security Identifiers para grupos.

---

# 5. Share Enumeration

## Enumerar todos los shares

```bash
enum4linux -S <target>
```

Ejemplo:

```bash
enum4linux -S 192.168.1.10
```

## Enumerar shares con permisos

```bash
enum4linux -S <target>
```

Muestra permisos de share y derechos de acceso.

## Listar shares accesibles

```bash
enum4linux -S <target>
```

Identifica shares a los que puedes acceder.

## Enumerar shares con autenticación

```bash
enum4linux -S -u <username> -p <password> <target>
```

Ejemplo:

```bash
enum4linux -S -u guest -p '' 192.168.1.10
```

## Comprobar shares de null session

```bash
enum4linux -N <target>
```

Testea shares accesibles con null sessions.

---

# 6. System Information

## Obtener información del SO

```bash
enum4linux -o <target>
```

Ejemplo:

```bash
enum4linux -o 192.168.1.10
```

## Obtener información del servidor

```bash
enum4linux -S <target>
```

Incluye versión del servidor e información del dominio.

## Obtener información del dominio

```bash
enum4linux -D <target>
```

Ejemplo:

```bash
enum4linux -D 192.168.1.10
```

## Obtener información de workstation

```bash
enum4linux -W <target>
```

## Obtener política de contraseñas

```bash
enum4linux -P <target>
```

Ejemplo:

```bash
enum4linux -P 192.168.1.10
```

Muestra configuración de complejidad, longitud y expiración de contraseñas.

---

# 7. Advanced Enumeration

## Enumerar todo

```bash
enum4linux -a <target>
```

Ejemplo:

```bash
enum4linux -a 192.168.1.10
```

Realiza todos los checks de enumeración.

## Enumerar con RID cycling

```bash
enum4linux -r <target>
```

Ejemplo:

```bash
enum4linux -r 192.168.1.10
```

Intenta enumerar usuarios vía RID cycling.

## Enumerar con rango RID específico

```bash
enum4linux -r <start-end> <target>
```

Ejemplo:

```bash
enum4linux -r 1000-1100 192.168.1.10
```

## Enumerar impresoras

```bash
enum4linux -p <target>
```

Ejemplo:

```bash
enum4linux -p 192.168.1.10
```

## Enumerar shares y usuarios

```bash
enum4linux -S -U <target>
```

Combina enumeración de shares y usuarios.

---

# 8. Authentication Options

## Autenticar con username y password

```bash
enum4linux -u <username> -p <password> <target>
```

Ejemplo:

```bash
enum4linux -u admin -p password123 192.168.1.10
```

## Autenticar solo con username

```bash
enum4linux -u <username> -p '' <target>
```

Ejemplo:

```bash
enum4linux -u guest -p '' 192.168.1.10
```

## Usar null session

```bash
enum4linux -N <target>
```

Intenta conectar con null session.

## Usar autenticación de dominio

```bash
enum4linux -u <username> -p <password> -d <domain> <target>
```

Ejemplo:

```bash
enum4linux -u user -p pass -d DOMAIN 192.168.1.10
```

## Usar autenticación con hash

```bash
enum4linux -u <username> -H <hash> <target>
```

Ejemplo:

```bash
enum4linux -u admin -H aad3b435b51404eeaad3b435b51404ee:hash 192.168.1.10
```

---

# 9. Output Options

## Guardar output en archivo

```bash
enum4linux <target> > output.txt
```

Ejemplo:

```bash
enum4linux -a 192.168.1.10 > enum_results.txt
```

## Output verbose

```bash
enum4linux -v <target>
```

Muestra progreso detallado de enumeración.

## Output legible por máquina

```bash
enum4linux -M <target>
```

Formato adecuado para parsing por scripts.

## Modo quiet

```bash
enum4linux -q <target>
```

Minimiza la verbosidad del output.

## Modo debug

```bash
enum4linux -d <target>
```

Muestra información debug para troubleshooting.

---

# 10. Common Attack Scenarios

## Enumeración básica

```bash
enum4linux 192.168.1.10
```

## Enumeración completa

```bash
enum4linux -a 192.168.1.10
```

## Solo enumeración de usuarios

```bash
enum4linux -U 192.168.1.10
```

## Solo enumeración de shares

```bash
enum4linux -S 192.168.1.10
```

## Solo enumeración de grupos

```bash
enum4linux -G 192.168.1.10
```

## RID cycling

```bash
enum4linux -r 192.168.1.10
```

## Check de política de contraseñas

```bash
enum4linux -P 192.168.1.10
```

## Test de null session

```bash
enum4linux -N 192.168.1.10
```

## Enumeración autenticada

```bash
enum4linux -u guest -p '' 192.168.1.10
```

## Escaneo verbose completo

```bash
enum4linux -a -v 192.168.1.10
```

---

# 11. Practical Workflows

## Flujo de trabajo básico de enumeración SMB

```text
1. Identificar objetivo Windows/Samba.
2. Ejecutar escaneo básico con enum4linux.
3. Enumerar usuarios.
4. Enumerar shares.
5. Comprobar política de contraseñas.
6. Documentar hallazgos.
```

## Ejemplo: Enumeración completa

```bash
# Escaneo básico
enum4linux 192.168.1.10

# Enumeración completa
enum4linux -a 192.168.1.10

# Output verbose
enum4linux -a -v 192.168.1.10

# Guardar resultados
enum4linux -a 192.168.1.10 > results.txt
```

## Ejemplo: Enumeración de usuarios

```bash
# Enumerar usuarios
enum4linux -U 192.168.1.10

# RID cycling
enum4linux -U -r 192.168.1.10

# Rango RID específico
enum4linux -U -r 500-550 192.168.1.10
```

## Ejemplo: Enumeración de shares

```bash
# Enumerar shares
enum4linux -S 192.168.1.10

# Comprobar shares de null session
enum4linux -N 192.168.1.10

# Enumeración de shares autenticada
enum4linux -S -u guest -p '' 192.168.1.10
```

## Ejemplo: Política de contraseñas

```bash
# Obtener política de contraseñas
enum4linux -P 192.168.1.10

# Escaneo completo con política
enum4linux -a -P 192.168.1.10
```

## Ejemplo: Ataque RID cycling

```bash
# RID cycling básico
enum4linux -r 192.168.1.10

# Rango específico
enum4linux -r 1000-1100 192.168.1.10

# Con enumeración de usuarios
enum4linux -U -r 192.168.1.10
```

---

# 12. Common Commands Reference

| Command | Description |
|---|---|
| `enum4linux -h` | Mostrar ayuda |
| `enum4linux -V` | Mostrar versión |
| `enum4linux <target>` | Enumeración básica |
| `enum4linux -a <target>` | Enumerar todo |
| `enum4linux -U <target>` | Enumerar usuarios |
| `enum4linux -G <target>` | Enumerar grupos |
| `enum4linux -S <target>` | Enumerar shares |
| `enum4linux -P <target>` | Obtener política de contraseñas |
| `enum4linux -o <target>` | Obtener información del SO |
| `enum4linux -r <target>` | RID cycling |
| `enum4linux -N <target>` | Test de null session |
| `enum4linux -v <target>` | Output verbose |
| `enum4linux -M <target>` | Output legible por máquina |
| `enum4linux -u <user> -p <pass>` | Autenticar |
| `enum4linux -d <domain>` | Especificar dominio |
| `enum4linux -H <hash>` | Usar autenticación con hash |
| `enum4linux -p <target>` | Enumerar impresoras |
| `enum4linux -W <target>` | Obtener información de workstation |
| `enum4linux -D <target>` | Obtener información del dominio |
| `enum4linux -q <target>` | Modo quiet |

---

# 13. Integration with Other Tools

## Usar con Nmap

```bash
# Escaneo SMB con Nmap
nmap -p 445 --script smb-enum-users 192.168.1.10

# Luego enum4linux
enum4linux -a 192.168.1.10
```

## Usar con Metasploit

```bash
# Enumeración SMB con Metasploit
use auxiliary/scanner/smb/smb_enumusers
set RHOSTS 192.168.1.10
run

# Luego enum4linux
enum4linux -U 192.168.1.10
```

## Usar con CrackMapExec

```bash
# Enumeración SMB con CrackMapExec
crackmapexec smb 192.168.1.10

# Luego enum4linux
enum4linux -a 192.168.1.10
```

## Usar con SMBClient

```bash
# Listar shares con smbclient
smbclient -L //192.168.1.10

# Luego enum4linux
enum4linux -S 192.168.1.10
```

---

# 14. Troubleshooting

## Connection refused

- Comprueba si el servicio SMB está en ejecución.
- Verifica que el puerto 445 o 139 está abierto.
- Asegúrate de que el objetivo es alcanzable.
- Comprueba las reglas del firewall.

## Access denied

- Prueba diferentes credenciales.
- Usa null session si está permitido.
- Comprueba los permisos del usuario.
- Verifica el método de autenticación.

## No se encontraron usuarios

- RID cycling puede estar bloqueado.
- Prueba un rango RID diferente.
- Usa enumeración autenticada.
- Comprueba si la enumeración de usuarios está permitida.

## Enumeración lenta

- Reduce el rango RID.
- Usa opciones de enumeración específicas.
- Comprueba la latencia de red.
- Verifica la capacidad de respuesta del objetivo.

## Errores de timeout

- Aumenta la configuración de timeout.
- Comprueba la conectividad de red.
- Verifica que el objetivo está online.
- Reduce el alcance de la enumeración.

---

# 15. Security Best Practices

## Verifica siempre los hallazgos

- Cruza información con otras herramientas.
- Verifica las cuentas de usuario manualmente.
- Comprueba el acceso a shares manualmente.
- Documenta todos los hallazgos.

## Respeta los límites legales

- Testea solo sistemas que poseas.
- Obtén autorización explícita.
- Sigue la divulgación responsable.
- Documenta todas las actividades.

## Minimiza el impacto

- Usa opciones de enumeración apropiadas.
- Evita RID cycling agresivo.
- Testea durante ventanas de mantenimiento.
- Monitoriza el sistema objetivo.

## Mantén las herramientas actualizadas

- Actualiza enum4linux regularmente.
- Mantente informado sobre nuevas técnicas.
- Sigue los advisories de seguridad.
- Testea en entornos controlados.

## Documenta todo

- Registra los resultados de enumeración.
- Nota los shares accesibles.
- Rastrea las cuentas de usuario.
- Documenta los problemas de seguridad.

---

# 16. Important Reminders

- Obtén siempre autorización explícita antes de usar Enum4Linux.
- Testea primero en un entorno de laboratorio controlado.
- No toda la información enumerada indica vulnerabilidades.
- Alguna enumeración puede triggerar alertas de seguridad.
- Mantén Enum4Linux actualizado regularmente.
- Valida los hallazgos manualmente; no confíes únicamente en resultados automatizados.
- Documenta todas las acciones, comandos y resultados.
- Preserva la evidencia original y los logs.
- Respeta el scope y las reglas del engagement.
- Entiende las implicaciones legales y éticas de tus acciones.

---

# 17. Quick Reference Examples

## Escaneo básico

```bash
enum4linux 192.168.1.10
```

## Enumeración completa

```bash
enum4linux -a 192.168.1.10
```

## Enumeración de usuarios

```bash
enum4linux -U 192.168.1.10
```

## Enumeración de shares

```bash
enum4linux -S 192.168.1.10
```

## Enumeración de grupos

```bash
enum4linux -G 192.168.1.10
```

## RID cycling

```bash
enum4linux -r 192.168.1.10
```

## Política de contraseñas

```bash
enum4linux -P 192.168.1.10
```

## Null session

```bash
enum4linux -N 192.168.1.10
```

## Autenticado

```bash
enum4linux -u guest -p '' 192.168.1.10
```

## Verbose

```bash
enum4linux -a -v 192.168.1.10
```

## Guardar output

```bash
enum4linux -a 192.168.1.10 > results.txt
```

---

# 18. Additional Resources

## Enum4Linux GitHub

```text
https://github.com/CiscoCXSecurity/enum4linux
```

## Documentación del protocolo SMB

```text
https://docs.microsoft.com/en-us/openspecs/
```

## Documentación de Samba

```text
https://www.samba.org/samba/docs/
```

## OWASP SMB Security

```text
https://owasp.org/www-project-web-security-testing-guide/
```

## Windows Security Baseline

```text
https://docs.microsoft.com/en-us/windows/security/
```
