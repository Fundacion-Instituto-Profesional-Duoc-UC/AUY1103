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

# Built-in Functions (Funciones Incorporadas)

---

Terraform es una herramienta de infraestructura como código (IaC) que te permite definir y aprovisionar recursos de infraestructura de manera declarativa. Terraform proporciona una amplia gama de funciones incorporadas que puedes usar dentro de tus archivos de configuración (generalmente escritos en el Lenguaje de Configuración de HashiCorp, o HCL) para manipular y transformar datos. Estas funciones te ayudan a realizar diversas tareas al definir tu infraestructura.

---

Aquí tienes algunas de las funciones incorporadas más utilizadas en Terraform:

-`concat(list1, list2, ...)`: Combina múltiples listas en una sola lista.

```
variable "list1" {
  type    = list
  default = ["a", "b"]
}

variable "list2" {
  type    = list
  default = ["c", "d"]
}

output "combined_list" {
  value = concat(var.list1, var.list2)
}
```
---

-`element(list, index)`: Devuelve el elemento en el índice especificado de una lista.
```
variable "my_list" {
  type    = list
  default = ["apple", "banana", "cherry"]
}

output "selected_element" {
  value = element(var.my_list, 1) # Devuelve "banana"
}
```
---
- `length(list)`: Devuelve el número de elementos en una lista.
```
variable "my_list" {
  type    = list
  default = ["apple", "banana", "cherry"]
}

output "list_length" {
  value = length(var.my_list) # Devuelve 3
}
```
-`map(key, value)`: Crea un mapa a partir de una lista de claves y una lista de valores.
```
variable "keys" {
  type    = list
  default = ["name", "age"]
}

variable "values" {
  type    = list
  default = ["Alice", 30]
}

output "my_map" {
  value = map(var.keys, var.values) # Devuelve {"name" = "Alice", "age" = 30}
}
```
---
-`lookup(map, key)`: Recupera el valor asociado con una clave específica en un mapa.
```
variable "my_map" {
  type    = map(string)
  default = {"name" = "Alice", "age" = "30"}
}

output "value" {
  value = lookup(var.my_map, "name") # Devuelve "Alice"
}
```
-`join(separator, list)`: Une los elementos de una lista en una única cadena de texto, usando el separador especificado.
```
variable "my_list" {
  type    = list
  default = ["apple", "banana", "cherry"]
}

output "joined_string" {
  value = join(", ", var.my_list) # Devuelve "apple, banana, cherry"
}
```
---
## Estos son solo algunos ejemplos de las funciones incorporadas disponibles en Terraform. Puedes encontrar más funciones y documentación detallada en la documentación oficial de Terraform, la cual se actualiza regularmente para incluir nuevas características y mejoras.