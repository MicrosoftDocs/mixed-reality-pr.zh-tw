---
title: HoloLens 研究模式
description: 在 HoloLens 上使用 Research 模式時，應用程式可以存取主要裝置感應器串流 (深度、環境追蹤和 IR 反射率) 。
author: hferrone
ms.author: v-hferrone
ms.date: 07/31/2020
ms.topic: article
keywords: 研究模式、cv、rs4、電腦視覺、研究、HoloLens、HoloLens 2
ms.openlocfilehash: 6c40ac814a5dacfdbb942aec8200f46157bea161
ms.sourcegitcommit: c41372e0c6ca265f599bff309390982642d628b8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/15/2020
ms.locfileid: "97530084"
---
# <a name="hololens-research-mode"></a><span data-ttu-id="e1cda-104">HoloLens 研究模式</span><span class="sxs-lookup"><span data-stu-id="e1cda-104">HoloLens Research Mode</span></span>

<span data-ttu-id="e1cda-105">研究模式是在 HoloLens (第一代) 裝置上引進，以提供金鑰感應器的存取權，特別是研究不打算進行部署的應用程式。</span><span class="sxs-lookup"><span data-stu-id="e1cda-105">Research Mode was introduced on HoloLens (1st Gen) devices to give access to key sensors, specifically for research applications that aren't intended for deployment.</span></span>  <span data-ttu-id="e1cda-106">HoloLens 2 的研究模式會保留 HoloLens 1 的功能，但會新增下列資料流程的存取權：</span><span class="sxs-lookup"><span data-stu-id="e1cda-106">Research Mode for HoloLens 2 keeps the capabilities of HoloLens 1 but adds access to the following streams:</span></span>

* <span data-ttu-id="e1cda-107">**可見的光線環境追蹤攝影機** -系統用來追蹤和地圖大樓的灰色縮放相機。</span><span class="sxs-lookup"><span data-stu-id="e1cda-107">**Visible Light Environment Tracking Cameras** - Gray-scale cameras used by the system for head tracking and map building.</span></span>
* <span data-ttu-id="e1cda-108">**深度相機** –以兩種模式運作：</span><span class="sxs-lookup"><span data-stu-id="e1cda-108">**Depth Camera** – Operates in two modes:</span></span>  
    + <span data-ttu-id="e1cda-109">AHAT、高頻率 (45 FPS) 用於手動追蹤的近深度感知。</span><span class="sxs-lookup"><span data-stu-id="e1cda-109">AHAT, high-frequency (45 FPS) near-depth sensing used for hand tracking.</span></span> <span data-ttu-id="e1cda-110">與第一個版本的簡短擲回模式不同的是，AHAT 提供的虛擬深度與1計量以外的階段換行。</span><span class="sxs-lookup"><span data-stu-id="e1cda-110">Differently from the first version short-throw mode, AHAT gives pseudo-depth with phase wrap beyond 1 meter.</span></span> 
    + <span data-ttu-id="e1cda-111">長時間擲回、低頻率 (1-5 FPS) [空間對應](../../design/spatial-mapping.md)所使用的深度感應</span><span class="sxs-lookup"><span data-stu-id="e1cda-111">Long-throw, low-frequency (1-5 FPS) far-depth sensing used by [Spatial Mapping](../../design/spatial-mapping.md)</span></span>

* <span data-ttu-id="e1cda-112">**兩個版本的 IR 反射率串流** -由 HoloLens 用來計算深度。</span><span class="sxs-lookup"><span data-stu-id="e1cda-112">**Two versions of the IR-reflectivity stream** - Used by the HoloLens to compute depth.</span></span> <span data-ttu-id="e1cda-113">這些影像會由紅外線發亮，並不受環境可見的光線影響。</span><span class="sxs-lookup"><span data-stu-id="e1cda-113">These images are illuminated by infrared and unaffected by ambient visible light.</span></span>

<span data-ttu-id="e1cda-114">如果您是使用 HoloLens 2，您也可以存取下列其他輸入：</span><span class="sxs-lookup"><span data-stu-id="e1cda-114">If you're using a HoloLens 2, you also have access to the additional inputs below:</span></span>

