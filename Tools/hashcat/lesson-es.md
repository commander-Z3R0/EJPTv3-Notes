# Hashcat Cheat Sheet

## Overview

Hashcat es la herramienta de recuperación de contraseñas más rápida y avanzada del mundo, utilizada para:

- Crackear hashes de contraseñas.
- Testear la fortaleza de contraseñas.
- Realizar ataques de diccionario.
- Ejecutar ataques de brute-force.
- Llevar a cabo evaluaciones de seguridad autorizadas.

Hashcat proporciona:

- Soporte para más de 300 tipos de hash.
- Múltiples modos de ataque.
- Aceleración por GPU.
- Ataques basados en reglas.
- Ataques con charset y máscaras personalizadas.

```text
Usa Hashcat únicamente contra hashes que poseas o tengas autorización explícita para testear.
```

---

# 1. Starting Hashcat

## Sintaxis básica

```bash
hashcat [options] -m <hash-type> -a <attack-mode> <hash-file> <wordlist>
```

## Mostrar ayuda

```bash
hashcat -h
```

## Mostrar versión

```bash
hashcat --version
```

## Mostrar tipos de hash soportados

```bash
hashcat --help | grep -A 100 "Hash modes"
```

## Mostrar modos de ataque soportados

```bash
hashcat --help | grep -A 10 "Attack modes"
```

## Actualizar Hashcat

```bash
git clone https://github.com/hashcat/hashcat.git
cd hashcat
make
sudo make install
```

---

# 2. Hash Types

## Tipos de hash comunes

```bash
# MD5
-m 0

# MD5 con salt
-m 10

# SHA1
-m 100

# SHA256
-m 1400

# SHA512
-m 1700

# NTLM (Windows)
-m 1000

# WPA/WPA2
-m 2500

# bcrypt
-m 3200

# SHA512crypt
-m 1800

# MD5crypt
-m 500

# Kerberos TGS
-m 13100

# Bitcoin wallet
-m 11300
```

## Listar todos los tipos de hash

```bash
hashcat --help
```

## Identificar tipo de hash

```bash
hashid <hash>
```

O usar herramientas online para identificar el tipo de hash.

---

# 3. Attack Modes

## Ataque de diccionario

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist>
```

Ejemplo:

```bash
hashcat -a 0 -m 0 hashes.txt rockyou.txt
```

## Ataque combinador

```bash
hashcat -a 1 -m <hash-type> <hash-file> <wordlist1> <wordlist2>
```

Ejemplo:

```bash
hashcat -a 1 -m 0 hashes.txt wordlist1.txt wordlist2.txt
```

## Ataque de brute-force

```bash
hashcat -a 3 -m <hash-type> <hash-file> <mask>
```

Ejemplo:

```bash
hashcat -a 3 -m 0 hashes.txt ?a?a?a?a?a?a
```

## Ataque híbrido (wordlist + mask)

```bash
hashcat -a 6 -m <hash-type> <hash-file> <wordlist> <mask>
```

Ejemplo:

```bash
hashcat -a 6 -m 0 hashes.txt rockyou.txt ?d?d?d?d
```

## Ataque híbrido (mask + wordlist)

```bash
hashcat -a 7 -m <hash-type> <hash-file> <mask> <wordlist>
```

Ejemplo:

```bash
hashcat -a 7 -m 0 hashes.txt ?d?d?d?d rockyou.txt
```

## Ataque basado en reglas

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> -r <rules>
```

Ejemplo:

```bash
hashcat -a 0 -m 0 hashes.txt rockyou.txt -r rules/best64.rule
```

---

# 4. Dictionary Attacks

## Ataque de diccionario básico

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist>
```

Ejemplo:

```bash
hashcat -a 0 -m 1000 hashes.txt rockyou.txt
```

## Ataque de diccionario con múltiples wordlists

```bash
hashcat -a 0 -m <hash-type> <hash-file> wordlist1.txt wordlist2.txt
```

## Ataque de diccionario con reglas

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> -r <rules-file>
```

Ejemplo:

```bash
hashcat -a 0 -m 0 hashes.txt rockyou.txt -r rules/best64.rule
```

## Ataque de diccionario con múltiples reglas

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> -r rules1.rule -r rules2.rule
```

## Ataque de diccionario con reglas personalizadas

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> -r custom.rule
```

