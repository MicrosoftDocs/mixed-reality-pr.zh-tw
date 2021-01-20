---
title: Unity 中的語音輸入
description: 瞭解 Unity 如何公開三種方式，將語音輸入、語音辨識和聽寫新增至您的 Windows Mixed Reality 應用程式。
author: thetuvix
ms.author: alexturn
ms.date: 03/21/2018
ms.topic: article
keywords: 語音輸入、KeywordRecognizer、GrammarRecognizer、麥克風、聽寫、語音、混合現實耳機、windows mixed reality 耳機、虛擬實境耳機、MRTK、混合現實工具組
ms.openlocfilehash: c6364b190ca90c5e6faf7fb8ef79314134e93cfc
ms.sourcegitcommit: d3a3b4f13b3728cfdd4d43035c806c0791d3f2fe
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/20/2021
ms.locfileid: "98583723"
---
# <a name="voice-input-in-unity"></a><span data-ttu-id="ad6b9-104">Unity 中的語音輸入</span><span class="sxs-lookup"><span data-stu-id="ad6b9-104">Voice input in Unity</span></span>

>[!NOTE]
><span data-ttu-id="ad6b9-105">除了下列資訊之外，請考慮使用適用于認知語音服務 SDK 的 Unity 外掛程式，其具有更好的語音精確度結果，並可讓您輕鬆存取語音轉換文字解碼和先進的語音功能，例如對話方塊、意圖型互動、轉譯、文字轉語音的合成和自然語言的語音辨識。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-105">Instead of the below information, consider using the Unity plug-in for the Cognitive Speech Services SDK which has much better Speech Accuracy results and provides easy access to speech-to-text decode and advanced speech features like dialog, intent based interaction, translation, text-to-speech synthesis and natural language speech recognition.</span></span> <span data-ttu-id="ad6b9-106">在這裡尋找範例和檔： https://docs.microsoft.com//azure/cognitive-services/speech-service/quickstart-csharp-unity</span><span class="sxs-lookup"><span data-stu-id="ad6b9-106">Find the sample and documentaion here: https://docs.microsoft.com//azure/cognitive-services/speech-service/quickstart-csharp-unity</span></span>   

<span data-ttu-id="ad6b9-107">Unity 公開三種將 [語音輸入](../../design/voice-input.md) 新增至 Unity 應用程式的方式。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-107">Unity exposes three ways to add [Voice input](../../design/voice-input.md) to your Unity application.</span></span>

<span data-ttu-id="ad6b9-108">KeywordRecognizer (兩種 PhraseRecognizers 類型的其中一種) ，您的應用程式可以指定要接聽的字串命令陣列。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-108">With the KeywordRecognizer (one of two types of PhraseRecognizers), your app can be given an array of string commands to listen for.</span></span> <span data-ttu-id="ad6b9-109">GrammarRecognizer (PhraseRecognizer) 的另一種類型時，您的應用程式可以取得定義特定文法的 SRGS 檔案來接聽。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-109">With the GrammarRecognizer (the other type of PhraseRecognizer), your app can be given an SRGS file defining a specific grammar to listen for.</span></span> <span data-ttu-id="ad6b9-110">使用 DictationRecognizer 時，您的應用程式可以接聽任何單字，並提供使用者語音的備註或其他顯示。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-110">With the DictationRecognizer, your app can listen for any word and provide the user with a note or other display of their speech.</span></span>

>[!NOTE]
><span data-ttu-id="ad6b9-111">一次只能處理聽寫或片語辨識。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-111">Only dictation or phrase recognition can be handled at once.</span></span> <span data-ttu-id="ad6b9-112">這表示，如果 GrammarRecognizer 或 KeywordRecognizer 處於作用中狀態，DictationRecognizer 就無法作用，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-112">That means if a GrammarRecognizer or KeywordRecognizer is active, a DictationRecognizer can not be active and vice versa.</span></span>

## <a name="enabling-the-capability-for-voice"></a><span data-ttu-id="ad6b9-113">啟用語音的功能</span><span class="sxs-lookup"><span data-stu-id="ad6b9-113">Enabling the capability for Voice</span></span>

