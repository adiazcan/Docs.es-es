---
title: Activación de middleware basada en Factory en ASP.NET Core
author: guardrex
description: Aprenda a usar middleware fuertemente tipado con la implementación de una activación basada en Factory en ASP.NET Core.
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 01/29/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 76ba257abfb11e0c2950b974f837c6ae5818a6a1
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a>Activación de middleware basada en Factory en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) es un punto de extensibilidad para la activación de [middleware](xref:fundamentals/middleware/index).

Los métodos de extensión `UseMiddleware` comprueban si un tipo registrado de middleware implementa `IMiddleware`. Si es así, la instancia `IMiddlewareFactory` registrada en el contenedor se usa para resolver la implementación `IMiddleware` en lugar de usar la lógica de activación de middleware basado en convenciones. El middleware se registra como un servicio con ámbito o transitorio en el contenedor de servicios de la aplicación.

Ventajas:

* Activación a petición (inyección de servicios con ámbito)
* Tipado fuerte de middleware

`IMiddleware` se activa a petición, por lo que los servicios se pueden insertar en el constructor del middleware.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

La aplicación de ejemplo muestra middleware activado por:

* Convención. Para obtener más información sobre la activación de middleware convencional, consulte el tema [Middleware](xref:fundamentals/middleware/index).
* Una implementación de [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware). La [clase MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) predeterminada activa el middleware.

Las implementaciones de middleware funcionan de forma idéntica y registran el valor proporcionado por un parámetro de cadena de consulta (`key`). El middleware usa un contexto de base de datos insertado (un servicio con ámbito) para registrar el valor de cadena de consulta en una base de datos en memoria.

## <a name="imiddleware"></a>IMiddleware

[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) define el middleware para la canalización de solicitudes de la aplicación. El método [InvokeAsync(HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) controla las solicitudes y devuelve una `Task` que representa la ejecución del middleware.

Middleware activado por convención:

[!code-csharp[](extensibility/sample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

Middleware activado por `MiddlewareFactory`:

[!code-csharp[](extensibility/sample/Middleware/IMiddlewareMiddleware.cs?name=snippet1)]

Las extensiones se crean para los middlewares:

[!code-csharp[](extensibility/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

No se pueden pasar objetos al middleware activado por Factory con `UseMiddleware`:

```csharp
public static IApplicationBuilder UseIMiddlewareMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<IMiddlewareMiddleware>(option);
}
```

El middleware activado por Factory se agrega al contenedor integrado en *Startup.cs*:

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet1&highlight=6)]

Ambos middlewares se registran en la canalización de procesamiento de solicitudes en `Configure`:

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet2&highlight=12-13)]

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) proporciona métodos para crear middleware. La implementación de Middleware Factory se registra en el contenedor como un servicio con ámbito.

La implementación `IMiddlewareFactory` predeterminada, [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), se encuentra en el paquete [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) ([fuente de referencia](https://github.com/aspnet/HttpAbstractions/blob/release/2.0/src/Microsoft.AspNetCore.Http/MiddlewareFactory.cs)).

## <a name="additional-resources"></a>Recursos adicionales

* [Middleware](xref:fundamentals/middleware/index)
* [Activación de middleware con un contenedor de terceros](xref:fundamentals/middleware/extensibility-third-party-container)
