---
title: "권한 부여에 의해 보호 되는 사용자 데이터와 ASP.NET Core 응용 프로그램 만들기"
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 05/22/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: security/authorization/secure-data
ms.openlocfilehash: 861ac619c7f5fb19a56c59536e20724d96bbddca
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/19/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="f8579-102">권한 부여에 의해 보호 되는 사용자 데이터와 ASP.NET Core 응용 프로그램 만들기</span><span class="sxs-lookup"><span data-stu-id="f8579-102">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="f8579-103">작성자: [Rick Anderson](https://twitter.com/RickAndMSFT) 및 [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="f8579-103">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="f8579-104">이 자습서에는 권한 부여에 의해 보호 되는 사용자 데이터와 웹 응용 프로그램을 만드는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-104">This tutorial shows how to create a web app with user data protected by authorization.</span></span> <span data-ttu-id="f8579-105">인증 된 (등록 된) 사용자 연락처 목록을 표시를 만들었습니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-105">It  displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="f8579-106">세 가지 보안 그룹이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-106">There are three security groups:</span></span>

* <span data-ttu-id="f8579-107">등록 된 사용자의 승인 된 모든 연락처 데이터를 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-107">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="f8579-108">등록 된 사용자 수 편집/삭제 자신의 데이터입니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-108">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="f8579-109">관리자 승인 하거나 연락 데이터를 거부할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-109">Managers can approve or reject contact data.</span></span> <span data-ttu-id="f8579-110">승인 된 연락처에만 사용자에 게 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-110">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="f8579-111">관리자 승인/거부 있으며 편집/삭제 된 데이터.</span><span class="sxs-lookup"><span data-stu-id="f8579-111">Administrators can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="f8579-112">다음 이미지에서는 Rick 사용자 (`rick@example.com`)에 로그인 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-112">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="f8579-113">사용자 Rick 수 있는 유일한 보기 승인 연락처 및 그의 연락처 편집/삭제.</span><span class="sxs-lookup"><span data-stu-id="f8579-113">User Rick can only view approved contacts and edit/delete his contacts.</span></span> <span data-ttu-id="f8579-114">마지막 레코드만 Rick에서 만든, 편집 표시 및 링크 삭제</span><span class="sxs-lookup"><span data-stu-id="f8579-114">Only the last record, created by Rick, displays edit and delete links</span></span>

![위에서 설명한 이미지](secure-data/_static/rick.png)

<span data-ttu-id="f8579-116">다음 그림에 `manager@contoso.com` 관리자 역할에서 서명 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-116">In the following image, `manager@contoso.com` is signed in and in the managers role.</span></span> 

![위에서 설명한 이미지](secure-data/_static/manager1.png)

<span data-ttu-id="f8579-118">다음 이미지는 관리자의 연락처 세부 정보 보기를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-118">The following image shows the  managers details view of a contact.</span></span>

![위에서 설명한 이미지](secure-data/_static/manager.png)

<span data-ttu-id="f8579-120">관리자 및 관리자의 승인 있고 단추를 거부 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-120">Only managers and administrators have the approve and reject buttons.</span></span>

<span data-ttu-id="f8579-121">다음 그림에 `admin@contoso.com` 관리자의 역할에서 서명 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-121">In the following image, `admin@contoso.com` is signed in and in the administrator’s role.</span></span> 

![위에서 설명한 이미지](secure-data/_static/admin.png)

<span data-ttu-id="f8579-123">관리자는 모든 권한을 갖습니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-123">The administrator has all privileges.</span></span> <span data-ttu-id="f8579-124">그녀는 모든 연락처 읽기/편집/삭제 수 한 연락처의 상태를 변경 했습니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-124">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="f8579-125">응용 프로그램에서 만들어진 [스 캐 폴딩](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) 다음 `Contact` 모델:</span><span class="sxs-lookup"><span data-stu-id="f8579-125">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller)  the following `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="f8579-126">A `ContactIsOwnerAuthorizationHandler` 인증 처리기를 사용 하면 사용자가 데이터를 편집할 수만 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-126">A `ContactIsOwnerAuthorizationHandler` authorization handler ensures that a user can only edit their data.</span></span> <span data-ttu-id="f8579-127">A `ContactManagerAuthorizationHandler` 처리기 권한 부여 관리자 승인 또는 거부 연락처를 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-127">A `ContactManagerAuthorizationHandler` authorization handler allows managers to approve or reject contacts.</span></span>  <span data-ttu-id="f8579-128">A `ContactAdministratorsAuthorizationHandler` 을 승인 하거나 거부할 연락처 연락처 편집/삭제 하는 관리자를 사용 하는 인증 처리기입니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-128">A `ContactAdministratorsAuthorizationHandler` authorization handler allows administrators to approve or reject contacts and to edit/delete contacts.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="f8579-129">필수 구성 요소</span><span class="sxs-lookup"><span data-stu-id="f8579-129">Prerequisites</span></span>

<span data-ttu-id="f8579-130">시작 자습서 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-130">This is not a beginning tutorial.</span></span> <span data-ttu-id="f8579-131">에 대해 잘 알고 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-131">You should be familiar with:</span></span>

* [<span data-ttu-id="f8579-132">ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="f8579-132">ASP.NET Core MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="f8579-133">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="f8579-133">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="f8579-134">시작 및 완료 된 앱</span><span class="sxs-lookup"><span data-stu-id="f8579-134">The starter and completed app</span></span>

<span data-ttu-id="f8579-135">[다운로드](xref:tutorials/index#how-to-download-a-sample) 는 [완료](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) 응용 프로그램입니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-135">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) app.</span></span> <span data-ttu-id="f8579-136">[테스트](#test-the-completed-app) 해당 보안 기능에 잘 알고 있으므로 완료 된 앱입니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-136">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span> 

### <a name="the-starter-app"></a><span data-ttu-id="f8579-137">시작 응용 프로그램</span><span class="sxs-lookup"><span data-stu-id="f8579-137">The starter app</span></span>

<span data-ttu-id="f8579-138">완성 된 샘플을 사용 하 여 코드를 비교 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-138">It's helpful to compare your code with the completed sample.</span></span>

<span data-ttu-id="f8579-139">[다운로드](xref:tutorials/index#how-to-download-a-sample) 는 [스타터](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) 응용 프로그램입니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-139">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) app.</span></span> 

<span data-ttu-id="f8579-140">참조 [스타터 앱 만들기](#create-the-starter-app) 원하는 처음부터 만들 경우.</span><span class="sxs-lookup"><span data-stu-id="f8579-140">See [Create the starter app](#create-the-starter-app) if you'd like to create it from scratch.</span></span>

<span data-ttu-id="f8579-141">데이터베이스를 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-141">Update the database:</span></span>

```none
   dotnet ef database update
