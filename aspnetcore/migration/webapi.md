---
title: Migrar de API Web de ASP.NET a ASP.NET Core
author: ardalis
description: Obtenga información acerca de cómo migrar una implementación de la API Web de ASP.NET Web API para MVC de ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/webapi
ms.openlocfilehash: 8d842877e49e317323d453e71ebb3302245f388d
ms.sourcegitcommit: 3d071fabaf90e32906df97b08a8d00e602db25c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/10/2018
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>Migrar de API Web de ASP.NET a ASP.NET Core

Por [Steve Smith](https://ardalis.com/) y [Scott Addie](https://scottaddie.com)

Las API Web son servicios HTTP que llegan a una amplia gama de clientes, incluidos los exploradores y dispositivos móviles. Núcleo de ASP.NET MVC incluye compatibilidad para la creación de las API Web proporcionan una manera única y coherente de la creación de aplicaciones web. En este artículo, se muestran los pasos necesarios para migrar una implementación de la API Web de ASP.NET Web API para MVC de ASP.NET Core.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="review-aspnet-web-api-project"></a>Proyecto de revisión ASP.NET Web API

Este artículo utiliza el proyecto de ejemplo, *ProductsApp*, creado en el artículo [Introducción a ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) como punto de partida. En el proyecto, un proyecto de ASP.NET Web API simple se configura como se indica a continuación.

En *Global.asax.cs*, se realiza una llamada a `WebApiConfig.Register`:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig` se define en *App_Start*, y tiene solo un estático `Register` método:

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


Esta clase configura [atributo enrutamiento](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), aunque realmente no se usa en el proyecto. También configura la tabla de enrutamiento, que se usa por la API Web de ASP.NET. En este caso, ASP.NET Web API esperará direcciones URL para que coincida con el formato */api/ {controller} / {id}*, con *{id}* que es opcional.

El *ProductsApp* proyecto incluye un solo controlador simple, que hereda de `ApiController` y expone dos métodos:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

Por último, el modelo, *producto*, utilizada por el *ProductsApp*, es una clase simple:

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

Ahora que tenemos un proyecto simple desde el que se va a iniciar, podemos demostrar cómo migrar este proyecto de API Web MVC de ASP.NET Core.

## <a name="create-the-destination-project"></a>Crear el proyecto de destino

Con Visual Studio, cree una solución nueva y vacía y asígnele el nombre *WebAPIMigration*. Agregar existente *ProductsApp* proyecto al, a continuación, agregue un nuevo proyecto de aplicación de ASP.NET Core Web a la solución. Asigne al nuevo proyecto *ProductsCore*.

![Abrir el cuadro de diálogo nuevo proyecto en las plantillas Web](webapi/_static/add-web-project.png)

A continuación, elija la plantilla de proyecto de API Web. Se migrará el *ProductsApp* contenido para este nuevo proyecto.

![Cuadro de diálogo nueva aplicación Web con la plantilla de proyecto de API Web seleccionado en la lista de plantillas de ASP.NET Core](webapi/_static/aspnet-5-webapi.png)

Eliminar el `Project_Readme.html` archivo desde el nuevo proyecto. La solución debe tener el siguiente aspecto:

![Solución de aplicación abierta en el Explorador de soluciones con archivos y carpetas de los proyectos ProductsApp y ProductsCore](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a>Migrar la configuración

Ya no usa ASP.NET Core *Global.asax*, *web.config*, o *App_Start* carpetas. En su lugar, se realizan todas las tareas de inicio en *Startup.cs* en la raíz del proyecto (vea [inicio de la aplicación](../fundamentals/startup.md)). En ASP.NET MVC de núcleo, el enrutamiento basado en atributos ahora se incluye de forma predeterminada cuando `UseMvc()` se denomina; y, es el enfoque recomendado para configurar las rutas de Web API (y es la forma en que el proyecto de inicio de la API Web controla el enrutamiento).

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

Suponiendo que desea utilizar el enrutamiento de atributo en el proyecto en el futuro, se necesita ninguna configuración adicional. Basta con aplicar los atributos según sea necesario para los controladores y acciones, como se hace en el ejemplo `ValuesController` clase que se incluye en el proyecto de inicio de la API Web:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

Tenga en cuenta la presencia de *[controlador]* en la línea 8. Enrutamiento basado en atributos ahora es compatible con determinados símbolos (tokens), como *[controlador]* y *[acción]*. Estos tokens se reemplazan en tiempo de ejecución con el nombre del controlador o acción, respectivamente, al que se ha aplicado el atributo. Esto sirve para reducir el número de cadenas mágicos en el proyecto y garantiza que las rutas se mantendrá sincronizadas con sus controladores correspondientes y las acciones cuando se aplican refactorizaciones rename automática.

Para migrar el controlador de API de productos, debemos primero copiaremos *ProductsController* al nuevo proyecto. A continuación, basta con incluir el atributo de ruta en el controlador:

```csharp
[Route("api/[controller]")]
```

También debe agregar el `[HttpGet]` atribuir a los dos métodos, ya que se deberían llamar a través de HTTP Get. Incluir la expectativa de un parámetro "id" en el atributo para `GetProduct()`:

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

En este momento, el enrutamiento está configurado correctamente; Sin embargo, aún no podemos probarlo. Se deben realizar cambios adicionales antes de *ProductsController* se compilará.

## <a name="migrate-models-and-controllers"></a>Migrar los modelos y controladores

El último paso del proceso de migración para este proyecto de API Web simple es copiar a través de los controladores y los modelos que se usan. En este caso, basta con copiar *Controllers/ProductsController.cs* desde el proyecto original al nuevo. A continuación, copie toda la carpeta modelos desde el proyecto original al nuevo. Ajustar los espacios de nombres para que coincida con el nuevo nombre de proyecto (*ProductsCore*).  En este momento, puede compilar la aplicación y encontrará un número de errores de compilación. Por lo general, estos deben estar en las siguientes categorías:

* *ApiController* no existe

* *System.Web.Http* no existe el espacio de nombres

* *IHttpActionResult* no existe

Afortunadamente, estos son muy fáciles corregir:

* Cambio *ApiController* a *controlador* (puede que necesite agregar *con Microsoft.AspNetCore.Mvc*)

* Elimine cualquiera con la instrucción que hace referencia a *System.Web.Http*

* Cambiar cualquier método devolver *IHttpActionResult* para devolver un *IActionResult*

Una vez que estos cambios se han realizado y que no use instrucciones using quita, el migrados *ProductsController* clase tiene el siguiente aspecto:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

Ahora puede ejecutar el proyecto migrado y vaya a */api/productos*; y, debería ver la lista completa de los 3 productos. Vaya a */api/products/1* y debería ver la primera el producto.

## <a name="microsoftaspnetcoremvcwebapicompatshim"></a>Microsoft.AspNetCore.Mvc.WebApiCompatShim

Es una herramienta muy útil cuando ASP.NET Web API migrar proyectos a ASP.NET Core el [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) biblioteca. La corrección de compatibilidad extiende ASP.NET Core para permitir a un número de distintas convenciones de Web API 2 para usarse. El ejemplo portar anteriormente en este documento es bastante básico, por lo que la corrección de compatibilidad no era necesaria. Para proyectos grandes, usando la corrección de compatibilidad puede ser útil para temporalmente tender API entre ASP.NET Core y ASP.NET Web API 2.

La corrección de compatibilidad de API Web está pensada para usarse como una medida temporal para facilitar la migración de grandes proyectos API Web a ASP.NET Core. Con el tiempo, los proyectos deben actualizarse para usar patrones principales de ASP.NET en lugar de depender de la corrección de compatibilidad. 

Entre las características de compatibilidad que se incluyen en Microsoft.AspNetCore.Mvc.WebApiCompatShim se incluyen:

* Agrega un `ApiController` tipo para que los tipos de base de controladores no tienen que actualizarse.
* Habilita el enlace de modelo de estilo Web API. Funciones de enlace del modelo MVC ASP.NET Core de forma similar a MVC 5, de forma predeterminada. Los cambios de la corrección de compatibilidad del modelo enlace para que sea más similar a las convenciones de enlace de API Web 2 modelo. Por ejemplo, los tipos complejos se enlazan automáticamente desde el cuerpo de solicitud.
* Extiende el enlace de modelos para que las acciones de controlador pueden tomar parámetros de tipo `HttpRequestMessage`.
* Agrega los formateadores de mensajes que permite a las acciones para devolver los resultados de tipo `HttpResponseMessage`.
* Agrega los métodos de respuesta adicionales que pueden usado para atender las respuestas Web API 2 acciones:
    * Generadores de HttpResponseMessage:
        * `CreateResponse<T>`
        * `CreateErrorResponse`
    * Métodos de resultado de acción:
        * `BadResuestErrorMessageResult`
        * `ExceptionResult`
        * `InternalServerErrorResult`
        * `InvalidModelStateResult`
        * `NegotiatedContentResult`
        * `ResponseMessageResult`
* Agrega una instancia de `IContentNegotiator` al contenedor de la aplicación DI y hace contenido tipos relacionados con la negociación de [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) disponibles. Esto incluye tipos como `DefaultContentNegotiator`, `MediaTypeFormatter`, etcetera.

Para usar la corrección de compatibilidad, debe:

* Referencia de la [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) paquete NuGet.
* Registrar los servicios de la corrección de compatibilidad con el contenedor de la aplicación DI mediante una llamada a `services.AddWebApiConventions()` en la aplicación `Startup.ConfigureServices` método.
* Definir las rutas de Web API específica mediante `MapWebApiRoute` en el `IRouteBuilder` en la aplicación `IApplicationBuilder.UseMvc` llamar.

## <a name="summary"></a>Resumen

Migrar un proyecto de ASP.NET Web API simple para MVC de ASP.NET Core es bastante sencilla, gracias a la compatibilidad integrada con las API Web de MVC de ASP.NET Core. La principal que deben migrar todos los proyectos de ASP.NET Web API son rutas, los controladores y los modelos, junto con las actualizaciones de los tipos utilizados por los controladores y acciones.
