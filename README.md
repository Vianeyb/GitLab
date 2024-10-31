# Instalación de GitLab en Ubuntu


## Prerrequisitos

- **Servidor Ubuntu**: Asegúrate de tener un servidor Ubuntu (18.04 o posterior).
- **Acceso Root**: Necesitas acceso como usuario root o un usuario con privilegios sudo.
- **Requisitos del sistema**: Verifica que tu servidor cumpla con los requisitos de GitLab. Para más detalles, consulta [la documentación oficial](https://docs.gitlab.com/ee/install/requirements.html).

## Pasos de Instalación

### 1. Actualizar el Sistema

Primero, asegúrate de que tu sistema esté actualizado:

```bash
sudo apt update
sudo apt upgrade -y
```
### 1.1. Instalación de Postfix

Para la instalación de Postfix, se recomienda configurarlo como "Sitio de Internet" para facilitar la entrega de correos electrónicos. Durante la instalación, sigue estos pasos:

1. Ejecuta el siguiente comando para instalar Postfix y otras dependencias necesarias:

    ```bash
    sudo apt install ca-certificates curl openssh-server postfix
    ```

2. Cuando aparezca la pantalla de configuración de Postfix, selecciona la opción **Sitio de Internet**.

3. En la siguiente pantalla, introduce el nombre de dominio de tu servidor (por ejemplo, `serverubuntu.com` o tu dominio configurado).

> **Nota**: Postfix se utiliza para enviar notificaciones de correo electrónico desde GitLab, como restablecimientos de contraseña o notificaciones de actividad. Asegúrate de que el servidor de correo esté configurado correctamente para enviar correos.

### 2. Instalación de GitLab

Una vez que las dependencias están instaladas, procedemos a la instalación de GitLab.

1. Ve a la carpeta `/tmp` y descarga el script de instalación de GitLab ejecutando el siguiente comando:

    ```bash
    cd /tmp
    curl -LO https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh
    ```

2. Ejecuta el script para configurar el repositorio de GitLab:

    ```bash
    sudo bash script.deb.sh
    ```

3. Una vez agregado el repositorio, instala GitLab usando el siguiente comando, reemplazando `http://gitlab.example.com` con el nombre de dominio o la dirección IP de tu servidor:

    ```bash
    sudo EXTERNAL_URL="http://gitlab.example.com" apt install gitlab-ee
    ```
    si no funciona con gitlab-ee puedes ejecutar -ce

4. Cuando finalice la instalación, configura GitLab ejecutando:

    ```bash
    sudo gitlab-ctl reconfigure
    ```

    Esto inicializará GitLab y lo preparará para su uso.

5. Edicion del archivo de configuracion
   Antes de poder usar la aplicación, deberá actualizar el archivo de configuración
   ```bash
   sudo nano /etc/gitlab/gitlab.rb
   ```
   Cerca de la parte superior, se encuentra la línea de configuración 
   external_url. Actualícela para que coincida con su dominio.
   ```
   ##! For more details on configuring external_url see:
   ##! https://docs.gitlab.com/omnibus/settings/configuration.html#configuring-  the-  external-url-for-gitlab
    external_url 'https://example.com'
   ```
   Guarde y cierre el archivo. Ejecute el siguiente comando para reconfigurar 
   GitLab:

   ```bash
   sudo gitlab-ctl reconfigure
   ```