```

<span data-ttu-id="f8579-142">응용 프로그램 실행을 탭의 **ContactManager** 링크를 선택한 만들기, 편집 및 연락처를 삭제를 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-142">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

<span data-ttu-id="f8579-143">이 자습서에는 안전한 사용자 데이터 응용 프로그램을 만드는 모든 주요 단계입니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-143">This tutorial has all the major steps to create the secure user data app.</span></span> <span data-ttu-id="f8579-144">완료 된 프로젝트를 참조 하는 것이 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-144">You may find it helpful to refer to the completed project.</span></span>

## <a name="modify-the-app-to-secure-user-data"></a><span data-ttu-id="f8579-145">사용자 데이터 보호를 위해 응용 프로그램 수정</span><span class="sxs-lookup"><span data-stu-id="f8579-145">Modify the app to secure user data</span></span>

<span data-ttu-id="f8579-146">다음 섹션에서는 보안 사용자 데이터 응용 프로그램을 만드는 모든 주요 단계 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-146">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="f8579-147">완료 된 프로젝트를 참조 하는 것이 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-147">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="f8579-148">사용자에 게 연락 데이터를 연결 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-148">Tie the contact data to the user</span></span>

<span data-ttu-id="f8579-149">ASP.NET을 사용 하 여 [Identity](xref:security/authentication/identity) 가 데이터를 하지만 다른 사용자가 데이터가 아닌 사용자 ID 사용자를 편집할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-149">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="f8579-150">추가 `OwnerID` 에 `Contact` 모델:</span><span class="sxs-lookup"><span data-stu-id="f8579-150">Add `OwnerID` to the `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]

<span data-ttu-id="f8579-151">`OwnerID`사용자의 id는 `AspNetUser` 테이블에 [Identity](xref:security/authentication/identity) 데이터베이스입니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-151">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="f8579-152">`Status` 필드 연락처 일반 사용자가 볼 수 있는지 여부를 결정 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-152">The `Status` field determines if a contact is viewable by general users.</span></span> 

