---
uid: mvc/mvc5
title: ASP.NET MVC 5 | Microsoft Docs
author: rick-anderson
description: "ASP.NET MVC 5 ASP.NET MVC 5는 AS.의 능력에 대 한 체계적인 디자인 패턴을 사용 하 여 확장 가능 하 고 표준 기반 웹 응용 프로그램을 구축 하기 위한 프레임 워크..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: f79fbf7f-59e5-4279-a832-c1a0294630f4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/mvc5
msc.type: content
ms.openlocfilehash: e57163469ae4606df0fc17e3e054b7696782a084
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-5"></a>ASP.NET MVC 5
====================
## <a name="whats-new-in-aspnet-mvc-5"></a>ASP.NET MVC 5의에서 새로운 기능

### <a name="one-aspnet"></a>One ASP.NET

웹 MVC 프로젝트 템플릿을 새 One ASP.NET 환경과 완벽 하 게 통합 합니다. MVC 프로젝트를 사용자 지정 하 고 하나의 ASP.NET 프로젝트 만들기 마법사를 사용 하 여 인증을 구성할 수 있습니다. ASP.NET MVC 5에 있는 소개 자습서에서 찾을 수 있습니다 [Getting Started with ASP.NET MVC 5](overview/getting-started/introduction/getting-started.md)합니다.

MVC 5로 MVC 4 프로젝트 업그레이드에 대 한 자세한 내용은 참조 하십시오. [ASP.NET MVC 4 및 Web API 프로젝트를 ASP.NET MVC 5 및 Web API 2로 업그레이드 하는 방법을](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)합니다.

### <a name="aspnet-identity"></a>ASP.NET ID

MVC 프로젝트 템플릿 인증 및 id 관리에 대 한 ASP.NET Id를 사용 하도록 업데이트 되었습니다. Facebook 및 Google 인증 및 새로운 구성원 API 자습서에서 찾을 수 있습니다 [Facebook, Google OAuth2 및 OpenID 로그온 ASP.NET MVC 5 응용 프로그램을 만들](overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) 및 [와 Secure ASP.NET MVC 응용 프로그램 배포 멤버 자격, OAuth, 및 Windows Azure 웹 사이트에 SQL 데이터베이스](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)합니다.

### <a name="bootstrap"></a>부트스트랩

