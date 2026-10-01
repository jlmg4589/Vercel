---
{"dg-publish":true,"permalink":"/bastionado-de-redes-y-sistemas/anexos/anexo-i-scripting-en-bash-para-el-bastionado/","tags":["asignatura/bastionado-de-redes-y-sistemas","anexo","bash","scripting"],"dgShowToc":true,"noteIcon":"","dg-note-properties":{"tipo":"anexo","asignatura":"Bastionado de redes y sistemas","autor":"José Luis Martínez García","licencia":"CC BY-NC-SA 4.0","fuente":"BASTIONADO/Repaso de Bash.pdf (revisado, corregido y ampliado)","tags":["asignatura/bastionado-de-redes-y-sistemas","anexo","bash","scripting"]}}
---


# Anexo I. Scripting en Bash para el bastionado

↑ [[Bastionado de redes y sistemas/Bastionado de redes y sistemas\|Bastionado de redes y sistemas]]

> [!info] Por qué este anexo
> Casi todas las tareas del módulo —auditar usuarios y permisos ([[Bastionado de redes y sistemas/Unidad 2 Control de acceso y autenticación/Prácticas/P2 - UD2 Permisos, ACL y atributos\|P2 - UD2 Permisos, ACL y atributos]]), cargar reglas de cortafuegos ([[Bastionado de redes y sistemas/Unidad 5 Configuración de dispositivos y sistemas/Prácticas/P1 - UD5 Cortafuegos con iptables\|P1 - UD5 Cortafuegos con iptables]], [[Bastionado de redes y sistemas/Unidad 5 Configuración de dispositivos y sistemas/Prácticas/P2 - UD5 Cortafuegos con nftables\|P2 - UD5 Cortafuegos con nftables]]), endurecer SSH ([[Bastionado de redes y sistemas/Unidad 7 Configuración de sistemas informáticos/Prácticas/P2 - UD7 Securización del servidor SSH\|P2 - UD7 Securización del servidor SSH]]) o automatizar copias ([[Bastionado de redes y sistemas/Unidad 7 Configuración de sistemas informáticos/Prácticas/P6 - UD7 Copias de seguridad\|P6 - UD7 Copias de seguridad]])— se vuelven **repetibles, documentables y verificables** cuando se escriben como *scripts*. Un bastionado que no se puede reproducir no se puede auditar.
>
> Este anexo repasa Bash (variables, comandos, *scripts*, estructuras de control y funciones) y añade lo necesario para escribir *scripts* **seguros** y útiles en tareas de seguridad.

## 1. Variables

### 1.1 Definición y consulta

En Bash las variables **no se declaran**: se crean al asignarles un valor. Si se consulta una variable que no existe, se expande a una **cadena vacía** (no da error, salvo que se use `set -u`).

```bash
variable="HOLA"        # SIN espacios alrededor del =
echo "$variable"       # HOLA
variable1="$variable"  # copia el valor de otra variable
echo "$variable1"      # HOLA
```

> [!warning] Errores frecuentes
> - `variable = "valor"` **falla**: Bash interpreta `variable` como un comando con los argumentos `=` y `"valor"`. La asignación no admite espacios.
> - Usa siempre **comillas dobles** al expandir: `"$variable"`. Sin ellas, un valor con espacios o comodines (`*`) se trocea y expande, lo que provoca fallos y vulnerabilidades (por ejemplo, `rm $fichero` con `fichero="/ *"`).
> - Usa `${variable}` cuando la variable va pegada a otro texto: `echo "${usuario}_backup"`.

### 1.2 Guardar la salida de un comando (sustitución de comandos)

```bash
fecha=$(date +%F)           # forma recomendada (anidable y legible)
kernel=`uname -r`           # forma antigua con acentos graves: evitar
echo "Copia del $fecha en kernel $kernel"
```

### 1.3 Aritmética

