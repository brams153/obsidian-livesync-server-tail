
# Obsidian LiveSync Server (Docker + Tailscale)

Este repositorio contiene una configuración optimizada para desplegar tu propio servidor **CouchDB** para **Obsidian LiveSync**, utilizando **Docker** y **Tailscale**.

El objetivo de esta configuración es proporcionar una solución de sincronización totalmente privada, autoalojada y segura, **sin necesidad de abrir puertos en tu router** ni configurar complicados certificados SSL.

## 🚀 Arquitectura
- **CouchDB:** Base de datos NoSQL donde residen tus notas de forma cifrada (E2EE).
- **Docker Compose:** Despliegue estandarizado y fácil de gestionar.
- **Tailscale:** Crea un túnel cifrado (Tailnet) entre tus dispositivos y el servidor, eliminando la exposición pública de tu base de datos.

## 📋 Requisitos Previos
1. **Docker y Docker Compose** instalados en tu servidor.
2. **Tailscale** instalado en tu servidor y en todos tus dispositivos personales (móvil, tablet, PC).
3. (Opcional) Un usuario de Linux con permisos de Docker.

## 🛠️ Instalación

1. **Clona este repositorio:**
```bash
   git clone <URL_DE_TU_REPOSITORIO>
   cd obsidian-livesync-server

```

2. **Configuración de permisos:**
CouchDB necesita permisos específicos para manejar la base de datos.

```bash
   mkdir -p data
   sudo chown -R 5984:5984 ./data

```

3. **Inicia el servidor:**

```bash
   sudo docker compose up -d

```

4. **Exposición segura mediante Tailscale:**
Una vez que el contenedor esté corriendo, redirige el tráfico de tu Tailnet al servicio local:

```bash
# Expone el puerto 5984 local a través de HTTPS en el puerto 443
sudo tailscale serve --https 443 [http://127.0.0.1:5984](http://127.0.0.1:5984)
```
## ⚙️ Configuración en Obsidian

Existen dos formas de conectar Obsidian con tu servidor. Se recomienda la configuración manual para tener mayor control sobre la base de datos:

### Configuración Manual
1. Abre la configuración del plugin **Self-hosted LiveSync** en Obsidian.
2. Desactiva el asistente automático y elige la configuración manual.
3. En la sección **Remote Server**, introduce los siguientes datos:
    * **Remote Server Type:** `CouchDB`
    * **URI:** `https://<TU-DOMINIO-TAILSCALE>` (Si usaste el puerto 443, no es necesario especificar puerto).
    * **User:** `<TU_USUARIO_DEFINIDO>`
    * **Password:** `<TU_CONTRASEÑA_SEGURA>`
4. En **Database Name**, ingresa el nombre para tu base de datos (ej: `obsidian_vault`).
5. Haz clic en **"Check and Fix database configuration"**. El plugin validará la conexión y creará los índices necesarios.
6. Haz clic en **"Apply"** para guardar los cambios.

### Configuración mediante Asistente
Si prefieres el proceso automatizado:
1. En el plugin, selecciona **"Setup Wizard"**.
2. Sigue los pasos e introduce la **URI**, **Usuario** y **Contraseña** cuando se te soliciten.
3. El asistente detectará automáticamente la base de datos y configurará los parámetros de sincronización por ti.


## 🔒 Seguridad (¡Importante!)

* **End-to-End Encryption (E2EE):** Activa siempre el cifrado E2EE en los ajustes del plugin de Obsidian. Esto garantiza que nadie (ni siquiera el administrador del servidor) pueda leer tus notas.
* **Path Obfuscation:** Actívalo para ocultar los nombres de tus archivos en la base de datos.
* **Backups:** Aunque LiveSync es muy estable, recuerda realizar copias de seguridad de tu carpeta `data` periódicamente.

## 📝 Licencia

Configuración base inspirada en el proyecto oficial de [Obsidian LiveSync](https://github.com/vrtmrz/obsidian-livesync).

## 🔗 Referencias y Créditos
Este repositorio es una configuración complementaria para [Obsidian LiveSync](https://github.com/vrtmrz/obsidian-livesync). 
- El plugin original fue desarrollado por **vrtmrz**. 
- Esta configuración dockerizada está basada en la documentación oficial de [Own Server Setup](https://github.com/vrtmrz/obsidian-livesync/blob/main/docs/setup_own_server.md).
