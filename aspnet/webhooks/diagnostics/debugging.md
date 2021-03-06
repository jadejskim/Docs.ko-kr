---
uid: webhooks/diagnostics/debugging
title: "디버깅 하는 ASP.NET Webhook | Microsoft Docs"
author: rick-anderson
description: "ASP.NET Webhook을 디버깅 하는 방법입니다."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 524cdf0246eda9ef213414923cd23a92a01f211e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-webhooks-debugging"></a>ASP.NET Webhook 디버깅  

## <a name="debugging-in-azure"></a>Azure의 디버깅

Azure에서 실행 하는 동안 웹 응용 프로그램을 디버깅 하려면이 자습서를 참조 하십시오 [Visual Studio를 사용 하 여 Azure 앱 서비스의 웹 앱의 문제를 해결](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs)합니다.

## <a name="debugging-with-source-and-symbols"></a>소스 및 기호를 사용 하 여 디버깅

사용자 고유의 코드를 디버깅 하는 것 외에도 되며 실제로 Microsoft ASP.NET Webhook을에 직접 디버그할 수는 모든.NET 합니다. 이 로컬 또는 원격으로 디버그 여부에 관계 없이 작동 합니다. 첫째,로 이동 하 여 소스 및 기호를 찾을 하도록 Visual Studio를 구성 **디버그** 차례로 **옵션 및 설정**합니다. 다음과 같은 옵션을 설정 합니다.

![옵션 및 설정](_static/SourceSymbols.png)

다음 링크를 추가 [symbolsource.org](http://symbolsource.org) 소스 및 기호를 다운로드 합니다. 이동 하는 **기호** 위의 메뉴의 탭 및 기호 위치에서 다음 추가:

```
http://srv.symbolsource.org/pdb/Public
```

또한 캐시 디렉터리에 약식 이름을; 있는지를 확인합니다 그렇지 않은 경우 파일 이름은 길어질 수 있습니다 너무 기호를 로드할에 발생 합니다. 샘플 경로 같습니다.

```
C:\SymCache
```

설정을 다음과 유사 하 게 같아야 합니다.

![옵션 기호 파일 위치 예](_static/SymSource.png)
