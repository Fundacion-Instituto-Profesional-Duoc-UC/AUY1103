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


# **Empezando con Terraform**
### Conceptos Fundamentales y Términos Clave

---

## **Conceptos Clave (1/4): Bloques Fundamentales**

* **Proveedor**: Un complemento que interactúa con la API de una plataforma específica (ej. AWS, Azure, Google Cloud). Define y administra los recursos.

* **Recurso**: Un componente de infraestructura que gestionas con Terraform (ej. una máquina virtual, una base de datos, una red).

* **Módulo**: Una unidad de código reutilizable y encapsulada. Permite empaquetar y compartir configuraciones de infraestructura.

---

## **Conceptos Clave (2/4): Estructura del Código**

* **Archivo de Configuración**: Archivos con extensión `.tf` (como `main.tf`) donde defines el estado deseado de tu infraestructura.

* **Variable**: Marcadores de posición para valores, lo que hace el código más flexible y reutilizable al pasar datos durante la ejecución.

* **Salida (Output)**: Valores generados por Terraform después de crear la infraestructura. Son útiles para mostrar información clave (ej. una dirección IP).

---

## **Conceptos Clave (3/4): El Flujo de Trabajo**

* **Archivo de Estado**: El archivo `terraform.tfstate` que registra el estado actual de tu infraestructura. Es crucial para que Terraform sepa qué gestionar.

* **Plan**: Una vista previa de los cambios. El comando `terraform plan` muestra qué acciones (crear, modificar, destruir) se realizarán.

* **Aplicar (Apply)**: El comando `terraform apply` ejecuta los cambios detallados en el plan para llevar la infraestructura al estado deseado.

---

## **Conceptos Clave (4/4): Entornos y Colaboración**

* **Espacio de Trabajo (Workspace)**: Permite gestionar múltiples entornos (ej. desarrollo, producción) con estados y configuraciones independientes.

* **Backend Remoto**: Una ubicación de almacenamiento compartida (ej. Amazon S3) para el archivo de estado, esencial para el trabajo en equipo y la seguridad.

---

## **Resumen**

Estos son los términos esenciales que encontrarás al trabajar con Terraform.

A medida que los uses, te familiarizarás con cómo se integran en tus flujos de trabajo de Infraestructura como Código (IaC).