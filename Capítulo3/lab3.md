# Laboratorio 3: Gestión de Cambios en la Infraestructura con OpenTofu

## Objetivo de la práctica

Al finalizar la práctica, serás capaz de:

- Aplicar cambios a la infraestructura de manera controlada con OpenTofu.
- Analizar y entender el impacto de los cambios antes de aplicarlos.
- Administrar versiones y modificaciones en la infraestructura en Azure.

## Duración aproximada
- 40 minutos.

---

**[⬅️ Atrás](https://netec-mx.github.io/OPE_TOF_EES1/Cap%C3%ADtulo2/lab2.html)** | **[Lista General](https://netec-mx.github.io/README.md)** | **[Siguiente ➡️](https://netec-mx.github.io/OPE_TOF_EES1/Cap%C3%ADtulo4/lab4.html)**

## Instrucciones

### Tarea 1: Preparación del Entorno

1. **Abrir Visual Studio Code**
   - Inicia Visual Studio Code desde el menú de inicio o con el comando `code` en la terminal.
   - Abre la terminal integrada con `Ctrl + Ñ` (o `Ctrl + Shift + P` y selecciona `Terminal: New Terminal`).

2. **Navegar a la Carpeta de Trabajo**
   - Asegúrate de estar en la carpeta principal de trabajo `OpenTofuLabs`:
     ```powershell
     cd OpenTofuLabs
     ```

3. **Crear un Directorio para este Laboratorio**
   - Crea una carpeta específica para este laboratorio:
     ```powershell
     mkdir Lab3_Gestion_Cambios
     cd Lab3_Gestion_Cambios
     ```
     ![tofu3](../images/lab3/img1.png)

4. **Inicializar un Proyecto de OpenTofu**
   - Ejecuta el siguiente comando para inicializar OpenTofu en la nueva carpeta:
     ```powershell
     cp ..\Lab1_Init\providers.tf .
     tofu init
     ```

**¡TAREA FINALIZADA!**

---

### Tarea 2: Aplicación de Cambios en la Infraestructura

1. **Definir el Archivo Principal `main.tf`**
   - Crea un archivo llamado `main.tf` y agrega el siguiente contenido:
     ```hcl
     resource "azurerm_resource_group" "alter-rg" {
       name     = "TofuManagedGroup"
       location = "East US"
     }
     ```
     ![tofu4](../images/lab3/img2.png)

2. **Crear el Grupo de Recursos en Azure**
   - Ejecuta los siguientes comandos en la terminal de VS Code:
     ```powershell
     tofu plan
     tofu apply -auto-approve
     ```
     ![tofu4](../images/lab3/img3.png)
   - Confirma que el grupo de recursos ha sido creado correctamente en Azure.

3. **Modificar la Infraestructura**
   - Abre `main.tf` y cambia la ubicación del grupo de recursos:
     ```hcl
     resource "azurerm_resource_group" "main" {
       name     = "TofuManagedGroup"
       location = "West US"
     }
     ```
     ![tofu5](../images/lab3/img4.png)

4. **Planificar los Cambios**
   - Antes de aplicar el cambio, revisa el impacto con:
     ```powershell
     tofu plan
     ```
     ![tofu6](../images/lab3/img5.png)

   - Este comando mostrará qué modificaciones se realizarán en Azure.

5. **Aplicar los Cambios**
   - Si el plan es correcto, ejecuta:
     ```powershell
     tofu apply -auto-approve
     ```
     ![tofu7](../images/lab3/img6.png)
   - Verifica que los cambios hayan sido aplicados correctamente.

**¡TAREA FINALIZADA!**

---

### Tarea 3: Gestión de Versiones y Auditoría de Cambios

1. **Habilitar el Control de Versiones**
   - Inicializa un repositorio Git en la carpeta del laboratorio:
   - **NOTA:** Esta es una practica, pero estos archivos normalmente no se suben al repositorio *`**.tfstate, *.tfstate.backup, .tofu/, *.tfplan, *.tfvars, *.tfvars.json, *.log, crash.log`**. Deben agregarse al archivo `.gitignore`
     ```powershell
     git init
     git add .
     git commit -m "Versión inicial de infraestructura"
     ```
     ![tofu8](../images/lab3/img7.png)
     ![tofu9](../images/lab3/img8.png)
     ![tofu10](../images/lab3/img9.png)

2. **Realizar Modificaciones Adicionales**
   - Edita `main.tf` para agregar etiquetas al grupo de recursos:
     ```hcl
     resource "azurerm_resource_group" "main" {
       name     = "TofuManagedGroup"
       location = "West US"
       tags = {
         environment = "Development"
         owner       = "Admin"
       }
     }
     ```
     ![tofu11](../images/lab3/img10.png)

3. **Planificar y Aplicar los Nuevos Cambios**
   - Revisa el impacto antes de aplicar:
     ```powershell
     tofu plan
     ```
     ![tofu12](../images/lab3/img11.png)
   - Aplica las modificaciones:
     ```powershell
     tofu apply -auto-approve
     ```
     ![tofu13](../images/lab3/img12.png)

4. **Registrar los Cambios en el Repositorio**
   - Guarda el estado actual con Git:
     ```powershell
     git add .
     git commit -m "Agregadas etiquetas al grupo de recursos"
     ```
     ![tofu14](../images/lab3/img13.png)

5. **Revisar el Historial de Cambios**
   - Consulta el historial de modificaciones con:
     ```powershell
     git log --oneline
     ```
     ![tofu15](../images/lab3/img14.png)

6. **Validar los Recursos en Azure**
   - Desde la terminal, ejecuta:
     ```powershell
     az group list
     ```
     ![tofu16](../images/lab3/img15.png)

**¡TAREA FINALIZADA!**

---

## Resumen

En esta práctica, aplicamos cambios a la infraestructura con OpenTofu, analizamos su impacto, gestionamos versiones con Git y auditamos modificaciones en Azure, asegurando un control eficiente de los cambios.

---

**[⬅️ Atrás](https://netec-mx.github.io/OPE_TOF_EES1/Cap%C3%ADtulo2/lab2.html)** | **[Lista General](https://netec-mx.github.io/README.md)** | **[Siguiente ➡️](https://netec-mx.github.io/OPE_TOF_EES1/Cap%C3%ADtulo4/lab4.html)**

---