---
title: 感知模擬
description: 使用認知模擬程式庫將沉浸式應用程式的模擬輸入自動化的指南
author: pbarnettms
ms.author: pbarnett
ms.date: 05/12/2020
ms.topic: article
keywords: HoloLens、模擬、測試
ms.openlocfilehash: 64028c3a1ad58cecfebc93aee325b73c3a6a649a
ms.sourcegitcommit: c41372e0c6ca265f599bff309390982642d628b8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/15/2020
ms.locfileid: "97530399"
---
# <a name="perception-simulation"></a><span data-ttu-id="952b7-104">感知模擬</span><span class="sxs-lookup"><span data-stu-id="952b7-104">Perception simulation</span></span>

<span data-ttu-id="952b7-105">您是否要為您的應用程式建立自動化測試？</span><span class="sxs-lookup"><span data-stu-id="952b7-105">Do you want to build an automated test for your app?</span></span> <span data-ttu-id="952b7-106">您是否想要讓您的測試超越元件層級的單元測試，並讓您的應用程式端對端實際執行？</span><span class="sxs-lookup"><span data-stu-id="952b7-106">Do you want your tests to go beyond component-level unit testing and really exercise your app end-to-end?</span></span> <span data-ttu-id="952b7-107">認知模擬就是您要尋找的內容。</span><span class="sxs-lookup"><span data-stu-id="952b7-107">Perception Simulation is what you're looking for.</span></span> <span data-ttu-id="952b7-108">認知模擬程式庫會將人類和世界的輸入資料傳送到您的應用程式，以便讓您的測試自動化。</span><span class="sxs-lookup"><span data-stu-id="952b7-108">The Perception Simulation library sends human and world input data to your app so you can automate your tests.</span></span> <span data-ttu-id="952b7-109">例如，您可以模擬人們的輸入，查看特定、可重複的位置，然後使用手勢或移動控制器。</span><span class="sxs-lookup"><span data-stu-id="952b7-109">For example, you can simulate the input of a human looking to a specific, repeatable position and then use a gesture or motion controller.</span></span>

<span data-ttu-id="952b7-110">認知模擬可將這類模擬輸入傳送給實體 HoloLens、HoloLens 模擬器 (第一代) 、HoloLens 2 模擬器或已安裝混合實境入口的電腦。</span><span class="sxs-lookup"><span data-stu-id="952b7-110">Perception Simulation can send simulated input like this to a physical HoloLens, the HoloLens emulator (first gen), the HoloLens 2 Emulator, or a PC with Mixed Reality Portal installed.</span></span> <span data-ttu-id="952b7-111">認知模擬會略過混合現實裝置上的即時感應器，並將模擬的輸入傳送給裝置上執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="952b7-111">Perception Simulation bypasses the live sensors on a Mixed Reality device and sends simulated input to applications running on the device.</span></span> <span data-ttu-id="952b7-112">應用程式會透過其一律使用的相同 Api 來接收這些輸入事件，而且無法分辨與實際感應器的執行與認知模擬之間的差異。</span><span class="sxs-lookup"><span data-stu-id="952b7-112">Applications receive these input events through the same APIs they always use and can't tell the difference between running with real sensors versus Perception Simulation.</span></span> <span data-ttu-id="952b7-113">認知模擬是 HoloLens 模擬器用來將模擬的輸入傳送到 HoloLens 虛擬機器的相同技術。</span><span class="sxs-lookup"><span data-stu-id="952b7-113">Perception Simulation is the same technology used by the HoloLens emulators to send simulated input to the HoloLens Virtual Machine.</span></span>

<span data-ttu-id="952b7-114">若要開始在程式碼中使用模擬，請先建立 IPerceptionSimulationManager 物件。</span><span class="sxs-lookup"><span data-stu-id="952b7-114">To begin using simulation in your code, start by creating an IPerceptionSimulationManager object.</span></span> <span data-ttu-id="952b7-115">您可以從該物件發出命令，以控制模擬「人」的屬性，包括頭部位置、手形位置和手勢。</span><span class="sxs-lookup"><span data-stu-id="952b7-115">From that object, you can issue commands to control properties of a simulated "human", including head position, hand position, and gestures.</span></span> <span data-ttu-id="952b7-116">您也可以啟用和操作移動控制器。</span><span class="sxs-lookup"><span data-stu-id="952b7-116">You can also enable and manipulate motion controllers.</span></span>

## <a name="setting-up-a-visual-studio-project-for-perception-simulation"></a><span data-ttu-id="952b7-117">設定 Visual Studio 專案以進行感知模擬</span><span class="sxs-lookup"><span data-stu-id="952b7-117">Setting Up a Visual Studio Project for Perception Simulation</span></span>
1. <span data-ttu-id="952b7-118">在您的開發電腦上[安裝 HoloLens 模擬器](../install-the-tools.md)。</span><span class="sxs-lookup"><span data-stu-id="952b7-118">[Install the HoloLens emulator](../install-the-tools.md) on your development PC.</span></span> <span data-ttu-id="952b7-119">模擬器包含您用來感知模擬的程式庫。</span><span class="sxs-lookup"><span data-stu-id="952b7-119">The emulator includes the libraries you' use for Perception Simulation.</span></span>
2. <span data-ttu-id="952b7-120">建立新的 Visual Studio c # 桌面專案 (主控台專案的效果很好，可以開始) 。</span><span class="sxs-lookup"><span data-stu-id="952b7-120">Create a new Visual Studio C# desktop project (a Console Project works great to get started).</span></span>
3. <span data-ttu-id="952b7-121">將下列二進位檔新增至您的專案，做為參考 (專案 >的 [新增 >參考 ...] ) 。您可以在% ProgramFiles (x86) % \ Microsoft XDE \\ (版本) 中找到它們，例如 (模擬器的 **% ProgramFiles) x86 HoloLens 2% \ microsoft XDE \\ 10.0.18362.0** 。</span><span class="sxs-lookup"><span data-stu-id="952b7-121">Add the following binaries to your project as references (Project->Add->Reference...). You can find them in %ProgramFiles(x86)%\Microsoft XDE\\(version), such as **%ProgramFiles(x86)%\Microsoft XDE\\10.0.18362.0** for the HoloLens 2 Emulator.</span></span>  <span data-ttu-id="952b7-122"> (注意：雖然二進位檔是 HoloLens 2 模擬器的一部分，但它們也適用于桌面上的 Windows Mixed Reality。 ) a。</span><span class="sxs-lookup"><span data-stu-id="952b7-122">(Note: although the binaries are part of the HoloLens 2 Emulator, they also work for Windows Mixed Reality on the desktop.) a.</span></span> <span data-ttu-id="952b7-123">用於感知模擬的 PerceptionSimulationManager.Interop.dll 受控 c # 包裝函式。</span><span class="sxs-lookup"><span data-stu-id="952b7-123">PerceptionSimulationManager.Interop.dll - Managed C# wrapper for Perception Simulation.</span></span>
    <span data-ttu-id="952b7-124">b.</span><span class="sxs-lookup"><span data-stu-id="952b7-124">b.</span></span> <span data-ttu-id="952b7-125">PerceptionSimulationRest.dll 程式庫，可將 web 通訊端通道設定為 HoloLens 或模擬器。</span><span class="sxs-lookup"><span data-stu-id="952b7-125">PerceptionSimulationRest.dll - Library for setting up a web-socket communication channel to the HoloLens or emulator.</span></span>
    <span data-ttu-id="952b7-126">c.</span><span class="sxs-lookup"><span data-stu-id="952b7-126">c.</span></span> <span data-ttu-id="952b7-127">SimulationStream.Interop.dll-用於模擬的共用類型。</span><span class="sxs-lookup"><span data-stu-id="952b7-127">SimulationStream.Interop.dll - Shared types for simulation.</span></span>
4. <span data-ttu-id="952b7-128">將「執行二進位 PerceptionSimulationManager.dll」新增至您的專案 a。</span><span class="sxs-lookup"><span data-stu-id="952b7-128">Add the implementation binary PerceptionSimulationManager.dll to your project a.</span></span> <span data-ttu-id="952b7-129">首先，將它新增為專案的二進位檔 (專案 >加入 >現有專案 ... ) 。將它儲存為連結，讓它不會將它複製到您的專案源資料夾。</span><span class="sxs-lookup"><span data-stu-id="952b7-129">First add it as a binary to the project (Project->Add->Existing Item...). Save it as a link so that it doesn't copy it to your project source folder.</span></span> <span data-ttu-id="952b7-130">![將 PerceptionSimulationManager.dll 新增至專案做為連結 ](images/saveaslink.png) b。</span><span class="sxs-lookup"><span data-stu-id="952b7-130">![Add PerceptionSimulationManager.dll to the project as a link](images/saveaslink.png) b.</span></span> <span data-ttu-id="952b7-131">然後，請確定它會複製到組建上的輸出檔案夾中。</span><span class="sxs-lookup"><span data-stu-id="952b7-131">Then make sure that it gets copied to your output folder on build.</span></span> <span data-ttu-id="952b7-132">這是在二進位檔的屬性工作表中。</span><span class="sxs-lookup"><span data-stu-id="952b7-132">This is in the property sheet for the binary.</span></span> <span data-ttu-id="952b7-133">![標示 PerceptionSimulationManager.dll 複製到輸出目錄](images/copyalways.png)</span><span class="sxs-lookup"><span data-stu-id="952b7-133">![Mark PerceptionSimulationManager.dll to copy to the output directory](images/copyalways.png)</span></span>
5. <span data-ttu-id="952b7-134">將使用中的方案平臺設定為 x64。</span><span class="sxs-lookup"><span data-stu-id="952b7-134">Set your active solution platform to x64.</span></span>  <span data-ttu-id="952b7-135"> (使用 Configuration Manager 來建立 x64 的平臺專案（如果尚不存在的話）。 ) </span><span class="sxs-lookup"><span data-stu-id="952b7-135">(Use the Configuration Manager to create a Platform entry for x64 if one doesn't already exist.)</span></span>

## <a name="creating-an-iperceptionsimulation-manager-object"></a><span data-ttu-id="952b7-136">建立 IPerceptionSimulation 管理員物件</span><span class="sxs-lookup"><span data-stu-id="952b7-136">Creating an IPerceptionSimulation Manager Object</span></span>

<span data-ttu-id="952b7-137">若要控制模擬，您要對從 IPerceptionSimulationManager 物件取出的物件發出更新。</span><span class="sxs-lookup"><span data-stu-id="952b7-137">To control simulation, you'll issue updates to objects retrieved from an IPerceptionSimulationManager object.</span></span> <span data-ttu-id="952b7-138">第一個步驟是取得該物件，並將它連接到您的目標裝置或模擬器。</span><span class="sxs-lookup"><span data-stu-id="952b7-138">The first step is to get that object and connect it to your target device or emulator.</span></span> <span data-ttu-id="952b7-139">您可以按一下[工具列](using-the-hololens-emulator.md)中的裝置入口網站按鈕，取得模擬器的 IP 位址</span><span class="sxs-lookup"><span data-stu-id="952b7-139">You can get the IP address of your emulator by clicking on the Device Portal button in the [toolbar](using-the-hololens-emulator.md)</span></span>

<span data-ttu-id="952b7-140">![開啟裝置入口網站圖示 ](images/emulator-deviceportal.png) **開啟裝置入口網站**：在模擬器中開啟 HoloLens OS 的 Windows 裝置入口網站。</span><span class="sxs-lookup"><span data-stu-id="952b7-140">![Open Device Portal icon](images/emulator-deviceportal.png) **Open Device Portal**: Open the Windows Device Portal for the HoloLens OS in the emulator.</span></span>  <span data-ttu-id="952b7-141">針對 Windows Mixed Reality，可以在 [設定 & 安全性] 底下的 [設定] 應用程式中，然後在 [啟用裝置入口網站] 下的 [使用下列方式連接：] 區段中，取得「適用于開發人員」。</span><span class="sxs-lookup"><span data-stu-id="952b7-141">For Windows Mixed Reality, this can be retrieved in the Settings app under "Update & Security", then "For developers" in the "Connect using:" section under "Enable Device Portal."</span></span>  <span data-ttu-id="952b7-142">請務必記下 IP 位址和埠。</span><span class="sxs-lookup"><span data-stu-id="952b7-142">Be sure to note both the IP address and port.</span></span>

<span data-ttu-id="952b7-143">首先，您會呼叫 RestSimulationStreamSink 來取得 RestSimulationStreamSink 物件。</span><span class="sxs-lookup"><span data-stu-id="952b7-143">First, you'll call RestSimulationStreamSink.Create to get a RestSimulationStreamSink object.</span></span> <span data-ttu-id="952b7-144">這是您將透過 HTTP 連接控制的目標裝置或模擬器。</span><span class="sxs-lookup"><span data-stu-id="952b7-144">This is the target device or emulator that you'll control over an http connection.</span></span> <span data-ttu-id="952b7-145">您的命令將會傳遞至裝置或模擬器上執行的 [Windows 裝置入口網站](using-the-windows-device-portal.md) ，並由其處理。</span><span class="sxs-lookup"><span data-stu-id="952b7-145">Your commands will be passed to and handled by the [Windows Device Portal](using-the-windows-device-portal.md) running on the device or emulator.</span></span> <span data-ttu-id="952b7-146">建立物件所需的四個參數如下：</span><span class="sxs-lookup"><span data-stu-id="952b7-146">The four parameters you'll need to create an object are:</span></span>
* <span data-ttu-id="952b7-147">Uri uri-目標裝置的 IP 位址 (例如 " https://123.123.123.123 " 或 " https://123.123.123.123:50080 " ) </span><span class="sxs-lookup"><span data-stu-id="952b7-147">Uri uri - IP address of the target device (e.g., "https://123.123.123.123" or "https://123.123.123.123:50080")</span></span>
* <span data-ttu-id="952b7-148">NetworkCredential 認證-用來連接到目標裝置或模擬器上 [Windows 裝置入口網站](using-the-windows-device-portal.md) 的使用者名稱/密碼。</span><span class="sxs-lookup"><span data-stu-id="952b7-148">System.Net.NetworkCredential credentials - Username/password for connecting to the [Windows Device Portal](using-the-windows-device-portal.md) on the target device or emulator.</span></span> <span data-ttu-id="952b7-149">如果您是透過本機位址連接到模擬器 (例如，168。*.*.\* ) 在同一部電腦上，將會接受任何認證。</span><span class="sxs-lookup"><span data-stu-id="952b7-149">If you're connecting to the emulator via its local address (e.g., 168.*.*.\*) on the same PC, any credentials will be accepted.</span></span>
* <span data-ttu-id="952b7-150">bool 標準-True 表示正常優先順序，false 表示低優先順序。</span><span class="sxs-lookup"><span data-stu-id="952b7-150">bool normal - True for normal priority, false for low priority.</span></span> <span data-ttu-id="952b7-151">您通常會想要在測試案例中將此設為 *true* ，以允許您的測試取得控制權。</span><span class="sxs-lookup"><span data-stu-id="952b7-151">You generally want to set this to *true* for test scenarios, which allows your test to take control.</span></span>  <span data-ttu-id="952b7-152">模擬器與 Windows Mixed Reality 模擬會使用低優先順序的連接。</span><span class="sxs-lookup"><span data-stu-id="952b7-152">The emulator and Windows Mixed Reality simulation use low-priority connections.</span></span>  <span data-ttu-id="952b7-153">如果您的測試也會使用低優先順序的連接，則最近建立的連接將會受到控制。</span><span class="sxs-lookup"><span data-stu-id="952b7-153">If your test also uses a low-priority connection, the most recently established connection will be in control.</span></span>
* <span data-ttu-id="952b7-154">CancellationToken token-Token，以取消非同步作業。</span><span class="sxs-lookup"><span data-stu-id="952b7-154">System.Threading.CancellationToken token - Token to cancel the async operation.</span></span>

<span data-ttu-id="952b7-155">其次，您將建立 IPerceptionSimulationManager。</span><span class="sxs-lookup"><span data-stu-id="952b7-155">Second, you'll create the IPerceptionSimulationManager.</span></span> <span data-ttu-id="952b7-156">這是您用來控制模擬的物件。</span><span class="sxs-lookup"><span data-stu-id="952b7-156">This is the object you use to control simulation.</span></span> <span data-ttu-id="952b7-157">這也必須在非同步方法中完成。</span><span class="sxs-lookup"><span data-stu-id="952b7-157">This must also be done in an async method.</span></span>

## <a name="control-the-simulated-human"></a><span data-ttu-id="952b7-158">控制模擬的人</span><span class="sxs-lookup"><span data-stu-id="952b7-158">Control the simulated Human</span></span>

<span data-ttu-id="952b7-159">IPerceptionSimulationManager 具有會傳回 ISimulatedHuman 物件的人類屬性。</span><span class="sxs-lookup"><span data-stu-id="952b7-159">An IPerceptionSimulationManager has a Human property that returns an ISimulatedHuman object.</span></span> <span data-ttu-id="952b7-160">若要控制模擬的人，請在此物件上執行作業。</span><span class="sxs-lookup"><span data-stu-id="952b7-160">To control the simulated human, perform operations on this object.</span></span> <span data-ttu-id="952b7-161">例如：</span><span class="sxs-lookup"><span data-stu-id="952b7-161">For example:</span></span>

```
manager.Human.Move(new Vector3(0.1f, 0.0f, 0.0f))
```

## <a name="basic-sample-c-console-application"></a><span data-ttu-id="952b7-162">基本的 c # 主控台應用程式範例</span><span class="sxs-lookup"><span data-stu-id="952b7-162">Basic Sample C# console application</span></span>

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.PerceptionSimulation;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            Task.Run(async () =>
            {
                RestSimulationStreamSink sink = null;
                CancellationToken token = new System.Threading.CancellationToken();

                try
                {
                    sink = await RestSimulationStreamSink.Create(
                        // use the IP address for your device/emulator
                        new Uri("https://169.254.227.115"),
                        // no credentials are needed for the emulator
                        new System.Net.NetworkCredential("", ""),
                        // normal priorty
                        true,
                        // cancel token
                        token);

                    IPerceptionSimulationManager manager = PerceptionSimulationManager.CreatePerceptionSimulationManager(sink);
                }
                catch (Exception e)
                {
                    Console.WriteLine(e);
                }

                // Always close the sink to return control to the previous application.
                if (sink != null)
                {
                    await sink.Close(token);
                }
            });

            // If main exits, the process exits.  
            Console.WriteLine("Press any key to exit...");
            Console.ReadLine();
        }
    }
}
```

## <a name="extended-sample-c-console-application"></a><span data-ttu-id="952b7-163">擴充的範例 c # 主控台應用程式</span><span class="sxs-lookup"><span data-stu-id="952b7-163">Extended Sample C# console application</span></span>

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.PerceptionSimulation;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            RestSimulationStreamSink sink = null;
            CancellationToken token = new System.Threading.CancellationToken();

            Task.Run(async () =>
            {
                try
                {
                    sink = await RestSimulationStreamSink.Create(
                        // use the IP address for your device/emulator
                        new Uri("https://169.254.227.115"),
                        // no credentials are needed for the emulator
                        new System.Net.NetworkCredential("", ""),
                        // normal priorty
                        true,
                        // cancel token
                        token);

                    IPerceptionSimulationManager manager = PerceptionSimulationManager.CreatePerceptionSimulationManager(sink);

                    // Now, we'll simulate a sequence of actions.
                    // Sleeps in-between each action give time to the system
                    // to be able to properly react.
                    // This is just an example. A proper automated test should verify
                    // that the app has behaved correctly
                    // before proceeding to the next step, instead of using Sleeps.

                    // Activate the right hand
                    manager.Human.RightHand.Activated = true;

                    // Simulate Bloom gesture, which should cause Shell to disappear
                    manager.Human.RightHand.PerformGesture(SimulatedGesture.Home);
                    Thread.Sleep(2000);

                    // Simulate Bloom gesture again... this time, Shell should reappear
                    manager.Human.RightHand.PerformGesture(SimulatedGesture.Home);
                    Thread.Sleep(2000);

                    // Simulate a Head rotation down around the X axis
                    // This should cause gaze to aim about the center of the screen
                    manager.Human.Head.Rotate(new Rotation3(0.04f, 0.0f, 0.0f));
                    Thread.Sleep(300);

                    // Simulate a finger press & release
                    // Should cause a tap on the center tile, thus launching it
                    manager.Human.RightHand.PerformGesture(SimulatedGesture.FingerPressed);
                    Thread.Sleep(300);
                    manager.Human.RightHand.PerformGesture(SimulatedGesture.FingerReleased);
                    Thread.Sleep(2000);

                    // Simulate a second finger press & release
                    // Should activate the app that was launched when the center tile was clicked
                    manager.Human.RightHand.PerformGesture(SimulatedGesture.FingerPressed);
                    Thread.Sleep(300);
                    manager.Human.RightHand.PerformGesture(SimulatedGesture.FingerReleased);
                    Thread.Sleep(5000);

                    // Simulate a Head rotation towards the upper right corner
                    manager.Human.Head.Rotate(new Rotation3(-0.14f, 0.17f, 0.0f));
                    Thread.Sleep(300);

                    // Simulate a third finger press & release
                    // Should press the Remove button on the app
                    manager.Human.RightHand.PerformGesture(SimulatedGesture.FingerPressed);
                    Thread.Sleep(300);
                    manager.Human.RightHand.PerformGesture(SimulatedGesture.FingerReleased);
                    Thread.Sleep(2000);

                    // Simulate Bloom gesture again... bringing the Shell back once more
                    manager.Human.RightHand.PerformGesture(SimulatedGesture.Home);
                    Thread.Sleep(2000);
                }
                catch (Exception e)
                {
                    Console.WriteLine(e);
                }
            });

            // If main exits, the process exits.  
            Console.WriteLine("Press any key to exit...");
            Console.ReadLine();

            // Always close the sink to return control to the previous application.
            if (sink != null)
            {
                sink.Close(token);
            }
        }
    }
}
```

