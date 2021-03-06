---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
title: "큐 중심 작업 패턴 (Azure로 응용 프로그램 빌딩 실제 클라우드) | Microsoft Docs"
author: MikeWasson
description: "실제 세계 클라우드로 응용 프로그램 빌딩 Azure 전자책 Scott Guthrie에서 개발 된 프레젠테이션을 기반으로 합니다. 13 패턴 및 그을 수 있는 방법에 설명..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: cc1ad51b-40c3-4c68-8620-9aaa0fd1f6cf
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
msc.type: authoredcontent
ms.openlocfilehash: ccfbaa26cbf610f847811e6f3c612458277046ed
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2018
---
<a name="queue-centric-work-pattern-building-real-world-cloud-apps-with-azure"></a>큐 중심 작업 패턴 (Azure로 응용 프로그램 빌딩 실제 클라우드)
====================
여 [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[다운로드 해결 프로젝트](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) 또는 [전자책 다운로드](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **실제 세계 클라우드로 응용 프로그램 빌딩 Azure** 전자책 Scott Guthrie에서 개발 된 프레젠테이션을 기반으로 합니다. 13 패턴에 설명 하 고 사례 수 있는 클라우드에 대 한 웹 앱을 개발 성공적으로 수행 되어야 합니다. 전자책에 대 한 정보를 참조 하십시오. [첫 번째 장](introduction.md)합니다.


앞에서 살펴본를 여러 서비스를 사용 하 여 될 수 있는 응용 프로그램의 효과적인 SLA는 "복합" SLA를는 *제품* 개별 sla 합니다. 예를 들어 수정 응용 프로그램 웹 사이트, 저장소 및 SQL 데이터베이스를 사용합니다. 이러한 서비스 중 하나라도 실패 하면 응용 프로그램 사용자에 게 오류가 반환 됩니다.

캐싱은 일시적 오류에 대 한 읽기 전용 콘텐츠를 처리 하는 좋은 방법입니다. 하지만 응용 프로그램 작업을 수행할 수 있어야 하는 경우에 어떻게? 예를 들어 사용자가 새 수정 작업을 제출 하는 경우 앱은 캐시에 작업을 바로 넣을 수 없습니다. 응용 프로그램이 처리할 수 있도록 영구 데이터 저장소로 수정 작업을 작성 해야 합니다.

있는 경우에 큐 중심 작업 패턴입니다. 이 패턴을 사용 하는 웹 계층 및 백 엔드 서비스 간의 느슨한 결합 합니다.

패턴의 작동 방식을 다음과 같습니다. 응용 프로그램 요청을 가져오면 작업 항목 큐 위에 배치 하 고 즉시 응답을 반환 합니다. 그런 다음 별도 백 엔드 프로세스 작업 항목 큐에서를 가져오며는 작업을 수행 합니다.

큐 중심 작업 패턴에 유용합니다.

- 시간이 오래 걸리는 (대기 시간이 긴) 하는 작업입니다.
- 필요한 작업을 외부 서비스를 항상 사용할 수 없습니다.
- 작업이 리소스 사용량이 (CPU).
- 속도 (갑작스러운 부하 필요)에 따라 평준화를 활용할 수 있는 작업입니다.

## <a name="reduced-latency"></a>대기 시간 단축된

큐는 시간이 많이 걸리는 작업을 수행 하는 언제 든 지 유용 합니다. 작업 변수를 사용할 경우 몇 초 이상일 차단 하는 대신 최종 사용자 작업 항목 큐에 두고 있습니다. "현재 진행 중,"는 사용자에 게 알림 하 고 백그라운드에서 작업을 처리 하는 큐 수신기를 사용 합니다.

예를 들어 온라인 상점에서 무언가 구매 하면 웹 사이트 확인 주문을 즉시 합니다. 하지만 전자 메일이 배달 트럭 이미에 의미는 아닙니다. 이러한 작업을 큐에 추가 되 고 백그라운드에서 되 전달용으로 항목을 준비 하 고 신용 검사를 수행 하 고 등입니다.

짧은 대기 시간으로 시나리오에 대 한 총 종단 간 시간 비교 작업을 동기적으로 수행 하는 큐를 사용 하 여 길 수도 있습니다. 하지만 다양 한 이점을 해당 단점은 보다 클 수 있습니다도 합니다.

## <a name="increased-reliability"></a>향상 된 안정성

수정 했으므로 되었습니다에서 우리가 보고 지금까지 하 버전에서는 웹 프런트 엔드는 SQL 데이터베이스 백 엔드에서 밀접 됩니다. SQL 데이터베이스 서비스를 사용할 수 없는 경우 사용자는 오류를 가져옵니다. 다시 시도가 작동 하지 않는 경우 (즉, 오류는 일시적인 이상), 할 수 있는 유일한 항목은 오류를 표시 하 고 나중에 다시 시도를 사용자에 게 요청 합니다.

![다이어그램 보여 주는 웹 백 엔드 SQL 데이터베이스에 실패 한 경우 실패 한 프런트 엔드](queue-centric-work-pattern/_static/image1.png)

사용자가 수정 작업을 제출 하는 경우에 큐를 사용 하 여, 응용 프로그램 큐로 메시지를 씁니다. 메시지 페이로드가 [JSON](http://json.org/) 작업의 표현입니다. 응용 프로그램 메시지는 큐에 기록 되는 즉시 반환 하 고 사용자에 게 즉시 성공 메시지를 표시 합니다.

오프 라인으로 전환의 SQL 데이터베이스 또는 큐 수신기---같은 백 엔드 서비스 사용자가 새 수정 작업을 제출 여전히 수 있습니다. 메시지가 백 엔드 서비스를 다시 사용할 수 있는 때까지 대기 방금 됩니다. 이 시점에서 백 엔드 서비스 백로그 catch 합니다.

![웹 프런트 엔드를 계속 SQL 데이터베이스 오류가 있을 때 작동을 보여 주는 다이어그램](queue-centric-work-pattern/_static/image2.png)

또한 이제 추가할 수 있습니다 더 많은 백 엔드 논리 프런트 엔드의 복원 력을 염려 하지 않고 있습니다. 예를 들어 다음는 새로운 해결 할당 될 때마다 소유자에 게 전자 메일 또는 SMS 메시지를 전송 하는 것이 좋습니다. 전자 메일 또는 SMS 서비스를 사용할 수 없는 경우에 기타 모든 요소를 처리할 수 있으며 다음 전자 메일/SMS 메시지를 보내기 위한 별도 큐에 메시지를 추가 수 있습니다.

효과적인 SLA는 웹 앱을 했습니다. 이전에 &times; 저장소 &times; SQL 데이터베이스 = 99.7%입니다. (참조 [실패 하기 위해 디자인](design-to-survive-failures.md).)

큐를 사용 하는 응용 프로그램을 변경 하는 경우 복합 99.8%의 SLA에 대 한 웹 프런트 엔드 웹 앱 및 저장소에 대해서만 따라 다릅니다. (참고는 큐 포함 되므로 Azure 저장소 서비스의 blob 저장소와 같은 SLA에 포함 됩니다.)

99.8% 보다 더 좋은 해야 할 경우에 두 개의 서로 다른 지역에 두 개의 큐를 만들 수 있습니다. 보조 복제본으로 주 서버, 고 다른 하나를 지정 합니다. 응용 프로그램에서 장애 조치할 보조 큐에 기본 큐를 사용할 수 없는 경우. 둘 다 사용할 수 없게 동시에 가능성이 매우 낮습니다.

## <a name="rate-leveling-and-independent-scaling"></a>속도 평준화 및 독립 크기 조정

큐는 이라고 하는 항목에 대 한 유용한도 *평준화 비율* 또는 *부하 평준화*합니다.

웹 앱은 종종 트래픽이 급격 한 증가에 취약 합니다. 자동 크기 조정을 자동으로 증가 된 웹 트래픽을 처리 하기 위해 웹 서버를 추가 하려면를 사용할 수 있지만 자동 크기 조정 갑작스러운 부하 스파이크를 처리 신속 하 게 반응 하 못할 수 있습니다. 웹 서버 큐에 메시지를 기록 하 여 수행 해야 하는 작업을 오프 로드할 수, 하는 경우 더 많은 트래픽을 처리할 수 있도록 합니다. 다음 백 엔드 서비스 큐에서 메시지를 읽은 하 고 처리할 수 있습니다. 큐의 깊이가 증가 되거나 수신 부하가 변경 됨에 따라 축소 됩니다.

백 엔드 서비스도 오프 로드 하 고 시간이 많이 걸리는 작업의 대부분에 웹 계층 트래픽 갑자기 증가 보다 쉽게 응답할 수 있습니다. 및 트래픽의 주어진된 시간에 모든 웹 서버 수도 더 적은 의해 처리 될 수 있으므로 비용을 절감 합니다.

있습니다 수 웹 계층 및 백 엔드 서비스를 독립적으로 확장할 합니다. 예를 들어 하나의 서버 큐 메시지 처리 하지만 세 가지 웹 서버를 할 수 있습니다. 또는 백그라운드에서 계산 집약적인 작업을 실행 하는 경우에 백 엔드 서버를 더을 할 수 있습니다.

![](queue-centric-work-pattern/_static/image3.png)

자동 크기 조정에는 웹 계층 뿐 아니라 백 엔드 서비스와 함께 작동 합니다. 수직 확장 또는 백 엔드 Vm의 CPU 사용량에 따라 큐에 작업을 처리 하는 Vm 수를 줄일 수 있습니다. 또는 큐에 있는 항목 수에 따라 자동 크기 조정을 할 수 있습니다. 예를 들어 10 개 미만의 항목을 큐에 유지 되도록 자동 크기 조정을 알 수 있습니다. 큐에 항목 수가 10 개, 자동 크기 조정이 Vm을 추가 합니다. 해결, 자동 크기 조정 추가 Vm 종료 됩니다.

## <a name="adding-queues-to-the-fix-it-application"></a>추가 큐에 대기 수정 사항이 응용 프로그램

큐 패턴을 구현 하려면 두 가지 수정 앱을 변경 해야 합니다.

- 사용자가 새 수정 작업을 제출는 태스크를 데이터베이스에 작성 하는 대신 큐에 삽입 합니다.
- 큐에 메시지를 처리 하는 백 엔드 서비스를 만듭니다.

큐에 사용 된 [Azure 큐 저장소 서비스](https://www.windowsazure.com/develop/net/how-to-guides/queue-service/)합니다. 또 다른 옵션은 사용 하도록 [Azure 서비스 버스](https://docs.microsoft.com/azure/service-bus/)합니다.

큐 서비스를 사용 하 여 기능을 결정 하려면 응용 프로그램 큐에 메시지를 받거나 보내기 위해 요구 하는 방법을 고려 합니다.

- 협동 생산자와 경쟁 소비자를 설정한 경우에 Azure 큐 저장소 서비스를 사용 하는 것이 좋습니다. "협력 생산자" 여러 프로세스 큐에 메시지 추가 하는 것을 의미 합니다. 여러 프로세스를 끌어오는 큐에서 메시지를 처리 하지만 특정된 메시지 하나 "."소비자에서 처리할 수만 "경쟁 소비자" 의미 단일 큐 수 보다 더 많은 처리량을 해야 하는 경우 추가 큐 및/또는 추가 저장소 계정을 사용 합니다.
- 필요한 경우 한 [게시/등록 모델](http://en.wikipedia.org/wiki/Publish/subscribe), Azure 서비스 버스 큐를 사용 하는 것이 좋습니다.

수정 앱 협동 생산자와 경쟁 소비자 모델에 적합합니다.

또 다른 고려 사항은 응용 프로그램 가용성입니다. 큐 저장소 서비스에는 blob 저장소에 대 한 사용 되므로 SLA는 영향을 주지 않습니다는를 사용 하 여 동일한 서비스의 일부입니다. Azure 서비스 버스는 자체 SLA와 별도 서비스입니다. 서비스 버스 큐를 사용 한다면 우리는 추가 SLA 백분율에 영향을 주지 있고 복합 SLA는 작아야 합니다. 큐 서비스를 선택할 때 사용자가 선택한 응용 프로그램 가용성에 미치는 영향을 이해 해야 합니다. 자세한 내용은 참조는 [리소스](#resources) 섹션.

## <a name="creating-queue-messages"></a>큐 메시지 만들기

수정 작업을 큐에 배치, 하기 웹 프런트 엔드는 다음 단계를 수행 합니다.

1. 만들기는 [CloudQueueClient](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueueclient.aspx) 인스턴스. `CloudQueueClient` 인스턴스를 사용 하 여 큐 서비스에 대 한 요청을 실행할 수 있습니다.
2. 아직 존재 하지 않는 경우 큐를 만듭니다.
3. 수정 작업을 직렬화 합니다.
4. 호출 [CloudQueue.AddMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync.aspx) 큐로 메시지를 넣을 수 있습니다.

생성자에서이 작업을 수행 하겠습니다 및 `SendMessageAsync` 새 방식의 `FixItQueueManager` 클래스입니다.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample1.cs?highlight=11-12,16,18-25)]

사용 하 여 여기에서 [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) fixit JSON 형식으로 serialize 하는 라이브러리입니다. 원하는 어떤 serialization 방법을 사용할 수 있습니다. JSON을 사용 하면 XML 보다 정보가 덜 자세 하면서 사람이 읽을 수 있다는 이점이 있습니다.

프로덕션 품질의 코드는 오류 처리 논리를 추가, 데이터베이스를 사용할 수 없게 하는 경우 일시 중지, 복구를 더욱 명확 하 게 처리, 응용 프로그램 시작 시에 큐를 만들려면 및 관리 "[포이즌" 메시지](https://msdn.microsoft.com/library/ms789028(v=vs.110).aspx)합니다. (포이즌 메시지는 몇 가지 이유로 처리할 수 없는 메시지. 않으려면 포이즌 메시지 큐에 있으면 여기서 작업자 역할에서 지속적으로 하려고 처리, 실패, 다시 시도 실패 및 등입니다.)

프런트 엔드 MVC 응용 프로그램에서 새 작업을 만드는 코드를 업데이트 해야 합니다. 호출 하는 대신 작업 저장소로,는 `SendMessageAsync` 위에 표시 된 메서드가 있습니다.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample2.cs?highlight=10)]

## <a name="processing-queue-messages"></a>큐 메시지 처리

큐에서 메시지를 처리 하려면 백 엔드 서비스를 만들어 보겠습니다. 백 엔드 서비스는 다음 단계를 수행 하는 무한 루프가 실행 됩니다.

1. 큐에서 다음 메시지를 가져옵니다.
2. 수정 작업에 메시지를 deserialize 합니다.
3. 데이터베이스에 수정 작업을 기록 합니다.

포함 된 Azure 클라우드 서비스에서 백 엔드 서비스를 호스트 만들겠습니다는 *작업자 역할*합니다. 작업자 역할 백 엔드 처리를 수행할 수 있는 하나 이상의 Vm으로 구성 됩니다. 이러한 Vm에서 실행 되는 코드는 제공 될 때 큐에서 메시지를 가져옵니다. 각 메시지에 대 한에서는 JSON 페이로드를 deserialize 하 고 웹 계층 앞부분에서 사용한 동일한 저장소를 사용 하 여 데이터베이스에 해결 것 작업 엔터티의 인스턴스를 작성 합니다.

다음 단계를 작업자를 추가 하는 방법에 표준 웹 프로젝트가 있는 솔루션에 역할 프로젝트를 보여 줍니다. 다음이 단계 수정 프로젝트에서 다운로드할 수 있는 이미 완료 된 합니다.

먼저 클라우드 서비스 프로젝트를 Visual Studio 솔루션에 추가 합니다. 솔루션을 마우스 오른쪽 단추로 클릭 하 고 선택 **추가**, 다음 **새 프로젝트**합니다. 왼쪽된 창에서 확장 **Visual C#** 선택 **클라우드**합니다.

[![](queue-centric-work-pattern/_static/image5.png)](queue-centric-work-pattern/_static/image4.png)

에 **새 Azure 클라우드 서비스** 대화 상자를 확장 하 고는 **Visual C#** 왼쪽된 창에서 노드. 선택 **작업자 역할** 오른쪽 화살표 아이콘을 클릭 합니다.

![](queue-centric-work-pattern/_static/image6.png)

(에 추가할 수도 있습니다는 *웹 역할*합니다. Fix It Azure 웹 사이트에서 실행 하는 대신 동일한 클라우드 서비스에서 프런트 엔드 여 실행할 수 있습니다. 프런트 엔드 및 백 엔드 간의 연결을 쉽게 조정할 수 있도록 몇 가지 장점이 있는 합니다. 그러나이 데모를 간단히 유지 하기 위해는 현재 Azure 앱 서비스 웹 앱에 프런트 엔드 유지로 클라우드 서비스에서 백 엔드를 실행 합니다.)

작업자 역할에 기본 이름이 할당 됩니다. 이름을 변경 하려면 오른쪽 창에서 작업자 역할 위로 마우스를 가져가고 연필 아이콘을 클릭 합니다.

![](queue-centric-work-pattern/_static/image7.png)

클릭 **확인** 대화 상자를 완료 합니다. 이 Visual Studio 솔루션에 두 개의 프로젝트를 추가합니다.

- Azure 프로젝트의 구성 정보를 포함 하 여 클라우드 서비스를 정의 합니다.
- 작업자 역할을 정의 하는 작업자 역할 프로젝트.

![](queue-centric-work-pattern/_static/image8.png)

자세한 내용은 참조 [Visual studio에서 Azure 프로젝트 만들기.](https://msdn.microsoft.com/library/windowsazure/ee405487.aspx)

작업자 역할 내 폴링할 메시지에 대 한 호출 하 여는 `ProcessMessageAsync` 의 메서드는 `FixItQueueManager` 앞에서 살펴본 클래스입니다.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample3.cs?highlight=25)]

`ProcessMessagesAsync` 메서드 메시지 대기 인지 여부를 확인 합니다. 에 메시지를 역직렬화 노드가 있을 경우는 `FixItTask` 엔터티 및 엔터티를 데이터베이스에 저장 합니다. 큐가 비어 될 때까지 반복 합니다.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample4.cs)]