MVC 프로젝트 템플릿을 사용 하도록 업데이트 되었습니다 [부트스트랩](http://getbootstrap.com/) 는 매끄러운 및 응답성이 뛰어난 디자인을 쉽게 사용자 지정할 수 있도록 합니다. 자세한 내용은 참조 [Visual Studio 2013 웹 프로젝트 템플릿에서 부트스트랩](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap)합니다.

### <a name="authentication-filters"></a>인증 필터

[인증 필터](http://www.dotnetcurry.com/showarticle.aspx?ID=957) 새로운 종류의 ASP.NET MVC 파이프라인에서 권한 부여 필터 전에 실행 하 고 인증 논리 당-작업을 지정할 수 있도록 하는 ASP.NET MVC에서 필터는 컨트롤러 마다 또는 모든 컨트롤러에 대 한 전역적으로 합니다. 인증 필터 요청에 자격 증명을 처리 하 고 해당 보안 주체를 제공 합니다. 인증 필터 권한이 없는 요청에 부응 인증 챌린지에 추가할 수도 있습니다. 참조 [ASP.NET MVC 5 인증 필터](http://www.dotnetcurry.com/showarticle.aspx?ID=957), [ASP.NET MVC 5의 인증 필터](http://theshravan.net/blog/authentication-filters-in-asp-net-mvc-5/) 및 [마지막으로 새 ASP.NET MVC 5 인증 필터!](http://hackwebwith.net/finally-the-new-asp-net-mvc-5-authentication-filters/)합니다.

### <a name="filter-overrides"></a>필터 재정

필터를 지정 하 여 지정 된 동작 메서드 또는 컨트롤러에 적용을 재정의할 수 있습니다는 [필터는 재정의](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5)합니다. 재정의 필터에 지정된 된 범위 (작업이 나 컨트롤러)에 대 한 실행 하지 않아야 하는 필터 형식 집합을 지정 합니다. 이 통해 전체적으로 적용 되지만 다음의 특정 작업이 나 컨트롤러에 적용 하 여 특정 전역 필터를 제외 하는 필터를 구성할 수 있습니다. 참조 [ASP.NET MVC 5 및 ASP.NET Web API 2의 새 필터 재정의 기능](https://weblogs.asp.net/imranbaloch/archive/2013/09/25/new-filter-overrides-in-asp-net-mvc-5-and-asp-net-web-api-2.aspx), [ASP.NET MVC 5 필터 재정의 기능을 사용 하는 방법을](http://hackwebwith.net/how-to-use-the-asp-net-mvc-5-filter-overrides-feature/), 및 [ASP.NET MVC 5에서 필터 재정의](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5)

### <a name="attribute-routing"></a>특성 라우팅

ASP.NET MVC에서 지 원하는 [특성 라우팅을](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx), Tim McCall, 작성자에 의해 기여 덕분에 [http://attributerouting.net](http://attributerouting.net)합니다. 특성 라우팅을 사용 하 여 작업 및 컨트롤러를 주석을 달아 프로그램 경로 지정할 수 있습니다.

## <a name="new-web-project-experience"></a>새 웹 프로젝트 경험

Visual Studio 2013에서 새 웹 프로젝트를 만드는 환경과 향상 된 했습니다. 에 **새 ASP.NET 웹 프로젝트** 대화 원하는, 어떠한 조합의 기술 (Web Forms, MVC, Web API)를 구성 하 고, 인증 옵션을 구성 단위 테스트 프로젝트를 추가 프로젝트 유형을 선택할 수 있습니다.

![새 ASP.NET 프로젝트](mvc5/_static/image1.png)

새 대화 상자를 사용 하는 템플릿 중 대부분에 대 한 기본 인증 옵션을 변경할 수 있습니다. 예를 들어, ASP.NET Web Forms 프로젝트를 만들 때 다음 옵션 중 선택할 수 있습니다.

- 인증 안 함
- 개별 사용자 계정 (ASP.NET 멤버 자격 또는 소셜 공급자 로그에서)
- 조직 계정 (인터넷 응용 프로그램에 Active Directory)
- Windows 인증 (인트라넷 응용 프로그램에서 Active Directory)

![인증 옵션](mvc5/_static/image2.png)

웹 프로젝트를 만들기 위한 새 프로세스에 대 한 자세한 내용은 참조 [Visual Studio 2013에서 ASP.NET 웹 프로젝트 만들기](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md)합니다. 새 인증 옵션에 대 한 자세한 내용은 참조 [ASP.NET Identity](../identity/overview/index.md)합니다.

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET 스 캐 폴딩

ASP.NET 스 캐 폴딩 ASP.NET 웹 응용 프로그램에 대 한 코드 생성 프레임 워크입니다. 쉽게 데이터 모델과 상호 작용 하는 프로젝트에 상용구 코드를 추가 합니다.

이전 버전의 Visual Studio를 스 캐 폴딩 ASP.NET MVC 프로젝트에 있었습니다. Visual Studio 2013, Web Forms를 포함 하 여 모든 ASP.NET 프로젝트에 대 한 스 캐 폴딩을 지금 사용할 수 있습니다. Visual Studio 2013 생성 페이지 Web Forms 프로젝트에 대 한 현재 지원 하지 않는 있지만 프로젝트에 MVC 종속성을 추가 하 여 스 캐 폴딩 Web Forms를 계속 사용할 수 있습니다. Web forms 페이지를 생성 하는 것에 대 한 지원이 향후 업데이트에서 추가 됩니다.

스 캐 폴딩을 사용할 때 필요한 모든 프로젝트에 종속성이 설치 되어을 확인 합니다. 예를 들어, ASP.NET Web Forms 프로젝트와 함께 시작 하 고 다음 스 캐 폴딩을 사용 하 여 웹 API 컨트롤러를 추가 하려면 필요한 NuGet 패키지와 참조를 프로젝트에 자동으로 추가 됩니다.

MVC 스 캐 폴딩 Web Forms 프로젝트를 추가 하려면 추가 **스 캐 폴드 된 새 항목** 선택 **MVC 5 종속성** 대화 상자 창에서. MVC; 스 캐 폴딩에 대 한 다음과 같은 최소이 고 전체. 최소을 선택 하면 NuGet 패키지와 ASP.NET MVC에 대 한 참조가 프로젝트에 추가 됩니다. 전체 옵션을 선택 하는 경우 MVC 프로젝트에 필요한 콘텐츠 파일 뿐만 아니라 최소한의 종속성 추가 됩니다.

비동기 컨트롤러를 스 캐 폴딩에 대 한 지원 Entity Framework 6의 새로운 비동기 기능을 사용 합니다.

자세한 내용과 자습서에 대 한 참조 [ASP.NET 스 캐 폴딩 개요](../visual-studio/overview/2013/aspnet-scaffolding-overview.md)합니다.

### <a name="getting-help-and-reporting-issues"></a>도움말 보기 및 문제를 보고 합니다.

- [알려진된 문제 및 주요 변경 내용 목록](../visual-studio/overview/2013/release-notes.md#knownissues)
- 도움말 보기 및 ASP.NET MVC 5에 논의 [포럼](https://forums.asp.net/1146.aspx)
- [ASP.NET MVC 5의 버그를 보고 합니다.](https://github.com/aspnet/AspNetWebStack/issues)
- [기능 요청](http://aspnet.uservoice.com/forums/41201-asp-net-mvc)

### <a name="upgrading-from-aspnet-mvc-4"></a>ASP.NET MVC 4에서 업그레이드

참조 [웹 API 프로젝트를 ASP.NET MVC 5 및 Web API 2 및 ASP.NET MVC 4를 업그레이드 하는 방법](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)