## <a name="note-on-6-dof-controllers"></a><span data-ttu-id="952b7-164">6 DOF 控制器上的附注</span><span class="sxs-lookup"><span data-stu-id="952b7-164">Note on 6-DOF controllers</span></span>

<span data-ttu-id="952b7-165">在模擬的 6 DOF 控制器上呼叫方法上的任何屬性之前，您必須先啟用控制器。</span><span class="sxs-lookup"><span data-stu-id="952b7-165">Before calling any properties on methods on a simulated 6-DOF controller, you must activate the controller.</span></span>  <span data-ttu-id="952b7-166">若未這麼做，將會導致例外狀況。</span><span class="sxs-lookup"><span data-stu-id="952b7-166">Not doing so will result in an exception.</span></span>  <span data-ttu-id="952b7-167">從 Windows 10 2019 年5月更新開始，只要將 ISimulatedSixDofController 物件的 Status 屬性設為 SimulatedSixDofControllerStatus，就可以安裝和啟用模擬的 DOF 控制器。</span><span class="sxs-lookup"><span data-stu-id="952b7-167">Starting with the Windows 10 May 2019 Update, simulated 6-DOF controllers can be installed and activated by setting the Status property on the ISimulatedSixDofController object to SimulatedSixDofControllerStatus.Active.</span></span>
<span data-ttu-id="952b7-168">在 Windows 10 2018 年10月更新及更早的版本中，您必須先呼叫 PerceptionSimulationDevice 工具（位於 \Windows\System32 資料夾中），以個別安裝模擬的 6 DOF 控制器。</span><span class="sxs-lookup"><span data-stu-id="952b7-168">In the Windows 10 October 2018 Update and earlier, you must separately install a simulated 6-DOF controller first by calling the PerceptionSimulationDevice tool located in the \Windows\System32 folder.</span></span>  <span data-ttu-id="952b7-169">這項工具的使用方式如下：</span><span class="sxs-lookup"><span data-stu-id="952b7-169">The usage of this tool is as follows:</span></span>


```
    PerceptionSimulationDevice.exe <action> 6dof <instance>
```

<span data-ttu-id="952b7-170">例如：</span><span class="sxs-lookup"><span data-stu-id="952b7-170">For example</span></span>

```
    PerceptionSimulationDevice.exe i 6dof 1
```

<span data-ttu-id="952b7-171">支援的動作包括：</span><span class="sxs-lookup"><span data-stu-id="952b7-171">Supported actions are:</span></span>
* <span data-ttu-id="952b7-172">i = 安裝</span><span class="sxs-lookup"><span data-stu-id="952b7-172">i = install</span></span>
* <span data-ttu-id="952b7-173">q = 查詢</span><span class="sxs-lookup"><span data-stu-id="952b7-173">q = query</span></span>
* <span data-ttu-id="952b7-174">r = 移除</span><span class="sxs-lookup"><span data-stu-id="952b7-174">r = remove</span></span>

<span data-ttu-id="952b7-175">支援的實例如下：</span><span class="sxs-lookup"><span data-stu-id="952b7-175">Supported instances are:</span></span>
* <span data-ttu-id="952b7-176">1 = 左方 6 DOF 控制器</span><span class="sxs-lookup"><span data-stu-id="952b7-176">1 = the left 6-DOF controller</span></span>
* <span data-ttu-id="952b7-177">2 = 右邊的 6 DOF 控制器</span><span class="sxs-lookup"><span data-stu-id="952b7-177">2 = the right 6-DOF controller</span></span>

<span data-ttu-id="952b7-178">進程的結束代碼會指出成功 (零傳回值) 或失敗 (非零傳回值) 。</span><span class="sxs-lookup"><span data-stu-id="952b7-178">The exit code of the process will indicate success (a zero return value) or failure (a non-zero return value).</span></span>  <span data-ttu-id="952b7-179">使用 ' q ' 動作來查詢是否已安裝控制器時，如果尚未安裝控制器，則傳回值會是零 (0) ，如果控制器已安裝則為 1 (1) 。</span><span class="sxs-lookup"><span data-stu-id="952b7-179">When using the 'q' action to query whether a controller is installed, the return value will be zero (0) if the controller isn't already installed or one (1) if the controller is installed.</span></span>

<span data-ttu-id="952b7-180">移除 Windows 10 2018 年10月更新或更早版本上的控制器時，請先透過 API 將其狀態設定為 Off，然後再呼叫 PerceptionSimulationDevice 工具。</span><span class="sxs-lookup"><span data-stu-id="952b7-180">When removing a controller on the Windows 10 October 2018 Update or earlier, set its status to Off via the API first, then call the PerceptionSimulationDevice tool.</span></span>

<span data-ttu-id="952b7-181">此工具必須以系統管理員身分執行。</span><span class="sxs-lookup"><span data-stu-id="952b7-181">This tool must be run as Administrator.</span></span>




## <a name="api-reference"></a><span data-ttu-id="952b7-182">應用程式開發介面參考</span><span class="sxs-lookup"><span data-stu-id="952b7-182">API Reference</span></span>

### <a name="microsoftperceptionsimulationsimulateddevicetype"></a><span data-ttu-id="952b7-183">PerceptionSimulation. SimulatedDeviceType</span><span class="sxs-lookup"><span data-stu-id="952b7-183">Microsoft.PerceptionSimulation.SimulatedDeviceType</span></span>

<span data-ttu-id="952b7-184">描述模擬裝置類型</span><span class="sxs-lookup"><span data-stu-id="952b7-184">Describes a simulated device type</span></span>

```
public enum SimulatedDeviceType
{
    Reference = 0
}
```

<span data-ttu-id="952b7-185">**PerceptionSimulation. SimulatedDeviceType 參考**</span><span class="sxs-lookup"><span data-stu-id="952b7-185">**Microsoft.PerceptionSimulation.SimulatedDeviceType.Reference**</span></span>

<span data-ttu-id="952b7-186">虛構的參考裝置，預設值為 PerceptionSimulationManager</span><span class="sxs-lookup"><span data-stu-id="952b7-186">A fictitious reference device, the default for PerceptionSimulationManager</span></span>

### <a name="microsoftperceptionsimulationheadtrackermode"></a><span data-ttu-id="952b7-187">PerceptionSimulation. HeadTrackerMode</span><span class="sxs-lookup"><span data-stu-id="952b7-187">Microsoft.PerceptionSimulation.HeadTrackerMode</span></span>

<span data-ttu-id="952b7-188">描述 head 追蹤器模式</span><span class="sxs-lookup"><span data-stu-id="952b7-188">Describes a head tracker mode</span></span>

```
public enum HeadTrackerMode
{
    Default = 0,
    Orientation = 1,
    Position = 2
}
```

<span data-ttu-id="952b7-189">**PerceptionSimulation. HeadTrackerMode. 預設值**</span><span class="sxs-lookup"><span data-stu-id="952b7-189">**Microsoft.PerceptionSimulation.HeadTrackerMode.Default**</span></span>

<span data-ttu-id="952b7-190">預設標頭追蹤。</span><span class="sxs-lookup"><span data-stu-id="952b7-190">Default Head Tracking.</span></span> <span data-ttu-id="952b7-191">這表示系統可能會根據執行時間條件來選取最佳的標頭追蹤模式。</span><span class="sxs-lookup"><span data-stu-id="952b7-191">This means the system may select the best head tracking mode based upon runtime conditions.</span></span>

<span data-ttu-id="952b7-192">**PerceptionSimulation. HeadTrackerMode 的方向**</span><span class="sxs-lookup"><span data-stu-id="952b7-192">**Microsoft.PerceptionSimulation.HeadTrackerMode.Orientation**</span></span>

<span data-ttu-id="952b7-193">僅限方向的標頭追蹤。</span><span class="sxs-lookup"><span data-stu-id="952b7-193">Orientation Only Head Tracking.</span></span> <span data-ttu-id="952b7-194">這表示追蹤的位置可能不可靠，而且某些相依于 head 位置的功能可能無法使用。</span><span class="sxs-lookup"><span data-stu-id="952b7-194">This means that the tracked position may not be reliable, and some functionality dependent on head position may not be available.</span></span>

<span data-ttu-id="952b7-195">**PerceptionSimulation. HeadTrackerMode. Position**</span><span class="sxs-lookup"><span data-stu-id="952b7-195">**Microsoft.PerceptionSimulation.HeadTrackerMode.Position**</span></span>

<span data-ttu-id="952b7-196">位置標頭追蹤。</span><span class="sxs-lookup"><span data-stu-id="952b7-196">Positional Head Tracking.</span></span> <span data-ttu-id="952b7-197">這表示追蹤的標頭位置和方向都是可靠的</span><span class="sxs-lookup"><span data-stu-id="952b7-197">This means that the tracked head position and orientation are both reliable</span></span>

### <a name="microsoftperceptionsimulationsimulatedgesture"></a><span data-ttu-id="952b7-198">PerceptionSimulation. SimulatedGesture</span><span class="sxs-lookup"><span data-stu-id="952b7-198">Microsoft.PerceptionSimulation.SimulatedGesture</span></span>

<span data-ttu-id="952b7-199">描述模擬的手勢</span><span class="sxs-lookup"><span data-stu-id="952b7-199">Describes a simulated gesture</span></span>

```
public enum SimulatedGesture
{
    None = 0,
    FingerPressed = 1,
    FingerReleased = 2,
    Home = 4,
    Max = Home
}
```

<span data-ttu-id="952b7-200">**PerceptionSimulation. SimulatedGesture. None**</span><span class="sxs-lookup"><span data-stu-id="952b7-200">**Microsoft.PerceptionSimulation.SimulatedGesture.None**</span></span>

<span data-ttu-id="952b7-201">用來表示沒有手勢的 sentinel 值。</span><span class="sxs-lookup"><span data-stu-id="952b7-201">A sentinel value used to indicate no gestures.</span></span>

<span data-ttu-id="952b7-202">**PerceptionSimulation. SimulatedGesture. FingerPressed**</span><span class="sxs-lookup"><span data-stu-id="952b7-202">**Microsoft.PerceptionSimulation.SimulatedGesture.FingerPressed**</span></span>

<span data-ttu-id="952b7-203">以手指按的手勢。</span><span class="sxs-lookup"><span data-stu-id="952b7-203">A finger pressed gesture.</span></span>

<span data-ttu-id="952b7-204">**PerceptionSimulation. SimulatedGesture. FingerReleased**</span><span class="sxs-lookup"><span data-stu-id="952b7-204">**Microsoft.PerceptionSimulation.SimulatedGesture.FingerReleased**</span></span>

<span data-ttu-id="952b7-205">手指放開手勢。</span><span class="sxs-lookup"><span data-stu-id="952b7-205">A finger released gesture.</span></span>

<span data-ttu-id="952b7-206">**PerceptionSimulation. SimulatedGesture 首頁**</span><span class="sxs-lookup"><span data-stu-id="952b7-206">**Microsoft.PerceptionSimulation.SimulatedGesture.Home**</span></span>

<span data-ttu-id="952b7-207">Home/system 手勢。</span><span class="sxs-lookup"><span data-stu-id="952b7-207">The home/system gesture.</span></span>

<span data-ttu-id="952b7-208">**PerceptionSimulation. SimulatedGesture. Max**</span><span class="sxs-lookup"><span data-stu-id="952b7-208">**Microsoft.PerceptionSimulation.SimulatedGesture.Max**</span></span>

<span data-ttu-id="952b7-209">有效手勢的最大值。</span><span class="sxs-lookup"><span data-stu-id="952b7-209">The maximum valid gesture.</span></span>

### <a name="microsoftperceptionsimulationsimulatedsixdofcontrollerstatus"></a><span data-ttu-id="952b7-210">PerceptionSimulation. SimulatedSixDofControllerStatus</span><span class="sxs-lookup"><span data-stu-id="952b7-210">Microsoft.PerceptionSimulation.SimulatedSixDofControllerStatus</span></span>

<span data-ttu-id="952b7-211">模擬 6 DOF 控制器的可能狀態。</span><span class="sxs-lookup"><span data-stu-id="952b7-211">The possible states of a simulated 6-DOF controller.</span></span>

```
public enum SimulatedSixDofControllerStatus
{
    Off = 0,
    Active = 1,
    TrackingLost = 2,
}
```

<span data-ttu-id="952b7-212">**PerceptionSimulation. SimulatedSixDofControllerStatus. Off**</span><span class="sxs-lookup"><span data-stu-id="952b7-212">**Microsoft.PerceptionSimulation.SimulatedSixDofControllerStatus.Off**</span></span>

<span data-ttu-id="952b7-213">6 DOF 控制器已關閉。</span><span class="sxs-lookup"><span data-stu-id="952b7-213">The 6-DOF controller is turned off.</span></span>

<span data-ttu-id="952b7-214">**PerceptionSimulation. SimulatedSixDofControllerStatus. Active**</span><span class="sxs-lookup"><span data-stu-id="952b7-214">**Microsoft.PerceptionSimulation.SimulatedSixDofControllerStatus.Active**</span></span>

