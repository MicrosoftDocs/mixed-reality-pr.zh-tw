---
title: 語音輸入
description: 在 HoloLens 2 和 Unreal 引擎中設定及使用語音輸入的教學課程
author: hferrone
ms.author: v-hferrone
ms.date: 06/10/2020
ms.topic: article
keywords: Windows Mixed Reality、Unreal、Unreal Engine 4、UE4、HoloLens 2、語音、語音輸入、語音辨識、混合現實、開發、功能、檔、指南、全息全像遊戲開發
ms.openlocfilehash: 88ab39de5f219691a6c3fe5b4ad3008d9614668e
ms.sourcegitcommit: 09599b4034be825e4536eeb9566968afd021d5f3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/03/2020
ms.locfileid: "91682241"
---
# <a name="voice-input-in-unreal"></a><span data-ttu-id="55830-104">Unreal 中的語音輸入</span><span class="sxs-lookup"><span data-stu-id="55830-104">Voice Input in Unreal</span></span>

## <a name="overview"></a><span data-ttu-id="55830-105">概觀</span><span class="sxs-lookup"><span data-stu-id="55830-105">Overview</span></span>
<span data-ttu-id="55830-106">Unreal 中的語音輸入可讓您與全息圖互動，而不需要使用手勢，且僅 HoloLens 2 支援。</span><span class="sxs-lookup"><span data-stu-id="55830-106">Voice input in Unreal allows you to interact with a hologram without having to use hand gestures and is only supported HoloLens 2.</span></span> <span data-ttu-id="55830-107">雖然 HoloLens 2 上的語音輸入是由在所有其他通用 Windows 應用程式中支援語音的相同引擎所提供，但 Unreal 會使用它自己的較有限引擎來處理語音輸入。</span><span class="sxs-lookup"><span data-stu-id="55830-107">Even though voice input on HoloLens 2 is powered by the same engine that supports speech in all other Universal Windows Apps, Unreal uses a more limited engine of its own to process voice input.</span></span> <span data-ttu-id="55830-108">這會將 Unreal 中的語音輸入功能限制為預先定義的語音對應，如下節所述。</span><span class="sxs-lookup"><span data-stu-id="55830-108">This limits voice input features in Unreal to predefined speech mappings, which is covered in the following sections.</span></span> 

## <a name="enabling-speech-recognition"></a><span data-ttu-id="55830-109">啟用語音辨識</span><span class="sxs-lookup"><span data-stu-id="55830-109">Enabling Speech Recognition</span></span>

<span data-ttu-id="55830-110">若要在 HoloLens 上啟用語音辨識：</span><span class="sxs-lookup"><span data-stu-id="55830-110">To enable speech recognition on HoloLens:</span></span>
1. <span data-ttu-id="55830-111">選取 [ **專案設定] > 平臺 > HoloLens > 功能** ，並啟用 **麥克風** 。</span><span class="sxs-lookup"><span data-stu-id="55830-111">Select **Project Settings > Platform > HoloLens > Capabilities** and enable **Microphone** .</span></span> 
2. <span data-ttu-id="55830-112">在 [設定] **> 隱私權 > 語音** ] 中啟用語音辨識，然後選取 [ **英文** ]。</span><span class="sxs-lookup"><span data-stu-id="55830-112">Enabled speech recognition in **Settings > Privacy > Speech** and select **English** .</span></span>

> [!NOTE]
> <span data-ttu-id="55830-113">語音辨識一律會以 [ **設定** ] 應用程式中所設定的 Windows 顯示語言運作。</span><span class="sxs-lookup"><span data-stu-id="55830-113">Speech recognition always functions in the Windows display language configured in the **Settings** app.</span></span> <span data-ttu-id="55830-114">建議您也啟用 **線上語音辨識** ，以獲得最佳的服務品質。</span><span class="sxs-lookup"><span data-stu-id="55830-114">It’s recommended that you also enable **Online speech recognition** for the best service quality.</span></span>

![Windows 語音辨識設定](images/unreal/speech-recognition-settings.png)

