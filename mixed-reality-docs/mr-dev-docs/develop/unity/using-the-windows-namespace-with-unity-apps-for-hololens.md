---
title: 適用于 HoloLens 的 WinRT Api 搭配 Unity
description: Leanr 如何在您的 Unity 混合現實專案中，使用 WinRT Api 和 Windows 命名空間來進行 HoloLens。
author: mikeriches
ms.author: mriches
ms.date: 03/21/2018
ms.topic: article
keywords: Unity、WinRT、windows mixed reality、API、逐步解說、混合現實耳機、windows mixed reality 耳機、虛擬實境耳機、混合現實 Api
ms.openlocfilehash: 2c57af72a10867b5ef4fc87ff96679e576d203f4
ms.sourcegitcommit: 2329db5a76dfe1b844e21291dbc8ee3888ed1b81
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98007318"
---
# <a name="winrt-apis-with-unity-for-hololens"></a><span data-ttu-id="f0173-104">適用于 HoloLens 的 WinRT Api 搭配 Unity</span><span class="sxs-lookup"><span data-stu-id="f0173-104">WinRT APIs with Unity for HoloLens</span></span>

<span data-ttu-id="f0173-105">此頁面說明如何在您的 Unity 專案中，將 WinRT Api 用於 HoloLens。</span><span class="sxs-lookup"><span data-stu-id="f0173-105">This page describes how to make use of WinRT APIs in your Unity project for HoloLens.</span></span>

## <a name="mixed-reality-apis"></a><span data-ttu-id="f0173-106">混合現實 Api</span><span class="sxs-lookup"><span data-stu-id="f0173-106">Mixed Reality APIs</span></span>

<span data-ttu-id="f0173-107">Windows SDK 的混合現實焦點子集已在 .NET Standard 2.0 相容投射中提供，您可以在專案中使用，而不需要預處理器指示詞。</span><span class="sxs-lookup"><span data-stu-id="f0173-107">A Mixed Reality focused subset of the Windows SDK has been made available in a .NET Standard 2.0 compatible projection, which you can use in your project without preprocessor directives.</span></span> <span data-ttu-id="f0173-108">Windows 中大部分的 Api。</span><span class="sxs-lookup"><span data-stu-id="f0173-108">Most APIs in the Windows.</span></span> <span data-ttu-id="f0173-109">認知和 Windows. 輸入. 空間命名空間包含在內，未來可能會擴充以包含其他 Api。</span><span class="sxs-lookup"><span data-stu-id="f0173-109">Perception and Windows.UI.Input.Spatial namespaces are included and may expand to include additional APIs in the future.</span></span> <span data-ttu-id="f0173-110">在編輯器中執行時，可以使用所投射的 Api，以啟用 [播放模式](https://docs.microsoft.com//windows/mixed-reality/unity-play-mode)。</span><span class="sxs-lookup"><span data-stu-id="f0173-110">The projected APIs can be used while running in the Editor, which enables the use of [Play Mode](https://docs.microsoft.com//windows/mixed-reality/unity-play-mode).</span></span> <span data-ttu-id="f0173-111">若要使用這項投射，請對您的專案進行下列修改：</span><span class="sxs-lookup"><span data-stu-id="f0173-111">To use this projection, make the following modifications to your project:</span></span>

1) <span data-ttu-id="f0173-112">使用[適用于 Unity 的 nuget](https://github.com/GlitchEnzo/NuGetForUnity)來新增[MixedReality DotNetWinRT](https://www.nuget.org/packages/Microsoft.Windows.MixedReality.DotNetWinRT) nuget 套件的參考。</span><span class="sxs-lookup"><span data-stu-id="f0173-112">Add a reference to the [Microsoft.Windows.MixedReality.DotNetWinRT](https://www.nuget.org/packages/Microsoft.Windows.MixedReality.DotNetWinRT) NuGet package using [NuGet for Unity](https://github.com/GlitchEnzo/NuGetForUnity).</span></span>
2) <span data-ttu-id="f0173-113">命名空間的前置詞參考 `Windows` `Microsoft.` ：</span><span class="sxs-lookup"><span data-stu-id="f0173-113">Prefix references to the `Windows` namespace with `Microsoft.`:</span></span>
```cs
using namespace Microsoft.Windows.Perception.Spatial;
```
3) <span data-ttu-id="f0173-114">以下列內容取代原生指標轉換 `FromNativePtr` ：</span><span class="sxs-lookup"><span data-stu-id="f0173-114">Replace native pointer casts with `FromNativePtr`:</span></span>
```cs
var worldOrigin = SpatialCoordinateSystem.FromNativePtr(unityWorldOriginPtr);
```

## <a name="conditionally-include-winrt-api-calls"></a><span data-ttu-id="f0173-115">有條件地包含 WinRT API 呼叫</span><span class="sxs-lookup"><span data-stu-id="f0173-115">Conditionally include WinRT API calls</span></span>

