---
title: 空間音訊教學課程-5。 使用殘響增加空間音訊的距離
description: 新增回音效果，以增強距離變化與空間音訊的意義。
author: kegodin
ms.author: v-hferrone
ms.date: 12/01/2019
ms.topic: article
keywords: 混合的現實、unity、教學課程、hololens2、空間音訊、MRTK、混合現實工具組、UWP、Windows 10、HRTF、前端相關的傳送功能、回音、Microsoft 空間定位器、音訊混音器、SFX 回音
ms.openlocfilehash: c63e5a239806c133e814eee8b44cbfb30f55aa5d
ms.sourcegitcommit: fbeff51cae92add88d2b960c9b7bbfb04d5a0291
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97002613"
---
# <a name="using-reverb-to-add-distance-to-spatial-audio"></a><span data-ttu-id="37df4-105">使用殘響增加空間音訊的距離</span><span class="sxs-lookup"><span data-stu-id="37df4-105">Using reverb to add distance to spatial audio</span></span>

## <a name="objectives"></a><span data-ttu-id="37df4-106">目標</span><span class="sxs-lookup"><span data-stu-id="37df4-106">Objectives</span></span>
<span data-ttu-id="37df4-107">在上一章中，我們將 spatialization 新增至音效，讓他們有意義的方向。</span><span class="sxs-lookup"><span data-stu-id="37df4-107">In previous chapters, we added spatialization to sounds to give them a sense of direction.</span></span> <span data-ttu-id="37df4-108">在第5章中，我們將新增一項回音效果，讓聲音的距離更合理。</span><span class="sxs-lookup"><span data-stu-id="37df4-108">In this 5th chapter, we'll add a reverb effect to give sounds a sense of distance.</span></span> <span data-ttu-id="37df4-109">我們的目標是：</span><span class="sxs-lookup"><span data-stu-id="37df4-109">Our objectives are to:</span></span>
* <span data-ttu-id="37df4-110">藉由新增回音來改善音效來源的認知距離</span><span class="sxs-lookup"><span data-stu-id="37df4-110">Improve perceived distance of sound sources by adding reverb</span></span>
* <span data-ttu-id="37df4-111">使用接聽程式與全像投影之間的距離，控制聲音的感覺距離</span><span class="sxs-lookup"><span data-stu-id="37df4-111">Control perceived distance of the sound using the listener's distance to the hologram</span></span>

## <a name="add-a-mixer-group-and-a-reverb-effect"></a><span data-ttu-id="37df4-112">新增混音器群組和回音效果</span><span class="sxs-lookup"><span data-stu-id="37df4-112">Add a mixer group and a reverb effect</span></span>
<span data-ttu-id="37df4-113">在 [第2章](unity-spatial-audio-ch2.md)中，我們新增了混音器。</span><span class="sxs-lookup"><span data-stu-id="37df4-113">In [Chapter 2](unity-spatial-audio-ch2.md), we added a mixer.</span></span> <span data-ttu-id="37df4-114">混音器依預設會包含一個名為 **Master** 的 **群組**。</span><span class="sxs-lookup"><span data-stu-id="37df4-114">The mixer includes one **Group** by default called **Master**.</span></span> <span data-ttu-id="37df4-115">因為我們只想要將回音效果套用至某些音效，所以讓我們為這些音效新增第二個 **群組** 。</span><span class="sxs-lookup"><span data-stu-id="37df4-115">Because we'll only want to apply a reverb effect to some sounds, let's add a second **Group** for those sounds.</span></span> <span data-ttu-id="37df4-116">若要加入 **群組**，請以滑鼠右鍵按一下 **音訊混音** 器中的 **主要** 群組，然後選擇 [**新增子群組**]：</span><span class="sxs-lookup"><span data-stu-id="37df4-116">To add a **Group**, right click on the **Master** group in the **Audio Mixer** and choose **Add child group**:</span></span>

![新增子群組](images/spatial-audio/add-child-group.png)