<span data-ttu-id="ad6b9-114">必須為應用程式宣告 **麥克風** 功能，才能使用語音輸入。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-114">The **Microphone** capability must be declared for an app to use Voice input.</span></span>
1. <span data-ttu-id="ad6b9-115">在 Unity 編輯器中，流覽至 [> Player 編輯 > 專案設定]，移至播放機設定</span><span class="sxs-lookup"><span data-stu-id="ad6b9-115">In the Unity Editor, go to the player settings by navigating to "Edit > Project Settings > Player"</span></span>
2. <span data-ttu-id="ad6b9-116">選取 [Windows 存放區] 索引標籤</span><span class="sxs-lookup"><span data-stu-id="ad6b9-116">Select on the "Windows Store" tab</span></span>
3. <span data-ttu-id="ad6b9-117">在 [發佈設定 > 功能] 區段中，檢查 **麥克風** 功能</span><span class="sxs-lookup"><span data-stu-id="ad6b9-117">In the "Publishing Settings > Capabilities" section, check the **Microphone** capability</span></span>

## <a name="phrase-recognition"></a><span data-ttu-id="ad6b9-118">片語辨識</span><span class="sxs-lookup"><span data-stu-id="ad6b9-118">Phrase Recognition</span></span>

<span data-ttu-id="ad6b9-119">若要讓您的應用程式接聽使用者所說的特定片語，請採取一些動作，您必須：</span><span class="sxs-lookup"><span data-stu-id="ad6b9-119">To enable your app to listen for specific phrases spoken by the user then take some action, you need to:</span></span>
1. <span data-ttu-id="ad6b9-120">使用 KeywordRecognizer 或 GrammarRecognizer 指定要接聽的片語</span><span class="sxs-lookup"><span data-stu-id="ad6b9-120">Specify which phrases to listen for using a KeywordRecognizer or GrammarRecognizer</span></span>
2. <span data-ttu-id="ad6b9-121">處理 OnPhraseRecognized 事件，並採取對應至已辨識片語的動作</span><span class="sxs-lookup"><span data-stu-id="ad6b9-121">Handle the OnPhraseRecognized event and take action corresponding to the phrase recognized</span></span>

### <a name="keywordrecognizer"></a><span data-ttu-id="ad6b9-122">KeywordRecognizer</span><span class="sxs-lookup"><span data-stu-id="ad6b9-122">KeywordRecognizer</span></span>

<span data-ttu-id="ad6b9-123">**命名空間：** *UnityEngine*</span><span class="sxs-lookup"><span data-stu-id="ad6b9-123">**Namespace:** *UnityEngine.Windows.Speech*</span></span><br>
<span data-ttu-id="ad6b9-124">**類型：** *KeywordRecognizer*、 *PhraseRecognizedEventArgs*、 *SpeechError*、 *SpeechSystemStatus*</span><span class="sxs-lookup"><span data-stu-id="ad6b9-124">**Types:** *KeywordRecognizer*, *PhraseRecognizedEventArgs*, *SpeechError*, *SpeechSystemStatus*</span></span>

<span data-ttu-id="ad6b9-125">我們需要一些 using 語句來節省一些按鍵：</span><span class="sxs-lookup"><span data-stu-id="ad6b9-125">We'll need a few using statements to save some keystrokes:</span></span>

```
using UnityEngine.Windows.Speech;
using System.Collections.Generic;
using System.Linq;
```

<span data-ttu-id="ad6b9-126">然後讓我們將幾個欄位新增至您的類別，以儲存辨識器和關鍵字 >動作字典：</span><span class="sxs-lookup"><span data-stu-id="ad6b9-126">Then let's add a few fields to your class to store the recognizer and keyword->action dictionary:</span></span>

```
KeywordRecognizer keywordRecognizer;
Dictionary<string, System.Action> keywords = new Dictionary<string, System.Action>();
```

