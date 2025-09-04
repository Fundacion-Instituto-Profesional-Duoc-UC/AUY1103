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

# Archivos .tfvars de Terraform
En Terraform, los archivos `.tfvars` (típicamente con la extensión .tfvars) se usan para asignar valores específicos a las variables de entrada definidas en tu configuración.

Permiten separar los valores de configuración de tu código de Terraform, facilitando la gestión de diferentes configuraciones para distintos entornos (ej. desarrollo, staging, producción) o para almacenar información sensible sin exponerla en tu código.

---
A continuación, se detalla el propósito de los archivos `.tfvars`:

- `Separación de la Configuración y el Código`: Las variables de entrada en Terraform están diseñadas para ser configurables, de modo que puedas usar el mismo código con diferentes conjuntos de valores. En lugar de codificar estos valores directamente en tus archivos .tf, usas archivos .tfvars para mantener la configuración separada. Esto facilita el mantenimiento y la gestión de configuraciones para diferentes entornos.

- `Información Sensible`: Los archivos .tfvars son un lugar común para almacenar información sensible como claves de API, credenciales de acceso o secretos. Estos valores sensibles pueden mantenerse fuera del sistema de control de versiones (como Git), mejorando la seguridad y previniendo la exposición accidental de secretos en tu base de código.

---

A continuación, se detalla el propósito de los archivos `.tfvars`:

- `Reutilización`: Al mantener los valores de configuración en archivos .tfvars separados, puedes reutilizar el mismo código de Terraform con diferentes conjuntos de variables. Esto es útil para crear infraestructura para diferentes proyectos o entornos usando un único conjunto de módulos de Terraform.

- `Colaboración`: Cuando se trabaja en equipo, cada miembro puede tener su propio archivo .tfvars para establecer valores específicos para su entorno o flujo de trabajo. Esto evita conflictos en la base de código cuando varias personas trabajan en el mismo proyecto de Terraform.

---

## Resumen
Así es como se utilizan típicamente los archivos .tfvars:

- Define tus variables de entrada en tu código de Terraform (ej. en un archivo variables.tf).

- Crea uno o más archivos .tfvars, cada uno con valores específicos para esas variables de entrada.

- Al ejecutar comandos de Terraform (ej. `terraform apply`, `terraform plan`), puedes especificar qué archivo .tfvars usar con la opción `-var-file`:

```
terraform apply -var-file=dev.tfvars
```
Al usar archivos `.tfvars`, puedes mantener tu código de Terraform más genérico y flexible, mientras personalizas las configuraciones para diferentes escenarios y entornos.