```bash
a=5; b=3
echo $(( a + b ))      # 8
(( a++ ))              # incrementa a
echo $(( 17 % 5 ))     # 2 (resto)
```

Bash solo trabaja con **enteros**; para decimales usa `bc` o `awk`.

### 1.4 Ámbito de las variables

1. Por defecto una variable **solo existe en el shell donde se crea** y desaparece al terminar ese proceso. Un shell hijo no la ve.
2. `export` copia la variable al **entorno de los procesos hijos** (nunca al revés: lo que haga el hijo no afecta al padre).

```bash
x=10
bash -c 'echo "hijo: x=$x"'   # hijo: x=   (no la conoce)
export x
bash -c 'echo "hijo: x=$x"; x=99'   # hijo: x=10
echo "padre: x=$x"            # padre: x=10 (el hijo solo modificó su copia)
```

Para que un *script* modifique variables del shell actual hay que **cargarlo** con `source script.sh` (o `. script.sh`) en lugar de ejecutarlo.

3. Dentro de funciones, `local` limita la variable a la función (ver §5.7).

### 1.5 Variables de entorno

Por convención se escriben en **mayúsculas** para distinguirlas de las del usuario (no es obligatorio). Cada usuario tiene **su propio** entorno: se carga al iniciar sesión (`/etc/profile`, `~/.profile`, `~/.bashrc`…) y dura hasta su cierre.

| Variable | Significado |
|---|---|
| `HOME` | Directorio personal |
| `USER` | Nombre del usuario |
| `UID` / `EUID` | Identificador real / efectivo del usuario (0 = root) |
| `SHELL` | Shell de inicio de sesión del usuario |
| `HOSTNAME` | Nombre del equipo |
| `PATH` | Directorios donde se buscan los comandos, separados por `:` |
| `PWD` / `OLDPWD` | Directorio actual / anterior (lo usa `cd -`) |
| `RANDOM` | Entero pseudoaleatorio entre 0 y 32767 |
| `PS1` | Formato del *prompt*: `\u` usuario, `\h` equipo, `\w` directorio, `\$` (`#` si es root, `$` si no) |
| `HISTFILE`, `HISTSIZE` | Fichero y tamaño del historial |
| `IFS` | Separadores de campo (espacio, tabulador, salto de línea) |

> [!danger] Seguridad: `RANDOM` y `PATH`
> - `$RANDOM` **no es criptográficamente seguro**: no lo uses para contraseñas, *tokens* ni claves. Usa `openssl rand -base64 24` o `head -c 32 /dev/urandom | base64`.
> - Nunca incluyas `.` ni directorios escribibles por otros en el `PATH` de root: un atacante podría dejar un `ls` malicioso (secuestro de `PATH`). En *scripts* privilegiados fija el `PATH` al principio o usa rutas absolutas.
> - El historial puede guardar contraseñas tecleadas en la línea de órdenes. `HISTCONTROL=ignorespace` evita guardar las órdenes que empiezan por espacio.

## 2. Comandos básicos para variables y E/S

| Comando | Uso |
|---|---|
| `unset var` | Elimina la variable (después se expande a vacío) |
| `set` | Muestra **todas** las variables (locales y de entorno) y funciones; también activa opciones del shell (`set -euo pipefail`) |
| `env` / `printenv` | Muestra solo las variables de **entorno** |
| `export VAR=valor` | Crea/exporta una variable de entorno |
| `readonly VAR=valor` | Constante: no se puede modificar ni borrar |
| `declare -i n` / `declare -a v` / `declare -A m` | Entero / *array* indexado / *array* asociativo |

### 2.1 `echo` y `printf`

```bash
echo -n "Sin salto de línea"
echo -e "Col1\tCol2\nLínea nueva"   # -e interpreta \t, \n, \c (\c corta la salida aquí)
printf "%-10s %5d\n" "usuario" 1001 # formato preciso y portable
```

`printf` es preferible a `echo -e` en *scripts*: su comportamiento es igual en todos los sistemas y no interpreta por error datos que empiecen por `-`.

