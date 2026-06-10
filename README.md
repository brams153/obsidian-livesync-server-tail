
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

En el plugin **Self-hosted LiveSync** dentro de Obsidian, configura:

* **Remote Server:** `CouchDB`
* **URI:** `https://<TU-DOMINIO-TAILSCALE>:5984` (O la IP de Tailscale de tu servidor).
* **Username:** `brams` (o el que definas)
* **Password:** `Ajmheito1` (¡Cámbiala en producción!)
* **Database Name:** `obsidian_vault`

Haz clic en **"Check and Fix database configuration"** y el plugin se encargará de crear los índices necesarios.

## 🔒 Seguridad (¡Importante!)

* **End-to-End Encryption (E2EE):** Activa siempre el cifrado E2EE en los ajustes del plugin de Obsidian. Esto garantiza que nadie (ni siquiera el administrador del servidor) pueda leer tus notas.
* **Path Obfuscation:** Actívalo para ocultar los nombres de tus archivos en la base de datos.
* **Backups:** Aunque LiveSync es muy estable, recuerda realizar copias de seguridad de tu carpeta `data` periódicamente.

## 📝 Licencia

Configuración base inspirada en el proyecto oficial de [Obsidian LiveSync](https://github.com/vrtmrz/obsidian-livesync).

