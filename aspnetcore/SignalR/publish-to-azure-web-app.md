---
title: Publicar un núcleo de ASP.NET SignalR aplicación a aplicación Web de Azure
author: rachelappel
description: Publicar un núcleo de ASP.NET SignalR aplicación a aplicación Web de Azure
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/20/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: fd7d38ad47d9004db2ae7b5858dc22609943f601
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2018
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a><span data-ttu-id="13c22-103">Publicar un núcleo de ASP.NET SignalR aplicación a una aplicación Web de Azure</span><span class="sxs-lookup"><span data-stu-id="13c22-103">Publish an ASP.NET Core SignalR app to an Azure Web App</span></span>

<span data-ttu-id="13c22-104">[Aplicación Web de Azure](/azure/app-service/app-service-web-overview) es un [Microsoft la informática en nube](https://azure.microsoft.com/) servicio de plataforma para hospedar las aplicaciones web, incluido ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="13c22-104">[Azure Web App](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="13c22-105">Publicar la aplicación</span><span class="sxs-lookup"><span data-stu-id="13c22-105">Publish the app</span></span>

<span data-ttu-id="13c22-106">Visual Studio proporciona herramientas integradas para la publicación en una aplicación Web de Azure.</span><span class="sxs-lookup"><span data-stu-id="13c22-106">Visual Studio provides built-in tools for publishing to an Azure Web App.</span></span> <span data-ttu-id="13c22-107">Puede usar el usuario de Visual Studio Code [CLI de Azure](/cli/azure) comandos para publicar aplicaciones para Azure.</span><span class="sxs-lookup"><span data-stu-id="13c22-107">Visual Studio Code user can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="13c22-108">Este artículo trata la publicación con las herramientas de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13c22-108">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="13c22-109">Para publicar una aplicación mediante la CLI de Azure, consulte [publicar una aplicación de ASP.NET Core en Azure con las herramientas de línea de comandos](xref:tutorials/publish-to-azure-webapp-using-cli).</span><span class="sxs-lookup"><span data-stu-id="13c22-109">To publish an app using Azure CLI, see [Publish an ASP.NET Core app to Azure with command line tools](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

<span data-ttu-id="13c22-110">Haga doble clic en el proyecto en **el Explorador de soluciones** y seleccione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="13c22-110">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span> <span data-ttu-id="13c22-111">Confirme que **crear nuevo** está activada en la **elegir un destino de publicación** cuadro de diálogo y seleccione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="13c22-111">Confirm that **Create new** is checked in the **Pick a publish target** dialog, and select **Publish**.</span></span>

![Selección de destino de publicación](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

<span data-ttu-id="13c22-113">Escriba la siguiente información en el **crear servicio en la aplicación** cuadro de diálogo y seleccione **crear**.</span><span class="sxs-lookup"><span data-stu-id="13c22-113">Enter the following information in the **Create App Service** dialog and select **Create**.</span></span>

| <span data-ttu-id="13c22-114">Elemento</span><span class="sxs-lookup"><span data-stu-id="13c22-114">Item</span></span> | <span data-ttu-id="13c22-115">Descripción</span><span class="sxs-lookup"><span data-stu-id="13c22-115">Description</span></span> |
| ---- | ----------- |
| <span data-ttu-id="13c22-116">**Nombre de la aplicación**</span><span class="sxs-lookup"><span data-stu-id="13c22-116">**App name**</span></span> | <span data-ttu-id="13c22-117">Un nombre único de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="13c22-117">A unique name of the app.</span></span> |
| <span data-ttu-id="13c22-118">**Suscripción**</span><span class="sxs-lookup"><span data-stu-id="13c22-118">**Subscription**</span></span> | <span data-ttu-id="13c22-119">La suscripción de Azure que usa la aplicación.</span><span class="sxs-lookup"><span data-stu-id="13c22-119">The Azure subscription that the app uses.</span></span> |
| <span data-ttu-id="13c22-120">**Grupo de recursos**</span><span class="sxs-lookup"><span data-stu-id="13c22-120">**Resource Group**</span></span> | <span data-ttu-id="13c22-121">El grupo de recursos relacionados a la que pertenece la aplicación.</span><span class="sxs-lookup"><span data-stu-id="13c22-121">The group of related resources to which the app belongs.</span></span>  |
| <span data-ttu-id="13c22-122">**Plan de hospedaje**</span><span class="sxs-lookup"><span data-stu-id="13c22-122">**Hosting Plan**</span></span> | <span data-ttu-id="13c22-123">El plan de precios de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="13c22-123">The pricing plan for the web app.</span></span> |

![Crear servicio de aplicaciones](publish-to-azure-web-app/_static/create-app-service-dialog.png)

<span data-ttu-id="13c22-125">Visual Studio realiza las tareas siguientes:</span><span class="sxs-lookup"><span data-stu-id="13c22-125">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="13c22-126">Crea un perfil de publicación que contiene la configuración de publicación.</span><span class="sxs-lookup"><span data-stu-id="13c22-126">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="13c22-127">Crea o utiliza un archivo *aplicación Web de Azure* con los detalles proporcionados.</span><span class="sxs-lookup"><span data-stu-id="13c22-127">Creates or uses an existing *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="13c22-128">Publica la aplicación.</span><span class="sxs-lookup"><span data-stu-id="13c22-128">Publishes the app.</span></span>
* <span data-ttu-id="13c22-129">Inicia un explorador, con la aplicación web publicada cargada.</span><span class="sxs-lookup"><span data-stu-id="13c22-129">Launches a browser, with the published web app loaded.</span></span>

<span data-ttu-id="13c22-130">Tenga en cuenta el formato de la dirección URL para la aplicación es *.azurewebsites {nombre de la aplicación} .net*.</span><span class="sxs-lookup"><span data-stu-id="13c22-130">Notice the format of the URL for the app is *{app name}.azurewebsites.net*.</span></span> <span data-ttu-id="13c22-131">Por ejemplo, una aplicación denominada `SignalRChattR` tiene una dirección URL similar a `https://signalrchattr.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="13c22-131">For example, an app named `SignalRChattR` has a URL that looks like `https://signalrchattr.azurewebsites.net`.</span></span>

<span data-ttu-id="13c22-132">Si se produce un error de HTTP 502.2, consulte [versión de vista previa de implementar ASP.NET Core para el servicio de aplicaciones de Azure](xref:host-and-deploy/azure-apps/index) para resolverlo.</span><span class="sxs-lookup"><span data-stu-id="13c22-132">If an HTTP 502.2 error occurs, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index) to resolve it.</span></span>

## <a name="configure-signalr-web-app"></a><span data-ttu-id="13c22-133">Configurar la aplicación web de SignalR</span><span class="sxs-lookup"><span data-stu-id="13c22-133">Configure SignalR web app</span></span>

<span data-ttu-id="13c22-134">Las aplicaciones ASP.NET SignalR Core que se publican como una aplicación Web de Azure debe tener [ARR afinidad](https://en.wikipedia.org/wiki/Application_Request_Routing) habilitado.</span><span class="sxs-lookup"><span data-stu-id="13c22-134">ASP.NET Core SignalR apps that are published as an Azure Web App must have [ARR Affinity](https://en.wikipedia.org/wiki/Application_Request_Routing) enabled.</span></span> <span data-ttu-id="13c22-135">[WebSockets](xref:fundamentals/websockets) debe habilitarse para permitir que el transporte de WebSockets a función.</span><span class="sxs-lookup"><span data-stu-id="13c22-135">[WebSockets](xref:fundamentals/websockets) should be enabled, to allow the WebSockets transport to function.</span></span>

<span data-ttu-id="13c22-136">En el portal de Azure, vaya a **configuración de la aplicación** para su aplicación web.</span><span class="sxs-lookup"><span data-stu-id="13c22-136">In the Azure portal, navigate to **App Settings** for your web app.</span></span> <span data-ttu-id="13c22-137">Establecer **WebSockets** a **en**y compruebe **ARR afinidad** es **en**.</span><span class="sxs-lookup"><span data-stu-id="13c22-137">Set **WebSockets** to **On**, and verify **ARR Affinity** is **On**.</span></span>

![Configuración de la aplicación Web Azure en el portal de Azure](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 <span data-ttu-id="13c22-139">WebSockets y otros transportes [están limitados según el Plan de servicio de aplicaciones](/azure/azure-subscription-service-limits#app-service-limits).</span><span class="sxs-lookup"><span data-stu-id="13c22-139">WebSockets and other transports [are limited based on the App Service Plan](/azure/azure-subscription-service-limits#app-service-limits).</span></span>

## <a name="related-resources"></a><span data-ttu-id="13c22-140">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="13c22-140">Related resources</span></span>

* [<span data-ttu-id="13c22-141">Publicar una aplicación de ASP.NET Core en Azure con las herramientas de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="13c22-141">Publish an ASP.NET Core app to Azure with command line tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [<span data-ttu-id="13c22-142">Publicar una aplicación de ASP.NET Core en Azure con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="13c22-142">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)
* [<span data-ttu-id="13c22-143">Hospedar e implementar aplicaciones de ASP.NET Core Preview en Azure</span><span class="sxs-lookup"><span data-stu-id="13c22-143">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)