* <span data-ttu-id="e1cda-115">**加速** 計–由系統用來判斷沿著 X、Y 和 Z 軸和重力的線性加速。</span><span class="sxs-lookup"><span data-stu-id="e1cda-115">**Accelerometer** – Used by the system to determine linear acceleration along the X, Y, and Z axes and gravity.</span></span>
* <span data-ttu-id="e1cda-116">**回轉** –系統用來判斷旋轉。</span><span class="sxs-lookup"><span data-stu-id="e1cda-116">**Gyro** – Used by the system to determine rotations.</span></span>
* <span data-ttu-id="e1cda-117">**磁力計** –由系統用來估計絕對方向。</span><span class="sxs-lookup"><span data-stu-id="e1cda-117">**Magnetometer** – Used by the system to estimate absolute orientation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e1cda-118">研究模式目前為公開預覽狀態。</span><span class="sxs-lookup"><span data-stu-id="e1cda-118">Research Mode is currently in Public Preview.</span></span> 

<span data-ttu-id="e1cda-119">![研究模式應用程式螢幕擷取畫面](images/sensor-stream-viewer.jpg)</span><span class="sxs-lookup"><span data-stu-id="e1cda-119">![Research Mode app screenshot](images/sensor-stream-viewer.jpg)</span></span><br>
<span data-ttu-id="e1cda-120">*測試應用程式的混合現實捕捉，可顯示研究模式中可用的八個感應器串流*</span><span class="sxs-lookup"><span data-stu-id="e1cda-120">*A mixed reality capture of a test application that displays the eight sensor streams available in Research Mode*</span></span>

## <a name="usage"></a><span data-ttu-id="e1cda-121">使用量</span><span class="sxs-lookup"><span data-stu-id="e1cda-121">Usage</span></span>

<span data-ttu-id="e1cda-122">研究模式是專為學術和產業研究人員所設計，可在電腦視覺和機器人的領域中探索新構想。</span><span class="sxs-lookup"><span data-stu-id="e1cda-122">Research Mode is designed for academic and industrial researchers exploring new ideas in the fields of Computer Vision and Robotics.</span></span>  <span data-ttu-id="e1cda-123">它不適用於企業環境中部署的應用程式，或可透過 Microsoft Store 或其他散發通道使用的應用程式。</span><span class="sxs-lookup"><span data-stu-id="e1cda-123">It's not intended for applications deployed in enterprise environments or available through the Microsoft Store or other distribution channels.</span></span>

<span data-ttu-id="e1cda-124">此外，Microsoft 不保證未來的硬體或作業系統更新將會支援研究模式或同等功能。</span><span class="sxs-lookup"><span data-stu-id="e1cda-124">Additionally, Microsoft doesn't provide assurances that Research Mode or equivalent functionality is going to be supported in future hardware or OS updates.</span></span> <span data-ttu-id="e1cda-125">不過，請勿讓您使用它來開發和測試新的想法！</span><span class="sxs-lookup"><span data-stu-id="e1cda-125">However, don't let that stop you from using it to develop and test new ideas!</span></span>

## <a name="security-and-performance"></a><span data-ttu-id="e1cda-126">安全性和效能</span><span class="sxs-lookup"><span data-stu-id="e1cda-126">Security and performance</span></span>

<span data-ttu-id="e1cda-127">即使使用研究模式功能的應用程式不在執行中，啟用 Research 模式仍會使用比使用 HoloLens 2 更高的電池電力。</span><span class="sxs-lookup"><span data-stu-id="e1cda-127">Enabling Research Mode uses more battery power than using the HoloLens 2 under normal conditions, even if the application using the Research Mode features isn't running.</span></span>  <span data-ttu-id="e1cda-128">啟用此模式也會降低裝置的整體安全性，因為應用程式可能會誤用感應器資料。</span><span class="sxs-lookup"><span data-stu-id="e1cda-128">Enabling this mode can also lower the overall security of your device because applications may misuse sensor data.</span></span>  <span data-ttu-id="e1cda-129">您可以在 [HoloLens 安全性常見問題](https://docs.microsoft.com/hololens/hololens-faq-security)中找到裝置安全性的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="e1cda-129">You can find more information on device security in the [HoloLens security FAQ](https://docs.microsoft.com/hololens/hololens-faq-security).</span></span>  

