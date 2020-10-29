---
title: SteamVR 常見問題
description: 超越標準取用者支援檔的 Advanced Windows Mixed Reality 疑難排解。
ms.topic: article
keywords: Windows Mixed Reality、混合的現實、虛擬實境、VR、MR、疑難排解、錯誤、協助、支援、SteamVR
ms.openlocfilehash: 0e355fc5035d7966f52642058d2f0ecc91b88351
ms.sourcegitcommit: 09599b4034be825e4536eeb9566968afd021d5f3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/03/2020
ms.locfileid: "91680285"
---
# <a name="steamvr-faqs"></a>SteamVR 常見問題

## <a name="how-can-i-play-steamvr-games-in-my-windows-mixed-reality-headset"></a>如何在我的 Windows Mixed Reality 耳機播放 SteamVR 遊戲？

1. [下載並安裝 SteamVR](https://steamcdn-a.akamaihd.net/client/installer/SteamWindowsMRInstaller.exe)。 當您開始 SteamVR 時，SteamVR 教學課程應會自動啟動。
2. 將您的耳機連接到您的電腦，然後開啟您的動作控制器。
3. 一旦載入 Windows Mixed Reality 首頁，並顯示您的控制器，請在您的桌面上開啟串流應用程式。
4. 使用流應用程式從您的串流庫啟動 SteamVR 遊戲。 若要在不離開耳機的情況下啟動 SteamVR 遊戲，請在 Windows Mixed Reality 的 [ **開始 > 所有應用程式** ] 下找到並啟動遊戲。 

## <a name="a-message-says-to-use-steamvr-with-windows-mixed-reality-you-need-to-install-the-latest-windows-update-or-windows-developer-mode-required"></a>訊息顯示「若要搭配 Windows Mixed Reality 使用 SteamVR，您必須安裝最新的 Windows Update」或「需要 Windows 開發人員模式」。

1. 確定您的電腦正在執行最新版本的 Windows 10。 移至 [ **設定 > 系統 >** ]，然後在 [Windows 規格] 下，確定 [作業系統組建] 為16299.64 或更新版本。
2. 請確定您沒有任何更新正在等候下載或安裝。 移至 [ **設定] > 更新 & 安全性 > Windows Update** 然後選取 [檢查更新]。 您可能必須多次檢查，直到沒有其他可用的更新，然後重新開機您的電腦。

## <a name="steamvr-is-crashing-after-updating-windows"></a>SteamVR 在更新 Windows 之後損毀。

某些較舊版本的 SteamVR Windows Mixed Reality 不再與 Windows 相容。 若要確保 SteamVR 的 Windows Mixed Reality 版本是最新的：
1. 在您的串流媒體櫃中，移至 [ **軟體 >] Windows Mixed Reality 以進行 SteamVR** 。
2. 以滑鼠右鍵按一下該檔案，然後移至 [屬性]。
3. 選取 [更新] 索引標籤和 [永遠將此應用程式保持在最新狀態]。
4. 請前往 [本機檔案] 索引標籤，然後選取 [驗證應用程式檔的完整性]，來強制更新。
5. 重新開機流和 SteamVR。

如果 SteamVR 在更新之後仍損毀，您可能會在電腦上安裝兩個適用于 SteamVR 的 Windows Mixed Reality。 若要確認是否為這種情況：
1. 在 [記事本] 中找出 ```%localappdata%\openvr\openvrpaths.vrpath``` 並開啟它。
2. 在 [外部驅動程式] 區段中，尋找 "MixedRealityVRDriver" 的多個專案 
   ```json
   "external_drivers" : [
      "D:\\Steam\\steamapps\\common\\MixedRealityVRDriver",
      "E:\\Steam\\steamapps\\common\\MixedRealityVRDriver"
   ],
   ```
3. 如果您看到多個專案，請移除兩個專案中的舊專案。 當您只有一個專案時，行尾應該不會再有一個逗號。 例如：
   ```json
   "external_drivers" : [
      "D:\\Steam\\steamapps\\common\\MixedRealityVRDriver"
   ],
   ```
4. 儲存並關閉檔案。
5. 重新開機流和 SteamVR。

## <a name="my-controllers-arent-working-as-expected-in-steamvr"></a>我的控制器在 SteamVR 中未如預期般運作。

1. 關閉 SteamVR。
2. 返回 Windows Mixed Reality 首頁，並確認您的控制器正在運作。
3. 再次啟動 SteamVR 體驗，您的控制器應該恢復正常。
4. 如果問題持續發生，請使用「混合現實」類別下的 [Windows 意見反應中樞](https://support.microsoft.com/en-us/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app) 來提出意見反應，並在摘要中包含 SteamVR。

請注意，您將在不同的遊戲中使用不同的移動控制器。 以下是一些可協助您開始使用的基本概念：
* 若要開啟 [串流] 儀表板，請在左側操縱杆上按直向下。
* 若要結束 SteamVR 遊戲並回到 Windows Mixed Reality 首頁，請按 [Windows] 按鈕。 然後選取畫面上顯示的 Mixed Reality 首頁按鈕。

## <a name="my-left-and-right-controllers-are-reversed-in-steamvr"></a>我的左側和右側控制器會在 SteamVR 中反轉。

從您的控制器開始遊戲，然後開啟左邊的控制器，後面接著右邊的控制器。

## <a name="my-games-are-running-slowly"></a>我的遊戲執行速度很慢。

1. 確定您的電腦符合 Windows Mixed Reality 的 SteamVR 規格，以及您所播放的 SteamVR 遊戲。
2. 在桌面上的混合實境入口中，選取 [暫停] 以停止桌面預覽版。
3. 移至 [ **設定] > 系統 >** [Windows 規格] 下的 [作業系統]，確定 [作業系統組建] 是16299.64 或更新版本。
4. 確定您的電腦具有最新的圖形驅動程式， ( Windows Update) 中的 [檢查更新]。
5. 檢查「工作管理員」以查看其他進程可能耗用您電腦上的資源。
6. 查看串流是否正在背景下載遊戲。 這可能會耗用資源，讓遊戲的執行效能不佳。
7. 沒有可見視窗 (的小型應用程式類別（例如 SteamVR Home) ）有已知的效能問題。 大部分的應用程式並不屬於這個類別，未來的更新將會提供修正。

如果您仍在遇到未預期的效能問題，請使用 [Windows 意見反應中樞](https://support.microsoft.com/en-us/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app)傳送意見反應給我們。 請務必遵循指示來 [納入 SteamVR 效能追蹤](using-steamvr-with-windows-mixed-reality.md#sharing-feedback-on-steamvr)。 

## <a name="steamvr-is-showing-a-compositor-error-for-example-shared-ipc-compositor-connect-failed-400"></a>SteamVR 顯示組合器錯誤 (例如「共用的 IPC 組合器連接失敗 (400) 」 ) 。

如果您的耳機和主要監視器位在兩個不同的視訊卡上，就可能發生這種情況。 將您的監視器附加至與耳機相同的介面卡，並將該監視設定為 [設定] 應用程式中的主要監視 **> 系統 > 顯示** 。

## <a name="steamvr-content-appears-in-the-wrong-place-like-beneath-the-floor-or-above-my-head"></a>SteamVR 內容會顯示在錯誤的位置，例如在地面下方或上方。

重設您的位置： 
1. 按一下左邊控制器的操縱杆以顯示「SteamVR 儀表板」。
2. 選取 [設定] 按鈕。
3. 選取 [重設已安裝的位置]。

## <a name="my-steam-app-closed-unexpectedly"></a>我的流應用程式意外關閉。

如果您鎖定電腦畫面、移除耳機、切換使用者，或電腦進入睡眠狀態，則會關閉流應用程式。