<span data-ttu-id="952b7-215">已開啟並追蹤6個 DOF 控制器。</span><span class="sxs-lookup"><span data-stu-id="952b7-215">The 6-DOF controller is turned on and tracked.</span></span>

<span data-ttu-id="952b7-216">**PerceptionSimulation. SimulatedSixDofControllerStatus. TrackingLost**</span><span class="sxs-lookup"><span data-stu-id="952b7-216">**Microsoft.PerceptionSimulation.SimulatedSixDofControllerStatus.TrackingLost**</span></span>

<span data-ttu-id="952b7-217">6 DOF 控制器已開啟，但無法追蹤。</span><span class="sxs-lookup"><span data-stu-id="952b7-217">The 6-DOF controller is turned on but cannot be tracked.</span></span>

### <a name="microsoftperceptionsimulationsimulatedsixdofcontrollerbutton"></a><span data-ttu-id="952b7-218">PerceptionSimulation. SimulatedSixDofControllerButton</span><span class="sxs-lookup"><span data-stu-id="952b7-218">Microsoft.PerceptionSimulation.SimulatedSixDofControllerButton</span></span>

<span data-ttu-id="952b7-219">模擬的 6 DOF 控制器上支援的按鈕。</span><span class="sxs-lookup"><span data-stu-id="952b7-219">The supported buttons on a simulated 6-DOF controller.</span></span>

```
public enum SimulatedSixDofControllerButton
{
    None = 0,
    Home = 1,
    Menu = 2,
    Grip = 4,
    TouchpadPress = 8,
    Select = 16,
    TouchpadTouch = 32,
    Thumbstick = 64,
    Max = Thumbstick
}
```

<span data-ttu-id="952b7-220">**PerceptionSimulation. SimulatedSixDofControllerButton. None**</span><span class="sxs-lookup"><span data-stu-id="952b7-220">**Microsoft.PerceptionSimulation.SimulatedSixDofControllerButton.None**</span></span>

<span data-ttu-id="952b7-221">用來指出沒有按鈕的 sentinel 值。</span><span class="sxs-lookup"><span data-stu-id="952b7-221">A sentinel value used to indicate no buttons.</span></span>

<span data-ttu-id="952b7-222">**PerceptionSimulation. SimulatedSixDofControllerButton 首頁**</span><span class="sxs-lookup"><span data-stu-id="952b7-222">**Microsoft.PerceptionSimulation.SimulatedSixDofControllerButton.Home**</span></span>

<span data-ttu-id="952b7-223">[首頁] 按鈕已按下。</span><span class="sxs-lookup"><span data-stu-id="952b7-223">The Home button is pressed.</span></span>

<span data-ttu-id="952b7-224">**PerceptionSimulation. SimulatedSixDofControllerButton 功能表**</span><span class="sxs-lookup"><span data-stu-id="952b7-224">**Microsoft.PerceptionSimulation.SimulatedSixDofControllerButton.Menu**</span></span>

<span data-ttu-id="952b7-225">已按下功能表按鈕。</span><span class="sxs-lookup"><span data-stu-id="952b7-225">The Menu button is pressed.</span></span>

<span data-ttu-id="952b7-226">**PerceptionSimulation. SimulatedSixDofControllerButton**</span><span class="sxs-lookup"><span data-stu-id="952b7-226">**Microsoft.PerceptionSimulation.SimulatedSixDofControllerButton.Grip**</span></span>

<span data-ttu-id="952b7-227">按下 [握住] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="952b7-227">The Grip button is pressed.</span></span>

<span data-ttu-id="952b7-228">**PerceptionSimulation. SimulatedSixDofControllerButton. TouchpadPress**</span><span class="sxs-lookup"><span data-stu-id="952b7-228">**Microsoft.PerceptionSimulation.SimulatedSixDofControllerButton.TouchpadPress**</span></span>

<span data-ttu-id="952b7-229">已按下觸控板。</span><span class="sxs-lookup"><span data-stu-id="952b7-229">The TouchPad is pressed.</span></span>

<span data-ttu-id="952b7-230">**PerceptionSimulation. SimulatedSixDofControllerButton. Select**</span><span class="sxs-lookup"><span data-stu-id="952b7-230">**Microsoft.PerceptionSimulation.SimulatedSixDofControllerButton.Select**</span></span>

<span data-ttu-id="952b7-231">已按下 [選取] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="952b7-231">The Select button is pressed.</span></span>

<span data-ttu-id="952b7-232">**PerceptionSimulation. SimulatedSixDofControllerButton. TouchpadTouch**</span><span class="sxs-lookup"><span data-stu-id="952b7-232">**Microsoft.PerceptionSimulation.SimulatedSixDofControllerButton.TouchpadTouch**</span></span>

<span data-ttu-id="952b7-233">觸碰觸控板。</span><span class="sxs-lookup"><span data-stu-id="952b7-233">The TouchPad is touched.</span></span>

<span data-ttu-id="952b7-234">**PerceptionSimulation. SimulatedSixDofControllerButton**</span><span class="sxs-lookup"><span data-stu-id="952b7-234">**Microsoft.PerceptionSimulation.SimulatedSixDofControllerButton.Thumbstick**</span></span>

<span data-ttu-id="952b7-235">已按下操縱杆。</span><span class="sxs-lookup"><span data-stu-id="952b7-235">The Thumbstick is pressed.</span></span>

<span data-ttu-id="952b7-236">**PerceptionSimulation. SimulatedSixDofControllerButton. Max**</span><span class="sxs-lookup"><span data-stu-id="952b7-236">**Microsoft.PerceptionSimulation.SimulatedSixDofControllerButton.Max**</span></span>

<span data-ttu-id="952b7-237">有效的最大按鈕。</span><span class="sxs-lookup"><span data-stu-id="952b7-237">The maximum valid button.</span></span>


### <a name="microsoftperceptionsimulationsimulatedeyescalibrationstate"></a><span data-ttu-id="952b7-238">PerceptionSimulation. SimulatedEyesCalibrationState</span><span class="sxs-lookup"><span data-stu-id="952b7-238">Microsoft.PerceptionSimulation.SimulatedEyesCalibrationState</span></span>

<span data-ttu-id="952b7-239">模擬眼睛的校正狀態</span><span class="sxs-lookup"><span data-stu-id="952b7-239">The calibration state of the simulated eyes</span></span>

```
public enum SimulatedGesture
{
    Unavailable = 0,
    Ready = 1,
    Configuring = 2,
    UserCalibrationNeeded = 3
}
```

<span data-ttu-id="952b7-240">**PerceptionSimulation. SimulatedEyesCalibrationState. 無法使用**</span><span class="sxs-lookup"><span data-stu-id="952b7-240">**Microsoft.PerceptionSimulation.SimulatedEyesCalibrationState.Unavailable**</span></span>

<span data-ttu-id="952b7-241">無法使用眼睛校正。</span><span class="sxs-lookup"><span data-stu-id="952b7-241">The eyes calibration is unavailable.</span></span>

<span data-ttu-id="952b7-242">**PerceptionSimulation. SimulatedEyesCalibrationState. Ready**</span><span class="sxs-lookup"><span data-stu-id="952b7-242">**Microsoft.PerceptionSimulation.SimulatedEyesCalibrationState.Ready**</span></span>

<span data-ttu-id="952b7-243">眼睛已經過校正。</span><span class="sxs-lookup"><span data-stu-id="952b7-243">The eyes have been calibrated.</span></span>  <span data-ttu-id="952b7-244">這是預設值。</span><span class="sxs-lookup"><span data-stu-id="952b7-244">This is the default value.</span></span>

<span data-ttu-id="952b7-245">**Microsoft.PerceptionSimulation.SimulatedEyesCalibrationState.Config是**</span><span class="sxs-lookup"><span data-stu-id="952b7-245">**Microsoft.PerceptionSimulation.SimulatedEyesCalibrationState.Configuring**</span></span>

<span data-ttu-id="952b7-246">正在校正眼睛。</span><span class="sxs-lookup"><span data-stu-id="952b7-246">The eyes are being calibrated.</span></span>

<span data-ttu-id="952b7-247">**PerceptionSimulation. SimulatedEyesCalibrationState. UserCalibrationNeeded**</span><span class="sxs-lookup"><span data-stu-id="952b7-247">**Microsoft.PerceptionSimulation.SimulatedEyesCalibrationState.UserCalibrationNeeded**</span></span>

<span data-ttu-id="952b7-248">需要校正眼睛。</span><span class="sxs-lookup"><span data-stu-id="952b7-248">The eyes need to be calibrated.</span></span>

### <a name="microsoftperceptionsimulationsimulatedhandjointtrackingaccuracy"></a><span data-ttu-id="952b7-249">PerceptionSimulation. SimulatedHandJointTrackingAccuracy</span><span class="sxs-lookup"><span data-stu-id="952b7-249">Microsoft.PerceptionSimulation.SimulatedHandJointTrackingAccuracy</span></span>

<span data-ttu-id="952b7-250">手邊的追蹤準確度。</span><span class="sxs-lookup"><span data-stu-id="952b7-250">The tracking accuracy of a joint of the hand.</span></span>

```
public enum SimulatedHandJointTrackingAccuracy
{
    Unavailable = 0,
    Approximate = 1,
    Visible = 2
}
```

<span data-ttu-id="952b7-251">**PerceptionSimulation. SimulatedHandJointTrackingAccuracy. 無法使用**</span><span class="sxs-lookup"><span data-stu-id="952b7-251">**Microsoft.PerceptionSimulation.SimulatedHandJointTrackingAccuracy.Unavailable**</span></span>

<span data-ttu-id="952b7-252">未追蹤聯合。</span><span class="sxs-lookup"><span data-stu-id="952b7-252">The joint isn't tracked.</span></span>

<span data-ttu-id="952b7-253">**PerceptionSimulation. SimulatedHandJointTrackingAccuracy 近似值**</span><span class="sxs-lookup"><span data-stu-id="952b7-253">**Microsoft.PerceptionSimulation.SimulatedHandJointTrackingAccuracy.Approximate**</span></span>

<span data-ttu-id="952b7-254">會推斷聯合位置。</span><span class="sxs-lookup"><span data-stu-id="952b7-254">The joint position is inferred.</span></span>

<span data-ttu-id="952b7-255">**PerceptionSimulation. SimulatedHandJointTrackingAccuracy 可見**</span><span class="sxs-lookup"><span data-stu-id="952b7-255">**Microsoft.PerceptionSimulation.SimulatedHandJointTrackingAccuracy.Visible**</span></span>

<span data-ttu-id="952b7-256">聯合已完全追蹤。</span><span class="sxs-lookup"><span data-stu-id="952b7-256">The joint is fully tracked.</span></span>

### <a name="microsoftperceptionsimulationsimulatedhandpose"></a><span data-ttu-id="952b7-257">PerceptionSimulation. SimulatedHandPose</span><span class="sxs-lookup"><span data-stu-id="952b7-257">Microsoft.PerceptionSimulation.SimulatedHandPose</span></span>

<span data-ttu-id="952b7-258">手邊的追蹤準確度。</span><span class="sxs-lookup"><span data-stu-id="952b7-258">The tracking accuracy of a joint of the hand.</span></span>

```
public enum SimulatedHandPose
{
    Closed = 0,
    Open = 1,
    Point = 2,
    Pinch = 3,
    Max = Pinch
}
```

<span data-ttu-id="952b7-259">**PerceptionSimulation. SimulatedHandPose. 關閉**</span><span class="sxs-lookup"><span data-stu-id="952b7-259">**Microsoft.PerceptionSimulation.SimulatedHandPose.Closed**</span></span>

<span data-ttu-id="952b7-260">手形的手指設定為反映封閉的姿勢。</span><span class="sxs-lookup"><span data-stu-id="952b7-260">The hand's finger joints are configured to reflect a closed pose.</span></span>

<span data-ttu-id="952b7-261">**PerceptionSimulation. SimulatedHandPose. Open**</span><span class="sxs-lookup"><span data-stu-id="952b7-261">**Microsoft.PerceptionSimulation.SimulatedHandPose.Open**</span></span>

<span data-ttu-id="952b7-262">手形的手指設定為反映開放式姿勢。</span><span class="sxs-lookup"><span data-stu-id="952b7-262">The hand's finger joints are configured to reflect an open pose.</span></span>

<span data-ttu-id="952b7-263">**PerceptionSimulation. SimulatedHandPose. Point**</span><span class="sxs-lookup"><span data-stu-id="952b7-263">**Microsoft.PerceptionSimulation.SimulatedHandPose.Point**</span></span>

<span data-ttu-id="952b7-264">手形的手指會設定為反映指標姿勢。</span><span class="sxs-lookup"><span data-stu-id="952b7-264">The hand's finger joints are configured to reflect a pointing pose.</span></span>

<span data-ttu-id="952b7-265">**PerceptionSimulation. SimulatedHandPose**</span><span class="sxs-lookup"><span data-stu-id="952b7-265">**Microsoft.PerceptionSimulation.SimulatedHandPose.Pinch**</span></span>

<span data-ttu-id="952b7-266">手形的手指設定為反映捏合姿勢。</span><span class="sxs-lookup"><span data-stu-id="952b7-266">The hand's finger joints are configured to reflect a pinching pose.</span></span>

<span data-ttu-id="952b7-267">**PerceptionSimulation. SimulatedHandPose. Max**</span><span class="sxs-lookup"><span data-stu-id="952b7-267">**Microsoft.PerceptionSimulation.SimulatedHandPose.Max**</span></span>

<span data-ttu-id="952b7-268">SimulatedHandPose 的最大有效值。</span><span class="sxs-lookup"><span data-stu-id="952b7-268">The maximum valid value for SimulatedHandPose.</span></span>


### <a name="microsoftperceptionsimulationplaybackstate"></a><span data-ttu-id="952b7-269">PerceptionSimulation. PlaybackState</span><span class="sxs-lookup"><span data-stu-id="952b7-269">Microsoft.PerceptionSimulation.PlaybackState</span></span>

<span data-ttu-id="952b7-270">描述播放的狀態。</span><span class="sxs-lookup"><span data-stu-id="952b7-270">Describes the state of a playback.</span></span>

```
public enum PlaybackState
{
    Stopped = 0,
    Playing = 1,
    Paused = 2,
    End = 3,
}
```

<span data-ttu-id="952b7-271">**PerceptionSimulation. PlaybackState. 已停止**</span><span class="sxs-lookup"><span data-stu-id="952b7-271">**Microsoft.PerceptionSimulation.PlaybackState.Stopped**</span></span>

<span data-ttu-id="952b7-272">錄製目前已停止並可供播放。</span><span class="sxs-lookup"><span data-stu-id="952b7-272">The recording is currently stopped and ready for playback.</span></span>

<span data-ttu-id="952b7-273">**PerceptionSimulation. PlaybackState. 播放**</span><span class="sxs-lookup"><span data-stu-id="952b7-273">**Microsoft.PerceptionSimulation.PlaybackState.Playing**</span></span>

<span data-ttu-id="952b7-274">錄製現正播放中。</span><span class="sxs-lookup"><span data-stu-id="952b7-274">The recording is currently playing.</span></span>

<span data-ttu-id="952b7-275">**PerceptionSimulation. PlaybackState. 暫停**</span><span class="sxs-lookup"><span data-stu-id="952b7-275">**Microsoft.PerceptionSimulation.PlaybackState.Paused**</span></span>

<span data-ttu-id="952b7-276">錄製目前已暫停。</span><span class="sxs-lookup"><span data-stu-id="952b7-276">The recording is currently paused.</span></span>

<span data-ttu-id="952b7-277">**PerceptionSimulation. PlaybackState. End**</span><span class="sxs-lookup"><span data-stu-id="952b7-277">**Microsoft.PerceptionSimulation.PlaybackState.End**</span></span>

<span data-ttu-id="952b7-278">錄製已到達結尾。</span><span class="sxs-lookup"><span data-stu-id="952b7-278">The recording has reached the end.</span></span>

### <a name="microsoftperceptionsimulationvector3"></a><span data-ttu-id="952b7-279">PerceptionSimulation. Vector3</span><span class="sxs-lookup"><span data-stu-id="952b7-279">Microsoft.PerceptionSimulation.Vector3</span></span>

<span data-ttu-id="952b7-280">描述三個元件向量，可能會描述3D 空間中的點或向量。</span><span class="sxs-lookup"><span data-stu-id="952b7-280">Describes a three components vector, which might describe a point or a vector in 3D space.</span></span>

```
public struct Vector3
{
    public float X;
    public float Y;
    public float Z;
    public Vector3(float x, float y, float z);
}
```

<span data-ttu-id="952b7-281">**PerceptionSimulation. Vector3. X**</span><span class="sxs-lookup"><span data-stu-id="952b7-281">**Microsoft.PerceptionSimulation.Vector3.X**</span></span>

<span data-ttu-id="952b7-282">此向量的 X 元件。</span><span class="sxs-lookup"><span data-stu-id="952b7-282">The X component of the vector.</span></span>

<span data-ttu-id="952b7-283">**PerceptionSimulation. Vector3. Y**</span><span class="sxs-lookup"><span data-stu-id="952b7-283">**Microsoft.PerceptionSimulation.Vector3.Y**</span></span>

<span data-ttu-id="952b7-284">此向量的 Y 元件。</span><span class="sxs-lookup"><span data-stu-id="952b7-284">The Y component of the vector.</span></span>

