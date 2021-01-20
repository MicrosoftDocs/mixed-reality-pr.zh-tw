---
title: 目光和停駐
description: 取得混合現實應用程式的眼睛和前端和停留輸入模型的一般總覽。
author: sostel
ms.author: sostel
ms.date: 10/31/2019
ms.topic: article
keywords: 混合的現實、注視、停留、互動、設計、眼睛追蹤、head 追蹤、混合現實耳機、windows mixed Reality 耳機、虛擬實境耳機、HoloLens、MRTK、混合現實工具組
ms.openlocfilehash: aa4fceeb8875da89fd7f84c3709ff6db07fd96f4
ms.sourcegitcommit: d3a3b4f13b3728cfdd4d43035c806c0791d3f2fe
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/20/2021
ms.locfileid: "98582133"
---
# <a name="gaze-and-dwell"></a><span data-ttu-id="cdc36-104">目光和停駐</span><span class="sxs-lookup"><span data-stu-id="cdc36-104">Gaze and dwell</span></span>

<span data-ttu-id="cdc36-105">當手上拿著工具和組件時，手勢可以很繁瑣或不可能進行。</span><span class="sxs-lookup"><span data-stu-id="cdc36-105">When hands are occupied with tools and parts, gestures can be tedious or impossible.</span></span>
<span data-ttu-id="cdc36-106">在某些內容中，語音命令也可能不可靠，例如在過高的情況下。</span><span class="sxs-lookup"><span data-stu-id="cdc36-106">Voice commands may also be unreliable in certain contexts, for example under excessively loud conditions.</span></span>
<span data-ttu-id="cdc36-107">「注視」和「停留」提供了一種熟悉且簡單的主要機制，可讓您在 HoloLens 上使用並無人參與。</span><span class="sxs-lookup"><span data-stu-id="cdc36-107">Gaze and dwell offers a familiar and easy-to-master mechanism for working heads-up and hands-free on HoloLens.</span></span>
<span data-ttu-id="cdc36-108">此外，「注視」和「停留」是很棒的回復，與操作環境中的雜訊干擾或無回應條件無關。</span><span class="sxs-lookup"><span data-stu-id="cdc36-108">Additionally, gaze and dwell is a great fallback, which is independent of noise interference or silence constraints in the operating environment.</span></span>
<span data-ttu-id="cdc36-109">我們將看看兩種不同的眼睛 _和停留_： [前端和停留](gaze-and-dwell-head.md) ，以及 [眼睛和停留](gaze-and-dwell-eyes.md)。</span><span class="sxs-lookup"><span data-stu-id="cdc36-109">We distinguish two variants of _gaze and dwell_: [Head-gaze and dwell](gaze-and-dwell-head.md) and [Eye-gaze and dwell](gaze-and-dwell-eyes.md).</span></span>

## <a name="scenarios"></a><span data-ttu-id="cdc36-110">案例</span><span class="sxs-lookup"><span data-stu-id="cdc36-110">Scenarios</span></span>

<span data-ttu-id="cdc36-111">當某人的手中有其他工作忙碌的情況下，看看並停留擅長，因為環境或社交條件約束，所以語音不是100% 可靠或無法使用。</span><span class="sxs-lookup"><span data-stu-id="cdc36-111">Gaze and dwell excels in scenarios where a person's hands are busy with other tasks, and voice isn't 100% reliable or available because of environmental or social constraints.</span></span>
<span data-ttu-id="cdc36-112">穿戴 HoloLens 的人員在修理汽車引擎時覆疊參考資訊就是個不錯的範例。</span><span class="sxs-lookup"><span data-stu-id="cdc36-112">A good example is a person wearing a HoloLens to overlay reference information while repairing a car engine.</span></span>
<span data-ttu-id="cdc36-113">他們的雙手忙著拿起工具，或在靠向引擎室時支撐其身體。</span><span class="sxs-lookup"><span data-stu-id="cdc36-113">Their hands are busy with tools or supporting their body as they lean into the engine compartment.</span></span>
<span data-ttu-id="cdc36-114">車庫空間很吵雜，充斥著工具的碰撞和唧唧聲，讓語音命令變得難以使用。</span><span class="sxs-lookup"><span data-stu-id="cdc36-114">The garage space is loud, with the constant banging and buzzing of tools, making voice commands difficult.</span></span>
<span data-ttu-id="cdc36-115">注視和停留可讓使用 HoloLens 的人安心地流覽其參考資料，而不會中斷其工作流程。</span><span class="sxs-lookup"><span data-stu-id="cdc36-115">Gaze and dwell allows the person using the HoloLens to confidently navigate their reference material without interrupting their workflow.</span></span>

## <a name="device-support"></a><span data-ttu-id="cdc36-116">裝置支援</span><span class="sxs-lookup"><span data-stu-id="cdc36-116">Device support</span></span>

