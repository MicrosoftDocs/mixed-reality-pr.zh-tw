---
title: DirectX 中的座標系統
description: 瞭解 DirectX 中的座標系統，以及空間定位器、參考框架和空間錨點的混合現實。 使用 SpatialStage 和處理追蹤遺失、儲存和載入錨點，以及影像穩定。
author: thetuvix
ms.author: alexturn
ms.date: 08/04/2020
ms.topic: article
keywords: 混合的現實、空間定位器、空間參考框架、空間座標系統、空間階段、範例程式碼、影像穩定、空間錨點、空間錨點存放區、追蹤遺失、逐步解說、混合現實耳機、windows mixed Reality 耳機、虛擬實境耳機
ms.openlocfilehash: 7bf2309f3fb6264d6b1a5232f7ead78b771c1649
ms.sourcegitcommit: 2bf79eef6a9b845494484f458443ef4f89d7efc0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/17/2020
ms.locfileid: "97613112"
---
# <a name="coordinate-systems-in-directx"></a><span data-ttu-id="2f851-105">DirectX 中的座標系統</span><span class="sxs-lookup"><span data-stu-id="2f851-105">Coordinate systems in DirectX</span></span>

> [!NOTE]
> <span data-ttu-id="2f851-106">本文與舊版 WinRT 原生 Api 相關。</span><span class="sxs-lookup"><span data-stu-id="2f851-106">This article relates to the legacy WinRT native APIs.</span></span>  <span data-ttu-id="2f851-107">針對新的原生應用程式專案，建議使用 **[OPENXR API](openxr-getting-started.md)**。</span><span class="sxs-lookup"><span data-stu-id="2f851-107">For new native app projects, we recommend using the **[OpenXR API](openxr-getting-started.md)**.</span></span>

<span data-ttu-id="2f851-108">[座標系統](../../design/coordinate-systems.md) 形成 Windows Mixed Reality api 所提供的空間理解基礎。</span><span class="sxs-lookup"><span data-stu-id="2f851-108">[Coordinate systems](../../design/coordinate-systems.md) form the basis for spatial understanding offered by Windows Mixed Reality APIs.</span></span>

<span data-ttu-id="2f851-109">現今的上建的 VR 或單一房間的 VR 裝置會為其追蹤空間建立一個主要座標系統。</span><span class="sxs-lookup"><span data-stu-id="2f851-109">Today's seated VR or single-room VR devices establish one primary coordinate system for their tracked space.</span></span> <span data-ttu-id="2f851-110">HoloLens 這類的混合現實裝置是針對大型未定義的環境所設計，而裝置會在使用者四處四處探索和學習其周圍。</span><span class="sxs-lookup"><span data-stu-id="2f851-110">Mixed Reality devices like HoloLens are designed for large undefined environments, with the device discovering and learning about its surroundings as the user walks around.</span></span> <span data-ttu-id="2f851-111">裝置可調整以持續改善使用者房間的知識，但會導致在應用程式存留期內，改變彼此之間關聯性的座標系統。</span><span class="sxs-lookup"><span data-stu-id="2f851-111">The device adapts to continually improving knowledge about the user's rooms, but results in coordinate systems that change their relationship to one another over the apps lifetime.</span></span> <span data-ttu-id="2f851-112">Windows Mixed Reality 支援各式各樣的裝置，範圍從內建的沉浸式耳機到世界附加的參考框架。</span><span class="sxs-lookup"><span data-stu-id="2f851-112">Windows Mixed Reality supports a wide spectrum of devices, ranging from seated immersive headsets through world-attached reference frames.</span></span>

>[!NOTE]
><span data-ttu-id="2f851-113">本文中的程式碼片段目前示範如何使用 c + + [/cx，而](creating-a-holographic-directx-project.md)不是 c + + 全像 c + + 全像 c + + 的 + 17 相容 c + +/WinRT。</span><span class="sxs-lookup"><span data-stu-id="2f851-113">The code snippets in this article currently demonstrate use of C++/CX rather than C++17-compliant C++/WinRT as used in the [C++ holographic project template](creating-a-holographic-directx-project.md).</span></span>  <span data-ttu-id="2f851-114">這些概念對 c + +/WinRT 專案而言是相等的，不過您必須轉譯程式碼。</span><span class="sxs-lookup"><span data-stu-id="2f851-114">The concepts are equivalent for a C++/WinRT project, though you will need to translate the code.</span></span>

## <a name="spatial-coordinate-systems-in-windows"></a><span data-ttu-id="2f851-115">Windows 中的空間座標系統</span><span class="sxs-lookup"><span data-stu-id="2f851-115">Spatial coordinate systems in Windows</span></span>

<span data-ttu-id="2f851-116">在 Windows 中用來因應實際座標系統原因的核心類型是 <a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialcoordinatesystem" target="_blank">SpatialCoordinateSystem</a>。</span><span class="sxs-lookup"><span data-stu-id="2f851-116">The core type used to reason about real-world coordinate systems in Windows is the <a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialcoordinatesystem" target="_blank">SpatialCoordinateSystem</a>.</span></span> <span data-ttu-id="2f851-117">此類型的實例代表任意座標系統，提供取得轉換矩陣資料的方法，您可以使用此方法在兩個座標系統之間進行轉換，而不需要瞭解每個座標系統的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="2f851-117">An instance of this type represents an arbitrary coordinate system, providing a method for getting transformation matrix data that you can use to transform between two coordinate systems without understanding the details of each.</span></span>

<span data-ttu-id="2f851-118">傳回空間資訊的方法會接受 SpatialCoordinateSystem 參數，讓您決定最適合傳回這些座標的座標系統。</span><span class="sxs-lookup"><span data-stu-id="2f851-118">Methods that return spatial information will accept a SpatialCoordinateSystem parameter to let you decide the coordinate system in which it's most useful for those coordinates to be returned.</span></span> <span data-ttu-id="2f851-119">空間資訊以點、光線或磁片區的形式表示在使用者的環境中，而這些座標的單位一律會是計量。</span><span class="sxs-lookup"><span data-stu-id="2f851-119">Spatial information is represented as points, rays, or volumes in the user's surroundings, and the units for these coordinates will always be in meters.</span></span>

<span data-ttu-id="2f851-120">SpatialCoordinateSystem 與其他座標系統有動態的關聯性，包括代表裝置位置的關聯性。</span><span class="sxs-lookup"><span data-stu-id="2f851-120">A SpatialCoordinateSystem has a dynamic relationship with other coordinate systems, including those that represent the device's position.</span></span> <span data-ttu-id="2f851-121">在任何時間點，裝置都可以找到某些座標系統，而不是其他系統。</span><span class="sxs-lookup"><span data-stu-id="2f851-121">At any point, the device can locate some coordinate systems and not others.</span></span> <span data-ttu-id="2f851-122">對於大部分的座標系統而言，您的應用程式必須準備好處理它們找不到的時間週期。</span><span class="sxs-lookup"><span data-stu-id="2f851-122">For most coordinate systems, your app must be ready to handle periods of time during which they cannot be located.</span></span>

<span data-ttu-id="2f851-123">您的應用程式不應該直接建立 SpatialCoordinateSystems，而是應該透過認知 Api 取用。</span><span class="sxs-lookup"><span data-stu-id="2f851-123">Your application shouldn't create SpatialCoordinateSystems directly - rather they should be consumed via the Perception APIs.</span></span> <span data-ttu-id="2f851-124">在認知 Api 中有三個主要的座標系統來源，每一個都對應至 [座標系統](../../design/coordinate-systems.md) 頁面上所述的概念：</span><span class="sxs-lookup"><span data-stu-id="2f851-124">There are three primary sources of coordinate systems in the Perception APIs, each of which map to a concept described on the [Coordinate systems](../../design/coordinate-systems.md) page:</span></span>
* <span data-ttu-id="2f851-125">若要取得固定的參考框架，請建立一個 <a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialstationaryframeofreference" target="_blank">SpatialStationaryFrameOfReference</a> ，或從目前的 <a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialstageframeofreference" target="_blank">SpatialStageFrameOfReference</a>取得一個。</span><span class="sxs-lookup"><span data-stu-id="2f851-125">To get a stationary frame of reference, create a <a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialstationaryframeofreference" target="_blank">SpatialStationaryFrameOfReference</a> or obtain one from the current <a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialstageframeofreference" target="_blank">SpatialStageFrameOfReference</a>.</span></span>
* <span data-ttu-id="2f851-126">若要取得空間錨點，請建立 <a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialanchor" target="_blank">SpatialAnchor</a>。</span><span class="sxs-lookup"><span data-stu-id="2f851-126">To get a spatial anchor, create a <a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialanchor" target="_blank">SpatialAnchor</a>.</span></span>
* <span data-ttu-id="2f851-127">若要取得附加的參考框架，請建立 <a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatiallocatorattachedframeofreference" target="_blank">SpatialLocatorAttachedFrameOfReference</a>。</span><span class="sxs-lookup"><span data-stu-id="2f851-127">To get an attached frame of reference, create a <a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatiallocatorattachedframeofreference" target="_blank">SpatialLocatorAttachedFrameOfReference</a>.</span></span>

<span data-ttu-id="2f851-128">這些物件所傳回的所有座標系統都是由右至右、+ y 向上、+ x 向右和 + z 向右。</span><span class="sxs-lookup"><span data-stu-id="2f851-128">All of the coordinate systems returned by these objects are right-handed, with +y up, +x to the right and +z backwards.</span></span> <span data-ttu-id="2f851-129">您可以記住，正 Z 軸指向的方向是指向正 x 方向的手指，然後將它們 curling 為正 y 方向。</span><span class="sxs-lookup"><span data-stu-id="2f851-129">You can remember which direction the positive z-axis points by pointing the fingers of either your left or right hand in the positive x direction and curling them into the positive y direction.</span></span> <span data-ttu-id="2f851-130">您拇指所指的方向，不論是指向您或指向您的反方向，即是該座標系統正 z 軸所指的方向。</span><span class="sxs-lookup"><span data-stu-id="2f851-130">The direction your thumb points, either toward or away from you, is the direction that the positive z-axis points for that coordinate system.</span></span> <span data-ttu-id="2f851-131">下圖顯示這兩個座標系統。</span><span class="sxs-lookup"><span data-stu-id="2f851-131">The following illustration shows these two coordinate systems.</span></span>

