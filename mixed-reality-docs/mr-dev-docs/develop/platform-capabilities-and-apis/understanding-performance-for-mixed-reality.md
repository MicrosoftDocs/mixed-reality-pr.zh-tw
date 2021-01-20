---
title: 瞭解混合現實的效能
description: 瞭解分析和優化 Windows Mixed Reality 應用程式效能的 advanced 資訊和詳細資料。
author: hferrone
ms.author: v-hferrone
ms.date: 3/26/2019
ms.topic: article
keywords: Windows Mixed Reality，混合的現實，虛擬實境，VR，MR，效能，優化，CPU，GPU
ms.openlocfilehash: 68aae6408a59b197227ab8cd9042e11f8a255d10
ms.sourcegitcommit: d3a3b4f13b3728cfdd4d43035c806c0791d3f2fe
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/20/2021
ms.locfileid: "98583084"
---
# <a name="understanding-performance-for-mixed-reality"></a><span data-ttu-id="c8c32-104">瞭解混合現實的效能</span><span class="sxs-lookup"><span data-stu-id="c8c32-104">Understanding performance for mixed reality</span></span>

<span data-ttu-id="c8c32-105">本文是瞭解您的混合現實應用程式效能重要性的簡介。</span><span class="sxs-lookup"><span data-stu-id="c8c32-105">This article is an introduction to understanding the significance of performance for your Mixed Reality app.</span></span>  <span data-ttu-id="c8c32-106">如果您的應用程式不是以最佳的畫面播放速率執行，則使用者體驗可能會大幅降低。</span><span class="sxs-lookup"><span data-stu-id="c8c32-106">User experience can be greatly degraded if your application doesn't run at optimal frame rate.</span></span> <span data-ttu-id="c8c32-107">全像是不穩定的，且環境的標頭追蹤將不穩定，而導致使用者體驗不佳。</span><span class="sxs-lookup"><span data-stu-id="c8c32-107">Holograms will appear unstable and head tracking of the environment will be inaccurate, leading to a poor experience for the user.</span></span> <span data-ttu-id="c8c32-108">您必須將效能視為混合現實開發的第一個類別功能，而不是波蘭文工作。</span><span class="sxs-lookup"><span data-stu-id="c8c32-108">Performance must be considered a first class feature for mixed reality development and not a polish task.</span></span>

<span data-ttu-id="c8c32-109">以下列出每個目標平臺的效能幀效能值。</span><span class="sxs-lookup"><span data-stu-id="c8c32-109">The performant framerate values for each target platform are listed below.</span></span>

| <span data-ttu-id="c8c32-110">平台</span><span class="sxs-lookup"><span data-stu-id="c8c32-110">Platform</span></span> | <span data-ttu-id="c8c32-111">目標畫面播放速率</span><span class="sxs-lookup"><span data-stu-id="c8c32-111">Target Frame Rate</span></span> |
|----------|-------------------|
| [<span data-ttu-id="c8c32-112">HoloLens</span><span class="sxs-lookup"><span data-stu-id="c8c32-112">HoloLens</span></span>](/hololens/hololens1-hardware) | <span data-ttu-id="c8c32-113">60 FPS</span><span class="sxs-lookup"><span data-stu-id="c8c32-113">60 FPS</span></span> |
| [<span data-ttu-id="c8c32-114">Windows Mixed Reality Ultra 電腦</span><span class="sxs-lookup"><span data-stu-id="c8c32-114">Windows Mixed Reality Ultra PCs</span></span>](../../discover/immersive-headset-hardware-details.md) | <span data-ttu-id="c8c32-115">90 FPS</span><span class="sxs-lookup"><span data-stu-id="c8c32-115">90 FPS</span></span> |
| [<span data-ttu-id="c8c32-116">Windows Mixed Reality 電腦</span><span class="sxs-lookup"><span data-stu-id="c8c32-116">Windows Mixed Reality PCs</span></span>](../../discover/immersive-headset-hardware-details.md) | <span data-ttu-id="c8c32-117">60 FPS</span><span class="sxs-lookup"><span data-stu-id="c8c32-117">60 FPS</span></span> |

