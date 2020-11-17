---
title: 針對 Windows Mixed Reality 設定新的 Unity 專案
description: 針對 Windows Mixed Reality 設定 Unity 專案的指示
author: thetuvix
ms.author: alexturn
ms.date: 07/29/2020
ms.topic: article
keywords: Unity，混合的現實，開發，使用者入門，新專案，Windows Mixed Reality，UWP，XR，效能
ms.openlocfilehash: cd7e6c5681c717c37368393a605998a2ab8e4175
ms.sourcegitcommit: dd13a32a5bb90bd53eeeea8214cd5384d7b9ef76
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94677667"
---
# <a name="configure-a-new-unity-project-for-windows-mixed-reality"></a><span data-ttu-id="6dc55-104">針對 Windows Mixed Reality 設定新的 Unity 專案</span><span class="sxs-lookup"><span data-stu-id="6dc55-104">Configure a New Unity project for Windows Mixed Reality</span></span> 

## <a name="overview"></a><span data-ttu-id="6dc55-105">概觀</span><span class="sxs-lookup"><span data-stu-id="6dc55-105">Overview</span></span>

<span data-ttu-id="6dc55-106">Windows Mixed Reality (WMR) 是在 Windows 10 作業系統中引進的 Microsoft 平臺。</span><span class="sxs-lookup"><span data-stu-id="6dc55-106">Windows Mixed Reality (WMR) is a Microsoft platform introduced as part of the Windows 10 operating system.</span></span> <span data-ttu-id="6dc55-107">WMR 平臺可讓您建立應用程式，以在全像全像 VR 的顯示裝置上呈現數位內容。</span><span class="sxs-lookup"><span data-stu-id="6dc55-107">The WMR platform lets you build applications that render digital content on holographic and VR display devices.</span></span>

<span data-ttu-id="6dc55-108">設定 WMR 時，您可以採用兩個路徑。</span><span class="sxs-lookup"><span data-stu-id="6dc55-108">When setting up for WMR, there are two paths you can take.</span></span> <span data-ttu-id="6dc55-109">第一個選項是安裝 [混合現實工具組 (MRTK) ](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/Installation.html)，這會自動設定 WMR 環境。</span><span class="sxs-lookup"><span data-stu-id="6dc55-109">Your first option is to install the [Mixed Reality Toolkit (MRTK)](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/Installation.html), which will automatically set up the WMR environment.</span></span> <span data-ttu-id="6dc55-110">第二個選項是手動變更一些 Unity 設定，以使用 WMR 進行滾動。</span><span class="sxs-lookup"><span data-stu-id="6dc55-110">The second option is to manually change a few Unity settings to get rolling with WMR.</span></span> 

> [!NOTE]
> <span data-ttu-id="6dc55-111">您稍後可以隨時匯入 MRTK，如此一來，就不會再進行手動路由。</span><span class="sxs-lookup"><span data-stu-id="6dc55-111">You can always import MRTK later on, so there's no penalty for going the manual route first.</span></span>

<span data-ttu-id="6dc55-112">如果您選擇 WMR 手動設定，您需要變更的設定會分成兩種類別：個別專案和每一場景。</span><span class="sxs-lookup"><span data-stu-id="6dc55-112">If you choose the WMR manual setup, the settings you need to change are broken-down into two categories: per-project and per-scene.</span></span>

## <a name="per-project-settings"></a><span data-ttu-id="6dc55-113">每個專案的設定</span><span class="sxs-lookup"><span data-stu-id="6dc55-113">Per-project settings</span></span>

