---
title: 磁碟區呈現
description: 體積型影像包含豐富的資訊，在整個磁片區中都不會有不透明度和色彩，而無法輕易地表示為表面。 瞭解如何有效率地轉譯 Windows Mixed Reality 內的體積型影像。
author: kevinkennedy
ms.author: kkennedy
ms.date: 03/21/2018
ms.topic: article
keywords: 體積型映射，磁片區轉譯，效能，混合現實
ms.openlocfilehash: c0b68a2368823e5699e24d66bfafe1e4e05bdce8
ms.sourcegitcommit: 2bf79eef6a9b845494484f458443ef4f89d7efc0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/17/2020
ms.locfileid: "97612942"
---
# <a name="volume-rendering"></a><span data-ttu-id="9a75b-105">磁碟區呈現</span><span class="sxs-lookup"><span data-stu-id="9a75b-105">Volume rendering</span></span>

<span data-ttu-id="9a75b-106">若為醫療 MRI 或工程量，請參閱 [維琪百科上的磁片](https://en.wikipedia.org/wiki/Volume_rendering)區轉譯。</span><span class="sxs-lookup"><span data-stu-id="9a75b-106">For medical MRI or engineering volumes, see [Volume Rendering on Wikipedia](https://en.wikipedia.org/wiki/Volume_rendering).</span></span> <span data-ttu-id="9a75b-107">這些「體積型影像」包含豐富的資訊，在整個磁片區中都不會有不透明度和色彩，而無法輕易地表示為 [多邊形網格](https://en.wikipedia.org/wiki/Polygon_mesh)之類的表面。</span><span class="sxs-lookup"><span data-stu-id="9a75b-107">These 'volumetric images' contain rich information with opacity and color throughout the volume that cannot be easily expressed as surfaces such as [polygonal meshes](https://en.wikipedia.org/wiki/Polygon_mesh).</span></span>

<span data-ttu-id="9a75b-108">改善效能的關鍵解決方案</span><span class="sxs-lookup"><span data-stu-id="9a75b-108">Key solutions to improve performance</span></span>
1. <span data-ttu-id="9a75b-109">錯誤：不完整的方法：顯示整個磁片區，通常執行速度太慢</span><span class="sxs-lookup"><span data-stu-id="9a75b-109">BAD: Naïve Approach: Show Whole Volume, generally runs too slowly</span></span>
2. <span data-ttu-id="9a75b-110">良好：剪下平面：只顯示磁片區的單一配量</span><span class="sxs-lookup"><span data-stu-id="9a75b-110">GOOD: Cutting Plane: Show only a single slice of the volume</span></span>
3. <span data-ttu-id="9a75b-111">良好：剪下的子卷：只顯示磁片區的幾個圖層</span><span class="sxs-lookup"><span data-stu-id="9a75b-111">GOOD: Cutting Sub-Volume: Show only a few layers of the volume</span></span>
4. <span data-ttu-id="9a75b-112">良好：降低磁片區轉譯的解析度 (請參閱「混合解析度場景轉譯」 ) </span><span class="sxs-lookup"><span data-stu-id="9a75b-112">GOOD: Lower the resolution of the volume rendering (see 'Mixed Resolution Scene Rendering')</span></span>

<span data-ttu-id="9a75b-113">只有特定數量的資訊可以從應用程式傳輸到任何特定畫面格的螢幕，也就是總記憶體頻寬。</span><span class="sxs-lookup"><span data-stu-id="9a75b-113">There's only a certain amount of information that can be transferred from the application to the screen in any particular frame, which is the total memory bandwidth.</span></span> <span data-ttu-id="9a75b-114">此外，轉換資料以進行呈現所需的任何處理 (或「陰影」 ) 都需要時間。</span><span class="sxs-lookup"><span data-stu-id="9a75b-114">Also, any processing (or 'shading') required to transform that data for presentation requires time.</span></span> <span data-ttu-id="9a75b-115">進行磁片區轉譯時的主要考慮如下：</span><span class="sxs-lookup"><span data-stu-id="9a75b-115">The primary considerations when doing volume rendering are as such:</span></span>
* <span data-ttu-id="9a75b-116">Screen-Width \* Screen-Height \* Screen-Count \* 磁片區-每個畫面的單位-圖元 = 總數量-樣本數</span><span class="sxs-lookup"><span data-stu-id="9a75b-116">Screen-Width \* Screen-Height \* Screen-Count \* Volume-Layers-On-That-Pixel = Total-Volume-Samples-Per-Frame</span></span>
* <span data-ttu-id="9a75b-117">1028 \* 720 \* 2 \* 256 = 378961920 (100% )  (完整 res 磁片區：太多樣本) </span><span class="sxs-lookup"><span data-stu-id="9a75b-117">1028 \* 720 \* 2 \* 256 = 378961920 (100%) (full res volume: too many samples)</span></span>
* <span data-ttu-id="9a75b-118">1028 \* 720 \* 2 \* 1 = 1480320 (0.3% 的完整)  (精簡配量：每圖元一個樣本，可順暢地執行) </span><span class="sxs-lookup"><span data-stu-id="9a75b-118">1028 \* 720 \* 2 \* 1 = 1480320 (0.3% of full) (thin slice: 1 sample per pixel, runs smoothly)</span></span>
* <span data-ttu-id="9a75b-119">1028 \* 720 \* 2 \* 10 = 14803200 (3.9% 的完整)  (subvolume 配量：每圖元10個樣本、相當順暢地執行，並以3d 顯示) </span><span class="sxs-lookup"><span data-stu-id="9a75b-119">1028 \* 720 \* 2 \* 10 = 14803200 (3.9% of full) (subvolume slice: 10 samples per pixel, runs fairly smoothly, looks 3d)</span></span>
* <span data-ttu-id="9a75b-120">200 \* 200 \* 2 \* 256 = 20480000 (5% 的完整)  (較低的 res 磁片區：較少的圖元、完整磁片區，看起來是3d，但有點模糊) </span><span class="sxs-lookup"><span data-stu-id="9a75b-120">200 \* 200 \* 2 \* 256 = 20480000 (5% of full) (lower res volume: fewer pixels, full volume, looks 3d but a bit blurry)</span></span>

## <a name="representing-3d-textures"></a><span data-ttu-id="9a75b-121">代表3D 紋理</span><span class="sxs-lookup"><span data-stu-id="9a75b-121">Representing 3D Textures</span></span>

<span data-ttu-id="9a75b-122">CPU：</span><span class="sxs-lookup"><span data-stu-id="9a75b-122">On the CPU:</span></span>

```
public struct Int3 { public int X, Y, Z; /* ... */ }
 public class VolumeHeader  {
   public readonly Int3 Size;
   public VolumeHeader(Int3 size) { this.Size = size;  }
   public int CubicToLinearIndex(Int3 index) {
     return index.X + (index.Y * (Size.X)) + (index.Z * (Size.X * Size.Y));
   }
   public Int3 LinearToCubicIndex(int linearIndex)
   {
     return new Int3((linearIndex / 1) % Size.X,
       (linearIndex / Size.X) % Size.Y,
       (linearIndex / (Size.X * Size.Y)) % Size.Z);
   }
   /* ... */
 }
 public class VolumeBuffer<T> {
   public readonly VolumeHeader Header;
   public readonly T[] DataArray;
   public T GetVoxel(Int3 pos)        {
     return this.DataArray[this.Header.CubicToLinearIndex(pos)];
   }
   public void SetVoxel(Int3 pos, T val)        {
     this.DataArray[this.Header.CubicToLinearIndex(pos)] = val;
   }
   public T this[Int3 pos] {
     get { return this.GetVoxel(pos); }
     set { this.SetVoxel(pos, value); }
   }
   /* ... */
 }
```

<span data-ttu-id="9a75b-123">在 GPU 上：</span><span class="sxs-lookup"><span data-stu-id="9a75b-123">On the GPU:</span></span>

```
float3 _VolBufferSize;
 int3 UnitVolumeToIntVolume(float3 coord) {
   return (int3)( coord * _VolBufferSize.xyz );
 }
 int IntVolumeToLinearIndex(int3 coord, int3 size) {
   return coord.x + ( coord.y * size.x ) + ( coord.z * ( size.x * size.y ) );
 }
 uniform StructuredBuffer<float> _VolBuffer;
 float SampleVol(float3 coord3 ) {
   int3 intIndex3 = UnitVolumeToIntVolume( coord3 );
   int index1D = IntVolumeToLinearIndex( intIndex3, _VolBufferSize.xyz);
   return __VolBuffer[index1D];
 }
```

## <a name="shading-and-gradients"></a><span data-ttu-id="9a75b-124">陰影和漸層</span><span class="sxs-lookup"><span data-stu-id="9a75b-124">Shading and Gradients</span></span>

<span data-ttu-id="9a75b-125">如何針對有用的視覺效果，為磁片區（例如 MRI）加上陰影。</span><span class="sxs-lookup"><span data-stu-id="9a75b-125">How to shade a volume, such as MRI, for useful visualization.</span></span> <span data-ttu-id="9a75b-126">主要方法是讓「濃度視窗」 (您想要在其中查看濃度的最小和最大) ，並只是調整到該空間以查看黑色和白色濃度。</span><span class="sxs-lookup"><span data-stu-id="9a75b-126">The primary method is to have an 'intensity window' (a min and max) that you want to see intensities within, and simply scale into that space to see the black and white intensity.</span></span> <span data-ttu-id="9a75b-127">然後，您可以將「色彩斜接」套用到該範圍內的值，並儲存為材質，讓濃度色譜的不同部分可以用陰影呈現不同的色彩：</span><span class="sxs-lookup"><span data-stu-id="9a75b-127">A 'color ramp' can then be applied to the values within that range, and stored as a texture, so that different parts of the intensity spectrum can be shaded different colors:</span></span>

```
float4 ShadeVol( float intensity ) {
   float unitIntensity = saturate( intensity - IntensityMin / ( IntensityMax - IntensityMin ) );
   // Simple two point black and white intensity:
   color.rgba = unitIntensity;
   // Color ramp method:
   color.rgba = tex2d( ColorRampTexture, float2( unitIntensity, 0 ) );
```

<span data-ttu-id="9a75b-128">在我們的許多應用程式中，我們會將原始強度值和「分割索引」 (儲存在分割不同部分（例如面板和骨骼）的磁片區中;這些區段是由專家在專用工具) 中建立的。</span><span class="sxs-lookup"><span data-stu-id="9a75b-128">In many of our applications, we store in our volume both a raw intensity value and a 'segmentation index' (to segment different parts such as skin and bone; these segments are created by experts in dedicated tools).</span></span> <span data-ttu-id="9a75b-129">這可以與上面的方法結合，以針對每個區段索引放置不同的色彩，或甚至不同的顏色斜坡：</span><span class="sxs-lookup"><span data-stu-id="9a75b-129">This can be combined with the approach above to put a different color, or even different color ramp for each segment index:</span></span>

```
// Change color to match segment index (fade each segment towards black):
 color.rgb = SegmentColors[ segment_index ] * color.a; // brighter alpha gives brighter color
```

## <a name="volume-slicing-in-a-shader"></a><span data-ttu-id="9a75b-130">著色器中的磁片區切割</span><span class="sxs-lookup"><span data-stu-id="9a75b-130">Volume Slicing in a Shader</span></span>

<span data-ttu-id="9a75b-131">最好的第一個步驟是建立可在磁片區中移動的「切割平面」、「切割 it」，以及每個時間點的掃描值。</span><span class="sxs-lookup"><span data-stu-id="9a75b-131">A great first step is to create a "slicing plane" that can move through the volume, 'slicing it', and how the scan values at each point.</span></span> <span data-ttu-id="9a75b-132">這假設有一個「VolumeSpace」 cube，代表磁片區位於世界空間的位置，可用來做為放置點數的參考：</span><span class="sxs-lookup"><span data-stu-id="9a75b-132">This assumes that there's a 'VolumeSpace' cube, which represents where the volume is in world space, that can be used as a reference for placing the points:</span></span>

```
// In the vertex shader:
 float4 worldPos = mul(_Object2World, float4(input.vertex.xyz, 1));
 float4 volSpace = mul(_WorldToVolume, float4(worldPos, 1));
```

```
// In the pixel shader:
 float4 color = ShadeVol( SampleVol( volSpace ) );
```

## <a name="volume-tracing-in-shaders"></a><span data-ttu-id="9a75b-133">著色器中的磁片區追蹤</span><span class="sxs-lookup"><span data-stu-id="9a75b-133">Volume Tracing in Shaders</span></span>

<span data-ttu-id="9a75b-134">如何使用 GPU 來執行 subvolume 追蹤 (會逐步解說幾個體素，然後將資料上的分層移回前端) ：</span><span class="sxs-lookup"><span data-stu-id="9a75b-134">How to use the GPU to do subvolume tracing (walks a few voxels deep, then layers on the data from back to front):</span></span>

```
float4 AlphaBlend(float4 dst, float4 src) {
   float4 res = (src * src.a) + (dst - dst * src.a);
   res.a = src.a + (dst.a - dst.a*src.a);
   return res;
 }
 float4 volTraceSubVolume(float3 objPosStart, float3 cameraPosVolSpace) {
   float maxDepth = 0.15; // depth in volume space, customize!!!
   float numLoops = 10; // can be 400 on nice PC
   float4 curColor = float4(0, 0, 0, 0);
   // Figure out front and back volume coords to walk through:
   float3 frontCoord = objPosStart;
   float3 backCoord = frontPos + (normalize(cameraPosVolSpace - objPosStart) * maxDepth);
   float3 stepCoord = (frontCoord - backCoord) / numLoops;
   float3 curCoord = backCoord;
   // Add per-pixel random offset, avoids layer aliasing:
   curCoord += stepCoord * RandomFromPositionFast(objPosStart);
   // Walk from back to front (to make front appear in-front of back):
   for (float i = 0; i < numLoops; i++) {
     float intensity = SampleVol(curCoord);
     float4 shaded = ShadeVol(intensity);
     curColor = AlphaBlend(curColor, shaded);
     curCoord += stepCoord;
   }
   return curColor;
 }
```

```
// In the vertex shader:
 float4 worldPos = mul(_Object2World, float4(input.vertex.xyz, 1));
 float4 volSpace = mul(_WorldToVolume, float4(worldPos.xyz, 1));
 float4 cameraInVolSpace = mul(_WorldToVolume, float4(_WorldSpaceCameraPos.xyz, 1));
```

```
// In the pixel shader:
 float4 color = volTraceSubVolume( volSpace, cameraInVolSpace );
```

## <a name="whole-volume-rendering"></a><span data-ttu-id="9a75b-135">整個磁片區轉譯</span><span class="sxs-lookup"><span data-stu-id="9a75b-135">Whole Volume Rendering</span></span>

<span data-ttu-id="9a75b-136">修改上述的 subvolume 程式碼，我們取得：</span><span class="sxs-lookup"><span data-stu-id="9a75b-136">Modifying the subvolume code above, we get:</span></span>

```
float4 volTraceSubVolume(float3 objPosStart, float3 cameraPosVolSpace) {
   float maxDepth = 1.73; // sqrt(3), max distance from point on cube to any other point on cube
   int maxSamples = 400; // just in case, keep this value within bounds
   // not shown: trim front and back positions to both be within the cube
   int distanceInVoxels = length(UnitVolumeToIntVolume(frontPos - backPos)); // measure distance in voxels
   int numLoops = min( distanceInVoxels, maxSamples ); // put a min on the voxels to sample
```

## <a name="mixed-resolution-scene-rendering"></a><span data-ttu-id="9a75b-137">混合解析度場景轉譯</span><span class="sxs-lookup"><span data-stu-id="9a75b-137">Mixed Resolution Scene Rendering</span></span>

<span data-ttu-id="9a75b-138">如何呈現低解析度的場景部分，並將它放回原位：</span><span class="sxs-lookup"><span data-stu-id="9a75b-138">How to render a part of the scene with a low resolution and put it back in place:</span></span>
1. <span data-ttu-id="9a75b-139">設定兩個非全螢幕攝影機，每個畫面都要跟著更新每個畫面格的每一眼</span><span class="sxs-lookup"><span data-stu-id="9a75b-139">Setup two off-screen cameras, one to follow each eye that update each frame</span></span>
2. <span data-ttu-id="9a75b-140">設定兩個低解析度轉譯目標 (也就是200x200 攝影機轉譯的每個) </span><span class="sxs-lookup"><span data-stu-id="9a75b-140">Setup two low-resolution render targets (that is, 200x200 each) that the cameras render into</span></span>
3. <span data-ttu-id="9a75b-141">設定在使用者前方移動的四個四個</span><span class="sxs-lookup"><span data-stu-id="9a75b-141">Set up a quad that moves in front of the user</span></span>

<span data-ttu-id="9a75b-142">每個畫面格：</span><span class="sxs-lookup"><span data-stu-id="9a75b-142">Each Frame:</span></span>
1. <span data-ttu-id="9a75b-143">在低解析度 (磁片區資料、昂貴的著色器等) 上繪製每個眼睛的呈現目標</span><span class="sxs-lookup"><span data-stu-id="9a75b-143">Draw the render targets for each eye at low-resolution (volume data, expensive shaders, and so on)</span></span>
2. <span data-ttu-id="9a75b-144">以完整解析度的方式繪製場景， (網格、UI 等等) </span><span class="sxs-lookup"><span data-stu-id="9a75b-144">Draw the scene normally as full resolution (meshes, UI, and so on)</span></span>
3. <span data-ttu-id="9a75b-145">在使用者前方、場景上繪製四個四個專案，然後將低 res 轉譯至</span><span class="sxs-lookup"><span data-stu-id="9a75b-145">Draw a quad in front of the user, over the scene, and project the low-res renders onto that</span></span>
4. <span data-ttu-id="9a75b-146">結果：具有低解析度但高密度磁片區資料的完整解析度元素的視覺化組合</span><span class="sxs-lookup"><span data-stu-id="9a75b-146">Result: visual combination of full-resolution elements with low-resolution but high-density volume data</span></span>