<span data-ttu-id="952b7-285">**PerceptionSimulation. Vector3. Z**</span><span class="sxs-lookup"><span data-stu-id="952b7-285">**Microsoft.PerceptionSimulation.Vector3.Z**</span></span>

<span data-ttu-id="952b7-286">此向量的 Z 元件。</span><span class="sxs-lookup"><span data-stu-id="952b7-286">The Z component of the vector.</span></span>

<span data-ttu-id="952b7-287">**PerceptionSimulation. Vector3. #ctor (system.string、system. single)**</span><span class="sxs-lookup"><span data-stu-id="952b7-287">**Microsoft.PerceptionSimulation.Vector3.#ctor(System.Single,System.Single,System.Single)**</span></span>

<span data-ttu-id="952b7-288">建立新的 Vector3。</span><span class="sxs-lookup"><span data-stu-id="952b7-288">Construct a new Vector3.</span></span>

<span data-ttu-id="952b7-289">參數</span><span class="sxs-lookup"><span data-stu-id="952b7-289">Parameters</span></span>
* <span data-ttu-id="952b7-290">x-向量的 x 元件。</span><span class="sxs-lookup"><span data-stu-id="952b7-290">x - The x component of the vector.</span></span>
* <span data-ttu-id="952b7-291">y-向量的 y 元件。</span><span class="sxs-lookup"><span data-stu-id="952b7-291">y - The y component of the vector.</span></span>
* <span data-ttu-id="952b7-292">z-向量的 z 元件。</span><span class="sxs-lookup"><span data-stu-id="952b7-292">z - The z component of the vector.</span></span>

### <a name="microsoftperceptionsimulationrotation3"></a><span data-ttu-id="952b7-293">PerceptionSimulation. Rotation3</span><span class="sxs-lookup"><span data-stu-id="952b7-293">Microsoft.PerceptionSimulation.Rotation3</span></span>

<span data-ttu-id="952b7-294">描述三個元件旋轉。</span><span class="sxs-lookup"><span data-stu-id="952b7-294">Describes a three components rotation.</span></span>

```
public struct Rotation3
{
    public float Pitch;
    public float Yaw;
    public float Roll;
    public Rotation3(float pitch, float yaw, float roll);
}
```

<span data-ttu-id="952b7-295">**PerceptionSimulation. Rotation3**</span><span class="sxs-lookup"><span data-stu-id="952b7-295">**Microsoft.PerceptionSimulation.Rotation3.Pitch**</span></span>

<span data-ttu-id="952b7-296">旋轉的音調元件，圍繞 X 軸。</span><span class="sxs-lookup"><span data-stu-id="952b7-296">The Pitch component of the Rotation, down around the X axis.</span></span>

<span data-ttu-id="952b7-297">**PerceptionSimulation. Rotation3. 偏擺**</span><span class="sxs-lookup"><span data-stu-id="952b7-297">**Microsoft.PerceptionSimulation.Rotation3.Yaw**</span></span>

<span data-ttu-id="952b7-298">旋轉的偏擺元件，位於 Y 軸的右方。</span><span class="sxs-lookup"><span data-stu-id="952b7-298">The Yaw component of the Rotation, right around the Y axis.</span></span>

<span data-ttu-id="952b7-299">**PerceptionSimulation. Rotation3。**</span><span class="sxs-lookup"><span data-stu-id="952b7-299">**Microsoft.PerceptionSimulation.Rotation3.Roll**</span></span>

<span data-ttu-id="952b7-300">旋轉的變換元件，位於 Z 軸的右方。</span><span class="sxs-lookup"><span data-stu-id="952b7-300">The Roll component of the Rotation, right around the Z axis.</span></span>

<span data-ttu-id="952b7-301">**PerceptionSimulation. Rotation3. #ctor (system.string、system. single)**</span><span class="sxs-lookup"><span data-stu-id="952b7-301">**Microsoft.PerceptionSimulation.Rotation3.#ctor(System.Single,System.Single,System.Single)**</span></span>

<span data-ttu-id="952b7-302">建立新的 Rotation3。</span><span class="sxs-lookup"><span data-stu-id="952b7-302">Construct a new Rotation3.</span></span>

<span data-ttu-id="952b7-303">參數</span><span class="sxs-lookup"><span data-stu-id="952b7-303">Parameters</span></span>
* <span data-ttu-id="952b7-304">音調-旋轉的音調部分。</span><span class="sxs-lookup"><span data-stu-id="952b7-304">pitch - The pitch component of the Rotation.</span></span>
* <span data-ttu-id="952b7-305">偏擺-旋轉的偏擺元件。</span><span class="sxs-lookup"><span data-stu-id="952b7-305">yaw - The yaw component of the Rotation.</span></span>
* <span data-ttu-id="952b7-306">向前復原：旋轉的變換元件。</span><span class="sxs-lookup"><span data-stu-id="952b7-306">roll - The roll component of the Rotation.</span></span>

### <a name="microsoftperceptionsimulationsimulatedhandjointconfiguration"></a><span data-ttu-id="952b7-307">PerceptionSimulation. SimulatedHandJointConfiguration</span><span class="sxs-lookup"><span data-stu-id="952b7-307">Microsoft.PerceptionSimulation.SimulatedHandJointConfiguration</span></span>

<span data-ttu-id="952b7-308">描述模擬手上的聯合設定。</span><span class="sxs-lookup"><span data-stu-id="952b7-308">Describes the configuration of a joint on a simulated hand.</span></span>

```
public struct SimulatedHandJointConfiguration
{
    public Vector3 Position;
    public Rotation3 Rotation;
    public SimulatedHandJointTrackingAccuracy TrackingAccuracy;
}
```

<span data-ttu-id="952b7-309">**PerceptionSimulation. SimulatedHandJointConfiguration. Position**</span><span class="sxs-lookup"><span data-stu-id="952b7-309">**Microsoft.PerceptionSimulation.SimulatedHandJointConfiguration.Position**</span></span>

<span data-ttu-id="952b7-310">聯合的位置。</span><span class="sxs-lookup"><span data-stu-id="952b7-310">The position of the joint.</span></span>

<span data-ttu-id="952b7-311">**PerceptionSimulation. SimulatedHandJointConfiguration. 旋轉**</span><span class="sxs-lookup"><span data-stu-id="952b7-311">**Microsoft.PerceptionSimulation.SimulatedHandJointConfiguration.Rotation**</span></span>

<span data-ttu-id="952b7-312">聯合的旋轉。</span><span class="sxs-lookup"><span data-stu-id="952b7-312">The rotation of the joint.</span></span>

<span data-ttu-id="952b7-313">**PerceptionSimulation. SimulatedHandJointConfiguration. TrackingAccuracy**</span><span class="sxs-lookup"><span data-stu-id="952b7-313">**Microsoft.PerceptionSimulation.SimulatedHandJointConfiguration.TrackingAccuracy**</span></span>

<span data-ttu-id="952b7-314">聯合的追蹤精確度。</span><span class="sxs-lookup"><span data-stu-id="952b7-314">The tracking accuracy of the joint.</span></span>


### <a name="microsoftperceptionsimulationfrustum"></a><span data-ttu-id="952b7-315">PerceptionSimulation</span><span class="sxs-lookup"><span data-stu-id="952b7-315">Microsoft.PerceptionSimulation.Frustum</span></span>

<span data-ttu-id="952b7-316">描述圖的視圖（像是相機一般使用的）。</span><span class="sxs-lookup"><span data-stu-id="952b7-316">Describes a view frustum, as typically used by a camera.</span></span>

```
public struct Frustum
{
    float Near;
    float Far;
    float FieldOfView;
    float AspectRatio;
}
```

<span data-ttu-id="952b7-317">**PerceptionSimulation，Near**</span><span class="sxs-lookup"><span data-stu-id="952b7-317">**Microsoft.PerceptionSimulation.Frustum.Near**</span></span>

<span data-ttu-id="952b7-318">包含在截量中的最小距離。</span><span class="sxs-lookup"><span data-stu-id="952b7-318">The minimum distance that is contained in the frustum.</span></span>

<span data-ttu-id="952b7-319">**PerceptionSimulation 到目前為止**</span><span class="sxs-lookup"><span data-stu-id="952b7-319">**Microsoft.PerceptionSimulation.Frustum.Far**</span></span>

<span data-ttu-id="952b7-320">包含在錐中的最大距離。</span><span class="sxs-lookup"><span data-stu-id="952b7-320">The maximum distance that is contained in the frustum.</span></span>

<span data-ttu-id="952b7-321">**PerceptionSimulation. FieldOfView**</span><span class="sxs-lookup"><span data-stu-id="952b7-321">**Microsoft.PerceptionSimulation.Frustum.FieldOfView**</span></span>

<span data-ttu-id="952b7-322">以弧度為單位之視圖的水準欄位， (小於 PI) 。</span><span class="sxs-lookup"><span data-stu-id="952b7-322">The horizontal field of view of the frustum, in radians (less than PI).</span></span>

<span data-ttu-id="952b7-323">**PerceptionSimulation. AspectRatio**</span><span class="sxs-lookup"><span data-stu-id="952b7-323">**Microsoft.PerceptionSimulation.Frustum.AspectRatio**</span></span>

<span data-ttu-id="952b7-324">水準欄位對視圖的水準對齊比例。</span><span class="sxs-lookup"><span data-stu-id="952b7-324">The ratio of horizontal field of view to vertical field of view.</span></span>

### <a name="microsoftperceptionsimulationsimulateddisplayconfiguration"></a><span data-ttu-id="952b7-325">PerceptionSimulation. SimulatedDisplayConfiguration</span><span class="sxs-lookup"><span data-stu-id="952b7-325">Microsoft.PerceptionSimulation.SimulatedDisplayConfiguration</span></span>

<span data-ttu-id="952b7-326">描述模擬耳機顯示器的設定。</span><span class="sxs-lookup"><span data-stu-id="952b7-326">Describes the configuration of the simulated headset's display.</span></span>

```
public struct SimulatedDisplayConfiguration
{
    public Vector3 LeftEyePosition;
    public Rotation3 LeftEyeRotation;
    public Vector3 RightEyePosition;
    public Rotation3 RightEyeRotation;
    public float Ipd;
    public bool ApplyEyeTransforms;
    public bool ApplyIpd;
}
```

<span data-ttu-id="952b7-327">**PerceptionSimulation. SimulatedDisplayConfiguration. LeftEyePosition**</span><span class="sxs-lookup"><span data-stu-id="952b7-327">**Microsoft.PerceptionSimulation.SimulatedDisplayConfiguration.LeftEyePosition**</span></span>

<span data-ttu-id="952b7-328">從頭部中央到左方的轉換，以利身歷聲轉譯的用途。</span><span class="sxs-lookup"><span data-stu-id="952b7-328">The transform from the center of the head to the left eye for purposes of stereo rendering.</span></span>

<span data-ttu-id="952b7-329">**PerceptionSimulation. SimulatedDisplayConfiguration. LeftEyeRotation**</span><span class="sxs-lookup"><span data-stu-id="952b7-329">**Microsoft.PerceptionSimulation.SimulatedDisplayConfiguration.LeftEyeRotation**</span></span>

<span data-ttu-id="952b7-330">針對身歷聲轉譯用途的左方旋轉。</span><span class="sxs-lookup"><span data-stu-id="952b7-330">The rotation of the left eye for purposes of stereo rendering.</span></span>

<span data-ttu-id="952b7-331">**PerceptionSimulation. SimulatedDisplayConfiguration. RightEyePosition**</span><span class="sxs-lookup"><span data-stu-id="952b7-331">**Microsoft.PerceptionSimulation.SimulatedDisplayConfiguration.RightEyePosition**</span></span>

<span data-ttu-id="952b7-332">從頭部中心到右邊的轉換，以提供身歷聲轉譯的目的。</span><span class="sxs-lookup"><span data-stu-id="952b7-332">The transform from the center of the head to the right eye for purposes of stereo rendering.</span></span>

<span data-ttu-id="952b7-333">**PerceptionSimulation. SimulatedDisplayConfiguration. RightEyeRotation**</span><span class="sxs-lookup"><span data-stu-id="952b7-333">**Microsoft.PerceptionSimulation.SimulatedDisplayConfiguration.RightEyeRotation**</span></span>

<span data-ttu-id="952b7-334">適用于身歷聲轉譯用途的正確眼睛旋轉。</span><span class="sxs-lookup"><span data-stu-id="952b7-334">The rotation of the right eye for purposes of stereo rendering.</span></span>

<span data-ttu-id="952b7-335">**PerceptionSimulation. SimulatedDisplayConfiguration Ipd**</span><span class="sxs-lookup"><span data-stu-id="952b7-335">**Microsoft.PerceptionSimulation.SimulatedDisplayConfiguration.Ipd**</span></span>

<span data-ttu-id="952b7-336">系統回報的 Ipd 值，用於身歷聲轉譯。</span><span class="sxs-lookup"><span data-stu-id="952b7-336">The Ipd value reported by the system for purposes of stereo rendering.</span></span>

<span data-ttu-id="952b7-337">**PerceptionSimulation. SimulatedDisplayConfiguration. ApplyEyeTransforms**</span><span class="sxs-lookup"><span data-stu-id="952b7-337">**Microsoft.PerceptionSimulation.SimulatedDisplayConfiguration.ApplyEyeTransforms**</span></span>

<span data-ttu-id="952b7-338">是否應將提供給左和右眼睛轉換的值視為有效，並套用至執行中的系統。</span><span class="sxs-lookup"><span data-stu-id="952b7-338">Whether the values provided for left and right eye transforms should be considered valid and applied to the running system.</span></span>

<span data-ttu-id="952b7-339">**PerceptionSimulation. SimulatedDisplayConfiguration. ApplyIpd**</span><span class="sxs-lookup"><span data-stu-id="952b7-339">**Microsoft.PerceptionSimulation.SimulatedDisplayConfiguration.ApplyIpd**</span></span>

<span data-ttu-id="952b7-340">是否應將為 Ipd 提供的值視為有效，並套用到執行中的系統。</span><span class="sxs-lookup"><span data-stu-id="952b7-340">Whether the value provided for Ipd should be considered valid and applied to the running system.</span></span>


### <a name="microsoftperceptionsimulationiperceptionsimulationmanager"></a><span data-ttu-id="952b7-341">PerceptionSimulation. IPerceptionSimulationManager</span><span class="sxs-lookup"><span data-stu-id="952b7-341">Microsoft.PerceptionSimulation.IPerceptionSimulationManager</span></span>

<span data-ttu-id="952b7-342">用來產生用來控制裝置的封包的根目錄。</span><span class="sxs-lookup"><span data-stu-id="952b7-342">Root for generating the packets used to control a device.</span></span>

```
public interface IPerceptionSimulationManager
{   
    ISimulatedDevice Device { get; }
    ISimulatedHuman Human { get; }
    void Reset();
}
```

<span data-ttu-id="952b7-343">**PerceptionSimulation. IPerceptionSimulationManager 裝置**</span><span class="sxs-lookup"><span data-stu-id="952b7-343">**Microsoft.PerceptionSimulation.IPerceptionSimulationManager.Device**</span></span>

<span data-ttu-id="952b7-344">取出模擬的裝置物件，以解讀模擬的人類和模擬的世界。</span><span class="sxs-lookup"><span data-stu-id="952b7-344">Retrieve the simulated device object that interprets the simulated human and the simulated world.</span></span>

<span data-ttu-id="952b7-345">**PerceptionSimulation. IPerceptionSimulationManager 人類**</span><span class="sxs-lookup"><span data-stu-id="952b7-345">**Microsoft.PerceptionSimulation.IPerceptionSimulationManager.Human**</span></span>

<span data-ttu-id="952b7-346">取出控制模擬人類的物件。</span><span class="sxs-lookup"><span data-stu-id="952b7-346">Retrieve the object that controls the simulated human.</span></span>

<span data-ttu-id="952b7-347">**PerceptionSimulation. IPerceptionSimulationManager 重設**</span><span class="sxs-lookup"><span data-stu-id="952b7-347">**Microsoft.PerceptionSimulation.IPerceptionSimulationManager.Reset**</span></span>

<span data-ttu-id="952b7-348">將模擬重設為其預設狀態。</span><span class="sxs-lookup"><span data-stu-id="952b7-348">Resets the simulation to its default state.</span></span>

### <a name="microsoftperceptionsimulationisimulateddevice"></a><span data-ttu-id="952b7-349">PerceptionSimulation. ISimulatedDevice</span><span class="sxs-lookup"><span data-stu-id="952b7-349">Microsoft.PerceptionSimulation.ISimulatedDevice</span></span>

<span data-ttu-id="952b7-350">描述裝置的介面，該裝置會解釋模擬的世界和模擬的人類</span><span class="sxs-lookup"><span data-stu-id="952b7-350">Interface describing the device, which interprets the simulated world and the simulated human</span></span>

```
public interface ISimulatedDevice
{
    ISimulatedHeadTracker HeadTracker { get; }
    ISimulatedHandTracker HandTracker { get; }
    void SetSimulatedDeviceType(SimulatedDeviceType type);
}
```

<span data-ttu-id="952b7-351">**PerceptionSimulation. ISimulatedDevice. HeadTracker**</span><span class="sxs-lookup"><span data-stu-id="952b7-351">**Microsoft.PerceptionSimulation.ISimulatedDevice.HeadTracker**</span></span>

<span data-ttu-id="952b7-352">從模擬裝置取出 Head 追蹤器。</span><span class="sxs-lookup"><span data-stu-id="952b7-352">Retrieve the Head Tracker from the Simulated Device.</span></span>