<span data-ttu-id="6dc55-114">您需要變更 WMR 的第一個設定是專案平臺：</span><span class="sxs-lookup"><span data-stu-id="6dc55-114">The first setting you need to change for WMR is the project platform:</span></span> 
1. <span data-ttu-id="6dc55-115">選取 [檔案 **> 組建設定 ...** ]</span><span class="sxs-lookup"><span data-stu-id="6dc55-115">Select **File > Build Settings...**</span></span>
2. <span data-ttu-id="6dc55-116">在 [平臺] 清單中選取 **通用 Windows 平臺**，然後按一下 [**切換平臺**]</span><span class="sxs-lookup"><span data-stu-id="6dc55-116">Select **Universal Windows Platform** in the Platform list and click **Switch Platform**</span></span>
3. <span data-ttu-id="6dc55-117">將 **SDK** 設定為 **通用 10**</span><span class="sxs-lookup"><span data-stu-id="6dc55-117">Set **SDK** to **Universal 10**</span></span>
4. <span data-ttu-id="6dc55-118">將 **目標裝置** 設定為 **任何裝置** 以支援沉浸式耳機或切換至 **HoloLens**</span><span class="sxs-lookup"><span data-stu-id="6dc55-118">Set **Target device** to **Any Device** to support immersive headsets or switch to **HoloLens**</span></span>
5. <span data-ttu-id="6dc55-119">將 **組建類型** 設定為 **D3D**</span><span class="sxs-lookup"><span data-stu-id="6dc55-119">Set **Build Type** to **D3D**</span></span>
6. <span data-ttu-id="6dc55-120">將 **UWP SDK** 設定為 **最新安裝**</span><span class="sxs-lookup"><span data-stu-id="6dc55-120">Set **UWP SDK** to **Latest installed**</span></span>

<img src="images/unity-uwp-settings.png" width="550px" alt="Unity XR Settings">
<span data-ttu-id="6dc55-121">*Unity XR 設定*</span><span class="sxs-lookup"><span data-stu-id="6dc55-121">*Unity XR settings*</span></span>

<span data-ttu-id="6dc55-122">當平臺設定正確之後，您需要讓 Unity 知道您的應用程式應該在匯出時建立一個 [沉浸式視圖](../../design/app-views.md) ，而不是2d 視圖：</span><span class="sxs-lookup"><span data-stu-id="6dc55-122">After the platform is configured correctly, you need to let Unity know that your app should create an [immersive view](../../design/app-views.md) instead of a 2D view when exported:</span></span>
1. <span data-ttu-id="6dc55-123">從 [**組建設定 ...** ] 視窗中，開啟 [**播放玩家設定**]。</span><span class="sxs-lookup"><span data-stu-id="6dc55-123">From the **Build Settings...** window, open **Player Settings...**</span></span>
2. <span data-ttu-id="6dc55-124">選取 [ **通用 Windows 平臺** ] 索引標籤的設定，並展開 [ **XR 設定** ] 群組</span><span class="sxs-lookup"><span data-stu-id="6dc55-124">Select the **Settings for Universal Windows Platform** tab and expand the **XR Settings** group</span></span>
3. <span data-ttu-id="6dc55-125">在 [ **XR 設定** ] 區段中，核取 [ **虛擬實境支援** ] 核取方塊，以新增 **虛擬實境裝置** 清單。</span><span class="sxs-lookup"><span data-stu-id="6dc55-125">In the **XR Settings** section, check the **Virtual Reality Supported** checkbox to add the **Virtual Reality Devices** list.</span></span>
4. <span data-ttu-id="6dc55-126">在 [ **XR 設定** ] 群組中，確認 **[Windows Mixed Reality]** 列為受支援的裝置。</span><span class="sxs-lookup"><span data-stu-id="6dc55-126">In the **XR Settings** group, confirm that **"Windows Mixed Reality"** is listed as a supported device.</span></span> <span data-ttu-id="6dc55-127"> (此選項可能會顯示為舊版 Unity 中的 **Windows 全息** 版) </span><span class="sxs-lookup"><span data-stu-id="6dc55-127">(this option may appear as **Windows Holographic** in older versions of Unity)</span></span>

<span data-ttu-id="6dc55-128">![Unity UWP 設定](images/xrsettings.png)</span><span class="sxs-lookup"><span data-stu-id="6dc55-128">![Unity UWP settings](images/xrsettings.png)</span></span><br>
<span data-ttu-id="6dc55-129">*Unity XR 設定*</span><span class="sxs-lookup"><span data-stu-id="6dc55-129">*Unity XR settings*</span></span>