## Ataque de diccionario con stdout

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> --stdout
```

Muestra las contraseñas generadas sin crackear.

---

# 5. Brute-Force Attacks

## Brute-force básico

```bash
hashcat -a 3 -m <hash-type> <hash-file> <mask>
```

Ejemplo:

```bash
hashcat -a 3 -m 0 hashes.txt ?a?a?a?a?a?a
```

## Brute-force numérico

```bash
hashcat -a 3 -m <hash-type> <hash-file> ?d?d?d?d?d?d
```

Ejemplo:

```bash
hashcat -a 3 -m 0 hashes.txt ?d?d?d?d
```

## Brute-force minúsculas

```bash
hashcat -a 3 -m <hash-type> <hash-file> ?l?l?l?l?l?l
```

Ejemplo:

```bash
hashcat -a 3 -m 0 hashes.txt ?l?l?l?l
```

## Brute-force mayúsculas

```bash
hashcat -a 3 -m <hash-type> <hash-file> ?u?u?u?u?u?u
```

Ejemplo:

```bash
hashcat -a 3 -m 0 hashes.txt ?u?u?u?u
```

## Brute-force mixto

```bash
hashcat -a 3 -m <hash-type> <hash-file> ?a?a?a?a?a?a
```

## Brute-force con charset personalizado

```bash
hashcat -a 3 -m <hash-type> <hash-file> -1 <charset> <custom-mask>
```

Ejemplo:

```bash
hashcat -a 3 -m 0 hashes.txt -1 abcdef ?1?1?1?1
```

## Máscara compleja

```bash
hashcat -a 3 -m <hash-type> <hash-file> ?u?l?l?l?d?d?d
```

Ejemplo:

```bash
hashcat -a 3 -m 0 hashes.txt ?u?l?l?l?d?d?d
```

---

# 6. Mask Attack

## Definiciones de charset de máscara

```text
?l = abcdefghijklmnopqrstuvwxyz (minúsculas)
?u = ABCDEFGHIJKLMNOPQRSTUVWXYZ (mayúsculas)
?d = 0123456789 (dígitos)
?h = 0123456789abcdef (hex minúsculas)
?H = 0123456789ABCDEF (hex mayúsculas)
?s =  !"#$%&'()*+,-./:;<=>?@[\]^_`{|}~ (caracteres especiales)
?a = ?l?u?d?s (todos los imprimibles)
?b = 0x00 - 0xff (todos los bytes)
```

## Charset personalizado

```bash
hashcat -a 3 -m <hash-type> <hash-file> -1 <charset> <mask>
```

Ejemplo:

```bash
hashcat -a 3 -m 0 hashes.txt -1 custom ?1?1?1?1
```

## Múltiples charsets personalizados

```bash
hashcat -a 3 -m <hash-type> <hash-file> -1 <charset1> -2 <charset2> <mask>
```

Ejemplo:

```bash
hashcat -a 3 -m 0 hashes.txt -1 abc -2 123 ?1?1?2?2
```

## Máscara con prefijo conocido

```bash
hashcat -a 3 -m <hash-type> <hash-file> Password?d?d?d
```

## Máscara con sufijo conocido

```bash
hashcat -a 3 -m <hash-type> <hash-file> ?l?l?l?l123
```

## Ejemplo de máscara compleja

```bash
hashcat -a 3 -m 0 hashes.txt ?u?l?l?l?l?l?d?d
```

---

# 7. Rule-Based Attacks

## Usar reglas incorporadas

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> -r <rules-file>
```

Ejemplo:

```bash
hashcat -a 0 -m 0 hashes.txt rockyou.txt -r rules/best64.rule
```

## Archivos de reglas comunes

```text
rules/best64.rule
rules/d3ad0ne.rule
rules/InsidePro-HashManager.rule
rules/OneRuleToRuleThemAll.rule
rules/T0XlC.rule
```

## Crear reglas personalizadas

Crea un archivo `custom.rule`:

```text
:
$1
$2
$3
^1
^2
^3
```

## Aplicar reglas personalizadas

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> -r custom.rule
```

## Múltiples archivos de reglas

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> -r rules1.rule -r rules2.rule
```

