---
title: 全像遠端版本歷程記錄
description: 隨時掌握 HoloLens 2 的全像「全像」遠端功能的版本歷程記錄。
author: florianbagarmicrosoft
ms.author: flbagar
ms.date: 12/01/2020
ms.topic: article
keywords: HoloLens、遠端、全像全像遠端、版本歷程記錄、混合現實耳機、windows mixed reality 耳機、虛擬實境耳機
ms.openlocfilehash: e1f80d0d2cbd02b78ed07e3ec60825ffe1059309
ms.sourcegitcommit: 3dad2adfdb5bdb8100d8d864f7845e34a3ef912d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/22/2021
ms.locfileid: "98699007"
---
# <a name="holographic-remoting-version-history"></a><span data-ttu-id="7fd3d-104">全像遠端版本歷程記錄</span><span class="sxs-lookup"><span data-stu-id="7fd3d-104">Holographic Remoting Version History</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7fd3d-105">本指南專為 HoloLens 2 上的全像攝影遠端所特有。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-105">This guidance is specific to Holographic Remoting on HoloLens 2.</span></span>

## <a name="version-241-january-22-2021"></a><span data-ttu-id="7fd3d-106">版本 2.4.1 (2021 年1月22日) <a name="v2.4.1"></a></span><span class="sxs-lookup"><span data-stu-id="7fd3d-106">Version 2.4.1 (January 22, 2021) <a name="v2.4.1"></a></span></span>

* <span data-ttu-id="7fd3d-107">修正 SpatialAnchorManager：： RequestStoreAsync 在連接時呼叫時無法可靠地運作的問題。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-107">Fixed issue with SpatialAnchorManager::RequestStoreAsync not working reliably when called while connecting.</span></span>
* <span data-ttu-id="7fd3d-108">修正 SpatialAnchorManager：： TrySave 未找出問題錨點時，無法正確儲存錨點的問題。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-108">Fixed issue with SpatialAnchorManager::TrySave not correctly saving an anchor if anchor in question is not locatable.</span></span>

## <a name="version-240-december-1-2020"></a><span data-ttu-id="7fd3d-109">版本 2.4.0 (2020 年12月1日) <a name="v2.4.0"></a></span><span class="sxs-lookup"><span data-stu-id="7fd3d-109">Version 2.4.0 (December 1, 2020) <a name="v2.4.0"></a></span></span>

* <span data-ttu-id="7fd3d-110">全像攝影遠端功能現在支援使用 [OPENXR API](../native/openxr.md)撰寫遠端應用程式。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-110">Holographic Remoting now support writing remote apps using the [OpenXR API](../native/openxr.md).</span></span> <span data-ttu-id="7fd3d-111">請參閱 [使用 OpenXR Api 撰寫全像的遠端執行遠端應用程式](holographic-remoting-create-remote-openxr.md) 以開始使用。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-111">Check out [Writing a Holographic Remoting remote app using OpenXR APIs](holographic-remoting-create-remote-openxr.md) to get started.</span></span>
* <span data-ttu-id="7fd3d-112">Bug 修正和穩定性改進。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-112">Bug fixes and stability improvements.</span></span>

## <a name="version-231-october-10-2020"></a><span data-ttu-id="7fd3d-113">2020年10月10日 (版本 2.3.1) <a name="v2.3.1"></a></span><span class="sxs-lookup"><span data-stu-id="7fd3d-113">Version 2.3.1 (October 10, 2020) <a name="v2.3.1"></a></span></span>

* <span data-ttu-id="7fd3d-114">修正了遠端引發預測的回歸，這會造成視覺抖動。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-114">Fixed regression with remote pose prediction, which caused visual jitter.</span></span>
* <span data-ttu-id="7fd3d-115">已實 PerceptionDeviceSetCreateFactoryOverride，可確保在全像全像的情況下隨附的 PerceptionDevice.dll 不會干擾 Windows 10 隨附的版本。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-115">Implemented PerceptionDeviceSetCreateFactoryOverride, which ensures that PerceptionDevice.dll shipped with Holographic Remoting doesn't interfere with the version shipped with Windows 10.</span></span>

## <a name="version-230-october-2-2020"></a><span data-ttu-id="7fd3d-116">版本 2.3.0 (2020 年10月2日) <a name="v2.3.0"></a></span><span class="sxs-lookup"><span data-stu-id="7fd3d-116">Version 2.3.0 (October 2, 2020) <a name="v2.3.0"></a></span></span>

