起點以及其他知名小說站都推薦用這個腳本[【小說】下載腳本](https://github.com/dodying/UserJs/tree/master/novel/novelDownloader)，遇到沒人願意適配的不知名小站再考慮我的腳本。

輕量級抓取腳本，用於下載網頁小説或其他文字內容，理論上通用於任何靜態寫入正文的小說網站、論壇、貼吧等而無需規則。

腳本會自動檢索頁面中的主要內容並下載（省得複製完gal攻略還要手動逐條刪除「某某某13級頭銜水龍王發表於X年X月X日來自XX客戶端」）。
如果位於小說目錄頁會遍歷所有章節並排序拼接後存為TXT文檔。

[![img](https://img.shields.io/github/stars/hoothin/UserScripts?style=social)](https://github.com/hoothin/UserScripts) [【高亮或者格式化網頁中選中的代碼，並統計字數】](https://greasyfork.org/scripts/24150-highlight-every-code)

---

# 操作說明
+ 打開小說目錄頁、論壇或貼吧內容頁
+ 按下 `CTRL+F9` 或點擊命令菜單
+ 按下 `SHIFT+CTRL+F9` 忽略目錄，僅下載當前頁


*對你有幫助的話，請透過[PayPal管道](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=rixixi@sina.com&item_name=Greasy+Fork+donation)请我喝一杯奶茶*

<img src='https://s2.loli.net/2022/01/06/lEqKWLHG7UBO6AY.jpg' alt="donation" width=420>

#怠惰心法
此功共有七層，以第一層最易，第七層最難。
### 第一層心法

 **CTRL+F9** 就完事了唄。
### 第二層心法

 假若章節連結沒有 xx 章、xx 節、xx 話之類的特徵字樣，可點擊**自定義下載**，輸入隨便一個章節名，例如 「眾神的風車」，即可標記所有同級連結為目錄章節並下載。假如頁面有兩套章節格式，可標記多個，例如「眾神的風車,風車的眾神」。亦可標記排除項，例如「眾神的風車01!02!03,風車的眾神!鐵幕」，代表標記「眾神的風車01」同級連結並排除含有 02 的項和含有 03 的項，同時標記「風車的眾神」同級連結並排除含有“鐵幕”的項。
### 第三層心法

 如果內頁沒有正文，但章節連結與真實內容連結有關聯，可通過**自定義下載**，替換連結內容獲取真實內容。例如 【`眾神的風車@@articles@@articlescontent`】，即可替換章節URL中的 article 為 articlescontent 。
### 第四層心法

 如果連結無法由直接替換得到最終地址，可用正則替換，例如【`眾神的風車@@articles(\d+)@@articlescontent_$1b`】，即可替換章節 URL 中的 articles1、articles2 為
 `articlescontent_1b、articlescontent_2b`
### 第五層心法

 輸入章節的 css 選擇器可以更精確地標記章節連結。例如`.l_chaptname>a`，代表 class 為 l_chaptname 的元素下的a連結。
 下載內容可能含有干擾碼，此時只需點擊**懶人小說下載設置**，輸入干擾碼的 css 選擇器即可排除干擾碼。例如 `.mask,.ksam`，代表刪除 class 為 mask 或者 ksam 的元素。
### 第六層心法

 假若正文不在內頁正文，是頁面加載後處理得到的，可點擊**自定義下載**，輸入自定義代碼對內頁進行分析獲取正確結果。例如 【`眾神的風車@@@@@@var noval=JSON.parse(data.querySelector("#meta-preload-data").content).novel;noval[Object.keys(noval)[0]].content;`】，即可忽略正文，只通過自定義代碼處理返回頁面獲取內容。代碼中使用 data 可以獲得返回頁面的 document ，最後一個表達式的值為最終寫入的內容。
### 第七層心法

 假若正文已經經過加密，需要解密才能獲取正確內容，可打開瀏覽器的控制台，自定義 dacProcess 函數，調取頁面中網站自身的解密代碼處理抓取的加密數據。例如控制台輸入`dacProcess=data=>{return decrypt(xxx);}` 代表調用網站的 decrypt 解密章節頁面返回的數據。然後再點擊**自定義下載**，需要注意自定義下載時標記章節是必需的。
###關於配置項
 **【以下功能需要通過油猴命令菜單進入】**

![img](https://greasyfork.s3.us-east-2.amazonaws.com/grg0pe1t13eth8t012bd1absp9id)
 - 自定義目錄：如https://xxx.xxx/book-**[20-99]**.html,https://xxx.xxx/book-**[01-10]**.html，意為下載book-20.html到book-99.html，以及book-01.html到book-10.html，使用**[1-10]**則不補0。
 - 章節選擇器自定義：輸入章節連結的css選擇器即可，後面可以接上url替換碼、以及自定義處理代碼。
 - 干擾碼：填入干擾碼的css選擇器，如`.mask,.ksam`，意為刪除class為mask或者ksam的元素。
 - 按標題名重新排序：是則把目錄頁所有連結按標題名排序後存入txt，否則按頁面位置順序排列。
 - 下載線程數：同時下載的線程數，默認為20，遇到存在限制的站點可調低。

### 自定義例子
 1. [***po18***](https://www.po18.tw/books/755779/articles)，章節的選擇器為 `.l_chaptname>a` ，輸入並下載後發現通過 url 無法下載正文內容，正文是 ajax 通過 articlescontent 下載的。此時可後接 `@@articles@@articlescontent` (@@ 分隔) 將章節 url 中的 articles 替換為 articlescontent 。 綜上 **`.l_chaptname>a@@articles@@articlescontent`** 即可適配該站。其中第一個 articles 可使用正則，例如 `@@articles(\d+)@@$1content` 代表將連結中的「articles1」「articles2」等替換為「1content」「2content」。
 2. [***pixiv***](https://www.pixiv.net/novel/series/7807554)，p站小說的章節選擇器為`main>section ul>li>div>a`，無需替換連結，因此後兩項留空。有6個@了 😂。正文在meta里，需要自定義代碼提取meta-preload數據的content項。綜上 **`main>section ul>li>div>a@@@@@@var noval=JSON.parse(data.querySelector("#meta-preload-data").content).novel;noval[Object.keys(noval)[0]].content;`** 即可下載p站小說。其中「data」代表抓取網頁的document對象，若返回的是純文本，則用 `data.body.innerText` 獲取。

### 測試網頁
+ http://www.gulongbbs.com/zhentan/bdlr/plje/Index.html
+ http://www.jhshe.cn/thread-1837-1-1.html
+ http://tieba.baidu.com/p/4871634479

### 為啥要寫這個腳本？
主要是<img src="https://stickershop.line-scdn.net/stickershop/v1/product/8692/LINEStorePC/main.png;compress=true" width=50 alt="怠惰啊" title="怠惰啊"/>