### <a name="updating-the-manifest"></a><span data-ttu-id="6dc55-130">更新資訊清單</span><span class="sxs-lookup"><span data-stu-id="6dc55-130">Updating the manifest</span></span>

<span data-ttu-id="6dc55-131">您的應用程式現在可以處理全像呈現和空間輸入。</span><span class="sxs-lookup"><span data-stu-id="6dc55-131">Your app can now handle holographic rendering and spatial input.</span></span> <span data-ttu-id="6dc55-132">不過，您的應用程式需要在其資訊清單中宣告適當的功能，以利用特定功能。</span><span class="sxs-lookup"><span data-stu-id="6dc55-132">However, your app needs to declare the appropriate capabilities in its manifest to take advantage of certain functionality.</span></span> <span data-ttu-id="6dc55-133">您可以前往 [ **播放程式設定] > 設定通用 Windows 平臺 > 發佈設定 > 功能**，找出您的專案功能。</span><span class="sxs-lookup"><span data-stu-id="6dc55-133">You can find your projects capabilities by going to **Player Settings > Settings for Universal Windows Platform > Publishing Settings > Capabilities**.</span></span> 

<span data-ttu-id="6dc55-134">建議您讓 Unity 中的資訊清單宣告將它們包含在您匯出的所有未來專案中。</span><span class="sxs-lookup"><span data-stu-id="6dc55-134">It's recommended that you make the manifest declarations in Unity to include them in all future projects that you export.</span></span> <span data-ttu-id="6dc55-135">針對混合現實啟用常用 Unity Api 的適用功能如下：</span><span class="sxs-lookup"><span data-stu-id="6dc55-135">The applicable capabilities for enabling commonly used Unity APIs for Mixed Reality are:</span></span>

