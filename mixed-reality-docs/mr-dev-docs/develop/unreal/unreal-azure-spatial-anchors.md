---
title: Unreal 中的 Azure Spatial Anchors
description: 在 Unreal Engine 中建立 Azure Spatial Anchors 的概觀。
author: hferrone
ms.author: v-hferrone
ms.date: 07/01/2020
ms.topic: tutorial
ms.localizationpriority: high
keywords: Unreal, Unreal Engine 4, UE4, HoloLens 2, azure, azure 開發, 空間錨點, 混合實境, 開發, 功能, 新專案, 模擬器, 文件, 指南, 全像投影, 遊戲開發, 混合實境頭戴式裝置, windows 混合實境頭戴式裝置, 虛擬實境頭戴式裝置
ms.openlocfilehash: b464292b606f6c375fe84a50867cac770cd8f001
ms.sourcegitcommit: 09522ab15a9008ca4d022f9e37fcc98f6eaf6093
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2020
ms.locfileid: "96354546"
---
# <a name="azure-spatial-anchors-in-unreal"></a><span data-ttu-id="be0ef-104">Unreal 中的 Azure Spatial Anchors</span><span class="sxs-lookup"><span data-stu-id="be0ef-104">Azure Spatial Anchors in Unreal</span></span>

## <a name="overview"></a><span data-ttu-id="be0ef-105">概觀</span><span class="sxs-lookup"><span data-stu-id="be0ef-105">Overview</span></span>