### 2.2 `read`

Lee un valor de la entrada estándar y lo asigna a una variable. Sintaxis: `read [opciones] variable`.

| Opción | Efecto |
|---|---|
| `-p "texto"` | Muestra un mensaje antes de leer |
| `-s` | Entrada silenciosa (contraseñas) |
| `-t N` | Tiempo máximo de espera de N segundos |
| `-r` | No interpreta `\` como escape (**úsala siempre**) |
| `-n N` | Lee solo N caracteres |
| `-a arr` | Guarda las palabras en un *array* |

```bash
read -rp "Usuario: " usuario
read -rsp "Contraseña: " clave; echo
read -rt 10 -p "¿Continuar? (s/n) " resp || echo "Tiempo agotado"
```

> [!tip] Nunca pases contraseñas como argumento (`script.sh miClave`): quedan en el historial y son visibles para cualquier usuario con `ps`. Pídelas con `read -s`, por la entrada estándar o desde un fichero con permisos `600`.

### 2.3 Redirecciones y tuberías

```bash
cmd > fich       # stdout a fichero (sobrescribe)
cmd >> fich      # stdout añadiendo
cmd 2> err.log   # stderr a fichero
cmd &> todo.log  # stdout y stderr
cmd 2>/dev/null  # descartar errores
cmd1 | cmd2      # salida de cmd1 como entrada de cmd2
cmd | tee -a log # ver por pantalla y guardar
```

## 3. Shell scripts

### 3.1 Concepto

Un *script* (guion) es un fichero de texto plano con órdenes que el intérprete ejecuta en secuencia. Se usan para tareas repetitivas o que deben hacerse siempre igual: exactamente lo que exige un bastionado.

### 3.2 Estructura

- **Primera línea (*shebang*)**: indica el intérprete. `#!/bin/bash` o, más portable, `#!/usr/bin/env bash`. Si se usa `#!/bin/sh`, en Debian/Ubuntu se ejecuta `dash`, que **no** admite `[[ ]]`, *arrays*, etc.
- Las líneas que empiezan por `#` (salvo el *shebang*) son **comentarios**.
- La extensión `.sh` no es obligatoria en Linux, pero ayuda a identificar el fichero.

### 3.3 Crear y ejecutar un script

1. Crear el fichero, por ejemplo con `nano hola.sh`.
2. Dar permiso de ejecución **solo a quien lo necesite**:

   ```bash
   chmod u+x hola.sh     # o chmod 700 / 750 según el caso
   ```

3. Ejecutarlo indicando la ruta (`./hola.sh`) o situándolo en un directorio del `PATH` (p. ej. `/usr/local/sbin` para *scripts* de administración).

También puede ejecutarse sin permiso de ejecución con `bash hola.sh`.

> [!danger] Corrección importante: no uses `chmod 777`
> El material original proponía `sudo chmod 777 script.sh`. Es **incorrecto** y contrario a cualquier bastionado:
> - `777` permite a **cualquier usuario modificar** el *script*. Si luego lo ejecuta root (o `cron` como root), un atacante obtiene escalada de privilegios inmediata.
> - Si eres el propietario **no necesitas `sudo`**.
>
> Recomendado: `chmod 700` (solo el propietario) o `chmod 750` con un grupo de administradores; propietario `root:root` para *scripts* que ejecuta root, en un directorio no escribible por otros.

### 3.4 Variables especiales

| Variable | Función |
|---|---|
| `$0` | Nombre (ruta) del *script* |
| `$1` … `$9`, `${10}` | Parámetros posicionales |
| `$#` | Número de parámetros |
| `"$@"` | Todos los parámetros, **cada uno como palabra separada** (la forma correcta de reenviarlos) |
| `"$*"` | Todos los parámetros en **una sola cadena** |
| `$?` | Código de salida del último comando (0 = éxito, ≠0 = error) |
| `$$` | PID del shell que ejecuta el *script* |
| `$!` | PID del último proceso lanzado en segundo plano |
| `shift` | Desplaza los parámetros: `$2` pasa a ser `$1`… |

