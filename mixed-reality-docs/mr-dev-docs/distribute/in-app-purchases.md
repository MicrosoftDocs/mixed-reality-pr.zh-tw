---
title: 應用程式內建購買功能
description: 瞭解如何在您的混合現實應用程式中，搭配 2D XAML views 和 stock Windows OS 快顯視窗使用應用程式內購買。
author: thetuvix
ms.author: alexturn
ms.date: 03/21/2018
ms.topic: article
keywords: 應用程式內購買、hololens、XAML、混合現實耳機、windows mixed reality 耳機、虛擬實境耳機
ms.openlocfilehash: dfc5a0cfcc7a4d63147a753c8892d65dfae5e495
ms.sourcegitcommit: d3a3b4f13b3728cfdd4d43035c806c0791d3f2fe
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/20/2021
ms.locfileid: "98582954"
---
# <a name="in-app-purchases"></a><span data-ttu-id="22431-104">應用程式內建購買功能</span><span class="sxs-lookup"><span data-stu-id="22431-104">In-app purchases</span></span>

<span data-ttu-id="22431-105">HoloLens 中支援應用程式內購買，但有一些工作可以進行設定。</span><span class="sxs-lookup"><span data-stu-id="22431-105">In-app purchases are supported in HoloLens, but there's some work to set them up.</span></span>

<span data-ttu-id="22431-106">若要使用應用程式購買功能，您必須：</span><span class="sxs-lookup"><span data-stu-id="22431-106">To use the in app-purchase functionality, you must:</span></span>
* <span data-ttu-id="22431-107">建立 XAML [2d 視圖](../design/app-views.md) 以顯示為平板</span><span class="sxs-lookup"><span data-stu-id="22431-107">Create a XAML [2D view](../design/app-views.md) to appear as a slate</span></span>
* <span data-ttu-id="22431-108">切換至該位置以啟用放置，使其離開沉浸式視圖</span><span class="sxs-lookup"><span data-stu-id="22431-108">Switch to it to activate placement, which leaves the immersive view</span></span>
* <span data-ttu-id="22431-109">呼叫 API： await [CurrentApp. RequestProductPurchaseAsync ( "DurableItemIAPName" ) ;](/uwp/api/windows.applicationmodel.store.currentapp#Windows_ApplicationModel_Store_CurrentApp_RequestProductPurchaseAsync_System_String_)</span><span class="sxs-lookup"><span data-stu-id="22431-109">Call the API: await [CurrentApp.RequestProductPurchaseAsync("DurableItemIAPName");](/uwp/api/windows.applicationmodel.store.currentapp#Windows_ApplicationModel_Store_CurrentApp_RequestProductPurchaseAsync_System_String_)</span></span>

<span data-ttu-id="22431-110">此 API 會顯示顯示應用程式內購買名稱、描述和價格的 stock Windows OS 快顯視窗。</span><span class="sxs-lookup"><span data-stu-id="22431-110">This API will bring up the stock Windows OS popup that shows the in-app purchase name, description, and price.</span></span> <span data-ttu-id="22431-111">然後，使用者可以選擇購買選項。</span><span class="sxs-lookup"><span data-stu-id="22431-111">The user can then choose purchase options.</span></span> <span data-ttu-id="22431-112">完成此動作之後，應用程式將需要顯示 UI，讓使用者能夠切換回其 [沉浸式視圖](../design/app-views.md)。</span><span class="sxs-lookup"><span data-stu-id="22431-112">Once the action is completed, the app will need to present UI, which allows the user to switch back to its [immersive view](../design/app-views.md).</span></span>

<span data-ttu-id="22431-113">針對以桌面為基礎 Windows Mixed Reality 沉浸式耳機的應用程式，在呼叫 RequestProductPurchaseAsync API 之前，不需要手動切換至 XAML 視圖。</span><span class="sxs-lookup"><span data-stu-id="22431-113">For apps targeting desktop-based Windows Mixed Reality immersive headsets, it isn't required to manually switch to a XAML view before calling the RequestProductPurchaseAsync API.</span></span>