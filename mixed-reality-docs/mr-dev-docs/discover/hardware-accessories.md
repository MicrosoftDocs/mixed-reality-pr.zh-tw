---
title: 硬體配件
description: 描述可搭配 Windows Mixed Reality 使用的配件類型，以及如何進行設定。
author: mattzmsft
ms.author: mazeller
ms.date: 05/20/2020
ms.topic: article
keywords: 作法、配件、藍牙、bt、控制器、遊戲台、clicker、xbox
ms.openlocfilehash: 7f51264a3914d028c9a027d70d5aa1999582110a
ms.sourcegitcommit: 09599b4034be825e4536eeb9566968afd021d5f3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/03/2020
ms.locfileid: "91682589"
---
# <a name="hardware-accessories"></a><span data-ttu-id="ac362-104">硬體配件</span><span class="sxs-lookup"><span data-stu-id="ac362-104">Hardware accessories</span></span>

<span data-ttu-id="ac362-105">Windows Mixed Reality 裝置支援配件。</span><span class="sxs-lookup"><span data-stu-id="ac362-105">Windows Mixed Reality devices support accessories.</span></span> <span data-ttu-id="ac362-106">您可以使用藍牙或 USB 將支援的配件與已連線的電腦配對到沉浸式耳機。</span><span class="sxs-lookup"><span data-stu-id="ac362-106">You can use Bluetooth or USB to pair supported accessories to an immersive headset by using the PC to which it is connected.</span></span>