3. <span data-ttu-id="55830-116">當應用程式第一次開始詢問您是否要啟用麥克風時，就會顯示對話方塊。</span><span class="sxs-lookup"><span data-stu-id="55830-116">A dialog will show up when the application first starts to ask if you want to enable the microphone.</span></span> <span data-ttu-id="55830-117">選取 [ **是]** 會在應用程式中啟動語音輸入。</span><span class="sxs-lookup"><span data-stu-id="55830-117">Selecting **Yes** starts voice input in the app.</span></span>

<span data-ttu-id="55830-118">語音輸入不需要任何特殊的 Windows Mixed Reality Api;它建置於現有的 Unreal Engine 4 [輸入](https://docs.unrealengine.com/Gameplay/Input/index.html) 對應 API 上。</span><span class="sxs-lookup"><span data-stu-id="55830-118">Voice input doesn’t require any special Windows Mixed Reality APIs; it's built on the existing Unreal Engine 4 [Input](https://docs.unrealengine.com/Gameplay/Input/index.html) mapping API.</span></span> 

## <a name="adding-speech-mappings"></a><span data-ttu-id="55830-119">新增語音對應</span><span class="sxs-lookup"><span data-stu-id="55830-119">Adding Speech Mappings</span></span>
<span data-ttu-id="55830-120">使用語音輸入時，連接語音轉換動作是很重要的步驟。</span><span class="sxs-lookup"><span data-stu-id="55830-120">Connecting speech to action is an important step when using voice input.</span></span> <span data-ttu-id="55830-121">這些對應會監視使用者可能說出的語音關鍵字應用程式，然後引發連結的動作。</span><span class="sxs-lookup"><span data-stu-id="55830-121">These mappings monitor the app for speech keywords that a user might say, then fire off a linked action.</span></span> <span data-ttu-id="55830-122">您可以透過下列方式尋找語音對應：</span><span class="sxs-lookup"><span data-stu-id="55830-122">You can find Speech Mappings by:</span></span>
1. <span data-ttu-id="55830-123">選取 [ **編輯] > 專案設定** 、滾動至 [ **引擎** ] 區段，然後按一下 [ **輸入** ]。</span><span class="sxs-lookup"><span data-stu-id="55830-123">Selecting **Edit > Project Settings** , scrolling to the **Engine** section, and clicking **Input** .</span></span>

<span data-ttu-id="55830-124">若要為跳躍命令新增語音對應：</span><span class="sxs-lookup"><span data-stu-id="55830-124">To add a new Speech Mapping for a jump command:</span></span>
1. <span data-ttu-id="55830-125">按一下 [ **+** **陣列元素** ] 旁的圖示，並填寫下列值：</span><span class="sxs-lookup"><span data-stu-id="55830-125">Click the **+** icon next to **Array elements** and fill out the following values:</span></span>
    * <span data-ttu-id="55830-126">**動作名稱** 的 **jumpWord**</span><span class="sxs-lookup"><span data-stu-id="55830-126">**jumpWord** for **Action Name**</span></span>
    * <span data-ttu-id="55830-127">**跳躍\*\*\*\*語音關鍵字**</span><span class="sxs-lookup"><span data-stu-id="55830-127">**jump** for **Speech Keyword**</span></span>

> [!NOTE]
> <span data-ttu-id="55830-128">任何英文字 (s) 或簡短句子 (s) 都可以當做關鍵字使用。</span><span class="sxs-lookup"><span data-stu-id="55830-128">Any English word(s) or short sentence(s) can be used as a keyword.</span></span> 

![UE4 引擎輸入設定](images/unreal/engine-input.png)

<span data-ttu-id="55830-130">語音對應可作為活動圖中的輸入元件，例如動作或軸對應，或作為藍圖節點。</span><span class="sxs-lookup"><span data-stu-id="55830-130">Speech Mappings can be used as Input components like Action or Axis Mappings or as blueprint nodes in the Event Graph.</span></span> <span data-ttu-id="55830-131">例如，您可以連結跳躍命令，根據說出單字的時間，列印出兩個不同的記錄檔：</span><span class="sxs-lookup"><span data-stu-id="55830-131">For example, you could link the jump command to print out two different logs depending on when the word is spoken:</span></span>

1. <span data-ttu-id="55830-132">按兩下藍圖，以在 **事件圖形** 中開啟。</span><span class="sxs-lookup"><span data-stu-id="55830-132">Double-click a blueprint to open it in the **Event Graph** .</span></span>
2. <span data-ttu-id="55830-133">以 **滑鼠右鍵按一下** 並搜尋語音對應的 **動作名稱** (在此案例中為 **JumpWord** ) ，然後按 **Enter 鍵** 。</span><span class="sxs-lookup"><span data-stu-id="55830-133">**Right-click** and search for the **Action Name** of your speech mapping (in this case **jumpWord** ), then hit **Enter** .</span></span> <span data-ttu-id="55830-134">這會將 **輸入動作** 節點新增至圖形。</span><span class="sxs-lookup"><span data-stu-id="55830-134">This adds an **Input Action** node to the graph.</span></span>
3. <span data-ttu-id="55830-135">拖放已 **按下** 的釘選以 **列印字串** 節點，如下圖所示。</span><span class="sxs-lookup"><span data-stu-id="55830-135">Drag and drop the **Pressed** pin to **Print String** node as shown in the image below.</span></span> <span data-ttu-id="55830-136">您可以讓釋 **出的** pin 空白，而不會對語音對應執行任何作業。</span><span class="sxs-lookup"><span data-stu-id="55830-136">You can leave the **Released** pin empty, it won't execute anything for speech mappings.</span></span>
 
![語音的簡單動作](images/unreal/voice-input-img-03.png)

4. <span data-ttu-id="55830-138">播放應用程式，比方說，在主控台 **中，將** 記錄檔列印出記錄！</span><span class="sxs-lookup"><span data-stu-id="55830-138">Play the app, say the word **jump** and watch the console print out the logs!</span></span>

<span data-ttu-id="55830-139">這就是開始在 Unreal 中新增語音輸入到 HoloLens 應用程式所需的所有設定。</span><span class="sxs-lookup"><span data-stu-id="55830-139">That's all the setup you'll need to start adding voice input to your HoloLens apps in Unreal.</span></span> <span data-ttu-id="55830-140">您可以在下列連結找到有關語音和互動的詳細資訊，並務必考慮您為使用者建立的體驗。</span><span class="sxs-lookup"><span data-stu-id="55830-140">You can find more information on speech and interactivity can be found at the links below, and be sure to think about the experience you're creating for your users.</span></span>

## <a name="next-development-checkpoint"></a><span data-ttu-id="55830-141">下一個開發檢查點</span><span class="sxs-lookup"><span data-stu-id="55830-141">Next Development Checkpoint</span></span>

<span data-ttu-id="55830-142">如果您正在遵循我們所配置的 Unreal 開發檢查點旅程，您接下來要探索混合現實平臺功能和 Api：</span><span class="sxs-lookup"><span data-stu-id="55830-142">If you're following the Unreal development checkpoint journey we've laid out, you're next task is exploring the Mixed Reality platform capabilities and APIs:</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="55830-143">HoloLens 相機</span><span class="sxs-lookup"><span data-stu-id="55830-143">HoloLens camera</span></span>](unreal-hololens-camera.md)

<span data-ttu-id="55830-144">您隨時都可以回到 [Unreal 開發檢查點](unreal-development-overview.md#2-core-building-blocks) 。</span><span class="sxs-lookup"><span data-stu-id="55830-144">You can always go back to the [Unreal development checkpoints](unreal-development-overview.md#2-core-building-blocks) at any time.</span></span>

## <a name="see-also"></a><span data-ttu-id="55830-145">另請參閱</span><span class="sxs-lookup"><span data-stu-id="55830-145">See also</span></span>
* [<span data-ttu-id="55830-146">語音輸入</span><span class="sxs-lookup"><span data-stu-id="55830-146">Voice Input</span></span>](../../design/voice-input.md)
* [<span data-ttu-id="55830-147">目光和行動</span><span class="sxs-lookup"><span data-stu-id="55830-147">Gaze and commit</span></span>](../../design/gaze-and-commit.md)
* [<span data-ttu-id="55830-148">本能互動</span><span class="sxs-lookup"><span data-stu-id="55830-148">Instinctual interactions</span></span>](../../design/interaction-fundamentals.md)

