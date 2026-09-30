---
{"dg-publish":true,"permalink":"/bastionado-de-redes-y-sistemas/unidad-3-administracion-de-credenciales-de-acceso/practicas/p1-ud-3-acceso-a-un-servidor-remoto-mediante-claves-ssh/","title":"Acceso a un servidor remoto mediante claves SSH","tags":["bastionado-de-redes-y-sistemas","practica","ssh"],"noteIcon":"","dg-note-properties":{"title":"Acceso a un servidor remoto mediante claves SSH","materia":"Bastionado de redes y sistemas","ciclo":"Formación Profesional","tags":["bastionado-de-redes-y-sistemas","practica","ssh"],"status":"En progreso"}}
---


# 📝 Práctica: Acceso a un servidor remoto mediante claves SSH

> [!note] Objetivos de la Práctica
> - Generar pares de claves asimétricas de distintos tipos.
> - Usar la clave pública como credencial de acceso a un servidor remoto.
> - Deshabilitar la autenticación por contraseña.
> - Gestionar claves con `ssh-agent`.

## 📐 Recordatorio Teórico

- [[Bastionado de redes y sistemas/Unidad 3 Administración de credenciales de acceso/Apuntes/3. Infraestructuras de clave pública (PKI)#3.9 Certificados y claves para el acceso a un servidor remoto (SSH)\|Claves para el acceso remoto con SSH]]
- [[Bastionado de redes y sistemas/Unidad 3 Administración de credenciales de acceso/Apuntes/3. Infraestructuras de clave pública (PKI)#3.1.1 Recordatorio: criptografía asimétrica\|Criptografía asimétrica]]
- [[Bastionado de redes y sistemas/Unidad 3 Administración de credenciales de acceso/Apuntes/2. Gestión de credenciales#2.1 Tipos de credenciales más utilizados\|Tipos de credenciales]]

**Escenario:** dos máquinas virtuales Linux en la misma red: un **cliente** y un **servidor** con OpenSSH Server instalado.

## 🛠️ Enunciado

### Paso 1. Comprobar el servidor

Revisa en el servidor el fichero `/etc/ssh/sshd_config` y localiza las directivas `Port`, `PermitRootLogin`, `PasswordAuthentication` y `AuthorizedKeysFile`. Identifica las claves de *host* del servidor:

```bash
ls -l /etc/ssh/ssh_host_*
```

### Paso 2. Generar el par de claves en el cliente

```bash
# RSA de 4096 bits (pon una passphrase)
ssh-keygen -t rsa -b 4096
# Otra clave de tipo ed25519
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519
ls -l ~/.ssh/
```

Explica qué fichero es la clave privada y cuál la pública.

### Paso 3. Copiar la clave pública al servidor

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub usuario@IP_SERVIDOR
```

Comprueba en el servidor que la clave se ha añadido a `~/.ssh/authorized_keys`.

### Paso 4. Acceder con la clave

```bash
ssh -i ~/.ssh/id_rsa usuario@IP_SERVIDOR
```

Observa que se pide la *passphrase* de la clave y no la contraseña del usuario.

### Paso 5. Usar ssh-agent

```bash
eval "$(ssh-agent)"
ssh-add ~/.ssh/id_rsa ~/.ssh/id_ed25519
ssh-add -l
ssh usuario@IP_SERVIDOR
```

### Paso 6. Exigir autenticación por clave

En el servidor, edita `/etc/ssh/sshd_config`:

```text
PermitRootLogin no
PasswordAuthentication no
AuthorizedKeysFile .ssh/authorized_keys
```

Reinicia el servicio y comprueba que desde otra cuenta **sin clave** no es posible entrar:

```bash
sudo systemctl restart ssh
ssh -o PubkeyAuthentication=no usuario@IP_SERVIDOR
```

### Paso 7. Cuestiones

1. ¿Qué ocurre la primera vez que te conectas al servidor? ¿Qué riesgo implica aceptar la huella?
2. ¿Por qué es importante proteger la clave privada con *passphrase*?
3. ¿Qué diferencia hay entre este mecanismo y un certificado X.509 emitido por una CA?

> [!important] Criterio de evaluación asociado
> RA3 b) Se han generado y utilizado diferentes certificados digitales como medio de acceso a un servidor remoto.

> [!quote]- Fuentes
> - `C11+-+S5_03+-+SSH+y+GPG.pdf`; `Bloque 2- Sistemas Control de Acceso.pdf` (configurar el acceso por certificado); `UT 7- Confiiguración de SI.pdf` (clave pública y privada)
