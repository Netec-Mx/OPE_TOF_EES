# Laboratorio 2: Creación de Variables, Salidas y Recursos en OpenTofu

## Objetivo de la práctica

Al finalizar la práctica, serás capaz de:

- Definir y utilizar variables en OpenTofu.
- Configurar y visualizar salidas (outputs).
- Declarar y administrar recursos en Azure mediante OpenTofu.

## Duración aproximada
- 40 minutos.

---

**[⬅️ Atrás](https://netec-mx.github.io/OPE_TOF_EES1/Cap%C3%ADtulo1/lab1.html)** | **[Lista General](https://netec-mx.github.io/OPE_TOF_EES1/)** | **[Siguiente ➡️](https://netec-mx.github.io/OPE_TOF_EES1/Cap%C3%ADtulo3/lab3.html)**

## Instrucciones

### Tarea 1: Configuración del Entorno

1. **Abrir Visual Studio Code**
   - Abre Visual Studio Code.
   - Abre la terminal (`Ctrl + Ñ`) y navega a la carpeta de trabajo:
     ```powershell
     cd OpenTofuLabs
     ```

2. **Crear un nuevo directorio para el laboratorio**
   - Dentro de `OpenTofuLabs`, crea una carpeta específica para este laboratorio:
     ```powershell
     mkdir Lab2_Variables_Recursos
     cd Lab2_Variables_Recursos
     cp ..\providers.tf .
     ```

3. **Inicializar un proyecto de OpenTofu**
   - Ejecuta el siguiente comando para inicializar OpenTofu en la nueva carpeta:
     ```powershell
     tofu init
     ```
   - Este comando descargará los proveedores necesarios y configurará el entorno para trabajar con OpenTofu.
     ![tofu2](../images/lab2/img1.png)

**¡TAREA FINALIZADA!**

---

### Tarea 2: Definición de Variables y Salidas

1. **Crear y Definir Variables en un Archivo `.tf`**
   - En `Lab2_Variables_Recursos`, crea un archivo llamado `variables.tf` y agrega el siguiente contenido:
     ```hcl
     variable "lab2-rg" {
       description = "Nombre del grupo de recursos en Azure"
       type        = string
     }

     variable "location" {
      description = "Ubicación del grupo de recursos"
      type        = string
     }
     ```
     ![tofu3](../images/lab2/img2.png)
2. **Definir Variables en un Archivo `.tfvars`**
   - Crea un archivo `terraform.tfvars` en la misma carpeta y agrega el siguiente contenido:
   **NOTA:** Cambia las letras `X` por numeros o letras aleatorias.
     ```hcl
     lab2-rg  = "lab2-tofu-rg-XX"
     location = "East US""
     ```
     ![tofu4](../images/lab2/img3.png)
   - Este archivo permite definir valores de variables sin modificar `variables.tf`.

3. **Definir Variables desde la Terminal**
   - Puedes sobrescribir valores al ejecutar OpenTofu con parámetros adicionales:
     ```powershell
     tofu plan -var="lab2-rg=CLIResourceGroup" -var="location=Central US"
     ```
     ![tofu5](../images/lab2/img4.png)
   - Este método es útil para configuraciones temporales o personalizadas sin modificar archivos.

4. **Definir Salidas (Outputs)**
   - En `Lab2_Variables_Recursos`, crea un archivo llamado `outputs.tf` con el siguiente contenido:
     ```hcl
     output "tofu-rg-out" {
       description = "Nombre del grupo de recursos creado"
       value       = azurerm_resource_group.main.name
     }

     output "tofu-location-out" {
       description = "Ubicación del grupo de recursos"
       value       = azurerm_resource_group.main.location
     }
     ```
     ![tofu6](../images/lab2/img5.png)

5. **Verificar la Sintaxis**
   - Dentro de la terminal de VS Code, ejecuta:
     ```powershell
     tofu fmt
     ```
   - `tofu fmt` formatea los archivos de configuración.

**¡TAREA FINALIZADA!**

---

### Tarea 3: Declaración y Creación de Recursos

1. **Crear el Archivo Principal `main.tf`**
   - En `Lab2_Variables_Recursos`, crea un archivo llamado `main.tf` con el siguiente contenido:
     ```hcl     
     resource "azurerm_resource_group" "main" {
       name     = var.resource_group_name
       location = var.location
     }
     ```
     ![tofu7](../images/lab2/img6.png)

2. **Inicializar y Planificar la Configuración**
   - Desde la terminal de VS Code, ejecuta:
     ```powershell
     tofu init
     tofu plan
     ```
   - `tofu init` inicializa el entorno y descarga dependencias.
   - `tofu plan` genera un plan de ejecución sin aplicarlo, mostrando los cambios que se realizarán en Azure.
   ![tofu8](../images/lab2/img7.png)

3. **Aplicar la Configuración y Crear los Recursos**
   - Si el plan es correcto y no hay errores, ejecuta:
     ```powershell
     tofu apply -auto-approve
     ```
   - OpenTofu desplegará los recursos en Azure.

4. **Verificar la Creación de Recursos**
   - Una vez completado, revisa los valores de salida ejecutando:
     ```powershell
     tofu output
     ```
   - Este comando mostrará los valores definidos en `outputs.tf`, confirmando la creación exitosa del recurso.
   ![tofu9](../images/lab2/img8.png)

5. **Validar los Recursos en Azure**
   - Desde la terminal, ejecuta:
     ```powershell
     az group list --output table
     ```
     ![tofu10](../images/lab2/img9.png)
   - Este comando consultará Azure para verificar que el grupo de recursos se haya creado correctamente.

**¡TAREA FINALIZADA!**

### Tarea Opcional: Nueva Variable y Aplicación de cambios

**Objetivo:** Crea una nueva variable y usala en un nuevo grupo de recursos.

- Agrega la siguiente variable llamada `retovar`
- Agrega el siguiente valor `reto-rg`
- Deja la ubicación por defecto
- Crea un nuevo RG que llame el valor de la nueva variable
- Agrega un output llamado `reto-out` que devuelva el nombre del RG

---

## Resumen

En esta práctica, se aprendio a definir variables mediante archivos `.tf`, `.tfvars` y la terminal, configuramos salidas para visualizar información clave, desplegamos un grupo de recursos en Azure con OpenTofu y validamos su correcta ejecución..

---

**[⬅️ Atrás](https://netec-mx.github.io/OPE_TOF_EES1/Cap%C3%ADtulo1/lab1.html)** | **[Lista General](https://netec-mx.github.io/OPE_TOF_EES1/)** | **[Siguiente ➡️](https://netec-mx.github.io/OPE_TOF_EES1/Cap%C3%ADtulo3/lab3.html)**

---