<span data-ttu-id="f8579-153">새 마이그레이션을 스 캐 폴딩 하 고 데이터베이스를 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-153">Scaffold a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
 ```

### <a name="require-ssl-and-authenticated-users"></a><span data-ttu-id="f8579-154">SSL 및 인증 된 사용자가 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-154">Require SSL and authenticated users</span></span>

<span data-ttu-id="f8579-155">에 `ConfigureServices` 의 메서드는 *Startup.cs* 파일에서 추가 된 [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) 권한 부여 필터:</span><span class="sxs-lookup"><span data-stu-id="f8579-155">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]

<span data-ttu-id="f8579-156">참조를 HTTPS로 HTTP 요청을 리디렉션할 [URL 다시 쓰기 미들웨어](xref:fundamentals/url-rewriting)합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-156">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="f8579-157">Visual Studio 코드를 사용 하 여 테스트 인증서를 SSL에 대 한 포함 되지 않은 로컬 플랫폼에 대 한 테스트 하거나 경우:</span><span class="sxs-lookup"><span data-stu-id="f8579-157">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="f8579-158">설정 `"LocalTest:skipSSL": true` 에 *appsettings.json* 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-158">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="f8579-159">인증 된 사용자가 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-159">Require authenticated users</span></span>

<span data-ttu-id="f8579-160">사용자를 인증 하도록 요구 하는 기본 인증 정책을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-160">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="f8579-161">인증을 사용 하 여 컨트롤러 또는 동작 메서드에서 옵트아웃을 선택할 수 있습니다는 `[AllowAnonymous]` 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-161">You can opt out of authentication at the controller or action method with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="f8579-162">이 접근 방식 추가 된 새 컨트롤러로 자동으로 내용의 포함 하도록 새 컨트롤러에 의존 하는 보다 더 안전 하 게 하는 인증 된 `[Authorize]` 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-162">With this approach, any new controllers added will automatically require authentication, which is safer than relying on new controllers to include the `[Authorize]` attribute.</span></span> <span data-ttu-id="f8579-163">다음을 추가 `ConfigureServices` 의 메서드는 *Startup.cs* 파일:</span><span class="sxs-lookup"><span data-stu-id="f8579-163">Add the following to  the `ConfigureServices` method of the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]

<span data-ttu-id="f8579-164">추가 `[AllowAnonymous]` 익명 사용자가 등록 하기 전에 사이트에 대 한 정보를 가져올 수 있도록 홈 컨트롤러에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-164">Add `[AllowAnonymous]` to the home controller so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="f8579-165">테스트 계정 구성</span><span class="sxs-lookup"><span data-stu-id="f8579-165">Configure the test account</span></span>

<span data-ttu-id="f8579-166">`SeedData` 두 계정, 관리자 및 관리자 클래스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-166">The `SeedData` class creates two accounts,  administrator and manager.</span></span> <span data-ttu-id="f8579-167">사용 하 여는 [암호 관리자 도구](xref:security/app-secrets) 이러한 계정에 대 한 암호를 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-167">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="f8579-168">프로젝트 디렉터리에서이 작업을 수행 (포함 된 디렉터리 *Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="f8579-168">Do this from the project directory (the directory containing *Program.cs*).</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="f8579-169">업데이트 `Configure` 테스트 암호를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-169">Update `Configure` to use the test password:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]

<span data-ttu-id="f8579-170">관리자 사용자 ID를 추가 하 고 `Status = ContactStatus.Approved` 연락처에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-170">Add the administrator user ID and `Status = ContactStatus.Approved` to the contacts.</span></span> <span data-ttu-id="f8579-171">사용자 ID 모든 연락처를 추가 하나만 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-171">Only one contact is shown, add the user ID to all contacts:</span></span>

[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="f8579-172">소유자, 관리자 및 관리자 권한 부여 처리기 만들기</span><span class="sxs-lookup"><span data-stu-id="f8579-172">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="f8579-173">만들기는 `ContactIsOwnerAuthorizationHandler` 클래스에 *권한 부여* 폴더입니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-173">Create a `ContactIsOwnerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="f8579-174">`ContactIsOwnerAuthorizationHandler` 리소스를 소유 하는 리소스에 대해 작동 하는 사용자를 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-174">The `ContactIsOwnerAuthorizationHandler` will verify the user acting on the resource owns the resource.</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="f8579-175">`ContactIsOwnerAuthorizationHandler` 호출 `context.Succeed` 현재 인증 된 사용자가 연락처 소유자 인 경우.</span><span class="sxs-lookup"><span data-stu-id="f8579-175">The `ContactIsOwnerAuthorizationHandler` calls `context.Succeed` if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="f8579-176">권한 부여 처리기는 일반적으로 반환 `context.Succeed` 에서 요구 사항이 충족 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-176">Authorization handlers generally return `context.Succeed` when the requirements are met.</span></span> <span data-ttu-id="f8579-177">반환 `Task.FromResult(0)` 요구 사항이 충족 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-177">They return `Task.FromResult(0)` when requirements are not met.</span></span> <span data-ttu-id="f8579-178">`Task.FromResult(0)`성공 또는 실패를 모두는 다른 권한 부여 핸들러를 실행 하도록 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-178">`Task.FromResult(0)` is neither success or failure, it allows other authorization handler to run.</span></span> <span data-ttu-id="f8579-179">명시적으로 실패 해야 할 경우 반환 `context.Fail()`합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-179">If you need to explicitly fail, return `context.Fail()`.</span></span>

