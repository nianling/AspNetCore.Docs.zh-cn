---
title: ASP.NET Core 中的简单授权
author: rick-anderson
description: 了解如何使用授权属性限制对 ASP.NET Core 控制器和操作的访问。
ms.author: riande
ms.date: 10/14/2016
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authorization/simple
ms.openlocfilehash: 6bd83473e168ba9100d4f6041d5d71139762b46c
ms.sourcegitcommit: fa89d6553378529ae86b388689ac2c6f38281bb9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2020
ms.locfileid: "86060106"
---
# <a name="simple-authorization-in-aspnet-core"></a>ASP.NET Core 中的简单授权

<a name="security-authorization-simple"></a>

ASP.NET Core 中的授权通过 <xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute> 和其各种参数来控制。 在最简单的形式中，将 `[Authorize]` 属性应用于控制器、操作或 Razor 页面，将对该组件的访问限制为任何经过身份验证的用户。

例如，以下代码将访问权限限制为 `AccountController` 任何经过身份验证的用户。

```csharp
[Authorize]
public class AccountController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

如果要对操作（而不是控制器）应用授权，请将属性应用于 `AuthorizeAttribute` 操作本身：

```csharp
public class AccountController : Controller
{
   public ActionResult Login()
   {
   }

   [Authorize]
   public ActionResult Logout()
   {
   }
}
```

现在只有经过身份验证的用户可以访问该 `Logout` 函数。

你还可以使用属性，以 `AllowAnonymous` 允许未通过身份验证的用户访问各个操作。 例如：

```csharp
[Authorize]
public class AccountController : Controller
{
    [AllowAnonymous]
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

这将仅允许经过身份验证的用户 `AccountController` 访问，但 `Login` 操作除外，无论用户是否经过身份验证或未经身份验证/匿名状态，都可以访问该操作。

> [!WARNING]
> `[AllowAnonymous]`跳过所有授权语句。 如果组合 `[AllowAnonymous]` 和任何 `[Authorize]` 属性，则 `[Authorize]` 忽略属性。 例如，如果在 `[AllowAnonymous]` 控制器级别应用，则 `[Authorize]` 会忽略同一控制器（或其中的任何操作）上的任何属性。

<a name="aarp"></a>

## <a name="authorize-attribute-and-razor-pages"></a>授权属性和 Razor Pages

<xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute>***不***能应用于 Razor 页面处理程序。 例如， `[Authorize]` 不能应用于 `OnGet` 、 `OnPost` 或任何其他页处理程序。 请考虑对具有不同处理程序的不同授权要求的页面使用 ASP.NET Core MVC 控制器。

以下两种方法可用于将授权应用于 Razor 页面处理程序方法：

* 对于需要不同授权的页面处理程序，请使用单独的页面。 将共享内容移动到一个或多个[分部视图](xref:mvc/views/partial)中。 如果可能，建议使用这种方法。
* 对于必须共享公共页面的内容，请编写一个作为[IAsyncPageFilter](xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter.OnPageHandlerSelectionAsync%2A)的一部分执行授权的筛选器。 [PageHandlerAuth](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/simple/samples/3.1/PageHandlerAuth) GitHub 项目演示了这种方法：
  * [AuthorizeIndexPageHandlerFilter](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/security/authorization/simple/samples/3.1/PageHandlerAuth/AuthorizeIndexPageHandlerFilter.cs)实现授权筛选器：[!code-csharp[](~/security/authorization/simple/samples/3.1/PageHandlerAuth/Pages/Index.cshtml.cs?name=snippet)]

  * [[AuthorizePageHandler]](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/simple/samples/3.1/PageHandlerAuth/Pages/Index.cshtml.cs#L16)属性应用于 `OnGet` 页面处理程序：[!code-csharp[](~/security/authorization/simple/samples/3.1/PageHandlerAuth/AuthorizeIndexPageHandlerFilter.cs?name=snippet)]

> [!WARNING]
> [PageHandlerAuth](https://github.com/pranavkm/PageHandlerAuth)示例***方法不执行以下操作***：
> * 结合应用于页面、页面模型或全局的授权属性进行撰写。 如果将一个或多个实例应用于页面，则组合授权属性会导致身份验证和授权多次执行 `AuthorizeAttribute` `AuthorizeFilter` 。
> * 与其他 ASP.NET Core 身份验证和授权系统一起工作。 你必须验证此方法是否适用于你的应用程序。

没有计划支持 `AuthorizeAttribute` Razor 页处理程序。 