<span data-ttu-id="ad6b9-127">現在將關鍵字加入至字典，例如，在 Start ( # A1 方法中。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-127">Now add a keyword to the dictionary, for example in of a Start() method.</span></span> <span data-ttu-id="ad6b9-128">在此範例中，我們要新增 "activate" 關鍵字：</span><span class="sxs-lookup"><span data-stu-id="ad6b9-128">We're adding the "activate" keyword in this example:</span></span>

```
//Create keywords for keyword recognizer
keywords.Add("activate", () =>
{
    // action to be performed when this keyword is spoken
});
```

<span data-ttu-id="ad6b9-129">建立關鍵字辨識器並告訴它要辨識的內容：</span><span class="sxs-lookup"><span data-stu-id="ad6b9-129">Create the keyword recognizer and tell it what we want to recognize:</span></span>

```
keywordRecognizer = new KeywordRecognizer(keywords.Keys.ToArray());
```

<span data-ttu-id="ad6b9-130">現在報名 OnPhraseRecognized 事件</span><span class="sxs-lookup"><span data-stu-id="ad6b9-130">Now register for the OnPhraseRecognized event</span></span>

```
keywordRecognizer.OnPhraseRecognized += KeywordRecognizer_OnPhraseRecognized;
```

<span data-ttu-id="ad6b9-131">範例處理常式如下：</span><span class="sxs-lookup"><span data-stu-id="ad6b9-131">An example handler is:</span></span>

```
private void KeywordRecognizer_OnPhraseRecognized(PhraseRecognizedEventArgs args)
{
    System.Action keywordAction;
    // if the keyword recognized is in our dictionary, call that Action.
    if (keywords.TryGetValue(args.text, out keywordAction))
    {
        keywordAction.Invoke();
    }
}
```

<span data-ttu-id="ad6b9-132">最後，開始辨識！</span><span class="sxs-lookup"><span data-stu-id="ad6b9-132">Finally, start recognizing!</span></span>

```
keywordRecognizer.Start();
```

### <a name="grammarrecognizer"></a><span data-ttu-id="ad6b9-133">GrammarRecognizer</span><span class="sxs-lookup"><span data-stu-id="ad6b9-133">GrammarRecognizer</span></span>

<span data-ttu-id="ad6b9-134">**命名空間：** *UnityEngine*</span><span class="sxs-lookup"><span data-stu-id="ad6b9-134">**Namespace:** *UnityEngine.Windows.Speech*</span></span><br>
<span data-ttu-id="ad6b9-135">**類型**： *GrammarRecognizer*、 *PhraseRecognizedEventArgs*、 *SpeechError*、 *SpeechSystemStatus*</span><span class="sxs-lookup"><span data-stu-id="ad6b9-135">**Types**: *GrammarRecognizer*, *PhraseRecognizedEventArgs*, *SpeechError*, *SpeechSystemStatus*</span></span>

<span data-ttu-id="ad6b9-136">如果您要使用 SRGS 指定辨識文法，則會使用 GrammarRecognizer。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-136">The GrammarRecognizer is used if you're specifying your recognition grammar using SRGS.</span></span> <span data-ttu-id="ad6b9-137">如果您的應用程式有多個關鍵字，如果您想要辨識更複雜的片語，或是想要輕鬆開啟和關閉命令集，這會很有用。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-137">This can be useful if your app has more than just a few keywords, if you want to recognize more complex phrases, or if you want to easily turn on and off sets of commands.</span></span> <span data-ttu-id="ad6b9-138">請參閱： [使用 SRGS XML 建立](/previous-versions/office/developer/speech-technologies/hh378349(v=office.14)) 檔案格式資訊的文法。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-138">See: [Create Grammars Using SRGS XML](/previous-versions/office/developer/speech-technologies/hh378349(v=office.14)) for file format information.</span></span>

<span data-ttu-id="ad6b9-139">一旦您有 SRGS 文法，而且它位於 [StreamingAssets 資料夾](https://docs.unity3d.com/Manual/StreamingAssets.html)的專案中：</span><span class="sxs-lookup"><span data-stu-id="ad6b9-139">Once you have your SRGS grammar, and it is in your project in a [StreamingAssets folder](https://docs.unity3d.com/Manual/StreamingAssets.html):</span></span>

```
<PROJECT_ROOT>/Assets/StreamingAssets/SRGS/myGrammar.xml
```

<span data-ttu-id="ad6b9-140">建立 GrammarRecognizer，並將路徑傳遞至您的 SRGS 檔案：</span><span class="sxs-lookup"><span data-stu-id="ad6b9-140">Create a GrammarRecognizer and pass it the path to your SRGS file:</span></span>

```
private GrammarRecognizer grammarRecognizer;
grammarRecognizer = new GrammarRecognizer(Application.streamingDataPath + "/SRGS/myGrammar.xml");
```

<span data-ttu-id="ad6b9-141">現在報名 OnPhraseRecognized 事件</span><span class="sxs-lookup"><span data-stu-id="ad6b9-141">Now register for the OnPhraseRecognized event</span></span>

```
grammarRecognizer.OnPhraseRecognized += grammarRecognizer_OnPhraseRecognized;
```

<span data-ttu-id="ad6b9-142">您將會取得一個回呼，其中包含您可以適當處理的 SRGS 文法中指定的資訊。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-142">You'll get a callback containing information specified in your SRGS grammar, which you can handle appropriately.</span></span> <span data-ttu-id="ad6b9-143">大部分的重要資訊都會在 semanticMeanings 陣列中提供。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-143">Most of the important information will be provided in the semanticMeanings array.</span></span>

```
private void Grammar_OnPhraseRecognized(PhraseRecognizedEventArgs args)
{
    SemanticMeaning[] meanings = args.semanticMeanings;
    // do something
}
```

<span data-ttu-id="ad6b9-144">最後，開始辨識！</span><span class="sxs-lookup"><span data-stu-id="ad6b9-144">Finally, start recognizing!</span></span>

```
grammarRecognizer.Start();
```

## <a name="dictation"></a><span data-ttu-id="ad6b9-145">聽寫</span><span class="sxs-lookup"><span data-stu-id="ad6b9-145">Dictation</span></span>

<span data-ttu-id="ad6b9-146">**命名空間：** *UnityEngine*</span><span class="sxs-lookup"><span data-stu-id="ad6b9-146">**Namespace:** *UnityEngine.Windows.Speech*</span></span><br>
<span data-ttu-id="ad6b9-147">**類型**： *DictationRecognizer*、 *SpeechError*、 *SpeechSystemStatus*</span><span class="sxs-lookup"><span data-stu-id="ad6b9-147">**Types**: *DictationRecognizer*, *SpeechError*, *SpeechSystemStatus*</span></span>

<span data-ttu-id="ad6b9-148">使用 DictationRecognizer 將使用者的語音轉換成文字。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-148">Use the DictationRecognizer to convert the user's speech to text.</span></span> <span data-ttu-id="ad6b9-149">DictationRecognizer 會公開 [聽寫](../../design/voice-input.md#dictation) 功能，並支援註冊和接聽假設和片語完成的事件，讓您可以在使用者說話和之後，將意見反應提供給您的使用者。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-149">The DictationRecognizer exposes [dictation](../../design/voice-input.md#dictation) functionality and supports registering and listening for hypothesis and phrase completed events, so you can give feedback to your user both while they speak and afterwards.</span></span> <span data-ttu-id="ad6b9-150">開始 ( # A1 並停止 ( # A3 方法，分別啟用和停用聽寫辨識。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-150">Start() and Stop() methods respectively enable and disable dictation recognition.</span></span> <span data-ttu-id="ad6b9-151">完成辨識器之後，應該使用 Dispose ( # A1 方法來處置它所使用的資源。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-151">Once done with the recognizer, it should be disposed using Dispose() method to release the resources it uses.</span></span> <span data-ttu-id="ad6b9-152">如果未在這之前釋出這些資源，它會在垃圾收集期間自動釋放這些資源。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-152">It will release these resources automatically during garbage collection at an additional performance cost if they aren't released before that.</span></span>

<span data-ttu-id="ad6b9-153">開始使用聽寫只需要幾個步驟：</span><span class="sxs-lookup"><span data-stu-id="ad6b9-153">There are only a few steps needed to get started with dictation:</span></span>
1. <span data-ttu-id="ad6b9-154">建立新的 DictationRecognizer</span><span class="sxs-lookup"><span data-stu-id="ad6b9-154">Create a new DictationRecognizer</span></span>
2. <span data-ttu-id="ad6b9-155">處理聽寫事件</span><span class="sxs-lookup"><span data-stu-id="ad6b9-155">Handle Dictation events</span></span>
3. <span data-ttu-id="ad6b9-156">啟動 DictationRecognizer</span><span class="sxs-lookup"><span data-stu-id="ad6b9-156">Start the DictationRecognizer</span></span>

### <a name="enabling-the-capability-for-dictation"></a><span data-ttu-id="ad6b9-157">啟用聽寫功能</span><span class="sxs-lookup"><span data-stu-id="ad6b9-157">Enabling the capability for dictation</span></span>

<span data-ttu-id="ad6b9-158">您必須為應用程式宣告「網際網路用戶端」功能（以及上面所述的「麥克風」功能），以利用聽寫。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-158">The "Internet Client" capability, along with the "Microphone" capability mentioned above, must be declared for an app to leverage dictation.</span></span>
1. <span data-ttu-id="ad6b9-159">在 Unity 編輯器中，流覽至 [> Player 編輯 > 專案設定] 頁面，移至播放機設定</span><span class="sxs-lookup"><span data-stu-id="ad6b9-159">In the Unity Editor, go to the player settings by navigating to "Edit > Project Settings > Player" page</span></span>
2. <span data-ttu-id="ad6b9-160">選取 [Windows 存放區] 索引標籤</span><span class="sxs-lookup"><span data-stu-id="ad6b9-160">Select on the "Windows Store" tab</span></span>
3. <span data-ttu-id="ad6b9-161">在 [發佈設定 > 功能] 區段中，檢查 **InternetClient** 功能</span><span class="sxs-lookup"><span data-stu-id="ad6b9-161">In the "Publishing Settings > Capabilities" section, check the **InternetClient** capability</span></span>

### <a name="dictationrecognizer"></a><span data-ttu-id="ad6b9-162">DictationRecognizer</span><span class="sxs-lookup"><span data-stu-id="ad6b9-162">DictationRecognizer</span></span>

<span data-ttu-id="ad6b9-163">建立 DictationRecognizer，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ad6b9-163">Create a DictationRecognizer like so:</span></span>

```
dictationRecognizer = new DictationRecognizer();
```

<span data-ttu-id="ad6b9-164">有四個聽寫事件可供訂閱和處理，以實行聽寫行為。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-164">There are four dictation events that can be subscribed to and handled to implement dictation behavior.</span></span>
1. <span data-ttu-id="ad6b9-165">DictationResult</span><span class="sxs-lookup"><span data-stu-id="ad6b9-165">DictationResult</span></span>
2. <span data-ttu-id="ad6b9-166">DictationComplete</span><span class="sxs-lookup"><span data-stu-id="ad6b9-166">DictationComplete</span></span>
3. <span data-ttu-id="ad6b9-167">DictationHypothesis</span><span class="sxs-lookup"><span data-stu-id="ad6b9-167">DictationHypothesis</span></span>
4. <span data-ttu-id="ad6b9-168">DictationError</span><span class="sxs-lookup"><span data-stu-id="ad6b9-168">DictationError</span></span>

<span data-ttu-id="ad6b9-169">**DictationResult**</span><span class="sxs-lookup"><span data-stu-id="ad6b9-169">**DictationResult**</span></span>

<span data-ttu-id="ad6b9-170">此事件會在使用者暫停（通常是在句子結尾）時引發。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-170">This event is fired after the user pauses, typically at the end of a sentence.</span></span> <span data-ttu-id="ad6b9-171">在這裡會傳回完整的可辨識字串。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-171">The full recognized string is returned here.</span></span>

<span data-ttu-id="ad6b9-172">首先，訂閱 DictationResult 事件：</span><span class="sxs-lookup"><span data-stu-id="ad6b9-172">First, subscribe to the DictationResult event:</span></span>

```
dictationRecognizer.DictationResult += DictationRecognizer_DictationResult;
```

<span data-ttu-id="ad6b9-173">然後處理 DictationResult 回呼：</span><span class="sxs-lookup"><span data-stu-id="ad6b9-173">Then handle the DictationResult callback:</span></span>

```
private void DictationRecognizer_DictationResult(string text, ConfidenceLevel confidence)
{
    // do something
}
```

<span data-ttu-id="ad6b9-174">**DictationHypothesis**</span><span class="sxs-lookup"><span data-stu-id="ad6b9-174">**DictationHypothesis**</span></span>

<span data-ttu-id="ad6b9-175">此事件會在使用者進行交談時持續引發。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-175">This event is fired continuously while the user is talking.</span></span> <span data-ttu-id="ad6b9-176">當辨識器接聽時，它會提供到目前為止所聽過的文字。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-176">As the recognizer listens, it provides text of what it's heard so far.</span></span>

<span data-ttu-id="ad6b9-177">首先，訂閱 DictationHypothesis 事件：</span><span class="sxs-lookup"><span data-stu-id="ad6b9-177">First, subscribe to the DictationHypothesis event:</span></span>

```
dictationRecognizer.DictationHypothesis += DictationRecognizer_DictationHypothesis;
```

<span data-ttu-id="ad6b9-178">然後處理 DictationHypothesis 回呼：</span><span class="sxs-lookup"><span data-stu-id="ad6b9-178">Then handle the DictationHypothesis callback:</span></span>

```
private void DictationRecognizer_DictationHypothesis(string text)
{
    // do something
}
```

<span data-ttu-id="ad6b9-179">**DictationComplete**</span><span class="sxs-lookup"><span data-stu-id="ad6b9-179">**DictationComplete**</span></span>

<span data-ttu-id="ad6b9-180">此事件會在辨識器停止時引發，不論是從停止 ( # A1 呼叫、發生超時或其他錯誤。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-180">This event is fired when the recognizer stops, whether from Stop() being called, a timeout occurring, or some other error.</span></span>

<span data-ttu-id="ad6b9-181">首先，訂閱 DictationComplete 事件：</span><span class="sxs-lookup"><span data-stu-id="ad6b9-181">First, subscribe to the DictationComplete event:</span></span>

```
dictationRecognizer.DictationComplete += DictationRecognizer_DictationComplete;
```

<span data-ttu-id="ad6b9-182">然後處理 DictationComplete 回呼：</span><span class="sxs-lookup"><span data-stu-id="ad6b9-182">Then handle the DictationComplete callback:</span></span>

```
private void DictationRecognizer_DictationComplete(DictationCompletionCause cause)
{
   // do something
}
```

<span data-ttu-id="ad6b9-183">**DictationError**</span><span class="sxs-lookup"><span data-stu-id="ad6b9-183">**DictationError**</span></span>

<span data-ttu-id="ad6b9-184">發生錯誤時，就會引發此事件。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-184">This event is fired when an error occurs.</span></span>

<span data-ttu-id="ad6b9-185">首先，訂閱 DictationError 事件：</span><span class="sxs-lookup"><span data-stu-id="ad6b9-185">First, subscribe to the DictationError event:</span></span>

```
dictationRecognizer.DictationError += DictationRecognizer_DictationError;
```

<span data-ttu-id="ad6b9-186">然後處理 DictationError 回呼：</span><span class="sxs-lookup"><span data-stu-id="ad6b9-186">Then handle the DictationError callback:</span></span>

```
private void DictationRecognizer_DictationError(string error, int hresult)
{
    // do something
}
```

<span data-ttu-id="ad6b9-187">當您訂閱並處理您所關心的聽寫事件之後，請啟動聽寫辨識器以開始接收事件。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-187">Once you've subscribed and handled the dictation events that you care about, start the dictation recognizer to begin receiving events.</span></span>

```
dictationRecognizer.Start();
```

<span data-ttu-id="ad6b9-188">如果您不想再保留 DictationRecognizer，則需要取消訂閱事件並處置 DictationRecognizer。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-188">If you no longer want to keep the DictationRecognizer around, you need to unsubscribe from the events and Dispose the DictationRecognizer.</span></span>

```
dictationRecognizer.DictationResult -= DictationRecognizer_DictationResult;
dictationRecognizer.DictationComplete -= DictationRecognizer_DictationComplete ;
dictationRecognizer.DictationHypothesis -= DictationRecognizer_DictationHypothesis ;
dictationRecognizer.DictationError -= DictationRecognizer_DictationError ;
dictationRecognizer.Dispose();
```

<span data-ttu-id="ad6b9-189">**提示**</span><span class="sxs-lookup"><span data-stu-id="ad6b9-189">**Tips**</span></span>
* <span data-ttu-id="ad6b9-190">開始 ( # A1 並停止 ( # A3 方法，分別啟用和停用聽寫辨識。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-190">Start() and Stop() methods respectively enable and disable dictation recognition.</span></span>
* <span data-ttu-id="ad6b9-191">完成辨識器之後，必須使用 Dispose ( # A1 方法處置，以釋放其使用的資源。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-191">Once done with the recognizer, it must be disposed using Dispose() method to release the resources it uses.</span></span> <span data-ttu-id="ad6b9-192">如果未在這之前釋出這些資源，它會在垃圾收集期間自動釋放這些資源。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-192">It will release these resources automatically during garbage collection at an additional performance cost if they aren't released before that.</span></span>
* <span data-ttu-id="ad6b9-193">在設定的一段時間後發生超時。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-193">Timeouts occur after a set period of time.</span></span> <span data-ttu-id="ad6b9-194">您可以檢查 DictationComplete 事件中的這些超時。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-194">You can check for these timeouts in the DictationComplete event.</span></span> <span data-ttu-id="ad6b9-195">有兩個需要注意的超時：</span><span class="sxs-lookup"><span data-stu-id="ad6b9-195">There are two timeouts to be aware of:</span></span>
   1. <span data-ttu-id="ad6b9-196">如果辨識器啟動，但在前五秒沒有聽到任何音訊，則會超時。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-196">If the recognizer starts and doesn't hear any audio for the first five seconds, it will time out.</span></span>
   2. <span data-ttu-id="ad6b9-197">如果辨識器已指定結果，但接著會聽到20秒的無回應，則會超時。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-197">If the recognizer has given a result, but then hears silence for 20 seconds, it will time out.</span></span>

## <a name="using-both-phrase-recognition-and-dictation"></a><span data-ttu-id="ad6b9-198">使用片語辨識和聽寫</span><span class="sxs-lookup"><span data-stu-id="ad6b9-198">Using both Phrase Recognition and Dictation</span></span>

<span data-ttu-id="ad6b9-199">如果您想要在您的應用程式中同時使用片語辨識和聽寫，您必須完全關閉，才能啟動另一個。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-199">If you want to use both phrase recognition and dictation in your app, you'll need to fully shut one down before you can start the other.</span></span> <span data-ttu-id="ad6b9-200">如果您有多個 KeywordRecognizers 正在執行，您可以將它們一次全部關閉：</span><span class="sxs-lookup"><span data-stu-id="ad6b9-200">If you have multiple KeywordRecognizers running, you can shut them all down at once with:</span></span>

```
PhraseRecognitionSystem.Shutdown();
```

<span data-ttu-id="ad6b9-201">為了將所有辨識器還原至先前的狀態，在 DictationRecognizer 停止之後，您可以呼叫：</span><span class="sxs-lookup"><span data-stu-id="ad6b9-201">In order to restore all recognizers to their previous state, after the DictationRecognizer has stopped, you can call:</span></span>

```
PhraseRecognitionSystem.Restart();
```

<span data-ttu-id="ad6b9-202">您也可以直接啟動 KeywordRecognizer，這也會重新開機 PhraseRecognitionSystem。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-202">You could also just start a KeywordRecognizer, which will restart the PhraseRecognitionSystem as well.</span></span>

## <a name="using-the-microphone-helper"></a><span data-ttu-id="ad6b9-203">使用麥克風協助程式</span><span class="sxs-lookup"><span data-stu-id="ad6b9-203">Using the microphone helper</span></span>

<span data-ttu-id="ad6b9-204">GitHub 上的混合現實工具組包含麥克風協助程式類別，可在系統上有可用的麥克風時提示開發人員。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-204">The Mixed Reality Toolkit on GitHub contains a microphone helper class to hint at developers if there's a usable microphone on the system.</span></span> <span data-ttu-id="ad6b9-205">其中一個用途是要檢查系統上是否有麥克風，然後才在應用程式中顯示任何語音互動提示。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-205">One use for it's where one would want to check if there's microphone on system before showing any speech interaction hints in the application.</span></span>

<span data-ttu-id="ad6b9-206">您可以在 [ [輸入/腳本/公用程式] 資料夾](https://github.com/Microsoft/MixedRealityToolkit-Unity/blob/htk_release/Assets/HoloToolkit/Input/Scripts/Utilities/MicrophoneHelper.cs)中找到麥克風協助程式腳本。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-206">The microphone helper script can be found in the [Input/Scripts/Utilities folder](https://github.com/Microsoft/MixedRealityToolkit-Unity/blob/htk_release/Assets/HoloToolkit/Input/Scripts/Utilities/MicrophoneHelper.cs).</span></span> <span data-ttu-id="ad6b9-207">GitHub 存放庫也包含示範如何使用 helper 的 [小型範例](https://github.com/Microsoft/MixedRealityToolkit-Unity/blob/htk_release/Assets/HoloToolkit-Examples/Input/Scripts/MicrophoneHelperSample.cs) 。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-207">The GitHub repo also contains a [small sample](https://github.com/Microsoft/MixedRealityToolkit-Unity/blob/htk_release/Assets/HoloToolkit-Examples/Input/Scripts/MicrophoneHelperSample.cs) demonstrating how to use the helper.</span></span>

## <a name="voice-input-in-mixed-reality-toolkit"></a><span data-ttu-id="ad6b9-208">混合現實工具組中的語音輸入</span><span class="sxs-lookup"><span data-stu-id="ad6b9-208">Voice input in Mixed Reality Toolkit</span></span>
<span data-ttu-id="ad6b9-209">您可以在此場景中找到語音輸入的範例。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-209">You can find the examples of the voice input in this scene.</span></span>

- [<span data-ttu-id="ad6b9-210">HoloToolkit-Examples/Input/場景/SpeechInputSource unity</span><span class="sxs-lookup"><span data-stu-id="ad6b9-210">HoloToolkit-Examples/Input/Scenes/SpeechInputSource.unity</span></span>](https://github.com/Microsoft/MixedRealityToolkit-Unity/blob/htk_release/Assets/HoloToolkit-Examples/Input/Scenes/SpeechInputSource.unity)

## <a name="next-development-checkpoint"></a><span data-ttu-id="ad6b9-211">下一個開發檢查點</span><span class="sxs-lookup"><span data-stu-id="ad6b9-211">Next Development Checkpoint</span></span>

<span data-ttu-id="ad6b9-212">如果您要遵循我們所配置的 Unity 開發檢查點旅程，您接下來要探索混合現實平臺功能和 Api：</span><span class="sxs-lookup"><span data-stu-id="ad6b9-212">If you're following the Unity development checkpoint journey we've laid out, you're next task is exploring the Mixed Reality platform capabilities and APIs:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ad6b9-213">共用體驗</span><span class="sxs-lookup"><span data-stu-id="ad6b9-213">Shared experiences</span></span>](shared-experiences-in-unity.md)

<span data-ttu-id="ad6b9-214">您可以隨時回到 [Unity 開發檢查點](unity-development-overview.md#2-core-building-blocks)。</span><span class="sxs-lookup"><span data-stu-id="ad6b9-214">You can always go back to the [Unity development checkpoints](unity-development-overview.md#2-core-building-blocks) at any time.</span></span>