<span data-ttu-id="c8c32-118">下列架構概述達到目標畫面播放速率的最佳作法。</span><span class="sxs-lookup"><span data-stu-id="c8c32-118">The framework below outlines best practices for hitting target frame rates.</span></span> <span data-ttu-id="c8c32-119">建議您閱讀 [unity 文章的效能建議](../unity/performance-recommendations-for-unity.md) ，以取得在 unity 環境中測量及改善畫面播放速率的秘訣。</span><span class="sxs-lookup"><span data-stu-id="c8c32-119">We recommend reading the [performance recommendations for Unity article](../unity/performance-recommendations-for-unity.md) for tips on measuring and improving framerate in the Unity environment.</span></span>

## <a name="understanding-performance-bottlenecks"></a><span data-ttu-id="c8c32-120">瞭解效能瓶頸</span><span class="sxs-lookup"><span data-stu-id="c8c32-120">Understanding performance bottlenecks</span></span>

<span data-ttu-id="c8c32-121">如果您的應用程式有表現不佳的畫面播放速率，則第一個步驟是分析並瞭解應用程式的計算密集型位置。</span><span class="sxs-lookup"><span data-stu-id="c8c32-121">If your app has an underperforming framerate, the first step is to analyze and understand where your application is computationally intensive.</span></span> <span data-ttu-id="c8c32-122">有兩個主要的處理器負責轉譯您場景的工作： CPU 和 GPU，每個都處理混合現實應用程式的不同層面。</span><span class="sxs-lookup"><span data-stu-id="c8c32-122">There are two primary processors responsible for the work to render your scene: the CPU and the GPU, each handling different aspects of your Mixed Reality app.</span></span> <span data-ttu-id="c8c32-123">可能發生瓶頸的三個主要位置如下：</span><span class="sxs-lookup"><span data-stu-id="c8c32-123">The three key places where bottlenecks may occur are:</span></span> 

1. <span data-ttu-id="c8c32-124">**應用程式執行緒-CPU** -
    負責您的應用程式邏輯，包括處理輸入、動畫、物理和其他應用程式邏輯。</span><span class="sxs-lookup"><span data-stu-id="c8c32-124">**App Thread - CPU** -
 Responsible for your app logic, including processing input, animations, physics, and other app logic.</span></span>
2. <span data-ttu-id="c8c32-125">**將執行緒 CPU 轉譯為 gpu** ，負責將您的繪製呼叫提交至 gpu。</span><span class="sxs-lookup"><span data-stu-id="c8c32-125">**Render Thread - CPU to GPU** - Responsible for submitting your draw calls to the GPU.</span></span> <span data-ttu-id="c8c32-126">當您的應用程式想要轉譯物件（例如 cube 或模型）時，此執行緒會將要求傳送至 GPU 以進行作業。</span><span class="sxs-lookup"><span data-stu-id="c8c32-126">When your app wants to render an object such as a cube or model, this thread sends a request to the GPU to do the operations.</span></span>
3. <span data-ttu-id="c8c32-127">**GPU** -最常用來處理應用程式的圖形管線，以將3d 資料 (模型、材質等) 轉換成圖元。</span><span class="sxs-lookup"><span data-stu-id="c8c32-127">**GPU** - Most commonly handles the graphics pipeline of your application to transform 3D data (models, textures, and so on) into pixels.</span></span> <span data-ttu-id="c8c32-128">它最終會產生要提交到您裝置畫面的2D 影像。</span><span class="sxs-lookup"><span data-stu-id="c8c32-128">It ultimately produces a 2D image to submit to your device's screen.</span></span>

![框架的存留期](images/lifetime-of-a-frame.png)

<span data-ttu-id="c8c32-130">通常，HoloLens 應用程式將會有 GPU 界限，但不一定會。</span><span class="sxs-lookup"><span data-stu-id="c8c32-130">Generally, HoloLens applications will be GPU bound, but not always.</span></span> <span data-ttu-id="c8c32-131">您可以使用下列工具和技術，來瞭解您的特定應用程式瓶頸的位置。</span><span class="sxs-lookup"><span data-stu-id="c8c32-131">Use the tools and techniques below to understand where your particular app is bottlenecked.</span></span>

## <a name="how-to-analyze-your-application"></a><span data-ttu-id="c8c32-132">如何分析您的應用程式</span><span class="sxs-lookup"><span data-stu-id="c8c32-132">How to analyze your application</span></span>

<span data-ttu-id="c8c32-133">有許多工具可讓您瞭解混合現實應用程式中的效能設定檔和潛在的瓶頸。</span><span class="sxs-lookup"><span data-stu-id="c8c32-133">There are many tools that allow you to understand the performance profile and potential bottlenecks in your mixed reality application.</span></span> 