## <a name="device-support"></a><span data-ttu-id="e1cda-130">裝置支援</span><span class="sxs-lookup"><span data-stu-id="e1cda-130">Device support</span></span>
<table><span data-ttu-id="e1cda-131">
    <colgroup>
    <col width="33%" />
    <col width="33%" />
    <col width="33%" /> </colgroup>
    </span><span class="sxs-lookup"><span data-stu-id="e1cda-131">
    <colgroup>
    <col width="33%" />
    <col width="33%" />
    <col width="33%" /> </colgroup>
    </span></span><tr>
        <td><span data-ttu-id="e1cda-132"><strong>功能</strong></span><span class="sxs-lookup"><span data-stu-id="e1cda-132"><strong>Feature</strong></span></span></td>
        <td><span data-ttu-id="e1cda-133"><a href="https://docs.microsoft.com/hololens/hololens1-hardware"><strong>HoloLens 第一代</strong></a></span><span class="sxs-lookup"><span data-stu-id="e1cda-133"><a href="https://docs.microsoft.com/hololens/hololens1-hardware"><strong>HoloLens first Gen</strong></a></span></span></td>
        <td><span data-ttu-id="e1cda-134"><a href="https://docs.microsoft.com/hololens/hololens2-hardware"><strong>HoloLens 2</strong></a></span><span class="sxs-lookup"><span data-stu-id="e1cda-134"><a href="https://docs.microsoft.com/hololens/hololens2-hardware"><strong>HoloLens 2</strong></a></span></span></td>
    </tr>
     <tr>
        <td><span data-ttu-id="e1cda-135">Head 追蹤攝影機</span><span class="sxs-lookup"><span data-stu-id="e1cda-135">Head Tracking Cameras</span></span></td>
        <td><span data-ttu-id="e1cda-136">✔️</span><span class="sxs-lookup"><span data-stu-id="e1cda-136">✔️</span></span></td>
        <td><span data-ttu-id="e1cda-137">✔️</span><span class="sxs-lookup"><span data-stu-id="e1cda-137">✔️</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="e1cda-138">IR 攝影機的深度 &</span><span class="sxs-lookup"><span data-stu-id="e1cda-138">Depth & IR Camera</span></span></td>
        <td><span data-ttu-id="e1cda-139">✔️</span><span class="sxs-lookup"><span data-stu-id="e1cda-139">✔️</span></span></td>
        <td><span data-ttu-id="e1cda-140">✔️</span><span class="sxs-lookup"><span data-stu-id="e1cda-140">✔️</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="e1cda-141">加速計</span><span class="sxs-lookup"><span data-stu-id="e1cda-141">Accelerometer</span></span></td>
        <td>❌</td>
        <td><span data-ttu-id="e1cda-142">✔️</span><span class="sxs-lookup"><span data-stu-id="e1cda-142">✔️</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="e1cda-143">迴轉儀</span><span class="sxs-lookup"><span data-stu-id="e1cda-143">Gyroscope</span></span></td>
        <td>❌</td>
        <td><span data-ttu-id="e1cda-144">✔️</span><span class="sxs-lookup"><span data-stu-id="e1cda-144">✔️</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="e1cda-145">磁力計</span><span class="sxs-lookup"><span data-stu-id="e1cda-145">Magnetometer</span></span></td>
        <td>❌</td>
        <td><span data-ttu-id="e1cda-146">✔️</span><span class="sxs-lookup"><span data-stu-id="e1cda-146">✔️</span></span></td>
    </tr>
</table>

## <a name="enabling-research-mode-hololens-first-gen-and-hololens-2"></a><span data-ttu-id="e1cda-147">啟用 Research 模式 (HoloLens first Gen 和 HoloLens 2) </span><span class="sxs-lookup"><span data-stu-id="e1cda-147">Enabling Research Mode (HoloLens first Gen and HoloLens 2)</span></span>

<span data-ttu-id="e1cda-148">研究模式是開發人員模式的延伸模組。</span><span class="sxs-lookup"><span data-stu-id="e1cda-148">Research Mode is an extension of Developer Mode.</span></span> <span data-ttu-id="e1cda-149">開始之前，必須先啟用裝置的開發人員功能，才能存取研究模式設定：</span><span class="sxs-lookup"><span data-stu-id="e1cda-149">Before starting, the developer features of the device need to be enabled to access the Research Mode settings:</span></span> 

