---
title: "구성 Id 기본 키 데이터 형식"
author: AdrienTorris
description: "이 문서에서는 ASP.NET Core Id 기본 키에 사용 되는 원하는 데이터 형식 구성 하기 위한 단계를 설명 합니다."
ms.author: scaddie
manager: wpickett
ms.date: 09/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: da46aa90a434a978a55467da982d746eb2d959ee
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/19/2018
---
# <a name="configure-the-aspnet-core-identity-primary-key-data-type"></a><span data-ttu-id="16940-103">ASP.NET Core Id 기본 키 데이터 형식 구성</span><span class="sxs-lookup"><span data-stu-id="16940-103">Configure the ASP.NET Core Identity primary key data type</span></span>

<span data-ttu-id="16940-104">ASP.NET Core Identity를 사용 하면 기본 키를 나타내는 데 사용 되는 데이터 형식을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16940-104">ASP.NET Core Identity allows you to configure the data type used to represent a primary key.</span></span> <span data-ttu-id="16940-105">사용 하 여 identity는 `string` 기본적으로 데이터 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="16940-105">Identity uses the `string` data type by default.</span></span> <span data-ttu-id="16940-106">이 동작을 재정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="16940-106">You can override this behavior.</span></span>

## <a name="customize-the-primary-key-data-type"></a><span data-ttu-id="16940-107">기본 키 데이터 형식을 사용자 지정</span><span class="sxs-lookup"><span data-stu-id="16940-107">Customize the primary key data type</span></span>

1. <span data-ttu-id="16940-108">사용자 지정 구현을 만듭니다는 [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="16940-108">Create a custom implementation of the [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) class.</span></span> <span data-ttu-id="16940-109">사용자 개체를 만드는 데 사용할 유형을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="16940-109">It represents the type to be used for creating user objects.</span></span> <span data-ttu-id="16940-110">다음 예제에서는 기본 `string` 형식으로 대체 됩니다 `Guid`합니다.</span><span class="sxs-lookup"><span data-stu-id="16940-110">In the following example, the default `string` type is replaced with `Guid`.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

1. <span data-ttu-id="16940-111">사용자 지정 구현을 만듭니다는 [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="16940-111">Create a custom implementation of the [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) class.</span></span> <span data-ttu-id="16940-112">역할 개체를 만드는 데 사용할 유형을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="16940-112">It represents the type to be used for creating role objects.</span></span> <span data-ttu-id="16940-113">다음 예제에서는 기본 `string` 형식으로 대체 됩니다 `Guid`합니다.</span><span class="sxs-lookup"><span data-stu-id="16940-113">In the following example, the default `string` type is replaced with `Guid`.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]
    
1. <span data-ttu-id="16940-114">사용자 지정 데이터베이스 컨텍스트 클래스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="16940-114">Create a custom database context class.</span></span> <span data-ttu-id="16940-115">Id에 사용 되는 Entity Framework 데이터베이스 컨텍스트 클래스에서 상속 합니다.</span><span class="sxs-lookup"><span data-stu-id="16940-115">It inherits from the Entity Framework database context class used for Identity.</span></span> <span data-ttu-id="16940-116">`TUser` 및 `TRole` 인수는 각각 이전 단계에서 만든 사용자 지정 사용자 및 역할의 클래스를 참조 합니다.</span><span class="sxs-lookup"><span data-stu-id="16940-116">The `TUser` and `TRole` arguments reference the custom user and role classes created in the previous step, respectively.</span></span> <span data-ttu-id="16940-117">`Guid` 기본 키에 대 한 데이터 형식을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="16940-117">The `Guid` data type is defined for the primary key.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
1. <span data-ttu-id="16940-118">응용 프로그램의 시작 클래스에서 Id 서비스를 추가할 때 사용자 지정 데이터베이스 컨텍스트 클래스를 등록 합니다.</span><span class="sxs-lookup"><span data-stu-id="16940-118">Register the custom database context class when adding the Identity service in the app's startup class.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="16940-119">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="16940-119">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    <span data-ttu-id="16940-120">`AddEntityFrameworkStores` 이 메서드를 사용 하지 않는 한 `TKey` ASP.NET Core에서 수행한 것으로 인수 1.x 합니다.</span><span class="sxs-lookup"><span data-stu-id="16940-120">The `AddEntityFrameworkStores` method doesn't accept a `TKey` argument as it did in ASP.NET Core 1.x.</span></span> <span data-ttu-id="16940-121">분석 하 여 기본 키의 데이터 형식을 유추할는 `DbContext` 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="16940-121">The primary key's data type is inferred by analyzing the `DbContext` object.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="16940-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="16940-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    <span data-ttu-id="16940-123">`AddEntityFrameworkStores` 메서드에 `TKey` 기본 키의 데이터 형식을 나타내는 인수입니다.</span><span class="sxs-lookup"><span data-stu-id="16940-123">The `AddEntityFrameworkStores` method accepts a `TKey` argument indicating the primary key's data type.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]
    
    ---

## <a name="test-the-changes"></a><span data-ttu-id="16940-124">변경 내용을 테스트합니다</span><span class="sxs-lookup"><span data-stu-id="16940-124">Test the changes</span></span>

<span data-ttu-id="16940-125">구성 변경으로 완료 되 면 기본 키를 나타내는 속성에는 새 데이터 형식을 반영 합니다.</span><span class="sxs-lookup"><span data-stu-id="16940-125">Upon completion of the configuration changes, the property representing the primary key reflects the new data type.</span></span> <span data-ttu-id="16940-126">다음 예제에서는 MVC 컨트롤러의 속성에 액세스 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="16940-126">The following example demonstrates accessing the property in an MVC controller.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]