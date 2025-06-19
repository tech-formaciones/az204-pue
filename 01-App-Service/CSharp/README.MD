
## OBJETIVO

* Crear una app en .NET Core
* Deplegarla en Azure App Service

---

### ⚠️ Requisitos previos:

* Tener instalado el **Azure CLI**
* Haber ejecutado `az login`
* Tener un **App Service** creado en Azure
* El plan del App Service debe soportar despliegue ZIP local (`az webapp deploy`)

---

### 1. Crear el proyecto MVC

```bash
dotnet new mvc -n MiProyectoMVC
cd MiProyectoMVC
```

---

### 2. Publicar el proyecto (compilarlo para producción)

```bash
dotnet publish -c Release -o ./publish
```

---

### 3. Crear un archivo `.zip`

```bash
cd publish
zip -r ../MiProyectoMVC.zip *
cd ..
```

> En PowerShell (Windows):

```powershell
Compress-Archive -Path .\publish\* -DestinationPath .\MiProyectoMVC.zip
```

---

### 4. Desplegar usando `az webapp deploy`

```bash
az webapp deploy \
  --resource-group NombreDelGrupoDeRecursos \
  --name NombreDelAppService \
  --src-path MiProyectoMVC.zip \
  --type zip
```

