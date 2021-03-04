---
title: README_LostTrackingService
description: MRTK 中的 LostTracking 服務總覽
author: CDiaz-MS
ms.author: cadia
ms.date: 01/12/2021
ms.localizationpriority: high
keywords: Unity、HoloLens、HoloLens 2、Mixed Reality、開發、MRTK、
ms.openlocfilehash: 1a4edbad5e908f6402eb6c0acc8643009b8f82c4
ms.sourcegitcommit: 97815006c09be0a43b3d9b33c1674150cdfecf2b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101780409"
---
# <a name="lost-tracking-visualization"></a><span data-ttu-id="4c8dd-104">遺失追蹤視覺效果</span><span class="sxs-lookup"><span data-stu-id="4c8dd-104">Lost tracking visualization</span></span>

![遺失追蹤](Images/LostTracking/LostTrackingVisualization.jpg)

<span data-ttu-id="4c8dd-106">遺失追蹤延伸模組服務會為遺失的追蹤狀態提供 HoloLens shell 樣式的動畫視覺效果。</span><span class="sxs-lookup"><span data-stu-id="4c8dd-106">Lost Tracking Extension Service provides HoloLens shell style animated visual for the lost tracking state.</span></span>

## <a name="how-to-use-lost-tracking-extensions"></a><span data-ttu-id="4c8dd-107">如何使用遺失的追蹤延伸模組</span><span class="sxs-lookup"><span data-stu-id="4c8dd-107">How to use lost tracking extensions</span></span>

<span data-ttu-id="4c8dd-108">在 MRTK 設定檔中，將 **遺失的追蹤服務** 新增至延伸模組。</span><span class="sxs-lookup"><span data-stu-id="4c8dd-108">In MRTK Profile, add **Lost Tracking Service** to the Extensions.</span></span> <span data-ttu-id="4c8dd-109">指派包含 **LostTrackingVisualPrefab** 的 **DefaultLostTrackingServiceProfile** 。</span><span class="sxs-lookup"><span data-stu-id="4c8dd-109">Assign **DefaultLostTrackingServiceProfile** which includes **LostTrackingVisualPrefab**.</span></span>

<img src="Images/LostTracking/LostTracking_Extensions.png" width="550" alt="Lost Tracking Extensions">
