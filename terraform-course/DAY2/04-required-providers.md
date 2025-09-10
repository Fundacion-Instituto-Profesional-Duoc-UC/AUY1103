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

## **Configuración del Proveedor**

---

## **Bloque `required_providers`**

Se usa para declarar y especificar los proveedores requeridos, su origen (`source`) y sus restricciones de versión.

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 3.0"
    }
    azurerm = {
      source  = "hashicorp/azurerm"
      version = ">= 2.0, < 3.0"
    }
  }
}
```
---

## Desglose del Código
El bloque terraform { ... } se usa para configurar el comportamiento de Terraform. Dentro de él, required_providers actúa como una lista de dependencias.

```
aws = {
  source  = "hashicorp/aws"
  version = "~> 3.0"
}
```
---

```
aws = {
  source  = "hashicorp/aws"
  version = "~> 3.0"
}
```

<p style="text-align: justify;">aws: Es un nombre local que le das al proveedor.</p>

source = "hashicorp/aws": Esta es la parte más importante. Le dice a Terraform dónde encontrar y descargar el proveedor. En este caso, es el proveedor oficial de AWS mantenido por HashiCorp en el Registro de Terraform.

version = "~> 3.0": ⛓️ Esta es una restricción de versión. El símbolo ~> (operador pesimista) significa "permite cualquier versión menor o de parche dentro de la versión 3". Es decir, aceptará las versiones 3.1, 3.5, etc., pero nunca instalará la versión 4.0, ya que podría incluir cambios que rompan tu código.