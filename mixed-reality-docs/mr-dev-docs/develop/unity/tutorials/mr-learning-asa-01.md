---
title: Azure Spatial Anchor 教學課程 - 1。 Azure Spatial Anchors 教學課程簡介
description: 完成此課程以了解如何在混合實境應用程式中實作 Azure Spatial Anchors。
author: jessemcculloch
ms.author: jemccull
ms.date: 07/01/2020
ms.topic: article
keywords: 混合實境, unity, 教學課程, hololens, MRTK, 混合實境工具組, UWP, Azure 空間錨點, ios, android, Windows 10, ARCore, macOS, Android 建置支援, ARKit
ms.localizationpriority: high
ms.openlocfilehash: 005bbf3f9ecb7ed7f15f78d4042b4090f00edca7
ms.sourcegitcommit: dd13a32a5bb90bd53eeeea8214cd5384d7b9ef76
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94679397"
---
# <a name="1-introduction-to-the-azure-spatial-anchors-tutorials"></a><span data-ttu-id="92c4d-105">1.Azure Spatial Anchors 教學課程簡介</span><span class="sxs-lookup"><span data-stu-id="92c4d-105">1. Introduction to the Azure Spatial Anchors tutorials</span></span>

## <a name="overview"></a><span data-ttu-id="92c4d-106">概觀</span><span class="sxs-lookup"><span data-stu-id="92c4d-106">Overview</span></span>

<span data-ttu-id="92c4d-107">歡迎使用 Azure Spatial Anchors 教學課程！</span><span class="sxs-lookup"><span data-stu-id="92c4d-107">Welcome to the Azure Spatial Anchors tutorials!</span></span> <span data-ttu-id="92c4d-108">透過本教學課程系列，您將了解 <a href="https://azure.microsoft.com/services/spatial-anchors" target="_blank">Azure Spatial Anchors</a> (ASA) 的基本概念，以及如何在真實世界中錨定完整的混合實境體驗。</span><span class="sxs-lookup"><span data-stu-id="92c4d-108">Through this tutorial series, you will learn the fundamentals of <a href="https://azure.microsoft.com/services/spatial-anchors" target="_blank">Azure Spatial Anchors</a> (ASA) and how to anchor a complete mixed reality experience in the real world.</span></span> <span data-ttu-id="92c4d-109">您也將了解如何將專案部署至 Android 和 iOS。</span><span class="sxs-lookup"><span data-stu-id="92c4d-109">You will also learn how to deploy your project to Android and iOS.</span></span>

<span data-ttu-id="92c4d-110">本系列的教學課程：</span><span class="sxs-lookup"><span data-stu-id="92c4d-110">Tutorials in this series:</span></span>

