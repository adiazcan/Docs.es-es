---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: Crear una restricción de ruta (VB) | Documentos de Microsoft
author: StephenWalther
description: En este tutorial, Stephen Walther se muestra cómo se puede controlar cómo el explorador solicita coincidencia rutas mediante la creación de restricciones de la ruta con expresiones regulares.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 2f50b371ac679218b06c4848e6d33516d29d3a82
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-route-constraint-vb"></a>Crear una restricción de ruta (VB)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> En este tutorial, Stephen Walther se muestra cómo se puede controlar cómo el explorador solicita coincidencia rutas mediante la creación de restricciones de la ruta con expresiones regulares.


Utilizar restricciones de la ruta para restringir las solicitudes del explorador que coinciden con una ruta determinada. Puede usar una expresión regular para especificar una restricción de ruta.

Por ejemplo, imagine que ha definido la ruta en el listado 1 en el archivo Global.asax.

**Lista 1 - Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

Lista 1 contiene una ruta con el nombre de producto. Puede usar la ruta de producto para asignar las solicitudes del explorador a la ProductController contenido en el listado 2.

**La lista 2 - Controllers\ProductController.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

Observe que la acción de Details() expuesta por el controlador del producto acepta un solo parámetro denominado productId. Este parámetro es un parámetro de número entero.

La ruta definida en la lista 1 coincide con cualquiera de las direcciones URL siguientes:

- /Product/23
- / / De producto 7

Desafortunadamente, la ruta también coincidirá con las direcciones URL siguientes:

- / Productos/bLa, bla
- /Product/apple

Dado que la acción Details() espera un parámetro de número entero, que realiza una solicitud que contiene un valor distinto de un valor entero se producirá un error. Por ejemplo, si escribe la dirección URL /Product/apple en el explorador, a continuación, obtendrá la página de error en la figura 1.


[![El cuadro de diálogo nuevo proyecto](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)

**Figura 01**: ver una página seccionar ([haga clic aquí para ver la imagen a tamaño completo](creating-a-route-constraint-vb/_static/image2.png))


¿Qué desea hacer en realidad es solo coincidirá con direcciones URL que contienen un productId entero adecuado. Puede usar una restricción al definir una ruta para restringir las direcciones URL que coinciden con la ruta. La ruta de producto modificada en el listado 3 contiene una restricción de expresión regular que coincida solo con enteros.

**Listing 3 - Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

La expresión regular \d+ coincide con uno o más números enteros. Esta restricción hace que la ruta de producto para que coincida con las direcciones URL siguientes:

- / Productos/3
- /Product/8999

Pero no las direcciones URL siguientes:

- /Product/apple
- / Producto

Estas solicitudes de explorador será procesadas por otra ruta o, si no hay ninguna ruta coincidente, un *no se pudo encontrar el recurso* se devolverá el error.

> [!div class="step-by-step"]
> [Anterior](creating-custom-routes-vb.md)
> [Siguiente](creating-a-custom-route-constraint-vb.md)
