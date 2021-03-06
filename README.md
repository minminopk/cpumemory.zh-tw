# 每位程式設計師都該知道的記憶體知識

本篇翻譯自 Ulrich Drepper 於 2007 年所寫的論文[《What Every Programmer Should Know About Memory》](https://people.freebsd.org/~lstewart/articles/cpumemory.pdf)。原文 PDF 長達 114 頁。

知道這篇論文已經很久了，但先前幾次總是讀沒幾頁就丟著去做別的事。一直沒機會讀個大概，總覺得是一件很可惜的事情。藉著這次一時興起，希望可以一邊翻譯、一邊多點動力試著把這篇論文讀完。

翻譯的順序大致上會與我讀的順序相同，所以不會是依序從第一節翻譯到最後一節，而且極有可能只會翻譯我有興趣（其實是耐心）看下去的部分。當然，如果願意和我一起邊讀論文邊翻譯也是很歡迎的。

~~基於「有借有還，再借不難」的原則，~~學生時代學到的相關知識已經通通都還給老師了。因此，也不把握對文章的內容有足夠充分的理解，所以難以對譯文加以補充，甚至可能會有錯譯的問題。如果文章有任何錯誤的地方，還望各位能夠不吝指正。

## 關於專有名詞

決定要將專有名詞翻成中文、或是保留英文原文，對我而言實在是個困難的問題。

部分專有名詞，如「記憶體（memory）」、「快取（cache）」、「匯流排（bus）」等，都已是十分通用的中文翻譯。但亦有如「memory bank」、「cache line」、「access line」等不太會被翻譯的名詞。但「memory」一下翻作「記憶體」、碰上「memory bank」又保留原文似乎也頗為尷尬。

在這篇文章中，我試著將每個詞彙都找個適當的中文翻譯，希望不會令人感到太過突兀。假如有任何更好的作法，也歡迎給我意見。