<span data-ttu-id="37df4-118">在此範例中，我們將新群組命名為「房間效應」。</span><span class="sxs-lookup"><span data-stu-id="37df4-118">In this example, we've named the new group "Room Effect".</span></span>

<span data-ttu-id="37df4-119">每個 **群組** 都有自己的一組效果。</span><span class="sxs-lookup"><span data-stu-id="37df4-119">Each **Group** has its own set of effects.</span></span> <span data-ttu-id="37df4-120">按一下新群組上的 [新增 **...** ]，然後選擇 [ **SFX 回音**：</span><span class="sxs-lookup"><span data-stu-id="37df4-120">Add a reverb effect to the new group by clicking **Add...** on the new group, and choosing **SFX Reverb**:</span></span>

![新增 SFX 回音](images/spatial-audio/add-sfx-reverb.png)

<span data-ttu-id="37df4-122">在音訊術語中，原始的 unreverberated 音訊稱為「 _乾路徑_」，而以「回音」篩選篩選之後的音訊則稱為「 _潮濕」路徑_。</span><span class="sxs-lookup"><span data-stu-id="37df4-122">In audio terminology, the original, unreverberated audio is called the _dry path_, and the audio after filtering with the reverb filter is called the _wet path_.</span></span> <span data-ttu-id="37df4-123">這兩個路徑都會傳送至音訊輸出，而其在這種混合中的相對強度稱為 _濕/幹組合_。</span><span class="sxs-lookup"><span data-stu-id="37df4-123">Both paths are sent to the audio output, and their relative strengths in this mixture is called the _wet/dry mix_.</span></span> <span data-ttu-id="37df4-124">濕/幹混合對距離的意義有強烈的影響。</span><span class="sxs-lookup"><span data-stu-id="37df4-124">The wet/dry mix strongly affects the sense of distance.</span></span>

<span data-ttu-id="37df4-125">**SFX 的回音** 包含可調整效果中濕/幹混合的控制項。</span><span class="sxs-lookup"><span data-stu-id="37df4-125">The **SFX Reverb** includes controls to adjust the wet/dry mix within the effect.</span></span> <span data-ttu-id="37df4-126">因為 **Microsoft 空間定位器** 外掛程式會處理試路徑，所以我們只會針對潮濕的路徑使用 **SFX 的回音** 。</span><span class="sxs-lookup"><span data-stu-id="37df4-126">Because the **Microsoft Spatializer** plugin handles the dry path, we'll be using the **SFX Reverb** only for the wet path.</span></span> <span data-ttu-id="37df4-127">在您的 **SFX 回音** 的偵測 **器** 窗格上：</span><span class="sxs-lookup"><span data-stu-id="37df4-127">On the **Inspector** pane of your **SFX Reverb**:</span></span>
* <span data-ttu-id="37df4-128">將 [乾等級] 屬性設定為最低的設定 (-10000 mB) </span><span class="sxs-lookup"><span data-stu-id="37df4-128">Set the Dry Level property to the lowest setting (-10000 mB)</span></span>
* <span data-ttu-id="37df4-129">將 [空間] 屬性設定為最高的設定 (0 mB) </span><span class="sxs-lookup"><span data-stu-id="37df4-129">Set the Room property to the highest setting (0 mB)</span></span>

<span data-ttu-id="37df4-130">這些變更之後， **SFX** 的偵測 **器** 窗格看起來會像這樣：</span><span class="sxs-lookup"><span data-stu-id="37df4-130">After these changes, the **Inspector** pane of the **SFX Reverb** will look like this:</span></span>

![SFX 回音屬性](images/spatial-audio/sfx-reverb-properties.png)

<span data-ttu-id="37df4-132">其他設定則控制模擬房間的感覺。</span><span class="sxs-lookup"><span data-stu-id="37df4-132">The other settings control the feel of the simulated room.</span></span> <span data-ttu-id="37df4-133">尤其是， **衰減時間** 與認知的房間大小有關。</span><span class="sxs-lookup"><span data-stu-id="37df4-133">In particular, **Decay Time** is related to perceived room size.</span></span> 

## <a name="enable-reverb-on-the-video-playback"></a><span data-ttu-id="37df4-134">啟用影片播放時的回音</span><span class="sxs-lookup"><span data-stu-id="37df4-134">Enable reverb on the video playback</span></span>
<span data-ttu-id="37df4-135">有兩個步驟可在音訊來源上啟用回音：</span><span class="sxs-lookup"><span data-stu-id="37df4-135">There are two steps to enable reverb on an audio source:</span></span>
* <span data-ttu-id="37df4-136">將 **音訊來源** 路由傳送至適當的 **群組**</span><span class="sxs-lookup"><span data-stu-id="37df4-136">Route the **Audio Source** to the appropriate **Group**</span></span>
* <span data-ttu-id="37df4-137">設定 **Microsoft 空間定位器** 外掛程式，以將音訊傳遞到 **群組** 進行處理</span><span class="sxs-lookup"><span data-stu-id="37df4-137">Set the **Microsoft Spatializer** plugin to pass audio into the **Group** for processing</span></span>

<span data-ttu-id="37df4-138">在下列步驟中，我們將調整腳本來控制音訊路由，並附加 **Microsoft 空間定位器** 外掛程式所提供的控制項腳本，將資料送入回送。</span><span class="sxs-lookup"><span data-stu-id="37df4-138">In the following steps, we'll adjust our script to control the audio routing, and attach a control script provided with the **Microsoft Spatializer** plugin to feed data into the reverb.</span></span>

<span data-ttu-id="37df4-139">在 [] 的 [偵測 **器** ] 窗格 **中，按一下**[ **新增元件** ]，然後新增 [ **房間效果傳送層級** 腳本]：</span><span class="sxs-lookup"><span data-stu-id="37df4-139">On the **Inspector** pane for the **Quad**, click **Add Component** and add the **Room Effect Send Level** script:</span></span>

![新增傳送層級腳本](images/spatial-audio/add-send-level-script.png)

> [!NOTE]
> <span data-ttu-id="37df4-141">除非您啟用 **Microsoft 空間定位器** 外掛程式的 **房間效果傳送層級** 功能，否則它不會將任何音訊傳回至 Unity 音訊引擎以進行效果處理。</span><span class="sxs-lookup"><span data-stu-id="37df4-141">Unless you enable **Room Effect Send Level** feature of the **Microsoft Spatializer** plugin, it doesn't send any audio back to the Unity audio engine for effect processing.</span></span>

<span data-ttu-id="37df4-142">**房間效果傳送層級** 元件包含圖形控制項，可將傳送至 Unity 音訊引擎的音訊層級設定為進行回音處理。</span><span class="sxs-lookup"><span data-stu-id="37df4-142">The **Room Effect Send Level** component includes a graph control that sets the level of the audio sent to the Unity audio engine for reverb processing.</span></span> <span data-ttu-id="37df4-143">按一下並向下拖曳曲線，將層級設定為 [30dB]：</span><span class="sxs-lookup"><span data-stu-id="37df4-143">Click and drag the curve downwards to set the level to about -30dB:</span></span>

![調整回音曲線](images/spatial-audio/adjust-reverb-curve.png)

<span data-ttu-id="37df4-145">接下來，將 **SpatializeOnOff** 腳本中的4個批註行取消批註。</span><span class="sxs-lookup"><span data-stu-id="37df4-145">Next, uncomment the 4 commented lines in the **SpatializeOnOff** script.</span></span> <span data-ttu-id="37df4-146">腳本現在看起來會像這樣：</span><span class="sxs-lookup"><span data-stu-id="37df4-146">The script will now look like this:</span></span>
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Audio;

[RequireComponent(typeof(AudioSource))]
public class SpatializeOnOff : MonoBehaviour
{
    public GameObject ButtonTextObject;
    public AudioMixerGroup RoomEffectGroup;
    public AudioMixerGroup MasterGroup;

    private AudioSource m_SourceObject;
    private bool m_IsSpatialized;
    private TMPro.TextMeshPro m_TextMeshPro;

    public void Start()
    {
        m_SourceObject = gameObject.GetComponent<AudioSource>();
        m_TextMeshPro = ButtonTextObject.GetComponent<TMPro.TextMeshPro>();
        SetSpatialized();
    }

    public void SwapSpatialization()
    {
        if (m_IsSpatialized)
        {
            SetStereo();
        }
        else
        {
            SetSpatialized();
        }
    }

    private void SetSpatialized()
    {
        m_IsSpatialized = true;
        m_SourceObject.spatialBlend = 1;
        m_TextMeshPro.SetText("Set Stereo");
        m_SourceObject.outputAudioMixerGroup = RoomEffectGroup;
    }

    private void SetStereo()
    {
        m_IsSpatialized = false;
        m_SourceObject.spatialBlend = 0;
        m_TextMeshPro.SetText("Set Spatialized");
        m_SourceObject.outputAudioMixerGroup = MasterGroup;
    }

}
```

<span data-ttu-id="37df4-147">取消批註這些行會將兩個屬性新增至腳本的 [偵測 **器** ] 窗格。</span><span class="sxs-lookup"><span data-stu-id="37df4-147">Uncommenting these lines adds two properties to the **Inspector** pane for the script.</span></span> <span data-ttu-id="37df4-148">若要在 **Spatialize** 的 [偵測 **器**] 窗格上設定這些設定，請在 [**四**：</span><span class="sxs-lookup"><span data-stu-id="37df4-148">To set these, on the **Inspector** pane of the **Spatialize On Off** component of the **Quad**:</span></span>
* <span data-ttu-id="37df4-149">將 [ **會議室效果群組** ] 屬性設定為新的房間效果混音器群組</span><span class="sxs-lookup"><span data-stu-id="37df4-149">Set the **Room Effect Group** property to your new Room Effect mixer group</span></span>
* <span data-ttu-id="37df4-150">將 [ **主要群組** ] 屬性設定為主要混音器群組</span><span class="sxs-lookup"><span data-stu-id="37df4-150">Set the **Master Group** property to the Master mixer group</span></span>

<span data-ttu-id="37df4-151">這些變更之後， **Spatialize On** 屬性看起來會像這樣：</span><span class="sxs-lookup"><span data-stu-id="37df4-151">After these changes, the **Spatialize On Off** properties will look like this:</span></span>

![Spatialize 關閉擴充](images/spatial-audio/spatialize-on-off-extended.png)

## <a name="next-steps"></a><span data-ttu-id="37df4-153">後續步驟</span><span class="sxs-lookup"><span data-stu-id="37df4-153">Next steps</span></span>

<span data-ttu-id="37df4-154">在 HoloLens 2 或 Unity 編輯器中試用您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="37df4-154">Try out your app on a HoloLens 2 or in the Unity editor.</span></span> <span data-ttu-id="37df4-155">現在，當您觸及應用程式中的按鈕以啟動 spatialization 時，腳本會將影片的音訊路由傳送至房間效果群組，以新增行程。</span><span class="sxs-lookup"><span data-stu-id="37df4-155">Now, when touching the button in the app to activate spatialization, the script will route the video's audio to the Room Effect Group to add reverb.</span></span> <span data-ttu-id="37df4-156">切換至身歷聲時，會將音訊路由傳送至主要群組，並避免新增回音。</span><span class="sxs-lookup"><span data-stu-id="37df4-156">When switching to stereo, it will route the audio to the Master group, and avoid adding reverb.</span></span>

<span data-ttu-id="37df4-157">您已完成適用于 Unity 的 HoloLens 2 空間音訊教學課程。</span><span class="sxs-lookup"><span data-stu-id="37df4-157">You've completed the HoloLens 2 spatial audio tutorials for Unity.</span></span> <span data-ttu-id="37df4-158">恭喜！</span><span class="sxs-lookup"><span data-stu-id="37df4-158">Congratulations!</span></span>


