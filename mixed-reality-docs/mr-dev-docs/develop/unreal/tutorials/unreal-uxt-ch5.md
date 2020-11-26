---
title: 5. 新增按鈕並重設棋子位置
description: 教學課程系列的第 5 部分 (共有 6 部分)，使用 Unreal Engine 4 和混合實境工具組 UX 工具外掛程式來建置簡單的國際象棋應用程式
author: hferrone
ms.author: v-hferrone
ms.date: 08/14/2020
ms.topic: article
ms.localizationpriority: high
keywords: Unreal, Unreal Engine 4, UE4, HoloLens, HoloLens 2, 混合實境, 教學課程, 開始使用, mrtk, uxt, UX 工具, 文件, 混合實境頭戴式裝置, windows 混合實境頭戴式裝置, 虛擬實境頭戴式裝置
ms.openlocfilehash: f903848b8d5c9c1dccfc00cd7bd6d16d2e491a5e
ms.sourcegitcommit: dd13a32a5bb90bd53eeeea8214cd5384d7b9ef76
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94679837"
---
# <a name="5-adding-a-button--resetting-piece-locations"></a><span data-ttu-id="4acf6-104">5.新增按鈕並重設棋子位置</span><span class="sxs-lookup"><span data-stu-id="4acf6-104">5. Adding a button & resetting piece locations</span></span>


## <a name="overview"></a><span data-ttu-id="4acf6-105">概觀</span><span class="sxs-lookup"><span data-stu-id="4acf6-105">Overview</span></span>

<span data-ttu-id="4acf6-106">在上一個教學課程中，您已將手動互動動作項目新增至 Pawn，並將操作工具元件新增至國際象棋盤，使其都能夠互動。</span><span class="sxs-lookup"><span data-stu-id="4acf6-106">In the previous tutorial, you added Hand Interaction Actors to the Pawn and Manipulator components to the chess board to make them both interactive.</span></span> <span data-ttu-id="4acf6-107">在本節中，您將建置國際象棋應用程式的功能，來繼續使用混合實境工具組 UX 工具外掛程式。</span><span class="sxs-lookup"><span data-stu-id="4acf6-107">In this section, you'll continue working with the Mixed Reality Toolkit UX Tools plugin by building out the features of your chess app.</span></span> <span data-ttu-id="4acf6-108">這包括建立新的函式，以及了解如何在藍圖中取得動作項目的參考。</span><span class="sxs-lookup"><span data-stu-id="4acf6-108">This includes creating a new function and learning how to get references to Actors in a Blueprint.</span></span> <span data-ttu-id="4acf6-109">本節結束時，您將可以在裝置或模擬器上封裝和部署混合實境應用程式。</span><span class="sxs-lookup"><span data-stu-id="4acf6-109">By the end of this section, you'll be ready to package and deploy the mixed reality app on a device or emulator.</span></span>

## <a name="objectives"></a><span data-ttu-id="4acf6-110">目標</span><span class="sxs-lookup"><span data-stu-id="4acf6-110">Objectives</span></span>

* <span data-ttu-id="4acf6-111">新增互動式按鈕</span><span class="sxs-lookup"><span data-stu-id="4acf6-111">Adding an interactive button</span></span>
* <span data-ttu-id="4acf6-112">建立函式來重設棋子的位置</span><span class="sxs-lookup"><span data-stu-id="4acf6-112">Creating a function to reset a pieces' location</span></span>
* <span data-ttu-id="4acf6-113">連結按鈕以在按下按鈕時觸發函式</span><span class="sxs-lookup"><span data-stu-id="4acf6-113">Hooking the button up to trigger the function when pressed</span></span>

## <a name="creating-a-reset-function"></a><span data-ttu-id="4acf6-114">建立重設函式</span><span class="sxs-lookup"><span data-stu-id="4acf6-114">Creating a reset function</span></span>
<span data-ttu-id="4acf6-115">您的第一項工作是建立函式藍圖，將棋子重設為其在場景中的原始位置。</span><span class="sxs-lookup"><span data-stu-id="4acf6-115">Your first task is to create a function blueprint that resets a chess piece to its original position in the scene.</span></span> 

1.  <span data-ttu-id="4acf6-116">開啟 **WhiteKing**、按一下 [ My 藍圖] 中 [函式] 區段旁的 **+** 圖示，並將其命名為 [Reset Location]。</span><span class="sxs-lookup"><span data-stu-id="4acf6-116">Open **WhiteKing**, click the **+** icon next to the **Functions** section in the **My Blueprint** and name it **Reset Location**.</span></span> 

