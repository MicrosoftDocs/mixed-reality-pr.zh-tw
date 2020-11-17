---
title: 適用于 HoloLens 的 WinRT Api 搭配 Unity
description: 說明如何在您的 Unity 專案中，將 WinRT Api (Windows 命名空間) 用於 HoloLens。
author: mikeriches
ms.author: mriches
ms.date: 03/21/2018
ms.topic: article
keywords: Unity、WinRT、windows mixed reality、API、逐步解說、混合現實耳機、windows mixed reality 耳機、虛擬實境耳機、混合現實 Api
ms.openlocfilehash: fb8d63a44a05f639becd96fc9198c57dd10aaafd
ms.sourcegitcommit: dd13a32a5bb90bd53eeeea8214cd5384d7b9ef76
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94679677"
---
# <a name="winrt-apis-with-unity-for-hololens"></a>適用于 HoloLens 的 WinRT Api 搭配 Unity

此頁面說明如何在您的 Unity 專案中，將 WinRT Api 用於 HoloLens。

## <a name="mixed-reality-apis"></a>混合現實 Api

Windows SDK 的混合現實焦點子集已在 .NET Standard 2.0 相容投射中提供，您可以在專案中使用，而不需要預處理器指示詞。 這包括 Windows 中的大部分 Api、命名空間命名空間，而且未來可能會擴充以包含其他 Api。 在編輯器中執行時，可以使用所投射的 Api，以啟用 [播放模式](https://docs.microsoft.com//windows/mixed-reality/unity-play-mode)。 若要使用這項投射，請對您的專案進行下列修改：

1) 使用[適用于 Unity 的 nuget](https://github.com/GlitchEnzo/NuGetForUnity)來新增[MixedReality DotNetWinRT](https://www.nuget.org/packages/Microsoft.Windows.MixedReality.DotNetWinRT) nuget 套件的參考。
2) 命名空間的前置詞參考 `Windows` `Microsoft.` ：
```cs
using namespace Microsoft.Windows.Perception.Spatial;
```
3) 以下列內容取代原生指標轉換 `FromNativePtr` ：
```cs
var worldOrigin = SpatialCoordinateSystem.FromNativePtr(unityWorldOriginPtr);
```

## <a name="conditionally-include-winrt-api-calls"></a>有條件地包含 WinRT API 呼叫

或者，您可以透過使用預處理器指示詞，利用 WinRT Api 針對通用 Windows 平臺和 Xbox One 平臺所建立的 Unity 專案;您在以 WinRT Api 為目標的 Unity 腳本中撰寫的任何程式碼，都必須有條件地包含在這些組建中。 

這可以透過 Unity 中的兩個步驟來完成：
1) API 相容性層級必須設定為 [播放機] 設定中的 [ **.net 4.6** ] 或 [ **.NET Standard 2.0** ]
    - **編輯**  > **專案設定**  > **播放機**  > **Configuration** 設定  > 將 **Api 相容性層級** 設為 **.net 4.6** 或 **.NET Standard 2.0**
2) 預處理器指示詞 **ENABLE_WINMD_SUPPORT** 必須包裝在任何 WinRT 的程式碼周圍

下列程式碼片段來自 Unity manual 頁面，適用于 [c # 腳本中的通用 Windows 平臺： WINRT API](https://docs.unity3d.com/Manual/windowsstore-scripts.html)。 在此範例中，會傳回廣告識別碼，但只會傳回 UWP 和 Xbox One 組建：

```
using UnityEngine;
public class WinRTAPI : MonoBehaviour {
    void Update() {
        auto adId = GetAdvertisingId();
        // ...
    }

    string GetAdvertisingId() {
        #if ENABLE_WINMD_SUPPORT
            return Windows.System.UserProfile.AdvertisingManager.AdvertisingId;
        #else
            return "";
        #endif
    }
}
```

## <a name="edit-your-scripts-in-a-unity-c-project"></a>在 Unity c # 專案中編輯您的腳本

當您按兩下 Unity 編輯器中的腳本時，它預設會在編輯器專案中啟動您的腳本。 WinRT Api 會顯示為未知，因為 Visual Studio 專案未參考 Windows 執行階段。 此外， **ENABLE_WINMD_SUPPORT** 指示詞將會被未定義，而且在您將專案建立到 UWP Visual Studio 解決方案之前，將會忽略任何 *#if* 包裝程式碼。

## <a name="see-also"></a>另請參閱
* [匯出和建置 Unity Visual Studio 解決方案](exporting-and-building-a-unity-visual-studio-solution.md)
* [Windows 執行階段支援 Unity](https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html)