<table>
    <colgroup>
    <col width="25%" />
    <col width="25%" />
    <col width="25%" />
    <col width="25%" />
    </colgroup>
    <tr>
        <td><span data-ttu-id="cdc36-117"><strong>輸入模型</strong></span><span class="sxs-lookup"><span data-stu-id="cdc36-117"><strong>Input model</strong></span></span></td>
        <td><span data-ttu-id="cdc36-118"><a href="/hololens/hololens1-hardware"><strong>HoloLens (第 1 代)</strong></a></span><span class="sxs-lookup"><span data-stu-id="cdc36-118"><a href="/hololens/hololens1-hardware"><strong>HoloLens (1st gen)</strong></a></span></span></td>
        <td><span data-ttu-id="cdc36-119"><a href="https://docs.microsoft.com/hololens/hololens2-hardware"><strong>HoloLens 2</strong></span><span class="sxs-lookup"><span data-stu-id="cdc36-119"><a href="https://docs.microsoft.com/hololens/hololens2-hardware"><strong>HoloLens 2</strong></span></span></td>
        <td><span data-ttu-id="cdc36-120"><a href="../discover/immersive-headset-hardware-details.md"><strong>沉浸式頭戴裝置</strong></a></span><span class="sxs-lookup"><span data-stu-id="cdc36-120"><a href="../discover/immersive-headset-hardware-details.md"><strong>Immersive headsets</strong></a></span></span></td>
    </tr>
     <tr>
        <td><span data-ttu-id="cdc36-121">頭部注視並佇留</span><span class="sxs-lookup"><span data-stu-id="cdc36-121">Head-gaze and dwell</span></span></td>
        <td><span data-ttu-id="cdc36-122">✔️ 建議使用</span><span class="sxs-lookup"><span data-stu-id="cdc36-122">✔️ Recommended</span></span></td>
        <td><span data-ttu-id="cdc36-123">✔️ 建議使用</span><span class="sxs-lookup"><span data-stu-id="cdc36-123">✔️ Recommended</span></span></td>
        <td><span data-ttu-id="cdc36-124">✔️ 建議使用</span><span class="sxs-lookup"><span data-stu-id="cdc36-124">✔️ Recommended</span></span></td>
    </tr>
     <tr>
        <td><span data-ttu-id="cdc36-125">眼部目光和停駐</span><span class="sxs-lookup"><span data-stu-id="cdc36-125">Eye-gaze and dwell</span></span></td>
        <td><span data-ttu-id="cdc36-126">❌ 無法使用</span><span class="sxs-lookup"><span data-stu-id="cdc36-126">❌ Not available</span></span></td>
        <td><span data-ttu-id="cdc36-127">✔️ 建議使用</span><span class="sxs-lookup"><span data-stu-id="cdc36-127">✔️ Recommended</span></span></td>
        <td><span data-ttu-id="cdc36-128">❌ 無法使用</span><span class="sxs-lookup"><span data-stu-id="cdc36-128">❌ Not available</span></span></td>
    </tr>
</table>


<br>

---

 ## <a name="see-also"></a><span data-ttu-id="cdc36-129">另請參閱</span><span class="sxs-lookup"><span data-stu-id="cdc36-129">See also</span></span>

* [<span data-ttu-id="cdc36-130">眼動式互動</span><span class="sxs-lookup"><span data-stu-id="cdc36-130">Eye-based interaction</span></span>](eye-gaze-interaction.md)
* [<span data-ttu-id="cdc36-131">HoloLens 2 的眼球追蹤</span><span class="sxs-lookup"><span data-stu-id="cdc36-131">Eye tracking on HoloLens 2</span></span>](eye-tracking.md)
* [<span data-ttu-id="cdc36-132">目光和行動</span><span class="sxs-lookup"><span data-stu-id="cdc36-132">Gaze and commit</span></span>](gaze-and-commit.md)
* [<span data-ttu-id="cdc36-133">手 - 直接操作</span><span class="sxs-lookup"><span data-stu-id="cdc36-133">Hands - Direct manipulation</span></span>](direct-manipulation.md)
* [<span data-ttu-id="cdc36-134">手 - 手勢</span><span class="sxs-lookup"><span data-stu-id="cdc36-134">Hands - Gestures</span></span>](gaze-and-commit.md#composite-gestures)
* [<span data-ttu-id="cdc36-135">手 - 指向和行動</span><span class="sxs-lookup"><span data-stu-id="cdc36-135">Hands - Point and commit</span></span>](point-and-commit.md)
* [<span data-ttu-id="cdc36-136">本能互動</span><span class="sxs-lookup"><span data-stu-id="cdc36-136">Instinctual interactions</span></span>](interaction-fundamentals.md)
* [<span data-ttu-id="cdc36-137">語音輸入</span><span class="sxs-lookup"><span data-stu-id="cdc36-137">Voice input</span></span>](voice-input.md)