```bash
#!/usr/bin/env bash
echo "Script: $0 | Nº args: $# | PID: $$"
for arg in "$@"; do echo "-> $arg"; done
ls /noexiste 2>/dev/null; echo "Código de salida: $?"
```

### 3.5 Códigos de salida

Todo *script* debe terminar con `exit 0` si va bien y con un valor distinto de 0 si falla; así lo pueden encadenar otros *scripts*, `cron`, `systemd` o una herramienta de monitorización.

```bash
cmd1 && cmd2   # cmd2 solo si cmd1 tiene éxito
cmd1 || cmd2   # cmd2 solo si cmd1 falla
```

## 4. Comprobaciones (condiciones)

Se usan con `test`, `[ ]` o `[[ ]]`. `test 1 -lt 2` equivale a `[ 1 -lt 2 ]`. **Los espacios tras `[` y antes de `]` son obligatorios** porque `[` es un comando.

En *scripts* Bash se recomienda `[[ ]]`: es propio de Bash y más seguro, porque no trocea las variables sin comillas y admite comodines, expresiones regulares (`=~`) y los operadores lógicos `&&` y `||`.

### 4.1 Números

| Operador | Significado |
|---|---|
| `-eq` / `-ne` | igual / distinto |
| `-lt` / `-le` | menor / menor o igual |
| `-gt` / `-ge` | mayor / mayor o igual |

En aritmética también puede usarse `(( a > b ))`.

### 4.2 Cadenas

| Expresión | Verdadera si… |
|---|---|
| `"$a" = "$b"` (o `==` en `[[ ]]`) | son iguales |
| `"$a" != "$b"` | son distintas |
| `-z "$a"` | la cadena está vacía |
| `-n "$a"` | la cadena no está vacía |
| `[[ $a =~ ^[0-9]+$ ]]` | cumple la expresión regular |
| `[[ $a == *.log ]]` | encaja con el patrón |

### 4.3 Ficheros y directorios

| Expresión | Verdadera si… |
|---|---|
| `-e ruta` | existe |
| `-f ruta` | es un fichero regular |
| `-d ruta` | es un directorio |
| `-L ruta` | es un enlace simbólico |
| `-s ruta` | existe y no está vacío |
| `-r` / `-w` / `-x ruta` | el usuario actual puede leer / escribir / ejecutar |
| `-O ruta` | el usuario actual es el propietario |
| `-u` / `-g` / `-k ruta` | tiene SUID / SGID / *sticky bit* |
| `r1 -nt r2` | r1 es **más reciente** que r2 |
| `r1 -ot r2` | r1 es **más antiguo** que r2 |

### 4.4 Operadores lógicos

| Dentro de `[ ]` | Dentro de `[[ ]]` / entre comandos | Significado |
|---|---|---|
| `!` | `!` | negación |
| `-a` (obsoleto) | `&&` | Y |
| `-o` (obsoleto) | `\|\|` | O |

```bash
[[ -f "$f" && -r "$f" ]] && echo "Fichero legible"
[ -f "$f" ] && [ -r "$f" ] && echo "Equivalente con test"
```

## 5. Estructuras de control

### 5.1 `if`

```bash
if [[ $EUID -ne 0 ]]; then
    echo "Este script debe ejecutarse como root" >&2
    exit 1
elif [[ ! -f /etc/ssh/sshd_config ]]; then
    echo "OpenSSH no está instalado" >&2
    exit 2
else
    echo "Comprobaciones iniciales correctas"
fi
```

`if` evalúa el **código de salida de un comando**, no solo corchetes:

```bash
if grep -q '^PermitRootLogin no' /etc/ssh/sshd_config; then
    echo "OK: root no puede entrar por SSH"
fi
```

### 5.2 `case`