1. <span data-ttu-id="92c4d-111">[簡介](mr-learning-asa-01.md) (您正在此教學課程中)</span><span class="sxs-lookup"><span data-stu-id="92c4d-111">[Introduction](mr-learning-asa-01.md) (You're already here)</span></span>
2. [<span data-ttu-id="92c4d-112">開始使用 Azure Spatial Anchors</span><span class="sxs-lookup"><span data-stu-id="92c4d-112">Getting started with Azure Spatial Anchors</span></span>](mr-learning-asa-02.md)
3. [<span data-ttu-id="92c4d-113">儲存、擷取和共用 Azure Spatial Anchors</span><span class="sxs-lookup"><span data-stu-id="92c4d-113">Saving, retrieving, and sharing Azure Spatial Anchors</span></span>](mr-learning-asa-03.md)
4. [<span data-ttu-id="92c4d-114">顯示 Azure Spatial Anchor 意見反應</span><span class="sxs-lookup"><span data-stu-id="92c4d-114">Displaying Azure Spatial Anchor feedback</span></span>](mr-learning-asa-04.md)
5. [<span data-ttu-id="92c4d-115">適用於 Android 和 iOS 的 Azure Spatial Anchors</span><span class="sxs-lookup"><span data-stu-id="92c4d-115">Azure Spatial Anchors for Android and iOS</span></span>](mr-learning-asa-05.md)

## <a name="objectives"></a><span data-ttu-id="92c4d-116">目標</span><span class="sxs-lookup"><span data-stu-id="92c4d-116">Objectives</span></span>

* <span data-ttu-id="92c4d-117">了解如何建立空間錨點，並從 Azure 提取這些錨點</span><span class="sxs-lookup"><span data-stu-id="92c4d-117">Learn how to create spatial anchors and fetch them from Azure</span></span>
* <span data-ttu-id="92c4d-118">了解如何跨多個應用程式工作階段來對齊空間</span><span class="sxs-lookup"><span data-stu-id="92c4d-118">Learn how to achieve spatial alignment across app sessions</span></span>
* <span data-ttu-id="92c4d-119">了解如何在多個裝置之間對齊空間</span><span class="sxs-lookup"><span data-stu-id="92c4d-119">Learn how to achieve spatial alignment between multiple devices</span></span>
* <span data-ttu-id="92c4d-120">了解如何準備和置專案，並部署至 Android 和 iOS</span><span class="sxs-lookup"><span data-stu-id="92c4d-120">Learn how to prepare, build, and deploy your project to Android and iOS</span></span>

## <a name="prerequisites"></a><span data-ttu-id="92c4d-121">必要條件</span><span class="sxs-lookup"><span data-stu-id="92c4d-121">Prerequisites</span></span>

* <span data-ttu-id="92c4d-122">已[安裝正確工具](../../install-the-tools.md)的 Windows 10 電腦</span><span class="sxs-lookup"><span data-stu-id="92c4d-122">A Windows 10 computer configured with the correct [tools installed](../../install-the-tools.md)</span></span>
* <span data-ttu-id="92c4d-123">Windows 10 SDK 10.0.18362.0 或更新版本</span><span class="sxs-lookup"><span data-stu-id="92c4d-123">Windows 10 SDK 10.0.18362.0 or later version</span></span>
* <span data-ttu-id="92c4d-124">已[針對開發而設定](../../platform-capabilities-and-apis/using-visual-studio.md#enabling-developer-mode)的 HoloLens 2 裝置</span><span class="sxs-lookup"><span data-stu-id="92c4d-124">A HoloLens 2 device [configured for development](../../platform-capabilities-and-apis/using-visual-studio.md#enabling-developer-mode)</span></span>
* <span data-ttu-id="92c4d-125">已安裝 Unity 2019.3.15 的 <a href="https://docs.unity3d.com/Manual/GettingStartedInstallingHub.html" target="_blank">Unity Hub</a>，且已新增通用 Windows 平台組建支援模組</span><span class="sxs-lookup"><span data-stu-id="92c4d-125"><a href="https://docs.unity3d.com/Manual/GettingStartedInstallingHub.html" target="_blank">Unity Hub</a> with Unity 2019.3.15 installed and the Universal Windows Platform Build Support module added</span></span>
* <span data-ttu-id="92c4d-126">完成[建立空間錨點資源](https://docs.microsoft.com/azure/spatial-anchors/quickstarts/get-started-unity-hololens#create-a-spatial-anchors-resource)一節，其位於[教學課程：建立使用 Azure Spatial Anchors 的 Unity HoloLens 應用程式](https://docs.microsoft.com/azure/spatial-anchors/quickstarts/get-started-unity-hololens)</span><span class="sxs-lookup"><span data-stu-id="92c4d-126">Completed the [Create a Spatial Anchors resource](https://docs.microsoft.com/azure/spatial-anchors/quickstarts/get-started-unity-hololens#create-a-spatial-anchors-resource) section of the [Quickstart: Create a Unity HoloLens app that uses Azure Spatial Anchors](https://docs.microsoft.com/azure/spatial-anchors/quickstarts/get-started-unity-hololens) tutorial</span></span>
* <span data-ttu-id="92c4d-127">完成[使用者入門教學課程](mr-learning-base-01.md)系列或一些基本的 Unity 和 MRTK 體驗</span><span class="sxs-lookup"><span data-stu-id="92c4d-127">Finished the [Getting started tutorials](mr-learning-base-01.md) series or some basic prior experience with Unity and MRTK</span></span>
* <span data-ttu-id="92c4d-128">如果想要部署至 Android 及 HoloLens</span><span class="sxs-lookup"><span data-stu-id="92c4d-128">If you intend to deploy to Android as well as HoloLens</span></span>
  * <span data-ttu-id="92c4d-129">已啟用<a href="https://developer.android.com/studio/debug/dev-options" target="_blank">開發人員</a>和具有 <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">ARCore 功能</a>的 Android 裝置，可透過 USB 連接至您的 Windows 或 macOS 電腦</span><span class="sxs-lookup"><span data-stu-id="92c4d-129">A <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">developer enabled</a> and <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">ARCore capable</a> Android device with USB connection to your Windows or macOS computer</span></span>
  * <span data-ttu-id="92c4d-130">已安裝 Unity 2019.3.15 的 <a href="https://docs.unity3d.com/Manual/GettingStartedInstallingHub.html" target="_blank">Unity Hub</a>，且已新增 Android 組建支援模組</span><span class="sxs-lookup"><span data-stu-id="92c4d-130"><a href="https://docs.unity3d.com/Manual/GettingStartedInstallingHub.html" target="_blank">Unity Hub</a> with Unity 2019.3.15 installed and the Android Build Support module added</span></span>
* <span data-ttu-id="92c4d-131">如果想要部署至 iOS 及 HoloLens</span><span class="sxs-lookup"><span data-stu-id="92c4d-131">If you intend to deploy to iOS as well as HoloLens</span></span>
  * <span data-ttu-id="92c4d-132">已安裝最新版 <a href="https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12" target="_blank">Xcode</a> 和 <a href="https://cocoapods.org" target="_blank">CocoaPods</a> 的 macOS 電腦</span><span class="sxs-lookup"><span data-stu-id="92c4d-132">A macOS computer with the latest version of <a href="https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12" target="_blank">Xcode</a> and <a href="https://cocoapods.org" target="_blank">CocoaPods</a> installed</span></span>
  * <span data-ttu-id="92c4d-133"><a href="https://developer.apple.com/documentation/arkit/verifying_device_support_and_user_permission" target="_blank">ARKit 相容</a>的 iOS 裝置可透過 USB 連接至您的 macOS 電腦</span><span class="sxs-lookup"><span data-stu-id="92c4d-133">An <a href="https://developer.apple.com/documentation/arkit/verifying_device_support_and_user_permission" target="_blank">ARKit compatible</a> iOS device with USB connection to your macOS computer</span></span>
  * <span data-ttu-id="92c4d-134">已安裝 Unity 2019.3.15 的 <a href="https://docs.unity3d.com/Manual/GettingStartedInstallingHub.html" target="_blank">Unity Hub</a>，且已新增 iOS 組建支援模組</span><span class="sxs-lookup"><span data-stu-id="92c4d-134"><a href="https://docs.unity3d.com/Manual/GettingStartedInstallingHub.html" target="_blank">Unity Hub</a> with Unity 2019.3.15 installed and the iOS Build Support module added</span></span>

> [!CAUTION]
> <span data-ttu-id="92c4d-135">本教學課程系列的建議混合實境工具組版本是 MRTK 2.4.0。</span><span class="sxs-lookup"><span data-stu-id="92c4d-135">The recommended Mixed Reality Toolkit version for this tutorial series is MRTK 2.4.0.</span></span>

> [!CAUTION]
> <span data-ttu-id="92c4d-136">本教學課程系列的建議 Unity 版本是 Unity 2019.3.15。</span><span class="sxs-lookup"><span data-stu-id="92c4d-136">The recommended Unity version for this tutorial series is Unity 2019.3.15.</span></span> <span data-ttu-id="92c4d-137">這個版本能取代上述連結必要條件中所述的任何 Unity 版本需求。</span><span class="sxs-lookup"><span data-stu-id="92c4d-137">This supersedes any Unity version requirements stated in the prerequisites linked above.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="92c4d-138">下一個教學課程：2.開始使用 Azure Spatial Anchors</span><span class="sxs-lookup"><span data-stu-id="92c4d-138">Next Tutorial: 2. Getting started with Azure Spatial Anchors</span></span>](mr-learning-asa-02.md)