<span data-ttu-id="2f851-132">![左側和右側座標系統](images/left-hand-right-hand.gif)</span><span class="sxs-lookup"><span data-stu-id="2f851-132">![Left-hand and right-hand coordinate systems](images/left-hand-right-hand.gif)</span></span><br>
<span data-ttu-id="2f851-133">*左側和右側座標系統*</span><span class="sxs-lookup"><span data-stu-id="2f851-133">*Left-hand and right-hand coordinate systems*</span></span>

<span data-ttu-id="2f851-134">您可以使用 <a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatiallocator" target="_blank">SpatialLocator</a> 類別，根據 HoloLens 位置，在 SpatialCoordinateSystem 中建立附加或固定的參考框架。</span><span class="sxs-lookup"><span data-stu-id="2f851-134">Use the <a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatiallocator" target="_blank">SpatialLocator</a> class to create either an attached or stationary frame of reference to bootstrap into a SpatialCoordinateSystem based on the HoloLens position.</span></span> <span data-ttu-id="2f851-135">若要深入瞭解此程式，請繼續下一節。</span><span class="sxs-lookup"><span data-stu-id="2f851-135">Continue to the next section to learn more about this process.</span></span>

## <a name="place-holograms-in-the-world-using-a-spatial-stage"></a><span data-ttu-id="2f851-136">使用空間階段來放置全球的全像影像</span><span class="sxs-lookup"><span data-stu-id="2f851-136">Place holograms in the world using a spatial stage</span></span>

<span data-ttu-id="2f851-137">透明的座標系統 Windows Mixed Reality 沉浸式耳機是使用 static <a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialstageframeofreference.current" target="_blank">SpatialStageFrameOfReference：： Current</a> 屬性來存取。</span><span class="sxs-lookup"><span data-stu-id="2f851-137">The coordinate system for opaque Windows Mixed Reality immersive headsets is accessed using the static <a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialstageframeofreference.current" target="_blank">SpatialStageFrameOfReference::Current</a> property.</span></span> <span data-ttu-id="2f851-138">此 API 提供：</span><span class="sxs-lookup"><span data-stu-id="2f851-138">This API provides:</span></span>

* <span data-ttu-id="2f851-139">座標系統</span><span class="sxs-lookup"><span data-stu-id="2f851-139">A coordinate system</span></span>
* <span data-ttu-id="2f851-140">玩家是否在裝置上或行動裝置的相關資訊</span><span class="sxs-lookup"><span data-stu-id="2f851-140">Information about whether the player is seated or mobile</span></span>
* <span data-ttu-id="2f851-141">當玩家為行動裝置時，要流覽的安全區域界限</span><span class="sxs-lookup"><span data-stu-id="2f851-141">The boundary of a safe area for walking around if the player is mobile</span></span>
* <span data-ttu-id="2f851-142">指出耳機是否為方向。</span><span class="sxs-lookup"><span data-stu-id="2f851-142">An indication of whether the headset is directional.</span></span> 
* <span data-ttu-id="2f851-143">空間階段更新的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="2f851-143">An event handler for updates to the spatial stage.</span></span>

<span data-ttu-id="2f851-144">首先，我們會取得空間階段並訂閱其更新：</span><span class="sxs-lookup"><span data-stu-id="2f851-144">First, we get the spatial stage and subscribe for updates to it:</span></span> 

<span data-ttu-id="2f851-145">**空間階段初始化** 的程式碼</span><span class="sxs-lookup"><span data-stu-id="2f851-145">Code for **Spatial stage initialization**</span></span>

```
SpatialStageManager::SpatialStageManager(
    const std::shared_ptr<DX::DeviceResources>& deviceResources, 
    const std::shared_ptr<SceneController>& sceneController)
    : m_deviceResources(deviceResources), m_sceneController(sceneController)
{
    // Get notified when the stage is updated.
    m_spatialStageChangedEventToken = SpatialStageFrameOfReference::CurrentChanged +=
        ref new EventHandler<Object^>(std::bind(&SpatialStageManager::OnCurrentChanged, this, _1));

    // Make sure to get the current spatial stage.
    OnCurrentChanged(nullptr);
}
```

<span data-ttu-id="2f851-146">在 OnCurrentChanged 方法中，您的應用程式應該檢查空間階段並更新播放機體驗。</span><span class="sxs-lookup"><span data-stu-id="2f851-146">In the OnCurrentChanged method, your app should inspect the spatial stage and update the player experience.</span></span> <span data-ttu-id="2f851-147">在此範例中，我們會提供階段界限的視覺效果，以及使用者指定的起點和移動屬性範圍的開始位置。</span><span class="sxs-lookup"><span data-stu-id="2f851-147">In this example, we provide a visualization of the stage boundary and the start position specified by the user and the stage's range of view and range of movement properties.</span></span> <span data-ttu-id="2f851-148">當無法提供階段時，我們也會切換回我們自己的固定座標系統。</span><span class="sxs-lookup"><span data-stu-id="2f851-148">We also fall back to our own stationary coordinate system, when a stage cannot be provided.</span></span>


<span data-ttu-id="2f851-149">**空間階段更新** 的程式碼</span><span class="sxs-lookup"><span data-stu-id="2f851-149">Code for **Spatial stage update**</span></span>

```
void SpatialStageManager::OnCurrentChanged(Object^ /*o*/)
{
    // The event notifies us that a new stage is available.
    // Get the current stage.
    m_currentStage = SpatialStageFrameOfReference::Current;

    // Clear previous content.
    m_sceneController->ClearSceneObjects();

    if (m_currentStage != nullptr)
    {
        // Obtain stage geometry.
        auto stageCoordinateSystem = m_currentStage->CoordinateSystem;
        auto boundsVertexArray = m_currentStage->TryGetMovementBounds(stageCoordinateSystem);

        // Visualize the area where the user can move around.
        std::vector<float3> boundsVertices;
        boundsVertices.resize(boundsVertexArray->Length);
        memcpy(boundsVertices.data(), boundsVertexArray->Data, boundsVertexArray->Length * sizeof(float3));
        std::vector<unsigned short> indices = TriangulatePoints(boundsVertices);
        m_stageBoundsShape =
            std::make_shared<SceneObject>(
                    m_deviceResources,
                    reinterpret_cast<std::vector<XMFLOAT3>&>(boundsVertices),
                    indices,
                    XMFLOAT3(DirectX::Colors::SeaGreen),
                    stageCoordinateSystem);
        m_sceneController->AddSceneObject(m_stageBoundsShape);

        // In this sample, we draw a visual indicator for some spatial stage properties.
        // If the view is forward-only, the indicator is a half circle pointing forward - otherwise, it
        // is a full circle.
        // If the user can walk around, the indicator is blue. If the user is seated, it is red.

        // The indicator is rendered at the origin - which is where the user declared the center of the
        // stage to be during setup - above the plane of the stage bounds object.
        float3 visibleAreaCenter = float3(0.f, 0.001f, 0.f);

        // Its shape depends on the look direction range.
        std::vector<float3> visibleAreaIndicatorVertices;
        if (m_currentStage->LookDirectionRange == SpatialLookDirectionRange::ForwardOnly)
        {
            // Half circle for forward-only look direction range.
            visibleAreaIndicatorVertices = CreateCircle(visibleAreaCenter, 0.25f, 9, XM_PI);
        }
        else
        {
            // Full circle for omnidirectional look direction range.
            visibleAreaIndicatorVertices = CreateCircle(visibleAreaCenter, 0.25f, 16, XM_2PI);
        }

        // Its color depends on the movement range.
        XMFLOAT3 visibleAreaColor;
        if (m_currentStage->MovementRange == SpatialMovementRange::NoMovement)
        {
            visibleAreaColor = XMFLOAT3(DirectX::Colors::OrangeRed);
        }
        else
        {
            visibleAreaColor = XMFLOAT3(DirectX::Colors::Aqua);
        }

        std::vector<unsigned short> visibleAreaIndicatorIndices = TriangulatePoints(visibleAreaIndicatorVertices);

        // Visualize the look direction range.
        m_stageVisibleAreaIndicatorShape =
            std::make_shared<SceneObject>(
                    m_deviceResources,
                    reinterpret_cast<std::vector<XMFLOAT3>&>(visibleAreaIndicatorVertices),
                    visibleAreaIndicatorIndices,
                    visibleAreaColor,
                    stageCoordinateSystem);
        m_sceneController->AddSceneObject(m_stageVisibleAreaIndicatorShape);
    }
    else
    {
        // No spatial stage was found.
        // Fall back to a stationary coordinate system.
        auto locator = SpatialLocator::GetDefault();
        if (locator)
        {
            m_stationaryFrameOfReference = locator->CreateStationaryFrameOfReferenceAtCurrentLocation();

            // Render an indicator, so that we know we fell back to a mode without a stage.
            std::vector<float3> visibleAreaIndicatorVertices;
            float3 visibleAreaCenter = float3(0.f, -2.0f, 0.f);
            visibleAreaIndicatorVertices = CreateCircle(visibleAreaCenter, 0.125f, 16, XM_2PI);
            std::vector<unsigned short> visibleAreaIndicatorIndices = TriangulatePoints(visibleAreaIndicatorVertices);
            m_stageVisibleAreaIndicatorShape =
                std::make_shared<SceneObject>(
                    m_deviceResources,
                    reinterpret_cast<std::vector<XMFLOAT3>&>(visibleAreaIndicatorVertices),
                    visibleAreaIndicatorIndices,
                    XMFLOAT3(DirectX::Colors::LightSlateGray),
                    m_stationaryFrameOfReference->CoordinateSystem);
            m_sceneController->AddSceneObject(m_stageVisibleAreaIndicatorShape);
        }
    }
}
```