## Generar reglas con stats

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> -r rules/best64.rule --stdout
```

## Ejemplos de reglas

```text
:          - No hacer nada
$1         - Añadir 1 al final
$2         - Añadir 2 al final
^1         - Añadir 1 al principio
c          - Capitalizar primera letra
C          - Capitalizar todas las letras
l          - Minúsculas todas las letras
u          - Mayúsculas todas las letras
i          - Invertir caso
d          - Duplicar palabra
p          - Permutar caracteres
```

---

# 8. Performance Options

## Especificar dispositivo GPU

```bash
hashcat -d <device-id> -m <hash-type> <hash-file> <wordlist>
```

Ejemplo:

```bash
hashcat -d 1 -m 0 hashes.txt rockyou.txt
```

## Usar todas las GPUs

```bash
hashcat -d 1,2,3,4 -m <hash-type> <hash-file> <wordlist>
```

## Establecer perfil de workload

```bash
hashcat -w <profile> -m <hash-type> <hash-file> <wordlist>
```

Perfiles:

- 1 - Low (optimizado para background)
- 2 - Default (balanceado)
- 3 - High (optimizado para velocidad)
- 4 - Insane (máximo rendimiento)

## Limitar tiempo de kernel

```bash
hashcat --kernel-timeout <seconds> -m <hash-type> <hash-file> <wordlist>
```

## Establecer número de threads

```bash
hashcat -t <threads> -m <hash-type> <hash-file> <wordlist>
```

## Usar solo CPU

```bash
hashcat --force -m <hash-type> <hash-file> <wordlist>
```

---

# 9. Output Options

## Especificar archivo de output

```bash
hashcat -o <output-file> -m <hash-type> <hash-file> <wordlist>
```

Ejemplo:

```bash
hashcat -o cracked.txt -m 0 hashes.txt rockyou.txt
```

## Mostrar contraseñas crackeadas

```bash
hashcat --show -m <hash-type> <hash-file>
```

## Mostrar hashes restantes

```bash
hashcat --left -m <hash-type> <hash-file>
```

## Formato de output

```bash
hashcat -o <output-file> --outfile-format <format> -m <hash-type> <hash-file> <wordlist>
```

Formatos:

- 1 - Hash:Plain
- 2 - Plain
- 3 - Hex-Plain
- 4 - Crack-Pos
- 5 - Timestamp:Plain

## Output verbose

```bash
hashcat -v -m <hash-type> <hash-file> <wordlist>
```

## Modo quiet

```bash
hashcat -q -m <hash-type> <hash-file> <wordlist>
```

## Modo debug

```bash
hashcat --debug-mode <mode> -m <hash-type> <hash-file> <wordlist>
```

---

# 10. Session Management

## Especificar nombre de sesión

```bash
hashcat --session <name> -m <hash-type> <hash-file> <wordlist>
```

Ejemplo:

```bash
hashcat --session mycrack -m 0 hashes.txt rockyou.txt
```

## Listar sesiones

```bash
hashcat --list
```

## Restaurar sesión

```bash
hashcat --restore --session <name>
```

## Eliminar sesión

```bash
hashcat --remove --session <name>
```

## Guardar progreso

```bash
hashcat --checkpoint-disable -m <hash-type> <hash-file> <wordlist>
```

## Pausar sesión

Presiona `p` durante la ejecución para pausar.

## Resumir sesión

Presiona `s` durante la ejecución para resumir.

## Salir de sesión

Presiona `q` durante la ejecución para salir.

---

# 11. Advanced Options

## Gestión de potfile

```bash
# Mostrar potfile
hashcat --show -m <hash-type> <hash-file>

# Deshabilitar potfile
hashcat --potfile-disable -m <hash-type> <hash-file> <wordlist>