|  <span data-ttu-id="6dc55-136">功能</span><span class="sxs-lookup"><span data-stu-id="6dc55-136">Capability</span></span>  |  <span data-ttu-id="6dc55-137">需要功能的 Api</span><span class="sxs-lookup"><span data-stu-id="6dc55-137">APIs requiring capability</span></span> | 
|----------|----------|
|  <span data-ttu-id="6dc55-138">SpatialPerception</span><span class="sxs-lookup"><span data-stu-id="6dc55-138">SpatialPerception</span></span>  |  <span data-ttu-id="6dc55-139">SurfaceObserver (在 HoloLens 上存取 [空間對應](../../design/spatial-mapping.md)網格) &mdash; *不需要一般空間追蹤耳機的功能*</span><span class="sxs-lookup"><span data-stu-id="6dc55-139">SurfaceObserver (access to [spatial mapping](../../design/spatial-mapping.md) meshes on HoloLens)&mdash;*No capability needed for general spatial tracking of the headset*</span></span> | 
|  <span data-ttu-id="6dc55-140">攝像頭</span><span class="sxs-lookup"><span data-stu-id="6dc55-140">WebCam</span></span>  |  <span data-ttu-id="6dc55-141">PhotoCapture 和 VideoCapture</span><span class="sxs-lookup"><span data-stu-id="6dc55-141">PhotoCapture and VideoCapture</span></span> | 
|  <span data-ttu-id="6dc55-142">PicturesLibrary/VideosLibrary</span><span class="sxs-lookup"><span data-stu-id="6dc55-142">PicturesLibrary / VideosLibrary</span></span>  |  <span data-ttu-id="6dc55-143">PhotoCapture 或 VideoCapture，分別 (儲存已捕捉的內容時) </span><span class="sxs-lookup"><span data-stu-id="6dc55-143">PhotoCapture or VideoCapture, respectively (when storing the captured content)</span></span> | 
|  <span data-ttu-id="6dc55-144">麥克風</span><span class="sxs-lookup"><span data-stu-id="6dc55-144">Microphone</span></span>  |  <span data-ttu-id="6dc55-145">VideoCapture 捕獲音訊) 、DictationRecognizer、GrammarRecognizer 和 KeywordRecognizer 時的 (</span><span class="sxs-lookup"><span data-stu-id="6dc55-145">VideoCapture (when capturing audio), DictationRecognizer, GrammarRecognizer, and KeywordRecognizer</span></span> | 
|  <span data-ttu-id="6dc55-146">InternetClient</span><span class="sxs-lookup"><span data-stu-id="6dc55-146">InternetClient</span></span>  |  <span data-ttu-id="6dc55-147">DictationRecognizer (並使用 Unity Profiler) </span><span class="sxs-lookup"><span data-stu-id="6dc55-147">DictationRecognizer (and to use the Unity Profiler)</span></span> | 

### <a name="quality-settings"></a><span data-ttu-id="6dc55-148">品質設定</span><span class="sxs-lookup"><span data-stu-id="6dc55-148">Quality settings</span></span>

<span data-ttu-id="6dc55-149">HoloLens 有行動類別 GPU。</span><span class="sxs-lookup"><span data-stu-id="6dc55-149">HoloLens has a mobile-class GPU.</span></span> <span data-ttu-id="6dc55-150">如果您的應用程式是以 HoloLens 為目標，您會想要調整應用程式中的品質設定以獲得最快的效能，以確保它會維持完整的畫面播放速率：</span><span class="sxs-lookup"><span data-stu-id="6dc55-150">If your app is targeting HoloLens, you'll want the quality settings in your app tuned for fastest performance to ensure it maintains full frame-rate:</span></span>
1. <span data-ttu-id="6dc55-151">選取 [ **編輯] > 專案設定 > 品質**</span><span class="sxs-lookup"><span data-stu-id="6dc55-151">Select **Edit > Project Settings > Quality**</span></span>
2. <span data-ttu-id="6dc55-152">選取 [ **Windows Store** ] 標誌底下的 **下拉式清單**，然後選取 [**非常低**]。</span><span class="sxs-lookup"><span data-stu-id="6dc55-152">Select the **dropdown** under the **Windows Store** logo and select **Very Low**.</span></span> <span data-ttu-id="6dc55-153">您會知道當 Windows Store 資料行中的方塊和 **非常低** 的資料列中的方塊為綠色時，會正確套用設定。</span><span class="sxs-lookup"><span data-stu-id="6dc55-153">You'll know the setting is applied correctly when the box in the Windows Store column and **Very Low** row is green.</span></span>

<span data-ttu-id="6dc55-154">![Unity 品質設定](images/getting-started-unity-quality-settings.jpg)</span><span class="sxs-lookup"><span data-stu-id="6dc55-154">![Unity quality settings](images/getting-started-unity-quality-settings.jpg)</span></span><br>
<span data-ttu-id="6dc55-155">*Unity 品質設定*</span><span class="sxs-lookup"><span data-stu-id="6dc55-155">*Unity quality settings*</span></span>

## <a name="per-scene-settings"></a><span data-ttu-id="6dc55-156">每場景設定</span><span class="sxs-lookup"><span data-stu-id="6dc55-156">Per-scene settings</span></span>

### <a name="unity-camera-settings"></a><span data-ttu-id="6dc55-157">Unity 攝影機設定</span><span class="sxs-lookup"><span data-stu-id="6dc55-157">Unity camera settings</span></span>

<span data-ttu-id="6dc55-158">若已核取 [ **虛擬事實** ]，則 [Unity 攝影機](camera-in-unity.md) 元件會處理 [標頭追蹤和 stereoscopic](../platform-capabilities-and-apis/rendering.md)轉譯。</span><span class="sxs-lookup"><span data-stu-id="6dc55-158">With **Virtual Reality Supported** checked, the [Unity Camera](camera-in-unity.md) component handles [head tracking and stereoscopic rendering](../platform-capabilities-and-apis/rendering.md).</span></span> <span data-ttu-id="6dc55-159">這表示您不需要使用自訂相機來取代主要攝影機物件。</span><span class="sxs-lookup"><span data-stu-id="6dc55-159">That means there's no need for you to replace the Main Camera object with a custom camera.</span></span>

<span data-ttu-id="6dc55-160">如果您的應用程式是以 HoloLens 為目標，則您必須變更一些設定，以針對裝置的透明顯示進行優化。</span><span class="sxs-lookup"><span data-stu-id="6dc55-160">If your app is targeting HoloLens specifically, you need to change a few settings to optimize for the device's transparent displays.</span></span> <span data-ttu-id="6dc55-161">這些設定可讓您的全像全球內容顯示到實體世界：</span><span class="sxs-lookup"><span data-stu-id="6dc55-161">These settings allow your holographic content to show through to the physical world:</span></span>
1. <span data-ttu-id="6dc55-162">**在階層中，選取\*\*\*\*主要攝影機**</span><span class="sxs-lookup"><span data-stu-id="6dc55-162">In the **Hierarchy**, select the **Main Camera**</span></span>
2. <span data-ttu-id="6dc55-163">在 [偵測 **器** ] 面板中，將轉換 **位置** 設定為 **0、0、0** ，讓使用者的標頭位置開始于 Unity world 來源。</span><span class="sxs-lookup"><span data-stu-id="6dc55-163">In the **Inspector** panel, set the transform **position** to **0, 0, 0** so the location of the user's head starts at the Unity world origin.</span></span>
3. <span data-ttu-id="6dc55-164">將 **清除旗標** 變更為 **純色**。</span><span class="sxs-lookup"><span data-stu-id="6dc55-164">Change **Clear Flags** to **Solid Color**.</span></span>
4. <span data-ttu-id="6dc55-165">將 **背景** 色彩變更為 **RGBA 0、0、0、0**。</span><span class="sxs-lookup"><span data-stu-id="6dc55-165">Change the **Background** color to **RGBA 0,0,0,0**.</span></span> <span data-ttu-id="6dc55-166">在 HoloLens 中，黑色呈現為透明。</span><span class="sxs-lookup"><span data-stu-id="6dc55-166">Black renders as transparent in HoloLens.</span></span>
5. <span data-ttu-id="6dc55-167">變更 **裁剪平面-接近** [HoloLens 建議](camera-in-unity.md#clip-planes) 的 0.85 (計量) 。</span><span class="sxs-lookup"><span data-stu-id="6dc55-167">Change **Clipping Planes - Near** to the [HoloLens recommended](camera-in-unity.md#clip-planes) 0.85 (meters).</span></span>

<span data-ttu-id="6dc55-168">![Unity 攝影機設定](images/Unitycamerasettings.png)</span><span class="sxs-lookup"><span data-stu-id="6dc55-168">![Unity camera settings](images/Unitycamerasettings.png)</span></span><br>
<span data-ttu-id="6dc55-169">*Unity 攝影機設定*</span><span class="sxs-lookup"><span data-stu-id="6dc55-169">*Unity camera settings*</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6dc55-170">如果您刪除並建立新的相機，請確定您的新相機已標記為 **MainCamera**。</span><span class="sxs-lookup"><span data-stu-id="6dc55-170">If you delete and create a new camera, make sure your new camera is tagged as **MainCamera**.</span></span>

## <a name="see-also"></a><span data-ttu-id="6dc55-171">另請參閱</span><span class="sxs-lookup"><span data-stu-id="6dc55-171">See also</span></span>
* [<span data-ttu-id="6dc55-172">MRTK - 安裝指南 (GitHub)</span><span class="sxs-lookup"><span data-stu-id="6dc55-172">MRTK - Installation guide (GitHub)</span></span>](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/Installation.html)
* [<span data-ttu-id="6dc55-173">MRTK - 文件首頁 (GitHub)</span><span class="sxs-lookup"><span data-stu-id="6dc55-173">MRTK - Documentation home (GitHub)</span></span>](https://microsoft.github.io/MixedRealityToolkit-Unity/README.html)
* [<span data-ttu-id="6dc55-174">安裝工具</span><span class="sxs-lookup"><span data-stu-id="6dc55-174">Install the tools</span></span>](../install-the-tools.md)
* [<span data-ttu-id="6dc55-175">Unity 開發總覽</span><span class="sxs-lookup"><span data-stu-id="6dc55-175">Unity Development Overview</span></span>](unity-development-overview.md)