<span data-ttu-id="c8c32-134">以下是一些常見的工具，可協助您收集應用程式的深入分析資訊：</span><span class="sxs-lookup"><span data-stu-id="c8c32-134">Below are some common tools to help you gather deep profiling information for your application:</span></span>
- [<span data-ttu-id="c8c32-135">Intel 圖形效能分析器</span><span class="sxs-lookup"><span data-stu-id="c8c32-135">Intel Graphics Performance Analyzers</span></span>](https://software.intel.com/gpa)
- [<span data-ttu-id="c8c32-136">Visual Studio 圖形偵錯工具</span><span class="sxs-lookup"><span data-stu-id="c8c32-136">Visual Studio Graphics Debuggers</span></span>](/visualstudio/debugger/graphics/visual-studio-graphics-diagnostics)
- [<span data-ttu-id="c8c32-137">Unity Profiler</span><span class="sxs-lookup"><span data-stu-id="c8c32-137">Unity Profiler</span></span>](https://docs.unity3d.com/Manual/Profiler.html)
- [<span data-ttu-id="c8c32-138">Unity 框架偵錯工具</span><span class="sxs-lookup"><span data-stu-id="c8c32-138">Unity Frame Debugger</span></span>](https://docs.unity3d.com/Manual/FrameDebugger.html)

### <a name="how-to-profile-in-any-environment"></a><span data-ttu-id="c8c32-139">如何在任何環境中進行分析</span><span class="sxs-lookup"><span data-stu-id="c8c32-139">How to profile in any environment</span></span>

<span data-ttu-id="c8c32-140">判斷您的應用程式是否為 GPU 或受 CPU 限制的其中一種方式，是降低轉譯目標輸出的解析度。</span><span class="sxs-lookup"><span data-stu-id="c8c32-140">One way to determine if your app is GPU or CPU bound is to lower the resolution of the render target output.</span></span> <span data-ttu-id="c8c32-141">藉由減少要計算的圖元數，您將會降低 GPU 負載。</span><span class="sxs-lookup"><span data-stu-id="c8c32-141">By reducing the number of pixels to calculate, you'll reduce your GPU load.</span></span> <span data-ttu-id="c8c32-142">裝置會轉譯成較小的材質，然後轉譯為顯示您最終影像的向上取樣。</span><span class="sxs-lookup"><span data-stu-id="c8c32-142">The device will render to a smaller texture, then up-sample to display your final image.</span></span>

<span data-ttu-id="c8c32-143">減少轉譯解析度之後，如果：</span><span class="sxs-lookup"><span data-stu-id="c8c32-143">After lowering rendering resolution, if:</span></span>
1) <span data-ttu-id="c8c32-144">應用程式的畫面播放速率增加時，您可能 **會** 受到 **GPU 的限制**</span><span class="sxs-lookup"><span data-stu-id="c8c32-144">Application framerate **increases**, then you're likely **GPU Bound**</span></span>
1) <span data-ttu-id="c8c32-145">應用程式 **的幀** 速率沒有變更，因此您可能會受 **限於 CPU**</span><span class="sxs-lookup"><span data-stu-id="c8c32-145">Application framerate **unchanged**, then you're likely **CPU Bound**</span></span>

>[!NOTE]
><span data-ttu-id="c8c32-146">Unity 能讓您在執行時間透過 *[XRSettings renderViewportScale](https://docs.unity3d.com/ScriptReference/XR.XRSettings-renderViewportScale.html)* 屬性，輕鬆修改應用程式的轉譯目標解析度。</span><span class="sxs-lookup"><span data-stu-id="c8c32-146">Unity provides the ability to easily modify the render target resolution of your application at runtime through the *[XRSettings.renderViewportScale](https://docs.unity3d.com/ScriptReference/XR.XRSettings-renderViewportScale.html)* property.</span></span> <span data-ttu-id="c8c32-147">裝置上顯示的最終映射具有固定的解析度。</span><span class="sxs-lookup"><span data-stu-id="c8c32-147">The final image presented on device has a fixed resolution.</span></span> <span data-ttu-id="c8c32-148">平臺會取樣較低解析度的輸出，以建立較高的解析度影像，以便在顯示時呈現。</span><span class="sxs-lookup"><span data-stu-id="c8c32-148">The platform will sample the lower resolution output to build a higher resolution image for rendering on displays.</span></span> 
>
>```CS
>UnityEngine.XR.XRSettings.renderScale = 0.7f;
>```

## <a name="how-to-improve-your-application"></a><span data-ttu-id="c8c32-149">如何改進您的應用程式</span><span class="sxs-lookup"><span data-stu-id="c8c32-149">How to improve your application</span></span>

### <a name="cpu-performance-recommendations"></a><span data-ttu-id="c8c32-150">CPU 效能建議</span><span class="sxs-lookup"><span data-stu-id="c8c32-150">CPU performance recommendations</span></span>

<span data-ttu-id="c8c32-151">一般來說，在 CPU 上的混合現實應用程式中，大部分的工作都牽涉到執行場景的「模擬」，並處理您的應用程式邏輯。</span><span class="sxs-lookup"><span data-stu-id="c8c32-151">Generally, most work in a mixed reality application on the CPU involves doing the "simulation" of the scene and processing your application logic.</span></span> <span data-ttu-id="c8c32-152">以下是優化的目的地區域：</span><span class="sxs-lookup"><span data-stu-id="c8c32-152">The following areas are targeted for optimization:</span></span>

- <span data-ttu-id="c8c32-153">動畫</span><span class="sxs-lookup"><span data-stu-id="c8c32-153">Animations</span></span>
- <span data-ttu-id="c8c32-154">物理特性</span><span class="sxs-lookup"><span data-stu-id="c8c32-154">Physics</span></span>
- <span data-ttu-id="c8c32-155">記憶體配置</span><span class="sxs-lookup"><span data-stu-id="c8c32-155">Memory allocations</span></span>
- <span data-ttu-id="c8c32-156">複雜的演算法 (即</span><span class="sxs-lookup"><span data-stu-id="c8c32-156">Complex algorithms (i.e</span></span> <span data-ttu-id="c8c32-157">反向運動，路徑-尋找) </span><span class="sxs-lookup"><span data-stu-id="c8c32-157">inverse kinematics, path-finding)</span></span>

### <a name="gpu-performance-recommendations"></a><span data-ttu-id="c8c32-158">GPU 效能建議</span><span class="sxs-lookup"><span data-stu-id="c8c32-158">GPU performance recommendations</span></span>

#### <a name="understanding-bandwidth-vs-fill-rate"></a><span data-ttu-id="c8c32-159">瞭解頻寬與填滿率</span><span class="sxs-lookup"><span data-stu-id="c8c32-159">Understanding bandwidth vs. fill rate</span></span>
<span data-ttu-id="c8c32-160">在 GPU 上轉譯框架時，應用程式會受到記憶體頻寬或填滿率的限制。</span><span class="sxs-lookup"><span data-stu-id="c8c32-160">When rendering a frame on the GPU, an application is either bound by memory bandwidth or fill rate.</span></span>

- <span data-ttu-id="c8c32-161">**記憶體頻寬** 是 GPU 可從記憶體進行的讀取和寫入速率</span><span class="sxs-lookup"><span data-stu-id="c8c32-161">**Memory bandwidth** is the rate of reads and writes the GPU can do from memory</span></span>
    - <span data-ttu-id="c8c32-162">若要找出頻寬限制，請減少材質品質，並檢查畫面播放速率是否已改善。</span><span class="sxs-lookup"><span data-stu-id="c8c32-162">To identify bandwidth limitations, reduce texture quality and check if the framerate has improved.</span></span>
    - <span data-ttu-id="c8c32-163">在 Unity 中，變更 [**編輯**   >  **專案設定**  >  \*\*[品質] 設定](https://docs.unity3d.com/Manual/class-QualitySettings.html)\*\* 中的材質品質。</span><span class="sxs-lookup"><span data-stu-id="c8c32-163">In Unity, change **Texture Quality** in **Edit** > **Project Settings** > **[Quality Settings](https://docs.unity3d.com/Manual/class-QualitySettings.html)**.</span></span>
- <span data-ttu-id="c8c32-164">**填滿率** 是指 GPU 每秒可繪製的圖元。</span><span class="sxs-lookup"><span data-stu-id="c8c32-164">**Fill rate** refers to the pixels that can be drawn per second by the GPU.</span></span>
    - <span data-ttu-id="c8c32-165">若要識別填滿速率限制，請降低顯示器解析度，並檢查是否已改善幀。</span><span class="sxs-lookup"><span data-stu-id="c8c32-165">To identify fill rate limitations, lower the display resolution and check if framerate improved.</span></span> 
    - <span data-ttu-id="c8c32-166">在 Unity 中，使用  *[XRSettings. renderViewportScale](https://docs.unity3d.com/ScriptReference/XR.XRSettings-renderViewportScale.html)* 屬性</span><span class="sxs-lookup"><span data-stu-id="c8c32-166">In Unity, use the  *[XRSettings.renderViewportScale](https://docs.unity3d.com/ScriptReference/XR.XRSettings-renderViewportScale.html)* property</span></span>

<span data-ttu-id="c8c32-167">記憶體頻寬通常牽涉到下列其中一項的優化：</span><span class="sxs-lookup"><span data-stu-id="c8c32-167">Memory bandwidth generally involves optimizations to either:</span></span>
1) <span data-ttu-id="c8c32-168">較低材質解析度</span><span class="sxs-lookup"><span data-stu-id="c8c32-168">Lower texture resolutions</span></span>
2) <span data-ttu-id="c8c32-169">使用較少的材質 (法線、反射等等) </span><span class="sxs-lookup"><span data-stu-id="c8c32-169">Use fewer textures (normals, specular, and so on)</span></span>

<span data-ttu-id="c8c32-170">填滿率著重于減少需要針對最終轉譯圖元計算的作業數目，包括：</span><span class="sxs-lookup"><span data-stu-id="c8c32-170">Fill rate is focused on reducing the number of operations that need to be computed for a final rendered pixel, including:</span></span>
1) <span data-ttu-id="c8c32-171">要呈現/處理的物件數目</span><span class="sxs-lookup"><span data-stu-id="c8c32-171">Number of objects to render/process</span></span>
2) <span data-ttu-id="c8c32-172">每個著色器的作業數目</span><span class="sxs-lookup"><span data-stu-id="c8c32-172">Number of operations per shader</span></span>
3) <span data-ttu-id="c8c32-173">最終結果的 GPU 階段數 (幾何著色器、後置處理效果等等) </span><span class="sxs-lookup"><span data-stu-id="c8c32-173">Number of GPU stages to final result (geometry shaders, post-processing effects, and so on)</span></span>
4) <span data-ttu-id="c8c32-174">轉譯 (顯示解析度) 的圖元數</span><span class="sxs-lookup"><span data-stu-id="c8c32-174">Number of pixels to render (display resolution)</span></span>

