---
title: "단순 권한 부여"
author: rick-anderson
description: "이 문서에서는 ASP.NET Core 컨트롤러 및 동작에 대 한 액세스를 제한 하려면 권한 부여 특성을 사용 하는 방법에 설명 합니다."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/simple
ms.openlocfilehash: f1d5671785da815f2f4fcf5bef1352f4c9e62877
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/19/2018
---
# <a name="simple-authorization"></a><span data-ttu-id="6db65-103">단순 권한 부여</span><span class="sxs-lookup"><span data-stu-id="6db65-103">Simple Authorization</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="6db65-104">MVC에서 권한 부여를 통해 제어 됩니다는 `AuthorizeAttribute` 특성 및 해당 다양 한 매개 변수입니다.</span><span class="sxs-lookup"><span data-stu-id="6db65-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="6db65-105">가장 간단한 적용 된 `AuthorizeAttribute` 특성는 컨트롤러나 작업 제한에 대 한 액세스는 컨트롤러 또는 모든 인증 된 사용자 동작을 합니다.</span><span class="sxs-lookup"><span data-stu-id="6db65-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="6db65-106">다음 코드에 대 한 액세스를 제한 하는 예를 들어는 `AccountController` 모든 인증 된 사용자입니다.</span><span class="sxs-lookup"><span data-stu-id="6db65-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="6db65-107">컨트롤러 아니라 작업에 권한 부여를 적용 하려면 적용 된 `AuthorizeAttribute` 동작 자체에 특성:</span><span class="sxs-lookup"><span data-stu-id="6db65-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="6db65-108">이제 인증 된 사용자만 액세스할 수는 `Logout` 함수입니다.</span><span class="sxs-lookup"><span data-stu-id="6db65-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="6db65-109">사용할 수도 있습니다는 `AllowAnonymousAttribute` 특성 개별 작업에 대 한 인증 되지 않은 사용자가 액세스할 수 있도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="6db65-109">You can also use the `AllowAnonymousAttribute` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="6db65-110">예:</span><span class="sxs-lookup"><span data-stu-id="6db65-110">For example:</span></span>

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

<span data-ttu-id="6db65-111">이렇게 하면 인증 된 사용자만는 `AccountController`를 제외 하 고는 `Login` / 익명 인증 된 인증 되지 않은 상태에 관계 없이 모든 사용자가 액세스할 수 있는 작업입니다.</span><span class="sxs-lookup"><span data-stu-id="6db65-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

>[!WARNING]
> <span data-ttu-id="6db65-112">`[AllowAnonymous]`모든 권한 부여 문을 무시합니다.</span><span class="sxs-lookup"><span data-stu-id="6db65-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="6db65-113">결합 되어 감사가 만들어집니다를 적용 하는 경우 `[AllowAnonymous]` 임의의 `[Authorize]` 특성 설정한 다음 권한 부여 특성은 항상 무시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="6db65-113">If you apply combine `[AllowAnonymous]` and any `[Authorize]` attribute then the Authorize attributes will always be ignored.</span></span> <span data-ttu-id="6db65-114">예를 적용 하는 경우 `[AllowAnonymous]` 모든 컨트롤러 수준 `[Authorize]` 내 모든 작업 또는 동일한 컨트롤러에서 특성이 무시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="6db65-114">For example if you apply `[AllowAnonymous]` at the controller level any `[Authorize]` attributes on the same controller, or on any action within it will be ignored.</span></span>