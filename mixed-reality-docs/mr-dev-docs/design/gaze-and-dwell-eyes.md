---
title: 眼部目光和停駐
description: 眼部目光和停駐輸入模型的概觀
author: sostel
ms.author: sostel
ms.date: 10/29/2019
ms.topic: article
ms.localizationpriority: high
keywords: 眼球追蹤, Mixed Reality, 輸入, 眼部注視, 眼部定向, HoloLens 2, 眼動式選取, 停駐
ms.openlocfilehash: b7c2b22c24336813231e7e5ea4742f03b9d78004
ms.sourcegitcommit: 09599b4034be825e4536eeb9566968afd021d5f3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/03/2020
ms.locfileid: "91696754"
---
# <a name="eye-gaze-and-dwell"></a><span data-ttu-id="5d33f-104">眼部目光和停駐</span><span class="sxs-lookup"><span data-stu-id="5d33f-104">Eye-gaze and dwell</span></span>

<span data-ttu-id="5d33f-105">「眼部目光和停駐」  互動模型是[眼部目光和行動](gaze-and-commit.md)互動模型的一個特殊案例：</span><span class="sxs-lookup"><span data-stu-id="5d33f-105">The _"eye-gaze and dwell"_ interaction model is a special case of the [eye-gaze and commit](gaze-and-commit.md) interaction model:</span></span>
1. <span data-ttu-id="5d33f-106">只需觀看目標，而</span><span class="sxs-lookup"><span data-stu-id="5d33f-106">Simply look at a target and</span></span> 
2. <span data-ttu-id="5d33f-107">若要確認您選取此目標的意圖，請執行次要明確輸入：只要持續看著您要選取的目標。 </span><span class="sxs-lookup"><span data-stu-id="5d33f-107">To confirm your intention to select the target, perform a secondary explicit input: _Simply keep looking at the target you would like to select_ .</span></span>

## <a name="advantages-of-the-eye-gaze-and-dwell-interaction-model"></a><span data-ttu-id="5d33f-108">眼部目光和停駐互動模型的優點</span><span class="sxs-lookup"><span data-stu-id="5d33f-108">Advantages of the "eye-gaze and dwell" interaction model</span></span> 
<span data-ttu-id="5d33f-109">當您的手上已有工作或持有工具時，可能沒辦法使用雙手來與全像投影互動。</span><span class="sxs-lookup"><span data-stu-id="5d33f-109">When your hands are already occupied with a task or holding tools, using them for interacting with holograms may not be an option.</span></span>
<span data-ttu-id="5d33f-110">選取全像投影的無手互動方式是「眼部目光和停駐」，也就是「觀看並注視」  。</span><span class="sxs-lookup"><span data-stu-id="5d33f-110">A hands-free interaction alternative for selecting holograms is "eye-gaze and dwell" or in other words: _"look and stare"_ .</span></span> <span data-ttu-id="5d33f-111">這種方法的優點是，即使是可能無法完全轉動頭部或身體的嚴重受限使用者，也可以與全像投影互動 (例如，在高度限制的工作環境中)。</span><span class="sxs-lookup"><span data-stu-id="5d33f-111">The advantage of this approach is that even severely constrained users who may not be able to fully turn their heads or bodies can interact with holograms (e.g., in a highly confined work environment).</span></span>
<span data-ttu-id="5d33f-112">使用者只要持續看著想要選取的目標，螢幕上就會顯示不同的停駐回饋來指示進度。</span><span class="sxs-lookup"><span data-stu-id="5d33f-112">The user simply keeps looking at the target they would like to select and different dwell feedback is displayed to indicate the process.</span></span>


## <a name="challenges-of-the-eye-gaze-and-dwell-interaction-model"></a><span data-ttu-id="5d33f-113">眼部目光和停駐互動模型的挑戰</span><span class="sxs-lookup"><span data-stu-id="5d33f-113">Challenges of the "eye-gaze and dwell" interaction model</span></span>
<span data-ttu-id="5d33f-114">一般來說，我們建議您只在無法使用語音輸入或手部輸入的情況下，才使用停駐型互動來做為最後的遞補。</span><span class="sxs-lookup"><span data-stu-id="5d33f-114">In general, we  recommend to only use dwell-based activations as a last fall-back if neither voice input nor hand input is available.</span></span> <span data-ttu-id="5d33f-115">原因是停駐時間的選擇可能會很棘手。</span><span class="sxs-lookup"><span data-stu-id="5d33f-115">The reason is that the choice of dwell time can be pretty tricky.</span></span> <span data-ttu-id="5d33f-116">新手使用者可以接受較長時間的停駐，而熟練的使用者則想要較快速有效率的瀏覽體驗。</span><span class="sxs-lookup"><span data-stu-id="5d33f-116">Novice users are ok with longer dwell times, while expert users want to quickly and efficiently navigate through their experiences.</span></span> <span data-ttu-id="5d33f-117">這會導致如何將停駐時間調整為使用者特定需求的挑戰。</span><span class="sxs-lookup"><span data-stu-id="5d33f-117">This leads to the challenge of how to adjust the dwell time to the specific needs of a user.</span></span>
<span data-ttu-id="5d33f-118">如果停駐時間太短：使用者可能會因為全像投影隨時都在反應其眼部目光而應接不暇。</span><span class="sxs-lookup"><span data-stu-id="5d33f-118">If the dwell time is too short: The user may feel overwhelmed by having holograms react to their eye-gaze all the time.</span></span> <span data-ttu-id="5d33f-119">如果停駐時間太長：體驗可能會變得太慢和斷續，因為使用者必須一直長時間看著目標。</span><span class="sxs-lookup"><span data-stu-id="5d33f-119">If the dwell time is too long: The experience may feel too slow and interruptive as the user has to keep looking at targets for a long time.</span></span>

