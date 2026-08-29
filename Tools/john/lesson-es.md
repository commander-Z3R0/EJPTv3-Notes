# John the Ripper Cheat Sheet

## Overview

John the Ripper (John) es un cracker de contraseñas rápido utilizado para:

- Crackear hashes de contraseñas.
- Testear la fortaleza de contraseñas.
- Realizar ataques de diccionario.
- Ejecutar ataques de brute-force.
- Llevar a cabo evaluaciones de seguridad autorizadas.

John proporciona:

- Soporte para muchos tipos de hash.
- Múltiples modos de ataque.
- Ataques con wordlist y basados en reglas.
- Modo incremental (brute-force).
- Detección automática de tipo de hash.

```text
Usa John the Ripper únicamente contra hashes que poseas o tengas autorización explícita para testear.
```

---

# 1. Starting John

## Sintaxis básica

```bash
john [options] <hash-file>
```

## Mostrar ayuda

```bash
john --help
```

## Mostrar versión

```bash
john --version
```

## Actualizar John

```bash
git clone https://github.com/openwall/john.git
cd john/src
make
```

## Formatos disponibles

```bash
john --list=formats
```

---

# 2. Hash Preparation

## Identificar tipo de hash

```bash
hashid <hash>
```

O usar herramientas online para identificar el tipo de hash.

## Preparar archivo de hash

```bash
echo "hash" > hashes.txt
```

O:

```bash
cat hashes.txt
```

## Formatos de hash comunes

```bash
# MD5
hash

# SHA1
hash

# SHA256
hash

# SHA512
hash

# NTLM
hash

# WPA/WPA2
hash

# bcrypt
hash

# MD5crypt
$1$salt$hash

# SHA512crypt
$6$salt$hash
```

---

# 3. Basic Cracking

## Ataque de diccionario básico

```bash
john <hash-file>
```

Ejemplo:

```bash
john hashes.txt
```

## Ataque de diccionario con wordlist

```bash
john --wordlist=<wordlist> <hash-file>
```

Ejemplo:

```bash
john --wordlist=rockyou.txt hashes.txt
```

## Especificar formato de hash

```bash
john --format=<format> --wordlist=<wordlist> <hash-file>
```

Ejemplo:

```bash
john --format=raw-md5 --wordlist=rockyou.txt hashes.txt
```

## Mostrar contraseñas crackeadas

```bash
john --show <hash-file>
```

## Mostrar hashes restantes

```bash
john --show <hash-file> | grep -v "password hashes cracked"
```

## Eliminar hashes crackeados

```bash
john --pot=<pot-file> <hash-file>
```

---

# 4. Attack Modes

## Ataque de diccionario

```bash
john --wordlist=<wordlist> <hash-file>
```

Ejemplo:

```bash
john --wordlist=rockyou.txt hashes.txt
```

## Incremental (brute-force)

```bash
john --incremental <hash-file>
```

## Incremental con charset específico

```bash
john --incremental=lowercase <hash-file>
```

## Incremental con charset personalizado

```bash
john --incremental=custom <hash-file>
```

## Modo externo

```bash
john --external=<mode> <hash-file>
```

## Ataque híbrido

```bash
john --wordlist=<wordlist> --rules <hash-file>
```

## Ataque basado en reglas

```bash
john --wordlist=<wordlist> --rules <hash-file>
```

---

# 5. Wordlist Options

## Usar rockyou.txt

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt <hash-file>
```

## Usar múltiples wordlists

```bash
john --wordlist=wordlist1.txt,wordlist2.txt <hash-file>
```

## Usar stdin

```bash
cat wordlist.txt | john --stdin <hash-file>
```

## Generar wordlist con crunch

```bash
crunch 8 8 -t password@@@ | john --stdin <hash-file>
```

## Usar wordlist personalizada

```bash
john --wordlist=custom.txt <hash-file>
```

## Wordlist con reglas

```bash
john --wordlist=wordlist.txt --rules <hash-file>
```

---

# 6. Rules

## Usar reglas por defecto

```bash
john --wordlist=<wordlist> --rules <hash-file>
```

Ejemplo:

```bash
john --wordlist=rockyou.txt --rules hashes.txt
```

## Usar reglas específicas

```bash
john --wordlist=<wordlist> --rules=<rules> <hash-file>
```

## Listar reglas disponibles

```bash
john --list=rules
```

## Crear reglas personalizadas

Edita `john.conf` y añade la sección de reglas:

```text
[List.Rules:MyRules]
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
john --wordlist=wordlist.txt --rules=MyRules <hash-file>
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

# 7. Hash Formats

## Raw MD5

```bash
john --format=raw-md5 --wordlist=rockyou.txt hashes.txt
```

## Raw SHA1

```bash
john --format=raw-sha1 --wordlist=rockyou.txt hashes.txt
```

## Raw SHA256

```bash
john --format=raw-sha256 --wordlist=rockyou.txt hashes.txt
```

## Raw SHA512

```bash
john --format=raw-sha512 --wordlist=rockyou.txt hashes.txt
```

