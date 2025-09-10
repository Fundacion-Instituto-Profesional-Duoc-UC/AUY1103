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

# **Proveedores en Terraform**

---

## **¿Qué es un Proveedor?**

Es un **complemento** 🔌 que permite a Terraform interactuar con una API.

- Proveedores de Nube (AWS, Azure, GCP)
- Proveedores de SaaS
- Otras APIs

---

## **Ejemplo: Proveedor de AWS**

Para crear recursos en AWS, se usa el proveedor de AWS.

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "example" {
  ami           = "ami-0123456789abcdef0"
  instance_type = "t2.micro"
}
```
---

# **3 Formas de Configurar Proveedores**

## **Contexto del Ejemplo**

Al definir un proveedor como `aws`, Terraform lo instalará y usará para crear los recursos definidos, como una máquina virtual.

Existen muchos proveedores además de AWS:
- `azurerm` - para Azure
- `google` - para Google Cloud Platform
- `kubernetes` - para Kubernetes

---

### **En el Módulo Raíz**

Es la forma más común. El proveedor está disponible para todos los recursos de la configuración.

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "example" {
  ami           = "ami-0123456789abcdef0"
  instance_type = "t2.micro"
}

```
---

### **En un Módulo Secundario**

Útil para reutilizar la misma configuración de proveedor en múltiples recursos o módulos.

```hcl
module "aws_vpc" {
  source = "./aws_vpc"
  providers = {
    aws = aws.us-west-2
  }
}

resource "aws_instance" "example" {
  ami = "ami-0123456789abcdef0"
  instance_type = "t2.micro"
  depends_on = [module.aws_vpc]
}
```
---

## **En el bloque `required_providers`**

Asegura que se utilice una **versión específica** del proveedor para garantizar la estabilidad.

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 3.79"
    }
  }
}

resource "aws_instance" "example" {
  ami = "ami-0123456789abcdef0"
  instance_type = "t2.micro"
}
```

---

La mejor manera de configurar proveedores depende de sus necesidades específicas. 

- Si solo usa un proveedor, configurarlo en el módulo raíz es la opción más sencilla. 
- Si usa varios proveedores o desea reutilizar la misma configuración en varios recursos, configurarlo en un módulo secundario es una buena opción. 
- Si desea asegurarse de usar una versión específica del proveedor, configurarlo en el bloque required_providers es la mejor opción.