```bash
case "$1" in
    start|iniciar)  echo "Arrancando...";;
    stop)           echo "Parando...";;
    [0-9]*)         echo "Has introducido un número";;
    *)              echo "Uso: $0 {start|stop}" >&2; exit 1;;
esac
```

Cada rama termina en `;;` y `*)` actúa como opción por defecto.

### 5.3 `while` (mientras se cumpla)

```bash
i=1
while (( i <= 5 )); do
    echo "Iteración $i"
    (( i++ ))
done
```

Leer un fichero línea a línea (forma correcta):

```bash
while IFS=: read -r usuario _ uid _ _ home shell; do
    echo "$usuario ($uid) -> $shell"
done < /etc/passwd
```

### 5.4 `until` (hasta que se cumpla)

```bash
until ping -c1 -W1 192.168.1.1 &>/dev/null; do
    echo "Esperando a la puerta de enlace..."
    sleep 2
done
echo "Red disponible"
```

### 5.5 `for`

```bash
for i in {1..5}; do echo "$i"; done              # rango
for (( i=0; i<5; i++ )); do echo "$i"; done      # estilo C
for f in /var/log/*.log; do echo "$f"; done      # ficheros (no uses `for f in $(ls)`)
for ip in 192.168.1.{1..254}; do
    ping -c1 -W1 "$ip" &>/dev/null && echo "$ip activo"
done
```

### 5.6 `break` y `continue`

`break` sale del bucle; `continue` salta a la siguiente iteración.

### 5.7 Funciones

Bloques de código reutilizables que reciben parámetros igual que un *script* (`$1`, `$2`, `$#`, `"$@"`). **Deben definirse antes de llamarlas.**

```bash
#!/usr/bin/env bash
log() {
    local nivel="$1"; shift
    printf '%s [%s] %s\n' "$(date '+%F %T')" "$nivel" "$*" | tee -a /var/log/bastionado.log
}

es_numero() {
    [[ $1 =~ ^[0-9]+$ ]]      # el código de salida es el resultado
}

log INFO "Inicio de la auditoría"
es_numero "42" && log INFO "42 es un número"
```

- `local` evita que las variables de la función sobrescriban las globales.
- `return N` devuelve un **código de estado** (0-255), no datos. Para devolver datos, la función hace `echo` y se recoge con `res=$(funcion)`.

## 6. Scripts seguros: buenas prácticas

> [!important] Plantilla recomendada
> ```bash
> #!/usr/bin/env bash
> # Descripción: ...   Autor: ...   Versión: 1.0   Fecha: ...
> set -Eeuo pipefail          # modo estricto
> IFS=$'\n\t'
> umask 077                   # ficheros creados solo legibles por el propietario
> export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
> readonly LOG=/var/log/$(basename "$0" .sh).log
>
> trap 'echo "Error en línea $LINENO" >&2' ERR
>
> [[ $EUID -eq 0 ]] || { echo "Ejecutar como root" >&2; exit 1; }
>
> main() {
>     # ...
>     :
> }
> main "$@"
> ```

| Práctica | Motivo |
|---|---|
| `set -e` | Detiene el *script* si un comando falla |
| `set -u` | Error si se usa una variable no definida (evita `rm -rf "$DIR/"` con `DIR` vacío) |
| `set -o pipefail` | Una tubería falla si falla cualquiera de sus comandos |
| Comillas dobles en todas las expansiones | Evita troceado, expansión de comodines e inyección |
| Validar entradas (`[[ $x =~ ^[a-z_][a-z0-9_-]*$ ]]`) | Nunca confiar en argumentos ni en datos leídos |
| No usar `eval` | Ejecuta como código cualquier dato: inyección de comandos |
| Ficheros temporales con `mktemp` y `trap 'rm -f "$tmp"' EXIT` | Evita condiciones de carrera y ataques por enlace simbólico en `/tmp` |
| `--` antes de rutas: `rm -- "$f"` | Impide que un nombre como `-rf` se tome como opción |
| Rutas absolutas o `PATH` fijado | Evita el secuestro de `PATH` |
| No guardar secretos en el *script* | Usar ficheros `600`, variables de entorno o un gestor de secretos |
| Registrar acciones (`logger -t mi_script "..."`) | Trazabilidad en `syslog`/`journald` |
| Analizar con **ShellCheck** (`shellcheck script.sh`) | Detecta errores y malas prácticas automáticamente |
| Probar con `bash -n` (sintaxis) y `bash -x` (traza) | Depuración |
| Ejecutar con el mínimo privilegio | Usar `sudo` solo para las órdenes necesarias, nunca `chmod 777` |
| Control de versiones (Git) | Historial y revisión de cambios del bastionado |

