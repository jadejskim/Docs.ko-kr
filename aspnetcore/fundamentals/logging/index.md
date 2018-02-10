---
title: "ASP.NET Core의 로깅"
author: ardalis
description: "ASP.NET Core의 로깅 프레임워크에 대해 알아봅니다. 기본 제공 로깅 공급자를 살펴보고 인기 있는 타사 공급자에 대해 알아봅니다."
manager: wpickett
ms.author: tdykstra
ms.date: 12/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/logging/index
ms.openlocfilehash: c8152b94311acb672e9810828b634c744cb46eae
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-logging-in-aspnet-core"></a>ASP.NET Core의 로깅 소개

작성자: [Steve Smith](https://ardalis.com/) 및 [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core는 다양한 로깅 공급자를 사용하는 로깅 API를 지원합니다. 기본 공급자를 통해 하나 이상의 대상에 로그를 보낼 수 있으며, 타사 로깅 프레임워크를 연결할 수 있습니다. 이 문서에서는 코드에 기본 제공 로깅 API 및 공급자를 사용하는 방법을 보여 줍니다.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[샘플 코드 보기 또는 다운로드](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2)([다운로드 방법](xref:tutorials/index#how-to-download-a-sample))

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[샘플 코드 보기 또는 다운로드](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample)([다운로드 방법](xref:tutorials/index#how-to-download-a-sample))

---

## <a name="how-to-create-logs"></a>로그를 만드는 방법

로그를 만들려면 [종속성 주입](xref:fundamentals/dependency-injection) 컨테이너에서 `ILogger` 개체를 가져옵니다.

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

그런 다음 해당 로거 개체에 대해 로깅 메서드를 호출합니다.

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

이 예에서는 *범주*로 `TodoController` 클래스를 사용하여 로그를 만듭니다. 범주는 [이 문서의 뒷부분](#log-category)에 설명되어 있습니다.

ASP.NET Core는 비동기 로거 메서드를 제공하지 않습니다. 비동기를 사용할 필요가 없도록 로깅 속도가 아주 빠르기 때문입니다. 로깅 속도가 빠르지 않은 경우 로깅 방식을 변경하는 방안을 고려해 보세요. 데이터 저장소가 느리면 로그 메시지를 빠른 저장소에 먼저 쓴 후 나중에 느린 저장소로 이동합니다. 예를 들어 다른 프로세스에서 읽고 지속하는 느린 저장소인 메시지 큐에 기록합니다.

## <a name="how-to-add-providers"></a>공급자를 추가하는 방법

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

로깅 공급자는 개발자가 `ILogger` 개체를 사용하여 만든 메시지를 가져와서 표시하거나 저장합니다. 예를 들어 콘솔 공급자는 콘솔에 메시지를 표시하고, Azure App Service 공급자는 메시지를 Azure BLOB 저장소에 저장할 수 있습니다.

공급자를 사용하려면 *Program.cs*에서 공급자의 `Add<ProviderName>` 확장 메서드를 호출합니다.

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

기본 프로젝트 템플릿을 사용하면 [CreateDefaultBuilder](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) 메서드를 사용하여 로깅할 수 있습니다.

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

로깅 공급자는 개발자가 `ILogger` 개체를 사용하여 만든 메시지를 가져와서 표시하거나 저장합니다. 예를 들어 콘솔 공급자는 콘솔에 메시지를 표시하고, Azure App Service 공급자는 메시지를 Azure BLOB 저장소에 저장할 수 있습니다.

공급자를 사용하려면 다음 예제처럼 NuGet 패키지를 설치하고 `ILoggerFactory` 인스턴스에 대한 공급자의 확장 메서드를 호출합니다.

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

ASP.NET Core [DI(종속성 주입](xref:fundamentals/dependency-injection))는 `ILoggerFactory` 인스턴스를 제공합니다. `AddConsole` 및 `AddDebug` 확장 메서드는 [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) 및 [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) 패키지에 정의됩니다. 각 확장 메서드는 `ILoggerFactory.AddProvider` 메서드를 호출하고, 공급자 인스턴스를 전달합니다. 

> [!NOTE]
> 이 문서의 응용 프로그램 예제에서는 `Startup` 클래스의 `Configure` 메서드에 로깅을 추가합니다. 먼저 실행되는 코드의 로그 출력을 가져오려면, 그 대신 `Startup` 클래스 생성자에서 로깅 공급자를 추가합니다. 

---

각 [기본 제공 로깅 공급자](#built-in-logging-providers)에 대한 정보와 [타사 로깅 공급자](#third-party-logging-providers)의 링크는 문서의 뒷부분에서 찾을 수 있습니다.

## <a name="sample-logging-output"></a>샘플 로깅 출력

이전 섹션에 나와 있는 샘플 코드를 사용하는 경우 명령줄에서 실행하면 콘솔에 로그가 표시됩니다. 다음은 콘솔 출력의 예입니다.

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 42.9286ms
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 148.889ms 404
```
 
이러한 로그는 `http://localhost:5000/api/todo/0`으로 이동하여 생성되었으며, 이는 이전 섹션에서 나온 두 `ILogger` 호출의 실행을 트리거합니다.

다음은 Visual Studio에서 응용 프로그램 예제를 실행하면 디버그 창에 표시되는 동일한 로그의 예입니다.

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

이전 섹션에서 `ILogger` 호출을 통해 생성된 로그는 "TodoApi.Controllers.TodoController"로 시작합니다. "Microsoft" 범주로 시작하는 로그는 ASP.NET Core에서 온 것입니다. ASP.NET Core 자체와 응용 프로그램 코드는 동일한 로깅 API와 동일한 로깅 공급자 사용합니다.

이 문서의 나머지 부분에서는 로깅에 대한 일부 세부 정보 및 옵션을 설명합니다.

## <a name="nuget-packages"></a>NuGet 패키지

`ILogger` 및 `ILoggerFactory` 인터페이스는 [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/)에 있으며, 두 인터페이스의 기본 구현은 [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/)에 있습니다.

## <a name="log-category"></a>로그 범주

*범주*는 개발자가 만드는 각 로그와 함께 포함됩니다. 개발자는 `ILogger` 개체를 만들 때 범주를 지정합니다. 모든 문자열을 범주로 사용할 수 있지만, 로그가 작성되는 클래스의 정규화된 이름을 사용해야 하는 규칙이 적용됩니다. "TodoApi.Controllers.TodoController"를 예로 들 수 있습니다.

범주를 문자열로 지정하거나 형식에서 범주가 파생되는 확장 메서드를 사용할 수 있습니다. 범주를 문자열로 지정하려면 아래와 같이 `ILoggerFactory` 인스턴스에 대해 `CreateLogger`를 호출합니다.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

대부분의 경우 다음 예제와 같이 `ILogger<T>`를 사용하는 편이 더 쉽습니다.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

이것은 정규화된 형식 이름 `T`를 사용하여 `CreateLogger`를 호출하는 것과 동일합니다.

## <a name="log-level"></a>로그 수준

로그를 쓸 때마다 [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel)을 지정합니다. 로그 수준은 심각도 또는 중요도 수준을 나타냅니다. 예를 들어 메서드가 정상적으로 종료되면 `Information` 로그를 쓰고, 메서드가 404 반환 코드를 반환하면 `Warning` 로그를 쓰고, 예기치 않은 예외를 catch하면 `Error` 로그를 씁니다.

다음 코드 예제에서 메서드 이름(예: `LogWarning`)은 로그 수준을 지정합니다. 첫 번째 매개 변수는 [Log event ID](#log-event-id)입니다. 두 번째 매개 변수는 [message template](#log-message-template)으로, 나머지 메서드 매개 변수에서 인수 값에 대한 자리 표시자를 제공합니다. 메서드 매개 변수는 이 문서의 뒷부분에 자세히 설명되어 있습니다.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

메서드 이름에 수준을 포함하는 로그 메서드는 [ILogger에 대한 확장 메서드](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions)입니다. 백그라운드에서 이러한 메서드는 `LogLevel` 매개 변수를 사용하는 `Log` 메서드를 호출합니다. 이러한 확장 메서드 중 하나를 호출하는 대신 `Log` 메서드를 직접 호출할 수 있지만, 구문이 비교적 복잡합니다. 자세한 내용은 [ILogger 인터페이스](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) 및 [로거 확장 소스 코드](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs)를 참조하세요.

ASP.NET Core는 다음 [로그 수준](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel)을 정의하며, 여기에 가장 낮은 심각도에서 가장 높은 심각도 순으로 정렬됩니다.

* 추적 = 0

  문제를 디버깅하는 개발자에게만 중요한 정보입니다. 이러한 메시지는 중요한 응용 프로그램 데이터를 포함할 수 있으므로 프로덕션 환경에서 사용하면 안 됩니다. *기본적으로 사용하지 않도록 설정됩니다.* 예: `Credentials: {"User":"someuser", "Password":"P@ssword"}`

* 디버그 = 1

  개발 및 디버깅하는 동안 단기적으로 유용한 정보입니다. 예: 로그 볼륨이 크기 때문에 문제를 해결하려는 경우 외에는 프로덕션 환경에서 `Entering method Configure with flag set to true.`일반적으로 수준 로그를 사용하지 않습니다`Debug`.

* 정보 = 2

  응용 프로그램의 일반적인 흐름을 추적합니다. 이러한 로그는 일반적으로 장기적인 가치가 있습니다. 예: `Request received for path /api/todo`

* 경고 = 3

  응용 프로그램 흐름에 비정상적인 또는 예기치 않은 이벤트가 있습니다. 응용 프로그램을 중지하지는 않지만 조사할 필요가 있는 오류 또는 기타 조건도 여기에 포함될 수 있습니다. 처리된 예외는 `Warning` 로그 수준을 사용하는 일반적인 장소입니다. 예: `FileNotFoundException for file quotes.txt.`

* 오류 = 4

  처리할 수 없는 오류 및 예외입니다. 이러한 메시지는 전체 응용 프로그램 오류가 아닌 현재 동작 또는 작업(예: 현재 HTTP 요청)의 오류를 의미합니다. 예제 로그 메시지: `Cannot insert record due to duplicate key violation.`

* 중요 = 5

  즉각적인 대응이 필요한 오류입니다. 예: 데이터 손실 시나리오, 디스크 공간 부족.

로그 수준을 사용하여 특정 저장 매체 또는 디스플레이 창에 기록되는 로그 출력의 양을 제어할 수 있습니다. 예를 들어 프로덕션 환경에서 `Information` 수준 이하의 모든 로그는 볼륨 데이터 저장소로 이동하고, `Warning` 수준 이상의 모든 로그는 가치 데이터 저장소로 이동할 수 있습니다. 개발하는 동안 심각도가 `Warning` 이상인 로그는 일반적으로 콘솔로 보냅니다. 문제를 해결해야 하는 상황이 오면 `Debug` 수준을 추가할 수 있습니다. 이 문서의 뒷부분에 나오는 [로그 필터링](#log-filtering) 섹션에서는 공급자가 처리하는 로그 수준을 제어하는 방법을 설명합니다.

ASP.NET Core 프레임워크는 프레임워크 이벤트에 대한 `Debug` 수준 로그를 씁니다. 이 문서 앞부분에 나온 로그 예제는 `Information` 수준 미만 로그를 제외했기 때문에 `Debug` 수준 로그가 하나도 표시되지 않았습니다. 다음은 콘솔 공급자에 대해 `Debug` 이상 로그를 표시하도록 구성된 응용 프로그램 예제를 실행할 경우의 콘솔 로그 예제입니다.

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:62555/api/todo/0
dbug: Microsoft.AspNetCore.Routing.Tree.TreeRouter[1]
      Request successfully matched the route with name 'GetTodo' and template 'api/Todo/{id}'.
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Update (TodoApi)' with id '089d59b6-92ec-472d-b552-cc613dfd625d' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Delete (TodoApi)' with id 'f3476abe-4bd9-4ad3-9261-3ead09607366' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action TodoApi.Controllers.TodoController.GetById (TodoApi)
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action method TodoApi.Controllers.TodoController.GetById (TodoApi), returned result Microsoft.AspNetCore.Mvc.NotFoundResult.
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 0.8788ms
dbug: Microsoft.AspNetCore.Server.Kestrel[9]
      Connection id "0HL6L7NEFF2QD" completed keep alive response.
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 2.7286ms 404
```

## <a name="log-event-id"></a>로그 이벤트 ID

로그를 쓸 때마다 *이벤트 ID*를 지정합니다. 샘플 앱은 로컬로 정의된 `LoggingEvents` 클래스를 사용하여 이 작업을 수행합니다.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

이벤트 ID는 로깅된 이벤트 집합을 서로 연결하는 데 사용할 수 있는 정수 값입니다. 예를 들어 장바구니에 항목 추가에 대한 로그는 이벤트 ID 1000이고 구매 완료에 대한 로그는 이벤트 ID 1001입니다.

로깅 출력에서 이벤트 ID는 공급자에 따라 필드에 저장되거나 텍스트 메시지에 포함될 수 있습니다. 디버그 공급자는 이벤트 ID를 표시하지 않지만, 콘솔 공급자는 범주 뒤에서 대괄호 안에 이벤트 ID를 표시합니다.

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a>로그 메시지 템플릿

로그 메시지를 쓸 때마다 메시지 템플릿을 제공합니다. 메시지 템플릿은 문자열일 수도 있고, 인수 값이 배치되는 명명된 자리 표시자를 포함할 수도 있습니다. 템플릿은 형식 문자열이 아니고, 자리 표시자는 번호가 아닌 이름이 지정되어야 합니다.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

값을 제공하는 데 사용되는 매개 변수를 결정하는 것은 자리 표시자의 번호가 아닌 이름입니다. 다음과 같은 코드를 가정해 봅니다.

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

결과로 만들어진 로그 메시지는 다음과 같습니다.

```
Parameter values: parm1, parm2
```

로깅 프레임워크는 이 방식으로 메시지를 포맷하여 로깅 공급자가 [구조적 로깅이라고도 하는 의미 체계 로깅](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)을 구현할 수 있게 해줍니다. 인수 자체는 포맷된 메시지 템플릿이 아닌 로깅 시스템으로 전달되기 때문에 로깅 공급자가 메시지 템플릿 외에도 매개 변수 값을 필드로 저장할 수 있습니다. 로그 출력을 Azure Table Storage로 전달하는 경우 로거 메서드 호출은 다음과 같습니다.

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

각 Azure Table 엔터티는 로그 데이터에 대한 쿼리를 간소화하는 `ID` 및 `RequestTime` 속성을 가질 수 있습니다. 문자 메시지의 시간 초과를 구문 분석할 필요 없이 특정 `RequestTime` 범위 내에서 모든 로그를 찾을 수 있습니다.

## <a name="logging-exceptions"></a>로깅 처리

로거 메서드는 다음 예제와 같이 예외를 전달할 수 있는 오버로드를 갖고 있습니다.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

공급자들은 다양한 방식으로 예외 정보를 처리합니다. 다음은 위에서 보여드린 코드의 디버그 공급자 출력의 예제입니다.

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>로깅 필터링

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

특정 공급자 및 범주 또는 모든 공급자나 모든 범주의 최소 로그 수준을 지정할 수 있습니다. 최소 수준 미만인 로그는 해당 공급자에게 전달되지 않으므로 표시되거나 저장되지 않습니다. 

모든 로그를 표시하지 않으려면 최소 로그 수준으로 `LogLevel.None`을 지정하면 됩니다. `LogLevel.None`의 정수 값은 `LogLevel.Critical`(5)보다 높은 6입니다.

**구성에서 필터 규칙 만들기**

프로젝트 템플릿은 콘솔 및 디버그 공급자에 대한 로깅을 설정하는 `CreateDefaultBuilder`를 호출하는 코드를 만듭니다. 또한 `CreateDefaultBuilder` 메서드는 다음과 같은 코드를 사용하여 `Logging` 섹션에서 구성을 조회하는 로깅을 설정합니다.

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

구성 데이터는 다음 예제와 같이 공급자 및 범주별로 최소 로그 수준을 지정합니다.

[!code-json[](index/sample2/appsettings.json)]

이 JSON은 6개의 필터 규칙을 만드는데, 하나는 디버그 공급자용이고, 4개는 콘솔 공급자용이고, 나머지 하나는 모든 공급자에 적용됩니다. `ILogger` 개체가 생성될 때 각 공급자에 대해 이러한 규칙 중 하나를 선택하는 방법은 나중에 살펴보겠습니다.

**코드의 필터 규칙**

다음 예제와 같이 코드에서 필터 규칙을 등록할 수 있습니다.

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

두 번째 `AddFilter`는 형식 이름을 사용하여 디버그 공급자를 지정합니다. 첫 번째 `AddFilter`는 공급자 형식을 지정하지 않으며, 따라서 모든 공급자에 적용됩니다.

**필터링 규칙 적용 방식**

이전 예제의 구성 데이터 및 `AddFilter` 코드는 다음 표에 표시된 규칙을 만듭니다. 처음 6개는 구성 예제에서 가져온 것이고 마지막 2개는 코드 예제에서 가져온 것입니다.

| 수 | 공급자      | 다음으로 시작하는 범주...          | 최소 로그 수준 |
| :----: | ------------- | --------------------------------------- | ----------------- |
| 1      | 디버그         | 모든 범주                          | 정보       |
| 2      | 콘솔       | Microsoft.AspNetCore.Mvc.Razor.Internal | 경고           |
| 3      | 콘솔       | Microsoft.AspNetCore.Mvc.Razor.Razor    | 디버그             |
| 4      | 콘솔       | Microsoft.AspNetCore.Mvc.Razor          | Error             |
| 5      | 콘솔       | 모든 범주                          | 정보       |
| 6      | 모든 공급자 | 모든 범주                          | 디버그             |
| 7      | 모든 공급자 | 시스템                                  | 디버그             |
| 8      | 디버그         | Microsoft                               | 추적             |

로그를 쓰는 `ILogger` 개체를 만들 때 `ILoggerFactory` 개체는 공급자마다 해당 로거에 적용할 단일 규칙을 선택합니다. `ILogger` 개체를 통해 작성된 모든 메시지는 선택한 규칙을 기반으로 필터링됩니다. 사용 가능한 규칙 중에서 각 공급자 및 범주 쌍에 적용 가능한 가장 구체적인 규칙이 선택됩니다.

지정된 범주에 대한 `ILogger`가 생성되면 다음 알고리즘이 각 공급자에 사용됩니다.

* 공급자 또는 공급자의 별칭과 일치하는 모든 규칙을 선택합니다. 일치 항목이 없는 경우 빈 공급자와 함께 모든 규칙을 선택합니다.
* 앞 단계의 결과에서 일치하는 범주 접두사가 가장 긴 규칙을 선택합니다. 일치 항목이 없는 경우 범주를 지정하지 않는 규칙을 모두 선택합니다.
* 여러 규칙을 선택하는 경우 **마지막** 규칙을 사용합니다.
* 규칙을 선택하지 않는 경우 `MinimumLevel`을 사용합니다.
 
예를 들어 이전 단계의 규칙 목록을 갖고 있고 "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" 범주에 대한 `ILogger` 개체를 만든다고 가정해 봅시다.

* 디버그 공급자에게는 규칙 1, 6, 8이 적용됩니다. 규칙 8이 가장 구체적이므로 선택됩니다.
* 콘솔 공급자에게는 규칙 3, 4, 5, 6이 적용됩니다. 규칙 3이 가장 구체적입니다.

`ILogger`를 사용하여 "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" 범주에 대한 로그를 만들 때 수준 `Trace` 이상의 로그는 디버그 공급자로 이동하고 수준 `Debug` 이상의 로그는 콘솔 공급자로 이동합니다.

**공급자 별칭**

형식 이름을 사용하여 구성에서 공급자를 지정할 수 있지만, 각 공급자는 길이가 짧아 사용하기 간편한 *별칭*을 정의합니다. 기본 공급자의 경우 다음 별칭을 사용합니다.

- 콘솔
- 디버그
- EventLog
- AzureAppServices
- TraceSource
- EventSource

**기본 최소 수준**

특정 공급자 및 범주에 대한 구성 또는 코드의 규칙이 적용되지 않는 경우에만 효력이 있는 최소 수준 설정이 있습니다. 다음 예제는 최소 수준을 설정하는 방법을 보여 줍니다.

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

최소 수준을 명시적으로 설정하지 않으면 `Trace` 및 `Debug` 로그를 무시하는 `Information`이 기본값으로 설정됩니다.

**필터 함수**

필터 함수에서 필터링 규칙을 적용할 코드를 작성할 수 있습니다. 필터 함수는 구성 또는 코드를 통해 규칙이 할당되지 않은 모든 공급자와 범주에 대해 호출됩니다. 함수의 코드는 공급자 형식, 범주 및 로그 수준에 액세스하여 메시지를 기록할 것인지 여부를 결정할 수 있습니다. 예:

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

일부 로깅 공급자는 로그 수준 및 범주에 따라 로그를 저장 매체에 기록할 것인지 아니면 무시할 것인지를 개발자가 지정할 수 있게 해줍니다.

`AddConsole` 및 `AddDebug` 확장 메서드는 필터링 조건을 전달할 수 있는 오버로드를 제공합니다. 다음 샘플 코드는 콘솔 공급자가 `Warning` 수준 미만의 로그를 무시하게 만들고, 디버그 공급자는 프레임워크에서 만드는 로그를 무시합니다.

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

`AddEventLog` 메서드는 `EventLogSettings` 인스턴스를 사용하는 오버로드를 갖고 있으며, 이 인스턴스의 `Filter` 속성에는 필터링 함수가 포함될 수 있습니다. TraceSource 공급자는 오버로드를 제공하지 않습니다. 로깅 수준 및 기타 매개 변수가 사용하는 `SourceSwitch` 및 `TraceListener`를 기반으로 하기 때문입니다.

`WithFilter` 확장 메서드를 사용하여 `ILoggerFactory` 인스턴스에 등록된 모든 공급자에 대한 필터링 규칙을 설정할 수 있습니다. 아래 예제는 프레임워크 로그(범주가 "Microsoft" 또는 "시스템"으로 시작)를 경고로 제한하고 디버그 수준에서 앱 로그를 허용합니다.

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

필터링을 사용하여 특정 범주에 대한 로그를 기록하지 못하게 하려면 해당 범주의 최소 로그 수준으로 `LogLevel.None`을 지정하면 됩니다. `LogLevel.None`의 정수 값은 `LogLevel.Critical`(5)보다 높은 6입니다.

`WithFilter` 확장 메서드는 [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet 패키지에서 제공합니다. 이 메서드는 등록된 모든 로거 공급자로 전달되는 메시지를 필터링하는 새로운 `ILoggerFactory` 인스턴스를 반환합니다. 원래 `ILoggerFactory` 인스턴스를 포함하여 어떤 `ILoggerFactory` 인스턴스에도 영향을 주지 않습니다.

---

## <a name="log-scopes"></a>로그 점수

*범위* 내의 논리적 작업 집합을 그룹화하여 동일한 데이터를 해당 집합의 일부로 생성된 각 로그에 연결할 수 있습니다. 예를 들어 트랜잭션 처리의 일부로 생성되는 모든 로그가 트랜잭션 ID를 포함하게 만들 수 있습니다.

범위는 `ILogger.BeginScope<TState>` 메서드에서 반환하는 `IDisposable` 형식이며 삭제될 때까지 유지됩니다. 다음과 같이 로거 호출을 `using` 블록에 래핑하여 범위를 사용합니다.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

다음은 콘솔 공급자에 대한 범위를 사용하도록 설정하는 코드입니다.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

*Program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> 범위 기반 로깅을 사용하려면 `IncludeScopes` 콘솔 로거 옵션을 구성해야 합니다. *appsettings* 구성 파일을 사용하여 `IncludeScopes`를 구성하는 기능은 ASP.NET Core 2.1 릴리스에서 제공될 예정입니다.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

*Startup.cs*:

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

각 로그 메시지는 범위 정보를 포함하고 있습니다.

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a>기본 로깅 공급자

ASP.NET Core는 다음 공급자를 제공합니다.

* [콘솔](#console)
* [디버그](#debug)
* [EventSource](#eventsource)
* [EventLog](#eventlog)
* [TraceSource](#tracesource)
* [Azure App Service](#appservice)

<a id="console"></a>
### <a name="the-console-provider"></a>콘솔 공급자

[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) 공급자 패키지는 콘솔에 로그 출력을 보냅니다. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddConsole()
```

[AddConsole 오버로드](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions)를 사용하여 최소 로그 수준, 필터 함수 및 범위 지원 여부를 나타내는 부울을 전달할 수 있습니다. 다른 옵션으로는 `IConfiguration` 개체를 전달할 수 있으며, 이 개체는 범위 지원 및 로깅 수준을 지정할 수 있습니다. 

프로덕션 환경에서 콘솔 공급자를 사용하려고 생각 중인 경우 성능에 상당한 영향이 있다는 점에 주의해야 합니다.

Visual Studio에서 새 프로젝트를 만들면 `AddConsole` 메서드는 다음과 같습니다.

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

이 코드는 *appSettings.json* 파일의 `Logging` 섹션을 참조합니다.

[!code-json[](index/sample//appsettings.json)]

[로그 필터링](#log-filtering) 섹션에서 설명했듯이, 제한 프레임워크를 표시한 설정은 경고에 기록되는 한편 앱이 디버그 수준에서 기록하는 것을 허용합니다. 자세한 내용은 [구성](xref:fundamentals/configuration/index)을 참조하세요.

---

<a id="debug"></a>
### <a name="the-debug-provider"></a>디버그 공급자

[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) 공급자 패키지는 [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) 클래스(`Debug.WriteLine` 메서드 호출)를 사용하여 로그 출력을 씁니다.

Linux에서 이 공급자는 */var/log/message*에 로그를 씁니다.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

[AddDebug 오버로드](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions)를 사용하여 최소 수준 로그 또는 필터 함수를 전달할 수 있습니다.

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a>EventSource 공급자

ASP.NET Core 1.1.0을 대상으로 하는 앱의 경우 [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) 공급자 패키지가 이벤트 추적을 구현할 수 있습니다. Windows에서는 [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803)를 사용합니다. 플랫폼 간 공급자이지만 이벤트를 수집하지 않으며 Linux 또는 macOS용 도구를 표시합니다. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

로그를 수집하고 보는 좋은 방법은 [PerfView 유틸리티](https://www.microsoft.com/download/details.aspx?id=28567)를 사용하는 것입니다. ETW 로그를 보는 다른 도구가 있지만, PerfView는 ASP.NET에서 내보내는 ETW 이벤트를 처리하기에 가장 좋은 환경을 제공합니다. 

이 공급자가 기록한 이벤트를 수집하도록 PerfView를 구성하려면 **추가 공급자** 목록에 `*Microsoft-Extensions-Logging` 문자열을 추가합니다. (문자열의 시작 부분에 별표를 누락하지 마세요.)

![Perfview 추가 공급자](index/_static/perfview-additional-providers.png)

Nano Server에서 이벤트를 캡처하려면 몇 가지 추가 설정이 필요합니다.

* PowerShell 원격 기능을 Nano Server에 연결합니다.

  ```powershell
  Enter-PSSession [name]
  ```

* ETW 세션을 만합니다.

  ```powershell
  New-EtwTraceSession -Name "MyAppTrace" -LocalFilePath C:\trace.etl
  ```

* 필요에 따라 [CLR](https://docs.microsoft.com/dotnet/framework/performance/clr-etw-providers), ASP.NET Core 등에 대한 ETW 공급자를 추가합니다. ASP.NET Core 공급자 GUID는 `3ac73b97-af73-50e9-0822-5da4367920d0`입니다. 

  ```powershell
  Add-EtwTraceProvider -Guid "{e13c0d23-ccbc-4e12-931b-d9cc2eee27e4}" -SessionName MyAppTrace
  Add-EtwTraceProvider -Guid "{3ac73b97-af73-50e9-0822-5da4367920d0}" -SessionName MyAppTrace
  ```

* 사이트를 실행하고 추적 정보에 대한 원하는 작업을 수행합니다.

* 작업을 마쳤으면 추적 세션을 중지합니다.

  ```powershell
  Stop-EtwTraceSession -Name "MyAppTrace"
  ```

다른 Windows 버전에서 하듯이, 실행 결과로 얻은 *C:\trace.etl* 파일을 PerfView로 분석할 수 있습니다.

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a>Windows EventLog 공급자

[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) 공급자 패키지는 Windows 이벤트 로그에 로그 출력을 보냅니다.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

[AddEventLog 오버로드](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions)를 사용하여 `EventLogSettings` 또는 최소 로그 수준을 전달할 수 있습니다.

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a>TraceSource 공급자

[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) 공급자 패키지는 [System.Diagnostics.TraceSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracesource) 라이브러리 및 공급자를 사용합니다.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

[AddTraceSource 오버로드](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions)를 사용하여 원본 스위치 및 추적 수신기를 전달할 수 있습니다.

이 공급자를 사용하려면 응용 프로그램이 .NET Framework(.NET Core가 아닌)에서 실행되어야 합니다. 공급자를 통해 응용 프로그램 예제에 사용된 [TextWriterTraceListener](https://docs.microsoft.com/dotnet/api/system.diagnostics.textwritertracelistenerr)처럼 다양한 [수신기](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/trace-listeners)로 메시지를 라우팅할 수 있습니다.

다음은 `Warning` 이상 메시지를 콘솔 창에 기록하는 `TraceSource` 공급자를 구성하는 예제입니다.

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a>Azure App Service 공급자

[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) 공급자 패키지는 Azure App Service 앱의 파일 시스템과 Azure Storage 계정의 [BLOB 저장소](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage)에 텍스트 파일을 기록합니다. 공급자는 ASP.NET Core 1.1.0을 대상으로 하는 앱에만 사용할 수 있습니다. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

.NET Core를 대상으로 지정하는 경우 공급자 패키지를 설치하거나 명시적으로 `AddAzureWebAppDiagnostics`을 호출하지 않아도 됩니다. Azure App Service에 앱을 배포하면 자동으로 앱에 공급자가 제공됩니다.

.NET Framework를 대상으로 지정하는 경우 패키지 공급자를 프로젝트에 추가하고 `AddAzureWebAppDiagnostics`을 호출합니다.

```csharp
logging.AddAzureWebAppDiagnostics();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

`AddAzureWebAppDiagnostics` 오버로드를 사용하여 로깅 출력 템플릿, BLOB 이름, 파일 크기 제한 등의 기본 설정을 재정의하는 데 사용할 수 있는 [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs)를 전달할 수 있습니다. (*출력 템플릿*은 `ILogger` 메서드를 호출할 때 제공하는 로그를 기반으로 모든 로그에 적용되는 메시지 템플릿입니다.)

---

App Service 앱에 배포할 때 응용 프로그램은 Azure Portal **App Service** 페이지의 [진단 로그](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) 섹션에 있는 설정을 따릅니다. 이러한 설정을 변경하면 앱을 다시 시작하거나 코드를 다시 배포할 필요 없이 변경 내용이 즉시 적용됩니다. 

![Azure 로깅 설정](index/_static/azure-logging-settings.png)

로그 파일의 기본 위치는 *D:\\home\\LogFiles\\Application* 폴더이며, 기본 파일 이름은 *diagnostics-yyyymmdd.txt*입니다. 기본 파일 크기 제한은 10MB이고, 보존되는 기본 최대 파일 수는 2입니다. 기본 BLOB 이름은 *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*입니다. 기본 동작에 대한 자세한 내용은 [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs)를 참조하세요.

공급자는 프로젝트가 Azure 환경에서 실행되는 경우에만 작동합니다. 로컬로 실행하는 경우에는 아무 영향도 없습니다. Blob에 대한 로컬 파일 또는 로컬 개발 저장소에 기록하지 않습니다.

## <a name="third-party-logging-providers"></a>타사 로깅 공급자

다음은 ASP.NET Core와 호환되는 일부 타사 로깅 프레임워크입니다.

* [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) - Elmah.Io 서비스의 공급자

* [JSNLog](http://jsnlog.com) - 서버 쪽 로그에 JavaScript 예외 및 기타 클라이언트 쪽 이벤트를 기록합니다.

* [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) - Loggr 서비스의 공급자

* [NLog](https://github.com/NLog/NLog.Extensions.Logging) - NLog 라이브러리의 공급자

* [Serilog](https://github.com/serilog/serilog-extensions-logging) - Serilog 라이브러리의 공급자

일부 타사 프레임워크는 [구조적 로깅이라고도 하는 의미 체계 로깅](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)을 수행할 수 있습니다.

타사 프레임워크를 사용하는 방법은 기본 공급자 중 하나를 사용하는 방법과 비슷합니다. 프로젝트에 NuGet 패키지를 추가하고 `ILoggerFactory`에 대한 확장 메서드를 호출하면 됩니다. 자세한 내용은 각 프레임워크의 설명서를 참조하십시오.

다른 로깅 프레임워크 또는 개발자 고유의 로깅 요구 사항을 지원하는 사용자 지정 공급자를 만들 수도 있습니다.

## <a name="azure-log-streaming"></a>Azure 로그 스트리밍

Azure 로그 스트리밍을 통해 로그 작업을 실시간으로 볼 수 있습니다. 

* 응용 프로그램 서버 
* 웹 서버
* 실패한 요청 추적 

Azure 로그 스트리밍을 구성하려면: 

* 응용 프로그램의 포털 페이지에서 **진단 로그** 페이지로 이동합니다.
* **응용 프로그램 로깅(파일 시스템)**을 켭니다. 

![Azure Portal 진단 로그 페이지](index/_static/azure-diagnostic-logs.png)

**로그 스트리밍** 페이지로 이동하여 응용 프로그램 메시지를 봅니다. 응용 프로그램이 `ILogger` 인터페이스를 통해 기록한 것입니다. 

![Azure Portal 응용 프로그램 로그 스트리밍](index/_static/azure-log-streaming.png)


## <a name="see-also"></a>참고 항목

[LoggerMessage를 사용한 고성능 로깅](xref:fundamentals/logging/loggermessage)
