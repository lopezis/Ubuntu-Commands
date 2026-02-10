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
`sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak`


## 4. Parámetros Críticos a Modificar
Usa sudo nano `/etc/ssh/sshd_config` y ajusta lo siguiente:

### 4.1. Cambiar el Puerto (Seguridad por Oscuridad):
Cambia el puerto 22 por uno alto (ej. 52222) para evitar escaneos masivos.
`Port 52222`
### 4.2. Deshabilitar el Login de Root:
Nunca permitas entrar directamente como superusuario.
`PermitRootLogin no`
### 4.3. Deshabilitar Contraseñas (Solo Llaves SSH):
Es la mejora de seguridad más importante.
`PasswordAuthentication no`
### 4.4. Limitar Intentos de Autenticación:
`MaxAuthTries 3`
### 4.5. Deshabilitar Contraseñas Vacías:
`PermitEmptyPasswords no`

--
>[!IMPORTANT]
>Después de cualquier cambio, reinicia el servicio para aplicar los efectos:
>`sudo systemctl restart ssh`