> [!warning] Scripts con SUID
> Linux **ignora el bit SUID en scripts** precisamente porque son inseguros. Si un usuario debe ejecutar un *script* con privilegios, se configura una regla concreta en `sudoers` (`visudo`), con ruta absoluta y sin comodines.

## 7. Automatización

### 7.1 `cron`

```bash
crontab -e            # crontab del usuario
# min hora día mes díasemana  orden
30 2 * * * /usr/local/sbin/backup.sh >> /var/log/backup.log 2>&1
```

Recuerda: `cron` usa un `PATH` mínimo, por eso conviene fijarlo en el *script*. Restringe quién puede usarlo con `/etc/cron.allow` y revisa `/etc/crontab`, `/etc/cron.d/` y los *crontabs* de usuarios en las auditorías (es un mecanismo de **persistencia** muy usado por atacantes).

### 7.2 Temporizadores de `systemd`

Alternativa moderna, con registro en `journald` y control de dependencias:

```ini
# /etc/systemd/system/auditoria.service
[Unit]
Description=Auditoría diaria de bastionado
[Service]
Type=oneshot
ExecStart=/usr/local/sbin/auditoria.sh

# /etc/systemd/system/auditoria.timer
[Unit]
Description=Ejecuta la auditoría a diario
[Timer]
OnCalendar=daily
Persistent=true
[Install]
WantedBy=timers.target
```

```bash
systemctl daemon-reload && systemctl enable --now auditoria.timer
systemctl list-timers
```

Ver también [[Bastionado de redes y sistemas/Unidad 7 Configuración de sistemas informáticos/Prácticas/P1 - UD7 Gestión de servicios con systemd y TCP Wrappers\|P1 - UD7 Gestión de servicios con systemd y TCP Wrappers]].

## 8. Scripts de ejemplo para el bastionado

### 8.1 Auditoría rápida de cuentas y permisos

```bash
#!/usr/bin/env bash
set -euo pipefail
[[ $EUID -eq 0 ]] || { echo "Ejecutar como root" >&2; exit 1; }

echo "== Cuentas con UID 0 (solo debería aparecer root) =="
awk -F: '$3 == 0 {print $1}' /etc/passwd

echo "== Cuentas sin contraseña =="
awk -F: '$2 == "" {print $1}' /etc/shadow

echo "== Usuarios con shell interactiva =="
grep -Ev '(/nologin|/false)$' /etc/passwd | cut -d: -f1

echo "== Ficheros SUID/SGID =="
find / -xdev -type f \( -perm -4000 -o -perm -2000 \) -printf '%m %u %p\n' 2>/dev/null

echo "== Ficheros escribibles por todos (sin sticky) =="
find / -xdev -type f -perm -0002 2>/dev/null

echo "== Ficheros sin propietario =="
find / -xdev \( -nouser -o -nogroup \) 2>/dev/null

echo "== Permisos de ficheros críticos =="
stat -c '%a %U:%G %n' /etc/passwd /etc/shadow /etc/group /etc/gshadow /etc/sudoers
```