#### <a name="reduce-polygon-count"></a><span data-ttu-id="c8c32-175">減少多邊形計數</span><span class="sxs-lookup"><span data-stu-id="c8c32-175">Reduce polygon count</span></span>

<span data-ttu-id="c8c32-176">較高的多邊形計數會導致 GPU 的更多作業，因此減少場景中的 [多邊形數目](/dynamics365/mixed-reality/import-tool/optimize-models#performance-targets) 可減少轉譯時間。</span><span class="sxs-lookup"><span data-stu-id="c8c32-176">Higher polygon counts result in more operations for the GPU, so [reducing the number of polygons](/dynamics365/mixed-reality/import-tool/optimize-models#performance-targets) in your scene reduces the render time.</span></span> <span data-ttu-id="c8c32-177">還有其他因素會讓幾何的陰影變得相當昂貴，但是多邊形計數是最簡單的度量，可判斷呈現場景所需的工作量。</span><span class="sxs-lookup"><span data-stu-id="c8c32-177">There are other factors that make shading the geometry expensive, but polygon count is the simplest metric to determine how much work it will take to render a scene.</span></span>

#### <a name="limit-overdraw"></a><span data-ttu-id="c8c32-178">限制過度繪製</span><span class="sxs-lookup"><span data-stu-id="c8c32-178">Limit overdraw</span></span>

<span data-ttu-id="c8c32-179">當轉譯多個物件但未在螢幕上顯示時，如果遮蔽物件隱藏了多個物件，就會發生高過度繪製。</span><span class="sxs-lookup"><span data-stu-id="c8c32-179">High overdraw occurs when multiple objects are rendered but not shown on screen as they're hidden by an occluding object.</span></span> <span data-ttu-id="c8c32-180">想像一下一下有物件背後的牆。</span><span class="sxs-lookup"><span data-stu-id="c8c32-180">Imagine looking at a wall that has objects behind it.</span></span> <span data-ttu-id="c8c32-181">所有幾何都會處理以進行轉譯，但只需要轉譯不透明的牆，這會導致不必要的作業。</span><span class="sxs-lookup"><span data-stu-id="c8c32-181">All of the geometry would be processed for rendering, but only the opaque wall needs to be rendered, which results in unnecessary operations.</span></span>

#### <a name="shaders"></a><span data-ttu-id="c8c32-182">著色器</span><span class="sxs-lookup"><span data-stu-id="c8c32-182">Shaders</span></span>

<span data-ttu-id="c8c32-183">著色器是在 GPU 上執行的小型程式，且會在轉譯時執行兩個重要的步驟：</span><span class="sxs-lookup"><span data-stu-id="c8c32-183">Shaders are small programs that run on the GPU and do two important steps in rendering:</span></span>
1) <span data-ttu-id="c8c32-184">判斷應繪製哪些頂點，以及它們在螢幕空間 (頂點著色器的位置) </span><span class="sxs-lookup"><span data-stu-id="c8c32-184">Determining which vertices should be drawn and where they are in screen space (the Vertex shader)</span></span>
    - <span data-ttu-id="c8c32-185">每個網格的頂點著色器都會針對每個頂點執行。</span><span class="sxs-lookup"><span data-stu-id="c8c32-185">The Vertex shader is executed per vertex for every mesh.</span></span>
