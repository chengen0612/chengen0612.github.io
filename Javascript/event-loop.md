## event-loop & call-stack
`javascript*` `MFEE16*`

本篇為Javascript event-loop(事件迴圈) 的學習筆記

本次將以[參考資料[1]](https://www.youtube.com/watch?v=8aGhZQkoFbQ "Philip Roberts: What the heck is the event loop anyway?")的影片為主軸做學習

---
![](https://i.imgur.com/cHbZ8hK.png)
>Javascript是一種**single threaded language(單執行續語言)**，單執行續意味著這種程式語言一次只能處理一件事。
然而Javascript卻能在向資料庫發出請求或是等待setTimeout延遲的同時接著處理後續的工作而不是等待這些程指令運行完畢。
單執行續的Javascript為什麼能做到非同步執行呢?

運行一段Javascript程式碼時主要會牽扯到3個區塊，call stack, Web APIs, callback queue

**call stack**是一種資料結構，中文譯作呼叫堆疊。
每當Javascript執行函式，該函式就會被置於call stack的上方，而每當函式執行完畢便會從stack被移出。
這種資料結構的特色**FILO(first in last out)**，使Javascript在執行指令時得以記錄他們的先後順序以便程式正確運行。

**Web APIs**是瀏覽器提供給Javascript的工具，負責處理ajax, setTimeout等請求。
call stack接收到上述指令時會將該指令轉交給Web APIs處理。
這樣的機制允許Javascript達到多工處理，避免他在處理這類請求時像以往的單執行續語言一樣產生blocking(阻塞)。

**callback queue**則是負責存放Web APIs的執行結果的地方。
Web APIs處理完函示得到的結果會進入callback queue進行佇列。

---
>callback queue本身並不具備函示處理能力，因此他還是得將裡面的東西丟回stack處理。
而存放在queue當中的這些東西要在何時回到stack便是由**event loop(事件迴圈)**來決定。

用戶端在執行Javascript程式時，首先會將全部的程式碼打包成一個main()放到stack上，接著按照順序執行指令。
call stack會將執行過程中接收到的ajax, setTimeout等指令交給Web APIs並持續執行直到main()被清空。

event loop的工作就是要確保callback queue當中的任務待在佇列裡直到main()執行完畢。
一旦event loop判斷main()已經執行結束且stack裡面是空的，他就會一次一個的將queue當中的任務返回到stack執行。

由此得到結論，event loop是一個管理程式執行順序的機制。
他負責控管call stack當中的main()和Web APIs回傳結果的執行順序，
**他為程式的運行建立了一套規則，並提供給我們一個撰寫程式的參考依據。**

---
Philip在影片結尾為我們示範了瀏覽器如何處理頁面刷新這件事，同時示範了event loop如何影響了他。

理想的情況瀏覽器想以每16.6毫秒(每秒60偵)的頻率去刷新頁面為我們提供順暢的用戶體驗。
瀏覽器的刷新動作在**render queue**當中執行，此佇列如同call back queue一樣受event loop所管理。

倘若我們撰寫程式時忽略了event loop的存在，寫了一個過於複雜或更新頻率過高的指令。
瀏覽器有機會因call stack阻塞而無法刷新頁面導致畫面停擺，或因queue當中大量的任務而產生不自然的抖動。

---
參考資料:
- [Philip Roberts: What the heck is the event loop anyway?](https://www.youtube.com/watch?v=8aGhZQkoFbQ)
- [[第十六週] JavaScript 進階：事件迴圈 Event Loop、Stack、Queue](https://yakimhsu.com/project/project_w16_EventLoop.html)
- [[java] Memory 管理](https://lyenliang.github.io/2017/03/25/java-stack-and-heap/)

延伸閱讀:
- [JavaScript Event Loop And Call Stack Explained](https://felixgerschau.com/javascript-event-loop-call-stack/)
- [JavaScript's Memory Management Explained](https://felixgerschau.com/javascript-memory-management/#the-memory-heap-and-stack)
- [了解瀏覽器的棧內存 (Stack) ＆ 堆內存 (Heap)](https://ideascoffee.medium.com/%E6%99%AE%E9%80%9A%E9%A1%9E%E5%9E%8B%E5%92%8C%E5%B0%8D%E8%B1%A1%E7%9A%84%E5%8D%80%E5%88%A5-%E6%A3%A7%E5%85%A7%E5%AD%98-stack-%E5%A0%86%E5%85%A7%E5%AD%98-heap-44295724848c)
