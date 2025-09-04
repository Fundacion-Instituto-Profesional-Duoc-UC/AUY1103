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

# Expresiones Condicionales
Las expresiones condicionales en Terraform se usan para definir lógica condicional dentro de tus configuraciones. Permiten tomar decisiones o establecer valores basados en condiciones. Las expresiones condicionales se usan típicamente para controlar si los recursos se crean o configuran basándose en la evaluación de una condición.

La sintaxis para una expresión condicional en Terraform es:

```
condition ? true_val : false_val
```
---

```
condition ? true_val : false_val
```

- `condition` es una expresión que se evalúa como `true` o `false`.

- `true_val` es el valor que se devuelve si la condición es `true`.

- `false_val` es el valor que se devuelve si la condición es `false`.

---

## Ejemplo de Creación Condicional de Recursos
```
resource "aws_instance" "example" {
  count = var.create_instance ? 1 : 0

  ami           = "ami-XXXXXXXXXXXXXXXXX"
  instance_type = "t2.micro"
}
```
En este ejemplo, el atributo `count` del recurso `aws_instance` usa una expresión condicional. Si la variable `create_instance` es true, crea una instancia EC2. Si `create_instance` es false, crea cero instancias, omitiendo efectivamente la creación del recurso.

---

#### Ejemplo de Asignación Condicional de Variables

```
variable "environment" {
  description = "Tipo de entorno"
  type        = string
  default     = "development"
}

variable "production_subnet_cidr" {
  description = "Bloque CIDR para la subred de producción"
  type        = string
  default     = "10.0.1.0/24"
}

variable "development_subnet_cidr" {
  description = "Bloque CIDR para la subred de desarrollo"
  type        = string
  default     = "10.0.2.0/24"
}

resource "aws_security_group" "example" {
  name        = "example-sg"
  description = "Grupo de seguridad de ejemplo"

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = var.environment == "production" ? [var.production_subnet_cidr] : [var.development_subnet_cidr]
  }
}
```
---

En este ejemplo, el bloque `ingress` usa una expresión condicional para asignar un valor al atributo `cidr_blocks` basándose en el valor de la variable `environment`. Si `environment` se establece en `"production"`, usa la variable `production_subnet_cidr`; de lo contrario, usa la variable `development_subnet_cidr`.

---

### Configuración Condicional de Recursos
```
resource "aws_security_group" "example" {
  name = "example-sg"
  description = "Grupo de seguridad de ejemplo"

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = var.enable_ssh ? ["0.0.0.0/0"] : []
  }
}
```
En este ejemplo, el bloque `ingress` dentro del recurso `aws_security_group` usa una expresión condicional para controlar si se permite el acceso SSH. Si `enable_ssh` es true, permite el tráfico SSH desde cualquier origen (`"0.0.0.0/0"`); de lo contrario, no permite tráfico entrante.

---

### Las expresiones condicionales en Terraform proporcionan una forma poderosa de tomar decisiones y personalizar tus despliegues de infraestructura basados en diversas condiciones y variables. Mejoran la flexibilidad и la reutilización de tus configuraciones de Terraform.