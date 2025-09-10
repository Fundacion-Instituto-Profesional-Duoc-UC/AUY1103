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

# Variables

Las variables de entrada y salida en Terraform son esenciales para parametrizar y compartir valores dentro de tus configuraciones y módulos. Permiten que tus configuraciones sean más dinámicas, reutilizables y flexibles.

---

## Variables de Entrada

Las variables de entrada se usan para parametrizar tus configuraciones de Terraform. Permiten pasar valores a tus módulos o configuraciones desde el exterior. Las variables de entrada pueden definirse dentro de un módulo o en el nivel raíz de tu configuración. Así es como se define una variable de entrada:

```
variable "example_var" {
  description = "An example input variable"
  type        = string
  default     = "default_value"
}
```

En este ejemplo:

- `variable` se usa para declarar una variable de entrada llamada `example_var`.
- `description` proporciona una descripción legible de la variable.
- `type` especifica el tipo de dato de la variable (e.j., `string`, `number`, `list`, `map`, etc.).
- `default` proporciona un valor por defecto para la variable, el cual es opcional.
---

Luego puedes usar la variable de entrada dentro de tu módulo o configuración de esta manera:

```
resource "example_resource" "example" {
  name = var.example_var
  # other resource configurations
}
```

Haces referencia a la variable de entrada usando `var.example_var`.

---

## Variables de Salida

Las variables de salida te permiten exponer valores desde tu módulo o configuración, dejándolos disponibles para ser usados en otras partes de tu entorno de Terraform. Así es como se define una variable de salida:

```
output "example_output" {
  description = "An example output variable"
  value       = resource.example_resource.example.id
}
```

En este ejemplo:

- `output` se usa para declarar una variable de salida llamada `example_output`.
- `description` proporciona una descripción de la variable de salida.
- `value` especifica el valor que quieres exponer como una variable de salida. Este valor puede ser el atributo de un recurso, un valor computado o cualquier otra expresión.

---

Puedes hacer referencia a las variables de salida en el módulo raíz o en otros módulos usando la sintaxis `module.module_name.output_name`, donde `module_name` es el nombre del módulo que contiene la variable de salida.

Por ejemplo, si tienes una variable de salida llamada `example_output` en un módulo llamado `example_module`, puedes acceder a ella en el módulo raíz de esta manera:

```
output "root_output" {
  value = module.example_module.example_output
}
```

Esto te permite compartir datos y valores entre diferentes partes de tu configuración de Terraform y crear entornos de infraestructura como código más modulares y fáciles de mantener.