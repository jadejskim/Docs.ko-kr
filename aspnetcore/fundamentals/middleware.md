---
title: "ASP.NET Core 미들웨어"
author: rick-anderson
description: "ASP.NET Core 미들웨어 및 요청 파이프라인에 대해 알아봅니다."
manager: wpickett
ms.author: riande
ms.date: 01/22/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/middleware
ms.openlocfilehash: 697a3a8e475dda0d48a11dbfad8fbb0603aa1bf8
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-core-middleware-fundamentals"></a>ASP.NET Core 미들웨어 기본 사항

<a name="fundamentals-middleware"></a>

작성자: [Rick Anderson](https://twitter.com/RickAndMSFT) 및 [Steve Smith](https://ardalis.com/)

[샘플 코드 보기 또는 다운로드](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)([다운로드 방법](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-middleware"></a>미들웨어란?

미들웨어는 요청 및 응답을 처리하는 응용 프로그램 파이프라인으로 어셈블리되는 소프트웨어입니다. 각 구성 요소:

* 요청을 파이프라인의 다음 구성 요소로 전달할지 여부를 선택합니다.
* 파이프라인의 다음 구성 요소가 호출되기 전과 후에 작업을 수행할 수 있습니다. 

요청 대리자는 요청 파이프라인을 빌드하는 데 사용됩니다. 요청 대리자는 각 HTTP 요청을 처리합니다.

요청 대리자는 [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) 및 [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) 확장 메서드를 사용하여 구성됩니다. 개별 요청 대리자는 무명 메서드(인라인 미들웨어라고 함)로 인라인에서 지정되거나 다시 사용할 수 있는 클래스에서 정의될 수 있습니다. 이러한 다시 사용할 수 있는 클래스 및 인라인 무명 메서드는 *미들웨어* 또는 *미들웨어 구성 요소*입니다. 요청 파이프라인의 각 미들웨어 구성 요소는 파이프라인의 다음 구성 요소를 호출하거나 적절한 경우 체인을 단락(short-circuiting)하는 일을 담당합니다.

[HTTP 모듈을 미들웨어로 마이그레이션](../migration/http-modules.md)에서는 ASP.NET Core와 이전 버전의 요청 파이프라인 간의 차이점을 설명하고 더 많은 미들웨어 샘플을 제공합니다.

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a>IApplicationBuilder로 미들웨어 파이프라인 만들기

ASP.NET Core 요청 파이프라인은 이 다이어그램이 보여 주는 것과 같이 차례로 호출되는 요청 대리자의 시퀀스로 구성됩니다(실행의 스레드는 검은색 화살표를 따름).

![요청 수신을 표시하고, 세 가지 미들웨어를 통해 처리하는 요청 처리 패턴 및 응용 프로그램을 나가는 응답 각 미들웨어는 해당 논리를 실행하고 next() 문에서 다음 미들웨어에 대한 요청을 전달합니다. 세 번째 미들웨어가 요청을 처리한 후, 클라이언트에 대한 응답으로 응용 프로그램을 나가기 전에 각각 차례대로 next() 문 후에 추가 처리에 대한 이전 미들웨어 두 개를 통해 다시 전달됩니다.](middleware/_static/request-delegate-pipeline.png)

각 대리자는 다음 대리자 전과 후에 작업을 수행할 수 있습니다. 또한 대리자는 다음 대리자에 요청을 전달하지 않도록 결정할 수 있습니다. 이를 요청 파이프라인을 단락(short-circuiting)한다고 합니다. 단락(short-circuiting)은 불필요한 작업을 방지하기 때문에 보통 바람직합니다. 예를 들어 정적 파일 미들웨어는 정적 파일에 대한 요청을 반환하고 나머지 파이프라인을 단락(short-circuit)할 수 있습니다. 예외 처리 대리자는 파이프라인의 이후 단계에서 발생하는 예외를 catch할 수 있도록 파이프라인의 초기에 호출되어야 합니다.

가장 간단한 가능한 ASP.NET Core 앱은 모든 요청을 처리하는 단일 요청 대리자를 설정합니다. 이 경우 실제 요청 파이프라인은 포함하지 않습니다. 대신, 단일 익명 함수가 모든 HTTP 요청에 대한 응답에 호출됩니다.

[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]

첫 번째 [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) 대리자는 파이프라인을 종료합니다.

[app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions)를 사용하여 여러 요청 대리자를 함께 연결할 수 있습니다. `next` 매개 변수는 파이프라인의 다음 대리자를 나타냅니다. (*next* 매개 변수를 호출하지 *않고* 파이프라인을 단락(short-circuit)할 수 있습니다.) 이 예제에서 설명하듯이 일반적으로 다음 대리자 전과 후 모두에서 작업을 수행할 수 있습니다.

[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> 클라이언트에 응답을 전송한 후에 `next.Invoke`를 호출하지 마십시오. 응답이 시작된 후 `HttpResponse`로 변경하면 예외를 throw합니다. 예를 들어 헤더, 상태 코드 등을 설정하는 것 같은 변경은 예외를 throw합니다. `next`를 호출한 후 응답 본문에 작성하기:
> - 프로토콜 위반이 발생할 수 있습니다. 예를 들어 언급된 `content-length`보다 더 많이 작성하기.
> - 본문 형식을 손상시킬 수 있습니다. 예를 들어 CSS 파일에 HTML 바닥글 작성하기.
>
> [HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted)는 헤더가 전송되고 또는 본문이 작성되었는지 나타내는 데 유용한 힌트입니다.

## <a name="ordering"></a>정렬

미들웨어 구성 요소가 `Configure` 메서드에 추가되는 순서는 요청에서 호출되는 순서와 응답에 대한 역순서를 정의합니다. 이 순서 지정은 보안, 성능 및 기능에 중요합니다.

Configure 메서드(아래 참조)는 다음 미들웨어 구성 요소를 추가합니다.

1. 예외/오류 처리
2. 정적 파일 서버
3. 인증
4. MVC

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)


```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseAuthentication();               // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseIdentity();                     // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

-----------

위의 코드에서 `UseExceptionHandler`는 파이프라인에 추가된 첫 번째 미들웨어 구성 요소이므로 후속 호출에서 발생하는 모든 예외를 catch합니다.

정적 파일 미들웨어는 파이프라인 초기에 호출되므로 요청을 처리하고 나머지 구성 요소를 통과하지 않고 단락(short-circuit)할 수 있습니다. 정적 파일 미들웨어는 권한 부여 검사를 제공하지 **않습니다**. *wwwroot* 아래의 항목을 비롯한 제공되는 모든 파일은 공개적으로 사용할 수 있습니다. 정적 파일을 보호하는 방법은 [정적 파일 작업](xref:fundamentals/static-files)을 참조하세요.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)


요청이 정적 파일 미들웨어에서 처리되지 않는 경우 인증을 수행하는 ID 미들웨어(`app.UseAuthentication`)로 전달됩니다. ID는 인증되지 않은 요청을 단락(short-circuit)하지 않습니다. ID가 요청을 인증하지만 MVC가 특정 Razor 페이지 또는 컨트롤러 및 작업을 선택한 후에만 권한 부여(및 거부)가 발생합니다.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

요청이 정적 파일 미들웨어에서 처리되지 않는 경우 인증을 수행하는 ID 미들웨어(`app.UseIdentity`)로 전달됩니다. ID는 인증되지 않은 요청을 단락(short-circuit)하지 않습니다. ID가 요청을 인증하지만 MVC가 특정 컨트롤러 및 작업을 선택한 후에만 권한 부여(및 거부)가 발생합니다.

-----------

다음 예제는 정적 파일에 대한 요청이 응답 압축 미들웨어 전에 정적 파일 미들웨어에서 처리되는 미들웨어 순서를 설명합니다. 정적 파일은 이 미들웨어의 순서로 압축되지 않습니다. [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_)의 MVC 응답은 압축될 수 있습니다.

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name="middleware-run-map-use"></a>

### <a name="use-run-and-map"></a>Use, Run 및 Map

`Use`, `Run` 및 `Map`을 사용하여 HTTP 파이프라인을 구성합니다. `Use` 메서드는 파이프라인을 단락(short-circuit)할 수 있습니다(즉, `next` 요청 대리자를 호출하지 않는 경우). `Run`은 규칙이며 일부 미들웨어 구성 요소는 파이프라인의 끝에서 실행되는 `Run[Middleware]` 메서드를 노출할 수 있습니다.

`Map*` 확장은 파이프라인 분기에 규칙으로 사용됩니다. [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions)은 지정된 요청 경로의 일치를 기반으로 요청 파이프라인을 분기합니다. 요청 경로가 지정된 경로로 시작하는 경우 분기가 실행됩니다.

[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]

다음 표는 앞의 코드를 사용하여 `http://localhost:1234`의 요청 및 응답을 보여 줍니다.

| 요청 | 응답 |
| --- | --- |
| localhost:1234 | Hello from non-Map delegate.  |
| localhost:1234/map1 | Map Test 1 |
| localhost:1234/map2 | Map Test 2 |
| localhost:1234/map3 | Hello from non-Map delegate.  |

`Map`이 사용되는 경우 일치하는 경로 세그먼트는 `HttpRequest.Path`에서 제거되고 각 요청에 대해 `HttpRequest.PathBase`에 추가됩니다.

[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions)은 지정된 조건자의 결과를 기반으로 요청 파이프라인을 분기합니다. `Func<HttpContext, bool>` 형식의 조건자는 파이프라인의 새 분기에 요청을 매핑하는 데 사용될 수 있습니다. 다음 예제에서 조건자는 쿼리 문자열 변수 `branch`의 존재를 검색하는 데 사용됩니다.

[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]

다음 표는 앞의 코드를 사용하여 `http://localhost:1234`의 요청 및 응답을 보여 줍니다.

| 요청 | 응답 |
| --- | --- |
| localhost:1234 | Hello from non-Map delegate.  |
| localhost:1234/?branch=master | Branch used = master|

`Map`은 중첩을 지원합니다. 예를 들면 다음과 같습니다.

```csharp
app.Map("/level1", level1App => {
       level1App.Map("/level2a", level2AApp => {
           // "/level1/level2a"
           //...
       });
       level1App.Map("/level2b", level2BApp => {
           // "/level1/level2b"
           //...
       });
   });
   ```

`Map`은 여러 세그먼트를 한 번에 일치시킬 수도 있습니다. 예를 들면 다음과 같습니다.

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a>기본 제공 미들웨어

ASP.NET Core는 다음 미들웨어 구성 요소 및 추가되어야 하는 순서에 대한 설명과 함께 제공됩니다.

| 미들웨어 | 설명 | 순서 |
| ---------- | ----------- | ----- |
| [인증](xref:security/authentication/identity) | 인증 지원을 제공합니다. | `HttpContext.User`가 필요하기 전에. OAuth 콜백에 대한 터미널. |
| [CORS](xref:security/cors) | 원본 간 리소스 공유를 구성합니다. | CORS를 사용하는 구성 요소 이전. |
| [진단](xref:fundamentals/error-handling) | 진단을 구성합니다. | 오류를 생성하는 구성 요소 이전. |
| [ForwardedHeaders/HttpOverrides](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | 프록시된 헤더를 현재 요청에 전달합니다. | 업데이트된 필드를 사용하는 구성 요소 이전(예: 체계, 호스트, ClientIP, 메서드). |
| [응답 캐싱](xref:performance/caching/middleware) | 응답 캐시에 대한 지원을 제공합니다. | 캐싱이 필요한 구성 요소 이전. |
| [응답 압축](xref:performance/response-compression) | 응답 압축에 대한 지원을 제공합니다. | 압축이 필요한 구성 요소 이전. |
| [RequestLocalization](xref:fundamentals/localization) | 지역화 지원을 제공합니다. | 지역화 구분 구성 요소 이전. |
| [라우팅](xref:fundamentals/routing) | 요청 경로를 정의하고 제한합니다. | 경로 일치에 대한 터미널. |
| [세션](xref:fundamentals/app-state) | 사용자 세션 관리에 대한 지원을 제공합니다. | 세션이 필요한 구성 요소 이전. |
| [정적 파일](xref:fundamentals/static-files) | 정적 파일 및 디렉터리 검색 처리에 대한 지원을 제공합니다. | 요청이 파일과 일치하는 경우 터미널. |
| [URL 재작성](xref:fundamentals/url-rewriting) | URL 재작성 및 요청 리디렉션에 대한 지원을 제공합니다. | URL을 사용하는 구성 요소 이전. |
| [WebSockets](xref:fundamentals/websockets) | WebSocket 프로토콜을 활성화합니다. | WebSocket 요청을 수락하는 데 필요한 구성 요소 이전. |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a>미들웨어 작성

미들웨어는 일반적으로 클래스에서 캡슐화되고 확장 메서드로 노출됩니다. 쿼리 문자열에서 현재 요청에 대한 문화권을 설정하는 다음 미들웨어를 고려합니다.

[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]

참고: 위의 샘플 코드는 미들웨어 구성 요소 만들기를 보여 주는 데 사용됩니다. ASP.NET Core의 기본 제공 지역화 지원은 [전역화 및 지역화](xref:fundamentals/localization)를 참조하세요.

문화권을 전달하여 미들웨어를 테스트할 수 있습니다(예: `http://localhost:7997/?culture=no`).

다음 코드는 미들웨어 대리자를 클래스로 이동합니다.

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]

다음 확장 메서드는 [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder)를 통해 미들웨어를 공개합니다.

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

다음 코드는 `Configure`에서 미들웨어를 호출합니다.

[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

미들웨어는 해당 생성자에서 해당 종속성을 노출하여 [명시적 종속성 원칙](http://deviq.com/explicit-dependencies-principle/)을 따라야 합니다. 미들웨어는 *응용 프로그램 수명*당 한 번 생성됩니다. 서비스를 요청 내의 미들웨어와 공유해야 하는 경우 아래 *요청당 종속성*을 참조하세요.

미들웨어 구성 요소는 생성자 매개 변수를 통해 종속성 주입에서 해당 종속성을 확인할 수 있습니다. [`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary)는 추가 매개 변수를 직접 수락할 수도 있습니다.

### <a name="per-request-dependencies"></a>요청당 종속성

미들웨어는 요청당이 아닌 앱 시작 시 생성되므로 미들웨어 생성자에 의해 사용되는 *범위가 지정된* 수명 서비스는 각 요청 중에 다른 종속성 주입된 형식과 공유되지 않습니다. *범위가 지정된* 서비스를 미들웨어와 다른 형식 간에 공유해야 하는 경우 이러한 서비스를 `Invoke` 메서드의 서명에 추가합니다. `Invoke` 메서드는 종속성 주입으로 채워지는 추가 매개 변수를 수락할 수 있습니다. 예:

```c#
public class MyMiddleware
{
    private readonly RequestDelegate _next;

    public MyMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="resources"></a>리소스

* [이 문서에 사용되는 샘플 코드](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)
* [HTTP 모듈을 미들웨어로 마이그레이션](../migration/http-modules.md)
* [응용 프로그램 시작](startup.md)
* [기능 요청](request-features.md)