<span data-ttu-id="952b7-353">**PerceptionSimulation. ISimulatedDevice. HandTracker**</span><span class="sxs-lookup"><span data-stu-id="952b7-353">**Microsoft.PerceptionSimulation.ISimulatedDevice.HandTracker**</span></span>

<span data-ttu-id="952b7-354">從模擬裝置取出手形追蹤程式。</span><span class="sxs-lookup"><span data-stu-id="952b7-354">Retrieve the Hand Tracker from the Simulated Device.</span></span>

<span data-ttu-id="952b7-355">**PerceptionSimulation. ISimulatedDevice. SetSimulatedDeviceType (Microsoft. PerceptionSimulation. SimulatedDeviceType)**</span><span class="sxs-lookup"><span data-stu-id="952b7-355">**Microsoft.PerceptionSimulation.ISimulatedDevice.SetSimulatedDeviceType(Microsoft.PerceptionSimulation.SimulatedDeviceType)**</span></span>

<span data-ttu-id="952b7-356">設定模擬裝置的屬性，使其符合提供的裝置類型。</span><span class="sxs-lookup"><span data-stu-id="952b7-356">Set the properties of the simulated device to match the provided device type.</span></span>

<span data-ttu-id="952b7-357">參數</span><span class="sxs-lookup"><span data-stu-id="952b7-357">Parameters</span></span>
* <span data-ttu-id="952b7-358">類型-模擬裝置的新類型</span><span class="sxs-lookup"><span data-stu-id="952b7-358">type - The new type of Simulated Device</span></span>

### <a name="microsoftperceptionsimulationisimulateddevice2"></a><span data-ttu-id="952b7-359">PerceptionSimulation. ISimulatedDevice2</span><span class="sxs-lookup"><span data-stu-id="952b7-359">Microsoft.PerceptionSimulation.ISimulatedDevice2</span></span>

<span data-ttu-id="952b7-360">將 ISimulatedDevice 轉換成 ISimulatedDevice2，即可使用其他屬性</span><span class="sxs-lookup"><span data-stu-id="952b7-360">Additional properties are available by casting the ISimulatedDevice to ISimulatedDevice2</span></span>

```
public interface ISimulatedDevice2
{
    bool IsUserPresent { [return: MarshalAs(UnmanagedType.Bool)] get; [param: MarshalAs(UnmanagedType.Bool)] set; }
    SimulatedDisplayConfiguration DisplayConfiguration { get; set; }

};
```

<span data-ttu-id="952b7-361">**PerceptionSimulation. ISimulatedDevice2. IsUserPresent**</span><span class="sxs-lookup"><span data-stu-id="952b7-361">**Microsoft.PerceptionSimulation.ISimulatedDevice2.IsUserPresent**</span></span>

<span data-ttu-id="952b7-362">取得或設定模擬的人是否主動佩戴耳機。</span><span class="sxs-lookup"><span data-stu-id="952b7-362">Retrieve or set whether or not the simulated human is actively wearing the headset.</span></span>

<span data-ttu-id="952b7-363">**PerceptionSimulation. ISimulatedDevice2. DisplayConfiguration**</span><span class="sxs-lookup"><span data-stu-id="952b7-363">**Microsoft.PerceptionSimulation.ISimulatedDevice2.DisplayConfiguration**</span></span>

<span data-ttu-id="952b7-364">取得或設定模擬顯示的屬性。</span><span class="sxs-lookup"><span data-stu-id="952b7-364">Retrieve or set the properties of the simulated display.</span></span>

### <a name="microsoftperceptionsimulationisimulatedheadtracker"></a><span data-ttu-id="952b7-365">PerceptionSimulation. ISimulatedHeadTracker</span><span class="sxs-lookup"><span data-stu-id="952b7-365">Microsoft.PerceptionSimulation.ISimulatedHeadTracker</span></span>

<span data-ttu-id="952b7-366">描述模擬的裝置部分的介面，該部分會追蹤模擬人類的標頭。</span><span class="sxs-lookup"><span data-stu-id="952b7-366">Interface describing the portion of the simulated device that tracks the head of the simulated human.</span></span>

```
public interface ISimulatedHeadTracker
{
    HeadTrackerMode HeadTrackerMode { get; set; }
};
```

<span data-ttu-id="952b7-367">**PerceptionSimulation. ISimulatedHeadTracker. HeadTrackerMode**</span><span class="sxs-lookup"><span data-stu-id="952b7-367">**Microsoft.PerceptionSimulation.ISimulatedHeadTracker.HeadTrackerMode**</span></span>

<span data-ttu-id="952b7-368">抓取並設定目前的 head 追蹤器模式。</span><span class="sxs-lookup"><span data-stu-id="952b7-368">Retrieves and sets the current head tracker mode.</span></span>

### <a name="microsoftperceptionsimulationisimulatedhandtracker"></a><span data-ttu-id="952b7-369">PerceptionSimulation. ISimulatedHandTracker</span><span class="sxs-lookup"><span data-stu-id="952b7-369">Microsoft.PerceptionSimulation.ISimulatedHandTracker</span></span>

<span data-ttu-id="952b7-370">描述模擬的裝置部分的介面，以追蹤模擬人類的手</span><span class="sxs-lookup"><span data-stu-id="952b7-370">Interface describing the portion of the simulated device that tracks the hands of the simulated human</span></span>

```
public interface ISimulatedHandTracker
{
    Vector3 WorldPosition { get; }
    Vector3 Position { get; set; }
    float Pitch { get; set; }
    bool FrustumIgnored { [return: MarshalAs(UnmanagedType.Bool)] get; [param: MarshalAs(UnmanagedType.Bool)] set; }
    Frustum Frustum { get; set; }
}
```

<span data-ttu-id="952b7-371">**PerceptionSimulation. ISimulatedHandTracker. WorldPosition**</span><span class="sxs-lookup"><span data-stu-id="952b7-371">**Microsoft.PerceptionSimulation.ISimulatedHandTracker.WorldPosition**</span></span>

<span data-ttu-id="952b7-372">以計量表的關聯性抓取節點的位置。</span><span class="sxs-lookup"><span data-stu-id="952b7-372">Retrieve the position of the node with relation to the world, in meters.</span></span>

<span data-ttu-id="952b7-373">**PerceptionSimulation. ISimulatedHandTracker. Position**</span><span class="sxs-lookup"><span data-stu-id="952b7-373">**Microsoft.PerceptionSimulation.ISimulatedHandTracker.Position**</span></span>

<span data-ttu-id="952b7-374">取得和設定模擬的手追蹤器相對於 head 中央的位置。</span><span class="sxs-lookup"><span data-stu-id="952b7-374">Retrieve and set the position of the simulated hand tracker, relative to the center of the head.</span></span>

<span data-ttu-id="952b7-375">**PerceptionSimulation. ISimulatedHandTracker**</span><span class="sxs-lookup"><span data-stu-id="952b7-375">**Microsoft.PerceptionSimulation.ISimulatedHandTracker.Pitch**</span></span>

<span data-ttu-id="952b7-376">取出模擬的手追蹤器，並將其設定為向下。</span><span class="sxs-lookup"><span data-stu-id="952b7-376">Retrieve and set the downward pitch of the simulated hand tracker.</span></span>

<span data-ttu-id="952b7-377">**PerceptionSimulation. ISimulatedHandTracker. FrustumIgnored**</span><span class="sxs-lookup"><span data-stu-id="952b7-377">**Microsoft.PerceptionSimulation.ISimulatedHandTracker.FrustumIgnored**</span></span>

<span data-ttu-id="952b7-378">取得並設定是否忽略模擬的手追蹤器的截量。</span><span class="sxs-lookup"><span data-stu-id="952b7-378">Retrieve and set whether the frustum of the simulated hand tracker is ignored.</span></span> <span data-ttu-id="952b7-379">略過時，一律會顯示這兩個手。</span><span class="sxs-lookup"><span data-stu-id="952b7-379">When ignored, both hands are always visible.</span></span> <span data-ttu-id="952b7-380">未略過時 (預設) 手只會在其位於 [手形追蹤器] 的 [截量] 中時才會顯示。</span><span class="sxs-lookup"><span data-stu-id="952b7-380">When not ignored (the default) hands are only visible when they are within the frustum of the hand tracker.</span></span>

<span data-ttu-id="952b7-381">**PerceptionSimulation. ISimulatedHandTracker**</span><span class="sxs-lookup"><span data-stu-id="952b7-381">**Microsoft.PerceptionSimulation.ISimulatedHandTracker.Frustum**</span></span>

<span data-ttu-id="952b7-382">取出並設定用來判斷模擬的手追蹤器是否可以看到手的錐屬性。</span><span class="sxs-lookup"><span data-stu-id="952b7-382">Retrieve and set the frustum properties used to determine if hands are visible to the simulated hand tracker.</span></span>

### <a name="microsoftperceptionsimulationisimulatedhuman"></a><span data-ttu-id="952b7-383">PerceptionSimulation. ISimulatedHuman</span><span class="sxs-lookup"><span data-stu-id="952b7-383">Microsoft.PerceptionSimulation.ISimulatedHuman</span></span>

<span data-ttu-id="952b7-384">控制模擬人類的最上層介面。</span><span class="sxs-lookup"><span data-stu-id="952b7-384">Top-level interface for controlling the simulated human.</span></span>

```
public interface ISimulatedHuman 
{
    Vector3 WorldPosition { get; set; }
    float Direction { get; set; }
    float Height { get; set; }
    ISimulatedHand LeftHand { get; }
    ISimulatedHand RightHand { get; }
    ISimulatedHead Head { get; }s
    void Move(Vector3 translation);
    void Rotate(float radians);
}
```

<span data-ttu-id="952b7-385">**PerceptionSimulation. ISimulatedHuman. WorldPosition**</span><span class="sxs-lookup"><span data-stu-id="952b7-385">**Microsoft.PerceptionSimulation.ISimulatedHuman.WorldPosition**</span></span>

<span data-ttu-id="952b7-386">取得和設定與世界相關之節點的位置（以計量為單位）。</span><span class="sxs-lookup"><span data-stu-id="952b7-386">Retrieve and set the position of the node with relation to the world, in meters.</span></span> <span data-ttu-id="952b7-387">位置會對應到人類的點中央。</span><span class="sxs-lookup"><span data-stu-id="952b7-387">The position corresponds to a point at the center of the human's feet.</span></span>

<span data-ttu-id="952b7-388">**PerceptionSimulation. ISimulatedHuman. Direction**</span><span class="sxs-lookup"><span data-stu-id="952b7-388">**Microsoft.PerceptionSimulation.ISimulatedHuman.Direction**</span></span>

<span data-ttu-id="952b7-389">取得並設定模擬的人臉在世界中的方向。</span><span class="sxs-lookup"><span data-stu-id="952b7-389">Retrieve and set the direction the simulated human faces in the world.</span></span> <span data-ttu-id="952b7-390">0弧度會朝下負 Z 軸。</span><span class="sxs-lookup"><span data-stu-id="952b7-390">0 radians faces down the negative Z axis.</span></span> <span data-ttu-id="952b7-391">正弧度旋轉 Y 軸的順時針旋轉。</span><span class="sxs-lookup"><span data-stu-id="952b7-391">Positive radians rotate clockwise about the Y axis.</span></span>

<span data-ttu-id="952b7-392">**PerceptionSimulation. ISimulatedHuman. Height**</span><span class="sxs-lookup"><span data-stu-id="952b7-392">**Microsoft.PerceptionSimulation.ISimulatedHuman.Height**</span></span>

<span data-ttu-id="952b7-393">取得和設定模擬人類的高度（以計量為單位）。</span><span class="sxs-lookup"><span data-stu-id="952b7-393">Retrieve and set the height of the simulated human, in meters.</span></span>

<span data-ttu-id="952b7-394">**PerceptionSimulation. ISimulatedHuman. LeftHand**</span><span class="sxs-lookup"><span data-stu-id="952b7-394">**Microsoft.PerceptionSimulation.ISimulatedHuman.LeftHand**</span></span>

<span data-ttu-id="952b7-395">取出模擬人類的左邊。</span><span class="sxs-lookup"><span data-stu-id="952b7-395">Retrieve the left hand of the simulated human.</span></span>

<span data-ttu-id="952b7-396">**PerceptionSimulation. ISimulatedHuman. RightHand**</span><span class="sxs-lookup"><span data-stu-id="952b7-396">**Microsoft.PerceptionSimulation.ISimulatedHuman.RightHand**</span></span>

<span data-ttu-id="952b7-397">取出模擬人類的右手邊。</span><span class="sxs-lookup"><span data-stu-id="952b7-397">Retrieve the right hand of the simulated human.</span></span>

<span data-ttu-id="952b7-398">**PerceptionSimulation. ISimulatedHuman Head**</span><span class="sxs-lookup"><span data-stu-id="952b7-398">**Microsoft.PerceptionSimulation.ISimulatedHuman.Head**</span></span>

<span data-ttu-id="952b7-399">取出模擬人類的標頭。</span><span class="sxs-lookup"><span data-stu-id="952b7-399">Retrieve the head of the simulated human.</span></span>

<span data-ttu-id="952b7-400">**PerceptionSimulation. ISimulatedHuman. Move (PerceptionSimulation. Vector3)**</span><span class="sxs-lookup"><span data-stu-id="952b7-400">**Microsoft.PerceptionSimulation.ISimulatedHuman.Move(Microsoft.PerceptionSimulation.Vector3)**</span></span>

<span data-ttu-id="952b7-401">將模擬的人移至其目前位置（以計量為單位）。</span><span class="sxs-lookup"><span data-stu-id="952b7-401">Move the simulated human relative to its current position, in meters.</span></span>

<span data-ttu-id="952b7-402">參數</span><span class="sxs-lookup"><span data-stu-id="952b7-402">Parameters</span></span>
* <span data-ttu-id="952b7-403">轉譯-要移動的轉譯（相對於目前的位置）。</span><span class="sxs-lookup"><span data-stu-id="952b7-403">translation - The translation to move, relative to current position.</span></span>

<span data-ttu-id="952b7-404">**PerceptionSimulation. ISimulatedHuman. (System.object 旋轉)**</span><span class="sxs-lookup"><span data-stu-id="952b7-404">**Microsoft.PerceptionSimulation.ISimulatedHuman.Rotate(System.Single)**</span></span>

<span data-ttu-id="952b7-405">以順時針方向對 Y 軸旋轉模擬的人，相對於其目前方向</span><span class="sxs-lookup"><span data-stu-id="952b7-405">Rotate the simulated human relative to its current direction, clockwise about the Y axis</span></span>

<span data-ttu-id="952b7-406">參數</span><span class="sxs-lookup"><span data-stu-id="952b7-406">Parameters</span></span>
* <span data-ttu-id="952b7-407">弧度-繞著 Y 軸旋轉的量。</span><span class="sxs-lookup"><span data-stu-id="952b7-407">radians - The amount to rotate around the Y axis.</span></span>

### <a name="microsoftperceptionsimulationisimulatedhuman2"></a><span data-ttu-id="952b7-408">PerceptionSimulation. ISimulatedHuman2</span><span class="sxs-lookup"><span data-stu-id="952b7-408">Microsoft.PerceptionSimulation.ISimulatedHuman2</span></span>

<span data-ttu-id="952b7-409">將 ISimulatedHuman 轉換成 ISimulatedHuman2，即可使用其他屬性</span><span class="sxs-lookup"><span data-stu-id="952b7-409">Additional properties are available by casting the ISimulatedHuman to ISimulatedHuman2</span></span>

```
public interface ISimulatedHuman2
{
    /* New members in addition to those available on ISimulatedHuman */
    ISimulatedSixDofController LeftController { get; }
    ISimulatedSixDofController RightController { get; }
}
```

<span data-ttu-id="952b7-410">**PerceptionSimulation. ISimulatedHuman2. LeftController**</span><span class="sxs-lookup"><span data-stu-id="952b7-410">**Microsoft.PerceptionSimulation.ISimulatedHuman2.LeftController**</span></span>

<span data-ttu-id="952b7-411">取出左方 6 DOF 控制器。</span><span class="sxs-lookup"><span data-stu-id="952b7-411">Retrieve the left 6-DOF controller.</span></span>

<span data-ttu-id="952b7-412">**PerceptionSimulation. ISimulatedHuman2. RightController**</span><span class="sxs-lookup"><span data-stu-id="952b7-412">**Microsoft.PerceptionSimulation.ISimulatedHuman2.RightController**</span></span>

<span data-ttu-id="952b7-413">取出右邊的 6 DOF 控制器。</span><span class="sxs-lookup"><span data-stu-id="952b7-413">Retrieve the right 6-DOF controller.</span></span>


### <a name="microsoftperceptionsimulationisimulatedhand"></a><span data-ttu-id="952b7-414">PerceptionSimulation. ISimulatedHand</span><span class="sxs-lookup"><span data-stu-id="952b7-414">Microsoft.PerceptionSimulation.ISimulatedHand</span></span>

<span data-ttu-id="952b7-415">描述模擬人手的介面</span><span class="sxs-lookup"><span data-stu-id="952b7-415">Interface describing a hand of the simulated human</span></span>

```
public interface ISimulatedHand
{
    Vector3 WorldPosition { get; }
    Vector3 Position { get; set; }
    bool Activated { [return: MarshalAs(UnmanagedType.Bool)] get; [param: MarshalAs(UnmanagedType.Bool)] set; }
    bool Visible { [return: MarshalAs(UnmanagedType.Bool)] get; }
    void EnsureVisible();
    void Move(Vector3 translation);
    void PerformGesture(SimulatedGesture gesture);
}
```

