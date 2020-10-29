---
title: DirectX 中的本機錨點傳輸
description: 說明如何藉由傳輸空間錨點來同步處理兩個 HoloLens 裝置。
author: mikeriches
ms.author: mriches
ms.date: 03/21/2018
ms.topic: article
keywords: HoloLens，同步處理，空間錨點，傳輸，多人遊戲，視圖，案例，逐步解說，範例程式碼，傳輸，本機錨點傳輸，錨點匯出，錨點匯入
ms.openlocfilehash: 6d54b29a01617f9d78b7fdfec0ebc04a3cd48002
ms.sourcegitcommit: 09599b4034be825e4536eeb9566968afd021d5f3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/03/2020
ms.locfileid: "91679657"
---
# <a name="local-anchor-transfers-in-directx"></a><span data-ttu-id="3839e-104">DirectX 中的本機錨點傳輸</span><span class="sxs-lookup"><span data-stu-id="3839e-104">Local anchor transfers in DirectX</span></span>

<span data-ttu-id="3839e-105">在您無法使用 <a href="https://docs.microsoft.com/azure/spatial-anchors" target="_blank">Azure 空間錨點</a>的情況下，本機錨點轉移可讓一部 hololens 裝置匯出錨點，以供第二個 hololens 裝置匯入。</span><span class="sxs-lookup"><span data-stu-id="3839e-105">In situations where you cannot use <a href="https://docs.microsoft.com/azure/spatial-anchors" target="_blank">Azure Spatial Anchors</a>, local anchor transfers enable one HoloLens device to export an anchor to be imported by a second HoloLens device.</span></span>

>[!NOTE]
><span data-ttu-id="3839e-106">本機錨點傳輸可提供比 <a href="https://docs.microsoft.com/azure/spatial-anchors" target="_blank">Azure 空間錨點</a>更不健全的錨定回收，而且此方法不支援 IOS 和 Android 裝置。</span><span class="sxs-lookup"><span data-stu-id="3839e-106">Local anchor transfers provide less robust anchor recall than <a href="https://docs.microsoft.com/azure/spatial-anchors" target="_blank">Azure Spatial Anchors</a>, and iOS and Android devices are not supported by this approach.</span></span>

>[!NOTE]
><span data-ttu-id="3839e-107">本文中的程式碼片段目前示範如何使用 c + + [/cx，而](../develop/native/creating-a-holographic-directx-project.md)不是 c + + 全像 c + + 全像 c + + 的 + 17 相容 c + +/WinRT。</span><span class="sxs-lookup"><span data-stu-id="3839e-107">The code snippets in this article currently demonstrate use of C++/CX rather than C++17-compliant C++/WinRT as used in the [C++ holographic project template](../develop/native/creating-a-holographic-directx-project.md).</span></span>  <span data-ttu-id="3839e-108">這些概念對 c + +/WinRT 專案而言是相等的，不過您必須轉譯程式碼。</span><span class="sxs-lookup"><span data-stu-id="3839e-108">The concepts are equivalent for a C++/WinRT project, though you will need to translate the code.</span></span>

## <a name="transferring-spatial-anchors"></a><span data-ttu-id="3839e-109">傳輸空間錨點</span><span class="sxs-lookup"><span data-stu-id="3839e-109">Transferring spatial anchors</span></span>

<span data-ttu-id="3839e-110">您可以使用 [SpatialAnchorTransferManager](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchortransfermanager.aspx)，在 Windows Mixed Reality 裝置之間傳輸空間錨點。</span><span class="sxs-lookup"><span data-stu-id="3839e-110">You can transfer spatial anchors between Windows Mixed Reality devices by using the [SpatialAnchorTransferManager](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchortransfermanager.aspx).</span></span> <span data-ttu-id="3839e-111">此 API 可讓您將錨點與所有所需的支援感應器資料組合在一起，以找出世界中確切的位置，然後在另一個裝置上匯入該組合。</span><span class="sxs-lookup"><span data-stu-id="3839e-111">This API lets you bundle up an anchor with all the supporting sensor data needed to find that exact place in the world, and then import that bundle on another device.</span></span> <span data-ttu-id="3839e-112">當第二部裝置上的應用程式匯入該錨點之後，每個應用程式都可以使用該共用空間錨點的座標系統轉譯影像，然後在真實世界中出現在相同的位置。</span><span class="sxs-lookup"><span data-stu-id="3839e-112">Once the app on the second device has imported that anchor, each app can render holograms using that shared spatial anchor's coordinate system, which will then appear in the same place in the real world.</span></span>

<span data-ttu-id="3839e-113">請注意，空間錨點無法在不同的裝置類型之間傳輸，例如，可能無法使用沉浸式耳機來定位 HoloLens 空間錨點。</span><span class="sxs-lookup"><span data-stu-id="3839e-113">Note that spatial anchors are not able to transfer between different device types, for example a HoloLens spatial anchor may not be locatable using an immersive headset.</span></span>  <span data-ttu-id="3839e-114">轉移的錨點也與 iOS 或 Android 裝置不相容。</span><span class="sxs-lookup"><span data-stu-id="3839e-114">Transferred anchors are also not compatible with iOS or Android devices.</span></span>

## <a name="set-up-your-app-to-use-the-spatialperception-capability"></a><span data-ttu-id="3839e-115">設定您的應用程式以使用 >spatialperception 功能</span><span class="sxs-lookup"><span data-stu-id="3839e-115">Set up your app to use the spatialPerception capability</span></span>

<span data-ttu-id="3839e-116">您的應用程式必須先獲得使用 >spatialperception 功能的許可權，才能使用 [SpatialAnchorTransferManager](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchortransfermanager.aspx)。</span><span class="sxs-lookup"><span data-stu-id="3839e-116">Your app must be granted permission to use the spatialPerception capability before it can use the [SpatialAnchorTransferManager](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchortransfermanager.aspx).</span></span> <span data-ttu-id="3839e-117">這是必要的，因為傳輸空間錨點牽涉到共用一段時間內所搜集的感應器映射（該錨點可能包含機密資訊）。</span><span class="sxs-lookup"><span data-stu-id="3839e-117">This is necessary because transferring a spatial anchor involves sharing sensor images gathered over time in vicinity of that anchor, which might include sensitive information.</span></span>

<span data-ttu-id="3839e-118">在您應用程式的 package.appxmanifest 檔案中宣告這項功能。</span><span class="sxs-lookup"><span data-stu-id="3839e-118">Declare this capability in the package.appxmanifest file for your app.</span></span> <span data-ttu-id="3839e-119">以下是範例：</span><span class="sxs-lookup"><span data-stu-id="3839e-119">Here's an example:</span></span>