<span data-ttu-id="f8579-180">연락처 소유자가 데이터를 편집/삭제 자신의 고유 데이터 확인 요구 사항 매개 변수에 전달 된 작업을 필요가 없습니다 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-180">We allow contact owners to edit/delete their own data, so we don't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="f8579-181">관리자 권한 부여 처리기 만들기</span><span class="sxs-lookup"><span data-stu-id="f8579-181">Create a manager authorization handler</span></span>

<span data-ttu-id="f8579-182">만들기는 `ContactManagerAuthorizationHandler` 클래스에 *권한 부여* 폴더입니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-182">Create a `ContactManagerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="f8579-183">`ContactManagerAuthorizationHandler` 리소스에 대해 작동 하는 사용자는 관리자를 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-183">The `ContactManagerAuthorizationHandler` will verify the user acting on the resource is a manager.</span></span> <span data-ttu-id="f8579-184">관리자만 승인 하거나 (새롭거나 변경 된) 콘텐츠 변경 내용을 거부할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-184">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="f8579-185">관리자 권한 부여 처리기 만들기</span><span class="sxs-lookup"><span data-stu-id="f8579-185">Create an administrator authorization handler</span></span>

<span data-ttu-id="f8579-186">만들기는 `ContactAdministratorsAuthorizationHandler` 클래스에 *권한 부여* 폴더입니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-186">Create a `ContactAdministratorsAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="f8579-187">`ContactAdministratorsAuthorizationHandler` 리소스에 대해 작동 하는 사용자가 관리자를 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-187">The `ContactAdministratorsAuthorizationHandler` will verify the user acting on the resource is a administrator.</span></span> <span data-ttu-id="f8579-188">관리자는 모든 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-188">Administrator can do all operations.</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="f8579-189">인증 처리기를 등록 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-189">Register the authorization handlers</span></span>

<span data-ttu-id="f8579-190">Entity Framework Core를 사용 하 여 서비스를 위해 등록 되어야 [종속성 주입](xref:fundamentals/dependency-injection) 를 사용 하 여 [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions)합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-190">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="f8579-191">`ContactIsOwnerAuthorizationHandler` ASP.NET Core를 사용 하 여 [Identity](xref:security/authentication/identity), Entity Framework Core 기반입니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-191">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="f8579-192">사용할 수 있도록 서비스 컬렉션에는 처리기를 등록 된 `ContactsController` 통해 [종속성 주입](xref:fundamentals/dependency-injection)합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-192">Register the handlers with the service collection so they will be available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="f8579-193">다음 코드의 끝에 추가 `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f8579-193">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]

<span data-ttu-id="f8579-194">`ContactAdministratorsAuthorizationHandler`및 `ContactManagerAuthorizationHandler` 단일 항목으로 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-194">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="f8579-195">Singleton 이들은 하므로 EF를 사용 하지 않아 이며 필요한 모든 정보에는 `Context` 의 매개 변수는 `HandleRequirementAsync` 메서드.</span><span class="sxs-lookup"><span data-stu-id="f8579-195">They are singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

<span data-ttu-id="f8579-196">전체 `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f8579-196">The complete `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]

## <a name="update-the-code-to-support-authorization"></a><span data-ttu-id="f8579-197">권한 부여를 지원 하기 위한 코드를 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-197">Update the code to support authorization</span></span>