<span data-ttu-id="952b7-416">**PerceptionSimulation. ISimulatedHand. WorldPosition**</span><span class="sxs-lookup"><span data-stu-id="952b7-416">**Microsoft.PerceptionSimulation.ISimulatedHand.WorldPosition**</span></span>

<span data-ttu-id="952b7-417">以計量表的關聯性抓取節點的位置。</span><span class="sxs-lookup"><span data-stu-id="952b7-417">Retrieve the position of the node with relation to the world, in meters.</span></span>

<span data-ttu-id="952b7-418">**PerceptionSimulation. ISimulatedHand. Position**</span><span class="sxs-lookup"><span data-stu-id="952b7-418">**Microsoft.PerceptionSimulation.ISimulatedHand.Position**</span></span>

<span data-ttu-id="952b7-419">取得和設定模擬手相對於人類的位置，以量表為單位。</span><span class="sxs-lookup"><span data-stu-id="952b7-419">Retrieve and set the position of the simulated hand relative to the human, in meters.</span></span>

<span data-ttu-id="952b7-420">**PerceptionSimulation. ISimulatedHand。已啟用**</span><span class="sxs-lookup"><span data-stu-id="952b7-420">**Microsoft.PerceptionSimulation.ISimulatedHand.Activated**</span></span>

<span data-ttu-id="952b7-421">取得並設定是否目前已啟用手形。</span><span class="sxs-lookup"><span data-stu-id="952b7-421">Retrieve and set whether the hand is currently activated.</span></span>

<span data-ttu-id="952b7-422">**PerceptionSimulation. ISimulatedHand 可見**</span><span class="sxs-lookup"><span data-stu-id="952b7-422">**Microsoft.PerceptionSimulation.ISimulatedHand.Visible**</span></span>