```
<Capabilities>
  <uap2:Capability Name="spatialPerception" />
</Capabilities>
```

<span data-ttu-id="3839e-120">這項功能來自 **uap2** 命名空間。</span><span class="sxs-lookup"><span data-stu-id="3839e-120">The capability comes from the **uap2** namespace.</span></span> <span data-ttu-id="3839e-121">若要在您的資訊清單中取得這個命名空間的存取權，請將它包含在封裝> 專案中的 *xlmns* 屬性 &lt; 。</span><span class="sxs-lookup"><span data-stu-id="3839e-121">To get access to this namespace in your manifest, include it as an *xlmns* attribute in the &lt;Package> element.</span></span> <span data-ttu-id="3839e-122">以下是範例：</span><span class="sxs-lookup"><span data-stu-id="3839e-122">Here's an example:</span></span>

```
<Package
    xmlns="https://schemas.microsoft.com/appx/manifest/foundation/windows10"
    xmlns:mp="https://schemas.microsoft.com/appx/2014/phone/manifest"
    xmlns:uap="https://schemas.microsoft.com/appx/manifest/uap/windows10"
    xmlns:uap2="https://schemas.microsoft.com/appx/manifest/uap/windows10/2"
    IgnorableNamespaces="uap mp"
    >
```

<span data-ttu-id="3839e-123">**注意：** 您的應用程式必須在執行時間要求功能，才可存取 SpatialAnchor 匯出/匯入 Api。</span><span class="sxs-lookup"><span data-stu-id="3839e-123">**NOTE:** Your app will need to request the capability at runtime before it can access SpatialAnchor export/import APIs.</span></span> <span data-ttu-id="3839e-124">請參閱下列範例中的 [RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchortransfermanager.requestaccessasync.aspx) 。</span><span class="sxs-lookup"><span data-stu-id="3839e-124">See [RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchortransfermanager.requestaccessasync.aspx) in the examples below.</span></span>

## <a name="serialize-anchor-data-by-exporting-it-with-the-spatialanchortransfermanager"></a><span data-ttu-id="3839e-125">使用 SpatialAnchorTransferManager 匯出錨點資料，以將其序列化</span><span class="sxs-lookup"><span data-stu-id="3839e-125">Serialize anchor data by exporting it with the SpatialAnchorTransferManager</span></span>

<span data-ttu-id="3839e-126">Helper 函式包含在程式碼範例中，以匯出 (序列化) [SpatialAnchor](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchor.aspx) 資料。</span><span class="sxs-lookup"><span data-stu-id="3839e-126">A helper function is included in the code sample to export (serialize) [SpatialAnchor](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchor.aspx) data.</span></span> <span data-ttu-id="3839e-127">此匯出 API 會將索引鍵/值組集合中的所有錨點序列化，使字串與錨點產生關聯。</span><span class="sxs-lookup"><span data-stu-id="3839e-127">This export API serializes all anchors in a collection of key-value pairs associating strings with anchors.</span></span>

```
// ExportAnchorDataAsync: Exports a byte buffer containing all of the anchors in the given collection.
//
// This function will place data in a buffer using a std::vector<byte>. The ata buffer contains one or more
// Anchors if one or more Anchors were successfully imported; otherwise, it is ot modified.
//
task<bool> SpatialAnchorImportExportHelper::ExportAnchorDataAsync(
    vector<byte>* anchorByteDataOut,
    IMap<String^, SpatialAnchor^>^ anchorsToExport
    )
{
```

<span data-ttu-id="3839e-128">首先，我們需要設定資料流程。</span><span class="sxs-lookup"><span data-stu-id="3839e-128">First, we need to set up the data stream.</span></span> <span data-ttu-id="3839e-129">這可讓我們 ) 使用 TryExportAnchorsAsync 將資料放在應用程式所擁有的緩衝區中，而 2. ) 從匯出的位元組緩衝區資料流程（即 WinRT 資料串流）讀取資料到我們自己的記憶體緩衝區，也就是 std：： vector &lt; byte>。</span><span class="sxs-lookup"><span data-stu-id="3839e-129">This will allow us to 1.) use TryExportAnchorsAsync to put the data in a buffer owned by the app, and 2.) read data from the exported byte buffer stream - which is a WinRT data stream - into our own memory buffer, which is a std::vector&lt;byte>.</span></span>

```
// Create a random access stream to process the anchor byte data.
InMemoryRandomAccessStream^ stream = ref new InMemoryRandomAccessStream();
// Get an output stream for the anchor byte stream.
IOutputStream^ outputStream = stream->GetOutputStreamAt(0);
```

<span data-ttu-id="3839e-130">我們需要要求存取空間資料的許可權，包括系統匯出的錨點。</span><span class="sxs-lookup"><span data-stu-id="3839e-130">We need to ask permission to access spatial data, including anchors that are exported by the system.</span></span>

```
// Request access to spatial data.
auto accessRequestedTask = create_taskSpatialAnchorTransferManager::RequestAccessAsync()).then([anchorsToExport, utputStream](SpatialPerceptionAccessStatus status)
{
    if (status == SpatialPerceptionAccessStatus::Allowed)
    {
        // Access is allowed.
        // Export the indicated set of anchors.
        return create_task(SpatialAnchorTransferManager::TryExportAnchorsAsync(
            anchorsToExport,
            outputStream
            ));
    }
    else
    {
        // Access is denied.
        return task_from_result<bool>(false);
    }
});
```

<span data-ttu-id="3839e-131">如果我們取得 get 許可權並匯出錨點，就可以讀取資料流程。</span><span class="sxs-lookup"><span data-stu-id="3839e-131">If we do get permission and anchors are exported, we can read the data stream.</span></span> <span data-ttu-id="3839e-132">在這裡，我們也會示範如何建立 DataReader 和 InputStream，以用來讀取資料。</span><span class="sxs-lookup"><span data-stu-id="3839e-132">Here, we also show how to create the DataReader and InputStream we will use to read the data.</span></span>

```
// Get the input stream for the anchor byte stream.
IInputStream^ inputStream = stream->GetInputStreamAt(0);
// Create a DataReader, to get bytes from the anchor byte stream.
DataReader^ reader = ref new DataReader(inputStream);
return accessRequestedTask.then([anchorByteDataOut, stream, reader](bool nchorsExported)
{
    if (anchorsExported)
    {
        // Get the size of the exported anchor byte stream.
        size_t bufferSize = static_cast<size_t>(stream->Size);
        // Resize the output buffer to accept the data from the stream.
        anchorByteDataOut->reserve(bufferSize);
        anchorByteDataOut->resize(bufferSize);
        // Read the exported anchor store into the stream.
        return create_task(reader->LoadAsync(bufferSize));
    }
    else
    {
        return task_from_result<size_t>(0);
    }
```