<span data-ttu-id="f0173-116">您也可以使用預處理器指示詞，在針對通用 Windows 平臺和 Xbox One 平臺建立的 Unity 專案中使用 WinRT Api。</span><span class="sxs-lookup"><span data-stu-id="f0173-116">You can also use the WinRT APIs in Unity projects built for the Universal Windows Platform and Xbox One platform by using preprocessor directives.</span></span> <span data-ttu-id="f0173-117">您在以 WinRT Api 為目標的 Unity 腳本中撰寫的任何程式碼，都必須有條件地包含在這些組建中。</span><span class="sxs-lookup"><span data-stu-id="f0173-117">Any code that you write in Unity scripts that target WinRT APIs must be conditionally included for only those builds.</span></span> 

<span data-ttu-id="f0173-118">這可以透過 Unity 中的兩個步驟來完成：</span><span class="sxs-lookup"><span data-stu-id="f0173-118">This can be done via two steps in Unity:</span></span>
1) <span data-ttu-id="f0173-119">API 相容性層級必須設定為 [播放機] 設定中的 [ **.net 4.6** ] 或 [ **.NET Standard 2.0** ]</span><span class="sxs-lookup"><span data-stu-id="f0173-119">API compatibility level must be set to **.NET 4.6** or **.NET Standard 2.0** in the player settings</span></span>
    - <span data-ttu-id="f0173-120">**編輯**  > **專案設定**  > **播放機**  > 設定  > 將 **Api 相容性層級** 設為 **.net 4.6** 或 **.NET Standard 2.0**</span><span class="sxs-lookup"><span data-stu-id="f0173-120">**Edit** > **Project Settings** > **Player** > **Configuration** > **Api Compatibility Level** to **.NET 4.6** or **.NET Standard 2.0**</span></span>
2) <span data-ttu-id="f0173-121">預處理器指示詞 **ENABLE_WINMD_SUPPORT** 必須包裝在任何 WinRT 的程式碼周圍</span><span class="sxs-lookup"><span data-stu-id="f0173-121">The preprocessor directive **ENABLE_WINMD_SUPPORT** must be wrapped around any WinRT-leveraged code</span></span>

<span data-ttu-id="f0173-122">下列程式碼片段來自 Unity manual 頁面，適用于 [c # 腳本中的通用 Windows 平臺： WINRT API](https://docs.unity3d.com/Manual/windowsstore-scripts.html)。</span><span class="sxs-lookup"><span data-stu-id="f0173-122">The following code snippet is from the Unity manual page for [Universal Windows Platform: WinRT API in C# scripts](https://docs.unity3d.com/Manual/windowsstore-scripts.html).</span></span> <span data-ttu-id="f0173-123">在此範例中，會傳回廣告識別碼，但只會傳回 UWP 和 Xbox One 組建：</span><span class="sxs-lookup"><span data-stu-id="f0173-123">In this example, an advertising ID is returned, but only on UWP and Xbox One builds:</span></span>

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

## <a name="edit-your-scripts-in-a-unity-c-project"></a><span data-ttu-id="f0173-124">在 Unity c # 專案中編輯您的腳本</span><span class="sxs-lookup"><span data-stu-id="f0173-124">Edit your scripts in a Unity C# project</span></span>

<span data-ttu-id="f0173-125">當您按兩下 Unity 編輯器中的腳本時，它預設會在編輯器專案中啟動您的腳本。</span><span class="sxs-lookup"><span data-stu-id="f0173-125">When you double-click a script in the Unity editor, it will by default launch your script in an editor project.</span></span> <span data-ttu-id="f0173-126">WinRT Api 會顯示為未知，因為 Visual Studio 專案未參考 Windows 執行階段。</span><span class="sxs-lookup"><span data-stu-id="f0173-126">The WinRT APIs will appear to be unknown because the Visual Studio project doesn't reference the Windows Runtime.</span></span> <span data-ttu-id="f0173-127">**ENABLE_WINMD_SUPPORT** 指示詞未定義，而且會忽略任何 *#if* 包裝程式碼，直到您將專案建立到 UWP Visual Studio 解決方案為止。</span><span class="sxs-lookup"><span data-stu-id="f0173-127">The **ENABLE_WINMD_SUPPORT** directive is undefined and any *#if* wrapped code is ignored until you build your project into a UWP Visual Studio solution.</span></span>

## <a name="see-also"></a><span data-ttu-id="f0173-128">請參閱</span><span class="sxs-lookup"><span data-stu-id="f0173-128">See also</span></span>
* [<span data-ttu-id="f0173-129">匯出和建置 Unity Visual Studio 解決方案</span><span class="sxs-lookup"><span data-stu-id="f0173-129">Exporting and building a Unity Visual Studio solution</span></span>](exporting-and-building-a-unity-visual-studio-solution.md)
* [<span data-ttu-id="f0173-130">Windows 執行階段支援 Unity</span><span class="sxs-lookup"><span data-stu-id="f0173-130">Windows Runtime Support Unity</span></span>](https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html)
