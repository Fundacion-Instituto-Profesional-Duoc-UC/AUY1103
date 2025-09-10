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

# **Uso de Múltiples Proveedores**

---

## **Paso 1: Definir los Proveedores**

Define todos los proveedores que necesites en tu configuración (ej. en un archivo `providers.tf`).

1. Cree un archivo provider.tf en el directorio raíz de su proyecto Terraform.
2. En el archivo provider.tf, defina los proveedores de AWS y Azure. Por ejemplo:

```hcl
provider "aws" {
  region = "us-east-1"
}

provider "azurerm" {
  subscription_id = "your-azure-sub-id"
  client_id       = "your-azure-client-id"
  client_secret   = "your-azure-client-secret"
  tenant_id       = "your-azure-tenant-id"
}
```

---

3. En sus otros archivos de configuración de Terraform, puede usar los proveedores aws y azurerm para crear recursos en AWS y Azure, respectivamente.


```hcl
resource "aws_instance" "example" {
  ami = "ami-0123456789abcdef0"
  instance_type = "t2.micro"
}

resource "azurerm_virtual_machine" "example" {
  name = "example-vm"
  location = "eastus"
  size = "Standard_A1"
}
```

