---
title: 空間音訊教學課程-5。 使用殘響增加空間音訊的距離
description: 新增回音效果，以增強距離變化與空間音訊的意義。
author: kegodin
ms.author: kegodin
ms.date: 12/01/2019
ms.topic: article
keywords: 混合的現實、unity、教學課程、hololens2、空間音訊、MRTK、混合現實工具組、UWP、Windows 10、HRTF、前端相關的傳送功能、回音、Microsoft 空間定位器、音訊混音器、SFX 回音
ms.openlocfilehash: d688955910d667edbdb79e63dab16587e66064a4
ms.sourcegitcommit: dd13a32a5bb90bd53eeeea8214cd5384d7b9ef76
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94679697"
---
# <a name="using-reverb-to-add-distance-to-spatial-audio"></a>使用殘響增加空間音訊的距離

## <a name="objectives"></a>目標
在上一章中，我們將 spatialization 新增至音效，讓他們有意義的方向。 在第5章中，我們將新增一項回音效果，讓聲音的距離更合理。 我們的目標是：
* 藉由新增回音來改善音效來源的認知距離
* 使用接聽程式與全像投影之間的距離，控制聲音的感覺距離

## <a name="add-a-mixer-group-and-a-reverb-effect"></a>新增混音器群組和回音效果
在 [第2章](unity-spatial-audio-ch2.md)中，我們新增了混音器。 混音器依預設會包含一個名為 **Master** 的 **群組**。 因為我們只想要將回音效果套用至某些音效，所以讓我們為這些音效新增第二個 **群組** 。 若要加入 **群組**，請以滑鼠右鍵按一下 **音訊混音** 器中的 **主要** 群組，然後選擇 [**新增子群組**]：

![新增子群組](images/spatial-audio/add-child-group.png)

在此範例中，我們將新群組命名為「房間效應」。

每個 **群組** 都有自己的一組效果。 按一下新群組上的 [新增 **...** ]，然後選擇 [ **SFX 回音**：

![新增 SFX 回音](images/spatial-audio/add-sfx-reverb.png)

在音訊術語中，原始的 unreverberated 音訊稱為「 _乾路徑_」，而以「回音」篩選篩選之後的音訊則稱為「 _潮濕」路徑_。 這兩個路徑都會傳送至音訊輸出，而其在這種混合中的相對強度稱為 _濕/幹組合_。 濕/幹混合對距離的意義有強烈的影響。

**SFX 的回音** 包含可調整效果中濕/幹混合的控制項。 因為 **Microsoft 空間定位器** 外掛程式會處理試路徑，所以我們只會針對潮濕的路徑使用 **SFX 的回音** 。 在您的 **SFX 回音** 的偵測 **器** 窗格上：
* 將 [乾等級] 屬性設定為最低的設定 (-10000 mB) 
* 將 [空間] 屬性設定為最高的設定 (0 mB) 

這些變更之後， **SFX** 的偵測 **器** 窗格看起來會像這樣：

![SFX 回音屬性](images/spatial-audio/sfx-reverb-properties.png)

其他設定則控制模擬房間的感覺。 尤其是， **衰減時間** 與認知的房間大小有關。 

## <a name="enable-reverb-on-the-video-playback"></a>啟用影片播放時的回音
有兩個步驟可在音訊來源上啟用回音：
* 將 **音訊來源** 路由傳送至適當的 **群組**
* 設定 **Microsoft 空間定位器** 外掛程式，以將音訊傳遞到 **群組** 進行處理

在下列步驟中，我們將調整腳本來控制音訊路由，並附加 **Microsoft 空間定位器** 外掛程式所提供的控制項腳本，將資料送入回送。

在 [] 的 [偵測 **器** ] 窗格 **中，按一下**[ **新增元件** ]，然後新增 [ **房間效果傳送層級** 腳本]：

![新增傳送層級腳本](images/spatial-audio/add-send-level-script.png)

> [!NOTE]
> 除非您啟用 **Microsoft 空間定位器** 外掛程式的 **房間效果傳送層級** 功能，否則它不會將任何音訊傳回至 Unity 音訊引擎以進行效果處理。

**房間效果傳送層級** 元件包含圖形控制項，可將傳送至 Unity 音訊引擎的音訊層級設定為進行回音處理。 按一下並向下拖曳曲線，將層級設定為 [30dB]：

![調整回音曲線](images/spatial-audio/adjust-reverb-curve.png)

接下來，將 **SpatializeOnOff** 腳本中的4個批註行取消批註。 腳本現在看起來會像這樣：
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

取消批註這些行會將兩個屬性新增至腳本的 [偵測 **器** ] 窗格。 若要在 **Spatialize** 的 [偵測 **器**] 窗格上設定這些設定，請在 [**四**：
* 將 [ **會議室效果群組** ] 屬性設定為新的房間效果混音器群組
* 將 [ **主要群組** ] 屬性設定為主要混音器群組

這些變更之後， **Spatialize On** 屬性看起來會像這樣：

![Spatialize 關閉擴充](images/spatial-audio/spatialize-on-off-extended.png)

## <a name="next-steps"></a>後續步驟

在 HoloLens 2 或 Unity 編輯器中試用您的應用程式。 現在，當您觸及應用程式中的按鈕以啟動 spatialization 時，腳本會將影片的音訊路由傳送至房間效果群組，以新增行程。 切換至身歷聲時，會將音訊路由傳送至主要群組，並避免新增回音。

您已完成適用于 Unity 的 HoloLens 2 空間音訊教學課程。 恭喜！