2.  <span data-ttu-id="4acf6-117">從藍圖格線上的 [Reset Location] 拖曳並放開執行，以建立 **SetActorRelativeTransform** 節點。</span><span class="sxs-lookup"><span data-stu-id="4acf6-117">Drag and release the execution from **Reset Location** on the Blueprint grid to create a **SetActorRelativeTransform** node.</span></span> 
    * <span data-ttu-id="4acf6-118">此函式會為動作項目設定與其父系相對的變形 (位置、旋轉和縮放)。</span><span class="sxs-lookup"><span data-stu-id="4acf6-118">This function sets the transform (location, rotation, and scale) of an actor relative to its parent.</span></span> <span data-ttu-id="4acf6-119">您會使用此函式來重設棋盤上的國王位置，即使棋盤已從其原始位置移動也一樣。</span><span class="sxs-lookup"><span data-stu-id="4acf6-119">You’ll use this function to reset the king’s position on the board, even if the board has been moved from its original position.</span></span> 
    
3. <span data-ttu-id="4acf6-120">在事件圖形內部按一下滑鼠右鍵，選取 [進行轉換]，並將其 [位置] 變更為 **X =-26**、**Y = 4**、**Z = 0**。</span><span class="sxs-lookup"><span data-stu-id="4acf6-120">Right-click inside the Event Graph, select **Make Transform**, and change its **Location** to **X = -26**, **Y = 4**, **Z = 0**.</span></span>
    * <span data-ttu-id="4acf6-121">將其 [傳回值] 連線到 **SetActorRelativeTransform** 中的 [新增相對轉換] 釘選。</span><span class="sxs-lookup"><span data-stu-id="4acf6-121">Connect its **Return Value** to the **New Relative Transform** pin in **SetActorRelativeTransform**.</span></span> 

![重設位置函式](images/unreal-uxt/5-function.PNG)

<span data-ttu-id="4acf6-123">**編譯** 並 **儲存** 專案，然後回到主視窗。</span><span class="sxs-lookup"><span data-stu-id="4acf6-123">**Compile** and **Save** the project before returning to the Main window.</span></span> 


## <a name="adding-a-button"></a><span data-ttu-id="4acf6-124">新增按鈕</span><span class="sxs-lookup"><span data-stu-id="4acf6-124">Adding a button</span></span>
<span data-ttu-id="4acf6-125">既然已正確設定函式，您的下一項工作就是建立按鈕，讓該函式在被觸碰時啟動。</span><span class="sxs-lookup"><span data-stu-id="4acf6-125">Now that the function is setup correctly, your next task is to create a button that fires it off when touched.</span></span> 


1.  <span data-ttu-id="4acf6-126">按一下 [新增] > [藍圖類別]、展開 [所有類別] 區段，然後搜尋 **BP_ButtonHoloLens2**。</span><span class="sxs-lookup"><span data-stu-id="4acf6-126">Click **Add New > Blueprint Class**, expand the **All Classes** section, and search for **BP_ButtonHoloLens2**.</span></span> 
    * <span data-ttu-id="4acf6-127">將其命名為 **ResetButton**，然後按兩下以開啟藍圖</span><span class="sxs-lookup"><span data-stu-id="4acf6-127">Name it **ResetButton** and double click to open the Blueprint</span></span>

> [!NOTE]
> <span data-ttu-id="4acf6-128">**BP_ButtonHoloLens2** 是 3D 按鈕藍圖動作項目，屬於 UX 工具外掛程式。</span><span class="sxs-lookup"><span data-stu-id="4acf6-128">**BP_ButtonHoloLens2** is a 3D button Blueprint Actor that's part of the UX Tools plugin.</span></span>

![從 HoloLens 2 樣式按鈕建立新藍圖的子類別](images/unreal-uxt/5-subclass.PNG)

2. <span data-ttu-id="4acf6-130">確定已在 [元件] 面板中選取 **ResetButton(self)** 。</span><span class="sxs-lookup"><span data-stu-id="4acf6-130">Ensure **ResetButton(self)** is selected in the **Components** panel.</span></span> <span data-ttu-id="4acf6-131">在 [詳細資料] 面板中，瀏覽至 [按鈕] 區段。</span><span class="sxs-lookup"><span data-stu-id="4acf6-131">In the **Details** panel, navigate to the **Button** section.</span></span> <span data-ttu-id="4acf6-132">將預設 [按鈕標籤] 變更為 [重設]。</span><span class="sxs-lookup"><span data-stu-id="4acf6-132">Change the default **Button Label** to "Reset".</span></span> <span data-ttu-id="4acf6-133">展開 [按鈕圖示筆刷] 區段，然後按 [開啟圖示筆刷編輯器] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="4acf6-133">Expand the **Button Icon Brush** section and press the **Open Icon Brush Editor** button.</span></span> 

![設定按鈕上的標籤和圖示](images/unreal-uxt/5-buttonconfig.PNG)