2) <span data-ttu-id="c8c32-186">判斷圖元著色器 (每個圖元的色彩) </span><span class="sxs-lookup"><span data-stu-id="c8c32-186">Determining the color of each pixel (the Pixel shader)</span></span>
    - <span data-ttu-id="c8c32-187">圖元著色器是依圖元執行，並由幾何轉譯為目標呈現材質。</span><span class="sxs-lookup"><span data-stu-id="c8c32-187">The Pixel shader is executed per pixel and rendered by the geometry to the target render texture.</span></span>

<span data-ttu-id="c8c32-188">一般而言，著色器會進行許多轉換和光源計算。</span><span class="sxs-lookup"><span data-stu-id="c8c32-188">Typically, shaders do many transformations and lighting calculations.</span></span> <span data-ttu-id="c8c32-189">雖然複雜的光源模型、陰影和其他作業可以產生絕佳的結果，但它們也有價格。</span><span class="sxs-lookup"><span data-stu-id="c8c32-189">Although complex lighting models, shadows, and other operations can generate fantastic results, they also come with a price.</span></span> <span data-ttu-id="c8c32-190">減少著色器中計算的作業數目，可以大幅減少每個畫面的 GPU 所需的工作量。</span><span class="sxs-lookup"><span data-stu-id="c8c32-190">Reducing the number of operations computed in shaders can greatly reduce the work needed for the GPU per frame.</span></span>

