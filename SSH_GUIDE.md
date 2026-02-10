# Implementación de SSH en Ubuntu Server

Esta guía cubre desde la instalación básica hasta las configuraciones de seguridad avanzadas para proteger el acceso remoto a tu servidor Linux.

## 1. Configuración Inicial

### Instalación del Servidor
Si tu Ubuntu no tiene el servidor SSH activo, instálalo con:

#### bash
<code>sudo apt update
sudo apt install openssh-server -y</code>

## 2. Verificación del Servicio
Asegúrate de que esté corriendo correctamente:

* **Estado:** `sudo systemctl status ssh`
* **Iniciar:** `sudo systemctl start ssh`
* **Habilitar al arranque:** `sudo systemctl enable ssh`

## 3. Buenas Prácticas de Seguridad (Hardening)
La mayoría de los ataques son automatizados. Sigue estos pasos para mitigar el 90% de los riesgos.

### Editar el Archivo de Configuración
Todo se gestiona en `/etc/ssh/sshd_config.` Haz una copia de seguridad antes de editar:

#### bash
<br>`sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak`


## 4. Parámetros Críticos a Modificar
Usa sudo nano `/etc/ssh/sshd_config` y ajusta lo siguiente:

### 4.1. Cambiar el Puerto (Seguridad por Oscuridad):
Cambia el puerto 22 por uno alto (ej. 52222) para evitar escaneos masivos.
<br>`Port 52222`
### 4.2. Deshabilitar el Login de Root:
Nunca permitas entrar directamente como superusuario.
<br>`PermitRootLogin no`
### 4.3. Deshabilitar Contraseñas (Solo Llaves SSH):
Es la mejora de seguridad más importante.
<br>`PasswordAuthentication no`
### 4.4. Limitar Intentos de Autenticación:
`MaxAuthTries 3`
### 4.5. Deshabilitar Contraseñas Vacías:
`PermitEmptyPasswords no`
## 5. Implementación de Llaves SSH (Key-Based Auth)
Es mucho más seguro que cualquier contraseña tradicional.
### 5.1 En tu PC local: Genera el par de llaves (se recomienda el algoritmo Ed25519).
#### bash
<code>ssh-keygen -t ed25519 -C "tu_correo@ejemplo.com" </code>

### 5.2 Enviar la llave al servidor:
#### bash
<code>ssh-copy-id -p [PUERTO] usuario@ip-del-servidor</code>

### 5.3 Probar conexión:
#### bash
<code>ssh -p [PUERTO] usuario@ip-del-servidor</code>

## 6. Hacks y Tips Pro
### 6.1 Banner de Advertencia
Crea un archivo en /etc/issue.net con un mensaje de advertencia legal. Luego, en sshd_config, activa la línea:
<code>Banner /etc/issue.net</code>
### 6.2 Fail2Ban
Instala Fail2Ban para banear automáticamente IPs que fallen múltiples intentos de conexión.
#### bash
<code>sudo apt install fail2ban -y</code>

Configura una "jail" para SSH en /etc/fail2ban/jail.local.
### 6.3 SSH Alias (Config File Local)
Para no escribir toda la IP y el puerto cada vez, edita en tu PC local el archivo `~/.ssh/config`:
#### text
<code>Host mi-servidor
    HostName 1.2.3.4
    User miusuario
    Port 52222
    IdentityFile ~/.ssh/id_ed25519</code>


Ahora solo entra escribiendo: ssh mi-servidor
### 6.4 Auditoría de Accesos
Revisa quién ha intentado entrar a tu servidor con el comando lastlog o consulta los logs en tiempo real:
#### bash
<code>sudo tail -f /var/log/auth.log</code>



--
>[!IMPORTANT]
>Después de cualquier cambio, reinicia el servicio para aplicar los efectos:
>`sudo systemctl restart ssh`