<span data-ttu-id="f8579-198">이 섹션에서는 컨트롤러와 뷰 업데이트 및 작업 요구 사항 클래스를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-198">In this section, you update the controller and views and add an operations requirements class.</span></span>

### <a name="update-the-contacts-controller"></a><span data-ttu-id="f8579-199">연락처 컨트롤러를 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-199">Update the Contacts controller</span></span>

<span data-ttu-id="f8579-200">업데이트는 `ContactsController` 생성자:</span><span class="sxs-lookup"><span data-stu-id="f8579-200">Update the `ContactsController` constructor:</span></span>

* <span data-ttu-id="f8579-201">추가 `IAuthorizationService` 서비스가 권한 부여 처리기에 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-201">Add the `IAuthorizationService` service to  access to the authorization handlers.</span></span> 
* <span data-ttu-id="f8579-202">추가 `Identity` `UserManager` 서비스:</span><span class="sxs-lookup"><span data-stu-id="f8579-202">Add the `Identity` `UserManager` service:</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]

### <a name="add-a-contact-operations-requirements-class"></a><span data-ttu-id="f8579-203">연락처 작업 요구 사항 클래스를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-203">Add a contact operations requirements class</span></span>

<span data-ttu-id="f8579-204">추가 `ContactOperations` 클래스는 *권한 부여* 폴더입니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-204">Add the `ContactOperations` class to the *Authorization* folder.</span></span> <span data-ttu-id="f8579-205">이 클래스는 요구 사항이 응용 프로그램이 지 원하는:</span><span class="sxs-lookup"><span data-stu-id="f8579-205">This class  contain the requirements our app supports:</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

### <a name="update-create"></a><span data-ttu-id="f8579-206">업데이트 만들기</span><span class="sxs-lookup"><span data-stu-id="f8579-206">Update Create</span></span>

<span data-ttu-id="f8579-207">업데이트는 `HTTP POST Create` 메서드:</span><span class="sxs-lookup"><span data-stu-id="f8579-207">Update the `HTTP POST Create` method to:</span></span>

* <span data-ttu-id="f8579-208">사용자 ID를 추가 `Contact` 모델입니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-208">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="f8579-209">연락처를 소유 하 고 확인 하려면 권한 부여 처리기를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-209">Call the authorization handler to verify the user owns the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]

### <a name="update-edit"></a><span data-ttu-id="f8579-210">업데이트 편집</span><span class="sxs-lookup"><span data-stu-id="f8579-210">Update Edit</span></span>

<span data-ttu-id="f8579-211">모두 업데이트 `Edit` 연락처를 담당 하는 사용자를 확인 하 여 권한 부여 처리기를 사용 하는 메서드.</span><span class="sxs-lookup"><span data-stu-id="f8579-211">Update both `Edit` methods to use the authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="f8579-212">리소스 권한 부여를 수행 하는 것 때문에 사용할 수 없습니다는 `[Authorize]` 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-212">Because we are performing resource authorization we cannot use the `[Authorize]` attribute.</span></span> <span data-ttu-id="f8579-213">특성 평가 되는 경우 리소스에 대 한 액세스를 아직 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-213">We don't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="f8579-214">리소스 기반 권한 부여는 명령적 이어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-214">Resource based authorization must be imperative.</span></span> <span data-ttu-id="f8579-215">컨트롤러에서 로드 하거나 자체 처리기 내에서 로드 하 여 리소스에 액세스할 수 있는 검사를 수행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-215">Checks must be performed once we have access to the resource, either by loading it in our controller, or by loading it within the handler itself.</span></span> <span data-ttu-id="f8579-216">자주 리소스의 리소스 키를 전달 하 여 액세스 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-216">Frequently you will access the resource by passing in the resource key.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]

### <a name="update-the-delete-method"></a><span data-ttu-id="f8579-217">Delete 메서드를 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-217">Update the Delete method</span></span>

<span data-ttu-id="f8579-218">모두 업데이트 `Delete` 연락처를 담당 하는 사용자를 확인 하 여 권한 부여 처리기를 사용 하는 메서드.</span><span class="sxs-lookup"><span data-stu-id="f8579-218">Update both `Delete` methods to use the authorization handler to verify the user owns the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="f8579-219">인증 서비스는 뷰에 삽입</span><span class="sxs-lookup"><span data-stu-id="f8579-219">Inject the authorization service into the views</span></span>

