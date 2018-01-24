---
title: "ASP.NET Web API에서 마이그레이션"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/webapi
ms.openlocfilehash: aef00b63c9889100116facc610bec99f889e4c46
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/19/2018
---
# <a name="migrating-from-aspnet-web-api"></a><span data-ttu-id="ea657-102">ASP.NET Web API에서 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="ea657-102">Migrating from ASP.NET Web API</span></span>

<span data-ttu-id="ea657-103">여 [Steve Smith](https://ardalis.com/) 및 [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="ea657-103">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="ea657-104">웹 Api는 다양 한 브라우저 및 모바일 장치를 포함 한 클라이언트를 연결할 HTTP 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-104">Web APIs are HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="ea657-105">ASP.NET Core MVC 웹 응용 프로그램의 단일 방법을 제공 하는 Web Api 작성에 대 한 지원이 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-105">ASP.NET Core MVC includes support for building Web APIs providing a single, consistent way of building web applications.</span></span> <span data-ttu-id="ea657-106">이 문서에서 ASP.NET Core mvc 웹 API 구현 ASP.NET Web API에서 마이그레이션하는 데 필요한 단계 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-106">In this article, we demonstrate the steps required to migrate a Web API implementation from ASP.NET Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="ea657-107">[샘플 코드 보기 또는 다운로드](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample)([다운로드 방법](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ea657-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="review-aspnet-web-api-project"></a><span data-ttu-id="ea657-108">ASP.NET Web API 프로젝트 검토</span><span class="sxs-lookup"><span data-stu-id="ea657-108">Review ASP.NET Web API Project</span></span>

<span data-ttu-id="ea657-109">이 문서에서는 샘플 프로젝트 *ProductsApp*문서에서 만든 [ASP.NET Web API 시작](https://docs.microsoft.com/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) 기준으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-109">This article uses the sample project, *ProductsApp*, created in the article [Getting Started with ASP.NET Web API](https://docs.microsoft.com/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) as its starting point.</span></span> <span data-ttu-id="ea657-110">해당 프로젝트에서 간단한 ASP.NET 웹 API 프로젝트는 다음과 같이 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-110">In that project, a simple ASP.NET Web API  project is configured as follows.</span></span>

<span data-ttu-id="ea657-111">*Global.asax.cs*, 하도록 호출 `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="ea657-111">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="ea657-112">`WebApiConfig`에 정의 된 *App_Start*, 하나의 정적 있으며 `Register` 메서드:</span><span class="sxs-lookup"><span data-stu-id="ea657-112">`WebApiConfig` is defined in *App_Start*, and has just one static `Register` method:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


<span data-ttu-id="ea657-113">이 클래스를 구성 [특성 라우팅을](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2)프로젝트에 사용 되 고 실제로 하지 않지만, 합니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-113">This class configures [attribute routing](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="ea657-114">또한 ASP.NET Web API에서 사용 되는 라우팅 테이블을 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-114">It also configures the routing table which is used by ASP.NET Web API.</span></span> <span data-ttu-id="ea657-115">이 경우 ASP.NET Web API는 것을 예상 형식과 일치 하도록 Url */api/ {controller} / {id}*와 *{id}* 선택 사항입니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-115">In this case, ASP.NET Web API will expect URLs to match the format */api/{controller}/{id}*, with *{id}* being optional.</span></span>

<span data-ttu-id="ea657-116">*ProductsApp* 프로젝트에서 상속 하는 하나의 simple 컨트롤러에 `ApiController` 두 메서드를 노출 합니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-116">The *ProductsApp* project includes just one simple controller, which inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="ea657-117">모델을 마지막으로, *제품*여는 데 사용 되는 *ProductsApp*, 간단한 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-117">Finally, the model, *Product*, used by the *ProductsApp*, is a simple class:</span></span>

[!code-csharp[Main](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="ea657-118">시작 하는 간단한 프로젝트를 만들었으므로 이제 해당 ASP.NET Core mvc이 웹 API 프로젝트를 마이그레이션하는 방법에 보여줄 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-118">Now that we have a simple project from which to start, we can demonstrate how to migrate this Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="ea657-119">대상 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="ea657-119">Create the Destination Project</span></span>

<span data-ttu-id="ea657-120">새로 만든 빈 솔루션을 만들려면 Visual Studio를 사용 하 고 이름을 *WebAPIMigration*합니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-120">Using Visual Studio, create a new, empty solution, and name it *WebAPIMigration*.</span></span> <span data-ttu-id="ea657-121">기존 항목 추가 *ProductsApp* 다음 프로젝트를 솔루션에 새 ASP.NET Core 웹 응용 프로그램 프로젝트를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-121">Add the existing *ProductsApp* project to it, then, add a new ASP.NET Core Web Application Project to the solution.</span></span> <span data-ttu-id="ea657-122">새 프로젝트의 이름을 *ProductsCore*합니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-122">Name the new project *ProductsCore*.</span></span>

![웹 서식 파일을 새 프로젝트 대화 상자를 엽니다.](webapi/_static/add-web-project.png)

<span data-ttu-id="ea657-124">웹 API 프로젝트 템플릿을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-124">Next, choose the Web API project template.</span></span> <span data-ttu-id="ea657-125">마이그레이션할 예정는 *ProductsApp* 이 새 프로젝트에 대 한 내용입니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-125">We will migrate the *ProductsApp* contents to this new project.</span></span>

![ASP.NET Core 템플릿 목록에서 선택한 웹 API 프로젝트 템플릿 사용 하 여 새 웹 응용 프로그램 대화 상자](webapi/_static/aspnet-5-webapi.png)

<span data-ttu-id="ea657-127">삭제 된 `Project_Readme.html` 새 프로젝트의 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-127">Delete the `Project_Readme.html` file from the new project.</span></span> <span data-ttu-id="ea657-128">솔루션은 이제 다음과 같이 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-128">Your solution should now look like this:</span></span>

![응용 프로그램 솔루션 파일 및 ProductsApp 및 ProductsCore 프로젝트의 폴더를 나타내는 솔루션 탐색기에서 열기](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a><span data-ttu-id="ea657-130">구성 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="ea657-130">Migrate Configuration</span></span>

<span data-ttu-id="ea657-131">ASP.NET Core 더 이상 사용 되지 *Global.asax*, *web.config*, 또는 *App_Start* 폴더입니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-131">ASP.NET Core no longer uses *Global.asax*, *web.config*, or *App_Start* folders.</span></span> <span data-ttu-id="ea657-132">모든 시작 작업을 수행 하는 대신, *Startup.cs* 프로젝트의 루트에 (참조 [응용 프로그램 시작](../fundamentals/startup.md)).</span><span class="sxs-lookup"><span data-stu-id="ea657-132">Instead, all startup tasks are done in *Startup.cs* in the root of the project (see [Application Startup](../fundamentals/startup.md)).</span></span> <span data-ttu-id="ea657-133">ASP.NET Core MVC에서 라우팅 특성 기반 이제 기본적으로 포함 됩니다 때 `UseMvc()` 가 호출 되 고 이것은 웹 API 경로 구성 하는 데 권장 되는 방법 (이며 라우팅 Web API 시작 프로젝트를 처리 하는 방법).</span><span class="sxs-lookup"><span data-stu-id="ea657-133">In ASP.NET Core MVC, attribute-based routing is now included by default when `UseMvc()` is called; and, this is the recommended approach for configuring Web API routes (and is how the Web API starter project handles routing).</span></span>

[!code-none[Main](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=40)]

<span data-ttu-id="ea657-134">특성 라우팅을 앞으로 프로젝트에 사용 하려는 경우 추가 구성이 필요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-134">Assuming you want to use attribute routing in your project going forward, no additional configuration is needed.</span></span> <span data-ttu-id="ea657-135">샘플 에서처럼 컨트롤러 및 작업을 하는 데 필요한 특성을 적용 하기만 하면 `ValuesController` Web API 시작 프로젝트에 포함 된 클래스:</span><span class="sxs-lookup"><span data-stu-id="ea657-135">Simply apply the attributes as needed to your controllers and actions, as is done in the sample `ValuesController` class that is included in the Web API starter project:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

<span data-ttu-id="ea657-136">있다는 사실에 주의 *[컨트롤러]* 8입니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-136">Note the presence of *[controller]* on line 8.</span></span> <span data-ttu-id="ea657-137">이제 특성 기반 라우팅와 같은 특정 토큰, 지원 *[컨트롤러]* 및 *[작업]*합니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-137">Attribute-based routing now supports certain tokens, such as *[controller]* and *[action]*.</span></span> <span data-ttu-id="ea657-138">이러한 토큰은 런타임에 교체 됨 컨트롤러 또는 동작의 이름으로 각각 특성이 적용 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-138">These tokens are replaced at runtime with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="ea657-139">프로젝트에서 매직 문자열의 수를 줄이기 위해이 처리 되 고 확인 하는 경로 동기화 되어야 해당 컨트롤러 및 작업 항목과 함께 자동 이름 바꾸기 리팩터링 적용 되는 경우.</span><span class="sxs-lookup"><span data-stu-id="ea657-139">This serves to reduce the number of magic strings in the project, and it ensures the routes will be kept synchronized with their corresponding controllers and actions when automatic rename refactorings are applied.</span></span>

<span data-ttu-id="ea657-140">제품 API 컨트롤러를 마이그레이션하려면에서는 먼저 복사 *ProductsController* 새 프로젝트에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-140">To migrate the Products API controller, we must first copy *ProductsController* to the new project.</span></span> <span data-ttu-id="ea657-141">단순히 컨트롤러에서 경로 특성을 포함 한 다음:</span><span class="sxs-lookup"><span data-stu-id="ea657-141">Then simply include the route attribute on the controller:</span></span>

```csharp
[Route("api/[controller]")]
```

<span data-ttu-id="ea657-142">추가 해야는 `[HttpGet]` HTTP Get을 호출할 수는 모두 있으므로 두 개의 메서드 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-142">You also need to add the `[HttpGet]` attribute to the two methods, since they both should be called via HTTP Get.</span></span> <span data-ttu-id="ea657-143">에 대 한 특성에는 "id" 매개 변수를 기대 포함 `GetProduct()`:</span><span class="sxs-lookup"><span data-stu-id="ea657-143">Include the expectation of an "id" parameter in the attribute for `GetProduct()`:</span></span>

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

<span data-ttu-id="ea657-144">이 시점에서 제대로 구성 되어 라우팅 그러나 해당 테스트 아직 없습니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-144">At this point, routing is configured correctly; however, we can't yet test it.</span></span> <span data-ttu-id="ea657-145">추가 변경이 필요 하기 전에 *ProductsController* 컴파일됩니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-145">Additional changes must be made before *ProductsController* will compile.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="ea657-146">모델 및 컨트롤러 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="ea657-146">Migrate Models and Controllers</span></span>

<span data-ttu-id="ea657-147">이 간단한 Web API 프로젝트에 대 한 마이그레이션 프로세스의 마지막 단계는 컨트롤러 및를 사용 하는 모델을 통해 복사 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-147">The last step in the migration process for this simple Web API project is to copy over the Controllers and any Models they use.</span></span> <span data-ttu-id="ea657-148">이 경우 단순히 복사할 *Controllers/ProductsController.cs* 새로운 원래 프로젝트에서.</span><span class="sxs-lookup"><span data-stu-id="ea657-148">In this case, simply copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span> <span data-ttu-id="ea657-149">그런 다음 새로운 원래 프로젝트에서 전체 모델 폴더를 복사 합니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-149">Then, copy the entire Models folder from the original project to the new one.</span></span> <span data-ttu-id="ea657-150">새 프로젝트 이름과 일치 하도록 네임 스페이스 조정 (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="ea657-150">Adjust the namespaces to match the new project name (*ProductsCore*).</span></span>  <span data-ttu-id="ea657-151">이 시점에서 응용 프로그램을 빌드하고 컴파일 오류의 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-151">At this point, you can build the application, and you will find a number of compilation errors.</span></span> <span data-ttu-id="ea657-152">일반적으로 다음 범주에 속해 있어야 합니다. 이러한:</span><span class="sxs-lookup"><span data-stu-id="ea657-152">These should generally fall into the following categories:</span></span>

* <span data-ttu-id="ea657-153">*ApiController* 존재 하지 않는</span><span class="sxs-lookup"><span data-stu-id="ea657-153">*ApiController* does not exist</span></span>

* <span data-ttu-id="ea657-154">*System.Web.Http* 네임 스페이스가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-154">*System.Web.Http* namespace does not exist</span></span>

* <span data-ttu-id="ea657-155">*IHttpActionResult* 존재 하지 않는</span><span class="sxs-lookup"><span data-stu-id="ea657-155">*IHttpActionResult* does not exist</span></span>

<span data-ttu-id="ea657-156">다행히 이들은 모두를 쉽게 해결 합니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-156">Fortunately, these are all very easy to correct:</span></span>

* <span data-ttu-id="ea657-157">변경 *ApiController* 를 *컨트롤러* (추가 해야 할 수도 *Microsoft.AspNetCore.Mvc를 사용 하 여*)</span><span class="sxs-lookup"><span data-stu-id="ea657-157">Change *ApiController* to *Controller* (you may need to add *using Microsoft.AspNetCore.Mvc*)</span></span>

* <span data-ttu-id="ea657-158">삭제를 참조 하는 문을 사용 하 여 *System.Web.Http*</span><span class="sxs-lookup"><span data-stu-id="ea657-158">Delete any using statement referring to *System.Web.Http*</span></span>

* <span data-ttu-id="ea657-159">반환 하는 모든 메서드에 변경 *IHttpActionResult* 반환 하는 *IActionResult*</span><span class="sxs-lookup"><span data-stu-id="ea657-159">Change any method returning *IHttpActionResult* to return a *IActionResult*</span></span>

<span data-ttu-id="ea657-160">이러한 변경에 수행 하 고 사용 하지 않는 된 문을 사용 하 여 면 제거 마이그레이션된 *ProductsController* 클래스는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-160">Once these changes have been made and unused using statements removed, the migrated *ProductsController* class looks like this:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

<span data-ttu-id="ea657-161">마이그레이션된 프로젝트를 실행 하 고를 이제 있어야 */api/제품*; 및 3 제품의 전체 목록을 표시 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-161">You should now be able to run the migrated project and browse to */api/products*; and, you should see the full list of 3 products.</span></span> <span data-ttu-id="ea657-162">찾아 */api/products/1* 첫 번째 제품 표시 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-162">Browse to */api/products/1* and you should see the first product.</span></span>

## <a name="summary"></a><span data-ttu-id="ea657-163">요약</span><span class="sxs-lookup"><span data-stu-id="ea657-163">Summary</span></span>

<span data-ttu-id="ea657-164">ASP.NET Core mvc 간단한 ASP.NET 웹 API 프로젝트를 마이그레이션하는 매우 간단 고마워요 ASP.NET Core MVC의 웹 Api에 대 한 기본 제공 지원으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-164">Migrating a simple ASP.NET Web API project to ASP.NET Core MVC is fairly straightforward, thanks to the built-in support for Web APIs in ASP.NET Core MVC.</span></span> <span data-ttu-id="ea657-165">모든 ASP.NET Web API 프로젝트는 마이그레이션해야 할 주요 작업은 경로, 컨트롤러 및 컨트롤러 및 작업에서 사용 되는 형식에 대 한 업데이트와 함께 모델입니다.</span><span class="sxs-lookup"><span data-stu-id="ea657-165">The main pieces every ASP.NET Web API project will need to migrate are routes, controllers, and models, along with updates to the types used by  controllers and actions.</span></span>