# Potfile personalizado
hashcat --potfile-path <path> -m <hash-type> <hash-file> <wordlist>
```

## Ataque loopback

```bash
hashcat -a 0 -m <hash-type> <hash-file> <wordlist> --loopback
```

## Deshabilitar self-test

```bash
hashcat --self-test-disable -m <hash-type> <hash-file> <wordlist>
```

## Forzar ejecución

```bash
hashcat --force -m <hash-type> <hash-file> <wordlist>
```

## Kernel optimizado

```bash
hashcat -O -m <hash-type> <hash-file> <wordlist>
```

## Spinup delay

```bash
hashcat --spinup-damp <percent> -m <hash-type> <hash-file> <wordlist>
```

## Status timer

```bash
hashcat --status-timer <seconds> -m <hash-type> <hash-file> <wordlist>
```

---

# 12. Common Attack Scenarios

## Crack MD5

```bash
hashcat -a 0 -m 0 hashes.txt rockyou.txt
```

## Crack SHA256

```bash
hashcat -a 0 -m 1400 hashes.txt rockyou.txt
```

## Crack NTLM

```bash
hashcat -a 0 -m 1000 hashes.txt rockyou.txt
```

## Crack WPA/WPA2

```bash
hashcat -a 0 -m 2500 wifi.hccapx rockyou.txt
```

## Crack bcrypt

```bash
hashcat -a 0 -m 3200 hashes.txt rockyou.txt
```

## Brute-force 8 chars

```bash
hashcat -a 3 -m 0 hashes.txt ?a?a?a?a?a?a?a?a
```

## Ataque basado en reglas

```bash
hashcat -a 0 -m 0 hashes.txt rockyou.txt -r rules/best64.rule
```

## Ataque híbrido

```bash
hashcat -a 6 -m 0 hashes.txt rockyou.txt ?d?d?d?d
```

## Máscara personalizada

```bash
hashcat -a 3 -m 0 hashes.txt -1 abcdef ?1?1?1?1?1?1
```

## Múltiples wordlists

```bash
hashcat -a 0 -m 0 hashes.txt wordlist1.txt wordlist2.txt
```

---

# 13. Practical Workflows

## Flujo de trabajo básico de crackeo de contraseñas

```text
1. Identificar tipo de hash.
2. Preparar archivo de hash.
3. Seleccionar modo de ataque.
4. Elegir wordlist/reglas.
5. Ejecutar hashcat.
6. Revisar contraseñas crackeadas.
7. Documentar hallazgos.
```

## Ejemplo: Crack NTLM

```bash
# Preparar hashes
echo "hash1" > hashes.txt
echo "hash2" >> hashes.txt

# Crackear con rockyou
hashcat -a 0 -m 1000 hashes.txt rockyou.txt

# Mostrar resultados
hashcat --show -m 1000 hashes.txt
```

## Ejemplo: Crack WPA/WPA2

```bash
# Convertir a hccapx
hcxpcapngtool -o wifi.hccapx capture.pcapng

# Crackear
hashcat -a 0 -m 2500 wifi.hccapx rockyou.txt

# Mostrar resultados
hashcat --show -m 2500 wifi.hccapx
```

## Ejemplo: Brute-force

```bash
# 6 caracteres minúsculas
hashcat -a 3 -m 0 hashes.txt ?l?l?l?l?l?l

# 8 caracteres mixtos
hashcat -a 3 -m 0 hashes.txt ?a?a?a?a?a?a?a?a
```

## Ejemplo: Basado en reglas

```bash
# Con reglas best64
hashcat -a 0 -m 0 hashes.txt rockyou.txt -r rules/best64.rule

# Con reglas personalizadas
hashcat -a 0 -m 0 hashes.txt rockyou.txt -r custom.rule
```

## Ejemplo: Ataque híbrido

```bash
# Wordlist + 4 dígitos
hashcat -a 6 -m 0 hashes.txt rockyou.txt ?d?d?d?d

