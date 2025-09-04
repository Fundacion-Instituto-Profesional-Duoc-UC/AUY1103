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

# Implementación de Variables

El siguiente código de Terraform define y crea una instancia EC2 en AWS de forma flexible, usando variables para configurar sus características y exponiendo su IP pública como un valor de salida.

---

```
# Define an input variable for the EC2 instance type
variable "instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t2.micro"
}

# Define an input variable for the EC2 instance AMI ID
variable "ami_id" {
  description = "EC2 AMI ID"
  type        = string
}

# Configure the AWS provider using the input variables
provider "aws" {
  region      = "us-east-1"
}

# Create an EC2 instance using the input variables
resource "aws_instance" "example_instance" {
  ami           = var.ami_id
  instance_type = var.instance_type
}

# Define an output variable to expose the public IP address of the EC2 instance
output "public_ip" {
  description = "Public IP address of the EC2 instance"
  value       = aws_instance.example_instance.public_ip
}
```

---

## ⚙️ Variables de Entrada (Inputs)
El código comienza definiendo dos "parámetros" que permiten configurar la máquina virtual sin modificar el código principal.

variable "instance_type": Define el tipo de máquina a crear (ej. su potencia). Tiene un valor por defecto default = "t2.micro", lo que significa que si no se especifica otro, usará t2.micro automáticamente.

variable "ami_id": Define la imagen del sistema operativo (AMI) que usará la máquina. Es importante notar que esta variable no tiene un valor por defecto, lo que la convierte en un parámetro obligatorio. Terraform le pedirá al usuario que ingrese un valor para ami_id antes de ejecutar el plan.

```
# Define an input variable for the EC2 instance type
variable "instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t2.micro"
}

# Define an input variable for the EC2 instance AMI ID
variable "ami_id" {
  description = "EC2 AMI ID"
  type        = string
}
```
---
## 🖥️ Creación del Recurso (La Instancia EC2)
Esta es la sección que crea la infraestructura.
```
resource "aws_instance" "example_instance" {
  ami           = var.ami_id
  instance_type = var.instance_type
}
```

- El bloque `resource` le ordena a Terraform crear una instancia EC2 de AWS llamada `example_instance`.

- `ami = var.ami_id`: Configura la imagen de la instancia usando el valor de la variable de entrada ami_id.

- `instance_type = var.instance_type`: Configura el tipo de instancia usando el valor de la variable instance_type (en este caso, el valor por defecto t2.micro, a menos que se especifique otro).

---

## 📤 Variable de Salida (Output)
Esta parte del código expone información útil una vez que la infraestructura ha sido creada.

```
output "public_ip" {
  description = "Public IP address of the EC2 instance"
  value       = aws_instance.example_instance.public_ip
}
```

- El bloque `output` define un valor de salida llamado `public_ip`.

- `value = aws_instance.example_instance.public_ip`: Accede al atributo public_ip de la instancia EC2 que se acaba de crear y lo asigna como el valor de salida.

---

## Resumen del Flujo
Al ejecutar este código con terraform apply, ocurrirá lo siguiente:

- 1. Terraform verá que la variable ami_id es obligatoria y te pedirá que introduzcas un valor.

- 2. Utilizará el AMI que proporcionaste y el tipo de instancia por defecto (t2.micro) para crear la máquina virtual.

- 3. Una vez creada, Terraform mostrará la dirección IP pública de la nueva instancia en la sección "Outputs" de la terminal.