* <span data-ttu-id="7fd3d-117">已修正當攝影遠端播放程式暫停時可能發生的損毀。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-117">Fixed crashes, which can happen when Holographic Remoting Player is suspended.</span></span>
* <span data-ttu-id="7fd3d-118">穩定性改進。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-118">Stability improvements.</span></span>

## <a name="version-223-august-28-2020"></a><span data-ttu-id="7fd3d-119">版本 2.2.3 (2020 年8月28日) <a name="v2.2.3"></a></span><span class="sxs-lookup"><span data-stu-id="7fd3d-119">Version 2.2.3 (August 28, 2020) <a name="v2.2.3"></a></span></span>

* <span data-ttu-id="7fd3d-120">Bug 修正和穩定性改進。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-120">Bug fixes and stability improvements.</span></span>

## <a name="version-222-july-10-2020"></a><span data-ttu-id="7fd3d-121">2020年7月10日版本 2.2.2 () <a name="v2.2.2"></a></span><span class="sxs-lookup"><span data-stu-id="7fd3d-121">Version 2.2.2 (July 10, 2020) <a name="v2.2.2"></a></span></span>

* <span data-ttu-id="7fd3d-122">修正了 [HolographicCamera. LeftViewportParameters](/uwp/api/windows.graphics.holographic.holographiccamera.leftviewportparameters) 和 HolographicCamera 的問題。當您從 Windows Mixed Reality 耳機進行串流處理時， [RightViewportParameters](/uwp/api/windows.graphics.holographic.holographiccamera.rightviewportparameters) 不會傳回任何隱藏的區域網格頂點。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-122">Fixed issue with [HolographicCamera.LeftViewportParameters](/uwp/api/windows.graphics.holographic.holographiccamera.leftviewportparameters) and [HolographicCamera.RightViewportParameters](/uwp/api/windows.graphics.holographic.holographiccamera.rightviewportparameters) not returning any hidden area mesh vertices when streaming from a Windows Mixed Reality headset.</span></span>
* <span data-ttu-id="7fd3d-123">修正損毀，這種情況可能會導致網路連線不佳。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-123">Fixed crash, which can happen with poor network connection.</span></span>

## <a name="version-221-july-6-2020"></a><span data-ttu-id="7fd3d-124">版本 2.2.1 (2020 年7月6日) <a name="v2.2.1"></a></span><span class="sxs-lookup"><span data-stu-id="7fd3d-124">Version 2.2.1 (July 6, 2020) <a name="v2.2.1"></a></span></span>