##### <a name="shader-coding-recommendations"></a><span data-ttu-id="c8c32-191">著色器編碼建議</span><span class="sxs-lookup"><span data-stu-id="c8c32-191">Shader coding recommendations</span></span>

- <span data-ttu-id="c8c32-192">盡可能使用雙線性篩選</span><span class="sxs-lookup"><span data-stu-id="c8c32-192">Use bilinear filtering, whenever possible</span></span>
- <span data-ttu-id="c8c32-193">重新排列運算式以使用 MAD 內建函式，同時執行乘法和 add</span><span class="sxs-lookup"><span data-stu-id="c8c32-193">Rearrange expressions to use MAD intrinsics to do a multiply and an add at the same time</span></span>
- <span data-ttu-id="c8c32-194">在 CPU 上盡可能 Precalculate 並以常數形式傳遞至材質</span><span class="sxs-lookup"><span data-stu-id="c8c32-194">Precalculate as much as possible on the CPU and pass as constants to the material</span></span>
- <span data-ttu-id="c8c32-195">**偏好將作業從圖元著色器移至頂點著色器**</span><span class="sxs-lookup"><span data-stu-id="c8c32-195">**Favor moving operations from the pixel shader to the vertex shader**</span></span>
    - <span data-ttu-id="c8c32-196">一般來說，頂點的數目小於圖元數 (720p 是921600圖元、1080p 是2073600圖元，依此類推) </span><span class="sxs-lookup"><span data-stu-id="c8c32-196">Generally, the number of vertices is much smaller than the number of pixels (720p is 921,600 pixels, 1080p is 2,073,600 pixels, and so on)</span></span>