<span data-ttu-id="be0ef-106">Azure Spatial Anchors 是 Microsoft Mixed Reality 服務，可讓擴增實境裝置探索、共用及保存實體世界中的錨點。</span><span class="sxs-lookup"><span data-stu-id="be0ef-106">Azure Spatial Anchors is a Microsoft Mixed Reality service, allowing augmented reality devices to discover, share, and persist anchor points in the physical world.</span></span> <span data-ttu-id="be0ef-107">以下文件提供將 Azure Spatial Anchors 服務整合到 Unreal 專案中的指示。</span><span class="sxs-lookup"><span data-stu-id="be0ef-107">Documentation below provides instructions for integrating the Azure Spatial Anchors service into an Unreal project.</span></span> <span data-ttu-id="be0ef-108">如果您要尋找更多資訊，請參閱 [Azure Spatial Anchors 服務](https://azure.microsoft.com/services/spatial-anchors/)。</span><span class="sxs-lookup"><span data-stu-id="be0ef-108">If you're looking for more information, check out the [Azure Spatial Anchors service](https://azure.microsoft.com/services/spatial-anchors/).</span></span>

> [!NOTE]
> <span data-ttu-id="be0ef-109">如果您以 iOS 或 Android 作為目標，則 Unreal Engine 4.26 現在有適用於 ARKit 和 ARCore 支援的外掛程式。</span><span class="sxs-lookup"><span data-stu-id="be0ef-109">Unreal Engine 4.26 now has plugins for ARKit and ARCore support if you're targeting iOS or Android.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="be0ef-110">本機錨點會儲存在裝置上，而 Azure Spatial Anchors 會儲存在雲端。</span><span class="sxs-lookup"><span data-stu-id="be0ef-110">Local anchors are stored on device, while Azure Spatial Anchors are stored in the cloud.</span></span> <span data-ttu-id="be0ef-111">如果您要將錨點儲存在裝置本機上，我們有[本機空間錨點](unreal-spatial-anchors.md)文件，可協助您逐步完成此程序。</span><span class="sxs-lookup"><span data-stu-id="be0ef-111">If you're looking to store your anchors locally on a device, we have a [Local Spatial Anchors](unreal-spatial-anchors.md) document that can walk you through the process.</span></span> <span data-ttu-id="be0ef-112">請注意，您在同一個專案中可以有本機和 Azure 錨點，而不會發生衝突。</span><span class="sxs-lookup"><span data-stu-id="be0ef-112">Note that you can have local and Azure anchors in the same project without conflict.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="be0ef-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="be0ef-113">Prerequisites</span></span>

<span data-ttu-id="be0ef-114">若要完成本指南，請確定您：</span><span class="sxs-lookup"><span data-stu-id="be0ef-114">To complete this guide, make sure you have:</span></span>

- <span data-ttu-id="be0ef-115">已安裝 [Unreal 4.25 版](https://www.unrealengine.com/get-now)或更新版本</span><span class="sxs-lookup"><span data-stu-id="be0ef-115">Installed [Unreal version 4.25](https://www.unrealengine.com/get-now) or later</span></span>
- <span data-ttu-id="be0ef-116">在 Unreal 中設定了 [HoloLens 2 專案](tutorials/unreal-uxt-ch1.md)</span><span class="sxs-lookup"><span data-stu-id="be0ef-116">A [HoloLens 2 project](tutorials/unreal-uxt-ch1.md) setup in Unreal</span></span> 
- <span data-ttu-id="be0ef-117">請參閱 [Azure Spatial Anchors 概觀](https://docs.microsoft.com/azure/spatial-anchors/overview)</span><span class="sxs-lookup"><span data-stu-id="be0ef-117">Read through the [Azure Spatial Anchors overview](https://docs.microsoft.com/azure/spatial-anchors/overview)</span></span>
- <span data-ttu-id="be0ef-118">C++ 和 Unreal 的基本知識</span><span class="sxs-lookup"><span data-stu-id="be0ef-118">Basic knowledge on C++ and Unreal</span></span>

## <a name="getting-azure-spatial-anchors-account-info"></a><span data-ttu-id="be0ef-119">取得 Azure Spatial Anchors 帳戶資訊</span><span class="sxs-lookup"><span data-stu-id="be0ef-119">Getting Azure Spatial Anchors account info</span></span>

<span data-ttu-id="be0ef-120">在您的專案中使用 Azure Spatial Anchors 之前，您必須：</span><span class="sxs-lookup"><span data-stu-id="be0ef-120">Before using Azure Spatial Anchors in your project, you need to:</span></span>
* <span data-ttu-id="be0ef-121">[建立空間錨點資源](https://docs.microsoft.com/azure/spatial-anchors/quickstarts/get-started-hololens#create-a-spatial-anchors-resource)並複製下列帳戶欄位。</span><span class="sxs-lookup"><span data-stu-id="be0ef-121">[Create a spatial anchors resource](https://docs.microsoft.com/azure/spatial-anchors/quickstarts/get-started-hololens#create-a-spatial-anchors-resource) and copy the account fields listed below.</span></span> <span data-ttu-id="be0ef-122">這些值用來向您的應用程式帳戶驗證使用者：</span><span class="sxs-lookup"><span data-stu-id="be0ef-122">These values are used to authenticate users with your application's account:</span></span>
    * <span data-ttu-id="be0ef-123">**帳戶識別碼**</span><span class="sxs-lookup"><span data-stu-id="be0ef-123">**Account ID**</span></span>
    * <span data-ttu-id="be0ef-124">**帳戶金鑰**</span><span class="sxs-lookup"><span data-stu-id="be0ef-124">**Account Key**</span></span>

<span data-ttu-id="be0ef-125">如需詳細資訊，請參閱 [Azure Spatial Anchors 驗證](https://docs.microsoft.com/azure/spatial-anchors/concepts/authentication?tabs=csharp)文件。</span><span class="sxs-lookup"><span data-stu-id="be0ef-125">Check out the [Azure Spatial Anchors authentication](https://docs.microsoft.com/azure/spatial-anchors/concepts/authentication?tabs=csharp) docs for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="be0ef-126">Unreal 4.25 中的 Azure Spatial Anchors 不支援 Azure AD 驗證權杖，但這項功能的支援將會在後續版本中推出。</span><span class="sxs-lookup"><span data-stu-id="be0ef-126">Azure Spatial Anchors in Unreal 4.25 does not support Azure AD authentication tokens, but support for this functionality will be coming in a later release.</span></span>

## <a name="adding-azure-spatial-anchors-plugins"></a><span data-ttu-id="be0ef-127">新增 Azure Spatial Anchors 外掛程式</span><span class="sxs-lookup"><span data-stu-id="be0ef-127">Adding Azure Spatial Anchors plugins</span></span>

<span data-ttu-id="be0ef-128">在 Unreal 編輯器中啟用 Azure Spatial Anchors 外掛程式，方法如下：</span><span class="sxs-lookup"><span data-stu-id="be0ef-128">Enable the Azure Spatial Anchors plugins in the Unreal editor by:</span></span>
1. <span data-ttu-id="be0ef-129">按一下 [編輯] > [外掛程式] 並搜尋 **AzureSpatialAnchors** 和 **AzureSpatialAnchorsForWMR**。</span><span class="sxs-lookup"><span data-stu-id="be0ef-129">Clicking **Edit > Plugins** and searching for **AzureSpatialAnchors** and **AzureSpatialAnchorsForWMR**.</span></span>
2. <span data-ttu-id="be0ef-130">在兩個外掛程式中選取 [已啟用] 核取方塊，以允許存取您應用程式中的 Azure Spatial Anchors 藍圖程式庫。</span><span class="sxs-lookup"><span data-stu-id="be0ef-130">Select the **Enabled** checkbox in both plugins to allow access to the Azure Spatial Anchors blueprint libraries in your application.</span></span>

![空間錨點外掛程式](images/asa-unreal/unreal-spatial-anchors-img-01.png)

<span data-ttu-id="be0ef-132">完成後，請重新啟動 Unreal 編輯器，讓外掛程式變更生效。</span><span class="sxs-lookup"><span data-stu-id="be0ef-132">Once that's done, restart the Unreal Editor for the plugin changes to take effect.</span></span> <span data-ttu-id="be0ef-133">專案現在已準備好使用 Azure Spatial Anchors。</span><span class="sxs-lookup"><span data-stu-id="be0ef-133">The project is now ready to use Azure Spatial Anchors.</span></span>

## <a name="starting-a-spatial-anchors-session"></a><span data-ttu-id="be0ef-134">啟動空間錨點工作階段</span><span class="sxs-lookup"><span data-stu-id="be0ef-134">Starting a Spatial Anchors session</span></span>
<span data-ttu-id="be0ef-135">Azure Spatial Anchors 工作階段可讓用戶端應用程式與 Azure Spatial Anchors 服務進行通訊。</span><span class="sxs-lookup"><span data-stu-id="be0ef-135">An Azure Spatial Anchors session allows client applications to communicate with the Azure Spatial Anchors service.</span></span> <span data-ttu-id="be0ef-136">您必須建立並啟動 Azure Spatial Anchors 工作階段，才能建立、保存和共用 Azure Spatial Anchors：</span><span class="sxs-lookup"><span data-stu-id="be0ef-136">You'll need to create and start an Azure Spatial Anchors session to create, persist, and share Azure Spatial Anchors:</span></span>

1. <span data-ttu-id="be0ef-137">針對您在應用程式中使用的 Pawn 開啟藍圖。</span><span class="sxs-lookup"><span data-stu-id="be0ef-137">Open the blueprint for the Pawn you're using in the application.</span></span>
2. <span data-ttu-id="be0ef-138">為 [帳戶識別碼] 和 [帳戶金鑰] 新增兩個字串變數，然後從您的 Azure Spatial Anchors 帳戶指派對應的值，以驗證工作階段。</span><span class="sxs-lookup"><span data-stu-id="be0ef-138">Add two string variables for the **Account ID** and **Account Key**, then assign the corresponding values from your Azure Spatial Anchors account to authenticate the session.</span></span>

![空間錨點外掛程式](images/asa-unreal/unreal-spatial-anchors-img-02.png)

<span data-ttu-id="be0ef-140">啟動 Azure Spatial Anchors 工作階段，方法如下：</span><span class="sxs-lookup"><span data-stu-id="be0ef-140">Start an Azure Spatial Anchors session by:</span></span>
1. <span data-ttu-id="be0ef-141">檢查 [AR 工作階段] 是否正在 HoloLens 應用程式中執行，因為直到 AR 工作階段執行，才能啟動 Azure Spatial Anchors 工作階段。</span><span class="sxs-lookup"><span data-stu-id="be0ef-141">Checking that an **AR Session** is running in the HoloLens application, as the Azure Spatial Anchors session can't start until an AR Session is running.</span></span> <span data-ttu-id="be0ef-142">如果您尚未設定工作階段，請[建立 AR 工作階段資產](https://docs.microsoft.com/windows/mixed-reality/unreal-uxt-ch3#adding-the-session-asset)。</span><span class="sxs-lookup"><span data-stu-id="be0ef-142">If you don't have one setup, [create an AR Session asset](https://docs.microsoft.com/windows/mixed-reality/unreal-uxt-ch3#adding-the-session-asset).</span></span>
2. <span data-ttu-id="be0ef-143">新增 [啟動 Azure Spatial Anchors 工作階段] 自訂事件並加以設定，如下列螢幕擷取畫面所示。</span><span class="sxs-lookup"><span data-stu-id="be0ef-143">Adding the **Start Azure Spatial Anchors Session** custom event and configure it as shown in the screenshot below.</span></span>
    * <span data-ttu-id="be0ef-144">建立工作階段時，預設不會啟動工作階段，這可讓開發人員使用 Azure Spatial Anchors 服務來設定工作階段進行驗證。</span><span class="sxs-lookup"><span data-stu-id="be0ef-144">Creating a session doesn't start the session by default, which allows the developer to configure the session for authentication with the Azure Spatial Anchors service.</span></span>

![空間錨點外掛程式](images/asa-unreal/unreal-spatial-anchors-img-03.png)

3. <span data-ttu-id="be0ef-146">設定 Azure Spatial Anchors 工作階段，以提供 [帳戶識別碼] 和 [帳戶金鑰]。</span><span class="sxs-lookup"><span data-stu-id="be0ef-146">Configure the Azure Spatial Anchors session to provide the **Account ID** and **Account Key**.</span></span>

![空間錨點外掛程式](images/asa-unreal/unreal-spatial-anchors-img-04.png)

4. <span data-ttu-id="be0ef-148">啟動 Azure Spatial Anchors 工作階段，讓應用程式能夠建立及尋找 Azure Spatial Anchors。</span><span class="sxs-lookup"><span data-stu-id="be0ef-148">Start the Azure Spatial Anchors session, allowing the application to create and locate Azure Spatial Anchors.</span></span>

![空間錨點外掛程式](images/asa-unreal/unreal-spatial-anchors-img-05.png)

<span data-ttu-id="be0ef-150">當您不再使用該服務時，在事件圖形藍圖中清除 Azure Spatial Anchors 資源是很好的作法：</span><span class="sxs-lookup"><span data-stu-id="be0ef-150">It's good practice to clean up Azure Spatial Anchors resources in your Event Graph blueprint when you're no longer using the service:</span></span>

1. <span data-ttu-id="be0ef-151">停止 Azure Spatial Anchors 工作階段。</span><span class="sxs-lookup"><span data-stu-id="be0ef-151">Stop the Azure Spatial Anchors session.</span></span> <span data-ttu-id="be0ef-152">工作階段將不再執行，但其相關聯的資源仍會存在於 Azure Spatial Anchors 外掛程式中。</span><span class="sxs-lookup"><span data-stu-id="be0ef-152">The session will no longer be running, but its associated resources will still exist in the Azure Spatial Anchors plugin.</span></span>

![空間錨點外掛程式](images/asa-unreal/unreal-spatial-anchors-img-06.png)

2. <span data-ttu-id="be0ef-154">終結 Azure Spatial Anchors 工作階段，以清除 Azure Spatial Anchors 外掛程式仍知道的任何 Azure Spatial Anchors 工作階段資源。</span><span class="sxs-lookup"><span data-stu-id="be0ef-154">Destroy the Azure Spatial Anchors session to clean up any Azure Spatial Anchors session resources still known to the Azure Spatial Anchors plugin.</span></span>

![空間錨點外掛程式](images/asa-unreal/unreal-spatial-anchors-img-07.png)

<span data-ttu-id="be0ef-156">您的事件圖形藍圖看起來應該類似下列螢幕擷取畫面：</span><span class="sxs-lookup"><span data-stu-id="be0ef-156">Your Event Graph blueprint should look like the screenshot below:</span></span>

![空間錨點外掛程式](images/asa-unreal/unreal-spatial-anchors-img-08.png)


## <a name="creating-an-anchor"></a><span data-ttu-id="be0ef-158">建立錨點</span><span class="sxs-lookup"><span data-stu-id="be0ef-158">Creating an anchor</span></span>
<span data-ttu-id="be0ef-159">Azure Spatial Anchors 代表擴增實境應用程式空間中的實體世界姿態，其會將擴增實境內容鎖定至實體世界中的位置。</span><span class="sxs-lookup"><span data-stu-id="be0ef-159">An Azure Spatial Anchor represents a physical world pose in the augmented reality application space, which locks augmented reality content to locations in the physical world.</span></span> <span data-ttu-id="be0ef-160">Azure Spatial Anchors 也可以在不同的使用者間共用。</span><span class="sxs-lookup"><span data-stu-id="be0ef-160">Azure Spatial Anchors can also be shared among different users.</span></span> <span data-ttu-id="be0ef-161">此共用可讓在不同裝置上繪製的擴增實境內容放置於實體世界中的相同位置。</span><span class="sxs-lookup"><span data-stu-id="be0ef-161">This sharing allows augmented reality content drawn on different devices to be positioned in the same location in the physical world.</span></span> 

<span data-ttu-id="be0ef-162">若要建立新的 Azure Spatial Anchors：</span><span class="sxs-lookup"><span data-stu-id="be0ef-162">To create a new Azure Spatial Anchor:</span></span>
1. <span data-ttu-id="be0ef-163">檢查 Azure Spatial Anchors 工作階段是否正在執行。</span><span class="sxs-lookup"><span data-stu-id="be0ef-163">Check that an Azure Spatial Anchors session is running.</span></span> <span data-ttu-id="be0ef-164">若沒有任何正在執行的 Azure Spatial Anchors 工作階段，應用程式就無法建立或保存 Azure Spatial Anchors。</span><span class="sxs-lookup"><span data-stu-id="be0ef-164">The application can't create or persist an Azure Spatial Anchor when no Azure Spatial Anchors session is running.</span></span>

![空間錨點外掛程式](images/asa-unreal/unreal-spatial-anchors-img-09.png)

2. <span data-ttu-id="be0ef-166">建立或取得應保存其位置的 Unreal **[場景元件](https://docs.unrealengine.com/API/Runtime/Engine/Components/USceneComponent/index.html)** 。</span><span class="sxs-lookup"><span data-stu-id="be0ef-166">Create or obtain an Unreal **[Scene Component](https://docs.unrealengine.com/API/Runtime/Engine/Components/USceneComponent/index.html)** that should have its location persisted.</span></span> 
    * <span data-ttu-id="be0ef-167">在下圖中，[需要錨點的場景元件] 元件會當作變數使用。</span><span class="sxs-lookup"><span data-stu-id="be0ef-167">In the below image, the **Scene Component Needing Anchor** component is used as a variable.</span></span> <span data-ttu-id="be0ef-168">需要 Unreal 場景元件才能建立 [AR Pin](https://docs.unrealengine.com/BlueprintAPI/HoloLensAR/ARPin/index.html) 和 Azure Spatial Anchors 的應用程式世界轉換。</span><span class="sxs-lookup"><span data-stu-id="be0ef-168">An Unreal Scene Component is needed to establish an application world transform for an [AR Pin](https://docs.unrealengine.com/BlueprintAPI/HoloLensAR/ARPin/index.html) and Azure Spatial Anchor.</span></span>

![空間錨點外掛程式](images/asa-unreal/unreal-spatial-anchors-img-10.png)

<span data-ttu-id="be0ef-170">若要為 Unreal 場景元件建構並儲存 Azure Spatial Anchors：</span><span class="sxs-lookup"><span data-stu-id="be0ef-170">To construct and save an Azure Spatial Anchor for an Unreal Scene Component:</span></span>
1. <span data-ttu-id="be0ef-171">針對 Unreal 場景元件呼叫 [Pin 元件](https://docs.unrealengine.com/BlueprintAPI/ARAugmentedReality/Pin/PinComponent/index.html)，並指定場景元件的 [世界轉換] 作為用於 AR Pin 的世界轉換。</span><span class="sxs-lookup"><span data-stu-id="be0ef-171">Call the [Pin Component](https://docs.unrealengine.com/BlueprintAPI/ARAugmentedReality/Pin/PinComponent/index.html) for the Unreal Scene Component and specify the Scene Component's **World Transform** as the World Transform used for the AR Pin.</span></span>
    * <span data-ttu-id="be0ef-172">Unreal 會使用 AR Pin 來追蹤應用程式空間中的 AR 點，其用來建立 Azure Spatial Anchors。</span><span class="sxs-lookup"><span data-stu-id="be0ef-172">Unreal tracks AR points in the application space using AR Pins, which are used to create an Azure Spatial Anchor.</span></span> <span data-ttu-id="be0ef-173">在 Unreal 中，AR Pin 類似於 HoloLens 上的空間錨點。</span><span class="sxs-lookup"><span data-stu-id="be0ef-173">In Unreal, an AR Pin is analogous to a SpatialAnchor on HoloLens.</span></span>

![空間錨點外掛程式](images/asa-unreal/unreal-spatial-anchors-img-11.png)

2. <span data-ttu-id="be0ef-175">使用新建立的 AR Pin 呼叫 [建立雲端錨點]。</span><span class="sxs-lookup"><span data-stu-id="be0ef-175">Call **Create Cloud Anchor** using the newly created AR Pin.</span></span>
    * <span data-ttu-id="be0ef-176">「建立雲端錨點」會在本機建立 Azure Spatial Anchors，但不在 Azure Spatial Anchors 服務中。</span><span class="sxs-lookup"><span data-stu-id="be0ef-176">Create Cloud Anchor creates an Azure Spatial Anchor locally but not in the Azure Spatial Anchor service.</span></span> <span data-ttu-id="be0ef-177">在透過服務建立 Azure Spatial Anchors 之前，可以設定 Azure Spatial Anchors 的參數 (例如到期日)。</span><span class="sxs-lookup"><span data-stu-id="be0ef-177">Parameters for the Azure Spatial Anchor, such as an expiration date, can be set before creating the Azure Spatial Anchor with the service.</span></span>

![空間錨點外掛程式](images/asa-unreal/unreal-spatial-anchors-img-12.png)

3. <span data-ttu-id="be0ef-179">設定 Azure Spatial Anchors 到期。</span><span class="sxs-lookup"><span data-stu-id="be0ef-179">Set the Azure Spatial Anchor expiration.</span></span> <span data-ttu-id="be0ef-180">此函式的 Lifetime 參數可讓開發人員以秒為單位指定服務應維護錨點的時間長度。</span><span class="sxs-lookup"><span data-stu-id="be0ef-180">This function's Lifetime parameter allows the developer to specify in seconds how long the anchor should be maintained by the service.</span></span>
    * <span data-ttu-id="be0ef-181">例如，為期一週的到期日會採用 60秒 x 60 分鐘 x 24 小時 x 7 天 = 604,800 秒的值。</span><span class="sxs-lookup"><span data-stu-id="be0ef-181">For example, a week long expiration would take a value of 60 seconds x 60 minutes x 24 hours x seven days = 604,800 seconds.</span></span>

![空間錨點外掛程式](images/asa-unreal/unreal-spatial-anchors-img-13.png)

<span data-ttu-id="be0ef-183">設定錨點參數之後，請將錨點宣告為準備好儲存。</span><span class="sxs-lookup"><span data-stu-id="be0ef-183">After setting anchor parameters, declare the anchor as ready to save.</span></span> <span data-ttu-id="be0ef-184">在下列範例中，新建立的 Azure Spatial Anchors 會新增至需要儲存的一組 Azure Spatial Anchors。</span><span class="sxs-lookup"><span data-stu-id="be0ef-184">In the example below, the newly created Azure Spatial Anchor is added to a set of Azure Spatial Anchors needing saving.</span></span> <span data-ttu-id="be0ef-185">此集合會宣告為 Pawn 藍圖的變數。</span><span class="sxs-lookup"><span data-stu-id="be0ef-185">This set is declared as a variable for the Pawn blueprint.</span></span>

![空間錨點外掛程式](images/asa-unreal/unreal-spatial-anchors-img-14.png)

## <a name="saving-an-anchor"></a><span data-ttu-id="be0ef-187">儲存錨點</span><span class="sxs-lookup"><span data-stu-id="be0ef-187">Saving an Anchor</span></span>

<span data-ttu-id="be0ef-188">使用您的參數設定 Azure Spatial Anchors 之後，請呼叫 [儲存雲端錨點]。</span><span class="sxs-lookup"><span data-stu-id="be0ef-188">After configuring the Azure Spatial Anchor with your parameters, call **Save Cloud Anchor**.</span></span> <span data-ttu-id="be0ef-189">「儲存雲端錨點」會對 Azure Spatial Anchors 服務宣告錨點。</span><span class="sxs-lookup"><span data-stu-id="be0ef-189">Save Cloud Anchor declares the anchor to the Azure Spatial Anchors service.</span></span> <span data-ttu-id="be0ef-190">當「儲存雲端錨點」的呼叫成功時，Azure Spatial Anchors 可供 Azure Spatial Anchors 服務的其他使用者使用。</span><span class="sxs-lookup"><span data-stu-id="be0ef-190">When the call to Save Cloud Anchor succeeds, the Azure Spatial Anchor is available to other users of the Azure Spatial Anchor service.</span></span>  

![空間錨點外掛程式](images/asa-unreal/unreal-spatial-anchors-img-15.png)

> [!NOTE]
> <span data-ttu-id="be0ef-192">「儲存雲端錨點」是非同步函式，只能在遊戲執行緒事件 (例如 **EventTick**) 上呼叫。</span><span class="sxs-lookup"><span data-stu-id="be0ef-192">Save Cloud Anchor is an asynchronous function and can only be called on a game thread event such as **EventTick**.</span></span> <span data-ttu-id="be0ef-193">「儲存雲端錨點」可能不會顯示為自訂藍圖函式中可用的藍圖函式。</span><span class="sxs-lookup"><span data-stu-id="be0ef-193">Save Cloud Anchor may not appear as an available blueprint function in custom blueprint Functions.</span></span> <span data-ttu-id="be0ef-194">不過，其應可在 Pawn 事件圖形藍圖編輯器中使用。</span><span class="sxs-lookup"><span data-stu-id="be0ef-194">However, it should be available in the Pawn Event Graph blueprint editor.</span></span>

<span data-ttu-id="be0ef-195">在下列範例中，Azure Spatial Anchors 會在輸入事件回呼期間儲存在集合中。</span><span class="sxs-lookup"><span data-stu-id="be0ef-195">In the example below, the Azure Spatial Anchor is stored in a set during an input event callback.</span></span> <span data-ttu-id="be0ef-196">然後錨點會儲存在 EventTick 上。</span><span class="sxs-lookup"><span data-stu-id="be0ef-196">The anchor is then saved on the EventTick.</span></span> <span data-ttu-id="be0ef-197">視您的 Azure Spatial Anchors 工作階段所建立的空間資料量而定，儲存 Azure Spatial Anchors 可能會進行多次嘗試。</span><span class="sxs-lookup"><span data-stu-id="be0ef-197">Saving an Azure Spatial Anchor may take multiple attempts depending on the amount of spatial data that your Azure Spatial Anchors session has created.</span></span> <span data-ttu-id="be0ef-198">這就是為何檢查儲存呼叫是否成功是個好主意。</span><span class="sxs-lookup"><span data-stu-id="be0ef-198">That's why it's a good idea to check whether the save call succeeded.</span></span>

<span data-ttu-id="be0ef-199">若未儲存錨點，請將其重新新增至仍需要儲存的錨點集合。</span><span class="sxs-lookup"><span data-stu-id="be0ef-199">If the anchor doesn't save, re-add it to the set of anchors still needing to be saved.</span></span> <span data-ttu-id="be0ef-200">未來的 EventTick 會嘗試儲存錨點，直到其成功儲存在 Azure Spatial Anchors 服務為止。</span><span class="sxs-lookup"><span data-stu-id="be0ef-200">Future EventTicks will try to save the anchor until it's successfully stored in the Azure Spatial Anchor service.</span></span>

![空間錨點外掛程式](images/asa-unreal/unreal-spatial-anchors-img-16.png)

<span data-ttu-id="be0ef-202">儲存錨點之後，您可使用 AR Pin 的轉換作為參考轉換，以便將內容放在應用程式中。</span><span class="sxs-lookup"><span data-stu-id="be0ef-202">Once the anchor saves, you can use the AR Pins' transform as a reference transform for placing content in the application.</span></span> <span data-ttu-id="be0ef-203">其他使用者可以偵測此錨點，並針對實體世界中的不同裝置對齊 AR 內容。</span><span class="sxs-lookup"><span data-stu-id="be0ef-203">Other users can detect this anchor and align AR content for different devices in the physical world.</span></span>

## <a name="deleting-an-anchor"></a><span data-ttu-id="be0ef-204">刪除錨點</span><span class="sxs-lookup"><span data-stu-id="be0ef-204">Deleting an Anchor</span></span>

<span data-ttu-id="be0ef-205">您可藉由呼叫 [刪除雲端錨點]，從 Azure Spatial Anchors 服務刪除錨點。</span><span class="sxs-lookup"><span data-stu-id="be0ef-205">You can delete anchors from the Azure Spatial Anchor service by calling **Delete Cloud Anchor**.</span></span>

![空間錨點外掛程式](images/asa-unreal/unreal-spatial-anchors-img-17.png)

> [!NOTE]
> <span data-ttu-id="be0ef-207">「刪除雲端錨點」是潛伏的函式，只能在遊戲執行緒事件 (例如 EventTick) 上呼叫。</span><span class="sxs-lookup"><span data-stu-id="be0ef-207">Delete Cloud Anchor is a latent function and can only be called on a game thread event, such as EventTick.</span></span> <span data-ttu-id="be0ef-208">「刪除雲端錨點」可能不會顯示為自訂藍圖函式中可用的藍圖函式。</span><span class="sxs-lookup"><span data-stu-id="be0ef-208">Delete Cloud Anchor may not appear as an available blueprint function in custom blueprint Functions.</span></span> <span data-ttu-id="be0ef-209">不過，其應可在 Pawn 事件圖形藍圖編輯器中使用。</span><span class="sxs-lookup"><span data-stu-id="be0ef-209">It should however be available in the Pawn Event Graph blueprint editor.</span></span>

<span data-ttu-id="be0ef-210">在下列範例中，錨點會在自訂輸入事件上標示要刪除。</span><span class="sxs-lookup"><span data-stu-id="be0ef-210">In the example below, the anchor is flagged for deletion on a custom input event.</span></span> <span data-ttu-id="be0ef-211">然後會在 EventTick 上嘗試刪除。</span><span class="sxs-lookup"><span data-stu-id="be0ef-211">The deletion is then attempted on the EventTick.</span></span> <span data-ttu-id="be0ef-212">如果錨點刪除失敗，請將 Azure Spatial Anchors 新增至標示要刪除的錨點集合，然後在稍後的 EventTick 上再次嘗試。</span><span class="sxs-lookup"><span data-stu-id="be0ef-212">If the anchor deletion fails, add the Azure Spatial Anchor to the set of anchors flagged for deletion and tries again on later EventTicks.</span></span>

<span data-ttu-id="be0ef-213">您的事件圖形藍圖現在看起來應該類似下列螢幕擷取畫面：</span><span class="sxs-lookup"><span data-stu-id="be0ef-213">Your Event Graph blueprint should now look like the screenshot below:</span></span>

![空間錨點外掛程式](images/asa-unreal/unreal-spatial-anchors-img-18.png)


## <a name="locating-pre-existing-anchors"></a><span data-ttu-id="be0ef-215">尋找既有的錨點</span><span class="sxs-lookup"><span data-stu-id="be0ef-215">Locating pre-existing anchors</span></span>

<span data-ttu-id="be0ef-216">除了建立 Azure Spatial Anchors，您還可使用 Azure Spatial Anchors 服務來偵測對等節點所建立的錨點：</span><span class="sxs-lookup"><span data-stu-id="be0ef-216">In addition to creating Azure Spatial Anchors, you can detect anchors created by peers with the Azure Spatial Anchors service:</span></span>

1. <span data-ttu-id="be0ef-217">針對您想要偵測的錨點，取得 Azure Spatial Anchors 識別碼。</span><span class="sxs-lookup"><span data-stu-id="be0ef-217">Obtain an Azure Spatial Anchor identifier for the anchor that you would like to detect.</span></span>
    * <span data-ttu-id="be0ef-218">在先前的 Azure Spatial Anchors 工作階段中，可以為相同裝置所建立的錨點取得錨點識別碼。</span><span class="sxs-lookup"><span data-stu-id="be0ef-218">An anchor identifier can be obtained for an anchor created by the same device in a previous Azure Spatial Anchors session.</span></span> <span data-ttu-id="be0ef-219">其也可透過與 Azure Spatial Anchors 服務互動的對等裝置來建立及共用。</span><span class="sxs-lookup"><span data-stu-id="be0ef-219">It can also be created and shared by peer devices interacting with the Azure Spatial Anchors service.</span></span>

![空間錨點外掛程式](images/asa-unreal/unreal-spatial-anchors-img-24.png)

2. <span data-ttu-id="be0ef-221">將 **AzureSpatialAnchorsEvent** 元件新增至您的 Pawn 藍圖。</span><span class="sxs-lookup"><span data-stu-id="be0ef-221">Add an **AzureSpatialAnchorsEvent** component to your Pawn blueprint.</span></span>
    * <span data-ttu-id="be0ef-222">此元件可讓您訂閱各種 Azure Spatial Anchors 事件，例如找到 Azure Spatial Anchors 時所呼叫的事件。</span><span class="sxs-lookup"><span data-stu-id="be0ef-222">This component allows you to subscribe to various Azure Spatial Anchors events, such as events called when Azure Spatial Anchors are located.</span></span>

![空間錨點外掛程式](images/asa-unreal/unreal-spatial-anchors-img-19.png)

3. <span data-ttu-id="be0ef-224">針對 [AzureSpatialAnchorsEvent] 元件訂閱 [ASAAnchor 找出的委派]。</span><span class="sxs-lookup"><span data-stu-id="be0ef-224">Subscribe to the **ASAAnchor Located Delegate** for the **AzureSpatialAnchorsEvent** component.</span></span>
    * <span data-ttu-id="be0ef-225">委派可讓應用程式知道何時找到與 Azure Spatial Anchors 帳戶相關聯的新錨點。</span><span class="sxs-lookup"><span data-stu-id="be0ef-225">The delegate lets the application know when new anchors associated with the Azure Spatial Anchors account have been located.</span></span>
    * <span data-ttu-id="be0ef-226">透過事件回呼，使用 Azure Spatial Anchors 工作階段所建立對等節點的 Azure Spatial Anchors，預設不會建立 AR Pin。</span><span class="sxs-lookup"><span data-stu-id="be0ef-226">With the event callback, Azure Spatial Anchors created by peers using the Azure Spatial Anchors session won't have AR Pins created by default.</span></span> <span data-ttu-id="be0ef-227">若要為偵測到的 Azure Spatial Anchors 建立 AR Pin，開發人員可以呼叫 [建立以 Azure Cloud Spatial Anchors 為主的 ARPin]。</span><span class="sxs-lookup"><span data-stu-id="be0ef-227">To create an AR Pin for the detected Azure Spatial Anchor, developers can call **Create ARPin Around Azure Cloud Spatial Anchor**.</span></span>

![空間錨點外掛程式](images/asa-unreal/unreal-spatial-anchors-img-20.png)

<span data-ttu-id="be0ef-229">為了使用 Azure Spatial Anchors 服務找出對等節點所建立的 Azure Spatial Anchors，應用程式必須建立 [Azure Spatial Anchors 監看員]：</span><span class="sxs-lookup"><span data-stu-id="be0ef-229">In order to locate Azure Spatial Anchors created by peers using the Azure Spatial Anchor service, the application will have to create an **Azure Spatial Anchors Watcher**:</span></span>
1. <span data-ttu-id="be0ef-230">檢查 Azure Spatial Anchors 工作階段是否正在執行。</span><span class="sxs-lookup"><span data-stu-id="be0ef-230">Check that an Azure Spatial Anchors session is running.</span></span>
2. <span data-ttu-id="be0ef-231">建立 **AzureSpatialAnchorsLocateCriteria**。</span><span class="sxs-lookup"><span data-stu-id="be0ef-231">Create an **AzureSpatialAnchorsLocateCriteria**.</span></span>
    * <span data-ttu-id="be0ef-232">您可以指定各種位置參數，例如與使用者的距離或與另一個錨點的距離。</span><span class="sxs-lookup"><span data-stu-id="be0ef-232">You can specify various location parameters like distance from the user or distance from another anchor.</span></span>
3. <span data-ttu-id="be0ef-233">在 **AzureSpatialAnchorsLocateCritieria** 中宣告您所需的 Azure Spatial Anchor 識別碼。</span><span class="sxs-lookup"><span data-stu-id="be0ef-233">Declare your desired Azure Spatial Anchor identifier in the **AzureSpatialAnchorsLocateCritieria**.</span></span>
4. <span data-ttu-id="be0ef-234">呼叫 [建立監看員]。</span><span class="sxs-lookup"><span data-stu-id="be0ef-234">Call **Create Watcher**.</span></span>

![空間錨點外掛程式](images/asa-unreal/unreal-spatial-anchors-img-21.png)

<span data-ttu-id="be0ef-236">應用程式現在會開始尋找 Azure Spatial Anchors 服務已知的 Azure Spatial Anchors，這表示使用者可以找出其對等節點所建立的 Azure Spatial Anchors。</span><span class="sxs-lookup"><span data-stu-id="be0ef-236">The application now begins looking for Azure Spatial Anchors known to the Azure Spatial Anchors service, meaning that users can locate Azure Spatial Anchors created by their peers.</span></span>

<span data-ttu-id="be0ef-237">找出 Azure Spatial Anchor 之後，請呼叫 [停止監看員] 以停止 Azure Spatial Anchors 監看員並清除監看員資源。</span><span class="sxs-lookup"><span data-stu-id="be0ef-237">After locating the Azure Spatial Anchor, call **Stop Watcher** to stop the Azure Spatial Anchors Watcher and clean up watcher resources.</span></span>

![空間錨點外掛程式](images/asa-unreal/unreal-spatial-anchors-img-22.png)

<span data-ttu-id="be0ef-239">您的最終事件圖形藍圖現在看起來應該類似下列螢幕擷取畫面：</span><span class="sxs-lookup"><span data-stu-id="be0ef-239">Your final Event Graph blueprint should now look like the screenshot below:</span></span>

![空間錨點外掛程式](images/asa-unreal/unreal-spatial-anchors-img-23.png)

## <a name="next-development-checkpoint"></a><span data-ttu-id="be0ef-241">下一個開發檢查點</span><span class="sxs-lookup"><span data-stu-id="be0ef-241">Next Development Checkpoint</span></span>

<span data-ttu-id="be0ef-242">依循我們配置的 Unreal 開發檢查點旅程，此時您會探索 MRTK核心建置組塊。</span><span class="sxs-lookup"><span data-stu-id="be0ef-242">If you're following the Unreal development checkpoint journey we've laid out, you're in the midst of exploring the MRTK core building blocks.</span></span> <span data-ttu-id="be0ef-243">接下來，您可以繼續進行下一個建置組塊：</span><span class="sxs-lookup"><span data-stu-id="be0ef-243">From here, you can proceed to the next building block:</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="be0ef-244">空間對應</span><span class="sxs-lookup"><span data-stu-id="be0ef-244">Spatial mapping</span></span>](unreal-spatial-mapping.md)

<span data-ttu-id="be0ef-245">或者，直接跳到混合實境平台功能和 API 的主題：</span><span class="sxs-lookup"><span data-stu-id="be0ef-245">Or jump to Mixed Reality platform capabilities and APIs:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="be0ef-246">HoloLens 相機</span><span class="sxs-lookup"><span data-stu-id="be0ef-246">HoloLens camera</span></span>](unreal-hololens-camera.md)

<span data-ttu-id="be0ef-247">您可以隨時回到 [Unreal 開發檢查點](unreal-development-overview.md#2-core-building-blocks)。</span><span class="sxs-lookup"><span data-stu-id="be0ef-247">You can always go back to the [Unreal development checkpoints](unreal-development-overview.md#2-core-building-blocks) at any time.</span></span>


## <a name="next-steps"></a><span data-ttu-id="be0ef-248">接下來的步驟</span><span class="sxs-lookup"><span data-stu-id="be0ef-248">Next steps</span></span>
* [<span data-ttu-id="be0ef-249">本機空間錨點</span><span class="sxs-lookup"><span data-stu-id="be0ef-249">Local Spatial Anchors</span></span>](unreal-spatial-anchors.md)
* [<span data-ttu-id="be0ef-250">空間錨點文件</span><span class="sxs-lookup"><span data-stu-id="be0ef-250">Spatial Anchors documentation</span></span>](https://docs.microsoft.com/azure/spatial-anchors/)
* [<span data-ttu-id="be0ef-251">空間錨點功能</span><span class="sxs-lookup"><span data-stu-id="be0ef-251">Spatial Anchor features</span></span>](https://azure.microsoft.com/services/spatial-anchors/#features)
* [<span data-ttu-id="be0ef-252">有效的錨點體驗指導方針</span><span class="sxs-lookup"><span data-stu-id="be0ef-252">Effective anchor experience guidelines</span></span>](https://docs.microsoft.com/azure/spatial-anchors/concepts/guidelines-effective-anchor-experiences)