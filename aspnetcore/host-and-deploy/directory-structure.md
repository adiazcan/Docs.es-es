---
title: Estructura de directorios de ASP.NET Core
author: guardrex
description: Ver la estructura de directorio de aplicaciones de ASP.NET Core publicadas.
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/directory-structure
ms.openlocfilehash: 27f0f40aea1c55315642d7d6f9b9d7be3e111cb4
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2018
---
# <a name="directory-structure-of-published-aspnet-core-apps"></a><span data-ttu-id="3e807-103">Estructura de directorios de las aplicaciones ASP.NET Core publicadas</span><span class="sxs-lookup"><span data-stu-id="3e807-103">Directory structure of published ASP.NET Core apps</span></span>

<span data-ttu-id="3e807-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3e807-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3e807-105">En el núcleo de ASP.NET, el directorio de la aplicación, *publicar*, consta de los archivos de la aplicación, archivos de configuración, activos estáticos, paquetes y el tiempo de ejecución (para aplicaciones independientes).</span><span class="sxs-lookup"><span data-stu-id="3e807-105">In ASP.NET Core, the application directory, *publish*, is comprised of application files, config files, static assets, packages, and the runtime (for self-contained apps).</span></span>

| <span data-ttu-id="3e807-106">Tipo de aplicación</span><span class="sxs-lookup"><span data-stu-id="3e807-106">App Type</span></span>                       | <span data-ttu-id="3e807-107">Estructura de directorios</span><span class="sxs-lookup"><span data-stu-id="3e807-107">Directory Structure</span></span> |
| ------------------------------ | ------------------- |
| <span data-ttu-id="3e807-108">Implementación del marco de trabajo dependiente</span><span class="sxs-lookup"><span data-stu-id="3e807-108">Framework-dependent Deployment</span></span> | <ul><li><span data-ttu-id="3e807-109">publish\*</span><span class="sxs-lookup"><span data-stu-id="3e807-109">publish\*</span></span><ul><li><span data-ttu-id="3e807-110">registros de\* (si se incluye en publishOptions)</span><span class="sxs-lookup"><span data-stu-id="3e807-110">logs\* (if included in publishOptions)</span></span></li><li><span data-ttu-id="3e807-111">Refs\*</span><span class="sxs-lookup"><span data-stu-id="3e807-111">refs\*</span></span></li><li><span data-ttu-id="3e807-112">tiempos de ejecución\*</span><span class="sxs-lookup"><span data-stu-id="3e807-112">runtimes\*</span></span></li><li><span data-ttu-id="3e807-113">Vistas\* (si se incluye en publishOptions)</span><span class="sxs-lookup"><span data-stu-id="3e807-113">Views\* (if included in publishOptions)</span></span></li><li><span data-ttu-id="3e807-114">wwwroot\* (si se incluye en publishOptions)</span><span class="sxs-lookup"><span data-stu-id="3e807-114">wwwroot\* (if included in publishOptions)</span></span></li><li><span data-ttu-id="3e807-115">archivos .dll</span><span class="sxs-lookup"><span data-stu-id="3e807-115">.dll files</span></span></li><li><span data-ttu-id="3e807-116">myapp.deps.json</span><span class="sxs-lookup"><span data-stu-id="3e807-116">myapp.deps.json</span></span></li><li><span data-ttu-id="3e807-117">MyApp.dll</span><span class="sxs-lookup"><span data-stu-id="3e807-117">myapp.dll</span></span></li><li><span data-ttu-id="3e807-118">MyApp.pdb</span><span class="sxs-lookup"><span data-stu-id="3e807-118">myapp.pdb</span></span></li><li><span data-ttu-id="3e807-119">MyApp. PrecompiledViews.dll (si precompilar vistas Razor)</span><span class="sxs-lookup"><span data-stu-id="3e807-119">myapp.PrecompiledViews.dll (if precompiling Razor Views)</span></span></li><li><span data-ttu-id="3e807-120">MyApp. PrecompiledViews.pdb (si precompilar vistas Razor)</span><span class="sxs-lookup"><span data-stu-id="3e807-120">myapp.PrecompiledViews.pdb (if precompiling Razor Views)</span></span></li><li><span data-ttu-id="3e807-121">myapp.runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="3e807-121">myapp.runtimeconfig.json</span></span></li><li><span data-ttu-id="3e807-122">Web.config (si se incluye en publishOptions)</span><span class="sxs-lookup"><span data-stu-id="3e807-122">web.config (if included in publishOptions)</span></span></li></ul></li></ul> |
| <span data-ttu-id="3e807-123">Implementación independiente</span><span class="sxs-lookup"><span data-stu-id="3e807-123">Self-contained Deployment</span></span>      | <ul><li><span data-ttu-id="3e807-124">publish\*</span><span class="sxs-lookup"><span data-stu-id="3e807-124">publish\*</span></span><ul><li><span data-ttu-id="3e807-125">registros de\* (si se incluye en publishOptions)</span><span class="sxs-lookup"><span data-stu-id="3e807-125">logs\* (if included in publishOptions)</span></span></li><li><span data-ttu-id="3e807-126">Refs\*</span><span class="sxs-lookup"><span data-stu-id="3e807-126">refs\*</span></span></li><li><span data-ttu-id="3e807-127">Vistas\* (si se incluye en publishOptions)</span><span class="sxs-lookup"><span data-stu-id="3e807-127">Views\* (if included in publishOptions)</span></span></li><li><span data-ttu-id="3e807-128">wwwroot\* (si se incluye en publishOptions)</span><span class="sxs-lookup"><span data-stu-id="3e807-128">wwwroot\* (if included in publishOptions)</span></span></li><li><span data-ttu-id="3e807-129">archivos .dll</span><span class="sxs-lookup"><span data-stu-id="3e807-129">.dll files</span></span></li><li><span data-ttu-id="3e807-130">myapp.deps.json</span><span class="sxs-lookup"><span data-stu-id="3e807-130">myapp.deps.json</span></span></li><li><span data-ttu-id="3e807-131">MyApp.exe</span><span class="sxs-lookup"><span data-stu-id="3e807-131">myapp.exe</span></span></li><li><span data-ttu-id="3e807-132">MyApp.pdb</span><span class="sxs-lookup"><span data-stu-id="3e807-132">myapp.pdb</span></span></li><li><span data-ttu-id="3e807-133">MyApp. PrecompiledViews.dll (si precompilar vistas Razor)</span><span class="sxs-lookup"><span data-stu-id="3e807-133">myapp.PrecompiledViews.dll (if precompiling Razor Views)</span></span></li><li><span data-ttu-id="3e807-134">MyApp. PrecompiledViews.pdb (si precompilar vistas Razor)</span><span class="sxs-lookup"><span data-stu-id="3e807-134">myapp.PrecompiledViews.pdb (if precompiling Razor Views)</span></span></li><li><span data-ttu-id="3e807-135">myapp.runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="3e807-135">myapp.runtimeconfig.json</span></span></li><li><span data-ttu-id="3e807-136">Web.config (si se incluye en publishOptions)</span><span class="sxs-lookup"><span data-stu-id="3e807-136">web.config (if included in publishOptions)</span></span></li></ul></li></ul> |
<span data-ttu-id="3e807-137">\*Indica un directorio</span><span class="sxs-lookup"><span data-stu-id="3e807-137">\* Indicates a directory</span></span>