Relación: [[Bastionado de redes y sistemas/Unidad 2 Control de acceso y autenticación/Prácticas/P1 - UD2 Gestión segura de usuarios, grupos y contraseñas\|P1 - UD2 Gestión segura de usuarios, grupos y contraseñas]], [[Bastionado de redes y sistemas/Unidad 2 Control de acceso y autenticación/Prácticas/P2 - UD2 Permisos, ACL y atributos\|P2 - UD2 Permisos, ACL y atributos]].

### 8.2 Comprobación de la configuración de SSH

```bash
#!/usr/bin/env bash
set -uo pipefail
declare -A esperado=(
    [permitrootlogin]="no"
    [passwordauthentication]="no"
    [pubkeyauthentication]="yes"
    [x11forwarding]="no"
    [maxauthtries]="3"
)
# sshd -T muestra la configuración efectiva (incluye ficheros de sshd_config.d)
config=$(sshd -T 2>/dev/null) || { echo "No se pudo leer sshd" >&2; exit 2; }
fallos=0
for clave in "${!esperado[@]}"; do
    valor=$(awk -v k="$clave" '$1 == k {print $2}' <<< "$config")
    if [[ $valor == "${esperado[$clave]}" ]]; then
        printf '[OK]    %-24s %s\n' "$clave" "$valor"
    else
        printf '[FALLO] %-24s %s (esperado: %s)\n' "$clave" "$valor" "${esperado[$clave]}"
        (( fallos++ ))
    fi
done
exit $(( fallos > 0 ))
```

Relación: [[Bastionado de redes y sistemas/Unidad 7 Configuración de sistemas informáticos/Prácticas/P2 - UD7 Securización del servidor SSH\|P2 - UD7 Securización del servidor SSH]].

### 8.3 Detección de intentos de acceso fallidos

```bash
#!/usr/bin/env bash
# Muestra las IP con más de N intentos fallidos de SSH
set -euo pipefail
umbral="${1:-5}"
[[ $umbral =~ ^[0-9]+$ ]] || { echo "Uso: $0 [umbral]" >&2; exit 1; }

journalctl -u ssh -u sshd --since "24 hours ago" --no-pager 2>/dev/null \
  | grep -E 'Failed password|Invalid user' \
  | grep -oE '([0-9]{1,3}\.){3}[0-9]{1,3}' \
  | sort | uniq -c | sort -rn \
  | awk -v u="$umbral" '$1 > u {printf "%s intentos desde %s\n", $1, $2}'
```

En sistemas sin `journald` se puede leer `/var/log/auth.log` (Debian/Ubuntu) o `/var/log/secure` (RHEL). Para bloquear automáticamente, mejor usar **Fail2ban** que reinventarlo.

### 8.4 Cortafuegos básico con nftables

```bash
#!/usr/bin/env bash
set -euo pipefail
readonly ADMIN_NET="192.168.10.0/24"

nft -f - <<EOF
flush ruleset
table inet filtro {
  chain input {
    type filter hook input priority 0; policy drop;
    ct state established,related accept
    ct state invalid drop
    iif lo accept
    ip saddr $ADMIN_NET tcp dport 22 ct state new limit rate 5/minute accept
    icmp type echo-request limit rate 5/second accept
    log prefix "nft-drop: " counter drop
  }
  chain forward { type filter hook forward priority 0; policy drop; }
  chain output  { type filter hook output  priority 0; policy accept; }
}
EOF
nft list ruleset > /etc/nftables.conf
echo "Reglas aplicadas y guardadas"
```

> [!caution] Al aplicar reglas en remoto, prepara una vuelta atrás (p. ej. `sleep 120 && nft flush ruleset &` antes de cargarlas) para no quedarte sin acceso.

Relación: [[Bastionado de redes y sistemas/Unidad 5 Configuración de dispositivos y sistemas/Prácticas/P2 - UD5 Cortafuegos con nftables\|P2 - UD5 Cortafuegos con nftables]].

### 8.5 Copia de seguridad con rotación y verificación

