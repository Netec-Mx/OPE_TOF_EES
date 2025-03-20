# Laboratorio 1: Instalación y Configuración de OpenTofu en Azure

## Objetivo de la práctica

Al finalizar la práctica, serás capaz de:

- Instalar OpenTofu y las herramientas necesarias para trabajar con Azure.
- Realizar la configuración inicial de OpenTofu en Azure.

## Duración aproximada

- 30 minutos

---

**[⬅️ Atrás](https://netec-mx.github.io/OPE_TOF_EES1/Cap%C3%ADtulo4/lab4.html)** | **[Lista General](https://github.com/Netec-Mx/OPE_TOF_EES1/blob/main/README.md#-lista-de-laboratorios)** | **[Siguiente ➡️](https://netec-mx.github.io/OPE_TOF_EES1/Cap%C3%ADtulo2/lab2.html)**

## Instrucciones

### Tarea 1: Requisitos Previos

Antes de proceder con la instalación, asegúrate de cumplir con los siguientes requisitos:

**NOTA:** Puedes visitar esta página para validar los requisitos [Instalación de OpenTofu](https://opentofu.org/docs/intro/install/)

**IMPORTANTE:** Para esta practica se usara el Sistema Operativo **Windows**

1. **Sistema Operativo Compatible**: 
   - Verifica que tienes Windows 10. Para hacerlo:
     - Presiona `Win + R`, escribe `winver` y presiona `Enter`.
     - Asegúrate de que la versión es Windows 10.
2. **Espacio en Disco**: 
   - Comprueba que tienes al menos 10 GB de espacio libre:
     - Abre `Explorador de Archivos` → `Este equipo` → Revisa el almacenamiento disponible.
3. **Permisos de Administrador**: 
   - Asegúrate de que tienes permisos de administrador:
     - Haz clic derecho en `Símbolo del sistema` o `PowerShell` y selecciona `Ejecutar como administrador`.

**¡TAREA FINALIZADA!**

Ya cuentas con el entorno adecuado para la instalación.

---

### Tarea 2: Instalación de OpenTofu y Herramientas de Azure

Sigue los siguientes pasos para instalar las herramientas necesarias:

1. **Instalar Azure CLI**
   - Descarga el instalador desde [Microsoft](https://aka.ms/installazurecliwindows).
   - Ejecuta el instalador `.msi` y sigue las instrucciones en pantalla.
   - Una vez instalado, verifica la instalación con `az --version` en `PowerShell`.

   ![azversion](../images/lab1/img1.png)

2. **Instalar OpenTofu**
   - Descarga el binario de OpenTofu en zip desde [su sitio oficial](https://github.com/opentofu/opentofu/releases/download/v1.9.0/tofu_1.9.0_windows_386.zip).
   - Extrae el archivo `tofu.exe` en una ubicación accesible, por ejemplo: `C:\Program Files\OpenTofu`.
   - **NOTA:** Si no existe la carpeta `OpenTofu` debes crearla.
   - Agrega la ruta de OpenTofu a las variables de entorno:
     - Abre `Configuración avanzada del sistema` → `Variables de entorno`.
     - En `Path` de `Variables del Sistema`, haz clic en `Editar` y agrega la ruta donde copiaste `tofu.exe`.
     ![tofu1](../images/lab1/img2.png)
     - Guarda los cambios y cierra la ventana.
   - Verifica la instalación ejecutando en `PowerShell`:
   - **NOTA:** Quizas debas cerrar y abrir de nuevo `Powershell` para que tome efecto la variable

     ```powershell
     tofu --version
     ```

     ![tofu2](../images/lab1/img3.png)
3. **Instalar Visual Studio Code y la Extensión de Terraform**
   - Descarga [VS Code](https://code.visualstudio.com/) e instálalo.
   - **NOTA:** Ya esta instalado en el ambiente, busca el acceso directo para abrirlo y avanza al siguiente punto.
   - Abre VS Code, ve a `Extensiones` (`Ctrl + Shift + X`) y busca `HashiCorp Terraform`.
   - Instala la extensión para facilitar la edición y ejecución de archivos `.tf` en OpenTofu.
   ![tofu3](../images/lab1/img4.png)
   ![tofu4](../images/lab1/img5.png)

**¡TAREA FINALIZADA!**

Las herramientas de OpenTofu y Azure han sido instaladas correctamente.

---

### Tarea 3: Configuración Inicial de OpenTofu en Azure desde Visual Studio Code

1. **Crear la Carpeta de Trabajo**
   - Abre Visual Studio Code.
   - Abre la terminal (`Ctrl + Ñ`) y ejecuta:
     ```powershell
     cd .\Desktop\
     mkdir OpenTofuLabs
     cd OpenTofuLabs
     ```
   - Esta será la carpeta de trabajo para los laboratorios de OpenTofu.
   - Tambien abrela en el visualizador de "VSC", clic en el botón `Open Folder`
   ![tofu5](../images/lab1/img6.png)
   - Busca la carpeta `OpenTofuLabs` y abrela.
   - Una vez abierta confirma el autor.
   ![tofu6](../images/lab1/img7.png)

2. **Iniciar Sesión en Azure**
   - Desde la terminal de VS Code, ejecuta:
     ```powershell
     az login
     ```
   - Se abrirá una ventana en el navegador para iniciar sesión en tu cuenta de Azure.
   ![tofu7](../images/lab1/img8.png)
   - Ingresa los datos que se te asignaron al curso.
   - Una vez autenticado, verás los detalles de tu suscripción en la terminal.
   ![tofu8](../images/lab1/img9.png)
   - Confirma la suscripción escribe `1` o ejecuta `Enter`.

3. **Seleccionar la Suscripción Correcta (OPCIONAL)**
   - Desde la terminal de VS Code, ejecuta:
     ```powershell
     az account list --output table
     ```
   - Copia el `id` de la suscripción deseada y usa:
     ```powershell
     az account set --subscription "ID_DE_TU_SUSCRIPCIÓN"
     ```
    - Copia el `id` de la suscripción deseada y guardalo en un bloc de notas.

4. **Configurar Credenciales de Azure para OpenTofu**
   - Crea un archivo de configuración ejecutando, sustituye el paremetro `ID_DE_TU_SUSCRIPCIÓN` por el valor que copiaste:
     ```powershell
     az ad sp create-for-rbac --display-name="tofurole" --role="Contributor" --scopes="/subscriptions/ID_DE_TU_SUSCRIPCIÓN"
     ```
     ![tofu9](../images/lab1/img10.png)
   - Copia los valores de `appId`, `password` y `tenant` y configúralos como variables de entorno:
   - **NOTA:** Puedes guardarlos en un bloc de notas por si los requieres mas adelate.
     ```powershell
     $env:ARM_CLIENT_ID="appId"
     $env:ARM_CLIENT_SECRET="password"
     $env:ARM_TENANT_ID="tenant"
     $env:ARM_SUBSCRIPTION_ID="ID_DE_TU_SUSCRIPCIÓN"
     ```
     ![tofu10](../images/lab1/img11.png)
5. **Verificar Configuración**
   - Dentro de `OpenTofuLabs`, crea una carpeta específica para este laboratorio:
     ```powershell
     mkdir Lab1_Init
     cd Lab1_Init
     ```
   - Dentro de la carpeta, crea un archivo `providers.tf` con el siguiente contenido:
   ![tofu11](../images/lab1/img12.png)
     ```hcl
     provider "azurerm" {
       features {}
     }
     ```
     ![tofu12](../images/lab1/img13.png)
   - Ejecuta los siguientes comandos en la terminal de VS Code:
     ```powershell
     tofu init
     ```
     ![tofu13](../images/lab1/img14.png)
     ```powershell
     tofu validate
     ```
     ![tofu14](../images/lab1/img15.png)
   - Si no hay errores, la configuración es correcta.

**¡TAREA FINALIZADA!**

El entorno de OpenTofu en Azure está listo para su uso dentro de Visual Studio Code.

---

## Resumen

En esta práctica, hemos revisado los requisitos previos, instalado OpenTofu y las herramientas necesarias para trabajar con Azure y configurado el entorno en Windows. Ahora, todo se ejecuta desde Visual Studio Code en la carpeta `OpenTofuLabs`.

---

**[⬅️ Atrás](https://netec-mx.github.io/OPE_TOF_EES1/Cap%C3%ADtulo4/lab4.html)** | **[Lista General](https://netec-mx.github.io/README.md)** | **[Siguiente ➡️](https://netec-mx.github.io/OPE_TOF_EES1/Cap%C3%ADtulo2/lab2.html)**

---
