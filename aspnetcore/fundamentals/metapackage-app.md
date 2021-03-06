---
title: Metapaquete Microsoft.AspNetCore.App para ASP.NET Core 2.1 y versiones posteriores
author: Rick-Anderson
description: El metapaquete Microsoft.AspNetCore.App incluye todos los paquetes de ASP.NET Core y Entity Framework Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage-app
ms.openlocfilehash: 7c7f69a6176d3f7982a67106cb823ff42200b50e
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/18/2018
---
# <a name="microsoftaspnetcoreapp-metapackage-for-aspnet-core-21"></a>Metapaquete Microsoft.AspNetCore.App para ASP.NET Core 2.1

Esta característica requiere que ASP.NET Core 2.1 y versiones posteriores tengan como destino .NET Core 2.1 y versiones posteriores.

El [metapaquete](/dotnet/core/packages#metapackages) [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) para ASP.NET Core:

* No incluye dependencias de terceros excepto [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/), [Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/) e [IX-Async](https://www.nuget.org/packages/System.Interactive.Async/). Estas dependencias de terceros se consideran necesarias para garantizar el funcionamiento de las características de los principales marcos.
* Incluye todos los paquetes admitidos por el equipo de ASP.NET Core, excepto aquellos que contienen dependencias de terceros (distintos de los mencionados anteriormente).
* Incluye todos los paquetes admitidos por el equipo de Entity Framework Core, excepto aquellos que contienen dependencias de terceros (distintos de los mencionados anteriormente).

Todas las características de ASP.NET Core 2.1 y versiones posteriores, así como de Entity Framework Core 2.1 y versiones posteriores, están incluidas en el paquete `Microsoft.AspNetCore.App`. Las plantillas de proyecto predeterminada destinadas a ASP.NET 2.1 y versiones posteriores usan este paquete. Se recomienda que las aplicaciones que tengan como destino ASP.NET Core 2.1 y versiones posteriores, así como Entity Framework Core 2.1 y versiones posteriores, usen el paquete `Microsoft.AspNetCore.App`.

El número de versión del metapaquete `Microsoft.AspNetCore.App` representa la versión de ASP.NET Core y la versión de Entity Framework Core.

Mediante el metapaquete `Microsoft.AspNetCore.App` se proporcionan restricciones de versión que protegen la aplicación:

* Si se incluye un paquete que tiene una dependencia transitiva (no directa) en un paquete en `Microsoft.AspNetCore.App` y los números de versión son distintos, NuGet generará un error.
* Los demás paquetes agregados a la aplicación no pueden cambiar la versión de los paquetes que se incluyen en `Microsoft.AspNetCore.App`.
* La coherencia de versiones garantiza una experiencia fiable. `Microsoft.AspNetCore.App` se ha diseñado para evitar las combinaciones de versiones no probadas de bits relacionados que se usan conjuntamente en la misma aplicación.

Las aplicaciones que usan el metapaquete `Microsoft.AspNetCore.App` pueden aprovechar automáticamente el marco de uso compartido de ASP.NET Core. Al usar el metapaquete `Microsoft.AspNetCore.App`, **no** se implementa ningún recurso de los paquetes NuGet de ASP.NET Core a los que se hace referencia con la aplicación, porque el marco de uso compartido de ASP.NET Core ya contiene esos recursos. Los recursos del marco de uso compartido se precompilan para mejorar el tiempo de inicio de la aplicación. Para más información, vea "Marco de uso compartido" en [Empaquetado de distribución de .NET Core](/dotnet/core/build/distribution-packaging).

El siguiente archivo de proyecto *.csproj* hace referencia al metapaquete `Microsoft.AspNetCore.App` de ASP.NET Core:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>

```

El marcado anterior representa una plantilla típica de ASP.NET Core 2.1 y versiones posteriores. No especifica ningún número de versión para la referencia del paquete `Microsoft.AspNetCore.App`. Si no se especifica la versión, el SDK define una versión [implícita](https://github.com/dotnet/core/blob/master/release-notes/1.0/sdk/1.0-rc3-implicit-package-refs.md), es decir, `Microsoft.NET.Sdk.Web`. Es recomendable confiar en la versión implícita especificada por el SDK y no establecer de forma explícita el número de versión en la referencia del paquete. Puede dejar un comentario de GitHub en [Discussion for the Microsoft.AspNetCore.App implicit version](https://github.com/aspnet/Docs/issues/6430) (Debate sobre la versión implícita Microsoft.AspNetCore.App).

La versión implícita se establece en `major.minor.0` para las aplicaciones portátiles. El mecanismo de puesta al día del marco de uso compartido ejecutará la aplicación en la versión más reciente compatible entre los marcos de uso compartidos instalados. Para garantizar que se use la misma versión en el desarrollo, las pruebas y la producción, asegúrese de que en todos los entornos esté instalada la misma versión del marco de uso compartido. Para las aplicaciones autocontenidas, el número de versión implícita se establece en el valor `major.minor.patch` del marco de uso compartido incluido en el SDK instalado.

El hecho de especificar un número de versión en la referencia de `Microsoft.AspNetCore.App` **no** garantiza que se vaya a elegir la versión del marco de uso compartido. Por ejemplo, suponga que se especifica la versión "2.1.1", pero está instalada la "2.1.3". En ese caso, la aplicación usará el valor "2.1.3". Aunque no se recomienda, puede deshabilitar la puesta al día (revisión o secundaria). Para obtener más información sobre la puesta al día del host de dotnet y cómo configurar su comportamiento, vea [Dotnet host roll forward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md) (Puesta al día del host de dotnet).

El [metapaquete](/dotnet/core/packages#metapackages) `Microsoft.AspNetCore.App` no es un paquete habitual que se actualice desde NuGet. De forma similar a `Microsoft.NETCore.App`, `Microsoft.AspNetCore.App` representa un tiempo de ejecución compartido, con una semántica especial de control de versiones controlada de forma ajena a NuGet. Para obtener más información, vea [Paquetes, metapaquetes y marcos de trabajo](/dotnet/core/packages).

Si la aplicación ha usado `Microsoft.AspNetCore.All` anteriormente, consulte [Migración desde Microsoft.AspNetCore.All a Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate).