* <span data-ttu-id="e1cda-150">開啟 [ **開始] 功能表 > 設定** ]，然後選取 [ **更新**]。</span><span class="sxs-lookup"><span data-stu-id="e1cda-150">Open **Start Menu > Settings** and select **Updates**.</span></span>
* <span data-ttu-id="e1cda-151">**針對開發人員** 選取並啟用 **開發人員模式**。</span><span class="sxs-lookup"><span data-stu-id="e1cda-151">Select **For Developers** and enable **Developer Mode**.</span></span>
* <span data-ttu-id="e1cda-152">向下捲動並啟用 **裝置入口網站**。</span><span class="sxs-lookup"><span data-stu-id="e1cda-152">Scroll down and enable **Device Portal**.</span></span>

<span data-ttu-id="e1cda-153">啟用開發人員功能之後，請連線 [到裝置入口網站](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-hololens) 以啟用研究模式功能：</span><span class="sxs-lookup"><span data-stu-id="e1cda-153">After the developer features  are enabled, [connect to the device portal](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-hololens) to enable the Research Mode features:</span></span>

* <span data-ttu-id="e1cda-154">移至 **裝置入口網站** 中的 **系統 > 研究模式**。</span><span class="sxs-lookup"><span data-stu-id="e1cda-154">Go to **System > Research Mode** in the **Device Portal**.</span></span>
* <span data-ttu-id="e1cda-155">選取 [ **允許存取感應器串流**]。</span><span class="sxs-lookup"><span data-stu-id="e1cda-155">Select **Allow access to sensor stream**.</span></span>
* <span data-ttu-id="e1cda-156">從頁面頂端的 [ **電源** ] 功能表項目目重新開機裝置。</span><span class="sxs-lookup"><span data-stu-id="e1cda-156">Restart the device from the **Power** menu item at the top of the page.</span></span>

<span data-ttu-id="e1cda-157">當您重新開機裝置之後，透過 **裝置入口網站** 載入的應用程式就可以存取研究模式串流。</span><span class="sxs-lookup"><span data-stu-id="e1cda-157">Once you've restarted the device, the applications loaded through the **Device Portal** can access Research Mode streams.</span></span>

<span data-ttu-id="e1cda-158">![HoloLens 裝置入口網站的 [研究模式] 索引標籤](images/ResearchModeDevPortal.png)</span><span class="sxs-lookup"><span data-stu-id="e1cda-158">![Research Mode tab of HoloLens Device Portal](images/ResearchModeDevPortal.png)</span></span><br>
<span data-ttu-id="e1cda-159">*HoloLens 裝置入口網站中的研究強制回應視窗*</span><span class="sxs-lookup"><span data-stu-id="e1cda-159">*Research Mode window in the HoloLens Device Portal*</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e1cda-160">從組建19041.1356 開始提供 HoloLens 2 的研究模式。</span><span class="sxs-lookup"><span data-stu-id="e1cda-160">Research Mode for HoloLens 2 is available beginning with build 19041.1356.</span></span> <span data-ttu-id="e1cda-161">如果您需要在先前的組建中存取，請註冊我們的 [Insider preview](https://docs.microsoft.com/hololens/hololens-insider) 計畫。</span><span class="sxs-lookup"><span data-stu-id="e1cda-161">If you need access in an earlier build, sign up for our [Insider Preview](https://docs.microsoft.com/hololens/hololens-insider) program.</span></span>

### <a name="using-sensor-data-in-your-apps"></a><span data-ttu-id="e1cda-162">在您的應用程式中使用感應器資料</span><span class="sxs-lookup"><span data-stu-id="e1cda-162">Using sensor data in your apps</span></span>

