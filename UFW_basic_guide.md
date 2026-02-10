# Guía Rápida de UFW (Uncomplicated Firewall) en Ubuntu

UFW es la herramienta por defecto para la configuración de firewalls en Ubuntu. Está diseñada para ser simple y fácil de usar mediante la línea de comandos.

## Comandos Básicos

### 1. Estado y Activación
Antes de activar, asegúrate de permitir SSH si estás conectado de forma remota.

*   **Ver estado:** `sudo ufw status verbose`
*   **Activar:** `sudo ufw enable`
*   **Desactivar:** `sudo ufw disable`
*   **Reiniciar reglas:** `sudo ufw reset`

### 2. Configuración por Defecto
Lo ideal es denegar todo lo entrante y permitir todo lo saliente.
*   `sudo ufw default deny incoming`
*   `sudo ufw default allow outgoing`

### 3. Gestión de Reglas (Permitir y Denegar)

#### Por Puerto y Protocolo
*   **Permitir puerto 80 (HTTP):** `sudo ufw allow 80/tcp`
*   **Permitir puerto 443 (HTTPS):** `sudo ufw allow 443/tcp`
*   **Denegar un puerto:** `sudo ufw deny 23/tcp`

#### Por Servicio (Nombres predefinidos)
*   **Permitir SSH:** `sudo ufw allow ssh`
*   **Permitir Nginx Full:** `sudo ufw allow 'Nginx Full'`

#### Por Dirección IP
*   **Permitir IP específica:** `sudo ufw allow from 192.168.1.50`
*   **Permitir IP a un puerto específico:** `sudo ufw allow from 192.168.1.50 to any port 22`

### 4. Eliminar Reglas
La forma más fácil es mediante números de índice:
1.  Listar reglas con números: `sudo ufw status numbered`
2.  Eliminar la regla (ejemplo regla 3): `sudo ufw delete 3`

---

## Implementación Paso a Paso (Best Practices)

Si acabas de instalar tu servidor, sigue este orden para no quedar fuera:

1. **Permitir SSH:** `sudo ufw allow ssh`
2. **Activar Firewall:** `sudo ufw enable`
3. **Verificar:** `sudo ufw status`

---
> [!TIP]
> Siempre verifica el estado después de aplicar cambios importantes para evitar bloqueos accidentales.