<span data-ttu-id="2f851-150">以順時針順序提供定義階段界限的頂點集合。</span><span class="sxs-lookup"><span data-stu-id="2f851-150">The set of vertices that define the stage boundary are provided in clockwise order.</span></span> <span data-ttu-id="2f851-151">當使用者在使用時，Windows Mixed Reality shell 會在界限上繪製一個範圍，但是您可能會想要為自己的目的 triangularize walkable 區域。</span><span class="sxs-lookup"><span data-stu-id="2f851-151">The Windows Mixed Reality shell draws a fence at the boundary when the user approaches it, but you may want to triangularize the walkable area for your own purposes.</span></span> <span data-ttu-id="2f851-152">下列演算法可以用來 triangularize 階段。</span><span class="sxs-lookup"><span data-stu-id="2f851-152">The following algorithm can be used to triangularize the stage.</span></span>


<span data-ttu-id="2f851-153">**空間階段 triangularization** 的程式碼</span><span class="sxs-lookup"><span data-stu-id="2f851-153">Code for **Spatial stage triangularization**</span></span>

```
std::vector<unsigned short> SpatialStageManager::TriangulatePoints(std::vector<float3> const& vertices)
{
    size_t const& vertexCount = vertices.size();

    // Segments of the shape are removed as they are triangularized.
    std::vector<bool> vertexRemoved;
    vertexRemoved.resize(vertexCount, false);
    unsigned int vertexRemovedCount = 0;

    // Indices are used to define triangles.
    std::vector<unsigned short> indices;

    // Decompose into convex segments.
    unsigned short currentVertex = 0;
    while (vertexRemovedCount < (vertexCount - 2))
    {
        // Get next triangle:
        // Start with the current vertex.
        unsigned short index1 = currentVertex;

        // Get the next available vertex.
        unsigned short index2 = index1 + 1;

        // This cycles to the next available index.
        auto CycleIndex = [=](unsigned short indexToCycle, unsigned short stopIndex)
        {
            // Make sure the index does not exceed bounds.
            if (indexToCycle >= unsigned short(vertexCount))
            {
                indexToCycle -= unsigned short(vertexCount);
            }

            while (vertexRemoved[indexToCycle])
            {
                // If the vertex is removed, go to the next available one.
                ++indexToCycle;

                // Make sure the index does not exceed bounds.
                if (indexToCycle >= unsigned short(vertexCount))
                {
                    indexToCycle -= unsigned short(vertexCount);
                }

                // Prevent cycling all the way around.
                // Should not be needed, as we limit with the vertex count.
                if (indexToCycle == stopIndex)
                {
                    break;
                }
            }

            return indexToCycle;
        };
        index2 = CycleIndex(index2, index1);

        // Get the next available vertex after that.
        unsigned short index3 = index2 + 1;
        index3 = CycleIndex(index3, index1);

        // Vertices that may define a triangle inside the 2D shape.
        auto& v1 = vertices[index1];
        auto& v2 = vertices[index2];
        auto& v3 = vertices[index3];

        // If the projection of the first segment (in clockwise order) onto the second segment is 
        // positive, we know that the clockwise angle is less than 180 degrees, which tells us 
        // that the triangle formed by the two segments is contained within the bounding shape.
        auto v2ToV1 = v1 - v2;
        auto v2ToV3 = v3 - v2;
        float3 normalToV2ToV3 = { -v2ToV3.z, 0.f, v2ToV3.x };
        float projectionOntoNormal = dot(v2ToV1, normalToV2ToV3);
        if (projectionOntoNormal >= 0)
        {
            // Triangle is contained within the 2D shape.

            // Remove peak vertex from the list.
            vertexRemoved[index2] = true;
            ++vertexRemovedCount;

            // Create the triangle.
            indices.push_back(index1);
            indices.push_back(index2);
            indices.push_back(index3);

            // Continue on to the next outer triangle.
            currentVertex = index3;
        }
        else
        {
            // Triangle is a cavity in the 2D shape.
            // The next triangle starts at the inside corner.
            currentVertex = index2;
        }
    }

    indices.shrink_to_fit();
    return indices;
}
```

## <a name="place-holograms-in-the-world-using-a-stationary-frame-of-reference"></a><span data-ttu-id="2f851-154">使用固定的參考框架來放置全球的全像影像</span><span class="sxs-lookup"><span data-stu-id="2f851-154">Place holograms in the world using a stationary frame of reference</span></span>

