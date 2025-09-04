---
marp: true
theme: uncover
backgroundImage: url('../images/background.png')
backgroundSize: cover
color: #333
size: 16:9
---

<style>
section {
  font-size: 180%;
}
</style>

# **Configurar Terraform para AWS**
### Preparando las Credenciales de Acceso

---

## **Hoja de Ruta: Los 3 Pasos Clave**

Para que Terraform pueda interactuar con tu cuenta de AWS, seguiremos tres pasos principales:

1.  **Instalar la AWS CLI:** La herramienta base para la configuración.
2.  **Crear un Usuario IAM:** Generar credenciales de acceso programático.
3.  **Configurar Credenciales Locales:** Guardar las credenciales en tu equipo.

---

## **Paso 1: Instalar la AWS CLI**

La **AWS CLI (Interfaz de Línea de Comandos)** es un prerrequisito para configurar tus credenciales fácilmente.

* Si aún no la tienes, descárgala e instálala desde la página oficial de Amazon.

* **Enlace de descarga:** `https://aws.amazon.com/cli/`

---

## **Paso 2: Crear un Usuario de AWS IAM (1/3)**

Necesitamos un usuario específico para que Terraform actúe en tu nombre.

1.  Inicia sesión en la **Consola de Administración de AWS**.
2.  Navega hasta el servicio de **IAM** (Identity and Access Management).
3.  En el panel izquierdo, haz clic en **"Usuarios"** y luego en el botón **"Agregar usuario"**.

---

## **Paso 2: Crear un Usuario de AWS IAM (2/3)**

Al configurar el nuevo usuario, sigue estos pasos:

* **Nombre de usuario:** Elige un nombre descriptivo (ej. `terraform-user`).
* **Tipo de acceso:** Selecciona **"Acceso programático"**. ¡Esto es muy importante!
* **Permisos:** Asocia las políticas necesarias. Para empezar, puedes adjuntar **`AmazonEC2FullAccess`**.
* Haz clic en los siguientes pasos hasta llegar a la revisión final.

---

## **Paso 2: Crear un Usuario de AWS IAM (3/3)**

### **⚠️ ¡Atención! Guarda tus credenciales ⚠️**

Al crear el usuario, AWS te mostrará el **ID de la clave de acceso** y la **Clave de acceso secreta**.

* **Guarda estas credenciales en un lugar seguro.**
* Esta es la **única vez** que podrás ver la clave de acceso secreta. Si la pierdes, tendrás que generar una nueva.

---

## **Paso 3: Configurar las Credenciales Locales**

Ahora, usa la AWS CLI para guardar las credenciales que acabas de obtener.

1.  Abre tu terminal o línea de comandos.
2.  Ejecuta el siguiente comando:
    ```bash
    aws configure
    ```
3.  El sistema te pedirá los siguientes datos. Pega los valores que guardaste:
    - `AWS Access Key ID:`
    - `AWS Secret Access Key:`
    - `Default region name:` (ej. `us-east-1`, `eu-west-1`)
    - `Default output format:` (puedes presionar Enter o escribir `json`)

---

## **¡Listo!**

Una vez completado el `aws configure`, Terraform buscará y usará automáticamente estas credenciales para autenticarse con tu cuenta de AWS.