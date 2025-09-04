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

## Implementación de múltiples
## regiones en Terraform

Puede utilizar la palabra clave "alias" para implementar la configuración de infraestructura multirregional en Terraform.

Este código usa una característica de Terraform llamada alias de proveedor para crear recursos de AWS en dos regiones diferentes (us-east-1 y us-west-2) dentro del mismo proyecto.

Normalmente, solo puedes definir un bloque provider "aws". Al añadir un alias, puedes tener múltiples configuraciones para el mismo proveedor.

---
## La Configuración de Proveedores
## (Múltiples Regiones) 🌍
Aquí se definen dos configuraciones para el proveedor de AWS, cada una con un nombre único gracias al alias
```
provider "aws" {
  alias = "us-east-1"
  region = "us-east-1"
}

provider "aws" {
  alias = "us-west-2"
  region = "us-west-2"
}

resource "aws_instance" "example" {
  ami = "ami-0123456789abcdef0"
  instance_type = "t2.micro"
  provider = "aws.us-east-1"
}

resource "aws_instance" "example2" {
  ami = "ami-0123456789abcdef0"
  instance_type = "t2.micro"
  provider = "aws.us-west-2"
}
```
---
alias = "us-east-1": Le da el nombre "us-east-1" a esta configuración del proveedor de AWS, que apunta a la región us-east-1.

alias = "us-west-2": Le da el nombre "us-west-2" a esta otra configuración, que apunta a la región us-west-2

---

## La Creación de Recursos (Asignando una Región) 📍
Al crear un recurso, usas el meta-argumento provider para decirle a Terraform cuál de las configuraciones con alias debe usar.

```
# Esta instancia se creará en us-east-1
resource "aws_instance" "example" {
  ami           = "ami-0123456789abcdef0"
  instance_type = "t2.micro"
  provider      = aws.us-east-1 # Usa el proveedor con el alias "us-east-1"
}

# Esta instancia se creará en us-west-2
resource "aws_instance" "example2" {
  ami           = "ami-0123456789abcdef0"
  instance_type = "t2.micro"
  provider      = aws.us-west-2 # Usa el proveedor con el alias "us-west-2"
}
```