<span data-ttu-id="2f851-155">[SpatialStationaryFrameOfReference](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialstationaryframeofreference.aspx)類別代表參考的框架，其會在使用者四處移動時[保持](../../design/coordinate-systems.md#stationary-frame-of-reference)與使用者的周圍相關聯。</span><span class="sxs-lookup"><span data-stu-id="2f851-155">The [SpatialStationaryFrameOfReference](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialstationaryframeofreference.aspx) class represents a frame of reference that [remains stationary](../../design/coordinate-systems.md#stationary-frame-of-reference) relative to the user's surroundings as the user moves around.</span></span> <span data-ttu-id="2f851-156">這段參考的優先順序會讓協調在裝置附近保持穩定。</span><span class="sxs-lookup"><span data-stu-id="2f851-156">This frame of reference prioritizes keeping coordinates stable near the device.</span></span> <span data-ttu-id="2f851-157">SpatialStationaryFrameOfReference 的其中一個主要用途，是在轉譯全像轉譯引擎時，作為轉譯引擎內的基礎全局座標系統。</span><span class="sxs-lookup"><span data-stu-id="2f851-157">One key use of a SpatialStationaryFrameOfReference is to act as the underlying world coordinate system within a rendering engine when rendering holograms.</span></span>

<span data-ttu-id="2f851-158">若要取得 SpatialStationaryFrameOfReference，請使用 [SpatialLocator](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatiallocator.aspx) 類別並呼叫 [CreateStationaryFrameOfReferenceAtCurrentLocation](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatiallocator.createstationaryframeofreferenceatcurrentlocation.aspx)。</span><span class="sxs-lookup"><span data-stu-id="2f851-158">To get a SpatialStationaryFrameOfReference, use the [SpatialLocator](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatiallocator.aspx) class and call [CreateStationaryFrameOfReferenceAtCurrentLocation](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatiallocator.createstationaryframeofreferenceatcurrentlocation.aspx).</span></span>

<span data-ttu-id="2f851-159">從 Windows 全像應用程式範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="2f851-159">From the Windows Holographic app template code:</span></span>

```
           // The simplest way to render world-locked holograms is to create a stationary reference frame
           // when the app is launched. This is roughly analogous to creating a "world" coordinate system
           // with the origin placed at the device's position as the app is launched.
           referenceFrame = locator.CreateStationaryFrameOfReferenceAtCurrentLocation();
```
* <span data-ttu-id="2f851-160">固定的參考框架是設計來提供相對於整體空間的最佳調整位置。</span><span class="sxs-lookup"><span data-stu-id="2f851-160">Stationary reference frames are designed to provide a best-fit position relative to the overall space.</span></span> <span data-ttu-id="2f851-161">該參考框架內的個別位置可以稍微漂移。</span><span class="sxs-lookup"><span data-stu-id="2f851-161">Individual positions within that reference frame are allowed to drift slightly.</span></span> <span data-ttu-id="2f851-162">這是正常的，因為裝置會學習更多有關環境的資訊。</span><span class="sxs-lookup"><span data-stu-id="2f851-162">This is normal as the device learns more about the environment.</span></span>
* <span data-ttu-id="2f851-163">當需要精確放置個別的全像時，應該使用 SpatialAnchor 將個別的全像點錨定到真實世界中的某個位置，例如，使用者指出的某個點是特別感興趣的。</span><span class="sxs-lookup"><span data-stu-id="2f851-163">When precise placement of individual holograms is required, a SpatialAnchor should be used to anchor the individual hologram to a position in the real world - for example, a point the user indicates to be of special interest.</span></span> <span data-ttu-id="2f851-164">錨點位置不會漂移，但可以更正;錨點將會在發生更正之後，使用下一個畫面格中的更正位置開始。</span><span class="sxs-lookup"><span data-stu-id="2f851-164">Anchor positions don't drift, but can be corrected; the anchor will use the corrected position starting in the next frame after the correction has occurred.</span></span>

## <a name="place-holograms-in-the-world-using-spatial-anchors"></a><span data-ttu-id="2f851-165">使用空間錨點放置全球的全像影像</span><span class="sxs-lookup"><span data-stu-id="2f851-165">Place holograms in the world using spatial anchors</span></span>

<span data-ttu-id="2f851-166">[空間錨點](../../design/coordinate-systems.md#spatial-anchors) 是一種很好的方式，可在真實世界中的特定位置放置全像投影，系統可確保錨點會在一段時間內保持不變。</span><span class="sxs-lookup"><span data-stu-id="2f851-166">[Spatial anchors](../../design/coordinate-systems.md#spatial-anchors) are a great way to place holograms at a specific place in the real world, with the system ensuring the anchor stays in place over time.</span></span> <span data-ttu-id="2f851-167">本主題說明如何建立和使用錨點，以及如何使用錨點資料。</span><span class="sxs-lookup"><span data-stu-id="2f851-167">This topic explains how to create and use an anchor, and how to work with anchor data.</span></span>

<span data-ttu-id="2f851-168">您可以在選擇的 SpatialCoordinateSystem 中，于任何位置和方向建立 SpatialAnchor。</span><span class="sxs-lookup"><span data-stu-id="2f851-168">You can create a SpatialAnchor at any position and orientation within the SpatialCoordinateSystem of your choosing.</span></span> <span data-ttu-id="2f851-169">裝置目前必須能夠找出該座標系統，且系統不能達到其空間錨點的限制。</span><span class="sxs-lookup"><span data-stu-id="2f851-169">The device must be able to locate that coordinate system at the moment, and the system must not have reached its limit of spatial anchors.</span></span>

<span data-ttu-id="2f851-170">一旦定義之後，SpatialAnchor 的座標系統會持續調整以保持其初始位置的精確位置和方向。</span><span class="sxs-lookup"><span data-stu-id="2f851-170">Once defined, the coordinate system of a SpatialAnchor adjusts continually to keep the precise position and orientation of its initial location.</span></span> <span data-ttu-id="2f851-171">然後，您可以使用此 SpatialAnchor 來轉譯在確切位置的使用者環境中，會顯示為固定的全像影像。</span><span class="sxs-lookup"><span data-stu-id="2f851-171">You can then use this SpatialAnchor to render holograms that will appear fixed in the user's surroundings at that exact location.</span></span>

<span data-ttu-id="2f851-172">保留錨點的調整效果，會隨著錨點的距離增加而放大。</span><span class="sxs-lookup"><span data-stu-id="2f851-172">The effects of the adjustments that keep the anchor in place are magnified as distance from the anchor increases.</span></span> <span data-ttu-id="2f851-173">您應避免轉譯相對於錨點來源中超過3個計量的內容。</span><span class="sxs-lookup"><span data-stu-id="2f851-173">You should avoid rendering content relative to an anchor that is more than about 3 meters from that anchor's origin.</span></span>

<span data-ttu-id="2f851-174">[CoordinateSystem](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchor.coordinatesystem.aspx)屬性會取得座標系統，可讓您放置相對於錨點的內容，並在裝置調整錨點的精確位置時套用緩和措施。</span><span class="sxs-lookup"><span data-stu-id="2f851-174">The [CoordinateSystem](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchor.coordinatesystem.aspx) property gets a coordinate system that lets you place content relative to the anchor, with easing applied when the device adjusts the anchor's precise location.</span></span>

<span data-ttu-id="2f851-175">使用 [RawCoordinateSystem](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchor.rawcoordinatesystem.aspx) 屬性和對應的 [RawCoordinateSystemAdjusted](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchor.rawcoordinatesystemadjusted.aspx) 事件，自行管理這些調整。</span><span class="sxs-lookup"><span data-stu-id="2f851-175">Use the [RawCoordinateSystem](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchor.rawcoordinatesystem.aspx) property and the corresponding [RawCoordinateSystemAdjusted](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchor.rawcoordinatesystemadjusted.aspx) event to manage these adjustments yourself.</span></span>

### <a name="persist-and-share-spatial-anchors"></a><span data-ttu-id="2f851-176">保存和共用空間錨點</span><span class="sxs-lookup"><span data-stu-id="2f851-176">Persist and share spatial anchors</span></span>

<span data-ttu-id="2f851-177">您可以使用 [SpatialAnchorStore](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchorstore.aspx) 類別在本機保存 SpatialAnchor，然後在相同 HoloLens 裝置上的未來應用程式會話中取回。</span><span class="sxs-lookup"><span data-stu-id="2f851-177">You can persist a SpatialAnchor locally using the [SpatialAnchorStore](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchorstore.aspx) class and then get it back in a future app session on the same HoloLens device.</span></span>

<span data-ttu-id="2f851-178">藉由使用 <a href="https://docs.microsoft.com/azure/spatial-anchors/overview" target="_blank">Azure 空間錨點</a>，您可以從本機 SpatialAnchor 建立持久的雲端錨點，然後您的應用程式就可以在多個 HoloLens、IOS 和 Android 裝置上找到。</span><span class="sxs-lookup"><span data-stu-id="2f851-178">By using <a href="https://docs.microsoft.com/azure/spatial-anchors/overview" target="_blank">Azure Spatial Anchors</a>, you can create a durable cloud anchor from a local SpatialAnchor, which your app can then locate across multiple HoloLens, iOS and Android devices.</span></span>  <span data-ttu-id="2f851-179">藉由在多個裝置上共用一般空間錨點，每個使用者都可以在相同的實體位置即時看到相對於該錨點轉譯的內容。</span><span class="sxs-lookup"><span data-stu-id="2f851-179">By sharing a common spatial anchor across multiple devices, each user can see content rendered relative to that anchor in the same physical location in real time.</span></span> 

<span data-ttu-id="2f851-180">您也可以使用 <a href="https://docs.microsoft.com/azure/spatial-anchors/overview" target="_blank">Azure 空間錨點</a> ，在 HoloLens、IOS 和 Android 裝置上進行非同步全像保存。</span><span class="sxs-lookup"><span data-stu-id="2f851-180">You can also use <a href="https://docs.microsoft.com/azure/spatial-anchors/overview" target="_blank">Azure Spatial Anchors</a> for asynchronous hologram persistence across HoloLens, iOS, and Android devices.</span></span>  <span data-ttu-id="2f851-181">藉由共用長期雲端空間錨點，即使這些裝置不會同時存在，多個裝置也可以觀察經過一段時間的相同保存全息圖。</span><span class="sxs-lookup"><span data-stu-id="2f851-181">By sharing a durable cloud spatial anchor, multiple devices can observe the same persisted hologram over time, even if those devices aren't present together at the same time.</span></span>

<span data-ttu-id="2f851-182">若要開始在您的 HoloLens 應用程式中建立共用體驗，請嘗試5分鐘的 <a href="https://docs.microsoft.com/azure/spatial-anchors/quickstarts/get-started-hololens" target="_blank">Azure 空間錨點 HoloLens 快速入門</a>。</span><span class="sxs-lookup"><span data-stu-id="2f851-182">To get started building shared experiences in your HoloLens app, try out the 5-minute <a href="https://docs.microsoft.com/azure/spatial-anchors/quickstarts/get-started-hololens" target="_blank">Azure Spatial Anchors HoloLens quickstart</a>.</span></span>

<span data-ttu-id="2f851-183">當您啟動並執行 Azure 空間錨點之後，您就可以 <a href="https://docs.microsoft.com/azure/spatial-anchors/concepts/create-locate-anchors-cpp-winrt" target="_blank">在 HoloLens 上建立並找出錨點</a>。</span><span class="sxs-lookup"><span data-stu-id="2f851-183">Once you're up and running with Azure Spatial Anchors, you can then <a href="https://docs.microsoft.com/azure/spatial-anchors/concepts/create-locate-anchors-cpp-winrt" target="_blank">create and locate anchors on HoloLens</a>.</span></span>  <span data-ttu-id="2f851-184">適用于 <a href="https://docs.microsoft.com/azure/spatial-anchors/create-locate-anchors-overview" target="_blank">Android 和 iOS</a> 的逐步解說也可讓您在所有裝置上共用相同的錨點。</span><span class="sxs-lookup"><span data-stu-id="2f851-184">Walkthroughs are available for <a href="https://docs.microsoft.com/azure/spatial-anchors/create-locate-anchors-overview" target="_blank">Android and iOS</a> as well, enabling you to share the same anchors on all devices.</span></span>

### <a name="create-spatialanchors-for-holographic-content"></a><span data-ttu-id="2f851-185">建立全像攝影內容的 SpatialAnchors</span><span class="sxs-lookup"><span data-stu-id="2f851-185">Create SpatialAnchors for holographic content</span></span>

<span data-ttu-id="2f851-186">在此程式碼範例中，我們已修改 Windows 全像應用程式範本，以在偵測到已 **按下** 的手勢時建立錨點。</span><span class="sxs-lookup"><span data-stu-id="2f851-186">For this code sample, we modified the Windows Holographic app template to create anchors when the **Pressed** gesture is detected.</span></span> <span data-ttu-id="2f851-187">Cube 接著會在轉譯階段放置於錨點。</span><span class="sxs-lookup"><span data-stu-id="2f851-187">The cube is then placed at the anchor during the render pass.</span></span>

<span data-ttu-id="2f851-188">因為協助程式類別支援多個錨點，所以我們可以將多個 cube 放在我們想要使用此程式碼範例的位置！</span><span class="sxs-lookup"><span data-stu-id="2f851-188">Since multiple anchors are supported by the helper class, we can place as many cubes as we want to use this code sample!</span></span>

> [!NOTE]
> <span data-ttu-id="2f851-189">錨點的識別碼是您在應用程式中控制的內容。</span><span class="sxs-lookup"><span data-stu-id="2f851-189">The IDs for anchors are something you control in your app.</span></span> <span data-ttu-id="2f851-190">在此範例中，我們建立了一個根據目前儲存在應用程式錨點集合中之錨點數目的命名配置。</span><span class="sxs-lookup"><span data-stu-id="2f851-190">In this example, we have created a naming scheme that is sequential based on the number of anchors currently stored in the app's collection of anchors.</span></span>

```
   // Check for new input state since the last frame.
   SpatialInteractionSourceState^ pointerState = m_spatialInputHandler->CheckForInput();
   if (pointerState != nullptr)
   {
       // Try to get the pointer pose relative to the SpatialStationaryReferenceFrame.
       SpatialPointerPose^ pointerPose = pointerState->TryGetPointerPose(currentCoordinateSystem);
       if (pointerPose != nullptr)
       {
           // When a Pressed gesture is detected, the anchor will be created two meters in front of the user.

           // Get the gaze direction relative to the given coordinate system.
           const float3 headPosition = pointerPose->Head->Position;
           const float3 headDirection = pointerPose->Head->ForwardDirection;

           // The anchor position in the StationaryReferenceFrame.
           static const float distanceFromUser = 2.0f; // meters
           const float3 gazeAtTwoMeters = headPosition + (distanceFromUser * headDirection);

           // Create the anchor at position.
           SpatialAnchor^ anchor = SpatialAnchor::TryCreateRelativeTo(currentCoordinateSystem, gazeAtTwoMeters);

           if ((anchor != nullptr) && (m_spatialAnchorHelper != nullptr))
           {
               // In this example, we store the anchor in an IMap.
               auto anchorMap = m_spatialAnchorHelper->GetAnchorMap();

               // Create an identifier for the anchor.
               String^ id = ref new String(L"HolographicSpatialAnchorStoreSample_Anchor") + anchorMap->Size;

               anchorMap->Insert(id->ToString(), anchor);
           }
       }
   }
```

### <a name="asynchronously-load-and-cache-the-spatialanchorstore"></a><span data-ttu-id="2f851-191">以非同步方式載入和快取 SpatialAnchorStore</span><span class="sxs-lookup"><span data-stu-id="2f851-191">Asynchronously load, and cache, the SpatialAnchorStore</span></span>

<span data-ttu-id="2f851-192">讓我們來看看如何撰寫有助於處理這種持續性的 SampleSpatialAnchorHelper 類別，包括：</span><span class="sxs-lookup"><span data-stu-id="2f851-192">Let's see how to write a SampleSpatialAnchorHelper class that helps handle this persistence, including:</span></span>
* <span data-ttu-id="2f851-193">儲存記憶體中錨點的集合，由 Platform：： String 索引鍵編制索引。</span><span class="sxs-lookup"><span data-stu-id="2f851-193">Storing a collection of in-memory anchors, indexed by a Platform::String key.</span></span>
* <span data-ttu-id="2f851-194">從系統的 SpatialAnchorStore 載入錨點，它會與本機記憶體內部集合分開。</span><span class="sxs-lookup"><span data-stu-id="2f851-194">Loading anchors from the system's SpatialAnchorStore, which is kept separate from the local in-memory collection.</span></span>
* <span data-ttu-id="2f851-195">當應用程式選擇這麼做時，儲存錨點至 SpatialAnchorStore 的本機記憶體內部集合。</span><span class="sxs-lookup"><span data-stu-id="2f851-195">Saving the local in-memory collection of anchors to the SpatialAnchorStore when the app chooses to do so.</span></span>

<span data-ttu-id="2f851-196">以下說明如何將 [SpatialAnchor](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchor.aspx) 物件儲存在 [SpatialAnchorStore](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchorstore.aspx)中。</span><span class="sxs-lookup"><span data-stu-id="2f851-196">Here's how to save [SpatialAnchor](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchor.aspx) objects in the [SpatialAnchorStore](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchorstore.aspx).</span></span>

<span data-ttu-id="2f851-197">當類別啟動時，我們會以非同步方式要求 SpatialAnchorStore。</span><span class="sxs-lookup"><span data-stu-id="2f851-197">When the class starts up, we request the SpatialAnchorStore asynchronously.</span></span> <span data-ttu-id="2f851-198">這牽涉到在 API 載入錨點存放區時的系統 i/o，而且此 API 是非同步，因此 i/o 會是非封鎖的。</span><span class="sxs-lookup"><span data-stu-id="2f851-198">This involves system I/O as the API loads the anchor store, and this API is made asynchronous so that the I/O is non-blocking.</span></span>

```
   // Request the spatial anchor store, which is the WinRT object that will accept the imported anchor data.
   return create_task(SpatialAnchorManager::RequestStoreAsync())
       .then([](task<SpatialAnchorStore^> previousTask)
   {
       std::shared_ptr<SampleSpatialAnchorHelper> newHelper = nullptr;

       try
       {
           SpatialAnchorStore^ anchorStore = previousTask.get();

           // Once the SpatialAnchorStore has been loaded by the system, we can create our helper class.

           // Using "new" to access private constructor
           newHelper = std::shared_ptr<SampleSpatialAnchorHelper>(new SampleSpatialAnchorHelper(anchorStore));

           // Now we can load anchors from the store.
           newHelper->LoadFromAnchorStore();
       }
       catch (Exception^ exception)
       {
           PrintWstringToDebugConsole(
               std::wstring(L"Exception while loading the anchor store: ") +
               exception->Message->Data() +
               L"\n"
               );
       }

       // Return the initialized class instance.
       return newHelper;
   });
```

<span data-ttu-id="2f851-199">您將會獲得 SpatialAnchorStore，可讓您用來儲存錨點。</span><span class="sxs-lookup"><span data-stu-id="2f851-199">You'll be given a SpatialAnchorStore that you can use to save the anchors.</span></span> <span data-ttu-id="2f851-200">這是 IMapView，它會將字串的索引鍵值與 SpatialAnchors 的資料值產生關聯。</span><span class="sxs-lookup"><span data-stu-id="2f851-200">This is an IMapView that associates key values that are Strings, with data values that are SpatialAnchors.</span></span> <span data-ttu-id="2f851-201">在我們的範例程式碼中，我們會將它儲存在私用類別成員變數中，此變數可透過協助程式類別的公用函數來存取。</span><span class="sxs-lookup"><span data-stu-id="2f851-201">In our sample code, we store this in a private class member variable that is accessible through a public function of our helper class.</span></span>

```
   SampleSpatialAnchorHelper::SampleSpatialAnchorHelper(SpatialAnchorStore^ anchorStore)
   {
       m_anchorStore = anchorStore;
       m_anchorMap = ref new Platform::Collections::Map<String^, SpatialAnchor^>();
   }
```

>[!NOTE]
><span data-ttu-id="2f851-202">別忘了連接暫停/繼續事件，儲存並載入錨點存放區。</span><span class="sxs-lookup"><span data-stu-id="2f851-202">Don't forget to hook up the suspend/resume events to save and load the anchor store.</span></span>

```
   void HolographicSpatialAnchorStoreSampleMain::SaveAppState()
   {
       // For example, store information in the SpatialAnchorStore.
       if (m_spatialAnchorHelper != nullptr)
       {
           m_spatialAnchorHelper->TrySaveToAnchorStore();
       }
   }
```

```
   void HolographicSpatialAnchorStoreSampleMain::LoadAppState()
   {
       // For example, load information from the SpatialAnchorStore.
       LoadAnchorStore();
   }
```

### <a name="save-content-to-the-anchor-store"></a><span data-ttu-id="2f851-203">將內容儲存至錨點存放區</span><span class="sxs-lookup"><span data-stu-id="2f851-203">Save content to the anchor store</span></span>

<span data-ttu-id="2f851-204">當系統暫停您的應用程式時，您需要將空間錨點儲存至錨點存放區。</span><span class="sxs-lookup"><span data-stu-id="2f851-204">When the system suspends your app, you need to save your spatial anchors to the anchor store.</span></span> <span data-ttu-id="2f851-205">您也可以選擇在其他時間將錨點儲存至錨點存放區，因為您發現應用程式的執行需要。</span><span class="sxs-lookup"><span data-stu-id="2f851-205">You may also choose to save anchors to the anchor store at other times, as you find to be necessary for your app's implementation.</span></span>

<span data-ttu-id="2f851-206">當您準備好要嘗試將記憶體中的錨點儲存至 SpatialAnchorStore 時，您可以在集合中迴圈，並嘗試儲存每一個。</span><span class="sxs-lookup"><span data-stu-id="2f851-206">When you're ready to try saving the in-memory anchors to the SpatialAnchorStore, you can loop through your collection and try to save each one.</span></span>

```
   // TrySaveToAnchorStore: Stores all anchors from memory into the app's anchor store.
   //
   // For each anchor in memory, this function tries to store it in the app's AnchorStore. The operation will fail if
   // the anchor store already has an anchor by that name.
   //
   bool SampleSpatialAnchorHelper::TrySaveToAnchorStore()
   {
       // This function returns true if all the anchors in the in-memory collection are saved to the anchor
       // store. If zero anchors are in the in-memory collection, we will still return true because the
       // condition has been met.
       bool success = true;

       // If access is denied, 'anchorStore' will not be obtained.
       if (m_anchorStore != nullptr)
       {
           for each (auto& pair in m_anchorMap)
           {
               auto const& id = pair->Key;
               auto const& anchor = pair->Value;

               // Try to save the anchors.
               if (!m_anchorStore->TrySave(id, anchor))
               {
                   // This may indicate the anchor ID is taken, or the anchor limit is reached for the app.
                   success=false;
               }
           }
       }

       return success;
   }
```

### <a name="load-content-from-the-anchor-store-when-the-app-resumes"></a><span data-ttu-id="2f851-207">在應用程式繼續時從錨點存放區載入內容</span><span class="sxs-lookup"><span data-stu-id="2f851-207">Load content from the anchor store when the app resumes</span></span>

<span data-ttu-id="2f851-208">您可以還原 AnchorStore 中已儲存的錨點，方法是在您的應用程式繼續或任何時候，將它們從錨定存放區的 IMapView 傳送到您自己的記憶體中 SpatialAnchors 資料庫。</span><span class="sxs-lookup"><span data-stu-id="2f851-208">You can restore saved anchors in the AnchorStore by transferring them from the anchor store's IMapView to your own in-memory database of SpatialAnchors when your app resumes or at any time.</span></span>

<span data-ttu-id="2f851-209">若要從 SpatialAnchorStore 還原錨點，請將您感興趣的每一個錨點還原到您自己的記憶體中集合。</span><span class="sxs-lookup"><span data-stu-id="2f851-209">To restore anchors from the SpatialAnchorStore, restore each one that you're interested in to your own in-memory collection.</span></span>

<span data-ttu-id="2f851-210">您需要 SpatialAnchors 的記憶體內部資料庫，以將字串與您建立的 SpatialAnchors 產生關聯。</span><span class="sxs-lookup"><span data-stu-id="2f851-210">You need your own in-memory database of SpatialAnchors to associate Strings with the SpatialAnchors that you create.</span></span> <span data-ttu-id="2f851-211">在我們的範例程式碼中，我們選擇使用 Windows：： Foundation：： collection：： IMap 來儲存錨點，這樣可讓您輕鬆地針對 SpatialAnchorStore 使用相同的索引鍵和資料值。</span><span class="sxs-lookup"><span data-stu-id="2f851-211">In our sample code, we choose to use a Windows::Foundation::Collections::IMap to store the anchors, which makes it easy to use the same key and data value for the SpatialAnchorStore.</span></span>

```
   // This is an in-memory anchor list that is separate from the anchor store.
   // These anchors may be used, reasoned about, and so on before committing the collection to the store.
   Windows::Foundation::Collections::IMap<Platform::String^, Windows::Perception::Spatial::SpatialAnchor^>^ m_anchorMap;
```

>[!NOTE]
><span data-ttu-id="2f851-212">可能無法立即找出已還原的錨點。</span><span class="sxs-lookup"><span data-stu-id="2f851-212">An anchor that is restored might not be locatable right away.</span></span> <span data-ttu-id="2f851-213">例如，它可能是個別房間或不同大樓中的錨點。</span><span class="sxs-lookup"><span data-stu-id="2f851-213">For example, it might be an anchor in a separate room or in a different building altogether.</span></span> <span data-ttu-id="2f851-214">從 AnchorStore 取出的錨點應該先經過 locatability 測試，再加以使用。</span><span class="sxs-lookup"><span data-stu-id="2f851-214">Anchors retrieved from the AnchorStore should be tested for locatability before using them.</span></span>

<br>

>[!NOTE]
><span data-ttu-id="2f851-215">在此範例程式碼中，我們會從 AnchorStore 中取出所有錨點。</span><span class="sxs-lookup"><span data-stu-id="2f851-215">In this example code, we retrieve all anchors from the AnchorStore.</span></span> <span data-ttu-id="2f851-216">這不是必要條件;您的應用程式也可以選擇，並使用對您的實有意義的字串索引鍵值，選擇特定的錨點子集。</span><span class="sxs-lookup"><span data-stu-id="2f851-216">This is not a requirement; your app could just as well pick and choose a certain subset of anchors by using String key values that are meaningful to your implementation.</span></span>

```
   // LoadFromAnchorStore: Loads all anchors from the app's anchor store into memory.
   //
   // The anchors are stored in memory using an IMap, which stores anchors using a string identifier. Any string can be used as
   // the identifier; it can have meaning to the app, such as "Game_Leve1_CouchAnchor," or it can be a GUID that is generated
   // by the app.
   //
   void SampleSpatialAnchorHelper::LoadFromAnchorStore()
   {
       // If access is denied, 'anchorStore' will not be obtained.
       if (m_anchorStore != nullptr)
       {
           // Get all saved anchors.
           auto anchorMapView = m_anchorStore->GetAllSavedAnchors();
           for each (auto const& pair in anchorMapView)
           {
               auto const& id = pair->Key;
               auto const& anchor = pair->Value;
               m_anchorMap->Insert(id, anchor);
           }
       }
   }
```

### <a name="clear-the-anchor-store-when-needed"></a><span data-ttu-id="2f851-217">視需要清除錨定存放區</span><span class="sxs-lookup"><span data-stu-id="2f851-217">Clear the anchor store, when needed</span></span>

<span data-ttu-id="2f851-218">有時，您需要清除應用程式狀態並寫入新的資料。</span><span class="sxs-lookup"><span data-stu-id="2f851-218">Sometimes, you need to clear app state and write new data.</span></span> <span data-ttu-id="2f851-219">以下是使用 [SpatialAnchorStore](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchorstore.aspx)的方式。</span><span class="sxs-lookup"><span data-stu-id="2f851-219">Here's how you do that with the [SpatialAnchorStore](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchorstore.aspx).</span></span>

<span data-ttu-id="2f851-220">使用我們的 helper 類別，幾乎不需要包裝 Clear 函數。</span><span class="sxs-lookup"><span data-stu-id="2f851-220">Using our helper class, it's almost unnecessary to wrap the Clear function.</span></span> <span data-ttu-id="2f851-221">我們選擇在我們的範例執行中這麼做，因為我們的協助程式類別有權擁有 SpatialAnchorStore 實例。</span><span class="sxs-lookup"><span data-stu-id="2f851-221">We choose to do so in our sample implementation, because our helper class is given the responsibility of owning the SpatialAnchorStore instance.</span></span>

```
   // ClearAnchorStore: Clears the AnchorStore for the app.
   //
   // This function clears the AnchorStore. It has no effect on the anchors stored in memory.
   //
   void SampleSpatialAnchorHelper::ClearAnchorStore()
   {
       // If access is denied, 'anchorStore' will not be obtained.
       if (m_anchorStore != nullptr)
       {
           // Clear all anchors from the store.
           m_anchorStore->Clear();
       }
   }
```

### <a name="example-relating-anchor-coordinate-systems-to-stationary-reference-frame-coordinate-systems"></a><span data-ttu-id="2f851-222">範例：將錨點座標系統與固定參考框架座標系統相關聯</span><span class="sxs-lookup"><span data-stu-id="2f851-222">Example: Relating anchor coordinate systems to stationary reference frame coordinate systems</span></span>

<span data-ttu-id="2f851-223">假設您有一個錨點，而且您想要讓錨點座標系統中的某個東西與您已用於其他內容的 SpatialStationaryReferenceFrame 產生關聯。</span><span class="sxs-lookup"><span data-stu-id="2f851-223">Let's say you have an anchor, and you want to relate something in your anchor's coordinate system to the SpatialStationaryReferenceFrame you’re already using for your other content.</span></span> <span data-ttu-id="2f851-224">您可以使用 [TryGetTransformTo](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialcoordinatesystem.trygettransformto.aspx) 從錨點的座標系統取得轉換，使其成為固定參考框架的轉換：</span><span class="sxs-lookup"><span data-stu-id="2f851-224">You can use [TryGetTransformTo](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialcoordinatesystem.trygettransformto.aspx) to get a transform from the anchor’s coordinate system to that of the stationary reference frame:</span></span>

```
   // In this code snippet, someAnchor is a SpatialAnchor^ that has been initialized and is valid in the current environment.
   float4x4 anchorSpaceToCurrentCoordinateSystem;
   SpatialCoordinateSystem^ anchorSpace = someAnchor->CoordinateSystem;
   const auto tryTransform = anchorSpace->TryGetTransformTo(currentCoordinateSystem);
   if (tryTransform != nullptr)
   {
       anchorSpaceToCurrentCoordinateSystem = tryTransform->Value;
   }
```

<span data-ttu-id="2f851-225">此程式對於您有兩種方式很有用：</span><span class="sxs-lookup"><span data-stu-id="2f851-225">This process is useful to you in two ways:</span></span>
1. <span data-ttu-id="2f851-226">它會告訴您這兩個參考框架是否可以彼此相對地理解，以及。</span><span class="sxs-lookup"><span data-stu-id="2f851-226">It tells you if the two reference frames can be understood relative to one another, and;</span></span>
2. <span data-ttu-id="2f851-227">如果是的話，它會提供您直接從某個座標系統移至另一個座標系統的轉換。</span><span class="sxs-lookup"><span data-stu-id="2f851-227">If so, it provides you a transform to go directly from one coordinate system to the other.</span></span>

<span data-ttu-id="2f851-228">有了這項資訊，您就能瞭解兩個參考框架之間的物件之間的空間關聯性。</span><span class="sxs-lookup"><span data-stu-id="2f851-228">With this information, you have an understanding of the spatial relation between objects between the two reference frames.</span></span>

<span data-ttu-id="2f851-229">針對轉譯，您通常可以根據物件的原始參考框架或錨點來群組物件，以取得更好的結果。</span><span class="sxs-lookup"><span data-stu-id="2f851-229">For rendering, you can often obtain better results by grouping objects according to their original reference frame or anchor.</span></span> <span data-ttu-id="2f851-230">針對每個群組執行個別的繪圖階段。</span><span class="sxs-lookup"><span data-stu-id="2f851-230">Perform a separate drawing pass for each group.</span></span> <span data-ttu-id="2f851-231">使用相同座標系統初始建立之模型轉換的物件，視圖矩陣更準確。</span><span class="sxs-lookup"><span data-stu-id="2f851-231">The view matrices are more accurate for objects with model transforms that are created initially using the same coordinate system.</span></span>

## <a name="create-holograms-using-a-device-attached-frame-of-reference"></a><span data-ttu-id="2f851-232">使用裝置附加的參考框架建立全像影像</span><span class="sxs-lookup"><span data-stu-id="2f851-232">Create holograms using a device-attached frame of reference</span></span>

<span data-ttu-id="2f851-233">有時候您會想要轉譯一台 [保持連接](../../design/coordinate-systems.md#attached-frame-of-reference) 至裝置位置的全息圖，例如，包含偵錯工具資訊的面板，或是當裝置只能判斷其方向，而不是在空間中的位置時的參考訊息。</span><span class="sxs-lookup"><span data-stu-id="2f851-233">There are times when you want to render a hologram that [remains attached](../../design/coordinate-systems.md#attached-frame-of-reference) to the device's location, for example a panel with debugging information or an informational message when the device is only able to determine its orientation and not its position in space.</span></span> <span data-ttu-id="2f851-234">為了達成此目的，我們使用了附加的參考框架。</span><span class="sxs-lookup"><span data-stu-id="2f851-234">To accomplish this, we use an attached frame of reference.</span></span>

<span data-ttu-id="2f851-235">SpatialLocatorAttachedFrameOfReference 類別會定義座標系統（相對於裝置），而不是真實世界。</span><span class="sxs-lookup"><span data-stu-id="2f851-235">The SpatialLocatorAttachedFrameOfReference class defines coordinate systems, which are relative to the device rather than to the real-world.</span></span> <span data-ttu-id="2f851-236">這個框架有一個固定的標題，相對於使用者的周圍面，指向建立參考框架時使用者面對的方向。</span><span class="sxs-lookup"><span data-stu-id="2f851-236">This frame has a fixed heading relative to the user's surroundings that points in the direction the user was facing when the reference frame was created.</span></span> <span data-ttu-id="2f851-237">然後，此參考框架中的所有方向都會相對於該固定標題，即使使用者旋轉裝置也是一樣。</span><span class="sxs-lookup"><span data-stu-id="2f851-237">From then on, all orientations in this frame of reference are relative to that fixed heading, even as the user rotates the device.</span></span>

<span data-ttu-id="2f851-238">就 HoloLens 而言，此框架座標系統的原點是位於使用者頭部的旋轉中心，因此其位置不會受到頭部旋轉的影響。</span><span class="sxs-lookup"><span data-stu-id="2f851-238">For HoloLens, the origin of this frame's coordinate system is located at the center of rotation of the user's head, so that its position is not affected by head rotation.</span></span> <span data-ttu-id="2f851-239">您的應用程式可以指定相對於這個點的位移，以在使用者之前放置全像影像。</span><span class="sxs-lookup"><span data-stu-id="2f851-239">Your app can specify an offset relative to this point to position holograms in front of the user.</span></span>

<span data-ttu-id="2f851-240">若要取得 SpatialLocatorAttachedFrameOfReference，請使用 SpatialLocator 類別並呼叫 CreateAttachedFrameOfReferenceAtCurrentHeading。</span><span class="sxs-lookup"><span data-stu-id="2f851-240">To get a SpatialLocatorAttachedFrameOfReference, use the SpatialLocator class and call CreateAttachedFrameOfReferenceAtCurrentHeading.</span></span>

<span data-ttu-id="2f851-241">這適用于整個 Windows Mixed Reality 裝置的範圍。</span><span class="sxs-lookup"><span data-stu-id="2f851-241">This applies to the entire range of Windows Mixed Reality devices.</span></span>

### <a name="use-a-reference-frame-attached-to-the-device"></a><span data-ttu-id="2f851-242">使用附加至裝置的參考框架</span><span class="sxs-lookup"><span data-stu-id="2f851-242">Use a reference frame attached to the device</span></span>

<span data-ttu-id="2f851-243">這些章節會討論在 Windows 全像應用程式範本中所做的變更，以使用此 API 啟用裝置連結的參考框架。</span><span class="sxs-lookup"><span data-stu-id="2f851-243">These sections talk about what we changed in the Windows Holographic app template to enable a device-attached frame of reference using this API.</span></span> <span data-ttu-id="2f851-244">這種「附加」的全像是固定或錨定的全像投影，也可以在裝置暫時無法在世界中尋找其位置時使用。</span><span class="sxs-lookup"><span data-stu-id="2f851-244">This "attached" hologram will work alongside stationary or anchored holograms, and may also be used when the device is temporarily unable to find its position in the world.</span></span>

<span data-ttu-id="2f851-245">首先，我們變更了範本來儲存 SpatialLocatorAttachedFrameOfReference，而不是 SpatialStationaryFrameOfReference：</span><span class="sxs-lookup"><span data-stu-id="2f851-245">First, we changed the template to store a SpatialLocatorAttachedFrameOfReference instead of a SpatialStationaryFrameOfReference:</span></span>

<span data-ttu-id="2f851-246">從 **HolographicTagAlongSampleMain .h**：</span><span class="sxs-lookup"><span data-stu-id="2f851-246">From **HolographicTagAlongSampleMain.h**:</span></span>

```
   // A reference frame attached to the holographic camera.
   Windows::Perception::Spatial::SpatialLocatorAttachedFrameOfReference^   m_referenceFrame;
```

<span data-ttu-id="2f851-247">從 **HolographicTagAlongSampleMain .cpp**：</span><span class="sxs-lookup"><span data-stu-id="2f851-247">From **HolographicTagAlongSampleMain.cpp**:</span></span>

```
   // In this example, we create a reference frame attached to the device.
   m_referenceFrame = m_locator->CreateAttachedFrameOfReferenceAtCurrentHeading();
```

<span data-ttu-id="2f851-248">在更新期間，我們現在會在從幀預測取得的時間戳記上取得座標系統。</span><span class="sxs-lookup"><span data-stu-id="2f851-248">During the update, we now obtain the coordinate system at the time stamp obtained from with the frame prediction.</span></span>

```
   // Next, we get a coordinate system from the attached frame of reference that is
   // associated with the current frame. Later, this coordinate system is used for
   // for creating the stereo view matrices when rendering the sample content.
   SpatialCoordinateSystem^ currentCoordinateSystem =
       m_referenceFrame->GetStationaryCoordinateSystemAtTimestamp(prediction->Timestamp);
```

### <a name="get-a-spatial-pointer-pose-and-follow-the-users-gaze"></a><span data-ttu-id="2f851-249">取得空間指標姿勢，並遵循使用者的注視</span><span class="sxs-lookup"><span data-stu-id="2f851-249">Get a spatial pointer pose, and follow the user's Gaze</span></span>

<span data-ttu-id="2f851-250">我們想要讓範例全息圖遵循使用者的 [注視](../../design/gaze-and-commit.md)，類似于全像全像全像全像這樣的影像，</span><span class="sxs-lookup"><span data-stu-id="2f851-250">We want our example hologram to follow the user's [gaze](../../design/gaze-and-commit.md), similar to how the holographic shell can follow the user's gaze.</span></span> <span data-ttu-id="2f851-251">為此，我們必須從相同的時間戳記取得 SpatialPointerPose。</span><span class="sxs-lookup"><span data-stu-id="2f851-251">For this, we need to get the SpatialPointerPose from the same time stamp.</span></span>

```
SpatialPointerPose^ pose = SpatialPointerPose::TryGetAtTimestamp(currentCoordinateSystem, prediction->Timestamp);
```

<span data-ttu-id="2f851-252">這個 SpatialPointerPose 具有根據 [使用者目前的標題](gaze-in-directx.md)放置全息圖所需的資訊。</span><span class="sxs-lookup"><span data-stu-id="2f851-252">This SpatialPointerPose has the information needed to position the hologram according to the [user's current heading](gaze-in-directx.md).</span></span>

<span data-ttu-id="2f851-253">為了讓使用者更容易使用，我們使用線性插補 ( "lerp" ) 在一段時間內平滑變更的位置。</span><span class="sxs-lookup"><span data-stu-id="2f851-253">For user comfort, we use linear interpolation ("lerp") to smooth the change in position over a period of time.</span></span> <span data-ttu-id="2f851-254">這對使用者而言更適合用來鎖定全像看的全像影像。</span><span class="sxs-lookup"><span data-stu-id="2f851-254">This is more comfortable for the user than locking the hologram to their gaze.</span></span> <span data-ttu-id="2f851-255">Lerping 加上標籤的影像位置也可讓我們藉由抑制移動來使全像影像穩定。</span><span class="sxs-lookup"><span data-stu-id="2f851-255">Lerping the tag-along hologram's position also allows us to stabilize the hologram by dampening the movement.</span></span> <span data-ttu-id="2f851-256">如果未進行這項抑制，使用者會看到全息圖抖動，因為通常會視為使用者的 imperceptible 移動。</span><span class="sxs-lookup"><span data-stu-id="2f851-256">If we didn't do this dampening, the user would see the hologram jitter because of what are normally considered to be imperceptible movements of the user's head.</span></span>

<span data-ttu-id="2f851-257">從 **StationaryQuadRenderer：:P ositionhologram**：</span><span class="sxs-lookup"><span data-stu-id="2f851-257">From **StationaryQuadRenderer::PositionHologram**:</span></span>

```
   const float& dtime = static_cast<float>(timer.GetElapsedSeconds());

   if (pointerPose != nullptr)
   {
       // Get the gaze direction relative to the given coordinate system.
       const float3 headPosition  = pointerPose->Head->Position;
       const float3 headDirection = pointerPose->Head->ForwardDirection;

       // The tag-along hologram follows a point 2.0m in front of the user's gaze direction.
       static const float distanceFromUser = 2.0f; // meters
       const float3 gazeAtTwoMeters = headPosition + (distanceFromUser * headDirection);

       // Lerp the position, to keep the hologram comfortably stable.
       auto lerpedPosition = lerp(m_position, gazeAtTwoMeters, dtime * c_lerpRate);

       // This will be used as the translation component of the hologram's
       // model transform.
       SetPosition(lerpedPosition);
   }
```

>[!NOTE]
><span data-ttu-id="2f851-258">在 [偵錯工具] 面板的案例中，您可以選擇將全息圖重新放置到一小部分，使其不會對您的觀點。</span><span class="sxs-lookup"><span data-stu-id="2f851-258">In the case of a debugging panel, you might choose to reposition the hologram off to the side a little so that it doesn't obstruct your view.</span></span> <span data-ttu-id="2f851-259">以下是您可能會這麼做的範例。</span><span class="sxs-lookup"><span data-stu-id="2f851-259">Here's an example of how you might do that.</span></span>

<span data-ttu-id="2f851-260">若為 **StationaryQuadRenderer：:P ositionhologram**：</span><span class="sxs-lookup"><span data-stu-id="2f851-260">For **StationaryQuadRenderer::PositionHologram**:</span></span>

```
       // If you're making a debug view, you might not want the tag-along to be directly in the
       // center of your field of view. Use this code to position the hologram to the right of
       // the user's gaze direction.
       /*
       const float3 offset = float3(0.13f, 0.0f, 0.f);
       static const float distanceFromUser = 2.2f; // meters
       const float3 gazeAtTwoMeters = headPosition + (distanceFromUser * (headDirection + offset));
       */
```

### <a name="rotate-the-hologram-to-face-the-camera"></a><span data-ttu-id="2f851-261">旋轉全像相機的影像</span><span class="sxs-lookup"><span data-stu-id="2f851-261">Rotate the hologram to face the camera</span></span>

<span data-ttu-id="2f851-262">它並不足以放置全息圖，在此案例中為四;我們也必須旋轉物件，以面對使用者的臉。</span><span class="sxs-lookup"><span data-stu-id="2f851-262">It isn't enough to position the hologram, which in this case is a quad; we must also rotate the object to face the user.</span></span> <span data-ttu-id="2f851-263">這種旋轉會在世界空間中發生，因為這種類型的 billboarding 可讓全像是使用者環境的一部分。</span><span class="sxs-lookup"><span data-stu-id="2f851-263">This rotation occurs in world space, because this type of billboarding allows the hologram to remain a part of the user's environment.</span></span> <span data-ttu-id="2f851-264">視圖空間 billboarding 不是很舒適，因為全像全像是顯示方向的全像影像一樣，在這種情況下，您也必須在左右的視圖矩陣之間插補，以取得不會中斷身歷聲轉譯的視圖空間佈告欄轉換。</span><span class="sxs-lookup"><span data-stu-id="2f851-264">View-space billboarding isn't as comfortable because the hologram becomes locked to the display orientation; in that case, you would also have to interpolate between the left and right view matrices to acquire a view-space billboard transform that doesn't disrupt stereo rendering.</span></span> <span data-ttu-id="2f851-265">在這裡，我們將旋轉 X 和 Z 軸，以面對使用者的臉。</span><span class="sxs-lookup"><span data-stu-id="2f851-265">Here, we rotate on the X and Z axes to face the user.</span></span>

<span data-ttu-id="2f851-266">從 **StationaryQuadRenderer：： Update**：</span><span class="sxs-lookup"><span data-stu-id="2f851-266">From **StationaryQuadRenderer::Update**:</span></span>

```
   // Seconds elapsed since previous frame.
   const float& dTime = static_cast<float>(timer.GetElapsedSeconds());

   // Create a direction normal from the hologram's position to the origin of person space.
   // This is the z-axis rotation.
   XMVECTOR facingNormal = XMVector3Normalize(-XMLoadFloat3(&m_position));

   // Rotate the x-axis around the y-axis.
   // This is a 90-degree angle from the normal, in the xz-plane.
   // This is the x-axis rotation.
   XMVECTOR xAxisRotation = XMVector3Normalize(XMVectorSet(XMVectorGetZ(facingNormal), 0.f, -XMVectorGetX(facingNormal), 0.f));

   // Create a third normal to satisfy the conditions of a rotation matrix.
   // The cross product  of the other two normals is at a 90-degree angle to
   // both normals. (Normalize the cross product to avoid floating-point math
   // errors.)
   // Note how the cross product will never be a zero-matrix because the two normals
   // are always at a 90-degree angle from one another.
   XMVECTOR yAxisRotation = XMVector3Normalize(XMVector3Cross(facingNormal, xAxisRotation));

   // Construct the 4x4 rotation matrix.

   // Rotate the quad to face the user.
   XMMATRIX rotationMatrix = XMMATRIX(
       xAxisRotation,
       yAxisRotation,
       facingNormal,
       XMVectorSet(0.f, 0.f, 0.f, 1.f)
       );

   // Position the quad.
   const XMMATRIX modelTranslation = XMMatrixTranslationFromVector(XMLoadFloat3(&m_position));

   // The view and projection matrices are provided by the system; they are associated
   // with holographic cameras, and updated on a per-camera basis.
   // Here, we provide the model transform for the sample hologram. The model transform
   // matrix is transposed to prepare it for the shader.
   XMStoreFloat4x4(&m_modelConstantBufferData.model, XMMatrixTranspose(rotationMatrix * modelTranslation));
```

### <a name="render-the-attached-hologram"></a><span data-ttu-id="2f851-267">轉譯連接的全像影像</span><span class="sxs-lookup"><span data-stu-id="2f851-267">Render the attached hologram</span></span>

<span data-ttu-id="2f851-268">在此範例中，我們也選擇在 SpatialLocatorAttachedReferenceFrame 的座標系統中轉譯全息圖，也就是我們放置全息圖的位置。</span><span class="sxs-lookup"><span data-stu-id="2f851-268">For this example, we also choose to render the hologram in the coordinate system of the SpatialLocatorAttachedReferenceFrame, which is where we positioned the hologram.</span></span> <span data-ttu-id="2f851-269"> (如果我們決定使用其他座標系統轉譯，則必須從裝置附加的參考框架座標系統取得轉換至該座標系統。 ) </span><span class="sxs-lookup"><span data-stu-id="2f851-269">(If we had decided to render using another coordinate system, we would need to acquire a transform from the device-attached reference frame's coordinate system to that coordinate system.)</span></span>

<span data-ttu-id="2f851-270">從 **HolographicTagAlongSampleMain：： Render**：</span><span class="sxs-lookup"><span data-stu-id="2f851-270">From **HolographicTagAlongSampleMain::Render**:</span></span>

```
   // The view and projection matrices for each holographic camera will change
   // every frame. This function refreshes the data in the constant buffer for
   // the holographic camera indicated by cameraPose.
   pCameraResources->UpdateViewProjectionBuffer(
       m_deviceResources,
       cameraPose,
       m_referenceFrame->GetStationaryCoordinateSystemAtTimestamp(prediction->Timestamp)
       );
```

<span data-ttu-id="2f851-271">這樣就完成了！</span><span class="sxs-lookup"><span data-stu-id="2f851-271">That's it!</span></span> <span data-ttu-id="2f851-272">現在，全像是在使用者的注視方向之前，將2個計量的位置「定位」。</span><span class="sxs-lookup"><span data-stu-id="2f851-272">The hologram will now "chase" a position that is 2 meters in front of the user's gaze direction.</span></span>

>[!NOTE]
><span data-ttu-id="2f851-273">此範例也會載入其他內容-請參閱 StationaryQuadRenderer .cpp。</span><span class="sxs-lookup"><span data-stu-id="2f851-273">This example also loads additional content - see StationaryQuadRenderer.cpp.</span></span>

## <a name="handling-tracking-loss"></a><span data-ttu-id="2f851-274">處理追蹤遺失</span><span class="sxs-lookup"><span data-stu-id="2f851-274">Handling tracking loss</span></span>

<span data-ttu-id="2f851-275">當裝置無法在世界中找到自己的裝置時，應用程式會遇到「追蹤遺失」。</span><span class="sxs-lookup"><span data-stu-id="2f851-275">When the device can't locate itself in the world, the app experiences "tracking loss".</span></span> <span data-ttu-id="2f851-276">Windows Mixed Reality 的應用程式應該能夠處理位置追蹤系統的這類中斷。</span><span class="sxs-lookup"><span data-stu-id="2f851-276">Windows Mixed Reality apps should be able to handle such disruptions to the positional tracking system.</span></span> <span data-ttu-id="2f851-277">您可以在預設 SpatialLocator 上使用 LocatabilityChanged 事件，以觀察到這些中斷和建立的回應。</span><span class="sxs-lookup"><span data-stu-id="2f851-277">These disruptions can be observed, and responses created, by using the LocatabilityChanged event on the default SpatialLocator.</span></span>

<span data-ttu-id="2f851-278">從 **AppMain：： SetHolographicSpace：**</span><span class="sxs-lookup"><span data-stu-id="2f851-278">From **AppMain::SetHolographicSpace:**</span></span>

```
   // Be able to respond to changes in the positional tracking state.
   m_locatabilityChangedToken =
       m_locator->LocatabilityChanged +=
           ref new Windows::Foundation::TypedEventHandler<SpatialLocator^, Object^>(
               std::bind(&HolographicApp1Main::OnLocatabilityChanged, this, _1, _2)
               );
```

<span data-ttu-id="2f851-279">當您的應用程式收到 LocatabilityChanged 事件時，它可以視需要變更行為。</span><span class="sxs-lookup"><span data-stu-id="2f851-279">When your app receives a LocatabilityChanged event, it can change behavior as needed.</span></span> <span data-ttu-id="2f851-280">例如，在 PositionalTrackingInhibited 狀態下，您的應用程式可以暫停正常作業，並轉譯顯示警告訊息的 [標記沿著全像影像](coordinate-systems-in-directx.md#create-holograms-using-a-device-attached-frame-of-reference) 。</span><span class="sxs-lookup"><span data-stu-id="2f851-280">For example, in the PositionalTrackingInhibited state, your app can pause normal operation and render a [tag-along hologram](coordinate-systems-in-directx.md#create-holograms-using-a-device-attached-frame-of-reference) that displays a warning message.</span></span>

<span data-ttu-id="2f851-281">Windows 全像應用程式範本隨附已為您建立的 LocatabilityChanged 處理常式。</span><span class="sxs-lookup"><span data-stu-id="2f851-281">The Windows Holographic app template comes with a LocatabilityChanged handler already created for you.</span></span> <span data-ttu-id="2f851-282">根據預設，當位置追蹤無法使用時，它會在 debug 主控台中顯示警告。</span><span class="sxs-lookup"><span data-stu-id="2f851-282">By default, it displays a warning in the debug console when positional tracking is unavailable.</span></span> <span data-ttu-id="2f851-283">您可以將程式碼加入這個處理常式，以視需要提供應用程式的回應。</span><span class="sxs-lookup"><span data-stu-id="2f851-283">You can add code to this handler to provide a response as needed from your app.</span></span>

<span data-ttu-id="2f851-284">從 **AppMain .cpp：**</span><span class="sxs-lookup"><span data-stu-id="2f851-284">From **AppMain.cpp:**</span></span>

```
   void HolographicApp1Main::OnLocatabilityChanged(SpatialLocator^ sender, Object^ args)
   {
       switch (sender->Locatability)
       {
       case SpatialLocatability::Unavailable:
           // Holograms cannot be rendered.
           {
               String^ message = L"Warning! Positional tracking is " +
                                           sender->Locatability.ToString() + L".\n";
               OutputDebugStringW(message->Data());
           }
           break;

       // In the following three cases, it is still possible to place holograms using a
       // SpatialLocatorAttachedFrameOfReference.
       case SpatialLocatability::PositionalTrackingActivating:
           // The system is preparing to use positional tracking.

       case SpatialLocatability::OrientationOnly:
           // Positional tracking has not been activated.

       case SpatialLocatability::PositionalTrackingInhibited:
           // Positional tracking is temporarily inhibited. User action may be required
           // in order to restore positional tracking.
           break;

       case SpatialLocatability::PositionalTrackingActive:
           // Positional tracking is active. World-locked content can be rendered.
           break;
       }
   }
```

## <a name="spatial-mapping"></a><span data-ttu-id="2f851-285">空間對應</span><span class="sxs-lookup"><span data-stu-id="2f851-285">Spatial mapping</span></span>

<span data-ttu-id="2f851-286">[空間對應](spatial-mapping-in-directx.md)api 會利用座標系統來取得 surface 網格的模型轉換。</span><span class="sxs-lookup"><span data-stu-id="2f851-286">The [spatial mapping](spatial-mapping-in-directx.md) APIs make use of coordinate systems to get model transforms for surface meshes.</span></span>

## <a name="see-also"></a><span data-ttu-id="2f851-287">另請參閱</span><span class="sxs-lookup"><span data-stu-id="2f851-287">See also</span></span>
* [<span data-ttu-id="2f851-288">座標系統</span><span class="sxs-lookup"><span data-stu-id="2f851-288">Coordinate systems</span></span>](../../design/coordinate-systems.md)
* [<span data-ttu-id="2f851-289">空間錨點</span><span class="sxs-lookup"><span data-stu-id="2f851-289">Spatial anchors</span></span>](../../design/spatial-anchors.md)
* <span data-ttu-id="2f851-290"><a href="https://docs.microsoft.com/azure/spatial-anchors" target="_blank">Azure Spatial Anchors</a></span><span class="sxs-lookup"><span data-stu-id="2f851-290"><a href="https://docs.microsoft.com/azure/spatial-anchors" target="_blank">Azure Spatial Anchors</a></span></span>
* [<span data-ttu-id="2f851-291">DirectX 中的頭部和眼睛目光</span><span class="sxs-lookup"><span data-stu-id="2f851-291">Head and eye gaze in DirectX</span></span>](gaze-in-directx.md)
* [<span data-ttu-id="2f851-292">DirectX 中的手部和運動控制器</span><span class="sxs-lookup"><span data-stu-id="2f851-292">Hands and motion controllers in DirectX</span></span>](hands-and-motion-controllers-in-directx.md)
* [<span data-ttu-id="2f851-293">DirectX 中的空間對應</span><span class="sxs-lookup"><span data-stu-id="2f851-293">Spatial mapping in DirectX</span></span>](spatial-mapping-in-directx.md)