```bash
#!/usr/bin/env bash
set -euo pipefail
umask 077
readonly ORIGEN="/etc"
readonly DESTINO="/srv/backup"
readonly RETENCION=7
fichero="$DESTINO/etc_$(hostname)_$(date +%F_%H%M).tar.gz"

mkdir -p -- "$DESTINO"
tar -czpf "$fichero" -C / "${ORIGEN#/}"
sha256sum "$fichero" > "$fichero.sha256"
sha256sum -c --quiet "$fichero.sha256"
find "$DESTINO" -name 'etc_*.tar.gz*' -mtime +"$RETENCION" -delete
logger -t backup "Copia correcta: $fichero"
```

Relación: [[Bastionado de redes y sistemas/Unidad 7 Configuración de sistemas informáticos/Prácticas/P6 - UD7 Copias de seguridad\|P6 - UD7 Copias de seguridad]], [[Bastionado de redes y sistemas/Unidad 7 Configuración de sistemas informáticos/Apuntes/8. Sistemas de copias de seguridad\|8. Sistemas de copias de seguridad]].

### 8.6 Control de integridad de ficheros (mini-FIM)

```bash
#!/usr/bin/env bash
set -euo pipefail
readonly BASE=/var/lib/fim/base.sha256
readonly RUTAS=(/etc/passwd /etc/shadow /etc/sudoers /etc/ssh/sshd_config)
mkdir -p "$(dirname "$BASE")"
if [ "${1:-}" = "--init" ] || [ ! -f "$BASE" ]; then
    sha256sum "${RUTAS[@]}" > "$BASE"; chmod 600 "$BASE"
    echo "Línea base creada"; exit 0
fi
if ! sha256sum -c --quiet "$BASE"; then
    logger -p auth.warning -t fim "Cambios detectados en ficheros críticos"
    exit 1
fi
```

Es una versión didáctica de lo que hacen **AIDE**, **OSSEC** o **Wazuh** ([[Bastionado de redes y sistemas/Unidad 7 Configuración de sistemas informáticos/Prácticas/P3 - UD7 Instalación y configuración de OSSEC\|P3 - UD7 Instalación y configuración de OSSEC]]).

## 9. Actividades propuestas

1. Corrige estas líneas y explica el fallo: `nombre = "Ana"`, `if [$a -eq 3]`, `for f in $(ls *.txt)`, `chmod 777 backup.sh`.
2. Escribe `crear_usuario.sh` que reciba un nombre, lo valide con una expresión regular, compruebe que no existe (`id`), lo cree con `useradd -m -s /bin/bash`, fuerce el cambio de contraseña en el primer inicio (`chage -d 0`) y registre la acción con `logger`.
3. Amplía el *script* 8.1 para que genere un informe en Markdown con fecha en el nombre y lo guarde con permisos `600`.
4. Programa el *script* 8.3 con un temporizador de `systemd` que lo ejecute cada hora.
5. Pasa **ShellCheck** a todos tus *scripts* y corrige todas las advertencias.
6. Escribe un *script* que liste los puertos en escucha (`ss -tulpn`) y avise de los que no estén en una lista blanca definida en un *array*.

## 10. Referencias

- Manual de Bash: `man bash` y [GNU Bash Reference Manual](https://www.gnu.org/software/bash/manual/).
- [ShellCheck](https://www.shellcheck.net/) — análisis estático de *scripts*.
- [Google Shell Style Guide](https://google.github.io/styleguide/shellguide.html).
- CIS Benchmarks y guías CCN-STIC de bastionado de Linux (muchos controles se comprueban con *scripts* similares a los de §8).
- Lynis ([[Bastionado de redes y sistemas/Unidad 7 Configuración de sistemas informáticos/Prácticas/P5 - UD7 Análisis de rootkits y auditoría con Lynis\|P5 - UD7 Análisis de rootkits y auditoría con Lynis]]): auditor de bastionado escrito íntegramente en *shell script*; leer su código es un buen ejercicio.