<span data-ttu-id="3839e-133">從資料流程讀取位元組之後，我們可以將它們儲存到我們自己的資料緩衝區，如下所示。</span><span class="sxs-lookup"><span data-stu-id="3839e-133">After we read bytes from the stream, we can save them to our own data buffer like so.</span></span>

```
}).then([anchorByteDataOut, reader](size_t bytesRead)
{
    if (bytesRead > 0)
    {
        // Read the bytes from the stream, into our data output buffer.
        reader->ReadBytes(Platform::ArrayReference<byte>(&(*anchorByteDataOut)[0], bytesRead));
        return true;
    }
    else
    {
        return false;
    }
});
};
```

## <a name="deserialize-anchor-data-by-importing-it-into-the-system-using-the-spatialanchortransfermanager"></a><span data-ttu-id="3839e-134">使用 SpatialAnchorTransferManager 將錨點資料匯入系統以將其還原序列化</span><span class="sxs-lookup"><span data-stu-id="3839e-134">Deserialize anchor data by importing it into the system using the SpatialAnchorTransferManager</span></span>

<span data-ttu-id="3839e-135">Helper 函式包含在程式碼範例中，以載入先前匯出的資料。</span><span class="sxs-lookup"><span data-stu-id="3839e-135">A helper function is included in the code sample to load previously exported data.</span></span> <span data-ttu-id="3839e-136">此還原序列化函式會提供索引鍵/值組的集合，類似于 SpatialAnchorStore 所提供的內容，但我們從其他來源（例如網路通訊端）取得此資料。</span><span class="sxs-lookup"><span data-stu-id="3839e-136">This deserialization function provides a collection of key-value pairs, similar to what the SpatialAnchorStore provides - except that we got this data from another source, such as a network socket.</span></span> <span data-ttu-id="3839e-137">您可以使用應用程式內記憶體，或在應用程式的 SpatialAnchorStore) 適用的 (（如果適用的話）將此資料儲存在離線狀態之前處理與原因。</span><span class="sxs-lookup"><span data-stu-id="3839e-137">You can process and reason about this data before storing it offline, using in-app memory, or (if applicable) your app's SpatialAnchorStore.</span></span>

```
// ImportAnchorDataAsync: Imports anchors from a byte buffer that was previously exported.
//
// This function will import all anchors from a data buffer into an in-memory ollection of key, value
// pairs that maps String objects to SpatialAnchor objects. The Spatial nchorStore is not affected by
// this function unless you provide it as the target collection for import.
//
task<bool> SpatialAnchorImportExportHelper::ImportAnchorDataAsync(
    std::vector<byte>& anchorByteDataIn,
    IMap<String^, SpatialAnchor^>^ anchorMapOut
    )
{
```

<span data-ttu-id="3839e-138">首先，我們需要建立資料流程物件來存取錨點資料。</span><span class="sxs-lookup"><span data-stu-id="3839e-138">First, we need to create stream objects to access the anchor data.</span></span> <span data-ttu-id="3839e-139">我們會將緩衝區中的資料寫入系統緩衝區，因此我們將建立寫入記憶體中資料流程的 >datawriter，以達成將來自位元組緩衝區的錨點視為 SpatialAnchors 的目標。</span><span class="sxs-lookup"><span data-stu-id="3839e-139">We will be writing the data from our buffer to a system buffer, so we will create a DataWriter that writes to an in-memory data stream in order to accomplish our goal of getting anchors from a byte buffer into the system as SpatialAnchors.</span></span>

```
// Create a random access stream for the anchor data.
InMemoryRandomAccessStream^ stream = ref new InMemoryRandomAccessStream();
// Get an output stream for the anchor data.
IOutputStream^ outputStream = stream->GetOutputStreamAt(0);
// Create a writer, to put the bytes in the stream.
DataWriter^ writer = ref new DataWriter(outputStream);
```

<span data-ttu-id="3839e-140">同樣地，我們需要確保應用程式有權匯出空間錨點資料，其中可能包含有關使用者環境的私用資訊。</span><span class="sxs-lookup"><span data-stu-id="3839e-140">Once again, we need to ensure the app has permission to export spatial anchor data, which could include private information about the user's environment.</span></span>

```
// Request access to transfer spatial anchors.
return create_task(SpatialAnchorTransferManager::RequestAccessAsync()).then(
    [&anchorByteDataIn, writer](SpatialPerceptionAccessStatus status)
{
    if (status == SpatialPerceptionAccessStatus::Allowed)
    {
        // Access is allowed.
```

<span data-ttu-id="3839e-141">如果允許存取，我們可以從緩衝區將位元組寫入系統資料流程。</span><span class="sxs-lookup"><span data-stu-id="3839e-141">If access is allowed, we can write bytes from the buffer to a system data stream.</span></span>

```
// Write the bytes to the stream.
        byte* anchorDataFirst = &anchorByteDataIn[0];
        size_t anchorDataSize = anchorByteDataIn.size();
        writer->WriteBytes(Platform::ArrayReference<byte>(anchorDataFirst, anchorDataSize));
        // Store the stream.
        return create_task(writer->StoreAsync());
    }
    else
    {
        // Access is denied.
        return task_from_result<size_t>(0);
    }
```

<span data-ttu-id="3839e-142">如果成功將位元組儲存在資料流程中，我們可以嘗試使用 SpatialAnchorTransferManager 來匯入該資料。</span><span class="sxs-lookup"><span data-stu-id="3839e-142">If we were successful in storing bytes in the data stream, we can try to import that data using the SpatialAnchorTransferManager.</span></span>

```
}).then([writer, stream](unsigned int bytesWritten)
{
    if (bytesWritten > 0)
    {
        // Try to import anchors from the byte stream.
        return create_task(writer->FlushAsync())
            .then([stream](bool dataWasFlushed)
        {
            if (dataWasFlushed)
            {
                // Get the input stream for the anchor data.
                IInputStream^ inputStream = stream->GetInputStreamAt(0);
                return create_task(SpatialAnchorTransferManager::TryImportAnchorsAsync(inputStream));
            }
            else
            {
                return task_from_result<IMapView<String^, SpatialAnchor^>^>(nullptr);
            }
        });
    }
    else
    {
        return task_from_result<IMapView<String^, SpatialAnchor^>^>(nullptr);
    }
```

<span data-ttu-id="3839e-143">如果可以匯入資料，我們會取得索引鍵/值組的地圖視圖，以將字串與錨點產生關聯。</span><span class="sxs-lookup"><span data-stu-id="3839e-143">If the data is able to be imported, we get a map view of key-value pairs associating strings with anchors.</span></span> <span data-ttu-id="3839e-144">我們可以將它載入至我們自己的記憶體內部資料收集，並使用該集合來尋找我們有興趣使用的錨點。</span><span class="sxs-lookup"><span data-stu-id="3839e-144">We can load this into our own in-memory data collection, and use that collection to look for anchors that we are interested in using.</span></span>

```
}).then([anchorMapOut](task<Windows::Foundation::Collections::IMapView<String^, SpatialAnchor^>^>  previousTask)
{
    try
    {
        auto importedAnchorsMap = previousTask.get();
        // If the operation was successful, we get a set of imported anchors.
        if (importedAnchorsMap != nullptr)
        {
            for each (auto& pair in importedAnchorsMap)
            {
                // Note that you could look for specific anchors here, if you know their key values.
                auto const& id = pair->Key;
                auto const& anchor = pair->Value;
                // Append "Remote" to the end of the anchor name for disambiguation.
                std::wstring idRemote(id->Data());
                idRemote += L"Remote";
                String^ idRemoteConst = ref new String (idRemote.c_str());
                // Store the anchor in the current in-memory anchor map.
                anchorMapOut->Insert(idRemoteConst, anchor);
            }
            return true;
        }
    }
    catch (Exception^ exception)
    {
        OutputDebugString(L"Error: Unable to import the anchor data buffer bytes into the in-memory anchor collection.\n");
    }
    return false;
});
}
```

<span data-ttu-id="3839e-145">**注意：** 您可以匯入錨點，而不一定表示您可以立即使用它。</span><span class="sxs-lookup"><span data-stu-id="3839e-145">**NOTE:** Just because you can import an anchor, doesn't necessarily mean that you can use it right away.</span></span> <span data-ttu-id="3839e-146">錨點可能位於不同的房間，或完全位於另一個實體位置;錨點必須等到收到此錨點的裝置具有足夠的視覺資訊，以取得錨點建立所在的環境，以便還原錨點相對於已知目前環境的位置。</span><span class="sxs-lookup"><span data-stu-id="3839e-146">The anchor might be in a different room, or another physical location entirely; the anchor won't be locatable until the device that received it has enough visual information about the environment the anchor was created in, to restore the anchor's position relative to the known current environment.</span></span> <span data-ttu-id="3839e-147">用戶端的執行應該先嘗試找出相對於您本機座標系統或參考框架的錨點，然後再繼續嘗試將它用於即時內容。</span><span class="sxs-lookup"><span data-stu-id="3839e-147">The client implementation should try locating the anchor relative to your local coordinate system or reference frame before continuing on to try to use it for live content.</span></span> <span data-ttu-id="3839e-148">例如，請嘗試定期找出相對於目前座標系統的錨點，直到開始找出錨點為止。</span><span class="sxs-lookup"><span data-stu-id="3839e-148">For example, try locating the anchor relative to a current coordinate system periodically until the anchor begins to be locatable.</span></span>

## <a name="special-considerations"></a><span data-ttu-id="3839e-149">特殊考慮</span><span class="sxs-lookup"><span data-stu-id="3839e-149">Special Considerations</span></span>

<span data-ttu-id="3839e-150">[TryExportAnchorsAsync](https://msdn.microsoft.com/library/windows/apps/mt592763.aspx) API 允許將多個[SpatialAnchors](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchor.aspx)匯出至相同的不透明二進位 blob。</span><span class="sxs-lookup"><span data-stu-id="3839e-150">The [TryExportAnchorsAsync](https://msdn.microsoft.com/library/windows/apps/mt592763.aspx) API allows multiple [SpatialAnchors](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchor.aspx) to be exported into the same opaque binary blob.</span></span> <span data-ttu-id="3839e-151">不過，blob 將包含的資料會有些許差異，這取決於單一 SpatialAnchor 或多個 SpatialAnchors 是否會在單一呼叫中匯出。</span><span class="sxs-lookup"><span data-stu-id="3839e-151">However, there is a subtle difference in what data the blob will include, depending on whether a single SpatialAnchor or multiple SpatialAnchors are exported in a single call.</span></span>

### <a name="export-of-a-single-spatialanchor"></a><span data-ttu-id="3839e-152">匯出單一 SpatialAnchor</span><span class="sxs-lookup"><span data-stu-id="3839e-152">Export of a single SpatialAnchor</span></span>

<span data-ttu-id="3839e-153">Blob 會在 SpatialAnchor 的鄰近區域中包含環境的標記法，以便在匯入 SpatialAnchor 的裝置上辨識環境。</span><span class="sxs-lookup"><span data-stu-id="3839e-153">The blob contains a representation of the environment in the vicinity of the SpatialAnchor so that the environment can be recognized on the device that imports the SpatialAnchor.</span></span> <span data-ttu-id="3839e-154">匯入完成後，裝置將可使用新的 SpatialAnchor。</span><span class="sxs-lookup"><span data-stu-id="3839e-154">After the import completes, the new SpatialAnchor will be available to the device.</span></span> <span data-ttu-id="3839e-155">假設使用者最近位於錨點的區域內，則會將它定位，並可轉譯附加至 SpatialAnchor 的影像。</span><span class="sxs-lookup"><span data-stu-id="3839e-155">Assuming the user has recently been in vicinity of the anchor, it will be locatable and holograms attached to the SpatialAnchor can be rendered.</span></span> <span data-ttu-id="3839e-156">這些全像全像在匯出 SpatialAnchor 的原始裝置上，會顯示在相同的實體位置。</span><span class="sxs-lookup"><span data-stu-id="3839e-156">These holograms will show up in the same physical location that they did on the original device which exported the SpatialAnchor.</span></span>

![匯出單一 SpatialAnchor](images/singleanchor.png)

### <a name="export-of-multiple-spatialanchors"></a><span data-ttu-id="3839e-158">匯出多個 SpatialAnchors</span><span class="sxs-lookup"><span data-stu-id="3839e-158">Export of multiple SpatialAnchors</span></span>

<span data-ttu-id="3839e-159">如同匯出單一 SpatialAnchor，blob 會在所有指定 SpatialAnchors 的鄰近範圍中包含環境的表示。</span><span class="sxs-lookup"><span data-stu-id="3839e-159">Like the export of a single SpatialAnchor, the blob contains a representation of the environment in the vicinity of all the specified SpatialAnchors.</span></span> <span data-ttu-id="3839e-160">此外，blob 也包含包含的 SpatialAnchors 之間的連接相關資訊（如果它們位於相同的實體空間中）。</span><span class="sxs-lookup"><span data-stu-id="3839e-160">In addition, the blob contains information about the connections between the included SpatialAnchors, if they are located in the same physical space.</span></span> <span data-ttu-id="3839e-161">這表示，如果匯入兩個鄰近的 SpatialAnchors，則即使裝置只會在 *第一個* SpatialAnchor 的周圍辨識出環境，也會找出附加至 *第二* 個 SpatialAnchor 的全息圖，因為 blob 中包含了足夠的資料來計算兩個 SpatialAnchors 之間的轉換。</span><span class="sxs-lookup"><span data-stu-id="3839e-161">This means that if two nearby SpatialAnchors are imported, then a hologram attached to the *second* SpatialAnchor would be locatable even if the device only recognizes the environment around the *first* SpatialAnchor, because enough data to compute transform between the two SpatialAnchors was included in the blob.</span></span> <span data-ttu-id="3839e-162">如果兩個 SpatialAnchors 分別匯出 (兩個) TryExportSpatialAnchors 呼叫，則 blob 中可能沒有足夠的資料可供連接到第二個 SpatialAnchor，以在第一個找到時加以定位。</span><span class="sxs-lookup"><span data-stu-id="3839e-162">If the two SpatialAnchors were exported individually (two separate calls to TryExportSpatialAnchors) then there may not be enough data included in the blob for holograms attached to the second SpatialAnchor to be locatable when the first one is located.</span></span>

![使用單一 TryExportAnchorsAsync 呼叫匯出的多個錨點](images/multipleanchors.png) ![針對每個錨點使用個別 TryExportAnchorsAsync 呼叫匯出多個錨點](images/separateanchors.png)

## <a name="example-send-anchor-data-using-a-windowsnetworkingstreamsocket"></a><span data-ttu-id="3839e-165">範例：使用 Windows：：網路：： StreamSocket 傳送錨點資料</span><span class="sxs-lookup"><span data-stu-id="3839e-165">Example: Send anchor data using a Windows::Networking::StreamSocket</span></span>

<span data-ttu-id="3839e-166">在這裡，我們會提供一個範例，說明如何藉由在 TCP 網路上傳送來使用匯出的錨定資料。</span><span class="sxs-lookup"><span data-stu-id="3839e-166">Here, we provide an example of how to use exported anchor data by sending it across a TCP network.</span></span> <span data-ttu-id="3839e-167">這是來自 HolographicSpatialAnchorTransferSample。</span><span class="sxs-lookup"><span data-stu-id="3839e-167">This is from the HolographicSpatialAnchorTransferSample.</span></span>

<span data-ttu-id="3839e-168">WinRT StreamSocket 類別會使用 PPL 工作程式庫。</span><span class="sxs-lookup"><span data-stu-id="3839e-168">The WinRT StreamSocket class uses the PPL task library.</span></span> <span data-ttu-id="3839e-169">如果發生網路錯誤，則會使用重新擲回的例外狀況，將錯誤傳回至鏈中的下一個工作。</span><span class="sxs-lookup"><span data-stu-id="3839e-169">In the case of network errors, the error is returned to the next task in the chain using an exception that is re-thrown.</span></span> <span data-ttu-id="3839e-170">例外狀況包含表示錯誤狀態的 HRESULT。</span><span class="sxs-lookup"><span data-stu-id="3839e-170">The exception contains an HRESULT indicating the error status.</span></span>

### <a name="use-a-windowsnetworkingstreamsocketlistener-with-tcp-to-send-exported-anchor-data"></a><span data-ttu-id="3839e-171">使用 Windows：：網路：： StreamSocketListener 搭配 TCP 來傳送匯出的錨點資料</span><span class="sxs-lookup"><span data-stu-id="3839e-171">Use a Windows::Networking::StreamSocketListener with TCP to send exported anchor data</span></span>

<span data-ttu-id="3839e-172">建立接聽連接的伺服器實例。</span><span class="sxs-lookup"><span data-stu-id="3839e-172">Create a server instance that listens for a connection.</span></span>

```
void SampleAnchorTcpServer::ListenForConnection()
{
    // Make a local copy to avoid races with Closed events.
    StreamSocketListener^ streamSocketListener = m_socketServer;
    if (streamSocketListener == nullptr)
    {
        OutputDebugString(L"Server listening for client.\n");
        // Create the web socket connection.
        streamSocketListener = ref new StreamSocketListener();
        streamSocketListener->Control->KeepAlive = true;
        streamSocketListener->BindEndpointAsync(
            SampleAnchorTcpCommon::m_serverHost,
            SampleAnchorTcpCommon::m_tcpPort
            );
        streamSocketListener->ConnectionReceived +=
            ref new Windows::Foundation::TypedEventHandler<StreamSocketListener^, StreamSocketListenerConnectionReceivedEventArgs^>(
                std::bind(&SampleAnchorTcpServer::OnConnectionReceived, this, _1, _2)
                );
        m_socketServer = streamSocketListener;
    }
    else
    {
        OutputDebugString(L"Error: Stream socket listener not created.\n");
    }
}
```

<span data-ttu-id="3839e-173">當接收到連線時，請使用用戶端通訊端連接來傳送錨點資料。</span><span class="sxs-lookup"><span data-stu-id="3839e-173">When a connection is received, use the client socket connection to send anchor data.</span></span>

```
void SampleAnchorTcpServer::OnConnectionReceived(StreamSocketListener^ listener, StreamSocketListenerConnectionReceivedEventArgs^ args)
{
    m_socketForClient = args->Socket;
    if (m_socketForClient != nullptr)
    {
        // In this example, when the client first connects, we catch it up to the current state of our anchor set.
        OutputToClientSocket(m_spatialAnchorHelper->GetAnchorMap());
    }
}
```

<span data-ttu-id="3839e-174">現在，我們可以開始傳送包含所匯出錨點資料的資料流程。</span><span class="sxs-lookup"><span data-stu-id="3839e-174">Now, we can begin to send a data stream that contains the exported anchor data.</span></span>

```
void SampleAnchorTcpServer::OutputToClientSocket(IMap<String^, SpatialAnchor^>^ anchorsToSend)
{
    m_anchorTcpSocketStreamWriter = ref new DataWriter(m_socketForClient->OutputStream);
    OutputDebugString(L"Sending stream to client.\n");
    SendAnchorDataStream(anchorsToSend).then([this](task<bool> previousTask)
    {
        try
        {
            bool success = previousTask.get();
            if (success)
            {
                OutputDebugString(L"Anchor data sent!\n");
            }
            else
            {
                OutputDebugString(L"Error: Anchor data not sent.\n");
            }
        }
        catch (Exception^ exception)
        {
            HandleException(exception);
            OutputDebugString(L"Error: Anchor data was not sent.\n");
        }
    });
}
```

<span data-ttu-id="3839e-175">在我們可以傳送資料流程本身之前，我們必須先傳送標頭封包。</span><span class="sxs-lookup"><span data-stu-id="3839e-175">Before we can send the stream itself, we must first send a header packet.</span></span> <span data-ttu-id="3839e-176">這個標頭封包的長度必須是固定的，而且也必須指出錨定資料流程的位元組變數陣列長度;在此範例中，我們沒有其他要傳送的標頭資料，因此我們的標頭長度為4個位元組，且包含32位不帶正負號的整數。</span><span class="sxs-lookup"><span data-stu-id="3839e-176">This header packet must be of fixed length, and it must also indicate the length of the variable array of bytes that is the anchor data stream; in the case of this example we have no other header data to send, so our header is 4 bytes long and contains a 32-bit unsigned integer.</span></span>

```
Concurrency::task<bool> SampleAnchorTcpServer::SendAnchorDataLengthMessage(size_t dataStreamLength)
{
    unsigned int arrayLength = dataStreamLength;
    byte* data = reinterpret_cast<byte*>(&arrayLength);
    m_anchorTcpSocketStreamWriter->WriteBytes(Platform::ArrayReference<byte>(data, SampleAnchorTcpCommon::c_streamHeaderByteArrayLength));
    return create_task(m_anchorTcpSocketStreamWriter->StoreAsync()).then([this](unsigned int bytesStored)
    {
        if (bytesStored > 0)
        {
            OutputDebugString(L"Anchor data length stored in stream; Flushing stream.\n");
            return create_task(m_anchorTcpSocketStreamWriter->FlushAsync());
        }
        else
        {
            OutputDebugString(L"Error: Anchor data length not stored in stream.\n");
            return task_from_result<bool>(false);
        }
    });
}
Concurrency::task<bool> SampleAnchorTcpServer::SendAnchorDataStreamIMap<String^, SpatialAnchor^>^ anchorsToSend)
{
    return SpatialAnchorImportExportHelper::ExportAnchorDataAsync(
        &m_exportedAnchorStoreBytes,
        anchorsToSend
        ).then([this](bool anchorDataExported)
    {
        if (anchorDataExported)
        {
            const size_t arrayLength = m_exportedAnchorStoreBytes.size();
            if (arrayLength > 0)
            {
                OutputDebugString(L"Anchor data was exported; sending data stream length message.\n");
                return SendAnchorDataLengthMessage(arrayLength);
            }
        }
        OutputDebugString(L"Error: Anchor data was not exported.\n");
        // No data to send.
        return task_from_result<bool>(false);
```

<span data-ttu-id="3839e-177">一旦串流長度（以位元組為單位）傳送至用戶端之後，我們就可以繼續將資料流程本身寫入至通訊端資料流程。</span><span class="sxs-lookup"><span data-stu-id="3839e-177">Once the stream length, in bytes, has been sent to the client, we can proceed to write the data stream itself to the socket stream.</span></span> <span data-ttu-id="3839e-178">這會導致錨定存放區位元組傳送給用戶端。</span><span class="sxs-lookup"><span data-stu-id="3839e-178">This will cause the anchor store bytes to get sent to the client.</span></span>

```
}).then([this](bool dataLengthSent)
    {
        if (dataLengthSent)
        {
            OutputDebugString(L"Data stream length message sent; writing exported anchor store bytes to stream.\n");
            m_anchorTcpSocketStreamWriter->WriteBytes(Platform::ArrayReference<byte>(&m_exportedAnchorStoreBytes[0], m_exportedAnchorStoreBytes.size()));
            return create_task(m_anchorTcpSocketStreamWriter->StoreAsync());
        }
        else
        {
            OutputDebugString(L"Error: Data stream length message not sent.\n");
            return task_from_result<size_t>(0);
        }
    }).then([this](unsigned int bytesStored)
    {
        if (bytesStored > 0)
        {
            PrintWstringToDebugConsole(
                std::to_wstring(bytesStored) +
                L" bytes of anchor data written and stored to stream; flushing stream.\n"
                );
        }
        else
        {
            OutputDebugString(L"Error: No anchor data bytes were written to the stream.\n");
        }
        return task_from_result<bool>(false);
    });
}
```

<span data-ttu-id="3839e-179">如本主題稍早所述，我們必須做好準備，以處理包含網路錯誤狀態訊息的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="3839e-179">As noted earlier in this topic, we must be prepared to handle exceptions containing network error status messages.</span></span> <span data-ttu-id="3839e-180">針對非預期的錯誤，我們可以將例外狀況資訊寫入至 debug 主控台，如下所示。</span><span class="sxs-lookup"><span data-stu-id="3839e-180">For errors that are not expected, we can write the exception info to the debug console like so.</span></span> <span data-ttu-id="3839e-181">如果我們的程式碼範例無法完成連線，或是如果無法完成傳送錨點資料，這會讓我們有個線索。</span><span class="sxs-lookup"><span data-stu-id="3839e-181">This will give us a clue as to what happened if our code sample is unable to complete the connection, or if it is unable to finish sending the anchor data.</span></span>

```
void SampleAnchorTcpServer::HandleException(Exception^ exception)
{
    PrintWstringToDebugConsole(
        std::wstring(L"Connection error: ") +
        exception->ToString()->Data() +
        L"\n"
        );
}
```

### <a name="use-a-windowsnetworkingstreamsocket-with-tcp-to-receive-exported-anchor-data"></a><span data-ttu-id="3839e-182">使用具有 TCP 的 Windows：：網路：： StreamSocket 來接收匯出的錨點資料</span><span class="sxs-lookup"><span data-stu-id="3839e-182">Use a Windows::Networking::StreamSocket with TCP to receive exported anchor data</span></span>

<span data-ttu-id="3839e-183">首先，我們必須連接到伺服器。</span><span class="sxs-lookup"><span data-stu-id="3839e-183">First, we have to connect to the server.</span></span> <span data-ttu-id="3839e-184">此程式碼範例示範如何建立和設定 StreamSocket，並建立可讓您使用通訊端連接取得網路資料的 DataReader。</span><span class="sxs-lookup"><span data-stu-id="3839e-184">This code sample shows how to create and configure a StreamSocket, and create a DataReader that you can use to acquire network data using the socket connection.</span></span>

<span data-ttu-id="3839e-185">**注意：** 如果您執行此範例程式碼，請確定您在啟動用戶端之前，先設定並啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="3839e-185">**NOTE:** If you run this sample code, ensure that you configure and launch the server before starting the client.</span></span>

```
task<bool> SampleAnchorTcpClient::ConnectToServer()
{
    // Make a local copy to avoid races with Closed events.
    StreamSocket^ streamSocket = m_socketClient;
    // Have we connected yet?
    if (m_socketClient == nullptr)
    {
        OutputDebugString(L"Client is attempting to connect to server.\n");
        EndpointPair^ endpointPair = ref new EndpointPair(
            SampleAnchorTcpCommon::m_clientHost,
            SampleAnchorTcpCommon::m_tcpPort,
            SampleAnchorTcpCommon::m_serverHost,
            SampleAnchorTcpCommon::m_tcpPort
            );
        // Create the web socket connection.
        m_socketClient = ref new StreamSocket();
        // The client connects to the server.
        return create_task(m_socketClient->ConnectAsync(endpointPair, SocketProtectionLevel::PlainSocket)).then([this](task<void> previousTask)
        {
            try
            {
                // Try getting all exceptions from the continuation chain above this point.
                previousTask.get();
                m_anchorTcpSocketStreamReader = ref new DataReader(m_socketClient->InputStream);
                OutputDebugString(L"Client connected!\n");
                m_anchorTcpSocketStreamReader->InputStreamOptions = InputStreamOptions::ReadAhead;
                WaitForAnchorDataStream();
                return true;
            }
            catch (Exception^ exception)
            {
                if (exception->HResult == 0x80072741)
                {
                    // This code sample includes a very simple implementation of client/server
                    // endpoint detection: if the current instance tries to connect to itself,
                    // it is determined to be the server.
                    OutputDebugString(L"Starting up the server instance.\n");
                    // When we return false, we'll start up the server instead.
                    return false;
                }
                else if ((exception->HResult == 0x8007274c) || // connection timed out
                    (exception->HResult == 0x80072740)) // connection maxed at server end
                {
                    // If the connection timed out, try again.
                    ConnectToServer();
                }
                else if (exception->HResult == 0x80072741)
                {
                    // No connection is possible.
                }
                HandleException(exception);
                return true;
            }
        });
    }
    else
    {
        OutputDebugString(L"A StreamSocket connection to a server already exists.\n");
        return task_from_result<bool>(true);
    }
}
```

<span data-ttu-id="3839e-186">一旦連線之後，我們就可以等候伺服器傳送資料。</span><span class="sxs-lookup"><span data-stu-id="3839e-186">Once we have a connection, we can wait for the server to send data.</span></span> <span data-ttu-id="3839e-187">做法是在資料流程資料讀取器上呼叫 LoadAsync。</span><span class="sxs-lookup"><span data-stu-id="3839e-187">We do this by calling LoadAsync on the stream data reader.</span></span>

<span data-ttu-id="3839e-188">我們收到的第一組位元組應該一律是標頭封包，指出錨定資料流程位元組長度，如上一節中所述。</span><span class="sxs-lookup"><span data-stu-id="3839e-188">The first set of bytes we receive should always be the header packet, which indicates the anchor data stream byte length as described in the previous section.</span></span>

```
void SampleAnchorTcpClient::WaitForAnchorDataStream()
{
    if (m_anchorTcpSocketStreamReader == nullptr)
    {
        // We have not connected yet.
        return;
    }
    OutputDebugString(L"Waiting for server message.\n");
    // Wait for the first message, which specifies the byte length of the string data.
    create_task(m_anchorTcpSocketStreamReader->LoadAsync(SampleAnchorTcpCommon::c_streamHeaderByteArrayLength)).then([this](unsigned int numberOfBytes)
    {
        if (numberOfBytes > 0)
        {
            OutputDebugString(L"Server message incoming.\n");
            return ReceiveAnchorDataLengthMessage();
        }
        else
        {
            OutputDebugString(L"0-byte async task received, awaiting server message again.\n");
            WaitForAnchorDataStream();
            return task_from_result<size_t>(0);
        }
```

<span data-ttu-id="3839e-189">...</span><span class="sxs-lookup"><span data-stu-id="3839e-189">...</span></span>

```
task<size_t> SampleAnchorTcpClient::ReceiveAnchorDataLengthMessage()
{
    byte data[4];
    m_anchorTcpSocketStreamReader->ReadBytes(Platform::ArrayReference<byte>(data, SampleAnchorTcpCommon::c_streamHeaderByteArrayLength));
    unsigned int lengthMessageSize = *reinterpret_cast<unsigned int*>(data);
    if (lengthMessageSize > 0)
    {
        OutputDebugString(L"One or more anchors to be received.\n");
        return task_from_result<size_t>(lengthMessageSize);
    }
    else
    {
        OutputDebugString(L"No anchors to be received.\n");
        ConnectToServer();
    }
    return task_from_result<size_t>(0);
}
```

<span data-ttu-id="3839e-190">當我們收到標頭封包之後，我們知道應該預期的錨定資料位元組數目。</span><span class="sxs-lookup"><span data-stu-id="3839e-190">After we have received the header packet, we know how many bytes of anchor data we should expect.</span></span> <span data-ttu-id="3839e-191">我們可以繼續從資料流程讀取這些位元組。</span><span class="sxs-lookup"><span data-stu-id="3839e-191">We can proceed to read those bytes from the stream.</span></span>

```
}).then([this](size_t dataStreamLength)
    {
        if (dataStreamLength > 0)
        {
            std::wstring debugMessage = std::to_wstring(dataStreamLength);
            debugMessage += L" bytes of anchor data incoming.\n";
            OutputDebugString(debugMessage.c_str());
            // Prepare to receive the data stream in one or more pieces.
            m_anchorStreamLength = dataStreamLength;
            m_exportedAnchorStoreBytes.clear();
            m_exportedAnchorStoreBytes.resize(m_anchorStreamLength);
            OutputDebugString(L"Loading byte stream.\n");
            return ReceiveAnchorDataStream();
        }
        else
        {
            OutputDebugString(L"Error: Anchor data size not received.\n");
            ConnectToServer();
            return task_from_result<bool>(false);
        }
    });
}
```

<span data-ttu-id="3839e-192">以下是接收錨定資料流程的程式碼。</span><span class="sxs-lookup"><span data-stu-id="3839e-192">Here's our code for receiving the anchor data stream.</span></span> <span data-ttu-id="3839e-193">同樣地，我們會先從資料流程載入位元組;這項作業可能需要一些時間才能完成，因為 StreamSocket 會等待接收來自網路的位元組數量。</span><span class="sxs-lookup"><span data-stu-id="3839e-193">Again, we will first load the bytes from the stream; this operation may take some time to complete as the StreamSocket waits to receive that amount of bytes from the network.</span></span>

<span data-ttu-id="3839e-194">當載入作業完成時，我們可以讀取該位元組數目。</span><span class="sxs-lookup"><span data-stu-id="3839e-194">When the loading operation is complete, we can read that number of bytes.</span></span> <span data-ttu-id="3839e-195">如果我們收到我們預期錨點資料流程的位元組數，可以繼續匯入錨點資料;如果沒有，就必須有某種類型的錯誤。</span><span class="sxs-lookup"><span data-stu-id="3839e-195">If we received the number of bytes that we expect for the anchor data stream, we can go ahead and import the anchor data; if not, there must have been some sort of error.</span></span> <span data-ttu-id="3839e-196">例如，當伺服器實例在完成傳送資料流程之前終止，或在用戶端接收整個資料流程之前，網路中斷時，就會發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="3839e-196">For example, this can happen when the server instance terminates before it can finish sending the data stream, or the network goes down before the entire data stream can be received by the client.</span></span>

```
task<bool> SampleAnchorTcpClient::ReceiveAnchorDataStream()
{
    if (m_anchorStreamLength > 0)
    {
        // First, we load the bytes from the network socket.
        return create_task(m_anchorTcpSocketStreamReader->LoadAsync(m_anchorStreamLength)).then([this](size_t bytesLoadedByStreamReader)
        {
            if (bytesLoadedByStreamReader > 0)
            {
                // Once the bytes are loaded, we can read them from the stream.
                m_anchorTcpSocketStreamReader->ReadBytes(Platform::ArrayReference<byte>(&m_exportedAnchorStoreBytes[0],
                    bytesLoadedByStreamReader));
                // Check status.
                if (bytesLoadedByStreamReader == m_anchorStreamLength)
                {
                    // The whole stream has arrived. We can process the data.
                    // Informational message of progress complete.
                    std::wstring infoMessage = std::to_wstring(bytesLoadedByStreamReader);
                    infoMessage += L" bytes read out of ";
                    infoMessage += std::to_wstring(m_anchorStreamLength);
                    infoMessage += L" total bytes; importing the data.\n";
                    OutputDebugStringW(infoMessage.c_str());
                    // Kick off a thread to wait for a new message indicating another incoming anchor data stream.
                    WaitForAnchorDataStream();
                    // Process the data for the stream we just received.
                    return SpatialAnchorImportExportHelper::ImportAnchorDataAsync(m_exportedAnchorStoreBytes, m_spatialAnchorHelper->GetAnchorMap());
                }
                else
                {
                    OutputDebugString(L"Error: Fewer than expected anchor data bytes were received.\n");
                }
            }
            else
            {
                OutputDebugString(L"Error: No anchor bytes were received.\n");
            }
            return task_from_result<bool>(false);
        });
    }
    else
    {
        OutputDebugString(L"Warning: A zero-length data buffer was sent.\n");
        return task_from_result<bool>(false);
    }
}
```

<span data-ttu-id="3839e-197">同樣地，我們必須準備處理未知的網路錯誤。</span><span class="sxs-lookup"><span data-stu-id="3839e-197">Again, we must be prepared to handle unknown network errors.</span></span>

```
void SampleAnchorTcpClient::HandleException(Exception^ exception)
{
    std::wstring error = L"Connection error: ";
    error += exception->ToString()->Data();
    error += L"\n";
    OutputDebugString(error.c_str());
}
```

<span data-ttu-id="3839e-198">這樣就完成了！</span><span class="sxs-lookup"><span data-stu-id="3839e-198">That's it!</span></span> <span data-ttu-id="3839e-199">現在，您應該有足夠的資訊來嘗試找出網路上收到的錨點。</span><span class="sxs-lookup"><span data-stu-id="3839e-199">Now, you should have enough information to try locating the anchors received over the network.</span></span> <span data-ttu-id="3839e-200">同樣地，請注意，用戶端必須要有足夠的視覺追蹤資料，才能成功地找出錨點;如果無法立即運作，請試著稍候一段時間。</span><span class="sxs-lookup"><span data-stu-id="3839e-200">Again, note that the client must have enough visual tracking data for the space to successfully locate the anchor; if it doesn't work right away, try walking around for a while.</span></span> <span data-ttu-id="3839e-201">如果仍然無法運作，請讓伺服器傳送更多錨點，並使用網路通訊來同意用戶端所適用的訊息。</span><span class="sxs-lookup"><span data-stu-id="3839e-201">If it still doesn't work, have the server send more anchors, and use network communications to agree on one that works for the client.</span></span> <span data-ttu-id="3839e-202">您可以下載 HolographicSpatialAnchorTransferSample、設定用戶端和伺服器 Ip，以及將其部署至用戶端和伺服器 HoloLens 裝置，來嘗試此程式。</span><span class="sxs-lookup"><span data-stu-id="3839e-202">You can try this out by downloading the HolographicSpatialAnchorTransferSample, configuring your client and server IPs, and deploying it to client and server HoloLens devices.</span></span>

## <a name="see-also"></a><span data-ttu-id="3839e-203">另請參閱</span><span class="sxs-lookup"><span data-stu-id="3839e-203">See also</span></span>
* [<span data-ttu-id="3839e-204">平行模式程式庫 (PPL)</span><span class="sxs-lookup"><span data-stu-id="3839e-204">Parallel Patterns Library (PPL)</span></span>](https://msdn.microsoft.com/library/dd492418.aspx)
* [<span data-ttu-id="3839e-205">StreamSocket</span><span class="sxs-lookup"><span data-stu-id="3839e-205">Windows.Networking.StreamSocket</span></span>](https://msdn.microsoft.com/library/windows/apps/windows.networking.sockets.streamsocket.aspx)
* [<span data-ttu-id="3839e-206">StreamSocketListener</span><span class="sxs-lookup"><span data-stu-id="3839e-206">Windows.Networking.StreamSocketListener</span></span>](https://msdn.microsoft.com/library/windows/apps/windows.networking.sockets.streamsocketlistener.aspx)
