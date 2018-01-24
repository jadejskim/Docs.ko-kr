---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: "배치 ModalPopup (VB) | Microsoft Docs"
author: wenz
description: "들어에서 ModalPopup 컨트롤을 클라이언트 쪽 방법을 사용 하 여 모달 팝업을 만드는 간단한 방법을 제공 합니다. 그러나 컨트롤을 제공 하지 않습니다는 중..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 4b9c0790d1696bcf478bcdea089d4d3d92450369
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2017
---
<a name="positioning-a-modalpopup-vb"></a><span data-ttu-id="f1850-104">ModalPopup (VB) 위치 지정</span><span class="sxs-lookup"><span data-stu-id="f1850-104">Positioning a ModalPopup (VB)</span></span>
====================
<span data-ttu-id="f1850-105">으로 [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f1850-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f1850-106">[코드를 다운로드](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) 또는 [PDF 다운로드](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="f1850-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span></span>

> <span data-ttu-id="f1850-107">들어에서 ModalPopup 컨트롤을 클라이언트 쪽 방법을 사용 하 여 모달 팝업을 만드는 간단한 방법을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="f1850-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="f1850-108">그러나 컨트롤의 팝업 위치를 기본 제공 기능을 제공 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f1850-108">However the control does not offer a built-in functionality to position the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="f1850-109">개요</span><span class="sxs-lookup"><span data-stu-id="f1850-109">Overview</span></span>

<span data-ttu-id="f1850-110">들어에서 ModalPopup 컨트롤을 클라이언트 쪽 방법을 사용 하 여 모달 팝업을 만드는 간단한 방법을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="f1850-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="f1850-111">그러나 컨트롤의 팝업 위치를 기본 제공 기능을 제공 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f1850-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="f1850-112">단계</span><span class="sxs-lookup"><span data-stu-id="f1850-112">Steps</span></span>

<span data-ttu-id="f1850-113">ASP.NET AJAX 및 제어 Toolkit의 기능을 활성화 하는 데는 `ScriptManager`합니다.</span><span class="sxs-lookup"><span data-stu-id="f1850-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="f1850-114">제어 페이지에서 아무 곳 이나 배치 해야 합니다 (하지만 내는 `<form>` 요소):</span><span class="sxs-lookup"><span data-stu-id="f1850-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="f1850-115">다음으로 모달 팝업으로 사용 되는 패널을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="f1850-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="f1850-116">단추는 팝업을 닫습니다 ï ´ ù.</span><span class="sxs-lookup"><span data-stu-id="f1850-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="f1850-117">팝업이 표시 될 때마다 페이지에서 특정 위치에 배치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f1850-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="f1850-118">이 작업에 대 한 클라이언트 쪽 JavaScript 함수가 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f1850-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="f1850-119">먼저 패널에 액세스 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="f1850-119">It first tries to access the panel.</span></span> <span data-ttu-id="f1850-120">성공 하면 패널의 위치는 CSS 및 JavaScript (팝업에서의 위치는 변경)를 사용 하 여 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f1850-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="f1850-121">그러나 `ModalPopupExtender` 컨트롤이 또한 팝업 배치 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="f1850-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="f1850-122">따라서 JavaScript 코드를 반복 해 서 초의 1/10 마다 팝업을 배치합니다.</span><span class="sxs-lookup"><span data-stu-id="f1850-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

<span data-ttu-id="f1850-123">볼 수 있듯이의 반환 값은 `setTimeout()` JavaScript 메서드는 전역 변수에 저장 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f1850-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="f1850-124">따라서 필요에 따라 팝업의 반복 된 배치 중지를 사용 하는 `clearTimeout()` 메서드:</span><span class="sxs-lookup"><span data-stu-id="f1850-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

<span data-ttu-id="f1850-125">이제을 마지막으로 하는 필요에 따라 이러한 함수를 호출 하는 브라우저를 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="f1850-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="f1850-126">`movePanel()` 패널을 트리거하는 단추를 클릭할 때 JavaScript 함수를 호출 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f1850-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

<span data-ttu-id="f1850-127">및 `stopMoving()` 팝업을 닫을 수이 있습니다 하는 경우에 발생 되는 함수에서 트리거할 수는 `ModalPopupExtender` 제어:</span><span class="sxs-lookup"><span data-stu-id="f1850-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]


<span data-ttu-id="f1850-128">[![지정 된 위치에 모달 팝업 표시](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f1850-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="f1850-129">지정 된 위치에 모달 팝업 표시 ([전체 크기 이미지를 보려면 클릭](positioning-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f1850-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="f1850-130">이전</span><span class="sxs-lookup"><span data-stu-id="f1850-130">Previous</span></span>](handling-postbacks-from-a-modalpopup-vb.md)