## NTLM

```bash
john --format=NT --wordlist=rockyou.txt hashes.txt
```

## MD5crypt

```bash
john --format=md5crypt --wordlist=rockyou.txt hashes.txt
```

## SHA512crypt

```bash
john --format=sha512crypt --wordlist=rockyou.txt hashes.txt
```

## bcrypt

```bash
john --format=bcrypt --wordlist=rockyou.txt hashes.txt
```

## WPA/WPA2

```bash
john --format=wpapsk --wordlist=rockyou.txt wifi.hccapx
```

## Kerberos

```bash
john --format=krb5-17 --wordlist=rockyou.txt hashes.txt
```

---

# 8. Performance Options

## Usar todos los cores de CPU

```bash
john --fork=<cores> <hash-file>
```

Ejemplo:

```bash
john --fork=4 hashes.txt
```

## Establecer nombre de sesión

```bash
john --session=<name> <hash-file>
```

Ejemplo:

```bash
john --session=mycrack hashes.txt
```

## Restaurar sesión

```bash
john --restore=<name>
```

## Estado de la sesión actual

```bash
john --status=<name>
```

## Limitar tiempo de ejecución

```bash
john --max-run-time=<seconds> <hash-file>
```

## Limitar longitud de contraseña

```bash
john --min-length=<min> --max-length=<max> <hash-file>
```

## Modo single crack

```bash
john --single <hash-file>
```

---

# 9. Output Options

## Mostrar contraseñas crackeadas

```bash
john --show <hash-file>
```

## Mostrar crackeadas con formato

```bash
john --show --format=<format> <hash-file>
```

## Guardar output en archivo

```bash
john --show <hash-file> > cracked.txt
```

## Mostrar solo crackeadas

```bash
john --show <hash-file> | grep -v "password hashes cracked"
```

## Mostrar estadísticas

```bash
john --show <hash-file> | tail
```

## Ubicación del potfile

```bash
~/.john/john.pot
```

## Ver potfile

```bash
cat ~/.john/john.pot
```

---

# 10. Advanced Options

## Generar wordlist desde potfile

```bash
john --show <hash-file> | cut -d: -f2 > passwords.txt
```

## Hacer potfile legible

```bash
john --show <hash-file>
```

## Continuar sesión interrumpida

```bash
john --restore
```

## Detener sesión

Presiona `Ctrl+C` durante la ejecución.

## Guardar estado de sesión

John guarda automáticamente el estado de la sesión.

## Limpiar potfile

```bash
rm ~/.john/john.pot
```

## Usar potfile específico

```bash
john --pot=<pot-file> <hash-file>
```

## Deshabilitar potfile

```bash
john --pot=none <hash-file>
```

---

# 11. Common Attack Scenarios

## Ataque de diccionario básico

```bash
john --wordlist=rockyou.txt hashes.txt
```

## Crack NTLM

```bash
john --format=NT --wordlist=rockyou.txt hashes.txt
```

## Crack MD5

```bash
john --format=raw-md5 --wordlist=rockyou.txt hashes.txt
```

## Crack SHA256

```bash
john --format=raw-sha256 --wordlist=rockyou.txt hashes.txt
```

## Crack WPA/WPA2

```bash
john --format=wpapsk --wordlist=rockyou.txt wifi.hccapx
```

## Brute-force minúsculas

```bash
john --incremental=lowercase hashes.txt
```

## Ataque basado en reglas

```bash
john --wordlist=rockyou.txt --rules hashes.txt
```

## Modo single crack

```bash
john --single hashes.txt
```

## Múltiples wordlists

```bash
john --wordlist=wordlist1.txt,wordlist2.txt hashes.txt
```

## Formato personalizado

```bash
john --format=raw-md5 --wordlist=rockyou.txt hashes.txt
```

---

# 12. Practical Workflows

## Flujo de trabajo básico de crackeo de contraseñas

```text
1. Identificar tipo de hash.
2. Preparar archivo de hash.
3. Seleccionar modo de ataque.
4. Elegir wordlist/reglas.
5. Ejecutar John.
6. Revisar contraseñas crackeadas.
7. Documentar hallazgos.
```

## Ejemplo: Crack NTLM

```bash
# Preparar hashes
echo "hash1" > hashes.txt
echo "hash2" >> hashes.txt

# Crackear con rockyou
john --format=NT --wordlist=rockyou.txt hashes.txt

# Mostrar resultados
john --show hashes.txt
```

## Ejemplo: Crack WPA/WPA2

```bash
# Convertir a hccapx
hcxpcapngtool -o wifi.hccapx capture.pcapng

# Crackear
john --format=wpapsk --wordlist=rockyou.txt wifi.hccapx

# Mostrar resultados
john --show wifi.hccapx
```

## Ejemplo: Brute-force

```bash
# Solo minúsculas
john --incremental=lowercase hashes.txt

# Brute-force completo
john --incremental hashes.txt
```

## Ejemplo: Basado en reglas