<span data-ttu-id="f8579-220">현재 UI에 표시 된 편집한 사용자가을 수정할 수 없는 데이터에 대 한 링크를 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-220">Currently the UI shows edit and delete links for data the user cannot modify.</span></span> <span data-ttu-id="f8579-221">권한 부여 처리기 보기에 적용 하 여이 문제를 해결 합니다 했습니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-221">We'll fix that by applying the authorization handler to the views.</span></span>

<span data-ttu-id="f8579-222">권한 부여 서비스를 삽입할는 *Views/_ViewImports.cshtml* 파일 되므로 모든 보기에 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-222">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it will be available to all views:</span></span>

[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]

<span data-ttu-id="f8579-223">업데이트는 *Views/Contacts/Index.cshtml* Razor 뷰를만 편집을 표시 하 고 사용자에 게 수 편집/삭제 연락처에 대 한 링크를 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-223">Update the *Views/Contacts/Index.cshtml* Razor view to only display the edit and delete links for users who can edit/delete the contact.</span></span>

<span data-ttu-id="f8579-224">추가`@using ContactManager.Authorization;`</span><span class="sxs-lookup"><span data-stu-id="f8579-224">Add `@using ContactManager.Authorization;`</span></span>

<span data-ttu-id="f8579-225">업데이트는 `Edit` 및 `Delete` 하므로 편집 및 연락처를 삭제할 수 있는 권한 가진 사용자에 대 한 렌더링 되는 링크입니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-225">Update the `Edit` and `Delete` links so they are only rendered for users with permission to edit and delete the contact.</span></span>

[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]

<span data-ttu-id="f8579-226">경고: 데이터를 편집 또는 삭제 권한이 없는 사용자 로부터 링크 숨기기 응용 프로그램을 보호 하지 못합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-226">Warning: Hiding links from users that do not have permission to edit or delete data does not secure the app.</span></span> <span data-ttu-id="f8579-227">링크 숨기기 사용 하면 응용 프로그램 사용자 친숙 한 유효한 링크를 표시 하 여 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-227">Hiding links makes the app more user friendly by displaying only valid links.</span></span> <span data-ttu-id="f8579-228">사용자가 편집을 호출 하 고 자신이 소유 하지 않는 데이터에 대 한 작업을 삭제 하도록 생성 된 Url 해킹 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-228">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span>  <span data-ttu-id="f8579-229">컨트롤러는 액세스 보안 검사를 반복 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-229">The controller must repeat the access checks to be secure.</span></span>

### <a name="update-the-details-view"></a><span data-ttu-id="f8579-230">업데이트 세부 정보 보기</span><span class="sxs-lookup"><span data-stu-id="f8579-230">Update the Details view</span></span>

<span data-ttu-id="f8579-231">관리자 승인 또는 연락처를 거부할 수 있도록 자세히 보기를 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-231">Update the details view so managers can approve or reject contacts:</span></span>

[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]

## <a name="test-the-completed-app"></a><span data-ttu-id="f8579-232">완성 된 응용 프로그램 테스트</span><span class="sxs-lookup"><span data-stu-id="f8579-232">Test the completed app</span></span>

<span data-ttu-id="f8579-233">Visual Studio 코드를 사용 하 여 테스트 인증서를 SSL에 대 한 포함 되지 않은 로컬 플랫폼에 대 한 테스트 하거나 경우:</span><span class="sxs-lookup"><span data-stu-id="f8579-233">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="f8579-234">설정 `"LocalTest:skipSSL": true` 에 *appsettings.json* 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-234">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

<span data-ttu-id="f8579-235">응용 프로그램을 실행 한 경우 연락처가 삭제의 모든 레코드는 `Contact` 테이블 및 데이터베이스를 시드하고에 앱을 다시 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-235">If you have run the app and have contacts, delete all the records in the `Contact` table and restart the app to seed the database.</span></span> <span data-ttu-id="f8579-236">Visual Studio를 사용 하는 경우 끝내 고 시드 데이터베이스에 IIS Express를 다시 시작 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-236">If you are using Visual Studio, you need to exit and restart IIS Express to seed the database.</span></span>

<span data-ttu-id="f8579-237">연락처를 검색 하는 사용자를 등록 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-237">Register a user to browse the contacts.</span></span>

