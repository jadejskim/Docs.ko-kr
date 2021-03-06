---
title: "기타 API"
author: rick-anderson
description: "이 문서에서는 ASP.NET Core 데이터 보호 ISecret 인터페이스에 설명 합니다."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 26474de3a1c681a8c7f8b7812e9ef859f5d448bb
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2018
---
# <a name="miscellaneous-apis"></a>기타 API

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> 다음 인터페이스 중 하나를 구현 하는 형식은 스레드로부터 안전 해야 합니다. 여러 호출자에 대 한 합니다.

## <a name="isecret"></a>ISecret

`ISecret` 인터페이스 예: 암호화 키 자료의 비밀 값을 나타냅니다. 다음 API 화면을 포함 됩니다.

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

`WriteSecretIntoBuffer` 메서드는 원시 암호 값을 사용 하 여 제공 된 버퍼를 채웁니다. 이 API는 매개 변수로 버퍼를 사용 하는 이유 반환 하는 대신 한 `byte[]` 직접는이 기회 호출자에 게는 관리 되는 가비지 수집기에 대 한 보안 노출을 제한 하는 버퍼 개체를 고정 됩니다.

`Secret` 형식이의 구체적인 구현을 `ISecret` 비밀 값 처리 중인 메모리에 저장 합니다. Windows 플랫폼에서 암호 값이를 통해 암호화 [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx)합니다.
