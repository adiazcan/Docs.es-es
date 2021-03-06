---
title: Host principal de ASP.NET en un servicio de Windows
author: rick-anderson
description: Obtenga información acerca de cómo hospedar una aplicación de ASP.NET Core en un servicio de Windows.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: 29f83ee585c73aeb57a09f70ea8e28650c05ce69
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2018
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Host principal de ASP.NET en un servicio de Windows

Por [Tom Dykstra](https://github.com/tdykstra)

La manera recomendada para hospedar una aplicación de ASP.NET Core en Windows sin usar IIS es ejecutarlo en un [servicio de Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Cuando se hospeda como un servicio de Windows, la aplicación puede automáticamente inicio después se reinicia y se bloquea sin necesidad de intervención humana.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample)). Para obtener instrucciones sobre cómo ejecutar la aplicación de ejemplo, vea el ejemplo *README.md* archivo.

## <a name="prerequisites"></a>Requisitos previos

* Debe ejecutar la aplicación en el tiempo de ejecución de .NET Framework. En el *.csproj* de archivos, especifique los valores adecuados para [TargetFramework](/nuget/schema/target-frameworks) y [RuntimeIdentifier](/dotnet/articles/core/rid-catalog). Por ejemplo:

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  Al crear un proyecto en Visual Studio, utilice el **aplicación de ASP.NET Core (.NET Framework)** plantilla.

* Si la aplicación recibe las solicitudes de Internet (no solo desde una red interna), debe utilizar el [HTTP.sys](xref:fundamentals/servers/httpsys) servidor web (anteriormente conocidos como [WebListener](xref:fundamentals/servers/weblistener) para las aplicaciones de ASP.NET Core 1.x) en lugar de [Kestrel](xref:fundamentals/servers/kestrel). IIS se recomienda para su uso como un servidor proxy inverso con Kestrel para implementaciones de borde. Para más información, vea [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy) (Cuándo se debe usar Kestrel con un proxy inverso).

## <a name="get-started"></a>Primeros pasos

En esta sección se explica los cambios mínimos necesarios para configurar un proyecto de ASP.NET Core existente para que se ejecute en un servicio.

1. Instale el paquete NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

2. Realice los cambios siguientes en `Program.Main`:

   * Llame a `host.RunAsService` en lugar de `host.Run`.

   * Si el código llama `UseContentRoot`, use una ruta de acceso a la ubicación de publicación en lugar de `Directory.GetCurrentDirectory()`.

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   ---

3. Publique la aplicación en una carpeta. Use [publicar dotnet](/dotnet/articles/core/tools/dotnet-publish) o un [perfil de publicación de Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) que publica en una carpeta.

4. Probar al crear e iniciar el servicio.

   Abra un shell de comandos del sistema con privilegios administrativos para usar el [sc.exe](https://technet.microsoft.com/library/bb490995) herramienta de línea de comandos para crear e iniciar un servicio. Si el servicio se denomina MyService, publica en `c:\svc`, y con el nombre AspNetCoreService, los comandos son:

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   El `binPath` valor es la ruta de acceso al archivo ejecutable de la aplicación, que incluye el nombre del archivo ejecutable.

   ![Ventana de la consola crear e iniciar el ejemplo](windows-service/_static/create-start.png)

   Cuando termine de estos comandos, vaya a la misma ruta que cuando se ejecuta como una aplicación de consola (de forma predeterminada, `http://localhost:5000`):

   ![Ejecución en un servicio](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a>Proporcionan una manera para que se ejecute fuera de un servicio

Es más fácil de probar y depurar cuando se ejecuta fuera de un servicio, por lo que es habitual para agregar código que llama a `RunAsService` únicamente bajo ciertas condiciones. Por ejemplo, puede ejecutar la aplicación como una aplicación de consola con una `--console` si el depurador se adjunta o argumento de línea de comandos:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

---

## <a name="handle-stopping-and-starting-events"></a>Controlar detener e iniciar los eventos

Para controlar `OnStarting`, `OnStarted`, y `OnStopping` eventos, realice los siguientes cambios adicionales:

1. Cree una clase que deriva de `WebHostService`:

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. Crear un método de extensión para `IWebHost` que pasa personalizado `WebHostService` a `ServiceBase.Run`:

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. En `Program.Main`, llamar al nuevo método de extensión, `RunAsCustomService`, en lugar de `RunAsService`:

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   ---

Si la opción de instalación `WebHostService` código requiere un servicio de inserción de dependencias (como un registrador), lo obtenga el `Services` propiedad de `IWebHost`:

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Escenarios de servidor proxy y equilibrador de carga

Servicios que interactúan con las solicitudes de Internet o de una red corporativa y están detrás de un servidor proxy o el equilibrador de carga pueden requerir una configuración adicional. Para más información, vea [Configurar ASP.NET Core para trabajar con servidores proxy y equilibradores de carga](xref:host-and-deploy/proxy-load-balancer).

## <a name="acknowledgments"></a>Agradecimientos

En este artículo se escribió con la Ayuda de orígenes publicadas:

* [Hospedaje de ASP.NET Core como servicio de Windows](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [Cómo hospedar el núcleo de ASP.NET en un servicio de Windows](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