<span data-ttu-id="4acf6-135">這會開啟圖示筆刷編輯器，這是 UX 工具外掛程式所提供的公用程式，可讓您用來選取按鈕的新圖示。</span><span class="sxs-lookup"><span data-stu-id="4acf6-135">This will open up the Icon Brush Editor, which is a utility provided by the UX Tools plugin which you can use to select a new icon for your button.</span></span> 

![選取按鈕的圖示](images/unreal-uxt/5-iconbrusheditor.PNG)

<span data-ttu-id="4acf6-137">此外還有許多設定可供調整，用以設定您的按鈕。</span><span class="sxs-lookup"><span data-stu-id="4acf6-137">There are plenty of other settings you can adjust to configure your button.</span></span> <span data-ttu-id="4acf6-138">若要深入了解 UXT 可點按的按鈕元件，請參閱[文件](https://microsoft.github.io/MixedReality-UXTools-Unreal/version/public/0.9.x/Docs/PressableButton.html)。</span><span class="sxs-lookup"><span data-stu-id="4acf6-138">To learn more about the UXT Pressable Button component, check out the [documentation](https://microsoft.github.io/MixedReality-UXTools-Unreal/version/public/0.9.x/Docs/PressableButton.html).</span></span>

3. <span data-ttu-id="4acf6-139">在 [元件] 面板中按一下 [UxtPressableButton (繼承)]，然後將 [詳細資料] 面板向下捲動至 [事件] 區段。</span><span class="sxs-lookup"><span data-stu-id="4acf6-139">Click **UxtPressableButton (Inherited)** in the **Components** panel and scroll down the **Details** panel to the **Events** section.</span></span> 
    * <span data-ttu-id="4acf6-140">按一下 [On Button Pressed] 旁邊的綠色 **+** 按鈕，將事件新增至事件圖形，如此在按下按鈕時便會呼叫該事件。</span><span class="sxs-lookup"><span data-stu-id="4acf6-140">Click the green **+** button next to **On Button Pressed** to add an event to the Event Graph, which will be called when the button is pressed.</span></span> 
    
<span data-ttu-id="4acf6-141">從這裡開始，您會想要呼叫 **WhiteKing** 的 **Reset Location** 函式，該函式需要參考層級中的 **WhiteKing** 動作項目。</span><span class="sxs-lookup"><span data-stu-id="4acf6-141">From here, you’ll want to call **WhiteKing**’s **Reset Location** function, which needs a reference to the **WhiteKing** Actor in the Level.</span></span> 

4.  <span data-ttu-id="4acf6-142">在 [我的藍圖] 面板中，瀏覽至 [變數] 區段、按一下 **+** 按鈕，並將變數命名為 **WhiteKing**。</span><span class="sxs-lookup"><span data-stu-id="4acf6-142">In the **My Blueprint** panel, navigate to the **Variables** section , click the **+** button and name the variable **WhiteKing**.</span></span> 
    * <span data-ttu-id="4acf6-143">在 [詳細資料] 面板中，選取 [變數類型] 旁的下拉式清單，搜尋 **WhiteKing**，然後選取 [物件參考]。</span><span class="sxs-lookup"><span data-stu-id="4acf6-143">In the **Details** panel, select the dropdown next to **Variable Type**, search for **WhiteKing**, and select the **Object Reference**.</span></span> 
    * <span data-ttu-id="4acf6-144">勾選 [可編輯執行個體] 旁的方塊。</span><span class="sxs-lookup"><span data-stu-id="4acf6-144">Check the box next to **Instance Editable**.</span></span> <span data-ttu-id="4acf6-145">這可讓您從主要層級設定變數。</span><span class="sxs-lookup"><span data-stu-id="4acf6-145">This will allow the variable to be set from the Main Level.</span></span> 

![建立變數](images/unreal-uxt/5-var.PNG)

5.  <span data-ttu-id="4acf6-147">將 WhiteKing 變數從 [我的藍圖] > [變數] 拖曳到重設按鈕事件圖形，然後選擇 [取得 WhiteKing]。</span><span class="sxs-lookup"><span data-stu-id="4acf6-147">Drag the WhiteKing variable from **My Blueprint > Variables** onto the Reset Button Event Graph and choose **Get WhiteKing**.</span></span> 

## <a name="firing-the-function"></a><span data-ttu-id="4acf6-148">啟動函式</span><span class="sxs-lookup"><span data-stu-id="4acf6-148">Firing the function</span></span>
<span data-ttu-id="4acf6-149">剩下的工作就是在按下按鈕時，正式啟動重設函式。</span><span class="sxs-lookup"><span data-stu-id="4acf6-149">All that's left is to officially fire off the reset function when the button is pressed.</span></span>

1.  <span data-ttu-id="4acf6-150">拖放 WhiteKing 輸出連接，以放置新的節點。</span><span class="sxs-lookup"><span data-stu-id="4acf6-150">Drag the WhiteKing output pin and release to place a new node.</span></span> <span data-ttu-id="4acf6-151">選取 **Reset Location** 函式。</span><span class="sxs-lookup"><span data-stu-id="4acf6-151">Select the **Reset Location** function.</span></span> <span data-ttu-id="4acf6-152">最後，將輸出執行連接點從 [On Button Pressed] 拖曳至 [Reset Location] 上的輸入執行連接點。</span><span class="sxs-lookup"><span data-stu-id="4acf6-152">Finally, drag the outgoing execution pin from **On Button Pressed** to the incoming execution pin on **Reset Location**.</span></span> <span data-ttu-id="4acf6-153">**編譯** 並 **儲存** ResetButton 藍圖，然後回到主視窗。</span><span class="sxs-lookup"><span data-stu-id="4acf6-153">**Compile** and **Save** the ResetButton Blueprint, then return to the Main window.</span></span> 

![從 On Button Pressed 呼叫 Reset Location 函式](images/unreal-uxt/5-callresetloc.PNG)

2.  <span data-ttu-id="4acf6-155">將 **ResetButton** 拖曳到檢視區，並將其位置設定為 **X = 50**、**Y = -25** 和 **Z = 10**。</span><span class="sxs-lookup"><span data-stu-id="4acf6-155">Drag **ResetButton** into the viewport and set its location to **X = 50**, **Y = -25**, and **Z = 10**.</span></span> <span data-ttu-id="4acf6-156">將其旋轉設定為 **Z = 180**。</span><span class="sxs-lookup"><span data-stu-id="4acf6-156">Set its rotation to **Z = 180**.</span></span> <span data-ttu-id="4acf6-157">在 [預設值] 底下，將 **WhiteKing** 變數的值設定為 **WhiteKing**。</span><span class="sxs-lookup"><span data-stu-id="4acf6-157">Under **Default**, set the value of the **WhiteKing** variable to **WhiteKing**.</span></span>

![設定變數](images/unreal-uxt/5-buttonlevel.PNG)

<span data-ttu-id="4acf6-159">執行應用程式、將棋子移至新位置，然後按下 HoloLens 2 樣式按鈕，以查看作用中的重設邏輯！</span><span class="sxs-lookup"><span data-stu-id="4acf6-159">Run the app, move the chess piece to a new location, and press your HoloLens 2-style button to see the reset logic in action!</span></span>

<span data-ttu-id="4acf6-160">您現在有一個混合實境應用程式，其中包含可與其互動的棋子和棋盤，還有一個功能完整的按鈕，可重設棋子的位置。</span><span class="sxs-lookup"><span data-stu-id="4acf6-160">You now have a mixed reality app with a chess piece and board that you can interact with, as well as a fully functioning button that will reset the piece’s location.</span></span> <span data-ttu-id="4acf6-161">到目前為止，您可在 [GitHub](https://github.com/microsoft/MixedReality-Unreal-Samples/tree/master/ChessApp) 存放庫中找到完成的應用程式。</span><span class="sxs-lookup"><span data-stu-id="4acf6-161">You can find the completed app up to this point in its [GitHub](https://github.com/microsoft/MixedReality-Unreal-Samples/tree/master/ChessApp) repo.</span></span> <span data-ttu-id="4acf6-162">您可以隨意進行超越本教學課程範圍的內容並設定其餘的棋子，讓整個棋盤都可在您按下按鈕時重設。</span><span class="sxs-lookup"><span data-stu-id="4acf6-162">Feel free to go beyond this tutorial and set up the remainder of the chess pieces so that the entire board is reset when the button is pressed.</span></span>

![檢視區中的最終場景](images/unreal-uxt/5-endscene.PNG)

<span data-ttu-id="4acf6-164">您已準備好繼續進行本教學課程的最後一節，在這裡您將了解如何正確封裝應用程式，並將其部署至裝置或模擬器。</span><span class="sxs-lookup"><span data-stu-id="4acf6-164">You're ready to move on to the final section of this tutorial where you'll learn how to correctly package and deploy the app to a device or emulator.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4acf6-165">此時，您應先使用建議的 **[Unreal 效能設定](../performance-recommendations-for-unreal.md)** 更新您的專案，再將應用程式部署至裝置或模擬器。</span><span class="sxs-lookup"><span data-stu-id="4acf6-165">At this point, you should update your project with the recommended **[Unreal performance settings](../performance-recommendations-for-unreal.md)** before deploying your application to a device or emulator.</span></span>

[<span data-ttu-id="4acf6-166">下一節：6.封裝並部署至裝置或模擬器</span><span class="sxs-lookup"><span data-stu-id="4acf6-166">Next Section: 6. Packaging & deploying to device or emulator</span></span>](unreal-uxt-ch6.md)