# 4 dígitos + wordlist
hashcat -a 7 -m 0 hashes.txt ?d?d?d?d rockyou.txt
```

---

# 14. Common Commands Reference

| Command | Description |
|---|---|
| `hashcat -h` | Mostrar ayuda |
| `hashcat --version` | Mostrar versión |
| `hashcat -a 0` | Ataque de diccionario |
| `hashcat -a 1` | Ataque combinador |
| `hashcat -a 3` | Ataque de brute-force |
| `hashcat -a 6` | Ataque híbrido (wordlist + mask) |
| `hashcat -a 7` | Ataque híbrido (mask + wordlist) |
| `hashcat -m <type>` | Especificar tipo de hash |
| `hashcat -o <file>` | Archivo de output |
| `hashcat -r <rules>` | Usar archivo de reglas |
| `hashcat -d <device>` | Especificar dispositivo GPU |
| `hashcat -w <profile>` | Perfil de workload |
| `hashcat --show` | Mostrar crackeadas |
| `hashcat --left` | Mostrar restantes |
| `hashcat --restore` | Restaurar sesión |
| `hashcat --session <name>` | Nombre de sesión |
| `hashcat -O` | Kernel optimizado |
| `hashcat --force` | Forzar ejecución |
| `hashcat -v` | Output verbose |
| `hashcat -q` | Modo quiet |
| `hashcat --stdout` | Output a stdout |

---

# 15. Troubleshooting

## No se cargaron hashes

- Comprueba el formato del archivo de hash.
- Verifica que el tipo de hash es correcto.
- Asegúrate de que los hashes no estén ya crackeados.
- Comprueba los permisos del archivo.

## Dispositivo no encontrado

- Actualiza los drivers de la GPU.
- Comprueba la instalación de OpenCL.
- Verifica que el dispositivo es detectado: `hashcat -I`
- Prueba con un ID de dispositivo diferente.

## Out of memory

- Reduce el perfil de workload.
- Usa wordlists más pequeñas.
- Cierra otras aplicaciones de GPU.
- Usa CPU en su lugar.

## Rendimiento lento

- Aumenta el perfil de workload.
- Usa kernel optimizado `-O`.
- Comprueba la utilización de GPU.
- Verifica la refrigeración y thermal throttling.

## Hash inválido

- Verifica el tipo de hash.
- Comprueba el formato del hash.
- Elimina los hashes inválidos.
- Usa `--force` si es necesario.

---

# 16. Security Best Practices

## Verifica siempre los resultados

- Testea las contraseñas crackeadas manualmente.
- Verifica que el tipo de hash es correcto.
- Cruza información con otras herramientas.
- Documenta todos los hallazgos.

## Respeta los límites legales

- Solo crackea hashes que poseas.
- Obtén autorización explícita.
- Sigue la divulgación responsable.
- Documenta todas las actividades.

## Optimiza el rendimiento

- Usa modos de ataque apropiados.
- Selecciona wordlists eficientes.
- Aprovecha la aceleración por GPU.
- Monitoriza los recursos del sistema.

## Gestiona los recursos

- Monitoriza la temperatura de la GPU.
- Evita el sobrecalentamiento.
- Usa perfiles de workload apropiados.
- Toma descansos entre sesiones.

## Documenta todo

- Registra todos los comandos usados.
- Nota los tipos y fuentes de hash.
- Rastrea el progreso del crackeo.
- Documenta hallazgos y métodos.

---

# 17. Important Reminders

- Obtén siempre autorización explícita antes de usar Hashcat.
- Testea primero en un entorno de laboratorio controlado.
- No todos los hashes son crackeables en tiempo razonable.
- Algunos ataques pueden tomar tiempo significativo.
- Mantén Hashcat actualizado regularmente.
- Valida los hallazgos manualmente.
- Documenta todas las acciones y comandos.
- Preserva la evidencia original y los logs.
- Entiende las implicaciones legales y éticas.
- Respeta las políticas de contraseñas y seguridad.

---

# 18. Quick Reference Examples

## Ataque de diccionario básico

```bash
hashcat -a 0 -m 0 hashes.txt rockyou.txt
```

## Brute-force 6 chars

```bash
hashcat -a 3 -m 0 hashes.txt ?a?a?a?a?a?a
```

## Ataque basado en reglas

```bash
hashcat -a 0 -m 0 hashes.txt rockyou.txt -r rules/best64.rule
```

## Crack NTLM

```bash
hashcat -a 0 -m 1000 hashes.txt rockyou.txt
```

## Crack WPA/WPA2

```bash
hashcat -a 0 -m 2500 wifi.hccapx rockyou.txt
```

## Mostrar crackeadas

```bash
hashcat --show -m 0 hashes.txt
```

## Mostrar restantes

```bash
hashcat --left -m 0 hashes.txt
```

## Máscara personalizada

```bash
hashcat -a 3 -m 0 hashes.txt -1 abcdef ?1?1?1?1?1?1
```

## Ataque híbrido

```bash
hashcat -a 6 -m 0 hashes.txt rockyou.txt ?d?d?d?d
```

## Múltiples wordlists

```bash
hashcat -a 0 -m 0 hashes.txt wordlist1.txt wordlist2.txt
```

## Kernel optimizado

```bash
hashcat -O -a 0 -m 0 hashes.txt rockyou.txt
```

---

# 19. Additional Resources

## Hashcat Official

```text
https://hashcat.net/hashcat/
```

## Hashcat GitHub

```text
https://github.com/hashcat/hashcat
```

## Hashcat Rules

```text
https://github.com/hashcat/hashcat/tree/master/rules
```

## Hashcat Wiki

```text
https://hashcat.net/wiki/
```

## Weakpass Wordlists

```text
https://weakpass.com/
```
