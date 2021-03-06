---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: "자습서: SignalR 2 및 MVC 5 시작 | Microsoft Docs"
author: pfletcher
description: "이 자습서에는 실시간 채팅 응용 프로그램을 만들려면 ASP.NET SignalR 2를 사용 하는 방법을 보여 줍니다. SignalR MVC 5 응용 프로그램에 추가 하 고 채팅 보기를 만드는 중..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: ca0471114da7363c5df9d459308708e7ab4f8b84
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a>자습서: SignalR 2 및 MVC 5 시작
====================
여 [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

[완료 된 프로젝트를 다운로드 합니다.](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> 이 자습서에는 실시간 채팅 응용 프로그램을 만들려면 ASP.NET SignalR 2를 사용 하는 방법을 보여 줍니다. SignalR MVC 5 응용 프로그램에 추가 되며 채팅 보기를 만들어 보내고, 메시지를 표시 합니다. 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>자습서에서 사용 되는 소프트웨어 버전
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - MVC 5
> - SignalR 버전 2
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>이 자습서와 함께 Visual Studio 2012를 사용 하 여
> 
> 
> 이 자습서와 함께 Visual Studio 2012를 사용 하려면 다음을 수행 합니다.
> 
> - 업데이트 프로그램 [패키지 관리자](http://docs.nuget.org/docs/start-here/installing-nuget) 를 최신 버전입니다.
> - 설치는 [웹 플랫폼 설치 관리자](https://www.microsoft.com/web/downloads/platform.aspx)합니다.
> - 웹 플랫폼 설치 관리자에서 검색 하 고 설치 **ASP.NET 및 Visual Studio 2012 용 웹 도구 2013.1**합니다. SignalR 클래스에 대 한 Visual Studio 템플릿을와 같은 설치 **허브**합니다.
> - 일부 서식 파일 (와 같은 **OWIN 시작 클래스**)은 사용할 수 없습니다; 이러한 경우에 대 한 클래스 파일을 대신 사용 합니다.
> 
> 
> ## <a name="tutorial-versions"></a>자습서 버전
> 
> 이전 버전의 SignalR에 대 한 정보를 참조 하십시오. [SignalR 이전 버전](../older-versions/index.md)합니다.
> 
> ## <a name="questions-and-comments"></a>질문이 나 의견이
> 
> 이 자습서를 연결 하는 방법 및 페이지의 맨 아래에 주석에서 향상 될 수 있습니다 어떻게에 의견을 남겨 주세요. 자습서를 직접 관련 되지 않는 질문 해야 하도록를 게시할 수 있습니다는 [ASP.NET SignalR 포럼](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) 또는 [StackOverflow.com](http://stackoverflow.com/)합니다.


## <a name="overview"></a>개요

이 자습서는 ASP.NET SignalR 2 및 ASP.NET MVC 5 실시간 웹 응용 프로그램 개발을 소개합니다. 이 자습서에 동일한 채팅 응용 프로그램 코드를 사용 하 여는 [SignalR 시작 자습서](tutorial-getting-started-with-signalr.md), 있지만 MVC 5 응용 프로그램에 추가 하는 방법을 보여 줍니다.

이 항목의 다음 SignalR 개발 작업에 설명 합니다.

- MVC 5 응용 프로그램에 SignalR 라이브러리를 추가 합니다.
- 허브 및 클라이언트에 콘텐츠를 푸시 하려면 OWIN 시작 클래스 만들기
- 웹 페이지에서 SignalR jQuery 라이브러리를 사용 하 여 메시지를 보내고 허브에서 업데이트를 표시 합니다.

다음 스크린 샷에서 브라우저에서 실행 되는 완료 된 채팅 응용 프로그램을 보여 줍니다.

![채팅 인스턴스](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

섹션:

- [프로젝트를 설정](#setup)
- [샘플 실행](#run)
- [코드 검사](#code)
- [다음 단계](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>프로젝트를 설정

필수 구성 요소:

- Visual Studio 2013. Visual Studio가 참조 [ASP.NET 다운로드](https://www.asp.net/downloads) 무료 Visual Studio 2013 Express 개발 도구를 얻으려고 합니다.

이 섹션에는 ASP.NET MVC 5 응용 프로그램을 만들고 SignalR 라이브러리를 추가, 채팅 응용 프로그램을 만드는 방법을 보여 줍니다.

1. Visual Studio에서.NET Framework 4.5를 대상으로 하는 C# ASP.NET 응용 프로그램을 만듭니다 SignalRChat, 라는 이름을 지정 하 고 확인을 클릭 합니다.

    ![웹 만들기](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. 에 `New ASP.NET Project` 대화 상자를 닫고 선택 **MVC**를 클릭 하 고 **인증 변경**합니다.

    ![웹 만들기](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. 선택 **인증 안 함** 에 **인증 변경** 대화 상자를 닫고 클릭 **확인**합니다.

    ![인증 안 함 선택](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > 응용 프로그램에 대 한 다른 인증 공급자를 선택 하는 경우는 `Startup.cs` 클래스를 생성 됩니다; 직접 만들 필요가 `Startup.cs` 10 아래 단계에는 클래스입니다.
4. 클릭 **확인** 에 **새 ASP.NET 프로젝트** 대화 상자.
5. 열기는 **도구 | 라이브러리 패키지 관리자 | 패키지 관리자 콘솔** 다음 명령을 실행 합니다. 이 단계는 SignalR 기능을 사용 하는 어셈블리 참조 및 스크립트 파일 집합이 프로젝트에 추가 합니다.

    `install-package Microsoft.AspNet.SignalR`
6. **솔루션 탐색기**, 스크립트 폴더를 확장 합니다. SignalR에 대 한 스크립트 라이브러리를 프로젝트에 추가한 참고 합니다.

    ![스크립트 폴더](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. **솔루션 탐색기**프로젝트를 마우스 오른쪽 단추로 선택 **추가 | 새 폴더**, 라는 새 폴더를 추가 하 고 **허브**합니다.
8. 마우스 오른쪽 단추로 클릭는 **허브** 폴더를 클릭 하 여 **추가 | 새 항목**, 선택는 **Visual C# | 웹 | SignalR** 에서 노드는 **설치 됨** 창에서 **SignalR 허브 클래스 (v2)** 가운데 창에서 명명 된 새 허브를 만들고 **ChatHub.cs**합니다. 이 클래스는 모든 클라이언트에 메시지를 보내는 SignalR 서버 허브로 사용 합니다.

    ![새 허브를 만듭니다.](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. 코드는 **ChatHub** 를 다음 코드로 클래스입니다.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. Startup.cs 라는 새 클래스를 만듭니다. 다음에 파일의 내용을 변경 합니다.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. 편집는 `HomeController` 에서 클래스를 찾을 **Controllers/HomeController.cs** 클래스에 다음 메서드를 추가 합니다. 이 메서드는 반환 된 **채팅** 이후 단계에서 만들 보기.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. 마우스 오른쪽 단추로 클릭는 **뷰/홈** 폴더를 찾아 선택 **추가... | 보기**합니다.
13. 에 **뷰 추가** 대화 상자에서 새 뷰 이름 **채팅**합니다.

    ![보기 추가](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. 내용을 대체 **Chat.cshtml** 다음 코드를 사용 합니다.

    > [!IMPORTANT]
    > SignalR 및 기타 스크립트 라이브러리를 Visual Studio 프로젝트에 추가 하면 패키지 관리자는이 항목에 표시 된 버전 보다 더 최신 SignalR 스크립트 파일의 버전을 설치 될 수 있습니다. 코드에서 스크립트 참조가 프로젝트에 설치 된 스크립트 라이브러리의 버전이 일치 하는지 확인 합니다.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. **모두 저장** 프로젝트에 대 한 합니다.

<a id="run"></a>

## <a name="run-the-sample"></a>샘플 실행

1. F5 키를 눌러 디버그 모드에서 프로젝트를 실행 합니다.
2. 브라우저 주소 표시줄에 추가할 **/home/채팅** 프로젝트에 대 한 기본 페이지의 url입니다. 채팅 페이지 사용자 이름에 대 한 프롬프트 및 브라우저 인스턴스를 로드합니다.

    ![사용자 이름 입력](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. 사용자 이름을 입력 합니다.
4. 브라우저의 주소 표시줄에서 URL을 복사 하 고 두 인스턴스를 열어 자세한 브라우저를 사용 합니다. 각 브라우저 인스턴스에서 고유한 사용자 이름을 입력 합니다.
5. 각 브라우저 인스턴스에서 주석을 추가 하 고 클릭 **보낼**합니다. 모든 브라우저 인스턴스에 주석을 표시 되어야 합니다.

    > [!NOTE]
    > 이 간단한 채팅 응용 프로그램 서버에서 토론 컨텍스트를 유지 하지 않습니다. 허브 모든 현재 사용자에 게 의견을 브로드캐스트합니다. 사용자에 게 채팅을 나중에 조인에 참여할 때부터 추가 된 메시지 표시 됩니다.
6. 다음 스크린 샷에서 브라우저에서 실행 중인 채팅 응용 프로그램을 보여 줍니다.

    ![채팅 브라우저](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. **솔루션 탐색기**, 검사는 **스크립트 문서** 실행 중인 응용 프로그램에 대 한 노드입니다. 이 노드는 브라우저로 Internet Explorer를 사용 하는 경우 디버그 모드에서 표시 합니다. 라는 스크립트 파일이 **허브** SignalR 라이브러리를 런타임에 동적으로 생성 하는 합니다. 이 파일 jQuery 스크립트와 서버 쪽 코드 간의 통신을 관리합니다. Internet Explorer 이외의 브라우저를 사용 하면 동적 액세스할 수도 있습니다 **허브** 파일을 찾아 직접 예를 들어 http://mywebsite/signalr/hubs 합니다.

<a id="code"></a>

## <a name="examine-the-code"></a>코드 검사

SignalR 채팅 응용 프로그램에서는 두 가지 기본 SignalR 개발 작업: 서버에서 주 조정 개체로 허브를 만들고 SignalR jQuery 라이브러리를 사용 하 여 메시지를 주고받을 수 있습니다.

### <a name="signalr-hubs"></a>SignalR 허브

코드 예제에서는 **ChatHub** 클래스에서 파생 되는 **Microsoft.AspNet.SignalR.Hub** 클래스입니다. 파생 되는 **허브** 클래스는 SignalR 응용 프로그램을 작성 하는 유용한 방법은 합니다. 허브 클래스에 공용 메서드를 만들고 그런 다음 웹 페이지의 스크립트에서 호출 하 여 이러한 메서드에 액세스할 수 있습니다.

클라이언트 채팅 코드에서 호출할는 **ChatHub.Send** 메서드 새 메시지를 보내려고 합니다. 허브에 메시지를 보냅니다 모든 클라이언트를 호출 하 여 **Clients.All.addNewMessageToPage**합니다.

**보낼** 메서드 여러 허브 개념을 보여 줍니다.

- 클라이언트에서 호출할 수 있도록 허브에서 공용 메서드를 선언 합니다.
- 사용 하 여는 **Microsoft.AspNet.SignalR.Hub.Clients** 이 허브에 연결 된 속성을 모든 클라이언트에 액세스 합니다.
- 클라이언트에서 함수를 호출 (예:는 `addNewMessageToPage` 함수) 클라이언트를 업데이트 하려면.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR 및 jQuery

**Chat.cshtml** 코드 샘플에서 파일 보기에는 SignalR 허브와 통신 하는 SignalR jQuery 라이브러리를 사용 하는 방법을 보여 줍니다. 코드에는 필수 작업 자동 생성 된 프록시에는 허브에 대 한 서버를 클라이언트에 대 한 밀어넣기 콘텐츠를 호출할 수 있는 함수를 선언 하 고 연결을 시작 하는 허브에 메시지를 보낼 대 한 참조를 만듭니다.

다음 코드는 허브 프록시에 대 한 참조를 선언합니다.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> JavaScript에서 카멜식 대 /에서 서버 클래스 및 해당 멤버에 대 한 참조는 합니다. 코드 샘플 참조는 C# **ChatHub** 으로 JavaScript에서 클래스 **chatHub**합니다. 참조 하려는 경우는 `ChatHub` 클래스 규칙에 따른 파스칼와 jQuery에서 ChatHub.cs 클래스 파일을 편집 C#에서 마찬가지로 대/소문자 구분 합니다. 추가 `using` 문을 참조 하는 `Microsoft.AspNet.SignalR.Hubs` 네임 스페이스입니다. 그런 다음 추가 `HubName` 특성을 `ChatHub` 클래스, 예를 들어 `[HubName("ChatHub")]`합니다. jQuery 참조를 마지막으로 업데이트는 `ChatHub` 클래스입니다.


다음 코드를 스크립트에는 콜백 함수를 만드는 방법을 보여 줍니다. 서버에서 허브 클래스에는 각 클라이언트에 콘텐츠 업데이트 적용 하려면이 함수를 호출 합니다. 에 대 한 선택적 호출은 `htmlEncode` 함수를 HTML로 변환 하는 방법 보여 주는 스크립트 삽입을 방지 하는 방법으로 페이지에 표시 하기 전에 메시지 내용을 인코딩합니다.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

다음 코드에는 허브에 대 한 연결을 여는 방법을 보여 줍니다. 연결을 시작 하 고 다음에 클릭 이벤트를 처리 하는 함수 전달 하는 코드는 **보낼** 채팅 페이지의에서 단추입니다.

> [!NOTE]
> 이 방법을 사용 하면 이벤트 처리기 실행 되기 전에 연결이 설정 됩니다.


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>다음 단계

SignalR은 실시간 웹 응용 프로그램을 구축 하기 위한 프레임 워크는 배웠습니다. 여러 가지 SignalR 개발 작업에 배웠습니다: SignalR ASP.NET 응용 프로그램에 추가 하는 방법, 허브 클래스를 만드는 방법 및 보내고 허브에서 메시지를 수신 하는 방법입니다.

샘플 SignalR 응용 프로그램을 Azure에 배포 하는 방법에 대 한 연습을을 참조 하십시오. [Azure 앱 서비스의 웹 앱과 함께 사용 하 여 SignalR](../deployment/using-signalr-with-azure-web-sites.md)합니다. Visual Studio 웹 프로젝트는 Windows Azure 웹 사이트를 배포 하는 방법에 대 한 자세한 내용은 참조 하십시오. [Azure 앱 서비스에서 ASP.NET 웹 앱을 만들](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)합니다.

SignalR 개발 보다 발전된 된 개념을 알아보려면 SignalR 소스 코드 및 리소스에 대 한 다음 사이트를 방문 하십시오.

- [SignalR 프로젝트](http://signalr.net)
- [SignalR Github 및 샘플](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