큐 메시지 작은 트랜잭션이 발생에 대해 폴링하면 요금을 청구, 메시지가 처리 되기를 기다리는 경우 작업자 역할의 `RunAsync` 메서드가 폴링하기 전에 두 번째 다시 대기를 호출 하 여 `Task.Delay(1000)`합니다.

웹 프로젝트에서 비동기 코드를 추가 합니다. 자동으로 성능을 향상 시킬 수 IIS 제한 된 스레드 풀을 관리 하기 때문입니다. 작업자 역할 프로젝트에서 아닙니다. 작업자 역할의 확장성을 높이기 위해 다중 스레드 코드를 작성 하거나 비동기 코드를 사용 하 여 구현할 수 있습니다 [병렬 프로그래밍](https://msdn.microsoft.com/library/ff963553.aspx)합니다. 샘플 병렬 프로그래밍을 구현 하지 않는 있지만 병렬 프로그래밍을 구현할 수 있도록 코드를 비동기화 하는 방법을 보여 줍니다.

## <a name="summary"></a>요약

이 장에서 큐 중심 작업 패턴을 구현 하 여 응용 프로그램 응답 속도, 안정성 및 확장성을 개선 하는 방법을 살펴보았습니다.

이것은이 전자책에서 다루는 13 패턴의 마지막 있고은 물론 다른 패턴을 다양 하 게 하는 사례 성공적인 클라우드 앱을 빌드할 합니다. [최종 장](more-patterns-and-guidance.md) 이러한 13 패턴에서 다뤄진 하지 않은 항목에 대 한 리소스 링크를 제공 합니다.

<a id="resources"></a>
## <a name="resources"></a>리소스

큐에 대 한 자세한 내용은 다음 리소스를 참조 합니다.

설명서:

- [Microsoft Azure 저장소 큐 1 부: Getting Started](http://justazure.com/microsoft-azure-storage-queues-part-1-getting-started/)합니다. 문서, 로마 Schacherl 합니다.
- [실행 중인 백그라운드 작업이](https://msdn.microsoft.com/library/ff803365.aspx)의 5 장을 [3rd Edition 클라우드로 응용 프로그램 이동](https://msdn.microsoft.com/library/ff728592.aspx) Microsoft Patterns and Practices에서 합니다. (특히, 섹션 ["Azure 저장소 큐를 사용 하 여"](https://msdn.microsoft.com/library/ff803365.aspx#sec7).)
- [확장성 및 Azure에서 큐 기반 메시징 솔루션의 비용된 효율성을 극대화 하기 위한 유용한](https://msdn.microsoft.com/library/windowsazure/hh697709.aspx)합니다. 백서: Valery Mizonov 합니다.
- [Azure 큐 및 서비스 버스 큐 비교](https://msdn.microsoft.com/magazine/jj159884.aspx)합니다. MSDN Magazine 문서를 사용 하는 큐 서비스를 선택 하는 데 도움이 되는 추가 정보를 제공 합니다. 문서에 서비스 버스 ACS를 사용할 수 없는 경우 SB 큐는 사용할 수 없습니다 것을 의미 하는 인증에 대 한 ACS에 종속 되어 있는지 설명 합니다. 그러나 SB로 사용할 수 있도록 변경 되었습니다 문서 기록 되었기 때문에 [SAS 토큰](https://msdn.microsoft.com/library/windowsazure/dn170477.aspx) ACS 하는 대신 합니다.
- [Microsoft Patterns and Practices-Azure 지침](https://msdn.microsoft.com/library/dn568099.aspx)합니다. 비동기 메시징 입문서, 파이프와 필터 패턴, 트랜잭션을 보정 패턴, 경쟁 소비자 패턴, CQRS 패턴을 참조 하십시오.
- [CQRS 정보](https://msdn.microsoft.com/library/jj554200)합니다. Microsoft Patterns and Practices 여 CQRS에 대 한 전자책을 참고 합니다.

비디오:

- [FailSafe: 복원 력 있는 확장 가능한 클라우드 서비스를 만드는](https://channel9.msdn.com/Series/FailSafe)합니다. Marc Mercuri, Ulrich Homann, Mark Simms 하 여 비디오 시리즈를 9 개 부분으로 구성 합니다. 고급 개념 및 아키텍처 원칙 매우 액세스 가능 하 고 흥미로운 방법으로 스토리 실제 고객과 Microsoft 고객 자문 팀 (CAT) 환경에서 가져온 것으로 표시 합니다. Azure 저장소 서비스 및 큐 소개, 에피소드 5 35:13에서 시작을 참조 하십시오.

>[!div class="step-by-step"]
[이전](distributed-caching.md)
[다음](more-patterns-and-guidance.md)