<span data-ttu-id="952b7-423">取出 >simulateddevice 的目前是否看得到手 (也就是，是否在 HandTracker) 偵測到位置。</span><span class="sxs-lookup"><span data-stu-id="952b7-423">Retrieve whether the hand is currently visible to the SimulatedDevice (that is, whether it's in a position to be detected by the HandTracker).</span></span>

<span data-ttu-id="952b7-424">**PerceptionSimulation. ISimulatedHand. EnsureVisible**</span><span class="sxs-lookup"><span data-stu-id="952b7-424">**Microsoft.PerceptionSimulation.ISimulatedHand.EnsureVisible**</span></span>

<span data-ttu-id="952b7-425">將手上移，讓 >simulateddevice 可以看到它。</span><span class="sxs-lookup"><span data-stu-id="952b7-425">Move the hand such that it is visible to the SimulatedDevice.</span></span>

<span data-ttu-id="952b7-426">**PerceptionSimulation. ISimulatedHand. Move (PerceptionSimulation. Vector3)**</span><span class="sxs-lookup"><span data-stu-id="952b7-426">**Microsoft.PerceptionSimulation.ISimulatedHand.Move(Microsoft.PerceptionSimulation.Vector3)**</span></span>

<span data-ttu-id="952b7-427">移動模擬手相對於其目前位置的位置（以計量為單位）。</span><span class="sxs-lookup"><span data-stu-id="952b7-427">Move the position of the simulated hand relative to its current position, in meters.</span></span>

<span data-ttu-id="952b7-428">參數</span><span class="sxs-lookup"><span data-stu-id="952b7-428">Parameters</span></span>
* <span data-ttu-id="952b7-429">轉譯-要轉譯模擬手的數量。</span><span class="sxs-lookup"><span data-stu-id="952b7-429">translation - The amount to translate the simulated hand.</span></span>

<span data-ttu-id="952b7-430">**PerceptionSimulation. ISimulatedHand. PerformGesture (Microsoft. PerceptionSimulation. SimulatedGesture)**</span><span class="sxs-lookup"><span data-stu-id="952b7-430">**Microsoft.PerceptionSimulation.ISimulatedHand.PerformGesture(Microsoft.PerceptionSimulation.SimulatedGesture)**</span></span>

<span data-ttu-id="952b7-431">使用模擬的手勢來執行筆勢。</span><span class="sxs-lookup"><span data-stu-id="952b7-431">Perform a gesture using the simulated hand.</span></span>  <span data-ttu-id="952b7-432">只有在已啟用手形的情況下，系統才會偵測到此情況。</span><span class="sxs-lookup"><span data-stu-id="952b7-432">It will only be detected by the system if the hand is enabled.</span></span>

<span data-ttu-id="952b7-433">參數</span><span class="sxs-lookup"><span data-stu-id="952b7-433">Parameters</span></span>
* <span data-ttu-id="952b7-434">手勢-要執行的手勢。</span><span class="sxs-lookup"><span data-stu-id="952b7-434">gesture - The gesture to perform.</span></span>

### <a name="microsoftperceptionsimulationisimulatedhand2"></a><span data-ttu-id="952b7-435">PerceptionSimulation. ISimulatedHand2</span><span class="sxs-lookup"><span data-stu-id="952b7-435">Microsoft.PerceptionSimulation.ISimulatedHand2</span></span>

<span data-ttu-id="952b7-436">將 ISimulatedHand 轉換成 ISimulatedHand2，即可使用其他屬性。</span><span class="sxs-lookup"><span data-stu-id="952b7-436">Additional properties are available by casting an ISimulatedHand to ISimulatedHand2.</span></span>
```
public interface ISimulatedHand2
{
    /* New members in addition to those available on ISimulatedHand */
    Rotation3 Orientation { get; set; }
}
```

<span data-ttu-id="952b7-437">**PerceptionSimulation. ISimulatedHand2 的方向**</span><span class="sxs-lookup"><span data-stu-id="952b7-437">**Microsoft.PerceptionSimulation.ISimulatedHand2.Orientation**</span></span>

<span data-ttu-id="952b7-438">取出或設定模擬手的旋轉。</span><span class="sxs-lookup"><span data-stu-id="952b7-438">Retrieve or set the rotation of the simulated hand.</span></span>  <span data-ttu-id="952b7-439">當您沿著軸旋轉時，正弧度會順時針旋轉。</span><span class="sxs-lookup"><span data-stu-id="952b7-439">Positive radians rotate clockwise when looking along the axis.</span></span>

### <a name="microsoftperceptionsimulationisimulatedhand3"></a><span data-ttu-id="952b7-440">PerceptionSimulation. ISimulatedHand3</span><span class="sxs-lookup"><span data-stu-id="952b7-440">Microsoft.PerceptionSimulation.ISimulatedHand3</span></span>

<span data-ttu-id="952b7-441">將 ISimulatedHand 轉換成 ISimulatedHand3，即可使用其他屬性</span><span class="sxs-lookup"><span data-stu-id="952b7-441">Additional properties are available by casting an ISimulatedHand to ISimulatedHand3</span></span>
```
public interface ISimulatedHand3
{
    /* New members in addition to those available on ISimulatedHand and ISimulatedHand2 */
    GetJointConfiguration(SimulatedHandJoint joint, out SimulatedHandJointConfiguration jointConfiguration);
    SetJointConfiguration(SimulatedHandJoint joint, SimulatedHandJointConfiguration jointConfiguration);
    SetHandPose(SimulatedHandPose pose, bool animate);
}
```

<span data-ttu-id="952b7-442">**PerceptionSimulation. ISimulatedHand3. GetJointConfiguration**</span><span class="sxs-lookup"><span data-stu-id="952b7-442">**Microsoft.PerceptionSimulation.ISimulatedHand3.GetJointConfiguration**</span></span>

<span data-ttu-id="952b7-443">取得指定聯合的聯合設定。</span><span class="sxs-lookup"><span data-stu-id="952b7-443">Get the joint configuration for the specified joint.</span></span>

<span data-ttu-id="952b7-444">**PerceptionSimulation. ISimulatedHand3. SetJointConfiguration**</span><span class="sxs-lookup"><span data-stu-id="952b7-444">**Microsoft.PerceptionSimulation.ISimulatedHand3.SetJointConfiguration**</span></span>

<span data-ttu-id="952b7-445">設定指定之聯合的聯合設定。</span><span class="sxs-lookup"><span data-stu-id="952b7-445">Set the joint configuration for the specified joint.</span></span>

<span data-ttu-id="952b7-446">**PerceptionSimulation. ISimulatedHand3. SetHandPose**</span><span class="sxs-lookup"><span data-stu-id="952b7-446">**Microsoft.PerceptionSimulation.ISimulatedHand3.SetHandPose**</span></span>

<span data-ttu-id="952b7-447">使用選擇性旗標將手設為已知的姿勢，以建立動畫。</span><span class="sxs-lookup"><span data-stu-id="952b7-447">Set the hand to a known pose with an optional flag to animate.</span></span>  <span data-ttu-id="952b7-448">注意：動畫並不會立即反映其最終的聯合設定。</span><span class="sxs-lookup"><span data-stu-id="952b7-448">Note: animating won't result in joints immediately reflecting their final joint configurations.</span></span>


### <a name="microsoftperceptionsimulationisimulatedhead"></a><span data-ttu-id="952b7-449">PerceptionSimulation. ISimulatedHead</span><span class="sxs-lookup"><span data-stu-id="952b7-449">Microsoft.PerceptionSimulation.ISimulatedHead</span></span>

<span data-ttu-id="952b7-450">描述模擬人類標頭的介面。</span><span class="sxs-lookup"><span data-stu-id="952b7-450">Interface describing the head of the simulated human.</span></span>

```
public interface ISimulatedHead
{
    Vector3 WorldPosition { get; }
    Rotation3 Rotation { get; set; }
    float Diameter { get; set; }
    void Rotate(Rotation3 rotation);
}
```

<span data-ttu-id="952b7-451">**PerceptionSimulation. ISimulatedHead. WorldPosition**</span><span class="sxs-lookup"><span data-stu-id="952b7-451">**Microsoft.PerceptionSimulation.ISimulatedHead.WorldPosition**</span></span>

<span data-ttu-id="952b7-452">以計量表的關聯性抓取節點的位置。</span><span class="sxs-lookup"><span data-stu-id="952b7-452">Retrieve the position of the node with relation to the world, in meters.</span></span>

<span data-ttu-id="952b7-453">**PerceptionSimulation. ISimulatedHead. 旋轉**</span><span class="sxs-lookup"><span data-stu-id="952b7-453">**Microsoft.PerceptionSimulation.ISimulatedHead.Rotation**</span></span>

<span data-ttu-id="952b7-454">取出模擬標頭的旋轉。</span><span class="sxs-lookup"><span data-stu-id="952b7-454">Retrieve the rotation of the simulated head.</span></span> <span data-ttu-id="952b7-455">當您沿著軸旋轉時，正弧度會順時針旋轉。</span><span class="sxs-lookup"><span data-stu-id="952b7-455">Positive radians rotate clockwise when looking along the axis.</span></span>

<span data-ttu-id="952b7-456">**PerceptionSimulation. ISimulatedHead. 直徑**</span><span class="sxs-lookup"><span data-stu-id="952b7-456">**Microsoft.PerceptionSimulation.ISimulatedHead.Diameter**</span></span>

<span data-ttu-id="952b7-457">取出模擬的標頭直徑。</span><span class="sxs-lookup"><span data-stu-id="952b7-457">Retrieve the simulated head's diameter.</span></span> <span data-ttu-id="952b7-458">此值可用來判斷旋轉) 的前端 (點。</span><span class="sxs-lookup"><span data-stu-id="952b7-458">This value is used to determine the head's center (point of rotation).</span></span>

<span data-ttu-id="952b7-459">**PerceptionSimulation. ISimulatedHead (PerceptionSimulation. Rotation3)**</span><span class="sxs-lookup"><span data-stu-id="952b7-459">**Microsoft.PerceptionSimulation.ISimulatedHead.Rotate(Microsoft.PerceptionSimulation.Rotation3)**</span></span>

<span data-ttu-id="952b7-460">將模擬的標頭相對於其目前的旋轉旋轉。</span><span class="sxs-lookup"><span data-stu-id="952b7-460">Rotate the simulated head relative to its current rotation.</span></span> <span data-ttu-id="952b7-461">當您沿著軸旋轉時，正弧度會順時針旋轉。</span><span class="sxs-lookup"><span data-stu-id="952b7-461">Positive radians rotate clockwise when looking along the axis.</span></span>

<span data-ttu-id="952b7-462">參數</span><span class="sxs-lookup"><span data-stu-id="952b7-462">Parameters</span></span>
* <span data-ttu-id="952b7-463">旋轉-要旋轉的量。</span><span class="sxs-lookup"><span data-stu-id="952b7-463">rotation - The amount to rotate.</span></span>

### <a name="microsoftperceptionsimulationisimulatedhead2"></a><span data-ttu-id="952b7-464">PerceptionSimulation. ISimulatedHead2</span><span class="sxs-lookup"><span data-stu-id="952b7-464">Microsoft.PerceptionSimulation.ISimulatedHead2</span></span>

<span data-ttu-id="952b7-465">將 ISimulatedHead 轉換成 ISimulatedHead2，即可使用其他屬性</span><span class="sxs-lookup"><span data-stu-id="952b7-465">Additional properties are available by casting an ISimulatedHead to ISimulatedHead2</span></span>

```
public interface ISimulatedHead2
{
    /* New members in addition to those available on ISimulatedHead */
    ISimulatedEyes Eyes { get; }
}
```

<span data-ttu-id="952b7-466">**PerceptionSimulation. ISimulatedHead2 眼睛**</span><span class="sxs-lookup"><span data-stu-id="952b7-466">**Microsoft.PerceptionSimulation.ISimulatedHead2.Eyes**</span></span>

<span data-ttu-id="952b7-467">取出模擬人類的眼睛。</span><span class="sxs-lookup"><span data-stu-id="952b7-467">Retrieve the eyes of the simulated human.</span></span>

### <a name="microsoftperceptionsimulationisimulatedsixdofcontroller"></a><span data-ttu-id="952b7-468">PerceptionSimulation. ISimulatedSixDofController</span><span class="sxs-lookup"><span data-stu-id="952b7-468">Microsoft.PerceptionSimulation.ISimulatedSixDofController</span></span>

<span data-ttu-id="952b7-469">描述與模擬人類相關聯之 6 DOF 控制器的介面。</span><span class="sxs-lookup"><span data-stu-id="952b7-469">Interface describing a 6-DOF controller associated with the simulated human.</span></span>

```
public interface ISimulatedSixDofController
{
    Vector3 WorldPosition { get; }
    SimulatedSixDofControllerStatus Status { get; set; }
    Vector3 Position { get; }
    Rotation3 Orientation { get; set; }
    void Move(Vector3 translation);
    void PressButton(SimulatedSixDofControllerButton button);
    void ReleaseButton(SimulatedSixDofControllerButton button);
    void GetTouchpadPosition(out float x, out float y);
    void SetTouchpadPosition(float x, float y);
}
```

<span data-ttu-id="952b7-470">**PerceptionSimulation. ISimulatedSixDofController. WorldPosition**</span><span class="sxs-lookup"><span data-stu-id="952b7-470">**Microsoft.PerceptionSimulation.ISimulatedSixDofController.WorldPosition**</span></span>

<span data-ttu-id="952b7-471">以計量表的關聯性抓取節點的位置。</span><span class="sxs-lookup"><span data-stu-id="952b7-471">Retrieve the position of the node with relation to the world, in meters.</span></span>

<span data-ttu-id="952b7-472">**PerceptionSimulation. ISimulatedSixDofController。狀態**</span><span class="sxs-lookup"><span data-stu-id="952b7-472">**Microsoft.PerceptionSimulation.ISimulatedSixDofController.Status**</span></span>

<span data-ttu-id="952b7-473">取得或設定控制器的目前狀態。</span><span class="sxs-lookup"><span data-stu-id="952b7-473">Retrieve or set the current state of the controller.</span></span>  <span data-ttu-id="952b7-474">控制器狀態必須設定為 [關閉] 以外的值，才能讓任何移動、旋轉或按下按鈕的動作都成功。</span><span class="sxs-lookup"><span data-stu-id="952b7-474">The controller status must be set to a value other than Off before any calls to move, rotate, or press buttons will succeed.</span></span>

<span data-ttu-id="952b7-475">**PerceptionSimulation. ISimulatedSixDofController. Position**</span><span class="sxs-lookup"><span data-stu-id="952b7-475">**Microsoft.PerceptionSimulation.ISimulatedSixDofController.Position**</span></span>

<span data-ttu-id="952b7-476">取得或設定模擬控制器相對於人類的位置（以計量為單位）。</span><span class="sxs-lookup"><span data-stu-id="952b7-476">Retrieve or set the position of the simulated controller relative to the human, in meters.</span></span>

<span data-ttu-id="952b7-477">**PerceptionSimulation. ISimulatedSixDofController 的方向**</span><span class="sxs-lookup"><span data-stu-id="952b7-477">**Microsoft.PerceptionSimulation.ISimulatedSixDofController.Orientation**</span></span>

<span data-ttu-id="952b7-478">取得或設定模擬控制器的方向。</span><span class="sxs-lookup"><span data-stu-id="952b7-478">Retrieve or set the orientation of the simulated controller.</span></span>

<span data-ttu-id="952b7-479">**PerceptionSimulation. ISimulatedSixDofController. Move (PerceptionSimulation. Vector3)**</span><span class="sxs-lookup"><span data-stu-id="952b7-479">**Microsoft.PerceptionSimulation.ISimulatedSixDofController.Move(Microsoft.PerceptionSimulation.Vector3)**</span></span>

<span data-ttu-id="952b7-480">移動模擬控制器相對於其目前位置的位置（以計量為單位）。</span><span class="sxs-lookup"><span data-stu-id="952b7-480">Move the position of the simulated controller relative to its current position, in meters.</span></span>

<span data-ttu-id="952b7-481">參數</span><span class="sxs-lookup"><span data-stu-id="952b7-481">Parameters</span></span>
* <span data-ttu-id="952b7-482">轉譯-要轉譯模擬控制器的數量。</span><span class="sxs-lookup"><span data-stu-id="952b7-482">translation - The amount to translate the simulated controller.</span></span>

<span data-ttu-id="952b7-483">**PerceptionSimulation. ISimulatedSixDofController. PressButton (SimulatedSixDofControllerButton)**</span><span class="sxs-lookup"><span data-stu-id="952b7-483">**Microsoft.PerceptionSimulation.ISimulatedSixDofController.PressButton(SimulatedSixDofControllerButton)**</span></span>

<span data-ttu-id="952b7-484">按下模擬控制器上的按鈕。</span><span class="sxs-lookup"><span data-stu-id="952b7-484">Press a button on the simulated controller.</span></span>  <span data-ttu-id="952b7-485">只有在已啟用控制器的情況下，系統才會偵測到此情況。</span><span class="sxs-lookup"><span data-stu-id="952b7-485">It will only be detected by the system if the controller is enabled.</span></span>

<span data-ttu-id="952b7-486">參數</span><span class="sxs-lookup"><span data-stu-id="952b7-486">Parameters</span></span>
* <span data-ttu-id="952b7-487">按鈕-要按的按鈕。</span><span class="sxs-lookup"><span data-stu-id="952b7-487">button - The button to press.</span></span>

<span data-ttu-id="952b7-488">**PerceptionSimulation. ISimulatedSixDofController. ReleaseButton (SimulatedSixDofControllerButton)**</span><span class="sxs-lookup"><span data-stu-id="952b7-488">**Microsoft.PerceptionSimulation.ISimulatedSixDofController.ReleaseButton(SimulatedSixDofControllerButton)**</span></span>

<span data-ttu-id="952b7-489">釋放模擬控制器上的按鈕。</span><span class="sxs-lookup"><span data-stu-id="952b7-489">Release a button on the simulated controller.</span></span>  <span data-ttu-id="952b7-490">只有在已啟用控制器的情況下，系統才會偵測到此情況。</span><span class="sxs-lookup"><span data-stu-id="952b7-490">It will only be detected by the system if the controller is enabled.</span></span>

<span data-ttu-id="952b7-491">參數</span><span class="sxs-lookup"><span data-stu-id="952b7-491">Parameters</span></span>
* <span data-ttu-id="952b7-492">按鈕-要釋放的按鈕。</span><span class="sxs-lookup"><span data-stu-id="952b7-492">button - The button to release.</span></span>

<span data-ttu-id="952b7-493">**PerceptionSimulation. ISimulatedSixDofController. GetTouchpadPosition (out float，out float)**</span><span class="sxs-lookup"><span data-stu-id="952b7-493">**Microsoft.PerceptionSimulation.ISimulatedSixDofController.GetTouchpadPosition(out float, out float)**</span></span>

<span data-ttu-id="952b7-494">取得模擬控制器的觸控板上模擬手指的位置。</span><span class="sxs-lookup"><span data-stu-id="952b7-494">Get the position of a simulated finger on the simulated controller's touchpad.</span></span>

<span data-ttu-id="952b7-495">參數</span><span class="sxs-lookup"><span data-stu-id="952b7-495">Parameters</span></span>
* <span data-ttu-id="952b7-496">x-指手指的水準位置。</span><span class="sxs-lookup"><span data-stu-id="952b7-496">x - The horizontal position of the finger.</span></span>
* <span data-ttu-id="952b7-497">y-指手指的垂直位置。</span><span class="sxs-lookup"><span data-stu-id="952b7-497">y - The vertical position of the finger.</span></span>

<span data-ttu-id="952b7-498">**PerceptionSimulation. ISimulatedSixDofController. SetTouchpadPosition (float，float)**</span><span class="sxs-lookup"><span data-stu-id="952b7-498">**Microsoft.PerceptionSimulation.ISimulatedSixDofController.SetTouchpadPosition(float, float)**</span></span>

<span data-ttu-id="952b7-499">在模擬控制器的觸控板上設定模擬手指的位置。</span><span class="sxs-lookup"><span data-stu-id="952b7-499">Set the position of a simulated finger on the simulated controller's touchpad.</span></span>

<span data-ttu-id="952b7-500">參數</span><span class="sxs-lookup"><span data-stu-id="952b7-500">Parameters</span></span>
* <span data-ttu-id="952b7-501">x-指手指的水準位置。</span><span class="sxs-lookup"><span data-stu-id="952b7-501">x - The horizontal position of the finger.</span></span>
* <span data-ttu-id="952b7-502">y-指手指的垂直位置。</span><span class="sxs-lookup"><span data-stu-id="952b7-502">y - The vertical position of the finger.</span></span>

### <a name="microsoftperceptionsimulationisimulatedsixdofcontroller2"></a><span data-ttu-id="952b7-503">PerceptionSimulation. ISimulatedSixDofController2</span><span class="sxs-lookup"><span data-stu-id="952b7-503">Microsoft.PerceptionSimulation.ISimulatedSixDofController2</span></span>

<span data-ttu-id="952b7-504">將 ISimulatedSixDofController 轉換成 ISimulatedSixDofController2，即可使用其他屬性和方法</span><span class="sxs-lookup"><span data-stu-id="952b7-504">Additional properties and methods are available by casting an ISimulatedSixDofController to ISimulatedSixDofController2</span></span>

```
public interface ISimulatedSixDofController2
{
    /* New members in addition to those available on ISimulatedSixDofController */
    void GetThumbstickPosition(out float x, out float y);
    void SetThumbstickPosition(float x, float y);
    float BatteryLevel { get; set; }
}
```

<span data-ttu-id="952b7-505">**PerceptionSimulation. ISimulatedSixDofController2. GetThumbstickPosition (out float，out float)**</span><span class="sxs-lookup"><span data-stu-id="952b7-505">**Microsoft.PerceptionSimulation.ISimulatedSixDofController2.GetThumbstickPosition(out float, out float)**</span></span>

<span data-ttu-id="952b7-506">取得模擬控制器上模擬之操縱杆的位置。</span><span class="sxs-lookup"><span data-stu-id="952b7-506">Get the position of the simulated thumbstick on the simulated controller.</span></span>

<span data-ttu-id="952b7-507">參數</span><span class="sxs-lookup"><span data-stu-id="952b7-507">Parameters</span></span>
* <span data-ttu-id="952b7-508">x-操縱杆的水準位置。</span><span class="sxs-lookup"><span data-stu-id="952b7-508">x - The horizontal position of the thumbstick.</span></span>
* <span data-ttu-id="952b7-509">y-操縱杆的垂直位置。</span><span class="sxs-lookup"><span data-stu-id="952b7-509">y - The vertical position of the thumbstick.</span></span>

<span data-ttu-id="952b7-510">**PerceptionSimulation. ISimulatedSixDofController2. SetThumbstickPosition (float，float)**</span><span class="sxs-lookup"><span data-stu-id="952b7-510">**Microsoft.PerceptionSimulation.ISimulatedSixDofController2.SetThumbstickPosition(float, float)**</span></span>

<span data-ttu-id="952b7-511">設定模擬控制器上模擬之操縱杆的位置。</span><span class="sxs-lookup"><span data-stu-id="952b7-511">Set the position of the simulated thumbstick on the simulated controller.</span></span>

<span data-ttu-id="952b7-512">參數</span><span class="sxs-lookup"><span data-stu-id="952b7-512">Parameters</span></span>
* <span data-ttu-id="952b7-513">x-操縱杆的水準位置。</span><span class="sxs-lookup"><span data-stu-id="952b7-513">x - The horizontal position of the thumbstick.</span></span>
* <span data-ttu-id="952b7-514">y-操縱杆的垂直位置。</span><span class="sxs-lookup"><span data-stu-id="952b7-514">y - The vertical position of the thumbstick.</span></span>

<span data-ttu-id="952b7-515">**Microsoft.PerceptionSimulation.ISimulatedSixDofController2.BatteryLevel**</span><span class="sxs-lookup"><span data-stu-id="952b7-515">**Microsoft.PerceptionSimulation.ISimulatedSixDofController2.BatteryLevel**</span></span>

<span data-ttu-id="952b7-516">取得或設定模擬控制器的電池音量。</span><span class="sxs-lookup"><span data-stu-id="952b7-516">Retrieve or set the battery level of the simulated controller.</span></span>  <span data-ttu-id="952b7-517">值必須大於0.0 且小於或等於100.0。</span><span class="sxs-lookup"><span data-stu-id="952b7-517">The value must be greater than 0.0 and less than or equal to 100.0.</span></span>


### <a name="microsoftperceptionsimulationisimulatedeyes"></a><span data-ttu-id="952b7-518">PerceptionSimulation. ISimulatedEyes</span><span class="sxs-lookup"><span data-stu-id="952b7-518">Microsoft.PerceptionSimulation.ISimulatedEyes</span></span>

<span data-ttu-id="952b7-519">描述模擬人類眼睛的介面。</span><span class="sxs-lookup"><span data-stu-id="952b7-519">Interface describing the eyes of the simulated human.</span></span>

```
public interface ISimulatedEyes
{
    Rotation3 Rotation { get; set; }
    void Rotate(Rotation3 rotation);
    SimulatedEyesCalibrationState CalibrationState { get; set; }
    Vector3 WorldPosition { get; }
}
```

<span data-ttu-id="952b7-520">**PerceptionSimulation. ISimulatedEyes. 旋轉**</span><span class="sxs-lookup"><span data-stu-id="952b7-520">**Microsoft.PerceptionSimulation.ISimulatedEyes.Rotation**</span></span>

<span data-ttu-id="952b7-521">取出模擬眼睛的旋轉。</span><span class="sxs-lookup"><span data-stu-id="952b7-521">Retrieve the rotation of the simulated eyes.</span></span> <span data-ttu-id="952b7-522">當您沿著軸旋轉時，正弧度會順時針旋轉。</span><span class="sxs-lookup"><span data-stu-id="952b7-522">Positive radians rotate clockwise when looking along the axis.</span></span>

<span data-ttu-id="952b7-523">**PerceptionSimulation. ISimulatedEyes (PerceptionSimulation. Rotation3)**</span><span class="sxs-lookup"><span data-stu-id="952b7-523">**Microsoft.PerceptionSimulation.ISimulatedEyes.Rotate(Microsoft.PerceptionSimulation.Rotation3)**</span></span>

<span data-ttu-id="952b7-524">相對於目前的旋轉旋轉模擬眼睛。</span><span class="sxs-lookup"><span data-stu-id="952b7-524">Rotate the simulated eyes relative to its current rotation.</span></span> <span data-ttu-id="952b7-525">當您沿著軸旋轉時，正弧度會順時針旋轉。</span><span class="sxs-lookup"><span data-stu-id="952b7-525">Positive radians rotate clockwise when looking along the axis.</span></span>

<span data-ttu-id="952b7-526">參數</span><span class="sxs-lookup"><span data-stu-id="952b7-526">Parameters</span></span>
* <span data-ttu-id="952b7-527">旋轉-要旋轉的量。</span><span class="sxs-lookup"><span data-stu-id="952b7-527">rotation - The amount to rotate.</span></span>

<span data-ttu-id="952b7-528">**PerceptionSimulation. ISimulatedEyes. CalibrationState**</span><span class="sxs-lookup"><span data-stu-id="952b7-528">**Microsoft.PerceptionSimulation.ISimulatedEyes.CalibrationState**</span></span>

<span data-ttu-id="952b7-529">抓取或設定模擬眼睛的校正狀態。</span><span class="sxs-lookup"><span data-stu-id="952b7-529">Retrieves or sets the calibration state of the simulated eyes.</span></span>

<span data-ttu-id="952b7-530">**PerceptionSimulation. ISimulatedEyes. WorldPosition**</span><span class="sxs-lookup"><span data-stu-id="952b7-530">**Microsoft.PerceptionSimulation.ISimulatedEyes.WorldPosition**</span></span>

<span data-ttu-id="952b7-531">以計量表的關聯性抓取節點的位置。</span><span class="sxs-lookup"><span data-stu-id="952b7-531">Retrieve the position of the node with relation to the world, in meters.</span></span>


### <a name="microsoftperceptionsimulationisimulationrecording"></a><span data-ttu-id="952b7-532">PerceptionSimulation. ISimulationRecording</span><span class="sxs-lookup"><span data-stu-id="952b7-532">Microsoft.PerceptionSimulation.ISimulationRecording</span></span>

<span data-ttu-id="952b7-533">用來與載入以播放的單一錄製互動的介面。</span><span class="sxs-lookup"><span data-stu-id="952b7-533">Interface for interacting with a single recording loaded for playback.</span></span>

```
public interface ISimulationRecording
{
    StreamDataTypes DataTypes { get; }
    PlaybackState State { get; }
    void Play();
    void Pause();
    void Seek(UInt64 ticks);
    void Stop();
};
```

<span data-ttu-id="952b7-534">**PerceptionSimulation. ISimulationRecording 資料類型**</span><span class="sxs-lookup"><span data-stu-id="952b7-534">**Microsoft.PerceptionSimulation.ISimulationRecording.DataTypes**</span></span>

<span data-ttu-id="952b7-535">抓取錄製中的資料類型清單。</span><span class="sxs-lookup"><span data-stu-id="952b7-535">Retrieves the list of data types in the recording.</span></span>

<span data-ttu-id="952b7-536">**PerceptionSimulation. ISimulationRecording. 州**</span><span class="sxs-lookup"><span data-stu-id="952b7-536">**Microsoft.PerceptionSimulation.ISimulationRecording.State**</span></span>

<span data-ttu-id="952b7-537">捕獲錄製的目前狀態。</span><span class="sxs-lookup"><span data-stu-id="952b7-537">Retrieves the current state of the recording.</span></span>

<span data-ttu-id="952b7-538">**PerceptionSimulation. ISimulationRecording. Play**</span><span class="sxs-lookup"><span data-stu-id="952b7-538">**Microsoft.PerceptionSimulation.ISimulationRecording.Play**</span></span>

<span data-ttu-id="952b7-539">開始播放。</span><span class="sxs-lookup"><span data-stu-id="952b7-539">Start the playback.</span></span> <span data-ttu-id="952b7-540">如果錄製暫停，播放將從暫停的位置繼續;如果已停止，播放會從一開始開始。</span><span class="sxs-lookup"><span data-stu-id="952b7-540">If the recording is paused, playback will resume from the paused location; if stopped, playback will start at the beginning.</span></span> <span data-ttu-id="952b7-541">如果已經在播放，則會忽略此呼叫。</span><span class="sxs-lookup"><span data-stu-id="952b7-541">If already playing, this call is ignored.</span></span>

<span data-ttu-id="952b7-542">**PerceptionSimulation. ISimulationRecording. Pause**</span><span class="sxs-lookup"><span data-stu-id="952b7-542">**Microsoft.PerceptionSimulation.ISimulationRecording.Pause**</span></span>

<span data-ttu-id="952b7-543">在目前的位置暫停播放。</span><span class="sxs-lookup"><span data-stu-id="952b7-543">Pauses the playback at its current location.</span></span> <span data-ttu-id="952b7-544">如果記錄已停止，則會忽略呼叫。</span><span class="sxs-lookup"><span data-stu-id="952b7-544">If the recording is stopped, the call is ignored.</span></span>

<span data-ttu-id="952b7-545">**PerceptionSimulation. ISimulationRecording. Seek (System.object)**</span><span class="sxs-lookup"><span data-stu-id="952b7-545">**Microsoft.PerceptionSimulation.ISimulationRecording.Seek(System.UInt64)**</span></span>

<span data-ttu-id="952b7-546">搜尋指定時間的記錄 (從開始) 的 100-毫微秒間隔，然後在該位置暫停。</span><span class="sxs-lookup"><span data-stu-id="952b7-546">Seeks the recording to the specified time (in 100-nanoseconds intervals from the beginning) and pauses at that location.</span></span> <span data-ttu-id="952b7-547">如果時間超過錄製的結尾，則會在最後一個畫面上暫停。</span><span class="sxs-lookup"><span data-stu-id="952b7-547">If the time is beyond the end of the recording, it's paused at the last frame.</span></span>

<span data-ttu-id="952b7-548">參數</span><span class="sxs-lookup"><span data-stu-id="952b7-548">Parameters</span></span>
* <span data-ttu-id="952b7-549">刻度-要搜尋的時間。</span><span class="sxs-lookup"><span data-stu-id="952b7-549">ticks - The time to which to seek.</span></span>

<span data-ttu-id="952b7-550">**PerceptionSimulation. ISimulationRecording. Stop**</span><span class="sxs-lookup"><span data-stu-id="952b7-550">**Microsoft.PerceptionSimulation.ISimulationRecording.Stop**</span></span>

<span data-ttu-id="952b7-551">停止播放，並將位置重設為開頭。</span><span class="sxs-lookup"><span data-stu-id="952b7-551">Stops the playback and resets the position to the beginning.</span></span>

### <a name="microsoftperceptionsimulationisimulationrecordingcallback"></a><span data-ttu-id="952b7-552">PerceptionSimulation. ISimulationRecordingCallback</span><span class="sxs-lookup"><span data-stu-id="952b7-552">Microsoft.PerceptionSimulation.ISimulationRecordingCallback</span></span>

<span data-ttu-id="952b7-553">在播放期間接收狀態變更的介面。</span><span class="sxs-lookup"><span data-stu-id="952b7-553">Interface for receiving state changes during playback.</span></span>

```
public interface ISimulationRecordingCallback
{
    void PlaybackStateChanged(PlaybackState newState);
};
```

<span data-ttu-id="952b7-554">**PerceptionSimulation. ISimulationRecordingCallback. PlaybackStateChanged (Microsoft. PerceptionSimulation. PlaybackState)**</span><span class="sxs-lookup"><span data-stu-id="952b7-554">**Microsoft.PerceptionSimulation.ISimulationRecordingCallback.PlaybackStateChanged(Microsoft.PerceptionSimulation.PlaybackState)**</span></span>

<span data-ttu-id="952b7-555">在 ISimulationRecording 的播放狀態變更時呼叫。</span><span class="sxs-lookup"><span data-stu-id="952b7-555">Called when an ISimulationRecording's playback state has changed.</span></span>

<span data-ttu-id="952b7-556">參數</span><span class="sxs-lookup"><span data-stu-id="952b7-556">Parameters</span></span>
* <span data-ttu-id="952b7-557">newState-錄製的新狀態。</span><span class="sxs-lookup"><span data-stu-id="952b7-557">newState - The new state of the recording.</span></span>

### <a name="microsoftperceptionsimulationperceptionsimulationmanager"></a><span data-ttu-id="952b7-558">PerceptionSimulation. PerceptionSimulationManager</span><span class="sxs-lookup"><span data-stu-id="952b7-558">Microsoft.PerceptionSimulation.PerceptionSimulationManager</span></span>

<span data-ttu-id="952b7-559">用來建立感知模擬物件的根物件。</span><span class="sxs-lookup"><span data-stu-id="952b7-559">Root object for creating Perception Simulation objects.</span></span>

```
public static class PerceptionSimulationManager
{
    public static IPerceptionSimulationManager CreatePerceptionSimulationManager(ISimulationStreamSink sink);
    public static ISimulationStreamSink CreatePerceptionSimulationRecording(string path);
    public static ISimulationRecording LoadPerceptionSimulationRecording(string path, ISimulationStreamSinkFactory factory);
    public static ISimulationRecording LoadPerceptionSimulationRecording(string path, ISimulationStreamSinkFactory factory, ISimulationRecordingCallback callback);
```

<span data-ttu-id="952b7-560">**PerceptionSimulation. PerceptionSimulationManager. CreatePerceptionSimulationManager (Microsoft. PerceptionSimulation. ISimulationStreamSink)**</span><span class="sxs-lookup"><span data-stu-id="952b7-560">**Microsoft.PerceptionSimulation.PerceptionSimulationManager.CreatePerceptionSimulationManager(Microsoft.PerceptionSimulation.ISimulationStreamSink)**</span></span>

<span data-ttu-id="952b7-561">在物件上建立以產生模擬的封包，並將其傳遞至提供的接收。</span><span class="sxs-lookup"><span data-stu-id="952b7-561">Create on object for generating simulated packets and delivering them to the provided sink.</span></span>

<span data-ttu-id="952b7-562">參數</span><span class="sxs-lookup"><span data-stu-id="952b7-562">Parameters</span></span>
* <span data-ttu-id="952b7-563">接收-將接收所有產生的封包的接收。</span><span class="sxs-lookup"><span data-stu-id="952b7-563">sink - The sink that will receive all generated packets.</span></span>

<span data-ttu-id="952b7-564">傳回值</span><span class="sxs-lookup"><span data-stu-id="952b7-564">Return value</span></span>

<span data-ttu-id="952b7-565">建立的管理員。</span><span class="sxs-lookup"><span data-stu-id="952b7-565">The created Manager.</span></span>

<span data-ttu-id="952b7-566">**PerceptionSimulation. PerceptionSimulationManager. CreatePerceptionSimulationRecording (System.string)**</span><span class="sxs-lookup"><span data-stu-id="952b7-566">**Microsoft.PerceptionSimulation.PerceptionSimulationManager.CreatePerceptionSimulationRecording(System.String)**</span></span>

<span data-ttu-id="952b7-567">建立接收，以將所有收到的封包儲存在指定路徑的檔案中。</span><span class="sxs-lookup"><span data-stu-id="952b7-567">Create a sink, which stores all received packets in a file at the specified path.</span></span>

<span data-ttu-id="952b7-568">參數</span><span class="sxs-lookup"><span data-stu-id="952b7-568">Parameters</span></span>
* <span data-ttu-id="952b7-569">路徑-要建立之檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="952b7-569">path - The path of the file to create.</span></span>

<span data-ttu-id="952b7-570">傳回值</span><span class="sxs-lookup"><span data-stu-id="952b7-570">Return value</span></span>

<span data-ttu-id="952b7-571">建立的接收。</span><span class="sxs-lookup"><span data-stu-id="952b7-571">The created sink.</span></span>

<span data-ttu-id="952b7-572">**PerceptionSimulation. PerceptionSimulationManager. LoadPerceptionSimulationRecording (System.string，PerceptionSimulation. ISimulationStreamSinkFactory)**</span><span class="sxs-lookup"><span data-stu-id="952b7-572">**Microsoft.PerceptionSimulation.PerceptionSimulationManager.LoadPerceptionSimulationRecording(System.String,Microsoft.PerceptionSimulation.ISimulationStreamSinkFactory)**</span></span>

<span data-ttu-id="952b7-573">從指定的檔案載入錄製。</span><span class="sxs-lookup"><span data-stu-id="952b7-573">Load a recording from the specified file.</span></span>

<span data-ttu-id="952b7-574">參數</span><span class="sxs-lookup"><span data-stu-id="952b7-574">Parameters</span></span>
* <span data-ttu-id="952b7-575">路徑-要載入之檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="952b7-575">path - The path of the file to load.</span></span>
* <span data-ttu-id="952b7-576">factory-記錄用來在必要時建立 ISimulationStreamSink 的 factory。</span><span class="sxs-lookup"><span data-stu-id="952b7-576">factory - A factory used by the recording for creating an ISimulationStreamSink when required.</span></span>

<span data-ttu-id="952b7-577">傳回值</span><span class="sxs-lookup"><span data-stu-id="952b7-577">Return value</span></span>

<span data-ttu-id="952b7-578">載入的記錄。</span><span class="sxs-lookup"><span data-stu-id="952b7-578">The loaded recording.</span></span>

<span data-ttu-id="952b7-579">**PerceptionSimulation. PerceptionSimulationManager. LoadPerceptionSimulationRecording (System.string、PerceptionSimulation. ISimulationStreamSinkFactory、PerceptionSimulation. ISimulationRecordingCallback)**</span><span class="sxs-lookup"><span data-stu-id="952b7-579">**Microsoft.PerceptionSimulation.PerceptionSimulationManager.LoadPerceptionSimulationRecording(System.String,Microsoft.PerceptionSimulation.ISimulationStreamSinkFactory,Microsoft.PerceptionSimulation.ISimulationRecordingCallback)**</span></span>

<span data-ttu-id="952b7-580">從指定的檔案載入錄製。</span><span class="sxs-lookup"><span data-stu-id="952b7-580">Load a recording from the specified file.</span></span>

<span data-ttu-id="952b7-581">參數</span><span class="sxs-lookup"><span data-stu-id="952b7-581">Parameters</span></span>
* <span data-ttu-id="952b7-582">路徑-要載入之檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="952b7-582">path - The path of the file to load.</span></span>
* <span data-ttu-id="952b7-583">factory-記錄用來在必要時建立 ISimulationStreamSink 的 factory。</span><span class="sxs-lookup"><span data-stu-id="952b7-583">factory - A factory used by the recording for creating an ISimulationStreamSink when required.</span></span>
* <span data-ttu-id="952b7-584">回呼-回呼，可接收更新 regrading 記錄的狀態。</span><span class="sxs-lookup"><span data-stu-id="952b7-584">callback - A callback, which receives updates regrading the recording's status.</span></span>

<span data-ttu-id="952b7-585">傳回值</span><span class="sxs-lookup"><span data-stu-id="952b7-585">Return value</span></span>

<span data-ttu-id="952b7-586">載入的記錄。</span><span class="sxs-lookup"><span data-stu-id="952b7-586">The loaded recording.</span></span>

### <a name="microsoftperceptionsimulationstreamdatatypes"></a><span data-ttu-id="952b7-587">PerceptionSimulation. StreamDataTypes</span><span class="sxs-lookup"><span data-stu-id="952b7-587">Microsoft.PerceptionSimulation.StreamDataTypes</span></span>

<span data-ttu-id="952b7-588">描述不同類型的資料流程資料。</span><span class="sxs-lookup"><span data-stu-id="952b7-588">Describes the different types of stream data.</span></span>

```
public enum StreamDataTypes
{
    None = 0x00,
    Head = 0x01,
    Hands = 0x02,
    SpatialMapping = 0x08,
    Calibration = 0x10,
    Environment = 0x20,
    SixDofControllers = 0x40,
    Eyes = 0x80,
    DisplayConfiguration = 0x100
    All = None | Head | Hands | SpatialMapping | Calibration | Environment | SixDofControllers | Eyes | DisplayConfiguration
}
```

<span data-ttu-id="952b7-589">**PerceptionSimulation. StreamDataTypes. None**</span><span class="sxs-lookup"><span data-stu-id="952b7-589">**Microsoft.PerceptionSimulation.StreamDataTypes.None**</span></span>

<span data-ttu-id="952b7-590">用來指出沒有資料流程資料類型的 sentinel 值。</span><span class="sxs-lookup"><span data-stu-id="952b7-590">A sentinel value used to indicate no stream data types.</span></span>

<span data-ttu-id="952b7-591">**PerceptionSimulation. StreamDataTypes Head**</span><span class="sxs-lookup"><span data-stu-id="952b7-591">**Microsoft.PerceptionSimulation.StreamDataTypes.Head**</span></span>

<span data-ttu-id="952b7-592">標頭的位置和方向的資料流程。</span><span class="sxs-lookup"><span data-stu-id="952b7-592">Stream of data for the position and orientation of the head.</span></span>

<span data-ttu-id="952b7-593">**PerceptionSimulation. StreamDataTypes**</span><span class="sxs-lookup"><span data-stu-id="952b7-593">**Microsoft.PerceptionSimulation.StreamDataTypes.Hands**</span></span>

<span data-ttu-id="952b7-594">資料串流以取得手的位置和手勢。</span><span class="sxs-lookup"><span data-stu-id="952b7-594">Stream of data for the position and gestures of hands.</span></span>

<span data-ttu-id="952b7-595">**PerceptionSimulation. StreamDataTypes. SpatialMapping**</span><span class="sxs-lookup"><span data-stu-id="952b7-595">**Microsoft.PerceptionSimulation.StreamDataTypes.SpatialMapping**</span></span>

<span data-ttu-id="952b7-596">環境的空間對應資料串流。</span><span class="sxs-lookup"><span data-stu-id="952b7-596">Stream of data for spatial mapping of the environment.</span></span>

<span data-ttu-id="952b7-597">**PerceptionSimulation. StreamDataTypes. 校正**</span><span class="sxs-lookup"><span data-stu-id="952b7-597">**Microsoft.PerceptionSimulation.StreamDataTypes.Calibration**</span></span>

<span data-ttu-id="952b7-598">用於校正裝置的資料流程。</span><span class="sxs-lookup"><span data-stu-id="952b7-598">Stream of data for calibration of the device.</span></span> <span data-ttu-id="952b7-599">只有在遠端模式的系統才會接受校正封包。</span><span class="sxs-lookup"><span data-stu-id="952b7-599">Calibration packets are only accepted by a system in Remote Mode.</span></span>

<span data-ttu-id="952b7-600">**PerceptionSimulation. StreamDataTypes. 環境**</span><span class="sxs-lookup"><span data-stu-id="952b7-600">**Microsoft.PerceptionSimulation.StreamDataTypes.Environment**</span></span>

<span data-ttu-id="952b7-601">裝置環境的資料流程。</span><span class="sxs-lookup"><span data-stu-id="952b7-601">Stream of data for the environment of the device.</span></span>

<span data-ttu-id="952b7-602">**PerceptionSimulation. StreamDataTypes. SixDofControllers**</span><span class="sxs-lookup"><span data-stu-id="952b7-602">**Microsoft.PerceptionSimulation.StreamDataTypes.SixDofControllers**</span></span>

<span data-ttu-id="952b7-603">動作控制器的資料流程。</span><span class="sxs-lookup"><span data-stu-id="952b7-603">Stream of data for motion controllers.</span></span>

<span data-ttu-id="952b7-604">**PerceptionSimulation. StreamDataTypes 眼睛**</span><span class="sxs-lookup"><span data-stu-id="952b7-604">**Microsoft.PerceptionSimulation.StreamDataTypes.Eyes**</span></span>

<span data-ttu-id="952b7-605">以模擬人類的眼睛來串流資料。</span><span class="sxs-lookup"><span data-stu-id="952b7-605">Stream of data with the eyes of the simulated human.</span></span>

<span data-ttu-id="952b7-606">**PerceptionSimulation. StreamDataTypes. DisplayConfiguration**</span><span class="sxs-lookup"><span data-stu-id="952b7-606">**Microsoft.PerceptionSimulation.StreamDataTypes.DisplayConfiguration**</span></span>

<span data-ttu-id="952b7-607">以裝置的顯示設定來串流資料。</span><span class="sxs-lookup"><span data-stu-id="952b7-607">Stream of data with the display configuration of the device.</span></span>

<span data-ttu-id="952b7-608">**PerceptionSimulation. StreamDataTypes**</span><span class="sxs-lookup"><span data-stu-id="952b7-608">**Microsoft.PerceptionSimulation.StreamDataTypes.All**</span></span>

<span data-ttu-id="952b7-609">用來表示所有記錄資料類型的 sentinel 值。</span><span class="sxs-lookup"><span data-stu-id="952b7-609">A sentinel value used to indicate all recorded data types.</span></span>

### <a name="microsoftperceptionsimulationisimulationstreamsink"></a><span data-ttu-id="952b7-610">PerceptionSimulation. ISimulationStreamSink</span><span class="sxs-lookup"><span data-stu-id="952b7-610">Microsoft.PerceptionSimulation.ISimulationStreamSink</span></span>

<span data-ttu-id="952b7-611">物件，這個物件會接收來自模擬資料流程的資料封包。</span><span class="sxs-lookup"><span data-stu-id="952b7-611">An object that receives data packets from a simulation stream.</span></span>

```
public interface ISimulationStreamSink
{
    void OnPacketReceived(uint length, byte[] packet);
}
```

<span data-ttu-id="952b7-612">**PerceptionSimulation. ISimulationStreamSink. OnPacketReceived (uint length，byte [] 封包)**</span><span class="sxs-lookup"><span data-stu-id="952b7-612">**Microsoft.PerceptionSimulation.ISimulationStreamSink.OnPacketReceived(uint length, byte[] packet)**</span></span>

<span data-ttu-id="952b7-613">接收在內部輸入和建立版本的單一封包。</span><span class="sxs-lookup"><span data-stu-id="952b7-613">Receives a single packet, which is internally typed and versioned.</span></span>

<span data-ttu-id="952b7-614">參數</span><span class="sxs-lookup"><span data-stu-id="952b7-614">Parameters</span></span>
* <span data-ttu-id="952b7-615">長度-封包的長度。</span><span class="sxs-lookup"><span data-stu-id="952b7-615">length - The length of the packet.</span></span>
* <span data-ttu-id="952b7-616">封包-封包的資料。</span><span class="sxs-lookup"><span data-stu-id="952b7-616">packet - The data of the packet.</span></span>

### <a name="microsoftperceptionsimulationisimulationstreamsinkfactory"></a><span data-ttu-id="952b7-617">PerceptionSimulation. ISimulationStreamSinkFactory</span><span class="sxs-lookup"><span data-stu-id="952b7-617">Microsoft.PerceptionSimulation.ISimulationStreamSinkFactory</span></span>

<span data-ttu-id="952b7-618">建立 ISimulationStreamSink 的物件。</span><span class="sxs-lookup"><span data-stu-id="952b7-618">An object that creates ISimulationStreamSink.</span></span>

```
public interface ISimulationStreamSinkFactory
{
    ISimulationStreamSink CreateSimulationStreamSink();
}
```

<span data-ttu-id="952b7-619">**PerceptionSimulation. ISimulationStreamSinkFactory. CreateSimulationStreamSink ( # B1**</span><span class="sxs-lookup"><span data-stu-id="952b7-619">**Microsoft.PerceptionSimulation.ISimulationStreamSinkFactory.CreateSimulationStreamSink()**</span></span>

<span data-ttu-id="952b7-620">建立 ISimulationStreamSink 的單一實例。</span><span class="sxs-lookup"><span data-stu-id="952b7-620">Creates a single instance of ISimulationStreamSink.</span></span>

<span data-ttu-id="952b7-621">傳回值</span><span class="sxs-lookup"><span data-stu-id="952b7-621">Return value</span></span>

<span data-ttu-id="952b7-622">建立的接收。</span><span class="sxs-lookup"><span data-stu-id="952b7-622">The created sink.</span></span>