> [!IMPORTANT]
> <span data-ttu-id="7fd3d-125">[2.2.0](holographic-remoting-version-history.md#v2.2.0)版本的[Windows 應用程式認證套件](https://developer.microsoft.com/windows/downloads/app-certification-kit/)驗證將會失敗。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-125">[Windows App Certification Kit](https://developer.microsoft.com/windows/downloads/app-certification-kit/) validation with version [2.2.0](holographic-remoting-version-history.md#v2.2.0) will fail.</span></span> <span data-ttu-id="7fd3d-126">如果您是在版本 [2.2.0](holographic-remoting-version-history.md#v2.2.0) ，而且想要將您的應用程式提交至 Microsoft store p 租用期更新為至少版本2.2.1。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-126">In case you're on version [2.2.0](holographic-remoting-version-history.md#v2.2.0) and want to submit your application to the Microsoft store p lease updated to at least version 2.2.1.</span></span>
* <span data-ttu-id="7fd3d-127">修正 [Windows 應用程式認證套件](https://developer.microsoft.com/windows/downloads/app-certification-kit/) 合規性問題。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-127">Fixed [Windows App Certification Kit](https://developer.microsoft.com/windows/downloads/app-certification-kit/) compliance issues.</span></span>

## <a name="version-220-july-1-2020"></a><span data-ttu-id="7fd3d-128">版本 2.2.0 (2020 年7月1日) <a name="v2.2.0"></a></span><span class="sxs-lookup"><span data-stu-id="7fd3d-128">Version 2.2.0 (July 1, 2020) <a name="v2.2.0"></a></span></span>

* <span data-ttu-id="7fd3d-129">全像「全像遠端播放機」現在可以安裝在執行 [Windows Mixed Reality](../../discover/navigating-the-windows-mixed-reality-home.md)的電腦上，因此可以串流到沉浸式耳機。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-129">The Holographic Remoting player can now be installed on PCs running [Windows Mixed Reality](../../discover/navigating-the-windows-mixed-reality-home.md), making it possible to stream to immersive headsets.</span></span>
* <span data-ttu-id="7fd3d-130">您現在可以透過[SpatialInteractionSource](/uwp/api/windows.ui.input.spatial.spatialinteractionsource.controller#Windows_UI_Input_Spatial_SpatialInteractionSource_Controller)來取出[動態控制器](../../design/motion-controllers.md)和控制器專屬的資料，以支援移動控制站。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-130">[Motion controllers](../../design/motion-controllers.md) are now supported by Holographic Remoting and controller-specific data can be retrieved via [SpatialInteractionSource.Controller](/uwp/api/windows.ui.input.spatial.spatialinteractionsource.controller#Windows_UI_Input_Spatial_SpatialInteractionSource_Controller).</span></span>
* <span data-ttu-id="7fd3d-131">現在支援[SpatialStageFrameOfReference](/uwp/api/windows.perception.spatial.spatialstageframeofreference) ，而且可以透過[SpatialStageFrameOfReference](/uwp/api/windows.perception.spatial.spatialstageframeofreference.current)來取出目前的階段。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-131">[SpatialStageFrameOfReference](/uwp/api/windows.perception.spatial.spatialstageframeofreference) is now supported and the current stage can be retrieved via [SpatialStageFrameOfReference.Current](/uwp/api/windows.perception.spatial.spatialstageframeofreference.current).</span></span> <span data-ttu-id="7fd3d-132">此外，也可以透過 SpatialStageFrameOfReference 來要求新階段 [。 RequestNewStageAsync](/uwp/api/windows.perception.spatial.spatialstageframeofreference.requestnewstageasync)。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-132">Also, a new stage can be requested via [SpatialStageFrameOfReference.RequestNewStageAsync](/uwp/api/windows.perception.spatial.spatialstageframeofreference.requestnewstageasync).</span></span>
* <span data-ttu-id="7fd3d-133">在先前的版本中，會由全像遠端播放程式在玩家端處理預測。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-133">In previous versions, pose prediction was handled on the player side by the Holographic Remoting player.</span></span> <span data-ttu-id="7fd3d-134">從版本2.2.0 開始，全像攝影遠端處理有時間同步處理，而由遠端應用程式完全完成預測。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-134">Starting with version 2.2.0, Holographic Remoting has time synchronization, and prediction is fully done by the remote application.</span></span> <span data-ttu-id="7fd3d-135">在困難的網路情況下，使用者也應預期改良的全像影像穩定性。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-135">Users should also expect improved hologram stability under difficult network situations.</span></span>

## <a name="version-213-may-25-2020"></a><span data-ttu-id="7fd3d-136">版本 2.1.3 (2020 月25日) <a name="v2.1.3"></a></span><span class="sxs-lookup"><span data-stu-id="7fd3d-136">Version 2.1.3 (May 25, 2020) <a name="v2.1.3"></a></span></span>

* <span data-ttu-id="7fd3d-137">已變更 [HolographicSpace. CameraAdded](/uwp/api/windows.graphics.holographic.holographicspace.cameraadded) 事件的行為。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-137">Changed behavior of [HolographicSpace.CameraAdded](/uwp/api/windows.graphics.holographic.holographicspace.cameraadded) event.</span></span> <span data-ttu-id="7fd3d-138">在舊版中，在透過 [HolographicSpace](/uwp/api/windows.graphics.holographic.holographicspace.createnextframe)建立下一個畫面格時，**不** 保證新增的 [HolographicCamera](/uwp/api/windows.graphics.holographic.holographiccamera)也具有有效的 [HolographicCameraPose](/uwp/api/windows.graphics.holographic.holographiccamerapose) 。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-138">In previous versions, it was **not** guaranteed that an added [HolographicCamera](/uwp/api/windows.graphics.holographic.holographiccamera) also has a valid [HolographicCameraPose](/uwp/api/windows.graphics.holographic.holographiccamerapose) when creating the next frame via [HolographicSpace.CreateNextFrame](/uwp/api/windows.graphics.holographic.holographicspace.createnextframe).</span></span> <span data-ttu-id="7fd3d-139">從版本2.1.3 開始， [HolographicSpace. CameraAdded](/uwp/api/windows.graphics.holographic.holographicspace.cameraadded) 會與來自全像的遠端播放播放程式的姿勢資料進行同步處理。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-139">Starting with version 2.1.3, [HolographicSpace.CameraAdded](/uwp/api/windows.graphics.holographic.holographicspace.cameraadded) is synchronized with pose data coming from the Holographic Remoting Player.</span></span> <span data-ttu-id="7fd3d-140">使用者可能會預期當相機新增時，也會在下一個畫面上提供該相機的有效 [HolographicCameraPose](/uwp/api/windows.graphics.holographic.holographiccamerapose) 。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-140">Users can expect that when a camera is added there is also a valid [HolographicCameraPose](/uwp/api/windows.graphics.holographic.holographiccamerapose) available for that camera on the next frame.</span></span>
* <span data-ttu-id="7fd3d-141">已 **停用** DepthBufferStreamResolution，可用來透過 RemoteContext.ConfigureDepthVideoStream 停用深度緩衝區串流。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-141">Added **Disabled** to DepthBufferStreamResolution, which can be used to disable depth buffer streaming via RemoteContext.ConfigureDepthVideoStream.</span></span> <span data-ttu-id="7fd3d-142">請注意，如果使用 [HolographicCameraRenderingParameters，CommitDirect3D11DepthBuffer](/uwp/api/windows.graphics.holographic.holographiccamerarenderingparameters.commitdirect3d11depthbuffer) 將會失敗，並 *E_ILLEGAL_METHOD_CALL*。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-142">Note, if used [HolographicCameraRenderingParameters.CommitDirect3D11DepthBuffer](/uwp/api/windows.graphics.holographic.holographiccamerarenderingparameters.commitdirect3d11depthbuffer) will fail with *E_ILLEGAL_METHOD_CALL*.</span></span>
* <span data-ttu-id="7fd3d-143">全像是「全像遠端播放機」的啟動畫面已經過重新設計，現在不會封鎖使用者的觀點。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-143">The Holographic Remoting Player's startup screen has been redesigned and now doesn't block the users view.</span></span>
* <span data-ttu-id="7fd3d-144">穩定性改進與 bug 修正。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-144">Stability improvements and bug fixes.</span></span>

## <a name="version-212-april-5-2020"></a><span data-ttu-id="7fd3d-145">版本 2.1.2 (2020 年4月5日) <a name="v2.1.2"></a></span><span class="sxs-lookup"><span data-stu-id="7fd3d-145">Version 2.1.2 (April 5, 2020) <a name="v2.1.2"></a></span></span>

* <span data-ttu-id="7fd3d-146">使用小於2.1.0 的版本，修正最新全像全像遠端播放機和遠端應用程式之間的音訊回溯相容性問題。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-146">Fixed audio backwards compatibility issue between latest Holographic Remoting player and remote apps using version smaller than 2.1.0.</span></span>
* <span data-ttu-id="7fd3d-147">固定空間錨點問題，此問題未預期地關閉全像「全像」遠端播放機。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-147">Fixed spatial anchor issue, which unexpectedly closed the Holographic Remoting player.</span></span> <span data-ttu-id="7fd3d-148">此問題也會影響自訂玩家。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-148">This issue also affects custom players.</span></span>

## <a name="version-211-march-20-2020"></a><span data-ttu-id="7fd3d-149">版本 2.1.1 (2020 年3月20日) <a name="v2.1.1"></a></span><span class="sxs-lookup"><span data-stu-id="7fd3d-149">Version 2.1.1 (March 20, 2020) <a name="v2.1.1"></a></span></span>

* <span data-ttu-id="7fd3d-150">修正使用 AMD Gpu 時遠端應用程式的影片編碼問題。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-150">Fixed video encoding issue with remote apps when using AMD GPUs.</span></span>
* <span data-ttu-id="7fd3d-151">全像遠端播放機效能改進。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-151">Holographic Remoting Player performance improvements.</span></span>

## <a name="version-210-march-11-2020"></a><span data-ttu-id="7fd3d-152">版本 2.1.0 (2020 年3月11日) <a name="v2.1.0"></a></span><span class="sxs-lookup"><span data-stu-id="7fd3d-152">Version 2.1.0 (March 11, 2020) <a name="v2.1.0"></a></span></span>

* <span data-ttu-id="7fd3d-153">切換網路傳輸以使用 [RTP](https://en.wikipedia.org/wiki/Real-time_Transport_Protocol) via UDP。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-153">Switched network transport to use [RTP](https://en.wikipedia.org/wiki/Real-time_Transport_Protocol) via UDP.</span></span> <span data-ttu-id="7fd3d-154">安全連線現在會使用 [SRTP](https://en.wikipedia.org/wiki/Secure_Real-time_Transport_Protocol) 。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-154">Secure connections use [SRTP](https://en.wikipedia.org/wiki/Secure_Real-time_Transport_Protocol) now.</span></span> <span data-ttu-id="7fd3d-155">請注意，「全像」的「全像」、「全像」版本的 [遠端播放](holographic-remoting-player.md) 程式仍與所有先前</span><span class="sxs-lookup"><span data-stu-id="7fd3d-155">Note, the [Holographic Remoting Player](holographic-remoting-player.md) is still compatible with all previously release Holographic Remoting versions.</span></span> <span data-ttu-id="7fd3d-156">為了讓新的網路傳輸受益，全像是「全像遠端播放機」和「遠端應用程式」，必須使用版本2.1.0。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-156">To benefit from the new network transport both, the Holographic Remoting Player and the remote app in question, have to use version 2.1.0.</span></span>
* <span data-ttu-id="7fd3d-157">已新增對 [HolographicCameraRenderingParameters. CommitDirect3D11DepthBuffer](/uwp/api/windows.graphics.holographic.holographiccamerarenderingparameters.commitdirect3d11depthbuffer#Windows_Graphics_Holographic_HolographicCameraRenderingParameters_CommitDirect3D11DepthBuffer_Windows_Graphics_DirectX_Direct3D11_IDirect3DSurface_)的支援。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-157">Added support for [HolographicCameraRenderingParameters.CommitDirect3D11DepthBuffer](/uwp/api/windows.graphics.holographic.holographiccamerarenderingparameters.commitdirect3d11depthbuffer#Windows_Graphics_Holographic_HolographicCameraRenderingParameters_CommitDirect3D11DepthBuffer_Windows_Graphics_DirectX_Direct3D11_IDirect3DSurface_).</span></span> 

## <a name="version-2020-february-2-2020"></a><span data-ttu-id="7fd3d-158">2020年2月2日 (版本2.0.20 版) <a name="v2.0.20"></a></span><span class="sxs-lookup"><span data-stu-id="7fd3d-158">Version 2.0.20 (February 2, 2020) <a name="v2.0.20"></a></span></span>

* <span data-ttu-id="7fd3d-159">修正導致損毀的各種 bug。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-159">Fixed various bugs that lead to crashes.</span></span>

## <a name="version-2018-december-17-2019"></a><span data-ttu-id="7fd3d-160">版本2.0.18 版 (2019 年12月17日) <a name="v2.0.18"></a></span><span class="sxs-lookup"><span data-stu-id="7fd3d-160">Version 2.0.18 (December 17, 2019) <a name="v2.0.18"></a></span></span>

* <span data-ttu-id="7fd3d-161">新增對[HolographicViewConfiguration](/uwp/api/windows.graphics.holographic.holographicviewconfiguration)的支援</span><span class="sxs-lookup"><span data-stu-id="7fd3d-161">Added support for [HolographicViewConfiguration](/uwp/api/windows.graphics.holographic.holographicviewconfiguration)</span></span>
* <span data-ttu-id="7fd3d-162">修正導致損毀的各種 bug。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-162">Fixed various bugs that lead to crashes.</span></span>
* <span data-ttu-id="7fd3d-163">修正了 HolographicSpace 需要 CameraAdded 回呼才能接受，並在 HolographicFrame 中顯示為已新增攝影機的 bug。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-163">Fixed bug where a HolographicSpace.CameraAdded callback was required for a HolographicCamera to get accepted and show up as added camera in the HolographicFrame.</span></span>

## <a name="version-2016-november-11-2019"></a><span data-ttu-id="7fd3d-164">版本 2.0.16 (2019 年11月11日) <a name="2.0.16"></a></span><span class="sxs-lookup"><span data-stu-id="7fd3d-164">Version 2.0.16 (November 11, 2019) <a name="2.0.16"></a></span></span>

* <span data-ttu-id="7fd3d-165">修正 QR 代碼追蹤中的鎖死。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-165">Fixed deadlock in QR code tracking.</span></span>
* <span data-ttu-id="7fd3d-166">已修正 unhandeled 例外狀況，因為在主執行緒中有封鎖等候。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-166">Fixed unhandeled exception because of a blocking wait in main thread.</span></span>

## <a name="version-2014-october-26-2019"></a><span data-ttu-id="7fd3d-167">版本2.0.14 版 (2019 年10月26日) <a name="v2.0.14"></a></span><span class="sxs-lookup"><span data-stu-id="7fd3d-167">Version 2.0.14 (October 26, 2019) <a name="v2.0.14"></a></span></span>

* <span data-ttu-id="7fd3d-168">支援新的 PerceptionDevice Api (Windows 10 2019 年11月更新) 。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-168">Support for new PerceptionDevice APIs (Windows 10 November 2019 Update).</span></span>
* <span data-ttu-id="7fd3d-169">修正問題，這會導致 SpatialGestureRecognizer 觸發暫停手勢事件。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-169">Fixed issue, which prevent hold gesture events being triggered by SpatialGestureRecognizer.</span></span>
* <span data-ttu-id="7fd3d-170">修正使用 SpatialSurfaceObserver. SetBoundingVolume 時的執行緒問題。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-170">Fixed threading issue when using SpatialSurfaceObserver.SetBoundingVolume.</span></span>

## <a name="version-2012-october-18-2019"></a><span data-ttu-id="7fd3d-171">版本 2.0.12 (2019 年10月18日) <a name="v2.0.12"></a></span><span class="sxs-lookup"><span data-stu-id="7fd3d-171">Version 2.0.12 (October 18, 2019) <a name="v2.0.12"></a></span></span>

* <span data-ttu-id="7fd3d-172">修正使用 NavigationRail (X/Y/Z) 時，SpatialGestureRecognizer 中的損毀。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-172">Fixed crash in SpatialGestureRecognizer when using NavigationRail(X/Y/Z).</span></span>

## <a name="version-2010-october-10-2019"></a><span data-ttu-id="7fd3d-173">版本2.0.10 版 (2019 年10月10日) <a name="v2.0.10"></a></span><span class="sxs-lookup"><span data-stu-id="7fd3d-173">Version 2.0.10 (October 10, 2019) <a name="v2.0.10"></a></span></span>

* <span data-ttu-id="7fd3d-174">已修正使用 VR 控制器的觸發程式按鈕時的損毀。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-174">Fixed crash when using trigger button of VR controllers.</span></span> <span data-ttu-id="7fd3d-175">全像「全像」的遠端處理並不完全支援控制器，如果與 HoloLens 2 配對，則只有 [觸發程式] 按鈕和 [Windows] 按鈕</span><span class="sxs-lookup"><span data-stu-id="7fd3d-175">Holographic Remoting doesn't fully support controllers, only the trigger button and the Windows button are working if paired with HoloLens 2.</span></span>

## <a name="version-209-september-19-2019"></a><span data-ttu-id="7fd3d-176">版本 2.0.9 (2019 年9月19日) <a name="v2.0.9"></a></span><span class="sxs-lookup"><span data-stu-id="7fd3d-176">Version 2.0.9 (September 19, 2019) <a name="v2.0.9"></a></span></span>

* <span data-ttu-id="7fd3d-177">新增對[SpatialAnchorExporter](/uwp/api/windows.perception.spatial.spatialanchorexporter)的支援</span><span class="sxs-lookup"><span data-stu-id="7fd3d-177">Added support for [SpatialAnchorExporter](/uwp/api/windows.perception.spatial.spatialanchorexporter)</span></span>
* <span data-ttu-id="7fd3d-178">新增介面 ```IPlayerContext2``` (由 ```PlayerContext```) 提供下列成員來執行：</span><span class="sxs-lookup"><span data-stu-id="7fd3d-178">Added new interface ```IPlayerContext2``` (implemented by ```PlayerContext```) providing the following members:</span></span>
  - <span data-ttu-id="7fd3d-179">[BlitRemoteFrameTimeout](holographic-remoting-create-player.md#BlitRemoteFrameTimeout)  屬性。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-179">[BlitRemoteFrameTimeout](holographic-remoting-create-player.md#BlitRemoteFrameTimeout)  property.</span></span>
* <span data-ttu-id="7fd3d-180">已 ```Failed_RemoteFrameTooOld``` 將值新增至 ```BlitResult```</span><span class="sxs-lookup"><span data-stu-id="7fd3d-180">Added ```Failed_RemoteFrameTooOld``` value to ```BlitResult```</span></span>
* <span data-ttu-id="7fd3d-181">穩定性與可靠性的改進</span><span class="sxs-lookup"><span data-stu-id="7fd3d-181">Stability and reliability improvements</span></span>

## <a name="version-208-august-20-2019"></a><span data-ttu-id="7fd3d-182">版本 2.0.8 (2019 年8月20日) <a name="v2.0.8"></a></span><span class="sxs-lookup"><span data-stu-id="7fd3d-182">Version 2.0.8 (August 20, 2019) <a name="v2.0.8"></a></span></span>

* <span data-ttu-id="7fd3d-183">修正使用[IDXGISurface2](/windows/win32/api/dxgi1_2/nn-dxgi1_2-idxgisurface2)做為參數呼叫[HolographicCameraRenderingParameters](/uwp/api/windows.graphics.holographic.holographiccamerarenderingparameters.commitdirect3d11depthbuffer)時的損毀。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-183">Fixed crash when calling [HolographicCameraRenderingParameters.CommitDirect3D11DepthBuffer](/uwp/api/windows.graphics.holographic.holographiccamerarenderingparameters.commitdirect3d11depthbuffer) with a [IDXGISurface2](/windows/win32/api/dxgi1_2/nn-dxgi1_2-idxgisurface2) as parameter.</span></span>
* <span data-ttu-id="7fd3d-184">穩定性與可靠性的改進</span><span class="sxs-lookup"><span data-stu-id="7fd3d-184">Stability and reliability improvements</span></span>

## <a name="version-207-july-26-2019"></a><span data-ttu-id="7fd3d-185">版本 2.0.7 (2019 年7月26日) <a name="v2.0.7"></a></span><span class="sxs-lookup"><span data-stu-id="7fd3d-185">Version 2.0.7 (July 26, 2019) <a name="v2.0.7"></a></span></span>

* <span data-ttu-id="7fd3d-186">HoloLens 2 的全像全像的全像公開發行。</span><span class="sxs-lookup"><span data-stu-id="7fd3d-186">First public release of Holographic Remoting for HoloLens 2.</span></span>

## <a name="see-also"></a><span data-ttu-id="7fd3d-187">另請參閱</span><span class="sxs-lookup"><span data-stu-id="7fd3d-187">See Also</span></span>

* [<span data-ttu-id="7fd3d-188">使用 Windows Mixed Reality Api 撰寫全像遠端執行遠端應用程式</span><span class="sxs-lookup"><span data-stu-id="7fd3d-188">Writing a Holographic Remoting remote app using Windows Mixed Reality APIs</span></span>](holographic-remoting-create-remote-wmr.md)
* [<span data-ttu-id="7fd3d-189">使用 OpenXR Api 撰寫全像遠端執行遠端應用程式</span><span class="sxs-lookup"><span data-stu-id="7fd3d-189">Writing a Holographic Remoting remote app using OpenXR APIs</span></span>](holographic-remoting-create-remote-openxr.md)
* [<span data-ttu-id="7fd3d-190">撰寫自訂全像攝影遠端播放應用程式</span><span class="sxs-lookup"><span data-stu-id="7fd3d-190">Writing a custom Holographic Remoting player app</span></span>](holographic-remoting-create-player.md)
* [<span data-ttu-id="7fd3d-191">全像遠端的疑難排解和限制</span><span class="sxs-lookup"><span data-stu-id="7fd3d-191">Holographic Remoting troubleshooting and limitations</span></span>](holographic-remoting-troubleshooting.md)
* [<span data-ttu-id="7fd3d-192">全像攝影遠端軟體授權條款</span><span class="sxs-lookup"><span data-stu-id="7fd3d-192">Holographic Remoting software license terms</span></span>](/legal/mixed-reality/microsoft-holographic-remoting-software-license-terms)
* [<span data-ttu-id="7fd3d-193">Microsoft 隱私權聲明</span><span class="sxs-lookup"><span data-stu-id="7fd3d-193">Microsoft Privacy Statement</span></span>](https://go.microsoft.com/fwlink/?LinkId=521839)