<span data-ttu-id="f8579-238">완성 된 앱을 테스트 하는 쉬운 방법은 세 가지 서로 다른 브라우저 (또는 incognito/InPrivate 버전)를 시작 하려면입니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-238">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="f8579-239">하나의 브라우저에서 새 사용자를 등록, 예를 들어 `test@example.com`합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-239">In one browser, register a new user, for example, `test@example.com`.</span></span> <span data-ttu-id="f8579-240">각 브라우저에 다른 사용자로 로그인 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-240">Sign in to each browser with a different user.</span></span> <span data-ttu-id="f8579-241">다음 사항을 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-241">Verify the following:</span></span>

* <span data-ttu-id="f8579-242">등록 된 사용자의 승인 된 모든 연락처 데이터를 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-242">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="f8579-243">등록 된 사용자 수 편집/삭제 자신의 데이터입니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-243">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="f8579-244">관리자 승인 하거나 연락 데이터를 거부할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-244">Managers can approve or reject contact data.</span></span> <span data-ttu-id="f8579-245">`Details` 보기 보여줍니다 **승인** 및 **거부** 단추입니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-245">The `Details` view shows **Approve** and **Reject** buttons.</span></span> 
* <span data-ttu-id="f8579-246">관리자 승인/거부 있으며 편집/삭제 된 데이터.</span><span class="sxs-lookup"><span data-stu-id="f8579-246">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="f8579-247">사용자</span><span class="sxs-lookup"><span data-stu-id="f8579-247">User</span></span>| <span data-ttu-id="f8579-248">옵션</span><span class="sxs-lookup"><span data-stu-id="f8579-248">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="f8579-249">수 편집/삭제 자체 데이터</span><span class="sxs-lookup"><span data-stu-id="f8579-249">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="f8579-250">승인/거부 및 편집/삭제를 데이터 소유할 수 있습니까</span><span class="sxs-lookup"><span data-stu-id="f8579-250">Can approve/reject and edit/delete own data</span></span>  |
| admin@contoso.com | <span data-ttu-id="f8579-251">편집/삭제 있고 모든 데이터를 승인/거부 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-251">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="f8579-252">관리자가 브라우저에서 연락처를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-252">Create a contact in the administrators browser.</span></span> <span data-ttu-id="f8579-253">Delete에 대 한 URL을 복사 하 고 관리자 연락처에서 편집 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-253">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="f8579-254">테스트 사용자는 이러한 작업을 수행할 수를 확인 하려면 테스트 사용자의 브라우저에 다음이 링크를 붙여 넣습니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-254">Paste these links into the test user's browser to verify the test user cannot perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="f8579-255">시작 응용 프로그램 만들기</span><span class="sxs-lookup"><span data-stu-id="f8579-255">Create the starter app</span></span>

<span data-ttu-id="f8579-256">시작 응용 프로그램을 만드는 다음이 지침을 따릅니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-256">Follow these instructions to create the starter app.</span></span>

