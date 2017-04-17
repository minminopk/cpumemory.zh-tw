# 3.1. 概觀 CPU 快取

在深入 CPU 快取的技術細節之前，某些讀者或許會發現，先理解快取是如何融入現代電腦系統的「大局（big picture）」是有所幫助的。

<figure>
  <img src="../assets/figure-3.1.png" alt="圖 3.1：最簡易的快取配置">
  <figcaption>圖 3.1：最簡易的快取配置</figcaption>
</figure>

圖 3.1 顯示了最簡易的快取配置。其與能在早期找到的、採用 CPU 快取的系統架構是一致的。CPU 核心不再直接連結到主記憶體。[^16]所有的載入與儲存都必須經過快取。CPU 核心與快取之間的連線是一條特殊的、快速的連線。在這個簡化的示意圖上，主記憶體與快取都被連結到系統匯流排，其也會用來跟其它系統元件通訊。我們已經以「FSB」介紹過系統匯流排，這是它現今使用的名稱；見 2.2 節。在這一節中，我們會省略北橋；假定它存在，以方便 CPU 與主記憶體的溝通。

即便過去數十年來的大多電腦都採用馮紐曼架構（von Neumann architecture），但實驗證實分離程式碼與資料的快取是比較好的。Intel 自 1993 年起採用分離程式碼與資料的快取，就再也沒有回頭過。程式碼與資料所需的記憶體區域彼此相當獨立，這也是獨立的快取運作得更好的原因。近年來，另一個優點逐漸浮現：對大多數常見的處理器而言，指令解碼（decoding）的步驟是很慢的；快取解碼過的指令能夠讓執行加速，在不正確地預測或者無法預測的分支（branch）使得管線（pipeline）為空的情況下尤其如此。

在引入快取之後不久，系統變得越來越複雜。快取與主記憶體之間的速度差異再次增加，直到加入了另一層級的快取，比起第一層快取來得更大也更慢。僅僅提升第一層快取的大小，以經濟因素來說並非一個可行的辦法。今日，甚至有正常使用、具有三層快取的機器。具有這種處理器的系統看起來就像圖 3.2 那樣。隨著單一 CPU 中的核心數增加，未來快取層級也許會變得更多。

<figure>
  <img src="../assets/figure-3.2.png" alt="圖 3.2：具有三層快取的處理器">
  <figcaption>圖 3.2：具有三層快取的處理器</figcaption>
</figure>

圖 3.2 顯示了三層快取，並引入了我們將會在本文其餘部分使用的術語。L1d 是一階資料快取、L1i 是一階指令快取等等。注意，這只是張示意圖；實際上資料流從核心到主記憶體的路上並不需要通過任何較高層級的快取。CPU 設計者在快取介面的設計上有著很大的自由。對程式設計師來說，是看不到這些設計上的抉擇的。

此外，我們有多核心的處理器，每個核心都能擁有多條「執行緒（thread）」。一個核心與一條執行續的差別在於，不同的核心擁有（幾乎[^17]）所有硬體資源各自的副本。除非同時用到相同的資源––像是對外連線，否則核心是能夠完全獨立運作的。另一方面，執行緒則共享幾乎所有處理器的資源。Intel 的執行緒實作只讓其擁有個別的暫存器，甚至還是有限的––某些暫存器是共享的。所以，現代 CPU 的完整架構看起來就像圖 3.3。

<figure>
  <img src="../assets/figure-3.3.png" alt="圖 3.3：多處理器、多核心、多執行緒">
  <figcaption>圖 3.3：多處理器、多核心、多執行緒</figcaption>
</figure>

在這張圖中，我們有兩顆處理器，每顆兩個核心，各自擁有兩個執行緒。執行緒共享一階快取。核心（以深灰色為底）擁有獨立的一階快取。所有 CPU 的核心共享更高層級的快取。兩顆處理器（兩個淺灰色為底的大方塊）自然不會共享任何快取。這些全都很重要，在我們討論快取對多行程與多執行緒應用程式的影響時尤為如此。

[^16]: 在更早期的系統中，快取是如同 CPU 與主記憶體一樣接到系統匯流排上的。這比起實際的解法，更像是一種臨時解（hack）。

[^17]: 早期的多核心處理器甚至有分離的 2<sup>nd</sup> 階快取，並且沒有 3<sup>rd</sup> 階快取。