## <a name="design-recommendations"></a><span data-ttu-id="5d33f-120">設計建議</span><span class="sxs-lookup"><span data-stu-id="5d33f-120">Design recommendations</span></span>
<span data-ttu-id="5d33f-121">我們建議您對停駐回饋採取兩個狀態的做法：</span><span class="sxs-lookup"><span data-stu-id="5d33f-121">We recommend using a two-state approach for dwell feedback:</span></span>
1. <span data-ttu-id="5d33f-122">啟動延遲  ：當使用者開始注視目標時，不要立即發生任何事，因為這可能會導致使用者應接不暇的不愉快體驗。</span><span class="sxs-lookup"><span data-stu-id="5d33f-122">*Onset delay* : When the user starts looking at a target, nothing should immediately happen as this may result in an unpleasant and overwhelming user experience.</span></span> <span data-ttu-id="5d33f-123">改成啟動計時器來偵測使用者是刻意在注視目標或只是瞥了一眼。</span><span class="sxs-lookup"><span data-stu-id="5d33f-123">Instead start a timer to detect whether the user is intentionally staring at the target or merely glancing over it.</span></span>
<span data-ttu-id="5d33f-124">我們建議給定約 150-250 毫秒的啟動時間 (表示使用者關注與尋找大型目標)。</span><span class="sxs-lookup"><span data-stu-id="5d33f-124">We recommend an onset time of 150-250 ms in a given proximity (meaning the user is fixating vs. looking around on a large target).</span></span>  
2. <span data-ttu-id="5d33f-125">啟動停駐回饋  ：確定使用者刻意注視目標後，則會開始顯示停駐回饋，來通知使用者停駐啟動已經開始。</span><span class="sxs-lookup"><span data-stu-id="5d33f-125">*Start dwell feedback:* After ensuring that the user is intentionally looking at the target, start showing dwell feedback to inform the user that the dwell activation is being initiated.</span></span> 
3. <span data-ttu-id="5d33f-126">連續回饋  ：當使用者持續看著目標時，顯示持續的進度指示器，讓使用者知道他們必須繼續看著目標。</span><span class="sxs-lookup"><span data-stu-id="5d33f-126">*Continuous Feedback:* While the user keeps looking at the target, show a continuous progress indicator so that the user knows that they have to keep looking at the target.</span></span> <span data-ttu-id="5d33f-127">特別是針對眼部目光輸入，我們建議先從較大的圓圈或球體開始，再逐漸縮小圓圈或球體，以抓住使用者的視覺注意  。</span><span class="sxs-lookup"><span data-stu-id="5d33f-127">In particular for eye-gaze input, we recommend _pulling in the user's visual attention_ by starting out with a bigger circle or sphere that contracts into a smaller version.</span></span> <span data-ttu-id="5d33f-128">顯示最終狀態的指示器 (小圓圈)，協助與使用者溝通何時將完成停駐完成。</span><span class="sxs-lookup"><span data-stu-id="5d33f-128">Showing an indicator for the final state (small circle) helps to communicate to the user when the dwell will be finished.</span></span> <span data-ttu-id="5d33f-129">下方有範例圖。</span><span class="sxs-lookup"><span data-stu-id="5d33f-129">An example illustration is shown below.</span></span> 
4. <span data-ttu-id="5d33f-130">完成  ：如果使用者繼續關注目標 (再 650-850 毫秒)，便完成停駐啟動並選取注視的目標。</span><span class="sxs-lookup"><span data-stu-id="5d33f-130">*Finish:* If the user kept fixating the target (for another 650-850 ms), complete the dwell activation and select the looked-at target.</span></span>

![停駐狀態](images/eyes_dwellstate_recommendation.png)<br>

## <a name="see-also"></a><span data-ttu-id="5d33f-132">另請參閱</span><span class="sxs-lookup"><span data-stu-id="5d33f-132">See also</span></span>
* [<span data-ttu-id="5d33f-133">眼球追蹤</span><span class="sxs-lookup"><span data-stu-id="5d33f-133">Eye tracking</span></span>](eye-tracking.md)
* [<span data-ttu-id="5d33f-134">眼部目光和行動</span><span class="sxs-lookup"><span data-stu-id="5d33f-134">Eye-gaze and commit</span></span>](gaze-and-commit-eyes.md)
* [<span data-ttu-id="5d33f-135">目光和行動</span><span class="sxs-lookup"><span data-stu-id="5d33f-135">Gaze and commit</span></span>](gaze-and-commit.md)
* [<span data-ttu-id="5d33f-136">目光和停駐</span><span class="sxs-lookup"><span data-stu-id="5d33f-136">Gaze and dwell</span></span>](gaze-and-dwell.md)
* [<span data-ttu-id="5d33f-137">語音輸入</span><span class="sxs-lookup"><span data-stu-id="5d33f-137">Voice input</span></span>](../out-of-scope/voice-design.md)