<span data-ttu-id="ac362-107">如需搭配 HoloLens 使用藍牙附屬元件的相關資訊，請參閱 [連接到 bluetooth 和 USB-C 裝置](https://docs.microsoft.com/hololens/hololens-connect-devices)。</span><span class="sxs-lookup"><span data-stu-id="ac362-107">For information about using Bluetooth accessories with HoloLens, see [Connect to Bluetooth and USB-C devices](https://docs.microsoft.com/hololens/hololens-connect-devices).</span></span>

<span data-ttu-id="ac362-108">Windows Mixed Reality 沉浸式耳機需要[輸入的附屬和](../design/gaze-and-commit.md)[聲音](../design/voice-input.md)以外的配件。</span><span class="sxs-lookup"><span data-stu-id="ac362-108">Windows Mixed Reality immersive headsets require accessories for input beyond [gaze](../design/gaze-and-commit.md) and [voice](../design/voice-input.md).</span></span> <span data-ttu-id="ac362-109">支援的配件包括 **鍵盤和滑鼠** 、 **遊戲台** 和 **[移動控制器](../design/motion-controllers.md)** 。</span><span class="sxs-lookup"><span data-stu-id="ac362-109">Supported accessories include **keyboard and mouse** , **gamepad** , and **[motion controllers](../design/motion-controllers.md)** .</span></span>

## <a name="pairing-bluetooth-accessories"></a><span data-ttu-id="ac362-110">配對藍牙配件</span><span class="sxs-lookup"><span data-stu-id="ac362-110">Pairing Bluetooth accessories</span></span>

<span data-ttu-id="ac362-111">將 Bluetooth 周邊與沉浸式耳機配對類似于將 Bluetooth 周邊與 Windows 10 桌面或行動裝置配對：</span><span class="sxs-lookup"><span data-stu-id="ac362-111">Pairing a Bluetooth peripheral with an immersive headset is similar to pairing a Bluetooth peripheral with a Windows 10 desktop or mobile device:</span></span>

1. <span data-ttu-id="ac362-112">從 [開始] 功能表開啟 [ **設定** ] 應用程式</span><span class="sxs-lookup"><span data-stu-id="ac362-112">From the Start Menu, open the **Settings** app</span></span>
2. <span data-ttu-id="ac362-113">移至 **裝置**</span><span class="sxs-lookup"><span data-stu-id="ac362-113">Go to **Devices**</span></span>
3. <span data-ttu-id="ac362-114">使用滑杆開關開啟藍牙無線電</span><span class="sxs-lookup"><span data-stu-id="ac362-114">Turn on the Bluetooth radio if it is off using the slider switch</span></span>
4. <span data-ttu-id="ac362-115">將您的藍牙裝置置於配對模式。</span><span class="sxs-lookup"><span data-stu-id="ac362-115">Place your Bluetooth device in pairing mode.</span></span> <span data-ttu-id="ac362-116">這種方式會因裝置而異。</span><span class="sxs-lookup"><span data-stu-id="ac362-116">This varies from device to device.</span></span> <span data-ttu-id="ac362-117">在大部分的藍牙裝置上，只要按住一或多個按鈕就可以完成這項工作。</span><span class="sxs-lookup"><span data-stu-id="ac362-117">On most Bluetooth devices this is done by pressing and holding one or more buttons.</span></span>
5. <span data-ttu-id="ac362-118">等候裝置的名稱顯示在藍牙裝置清單中。</span><span class="sxs-lookup"><span data-stu-id="ac362-118">Wait for the name of the device to show up in the list of Bluetooth devices.</span></span> <span data-ttu-id="ac362-119">當它完成時，請選取裝置，然後選取 [ **配對** ] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="ac362-119">When it does, select the device then select the **Pair** button.</span></span> <span data-ttu-id="ac362-120">如果您附近有許多藍牙裝置，您可能需要滾動至 [藍牙裝置] 清單底部，以查看您嘗試配對的裝置。</span><span class="sxs-lookup"><span data-stu-id="ac362-120">If you have many Bluetooth devices nearby you may need to scroll to the bottom of the Bluetooth device list to see the device you are trying to pair.</span></span>
6. <span data-ttu-id="ac362-121">將 Bluetooth 周邊與輸入功能配對時 (例如： Bluetooth 鍵盤) ，可能會顯示6位數或8位數的 pin。</span><span class="sxs-lookup"><span data-stu-id="ac362-121">When pairing Bluetooth peripherals with input capability (e.g.: Bluetooth keyboards), a 6-digit or an 8-digit pin may be displayed.</span></span> <span data-ttu-id="ac362-122">請務必在週邊設備上輸入 pin，然後按 enter 鍵以完成與耳機的配對。</span><span class="sxs-lookup"><span data-stu-id="ac362-122">Be sure to type that pin on the peripheral and then press enter to complete pairing with the headset.</span></span>

## <a name="motion-controllers"></a><span data-ttu-id="ac362-123">運動控制器</span><span class="sxs-lookup"><span data-stu-id="ac362-123">Motion controllers</span></span>

<span data-ttu-id="ac362-124">沉浸式耳機支援 Windows Mixed Reality [動作控制器](../design/motion-controllers.md) ，但不支援 HoloLens。</span><span class="sxs-lookup"><span data-stu-id="ac362-124">Windows Mixed Reality [motion controllers](../design/motion-controllers.md) are supported by immersive headsets, but not HoloLens.</span></span> <span data-ttu-id="ac362-125">這些控制器使用沉浸式耳機中的感應器，在您的觀看視野中提供精確且回應性的移動追蹤，這表示不需要在您的空間中的牆上安裝硬體。</span><span class="sxs-lookup"><span data-stu-id="ac362-125">These controllers offer precise and responsive tracking of movement in your field of view using the sensors in the immersive headset, meaning there is no need to install hardware on the walls in your space.</span></span> <span data-ttu-id="ac362-126">每個控制器都有數種輸入方法。</span><span class="sxs-lookup"><span data-stu-id="ac362-126">Each controller features several methods of input.</span></span>

![Windows Mixed Reality 動作控制器](../design/images/winmr-ck-1080x1080-350px.jpg)

## <a name="bluetooth-keyboards"></a><span data-ttu-id="ac362-128">藍牙鍵盤</span><span class="sxs-lookup"><span data-stu-id="ac362-128">Bluetooth keyboards</span></span>

<span data-ttu-id="ac362-129">英文版的繁體中文藍牙鍵盤可以配對，並且可以在任何地方使用全像鍵盤。</span><span class="sxs-lookup"><span data-stu-id="ac362-129">English language Qwerty Bluetooth keyboards can be paired and used anywhere you can use the holographic keyboard.</span></span> <span data-ttu-id="ac362-130">取得高品質的鍵盤會產生差異，因此建議使用 [Microsoft 通用](https://www.microsoft.com/accessories/products/keyboards/universal-foldable-keyboard/gu5-00001) 的可折迭鍵盤或 [Microsoft Designer 藍牙桌面](https://www.microsoft.com/accessories/products/keyboards/designer-bluetooth-desktop/7n9-00001)。</span><span class="sxs-lookup"><span data-stu-id="ac362-130">Getting a quality keyboard makes a difference, so we recommend the [Microsoft Universal Foldable Keyboard](https://www.microsoft.com/accessories/products/keyboards/universal-foldable-keyboard/gu5-00001) or the [Microsoft Designer Bluetooth Desktop](https://www.microsoft.com/accessories/products/keyboards/designer-bluetooth-desktop/7n9-00001).</span></span>

## <a name="bluetooth-gamepads"></a><span data-ttu-id="ac362-131">藍牙 gamepads</span><span class="sxs-lookup"><span data-stu-id="ac362-131">Bluetooth gamepads</span></span>

<span data-ttu-id="ac362-132">您可以使用具有應用程式的控制器，以及專門啟用遊戲台支援的遊戲。</span><span class="sxs-lookup"><span data-stu-id="ac362-132">You can use a controller with apps and games that specifically enable gamepad support.</span></span> <span data-ttu-id="ac362-133">Gamepads 無法用來控制 HoloLens 使用者介面。</span><span class="sxs-lookup"><span data-stu-id="ac362-133">Gamepads cannot be used to control the HoloLens user interface.</span></span>

<span data-ttu-id="ac362-134">隨附于 Xbox One S 或銷售作為 Xbox One S 功能之配件的 Xbox 無線控制器，可讓它們與 HoloLens 和沉浸式耳機一起使用。</span><span class="sxs-lookup"><span data-stu-id="ac362-134">Xbox Wireless Controllers that come with the Xbox One S or sold as accessories for the Xbox One S feature Bluetooth connectivity that enable them to be used with HoloLens and immersive headsets.</span></span> <span data-ttu-id="ac362-135">Xbox 無線控制器 [必須先更新](https://support.xbox.com/xbox-one/accessories/update-controller-for-stereo-headset-adapter) ，才能搭配 HoloLens 使用。</span><span class="sxs-lookup"><span data-stu-id="ac362-135">The Xbox Wireless Controller [must be updated](https://support.xbox.com/xbox-one/accessories/update-controller-for-stereo-headset-adapter) before it can be used with HoloLens.</span></span>

<span data-ttu-id="ac362-136">其他品牌的藍牙 gamepads 可與 Windows Mixed Reality 裝置搭配運作，但支援會因應用程式而異。</span><span class="sxs-lookup"><span data-stu-id="ac362-136">Other brands of Bluetooth gamepads may work with Windows Mixed Reality devices, but support will vary by application.</span></span>

## <a name="other-bluetooth-accessories"></a><span data-ttu-id="ac362-137">其他藍牙配件</span><span class="sxs-lookup"><span data-stu-id="ac362-137">Other Bluetooth accessories</span></span>

<span data-ttu-id="ac362-138">只要周邊支援 Bluetooth HID 或 GATT 設定檔，就能夠與 HoloLens 配對。</span><span class="sxs-lookup"><span data-stu-id="ac362-138">As long as the peripheral supports either the Bluetooth HID or GATT profiles, it will be able to pair with HoloLens.</span></span> <span data-ttu-id="ac362-139">除了鍵盤、滑鼠和 HoloLens Clicker 以外的其他 Bluetooth HID 和 GATT 裝置，可能需要 Microsoft HoloLens 上的隨附應用程式才能完整運作。</span><span class="sxs-lookup"><span data-stu-id="ac362-139">Other Bluetooth HID and GATT devices besides keyboard, mice, and the HoloLens Clicker may require a companion application on Microsoft HoloLens to be fully functional.</span></span>

<span data-ttu-id="ac362-140">不支援的週邊設備包括：</span><span class="sxs-lookup"><span data-stu-id="ac362-140">Unsupported peripherals include:</span></span>

* <span data-ttu-id="ac362-141">不支援藍牙音訊設定檔中的週邊設備。</span><span class="sxs-lookup"><span data-stu-id="ac362-141">Peripherals in the Bluetooth audio profiles are not supported.</span></span>
* <span data-ttu-id="ac362-142">像是喇叭和耳機等藍牙音訊裝置在 [設定] 應用程式中可能會顯示為可用，但不支援使用 Microsoft HoloLens 作為音訊端點。</span><span class="sxs-lookup"><span data-stu-id="ac362-142">Bluetooth audio devices such as speakers and headsets may appear as available in the Settings app, but are not supported to be used with Microsoft HoloLens as an audio endpoint.</span></span>
* <span data-ttu-id="ac362-143">啟用 Bluetooth 的電話和電腦不支援配對並用於檔案傳輸。</span><span class="sxs-lookup"><span data-stu-id="ac362-143">Bluetooth-enabled phones and PCs are not supported to be paired and used for file transfer.</span></span>

## <a name="unpairing-a-bluetooth-peripheral"></a><span data-ttu-id="ac362-144">取消配對藍牙週邊設備</span><span class="sxs-lookup"><span data-stu-id="ac362-144">Unpairing a Bluetooth peripheral</span></span>

1. <span data-ttu-id="ac362-145">從 [開始] 功能表開啟 [ **設定** ] 應用程式</span><span class="sxs-lookup"><span data-stu-id="ac362-145">From the Start Menu, open the **Settings** app</span></span>
2. <span data-ttu-id="ac362-146">移至 **裝置**</span><span class="sxs-lookup"><span data-stu-id="ac362-146">Go to **Devices**</span></span>
3. <span data-ttu-id="ac362-147">開啟藍牙無線電（如果它已關閉）</span><span class="sxs-lookup"><span data-stu-id="ac362-147">Turn on the Bluetooth radio if it is off</span></span>
4. <span data-ttu-id="ac362-148">在可用的藍牙裝置清單中尋找您的裝置</span><span class="sxs-lookup"><span data-stu-id="ac362-148">Find your device in the list of available Bluetooth devices</span></span>
5. <span data-ttu-id="ac362-149">從清單中選取您的裝置，然後選取 [ **移除** ] 按鈕</span><span class="sxs-lookup"><span data-stu-id="ac362-149">Select your device from the list and then select the **Remove** button</span></span>
