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

# **Infraestructura como Código (IaC)**
### Un enfoque moderno para la gestión de la infraestructura

---

## **El Mundo Antes de IaC**

La gestión de la infraestructura era un proceso manual y laborioso:

1.  **Configuración manual de servidores:** Propenso a inconsistencias y errores humanos.
2.  **Falta de control de versiones:** Difícil seguimiento de cambios o capacidad de revertir a un estado anterior.
3.  **Excesiva documentación:** Se dependía de documentos que quedaban obsoletos rápidamente.
4.  **Automatización limitada:** Se limitaba a scripts básicos sin la robustez de las herramientas modernas.
5.  **Aprovisionamiento lento:** Un proceso que consumía mucho tiempo y generaba demoras en los proyectos.

---

## **La Solución: El Enfoque IaC**

IaC aborda estos desafíos proporcionando un enfoque **sistemático, automatizado y basado en código.**

* **Herramientas Populares:**
    - **Terraform**
    - AWS CloudFormation
    - Azure Resource Manager (ARM)

* **Beneficios:** Permite a las organizaciones definir, implementar y administrar su infraestructura de manera eficiente, consistente y adaptable a las necesidades modernas.

---

# **¿Por qué Terraform?**
### Razones clave para su popularidad

---

## **Razones para Elegir Terraform (1/2)**

1.  **Compatibilidad con Múltiples Nubes**: Permite usar el mismo código para aprovisionar recursos en AWS, Azure, Google Cloud e incluso on-premise.

2.  **Gran Ecosistema**: Vasto ecosistema de proveedores y módulos de la comunidad que ahorran tiempo y esfuerzo.

3.  **Sintaxis Declarativa**: Se especifica el **estado final** deseado de la infraestructura, lo que facilita la comprensión y el mantenimiento del código.

4.  **Gestión de Estado**: Mantiene un archivo de estado (`.tfstate`) que mapea el código con la infraestructura real, permitiendo la gestión de cambios.

---

## **Razones para Elegir Terraform (2/2)**

5.  **Flujo "Planificar y Aplicar"**: Permite previsualizar los cambios (`plan`) antes de aplicarlos (`apply`), previniendo modificaciones inesperadas.

6.  **Soporte de la Comunidad**: Una comunidad de usuarios grande y activa que ofrece una gran cantidad de documentación y tutoriales.

7.  **Integración con Otras Herramientas**: Se integra fácilmente con el ecosistema DevOps como Docker, Kubernetes, Ansible y Jenkins.

8.  **Lenguaje HCL**: Utiliza el Lenguaje de Configuración de HashiCorp (HCL), diseñado para ser legible y expresivo tanto para desarrolladores como para operadores.