#### <a name="remove-gpu-stages"></a><span data-ttu-id="c8c32-197">移除 GPU 階段</span><span class="sxs-lookup"><span data-stu-id="c8c32-197">Remove GPU stages</span></span>

<span data-ttu-id="c8c32-198">後續處理的效果可能很昂貴，而且會提高應用程式的填滿率，包括如 MSAA 的消除鋸齒技術。</span><span class="sxs-lookup"><span data-stu-id="c8c32-198">Post-processing effects can be expensive and increase the fill rate of your application, including anti-aliasing techniques like MSAA.</span></span> <span data-ttu-id="c8c32-199">在 HoloLens 上，建議您避免使用這些技術和其他著色器階段，例如幾何、輪廓和計算著色器。</span><span class="sxs-lookup"><span data-stu-id="c8c32-199">On HoloLens, we recommended avoiding these techniques and additional shader stages such as geometry, hull, and compute shaders.</span></span>

## <a name="memory-recommendations"></a><span data-ttu-id="c8c32-200">記憶體建議</span><span class="sxs-lookup"><span data-stu-id="c8c32-200">Memory recommendations</span></span>

<span data-ttu-id="c8c32-201">過度的記憶體配置和解除配置作業可能會導致效能不一致、凍結的框架和其他不利行為。</span><span class="sxs-lookup"><span data-stu-id="c8c32-201">Excessive memory allocation and deallocation operations can result in inconsistent performance, frozen frames, and other detrimental behavior.</span></span> <span data-ttu-id="c8c32-202">在 Unity 中進行開發時，請務必瞭解記憶體考慮，因為記憶體管理是由垃圾收集行程所控制。</span><span class="sxs-lookup"><span data-stu-id="c8c32-202">It's especially important to understand memory considerations when developing in Unity, since memory management is controlled by the garbage collector.</span></span>

#### <a name="object-pooling"></a><span data-ttu-id="c8c32-203">物件集區</span><span class="sxs-lookup"><span data-stu-id="c8c32-203">Object pooling</span></span>

<span data-ttu-id="c8c32-204">物件共用是一種常用的技巧，可降低持續配置和物件取消配置的成本。</span><span class="sxs-lookup"><span data-stu-id="c8c32-204">Object pooling is a popular technique to reduce the cost of continuous allocations and deallocations of objects.</span></span> <span data-ttu-id="c8c32-205">這是藉由配置相同物件的大型集區，並重複使用此集區中非使用中的可用實例來完成，而不是在一段時間內不斷產生和終結物件。</span><span class="sxs-lookup"><span data-stu-id="c8c32-205">This is done by allocating a large pool of identical objects and reusing inactive, available instances from this pool instead of constantly spawning and destroying objects over time.</span></span> <span data-ttu-id="c8c32-206">物件集區非常適合在應用程式期間有變數存留期的重複使用元件。</span><span class="sxs-lookup"><span data-stu-id="c8c32-206">Object pools are great for reuseable components that have variable lifetime during an app.</span></span>

## <a name="see-also"></a><span data-ttu-id="c8c32-207">另請參閱</span><span class="sxs-lookup"><span data-stu-id="c8c32-207">See also</span></span>
- [<span data-ttu-id="c8c32-208">對 Unity 的效能建議</span><span class="sxs-lookup"><span data-stu-id="c8c32-208">Performance recommendations for Unity</span></span>](../unity/performance-recommendations-for-unity.md)
- [<span data-ttu-id="c8c32-209">Unity 的建議設定</span><span class="sxs-lookup"><span data-stu-id="c8c32-209">Recommended settings for Unity</span></span>](../unity/recommended-settings-for-unity.md)
- [<span data-ttu-id="c8c32-210">優化3D 模型</span><span class="sxs-lookup"><span data-stu-id="c8c32-210">Optimize 3D models</span></span>](/dynamics365/mixed-reality/import-tool/optimize-models#performance-targets)
- [<span data-ttu-id="c8c32-211">轉換和優化即時3D 模型的最佳作法</span><span class="sxs-lookup"><span data-stu-id="c8c32-211">Best practices for converting and optimizing real-time 3D Models</span></span>](/dynamics365/mixed-reality/import-tool/best-practices)