---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: "1 단계: 파일-새 프로젝트 > | Microsoft Docs"
author: JoeStagner
description: "이 자습서 시리즈 모든 Tailspin Spyworks 샘플 응용 프로그램을 작성 하는 데 필요한 단계를 자세히 설명 합니다. 1 부에서는 개요 및 프로젝트 파일/새로 만들기를 다룹니다."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: bd840f9f3f5d723e6bc1bb35955a7770634e9483
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2017
---
<a name="part-1-file--new-project"></a>1 단계: 파일-새 프로젝트 >
====================
으로 [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks.NET 플랫폼에 대해 강력 하 고 확장 가능한 응용 프로그램을 만드는 방법을 매우 간단한는 하는 방법을 보여 줍니다. Off 쇼핑, 체크 아웃, 및 관리를 포함 하 여 온라인 상점에서는 만들려는 ASP.NET 4에서 멋진 새로운 기능을 사용 하는 방법을 보여 줍니다.
> 
> 이 자습서 시리즈 모든 Tailspin Spyworks 샘플 응용 프로그램을 작성 하는 데 필요한 단계를 자세히 설명 합니다. 1 부에서는 개요 및 프로젝트 파일/새로 만들기를 다룹니다.


## <a id="_Toc260221666"></a>개요

이 자습서에는 ASP.NET WebForms에 대 한 소개입니다. 에서는 느리게 시작 합니다, 그리고 초급 수준 웹 개발 발생 하므로 괜찮습니다.

म를 작성 하는 응용 프로그램에는 단순 온라인 저장소입니다.

![](tailspin-spyworks-part-1/_static/image1.jpg)


방문자 제품 범주별으로 찾아보기 수 있습니다.:

![](tailspin-spyworks-part-1/_static/image2.jpg)

단일 제품을 확인 하 고 자신의 카트에 추가:

![](tailspin-spyworks-part-1/_static/image3.jpg)

제거 하는 모든 항목을 더 이상 자신의 카트에 검토할 수 없습니다.

![](tailspin-spyworks-part-1/_static/image4.jpg)

체크 아웃 하기 묻는 메시지가 표시 됩니다.

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

순서를 지정한 후 간단한 확인 화면이 볼 수 있습니다.

![](tailspin-spyworks-part-1/_static/image7.jpg)


Visual Studio 2010에서 새 ASP.NET WebForms 프로젝트를 만들면 먼저 및 기능을 완전 한 기능의 응용 프로그램을 만들거나 증분 방식으로 추가 합니다. 프로세스를 진행 다룰 것 데이터베이스 액세스, 목록 및 그리드 보기, 페이지를 업데이트 하는 데이터, 데이터 유효성 검사, 마스터 페이지를 사용 하 여 일관 된 페이지 레이아웃, AJAX, 유효성 검사, 사용자 구성원 자격, 등에.

과정을 단계별로, 함께 진행할 수 또는에서 완성 된 응용 프로그램을 다운로드할 수 있습니다 [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)

Visual Studio 2010 또는 무료 Visual Web Developer 2010에서 사용할 수 있습니다 [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/)합니다. 응용 프로그램을 빌드하려면 SQL Server 또는 무료 SQL Server Express에 데이터베이스를 호스팅하는 사용할 수 있습니다.

## <a id="_Toc260221667"></a>파일 / 새 프로젝트

Visual Studio에서 파일 메뉴에서 새 프로젝트를 선택 하 여 시작 합니다. 이 통해 새 프로젝트 대화 상자를 가져옵니다.

![](tailspin-spyworks-part-1/_static/image8.jpg)

Visual C# 선택 / 웹 템플릿 왼쪽에 그룹화 하 고 가운데 열에서 "ASP.NET 웹 응용 프로그램" 템플릿을 선택 합니다. TailspinSpyworks 프로젝트 이름을 지정 하 고 확인 단추를 누릅니다.

![](tailspin-spyworks-part-1/_static/image9.jpg)

이 프로젝트를 만듭니다. 오른쪽에 솔루션 탐색기에서 해당 응용 프로그램에 포함 된 폴더에 살펴보겠습니다.

![](tailspin-spyworks-part-1/_static/image10.jpg)

빈 솔루션을 완전히 비어 있지 않은 – 기본 폴더 구조를 추가 합니다.

![](tailspin-spyworks-part-1/_static/image1.png)

Note ASP.NET 4 기본 프로젝트 템플릿에 의해 구현 되는 규칙입니다.

- "계정" 폴더는 ASP에 대 한 기본 사용자 인터페이스를 구현합니다. NET의 멤버 자격 하위 시스템입니다.
- "Scripts" 폴더는 클라이언트 쪽 JavaScript 파일의 리포지토리로 사용 하 고 핵심 jQuery.js 파일이 기본적으로 사용할 수 있습니다.
- "스타일" 폴더는이 웹 사이트 (CSS 스타일 시트) 시각적 개체를 구성 하는 데

응용 프로그램을 실행 하 고 default.aspx 페이지를 렌더링 하려면 f5 키를 눌러 म 하면 다음이 표시 됩니다.

![](tailspin-spyworks-part-1/_static/image11.jpg)

기본 WebForms 템플릿에서 Style.css 파일 CSS 클래스와는 Tailspin Spyworks 응용 프로그램에 대 한 원하는 시각적 asthetics 렌더링 하는 연결 된 이미지 파일을 바꿀 우리의 첫 번째 응용 프로그램 향상이 됩니다.

이렇게 한 다음 다음과 같이 default.aspx 페이지를 렌더링 합니다.

![](tailspin-spyworks-part-1/_static/image12.jpg)

위쪽 이미지 링크 확인 페이지 및 마스터 페이지에 추가 된 메뉴 항목의 오른쪽입니다. "로그인" 및 "계정" 링크 (기본 서식 파일에서 생성) 존재 하는 페이지 및 응용 프로그램 구축할 구현 해 페이지의 나머지 부분을 가리킵니다.

마스터 페이지의 스타일 디렉터리 위치를 변경 하려면 또한 하겠습니다. 이 기본 설정 값은 사항이 작업을 좀 더 쉽게 응용 프로그램을 나중에 "skinable" 확인 하도록 결정 했습니다.

마스터 페이지를 변경 해야이 작업을 수행한 후 모든.aspx 파일에 대 한 참조에서 생성 된 기본 ASP.NET WebForms 페이지입니다.

>[!div class="step-by-step"]
[다음](tailspin-spyworks-part-2.md)
