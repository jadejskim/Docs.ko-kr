---
title: "ASP.NET Core 응용 프로그램에서 SSL을 강제 적용"
author: rick-anderson
description: "웹 응용 프로그램에서 ASP.NET Core SSL을 요구 하는 방법을 보여 줍니다."
manager: wpickett
ms.author: riande
ms.date: 07/19/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 3b72cddb7a240ad6d6e1427796e9bb4f7003a3f7
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/03/2018
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a>ASP.NET Core 응용 프로그램에서 SSL을 강제 적용

작성자: [Rick Anderson](https://twitter.com/RickAndMSFT)

이 문서에서는 표시 하는 방법:

- 모든 요청 (HTTPS 요청에만 해당)에 대 한 SSL이 필요 합니다.
- HTTPS에 대 한 모든 HTTP 요청을 리디렉션하십시오.

## <a name="require-ssl"></a>SSL 필요

[RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) ssl을 사용 하는 데 사용 됩니다. 컨트롤러 또는이 특성을 사용 하 여 메서드를 데코레이팅 할 수 있습니다 하거나 아래와 같이 전체적으로 적용할 수 있습니다.

다음 코드를 추가 `ConfigureServices` 에 `Startup`:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

위의 강조 표시 된 코드에서는 모든 요청 사용 `HTTPS`, 따라서 HTTP 요청은 무시 됩니다. 다음 강조 표시 된 코드를 HTTPS로 모든 HTTP 요청을 리디렉션합니다.

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

참조 [URL 다시 쓰기 미들웨어](xref:fundamentals/url-rewriting) 자세한 정보에 대 한 합니다.

HTTPS를 전역적으로 요구 (`options.Filters.Add(new RequireHttpsAttribute());`) 보안 모범 사례입니다. 적용 된 `[RequireHttps]` 모든 컨트롤러에는 특성으로 전체적으로 HTTPS를 필요로 하는 컨트롤로 안전 하다 고 간주 되지 않습니다. 보장할 수 없습니다 적용할 저장 되므로 응용 프로그램에 추가 하는 새로운 컨트롤러는 `[RequireHttps]` 특성입니다.
