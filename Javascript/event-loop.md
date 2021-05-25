## event-loop & call-stack
`javascript*` `MFEE16*`

本篇為Javascript event-loop(事件迴圈) 的學習筆記

![](https://i.imgur.com/cHbZ8hK.png)

Javascript是一種single threaded language(單執行續語言)，單執行續意味著這種程式語言一次只能處理一件事。
然而Javascript卻能在向資料庫發出請求或是等待setTimeout延遲的同時接著執行後續的工作而不是停在原地等待這些程指令運行完畢。
單執行續的Javascript為什麼能做到非同步執行，首先必須了解Javascript程式碼時是如何運作的。

Javascript的資料結構為call stack(呼叫堆疊)。
每當Javascript執行一個函式，該函式就會被放置於stack的上方，而每當函式結束他就會從stack上方被移出。
這種資料結構的特色FILO(first in last out)，讓Javascript在執行指令時得以記錄他們的先後順序以便程式正確運行。

Javascript在執行後首先會將全部的程式碼打包起來放到stack上，接著由上至下執行程式碼。
執行Javascript程式時除了Javascript本身的call stack會處理指令以外，瀏覽器也提供了webAPI幫他處理
諸如setTimeout及ajax等指令。
由於Javascript接收到這些指令就會丟給webAPI處理
這樣的機制允許了Javascript像多執行續語言一般同時處理多個程式。

由webAPI處理完的程式會被傳送到callback queue暫存，並在




event-loop是Javascript管理回呼函式的機制，這個機制讓Javascript向瀏覽器提出請求後繼續執行其他指令。

Javascript的memory主要分為stack和heap兩部份。
stack用來存放string, number, boolean等原始型別(Primitive values)及references，heap則用來存放object及function。

參考資料:
- [Philip Roberts: What the heck is the event loop anyway?](https://www.youtube.com/watch?v=8aGhZQkoFbQ)
- [[第十六週] JavaScript 進階：事件迴圈 Event Loop、Stack、Queue](https://yakimhsu.com/project/project_w16_EventLoop.html)
- [[java] Memory 管理](https://lyenliang.github.io/2017/03/25/java-stack-and-heap/)

延伸閱讀:
- [JavaScript Event Loop And Call Stack Explained](https://felixgerschau.com/javascript-event-loop-call-stack/)
- [JavaScript's Memory Management Explained](https://felixgerschau.com/javascript-memory-management/#the-memory-heap-and-stack)
- [了解瀏覽器的棧內存 (Stack) ＆ 堆內存 (Heap)](https://ideascoffee.medium.com/%E6%99%AE%E9%80%9A%E9%A1%9E%E5%9E%8B%E5%92%8C%E5%B0%8D%E8%B1%A1%E7%9A%84%E5%8D%80%E5%88%A5-%E6%A3%A7%E5%85%A7%E5%AD%98-stack-%E5%A0%86%E5%85%A7%E5%AD%98-heap-44295724848c)