<span data-ttu-id="3e807-138">El contenido de la *publicar* directorio representa el *ruta de acceso raíz del contenido*, también denominado el *ruta de acceso base de aplicación*, de la implementación.</span><span class="sxs-lookup"><span data-stu-id="3e807-138">The contents of the *publish* directory represents the *content root path*, also called the *application base path*, of the deployment.</span></span> <span data-ttu-id="3e807-139">El nombre se asigna a la *publicar* su ubicación de directorio en la implementación, actúa como ruta de acceso física del servidor a la aplicación hospedada.</span><span class="sxs-lookup"><span data-stu-id="3e807-139">Whatever name is given to the *publish* directory in the deployment, its location serves as the server's physical path to the hosted application.</span></span> <span data-ttu-id="3e807-140">El *wwwroot* directory, si está presente, sólo contiene recursos estáticos.</span><span class="sxs-lookup"><span data-stu-id="3e807-140">The *wwwroot* directory, if present, only contains static assets.</span></span> <span data-ttu-id="3e807-141">El *registros* directorio puede incluirse en la implementación mediante la creación del proyecto y agregar la `<Target>` elemento se muestra a continuación para su *.csproj* archivo o creando físicamente el directorio en el servidor.</span><span class="sxs-lookup"><span data-stu-id="3e807-141">The *logs* directory may be included in the deployment by creating it in the project and adding the `<Target>` element shown below to your *.csproj* file or by physically creating the directory on the server.</span></span>

```xml
<Target Name="CreateLogsFolder" AfterTargets="Publish">
  <MakeDir Directories="$(PublishDir)Logs" 
           Condition="!Exists('$(PublishDir)Logs')" />
  <WriteLinesToFile File="$(PublishDir)Logs\.log" 
                    Lines="Generated file" 
                    Overwrite="True" 
                    Condition="!Exists('$(PublishDir)Logs\.log')" />
</Target>
```

<span data-ttu-id="3e807-142">El `<MakeDir>` elemento crea vacío *registros* carpeta en la salida publicada.</span><span class="sxs-lookup"><span data-stu-id="3e807-142">The `<MakeDir>` element creates an empty *Logs* folder in the published output.</span></span> <span data-ttu-id="3e807-143">El elemento utiliza el `PublishDir` propiedad para determinar la ubicación de destino para crear la carpeta.</span><span class="sxs-lookup"><span data-stu-id="3e807-143">The element uses the `PublishDir` property to determine the target location for creating the folder.</span></span> <span data-ttu-id="3e807-144">Varios métodos de implementación, por ejemplo, Web Deploy, omiten las carpetas vacías durante la implementación.</span><span class="sxs-lookup"><span data-stu-id="3e807-144">Several deployment methods, such as Web Deploy, skip empty folders during deployment.</span></span> <span data-ttu-id="3e807-145">El `<WriteLinesToFile>` elemento genera un archivo en el *registros* carpeta, lo que garantiza la implementación de la carpeta en el servidor.</span><span class="sxs-lookup"><span data-stu-id="3e807-145">The `<WriteLinesToFile>` element generates a file in the *Logs* folder, which guarantees deployment of the folder to the server.</span></span> <span data-ttu-id="3e807-146">Tenga en cuenta que la creación de carpetas todavía puede fallar si el proceso de trabajo no tiene acceso de escritura a la carpeta de destino.</span><span class="sxs-lookup"><span data-stu-id="3e807-146">Note that folder creation may still fail if the worker process doesn't have write access to the target folder.</span></span>

<span data-ttu-id="3e807-147">El directorio de implementación requiere permisos de lectura y ejecución, mientras que la *registros* directory requiere permisos de lectura/escritura.</span><span class="sxs-lookup"><span data-stu-id="3e807-147">The deployment directory requires Read/Execute permissions, while the *Logs* directory requires Read/Write permissions.</span></span> <span data-ttu-id="3e807-148">Directorios adicionales donde se escribirá activos requieren permisos de lectura/escritura.</span><span class="sxs-lookup"><span data-stu-id="3e807-148">Additional directories where assets will be written require Read/Write permissions.</span></span>