* <span data-ttu-id="f8579-257">만들기는 **ASP.NET Core 웹 응용 프로그램** 를 사용 하 여 [Visual Studio 2017](https://www.visualstudio.com/) "ContactManager" 라는</span><span class="sxs-lookup"><span data-stu-id="f8579-257">Create an **ASP.NET Core Web Application** using [Visual Studio 2017](https://www.visualstudio.com/) named "ContactManager"</span></span>

  * <span data-ttu-id="f8579-258">응용 프로그램을 만들 **개별 사용자 계정**합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-258">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="f8579-259">네임 스페이스는 샘플에는 네임 스페이스를 사용 하 여 일치 하므로 "ContactManager" 이름을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-259">Name it "ContactManager" so your namespace will match the namespace use in the sample.</span></span>

* <span data-ttu-id="f8579-260">다음 추가 `Contact` 모델:</span><span class="sxs-lookup"><span data-stu-id="f8579-260">Add the following `Contact` model:</span></span>

  [!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="f8579-261">스 캐 폴드 된 `Contact` Entity Framework Core를 사용 하 여 모델 및 `ApplicationDbContext` 데이터 컨텍스트.</span><span class="sxs-lookup"><span data-stu-id="f8579-261">Scaffold the `Contact` model using Entity Framework Core and the `ApplicationDbContext` data context.</span></span> <span data-ttu-id="f8579-262">모든 스 캐 폴딩 기본값을 적용 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-262">Accept all the scaffolding defaults.</span></span> <span data-ttu-id="f8579-263">사용 하 여 `ApplicationDbContext` contact 테이블에 클래스 데이터 컨텍스트에 대 한 배치는 [Identity](xref:security/authentication/identity) 데이터베이스입니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-263">Using `ApplicationDbContext` for the data context class  puts the contact table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="f8579-264">참조 [모델 추가](xref:tutorials/first-mvc-app/adding-model) 자세한 정보에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-264">See [Adding a model](xref:tutorials/first-mvc-app/adding-model) for more information.</span></span>

* <span data-ttu-id="f8579-265">업데이트는 **ContactManager** 고정는 *Views/Shared/_Layout.cshtml* 에서 파일을 `asp-controller="Home"` 를 `asp-controller="Contacts"` 하므로 눌러는 **ContactManager** 링크 연락처 컨트롤러를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-265">Update the **ContactManager** anchor in the *Views/Shared/_Layout.cshtml* file from `asp-controller="Home"` to `asp-controller="Contacts"` so tapping the **ContactManager** link will invoke the Contacts controller.</span></span> <span data-ttu-id="f8579-266">원래 태그:</span><span class="sxs-lookup"><span data-stu-id="f8579-266">The original markup:</span></span>

```html
   <a asp-area="" asp-controller="Home" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

<span data-ttu-id="f8579-267">업데이트 된 태그:</span><span class="sxs-lookup"><span data-stu-id="f8579-267">The updated markup:</span></span>

```html
   <a asp-area="" asp-controller="Contacts" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

* <span data-ttu-id="f8579-268">초기 마이그레이션을 스 캐 폴드 하 고 데이터베이스를 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-268">Scaffold the initial migration and update the database</span></span>

```none
   dotnet ef migrations add initial
   dotnet ef database update
   ```

* <span data-ttu-id="f8579-269">만들기, 편집 및 연락처를 삭제 하 여 앱을 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-269">Test the app by creating, editing and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="f8579-270">데이터베이스 시드</span><span class="sxs-lookup"><span data-stu-id="f8579-270">Seed the database</span></span>

<span data-ttu-id="f8579-271">추가 `SeedData` 클래스는 *데이터* 폴더입니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-271">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="f8579-272">샘플을 다운로드 하는 경우 복사할 수 있습니다는 *SeedData.cs* 파일을 여 *데이터* 시작 프로젝트의 폴더입니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-272">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]

<span data-ttu-id="f8579-273">강조 표시 된 코드의 끝에 추가 된 `Configure` 에서 메서드는 *Startup.cs* 파일:</span><span class="sxs-lookup"><span data-stu-id="f8579-273">Add the highlighted code to the end of the `Configure` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]

<span data-ttu-id="f8579-274">응용 프로그램 데이터베이스 시드 있는지 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-274">Test that the app seeded the database.</span></span> <span data-ttu-id="f8579-275">Seed 메서드 DB 연락처의 모든 행이 없는 경우 실행 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-275">The seed method does not run if there are any rows in the contact DB.</span></span>

### <a name="create-a-class-used-in-the-tutorial"></a><span data-ttu-id="f8579-276">자습서에서 사용 되는 클래스 만들기</span><span class="sxs-lookup"><span data-stu-id="f8579-276">Create a class used in the tutorial</span></span>

* <span data-ttu-id="f8579-277">라는 폴더를 만듭니다 *권한 부여*합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-277">Create a folder named *Authorization*.</span></span>
* <span data-ttu-id="f8579-278">복사는 *Authorization\ContactOperations.cs* 완료 된 프로젝트 다운로드에서 파일 또는 다음 코드를 복사 합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-278">Copy the *Authorization\ContactOperations.cs* file from the completed project download, or copy the following code:</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="f8579-279">추가 리소스</span><span class="sxs-lookup"><span data-stu-id="f8579-279">Additional resources</span></span>

* <span data-ttu-id="f8579-280">[ASP.NET Core 권한 부여 랩](https://github.com/blowdart/AspNetAuthorizationWorkshop)합니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-280">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="f8579-281">이 자습서에 도입 된 보안 기능에이 랩 자세히 알아봅니다.</span><span class="sxs-lookup"><span data-stu-id="f8579-281">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="f8579-282">ASP.NET Core에서 권한 부여: 단순, 역할, 클레임 기반 및 사용자 지정</span><span class="sxs-lookup"><span data-stu-id="f8579-282">Authorization in ASP.NET Core : Simple, role, claims-based and custom</span></span>](index.md)
* [<span data-ttu-id="f8579-283">사용자 지정 정책 기반 권한 부여</span><span class="sxs-lookup"><span data-stu-id="f8579-283">Custom policy-based authorization</span></span>](policies.md)