---
title: Usar JavaScriptServices para crear aplicaciones de una página de ASP.NET Core
author: scottaddie
description: Obtenga información acerca de las ventajas de usar JavaScriptServices para crear una aplicación de página única (SPA) respaldado por ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/spa-services
ms.openlocfilehash: fd893b7c62f38442bf5633a956786983763e6f9f
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2018
---
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a>Usar JavaScriptServices para crear aplicaciones de una página de ASP.NET Core

Por [Scott Addie](https://github.com/scottaddie) y [Fiyaz Hasan](http://fiyazhasan.me/)

Una aplicación de página única (SPA) es un tipo conocido de aplicación web debido a su experiencia de usuario enriquecida inherente. Integración de cliente marcos SPA o bibliotecas, como [Angular](https://angular.io/) o [React](https://facebook.github.io/react/), con un Framework de servidor como ASP.NET Core puede ser difícil. [JavaScriptServices](https://github.com/aspnet/JavaScriptServices) se desarrolló para reducir la fricción en el proceso de integración. Permite la operación sin problemas entre los distintos clientes y pilas de tecnología de servidor.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a>¿Qué es JavaScriptServices?

JavaScriptServices es un conjunto de tecnologías de cliente para ASP.NET Core. Su objetivo consiste en colocar ASP.NET Core como plataforma de servidor preferido de los desarrolladores para compilar SPAs.

JavaScriptServices consta de tres paquetes de NuGet distintos:
* [Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)
* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)
* [Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)

Estos paquetes son útiles si se:
* Ejecutar JavaScript en el servidor
* Usar un Framework de SPA o biblioteca
* Compilar recursos de lado cliente con Webpack

Gran parte de la atención en este artículo se coloca sobre cómo usar el paquete SpaServices.

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a>¿Qué es SpaServices?

Se creó para colocar ASP.NET Core como plataforma de servidor preferido de los desarrolladores para compilar SPAs SpaServices. SpaServices no es necesario desarrollar SPAs con ASP.NET Core, y no bloquea en un Framework de cliente en particular.

SpaServices ofrece una infraestructura útil como:
* [Procesamiento previo de servidor](#server-prerendering)
* [Middleware de desarrollo Webpack](#webpack-dev-middleware)
* [Sustitución del módulo de acceso rápido](#hot-module-replacement)
* [Aplicaciones auxiliares de enrutamientos](#routing-helpers)

Colectivamente, estos componentes de infraestructura mejoran el flujo de trabajo de desarrollo y la experiencia en tiempo de ejecución. Los componentes pueden adoptar individualmente.

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a>Requisitos previos para usar SpaServices

Para trabajar con SpaServices, instalar lo siguiente:
* [Node.js](https://nodejs.org/) (versión 6 o posterior) con npm
  * Para comprobar que estos componentes se instalan y se encuentra, ejecute lo siguiente desde la línea de comandos:

    ```console
    node -v && npm -v
    ```

Nota: Si va a implementar en un sitio web de Azure, no es necesario hacer nada aquí &mdash; Node.js está instalada y disponible en los entornos de servidor.

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * Si está en Windows con Visual Studio de 2017, está instalado el SDK seleccionando la **.NET Core el desarrollo multiplataforma** carga de trabajo.

* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) paquete de NuGet

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a>Procesamiento previo de servidor

Una aplicación universal (también conocido como mismo formato) es una aplicación de JavaScript capaz de ejecutar tanto en el servidor y el cliente. Angular, React y otros entornos populares proporciona una plataforma universal de este estilo de desarrollo de aplicaciones. La idea es representar primero los componentes de Framework en el servidor a través de Node.js y, a continuación, más delegar la ejecución al cliente.

ASP.NET Core [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro) proporcionada por SpaServices simplifica la implementación de procesamiento previo de servidor mediante la invocación de las funciones de JavaScript en el servidor.

### <a name="prerequisites"></a>Requisitos previos

Instale el software siguiente:
* [ASPNET-procesamiento previo](https://www.npmjs.com/package/aspnet-prerendering) npm paquete:

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a>Configuración

Las aplicaciones auxiliares de etiquetas se hacen reconocibles a través de registro del espacio de nombres en el proyecto *_ViewImports.cshtml* archivo:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

Estas aplicaciones auxiliares de etiquetas abstraer los detalles de comunicarse directamente con las API de bajo nivel mediante el aprovechamiento de una sintaxis similar a HTML dentro de la vista Razor:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a>El `asp-prerender-module` auxiliar de etiquetas

El `asp-prerender-module` se ejecuta la aplicación auxiliar de etiquetas, usado en el ejemplo de código anterior, *ClientApp/dist/main-server.js* en el servidor a través de Node.js. Para no complicarlo, *main server.js* archivo es un artefacto de la tarea de transpilation TypeScript y JavaScript en el [Webpack](http://webpack.github.io/) proceso de compilación. Webpack define un alias de punto de entrada de `main-server`; y cruce seguro del gráfico de dependencia para este alias comienza en el *ClientApp/arranque-server.ts* archivo:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

En el siguiente ejemplo Angular, el *ClientApp/arranque-server.ts* archivo utiliza el `createServerRenderer` función y `RenderResult` el tipo de la `aspnet-prerendering` paquete de npm para configurar la representación del servidor a través de Node.js. El marcado HTML destinado para la representación del lado servidor se pasa a una llamada de función de la resolución, que se incluye en JavaScript fuertemente tipado `Promise` objeto. El `Promise` es de importancia del objeto que proporciona asincrónicamente el marcado HTML a la página de inyección de código en el elemento del DOM al marcador de posición.

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a>El `asp-prerender-data` auxiliar de etiquetas

Cuando se acopla con el `asp-prerender-module` aplicación auxiliar de etiqueta, la `asp-prerender-data` etiqueta auxiliar puede utilizarse para pasar información contextual de la vista de Razor en el código de JavaScript del lado servidor. Por ejemplo, el siguiente marcado pasa los datos de usuario en el `main-server` módulo:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

Los datos recibidos `UserName` argumento se serializa utilizando el serializador JSON integrado y se almacena en la `params.data` objeto. En el siguiente ejemplo Angular, los datos se utilizan para construir un saludo personalizado dentro de un `h1` elemento:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

Nota: Los nombres de propiedad pasados en aplicaciones auxiliares de etiquetas se representan con **PascalCase** notación. Esto contrasta con JavaScript, donde los mismos nombres de propiedad se representan con **camelCase**. La configuración predeterminada de la serialización de JSON es responsable de esta diferencia.

Para ampliar el ejemplo de código anterior, datos pueden pasarse desde el servidor a la vista por hidratación el `globals` propiedad proporcionada para el `resolve` función:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

El `postList` matriz definida dentro de la `globals` objeto se adjunta en el explorador global `window` objeto. Esta activación variables en ámbito global elimina la duplicación de esfuerzos, especialmente ya que pertenece a la carga de los mismos datos una vez en el servidor y de nuevo en el cliente.

![variable postList global adjuntado al objeto de ventana](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a>Middleware de desarrollo Webpack

[Middleware de desarrollo Webpack](https://webpack.github.io/docs/webpack-dev-middleware.html) incorpora un flujo de trabajo de desarrollo mejorado mediante el cual Webpack crea recursos a petición. El middleware automáticamente compila y sirve de recursos del cliente cuando se vuelve a cargar una página en el explorador. El método alternativo consiste en invocar manualmente Webpack a través de script de compilación del proyecto npm cuando cambia una dependencia de otro fabricante o el código personalizado. Un npm Generar script en el *package.json* archivo se muestra en el ejemplo siguiente:

[!code-json[](../client-side/spa-services/sample/SpaServicesSampleApp/package.json?range=5)]

### <a name="prerequisites"></a>Requisitos previos

Instale el software siguiente:
* [ASPNET webpack](https://www.npmjs.com/package/aspnet-webpack) npm paquete:

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a>Configuración

Middleware de desarrollo Webpack está registrado en la canalización de solicitudes HTTP mediante el siguiente código en el *Startup.cs* del archivo `Configure` método:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

El `UseWebpackDevMiddleware` método de extensión se debe llamar antes [registrar archivos estáticos hospedaje](xref:fundamentals/static-files) a través de la `UseStaticFiles` método de extensión. Por motivos de seguridad, registre el middleware solo cuando la aplicación se ejecuta en modo de desarrollo.

El *webpack.config.js* del archivo `output.publicPath` propiedad indica el middleware para observar el `dist` carpeta cambios:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a>Sustitución del módulo de acceso rápido

Piense del Webpack [activa sustitución del módulo](https://webpack.js.org/concepts/hot-module-replacement/) característica (HMR) como una evolución del [Webpack Dev Middleware](#webpack-dev-middleware). HMR incorpora las mismas ventajas, pero optimiza aún más el flujo de trabajo de desarrollo al actualizar automáticamente el contenido de página después de compilar los cambios. No se debe confundir con una actualización del explorador, lo que interferiría con el estado actual de en memoria y la sesión de depuración de la aplicación SPA. Hay un vínculo directo entre el servicio de Middleware de desarrollo de Webpack y el explorador, lo que significa que los cambios se insertan en el explorador.

### <a name="prerequisites"></a>Requisitos previos

Instale el software siguiente:
* [middleware caliente webpack](https://www.npmjs.com/package/webpack-hot-middleware) npm paquete:

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a>Configuración

El componente HMR debe estar registrado en la canalización de solicitudes HTTP de MVC en el `Configure` método:

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

Al igual que ocurría con [Webpack Dev Middleware](#webpack-dev-middleware), `UseWebpackDevMiddleware` método de extensión se debe llamar antes el `UseStaticFiles` método de extensión. Por motivos de seguridad, registre el middleware solo cuando la aplicación se ejecuta en modo de desarrollo.

El *webpack.config.js* archivo debe definir una `plugins` de las matrices, incluso si se deja vacía:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

Después de cargar la aplicación en el explorador, pestaña de la consola de las herramientas de desarrollo proporciona confirmación de activación de HMR:

![Mensaje de conectado activa de sustitución del módulo](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a>Aplicaciones auxiliares de enrutamientos

En la mayoría SPAs basada en núcleo de ASP.NET, es conveniente enrutamiento de cliente además del enrutamiento del servidor. Los sistemas de enrutamiento SPA y MVC pueden trabajar independientemente sin interferencias. Hay, sin embargo, un borde mayúsculas supone desafíos: identificación de respuestas HTTP 404.

Considere el escenario en el que una ruta sin extensión de `/some/page` se utiliza. Se supone la solicitud no hacer coincidir el patrón una ruta de servidor, pero su patrón coincide con una ruta de cliente. Ahora considere una solicitud entrante para `/images/user-512.png`, por lo general que espera encontrar un archivo de imagen en el servidor. Si esa ruta de acceso del recurso solicitado no coincide con ninguna ruta de servidor o un archivo estático, no es probable que la aplicación de cliente podría controlarla, por lo general desea devolver un código de estado HTTP 404.

### <a name="prerequisites"></a>Requisitos previos

Instale el software siguiente:
* El paquete de cliente de enrutamiento npm. Utilizando Angular como ejemplo:

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a>Configuración

Un método de extensión denominado `MapSpaFallbackRoute` se utiliza en el `Configure` método:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

Sugerencia: Las rutas se evalúan en el orden en el que está configurados. Por lo tanto, la `default` ruta en el ejemplo de código anterior se utiliza en primer lugar para la coincidencia.

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a>Crear un nuevo proyecto

JavaScriptServices proporciona plantillas de aplicaciones configuradas previamente. SpaServices se utiliza en estas plantillas, junto con una diversidad de marcos y bibliotecas como Angular, React y Redux.

Estas plantillas se pueden instalar a través de la CLI de núcleo de .NET con el comando siguiente:

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

Se muestra una lista de plantillas SPA disponibles:

| Plantillas                                 | Nombre corto | Lenguaje | Etiquetas        |
|:------------------------------------------|:-----------|:---------|:------------|
| Núcleo de ASP.NET de MVC con Angular             | angular    | [C#]     | MVC/Web/SPA |
| Núcleo de ASP.NET de MVC con React.js            | react      | [C#]     | MVC/Web/SPA |
| Núcleo de ASP.NET MVC con React.js y reducción  | reactredux | [C#]     | MVC/Web/SPA |

Para crear un nuevo proyecto con una de las plantillas SPA, incluya el **nombre corto** de la plantilla en el [dotnet nueva](/dotnet/core/tools/dotnet-new) comando. El siguiente comando crea una aplicación Angular con ASP.NET MVC de núcleo configurado para el lado del servidor:

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a>Establecer el modo de configuración en tiempo de ejecución

Existen dos modos de configuración en tiempo de ejecución principal:
* **Desarrollo de**:
    * Incluye asignaciones de origen para facilitar la depuración.
    * No optimice el código del lado cliente para el rendimiento.
* **Producción**:
    * Excluye los mapas de código fuente.
    * Optimiza el código de cliente a través de agrupación y minificación.

ASP.NET Core utiliza una variable de entorno denominada `ASPNETCORE_ENVIRONMENT` para almacenar el modo de configuración. Vea **[establecer el entorno de](xref:fundamentals/environments#setting-the-environment)** para obtener más información.

### <a name="running-with-net-core-cli"></a>Ejecutar con .NET Core CLI

Restaurar el NuGet necesario y los paquetes de npm ejecutando el siguiente comando en la raíz del proyecto:

```console
dotnet restore && npm i
```

Compilar y ejecutar la aplicación:

```console
dotnet run
```

Inicia la aplicación en localhost según la [el modo de configuración en tiempo de ejecución](#runtime-config-mode). Vaya a `http://localhost:5000` en el explorador muestra la página de aterrizaje.

### <a name="running-with-visual-studio-2017"></a>Ejecutar con Visual Studio de 2017

Abra la *.csproj* archivo generado por la [dotnet nueva](/dotnet/core/tools/dotnet-new) comando. Los paquetes de NuGet y npm necesarios se restauran automáticamente al proyecto abierto. Este proceso de restauración puede tardar unos minutos y está lista para ejecutarse cuando finaliza la aplicación. Haga clic en el botón verde de ejecución o presione `Ctrl + F5`, y el explorador se abre en la página de inicio de la aplicación. La aplicación se ejecuta en localhost según la [el modo de configuración en tiempo de ejecución](#runtime-config-mode). 

<a name="app-testing"></a>

## <a name="testing-the-app"></a>Probar la aplicación

Plantillas de SpaServices están preconfiguradas para ejecutar pruebas de cliente mediante [Karma](https://karma-runner.github.io/1.0/index.html) y [Jasmine](https://jasmine.github.io/). Jasmine es una unidad popular marco de pruebas para JavaScript, mientras que Karma es un ejecutor de pruebas para las pruebas. Karma está configurado para trabajar con la [Webpack Dev Middleware](#webpack-dev-middleware) tal que el programador no es necesario detener y ejecutar la prueba cada vez que se realizan cambios. Si es el código que se ejecuta en el caso de prueba o el propio caso de prueba, la prueba se ejecuta automáticamente.

Usar la aplicación Angular como ejemplo, dos casos de prueba criterio Comidos ya se proporcionan para que la `CounterComponent` en el *counter.component.spec.ts* archivo:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

Abra el símbolo del sistema en el *ClientApp* directory. Ejecute el siguiente comando:

```console
npm test
```

La secuencia de comandos inicia el ejecutor de pruebas Karma, que lee la configuración definida en el *karma.conf.js* archivo. Entre otras opciones, el *karma.conf.js* identifica los archivos de prueba para ejecutarse a través de su `files` matriz:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a>Publicar la aplicación

Combinación de los activos de cliente generados y los artefactos de ASP.NET Core publicados en un paquete listo para implementar puede resultar complicada. Por suerte, SpaServices orquesta ese proceso de publicación completa con un destino de MSBuild personalizado denominado `RunWebpack`:

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

El destino de MSBuild tiene las siguientes responsabilidades:
1. Restaure los paquetes npm
1. Cree una versión de producción automatizado de los activos de terceros, de cliente
1. Cree una versión de producción automatizado de los activos de cliente personalizados
1. Copiar los activos Webpack generados en la carpeta de publicación

El destino de MSBuild se invoca cuando se ejecuta:

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a>Recursos adicionales

* [Documentos angulares](https://angular.io/docs)