<span data-ttu-id="e1cda-163">應用程式可以使用 [媒體基礎](https://msdn.microsoft.com/library/windows/desktop/ms694197) 存取相片和攝影機串流的相同方式來存取感應器串流資料。</span><span class="sxs-lookup"><span data-stu-id="e1cda-163">Applications can access the sensor stream data in the same way that [Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197) accesses photo and video camera streams.</span></span> 

<span data-ttu-id="e1cda-164">所有適用于 HoloLens 開發的 Api 也都可在研究模式中取得。</span><span class="sxs-lookup"><span data-stu-id="e1cda-164">All APIs that work for HoloLens development are also available in Research Mode.</span></span> <span data-ttu-id="e1cda-165">尤其是，應用程式會準確知道 HoloLens 在每個感應器畫面格捕獲時間的6DoF 空間中。</span><span class="sxs-lookup"><span data-stu-id="e1cda-165">In particular, the application  knows precisely where HoloLens is in 6DoF space at each sensor frame capture time.</span></span>

<span data-ttu-id="e1cda-166">我們的範例應用程式會顯示研究模式串流存取、使用 [內建函式和 extrinsics](https://docs.microsoft.com/windows/mixed-reality/locatable-camera#locating-the-device-camera-in-the-world)，以及記錄資料流程：</span><span class="sxs-lookup"><span data-stu-id="e1cda-166">We have sample applications showing Research Mode stream access, using the [intrinsics and extrinsics](https://docs.microsoft.com/windows/mixed-reality/locatable-camera#locating-the-device-camera-in-the-world), and recording streams:</span></span>
* [<span data-ttu-id="e1cda-167">HoloLens (第一代) </span><span class="sxs-lookup"><span data-stu-id="e1cda-167">HoloLens (first gen)</span></span>](https://github.com/Microsoft/HoloLensForCV)
* [<span data-ttu-id="e1cda-168">HoloLens 2</span><span class="sxs-lookup"><span data-stu-id="e1cda-168">HoloLens 2</span></span>](https://github.com/microsoft/HoloLens2ForCV)

## <a name="support"></a><span data-ttu-id="e1cda-169">支援</span><span class="sxs-lookup"><span data-stu-id="e1cda-169">Support</span></span>

<span data-ttu-id="e1cda-170">針對 HoloLens (第一代) ，請使用 HoloLensForCV 存放庫中的 [問題追蹤](https://github.com/Microsoft/HololensForCV/issues) 程式張貼意見反應，並追蹤已知問題。</span><span class="sxs-lookup"><span data-stu-id="e1cda-170">For HoloLens (first gen), use the [issue tracker](https://github.com/Microsoft/HololensForCV/issues) in the HoloLensForCV repository to post feedback and track known issues.</span></span>

<span data-ttu-id="e1cda-171">針對 HoloLens 2，請使用 HoloLens2ForCV 存放庫中的 [問題追蹤](https://github.com/microsoft/HoloLens2ForCV/issues) 程式張貼意見反應，並追蹤已知問題。</span><span class="sxs-lookup"><span data-stu-id="e1cda-171">For HoloLens 2, use the [issue tracker](https://github.com/microsoft/HoloLens2ForCV/issues) in the HoloLens2ForCV repository to post feedback and track known issues.</span></span>

## <a name="see-also"></a><span data-ttu-id="e1cda-172">另請參閱</span><span class="sxs-lookup"><span data-stu-id="e1cda-172">See also</span></span>

* [<span data-ttu-id="e1cda-173">Microsoft 媒體基礎</span><span class="sxs-lookup"><span data-stu-id="e1cda-173">Microsoft Media Foundation</span></span>](https://msdn.microsoft.com/library/windows/desktop/ms694197)
* [<span data-ttu-id="e1cda-174">HoloLensForCV GitHub 存放庫</span><span class="sxs-lookup"><span data-stu-id="e1cda-174">HoloLensForCV GitHub repo</span></span>](https://github.com/Microsoft/HoloLensForCV)
* [<span data-ttu-id="e1cda-175">HoloLens2ForCV GitHub 存放庫</span><span class="sxs-lookup"><span data-stu-id="e1cda-175">HoloLens2ForCV GitHub repo</span></span>](https://github.com/microsoft/HoloLens2ForCV)
* [<span data-ttu-id="e1cda-176">使用 Windows 裝置入口網站</span><span class="sxs-lookup"><span data-stu-id="e1cda-176">Using the Windows Device Portal</span></span>](using-the-windows-device-portal.md)