```bash
# Con reglas por defecto
john --wordlist=rockyou.txt --rules hashes.txt

# Con reglas personalizadas
john --wordlist=rockyou.txt --rules=MyRules hashes.txt
```

## Ejemplo: Múltiples formatos

```bash
# MD5
john --format=raw-md5 --wordlist=rockyou.txt hashes.txt

# SHA256
john --format=raw-sha256 --wordlist=rockyou.txt hashes.txt
```

---

# 13. Common Commands Reference

| Command | Description |
|---|---|
| `john --help` | Mostrar ayuda |
| `john --version` | Mostrar versión |
| `john <hash-file>` | Crack básico |
| `john --wordlist=<file>` | Ataque de diccionario |
| `john --incremental` | Brute-force |
| `john --rules` | Ataque basado en reglas |
| `john --format=<format>` | Especificar formato |
| `john --show` | Mostrar crackeadas |
| `john --restore` | Restaurar sesión |
| `john --status` | Mostrar estado |
| `john --session=<name>` | Nombre de sesión |
| `john --fork=<cores>` | Usar múltiples cores |
| `john --single` | Modo single crack |
| `john --stdin` | Leer desde stdin |
| `john --pot=<file>` | Especificar potfile |
| `john --list=formats` | Listar formatos |
| `john --list=rules` | Listar reglas |
| `john --max-run-time=<sec>` | Limitar tiempo de ejecución |
| `john --min-length=<min>` | Longitud mínima de contraseña |
| `john --max-length=<max>` | Longitud máxima de contraseña |

---

# 14. Troubleshooting

## No se cargaron hashes

- Comprueba el formato del archivo de hash.
- Verifica que el tipo de hash es correcto.
- Asegúrate de que los hashes no estén ya crackeados.
- Comprueba los permisos del archivo.

## Formato de hash desconocido

- Especifica el formato correcto con `--format`.
- Comprueba la sintaxis del hash.
- Elimina los hashes inválidos.
- Usa `--list=formats` para ver los formatos disponibles.

## Rendimiento lento

- Usa el modo de ataque apropiado.
- Selecciona wordlists eficientes.
- Usa `--fork` para múltiples cores.
- Considera usar Hashcat para aceleración por GPU.

## Sesión interrumpida

- Usa `--restore` para continuar.
- Comprueba que el archivo de sesión existe.
- Verifica que el potfile está intacto.
- Reinicia con los mismos parámetros.

## Hash inválido

- Verifica el tipo de hash.
- Comprueba el formato del hash.
- Elimina los hashes inválidos.
- Usa la especificación de formato correcta.

---

# 15. Security Best Practices

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
- Aprovecha múltiples cores.
- Monitoriza los recursos del sistema.

## Gestiona los recursos

- Monitoriza el uso de CPU.
- Evita el sobrecalentamiento.
- Usa carga de trabajo apropiada.
- Toma descansos entre sesiones.

## Documenta todo

- Registra todos los comandos usados.
- Nota los tipos y fuentes de hash.
- Rastrea el progreso del crackeo.
- Documenta hallazgos y métodos.

---

# 16. Important Reminders

- Obtén siempre autorización explícita antes de usar John.
- Testea primero en un entorno de laboratorio controlado.
- No todos los hashes son crackeables en tiempo razonable.
- Algunos ataques pueden tomar tiempo significativo.
- Mantén John actualizado regularmente.
- Valida los hallazgos manualmente.
- Documenta todas las acciones y comandos.
- Preserva la evidencia original y los logs.
- Entiende las implicaciones legales y éticas.
- Respeta las políticas de contraseñas y seguridad.

---

# 17. Quick Reference Examples

## Ataque de diccionario básico

```bash
john --wordlist=rockyou.txt hashes.txt
```

## Crack NTLM

```bash
john --format=NT --wordlist=rockyou.txt hashes.txt
```

## Crack MD5

```bash
john --format=raw-md5 --wordlist=rockyou.txt hashes.txt
```

## Mostrar crackeadas

```bash
john --show hashes.txt
```

## Ataque basado en reglas

```bash
john --wordlist=rockyou.txt --rules hashes.txt
```

## Brute-force

```bash
john --incremental hashes.txt
```

## Crack WPA/WPA2

```bash
john --format=wpapsk --wordlist=rockyou.txt wifi.hccapx
```

## Múltiples cores

```bash
john --fork=4 --wordlist=rockyou.txt hashes.txt
```

## Restaurar sesión

```bash
john --restore
```

## Formato personalizado

```bash
john --format=raw-sha256 --wordlist=rockyou.txt hashes.txt
```

## Modo single crack

```bash
john --single hashes.txt
```

---

# 18. Additional Resources

## John the Ripper Official

```text
http://www.openwall.com/john/
```

## John the Ripper GitHub

```text
https://github.com/openwall/john
```

## John the Ripper Wiki

```text
https://github.com/openwall/john/wiki
```

## Weakpass Wordlists

```text
https://weakpass.com/
```

## Hashcat (alternativa GPU)

```text
https://hashcat.net/hashcat/
```
