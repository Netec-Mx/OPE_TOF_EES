# Práctica 4. Administración de dependencias entre módulos en OpenTofu

## Objetivo de la práctica

Al finalizar la práctica, serás capaz de:

- Comprender y gestionar dependencias entre módulos en OpenTofu.
- Organizar la infraestructura en módulos reutilizables.
- Implementar relaciones entre módulos para mejorar la escalabilidad.

## Duración aproximada
- 60 minutos.

---

**[⬅️ Atrás](https://netec-mx.github.io/OPE_TOF_EES/Cap%C3%ADtulo3/lab3.html)** | **[Lista General](https://netec-mx.github.io/OPE_TOF_EES/)** | **[Siguiente ➡️](https://netec-mx.github.io/OPE_TOF_EES/Cap%C3%ADtulo1/lab1.html)**

## Instrucciones

### Tarea 1: Configuración del entorno

1. **Abrir Visual Studio Code**
   - Iniciar Visual Studio Code desde el menú de inicio o con el comando `code` en la terminal.
   - Abrir la terminal integrada con `Ctrl + Ñ` (o `Ctrl + Shift + P` y seleccionar `Terminal: New Terminal`).

2. **Navegar a la carpeta de trabajo**
   - Asegúrate de estar en la carpeta principal de trabajo `OpenTofuLabs`:
     ```powershell
     cd OpenTofuLabs
     ```

3. **Crear un directorio para este laboratorio**
   - Crear una carpeta específica para este laboratorio:
     ```powershell
     mkdir Lab4_Dependencias_Modulos
     cd Lab4_Dependencias_Modulos
     ```

4. **Inicializar un proyecto de OpenTofu**
   - Ejecutar el siguiente comando para inicializar OpenTofu en la nueva carpeta:
     ```powershell
     tofu init
     ```

**¡TAREA FINALIZADA!**

---

### Tarea 2: Creación de módulos en OpenTofu

1. **Crear la estructura de módulos**
   - Dentro de `Lab4_Dependencias_Modulos`, crear un directorio llamado `modules`:
     ```powershell
     mkdir modules
     ```
     ![tofu4](../images/lab4/img1.png)
   - Dentro de `modules`, crear dos carpetas para los módulos:
     ```powershell
     mkdir modules/resource_group
     mkdir modules/storage_account
     ```
     ![tofu5](../images/lab4/img2.png)

2. **Definir el módulo de grupo de recursos**
   - Dentro de `modules/resource_group`, crear un archivo `variables.tf`.
     ```hcl
     variable "rg-mod" {
       type        = string
       description = "Nombre del grupo de recursos"
     }
     
     variable "loc-mod" {
       type        = string
       description = "Ubicación del grupo de recursos"
     }
     ```
     ![tofu6](../images/lab4/img3.png)

   - Crear un archivo `main.tf` con el siguiente contenido en la misma carpeta:
     ```hcl
     resource "azurerm_resource_group" "rgmodule" {
       name     = var.rg-mod
       location = var.loc-mod
     }
     ```
     ![tofu7](../images/lab4/img4.png)
   
   - Crea un archivo `outputs.tf`:
     ```hcl
     output "resource_group_name" {
       value = azurerm_resource_group.rgmodule.name
     }
     ```
     ![tofu8](../images/lab4/img5.png)

3. **Definir el módulo de almacenamiento**
   - Dentro de `modules/storage_account`, crear un archivo `variables.tf`.
     ```hcl
     variable "sa-name" {
       type        = string
       description = "Nombre de la cuenta de almacenamiento"
     }
     
     variable "rg-name" {
       type        = string
       description = "Grupo de recursos donde se creará el almacenamiento"
     }
     
     variable "loc-name" {
       type        = string
       description = "Ubicación de la cuenta de almacenamiento"
     }
     ```
     ![tofu9](../images/lab4/img6.png)

   - Crear un archivo `main.tf` con el siguiente contenido, en la misma carpeta:
     ```hcl
     resource "azurerm_storage_account" "sa-module" {
       name                     = var.sa-name
       resource_group_name      = var.rg-name
       location                 = var.loc-name
       account_tier             = "Standard"
       account_replication_type = "LRS"
     }
     ```
     ![tofu10](../images/lab4/img7.png)
   
   - Crear un archivo `outputs.tf`:
     ```hcl
     output "sa-out" {
       value = azurerm_storage_account.sa-module.name
     }
     ```
     ![tofu11](../images/lab4/img8.png)

**¡TAREA FINALIZADA!**

---

### Tarea 3: Implementación de los Módulos y Gestión de Dependencias

1. **Crear el Archivo Principal `main.tf`**
   - En `Lab4_Dependencias_Modulos`, crear un archivo `main.tf` con el siguiente contenido:
   ![tofu12](../images/lab4/img9.png)
   - **NOTA:** Sustituir las letras `X` por letras y numeros aleatorios, en **minusculas**.
     ```hcl
     provider "azurerm" {
       features {}
       resource_provider_registrations = "none"
     }
     
     module "resource_group" {
       source              = "./modules/resource_group"
       rg-mod               = "RGFromModule"
       loc-mod              = "East US"
     }
     
     module "storage_account" {
       source                = "./modules/storage_account"
       sa-name                = "safrommodulexxxx"
       rg-name                = module.resource_group.resource_group_name
       loc-name               = "East US"
     }
     ```
     ![tofu13](../images/lab4/img10.png)

2. **Inicializar y aplicar la configuración**
   - Desde la terminal, ejecutar:
     ```powershell
     tofu init
     ```
     ![tofu14](../images/lab4/img11.png)
     ```powershell
     tofu plan
     ```
     ![tofu15](../images/lab4/img12.png)

   - `tofu plan` mostrará las dependencias entre los módulos antes de la ejecución.

3. **Aplicar la configuración y verificar resultados**
   - Ejecutar:
     ```powershell
     tofu apply -auto-approve
     ```
     ![tofu16](../images/lab4/img13.png)

4. **Validar los recursos en Azure**
   - Desde la terminal, ejecutar:
     ```powershell
     az group list --output table
     ```
     ![tofu17](../images/lab4/img14.png)
   - Verificar que se haya creado el Storage Account, desde la terminal, ejecutar:
     ```powershell
     az resource list --resource-group RGFromModule --output table
     ```
     ![tofu18](../images/lab4/img15.png)

**¡TAREA FINALIZADA!**

---

## Resumen

En esta práctica, aprendimos a gestionar dependencias entre módulos en OpenTofu, organizar la infraestructura en módulos reutilizables y aplicar cambios con propagación automática a los recursos dependientes.

---

**[⬅️ Atrás](https://netec-mx.github.io/OPE_TOF_EES/Cap%C3%ADtulo3/lab3.html)** | **[Lista General](https://netec-mx.github.io/OPE_TOF_EES/)** | **[Siguiente ➡️](https://netec-mx.github.io/OPE_TOF_EES/Cap%C3%ADtulo1/lab1.html)**

---
