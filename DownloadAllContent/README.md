## [English Readme]
Qidian and others recommend using this script [[Novel] Download Script](https://github.com/dodying/UserJs/tree/master/novel/novelDownloader). If you encounter a small site that no one is willing to adapt to, consider mine. script.

Do not use it on copyrighted sites. If it causes infringement or damage to the target site, you will be responsible for the consequences.

A lightweight crawling script used to download web novels or other text content. In theory, it can be used on any novel website, forum, post bar, etc. that statically writes text without rules.

The script will automatically retrieve the main content in the page and download it (<del>You don’t have to manually delete the gal guide one by one after copying "So-and-so level 13 title Water Dragon King was published on X, month, X, year, from XX client"</ del>).
If you are on the novel table of contents page, all chapters will be traversed, sorted and spliced, and then saved as a TXT document.

[![img](https://img.shields.io/github/stars/hoothin/UserScripts?style=social)](https://github.com/hoothin/UserScripts)

---

# Operation instructions
+ Open the novel catalog page, forum or Tieba content page
+ Press `CTRL+F9` or click on the command menu
+ Press `SHIFT+CTRL+F9` to ignore the directory and download only the current page

If you encounter a site with download errors, you can feel free to submit an issue to [Github](https://github.com/hoothin/UserScripts/issues)

*If it is helpful to you, you can use [![i](https://static.afdiancdn.com/favicon.ico) Love Power](https://afdian.net/a/hoothin) or [![i ](https://ko-fi.com/favicon-32x32.png) Ko-fi](https://ko-fi.com/hoothin) Please buy me a cup of milk tea. Welcome to the [💬Discord group](https://discord.com/invite/keqypXC6wD). *

![donate](https://s2.loli.net/2023/02/06/afTMxeASm48z5vE.jpg)

[Greasy Novel Downloader ZIP Extension](https://greasyfork.org/scripts/476943) When downloading, save TXT in chapters and package them as ZIP

## Lazy mentality
<del>It’s called laziness, but it’s actually diligence</del>
There are seven levels of this skill, with the first level being the easiest and the seventh level being the most difficult.
### The first level of mental skills (very easy)

  **CTRL+F9** That’s it.
### The second level of mental skills (super easy)

  If the chapter link does not have characteristic words such as xx chapter, xx section, xx words, etc., you can click **Custom Download** and enter any chapter name, such as "Windmill of the Gods", to mark all sibling links as directories. Chapter and download. If the page has two sets of chapter formats, you can also mark multiple ones, such as "Windmills of the Gods, Windmills of the Gods". You can also mark excluded items, such as "Windmills of the Gods 01! 02! 03, Windmills of the Gods! Iron Curtain", which means marking "Windmills of the Gods 01" sibling links and excluding items containing 02 and items containing 03. Also mark "Gods of Windmills" sibling links and exclude items containing "Iron Curtain".
### The third level of mental method (slightly easier)

  If there is no text on the inner page, but the chapter link is related to the real content link, you can use **custom download** to replace the link content to obtain the real content. For example, [`Windmill of the Gods@@articles@@articlescontent`] can replace articles in the chapter URL with articlescontent and automatically obtain the content.
### The fourth level of mental method (slightly difficult)

  If the final address of the link cannot be obtained by direct replacement, regular replacement can be used, such as [`Windmill of the Gods@@articles(\d+)@@articlescontent_$1b`], which can replace articles1 and articles2 in the chapter URL with
  `articlescontent_1b, articlescontent_2b`
### The Fifth Level of Mental Technique (Difficult)

  Input chapter css selectors for more precise marking of chapter links. For example, `.l_chaptname>a` represents the a link under the element with class l_chaptname.
  The downloaded content may contain interference codes. In this case, just click **Lazy Novel Download Settings** and enter the css selector of the interference codes to eliminate the interference codes. For example, `.mask,.ksam,font.jammer` means to delete elements with class mask or ksam or font elements with class jammer.
### The Sixth Level of Mental Technique (Super Difficult)

  If the text is not in the text of the inner page and is processed after the page is loaded, you can click **Custom Download** and enter the custom code to analyze the inner page to obtain the correct results. For example [`Windmill of the Gods@@@@@@var noval=JSON.parse(doc.querySelector("#meta-preload-data").content).novel;noval[Object.keys(noval)[0] ].content;`], you can return the page to obtain the content through custom code processing. Use doc in the code to get the document of the returned page, and the value of the last expression is the final written content.
 
  If false is returned, it represents an asynchronous callback. You can grab the content yourself and wait for the fetch to be successful before using cb(content) to return the fetched content.

  If there is no link in the chapter, the link jump will be generated after clicking. You can use the `>>` pipeline to process the captured elements to generate the chapter link. For details, see the example below.
### The seventh level of mental skills (extremely difficult)

  If the text has been encrypted and needs to be decrypted to obtain the correct content, you can open the browser console, customize the dacProcess function, and call the website's own decryption code in the page to process the captured encrypted data. For example, the console input `dacProcess=data=>{return decrypt(xxx);}` represents the data returned by calling the decrypt decryption chapter page of the website. Then click **Custom Download**. Please note that marking chapters is required when custom downloading.

### About configuration items
  **[The following functions need to be entered through the command menu of managers such as Tampermonkey]**

![img](https://greasyfork.s3.us-east-2.amazonaws.com/grg0pe1t13eth8t012bd1absp9id)
  - Custom directory: such as `https://xxx.xxx/book-**[20-99]**.html, https://xxx.xxx/book-**[01-10]**.html `, which means downloading book-20.html to book-99.html, and book-01.html to book-10.html. If **[1-10]** is used, 0 will not be added.
  - Chapter selector customization: Just enter the css selector of the chapter link, followed by the url replacement code and custom processing code.
  - Interference code: Fill in the css selector of the interference code, such as `.mask,.ksam`, which means to delete elements whose class is mask or ksam.
  - Reorder by title name: If so, all links on the directory page will be sorted by title name and then stored in txt, otherwise they will be sorted by page position.
  - Number of downloading threads: The number of downloading threads at the same time. The default is 20. It can be lowered when encountering sites with restrictions (for example, there are always chapters that fail to be obtained during downloading).

### Complete format description
<code>A certain chapter name/CSS selector [The selector can be followed by >> passing in the item for processing] **@@** Grab the regular match of the URL **@@** Regular replacement of the URL **@@ ** Process and return the final text based on the crawled returned content data</code>
#### Internal page processing example
Assume the chapter element is `a.links`
+ Use iframe to process internal page content
   `a.links@@@@@iframe:`
  - add sandbox to iframe
    `a.links@@@@@@iframe:sandbox:{allow-same-origin}`
  - Add initialization code to iframe
    `a.links@@@@@@iframe:init:{win.top=win.self}`
+ Customize the pagination crawling method for internal pages
   - Fetch via selector
     `a.links@@@@@@next:{a.next}`
   - Transparent code generation
     `a.links@@@@@@next:{return await getNextElement()}` You can use multiple layers of `{}` to avoid problems caused by braces in the code

<a id="example"></a>
### Custom download example, open the directory page and click [Custom Download] to paste and use. It is only a guide for rule instances. If there are any discrepancies, please modify it yourself.
+ [📕po18](https://www.po18.tw/books/755779/articles)
> The selector of the chapter is `.l_chaptname>a`. After inputting and downloading, I found that the main content cannot be downloaded through the url. The main text is downloaded through ajax through articlescontent. At this time, it can be followed by `@@articles@@articlescontent` (@@ separated) to replace articles in the chapter url with articlescontent . `.l_chaptname>a@@articles@@articlescontent` Paste into the command menu to download. The first articles can use regular rules. For example, `@@articles(\d+)@@$1content` means replacing "articles1" and "articles2" in the link with "1content" and "2content".
  ``` css
.l_chaptname>a @@ articles @@ articlescontent
  ```
  > If you need to download purchased VIP chapters, use this rule
  ``` javascript
  a.btn_L_blue>>let a=document.createElement("a");a.innerText=item.parentNode.parentNode.querySelector('.l_chaptname').innerText;a.href=item.href;return a;@@ articles@@articlescontent
  ```
+ [📕pixiv](https://www.pixiv.net/novel/series/7807554)
> The chapter selector of the p-site novel is `main>section ul>li div>a`, and there is no need to replace the link, so the last two items are left blank. There are 6 @s 😂. The main text is in meta, and you need to customize the code to extract the content item of meta-preload data. Among them, "doc" represents the document object of the crawled web page. If the returned text is plain text, use `doc.body.innerText` to obtain it.
  ``` javascript
main>section ul>li div>a @@@@@@ var noval=JSON.parse(doc.querySelector("#meta-preload-data").content).novel;noval[Object.keys(noval)[ 0]].content;
  ```
+ [📕Hongshu Chinese website](https://g.hongshu.com/chapterlist/91735.do)
> This site does not have a directory link. At this time, you can traverse the tags to create a directory link to download.
  ``` javascript
ul#lists>li>>let href=item.getAttribute("onclick").replace(/.*(http.*html).*/,"$1"),innerText=item.querySelector("span"). innerText;return {href:href,innerText:innerText};@@@@@@let rdtext=data.querySelector('div.rdtext');let sc=data.querySelector('div.ewm+script');if(sc&&rdtext){let code=sc.innerText.replace(/for\(var i=0x0;i<words.*/,"window.words=words;");eval(code);[].forEach.call(rdtext.querySelectorAll('span[class]'),span=>{let id=span.className.replace(/[^\d]/ig,"");span.innerText=words[id]}),rdtext.innerText};if(sc&&rdtext){let code=sc.innerText.replace(/for\(var i=0x0;i<words.*/,"window.words=words;");eval(code);[].forEach .call(rdtext.querySelectorAll('span[class]'),span=>{let id=span.className.replace(/[^\d]/ig,"");span.innerText=words[id]} ),rdtext.innerText};
  ```
+ [📕yuyan](https://yuyan.pw/)
  ``` css
https://yuyan.pw/novel/xxx/[xxxxxxx-xxxxxxx].html@@@@@@var c=data.querySelector('body>script:nth-of-type(8)').innerHTML. match(/var chapter =(.*?);\\n/)[1];eval(c).replaceAll("<br />","");
  ```
+ [📕Cuiweiju](https://www.cuiwei.org/book/28975/yijiequanyuledashi_mulu.html)
  ``` javascript
  .chapter-table>a@@@@@@fetch(data.querySelector("div.box-border>script").innerHTML.match(/\/chapter\/(.*?)"/)[0] ) .then(response => response.text()) .then(d => {eval("window.txtObj="+d.match(/_txt_call\((.*)\);/)[1]) ;for(k in txtObj.replace){txtObj.content=txtObj.content.replaceAll(txtObj.replace[k],k)}cb(unescape(txtObj.content.replace(/&#x(.*?); /g,'%u$1')));});return false;
  ```
+ [📕XXX](https://www.XXX.com/xen/market/remix/paid_column/1465280726219968513)
> There are no links to chapters on this page. Use the following rules to obtain chapter links. Only free visible content can be downloaded. For paid content, please become a member. Please explore the specific operations by yourself and bear the consequences.
  ``` javascript
  [class^=ChapterItem-root]>>let a=document.createElement("a");let pre=`https://${location.host}/market/paid_column/${location.href.replace(/ \D*(\d+)$/,"$1")}/section/`;a.href=pre+JSON.parse(item.dataset.zaExtraModule).card.content.id;a.innerText=item.querySelector ("div").innerText;return a;
  ```
+ [📕FU女屋](http://m.funvwu.com/)
  ``` javascript
  .chapterList>ul>li>a>>let href=item.href.replace(/.*goChapter\((\d+)\)/,"/noval/"+localStorage.booklist+"/$1.html"); item.href=href;return item;
  ```
+ [📕Ruochu Literature Network](https://www.ruochu.com/chapter/146456)
  ``` javascript
  ul.float-list>li>a@@www\\.ruochu\\.com/book/\d+/(\d+)@@a.ruochu.com/ajax/chapter/content/$1@@var content = data.body.innerText.match(/"htmlContent":"(.*)","status"/);if(!content)console.log(data.body.innerText);else{content=content[1] ;content.replace(/\\r/g,'\n')}
  ```
+ [📕明月中文网](https://www.56bok.com/list/27/27742.html)
  ``` javascript
  ul.readlist>li>a>>let href=item.getAttribute("onclick").replace(/.\*open\\('(.\*)','.\*/,"$1"); item.href=href;return item;
  ```
+ [📕Northeastern Novel Network](https://www.dbrxs.org/86/86323/)
> This site has internal paging, so it is necessary to use an asynchronous method. After fetching the content, it will not be returned temporarily. All the paging content will be requested and then spliced together and returned together.
  ``` javascript
  .chapterList li>a>>item.href=item.href.replace(/.*gotochapter\('(\d+)','(\d+)','(\d+)'\).*/," /$1/$2/$3.html");return item;@@@@@@let content=data.querySelector('#contentinfo,#ChapterView>div:nth-child(3)>div');if(! content)return data.body.innerText;content.innerHTML=content.innerHTML.replace(/<br>/g,"\n");content=content.innerText;let pages=data.querySelectorAll(".chapterPages>a :not(.curr)");if(pages){let num=pages.length,cur=0;content=[content];[].forEach.call(pages, (page,i)=>{let url =page.href.replace(/.*\((\d+),(\d+),(\d+),(\d+)\).*/,"/$1/$2/$3_$4.html") ;fetch(url).then(r => r.text()).then(d => {let doc = document.implementation.createHTMLDocument(''); doc.documentElement.innerHTML = d;let c=doc. querySelector('#contentinfo,#ChapterView>div:nth-child(3)>div');if(c){c.innerHTML=c.innerHTML.replace(/<br>/g,"\n"); content[i+1]=c.innerText;if(++cur>=num)cb(content.join("\n"));} }); });return false;}return content;
  ```
+ [📕Chandu Novel Network](https://www.cdxsw.cc/53/53458/)
> This site also has internal pagination. The difference is that its internal pagination needs to be loaded to know whether it exists, so it also returns asynchronously and calls back fetch until all paginations are analyzed.
  ``` javascript
.section-list>li>a@@@@@@let content="";let checkContent=(doc,over)=>{word=doc.querySelector('.word_read');if(!word)content+= '\n'+doc.body.innerText;else [].forEach.call(word.querySelectorAll('p,h3'),c=>content+='\n'+c.innerText);let next=doc. querySelector(".read_btn>a:nth-child(4)");if(next&&/_\d\.html/.test(next.href)){fetch(next.href).then(r => r .text()).then(d => {let _doc = document.implementation.createHTMLDocument('');_doc.documentElement.innerHTML = d;checkContent(_doc,over);});}else over();} ;checkContent(data,()=>{cb(content)});return false;
  ```
+ [📕lofter](https://kuencar.lofter.com/view)
> This site contains miscellaneous blog posts, so you need to manually crawl, filter, sort and download them.
  ``` javascript
body>>let title="Yu Liang/Shi Guang",chs=[];item.querySelectorAll("ul.list>li>a").forEach(a=>{if(a.children[0].innerText. indexOf(title)!=-1)chs.push(a)});return chs.reverse();
  ```
+ [📕Dingdian Novel Network](https://m.biqugeu.net/booklist/20128662.html)
> This site has the same 11 items
  ``` javascript
.book_last>dl>dd>a:not([style])@@@@@@let content="";let checkContent=(doc,over)=>{word=doc.querySelector('#chaptercontent'); if(!word)content+='\n'+doc.body.innerText;else {word.innerHTML=word.innerHTML.replace(/<br>/g,'\n');content+='\n'+ word.innerText;}let next=doc.querySelector("#pb_next");if(next&&/_\d\.html/.test(next.href)){fetch(next.href).then(r => r.arrayBuffer()).then(d => {let decoder = new TextDecoder("gbk");let text = decoder.decode(d);let _doc = document.implementation.createHTMLDocument('');_doc.documentElement .innerHTML = text;checkContent(_doc,over);});}else over();};checkContent(data,()=>{cb(content)});return false;
  ```
+ [📕Otaku Novel Network](http://www.zhainanxs.com/mytool/getChapterList/)
> The directory link of this site is hidden, so it needs to be constructed manually, the same as item 10. However, because the text on this site has been replaced by a placeholder image, someone needs to organize the comparison table, otherwise there will be missing words. If you are setting up
Turn on `Keep the URL of the content image` in the center, and use it with the zip extension to save all the images in the zip. At this time, use an OCR recognition program (such as [ImgCodeCheck](https://github.com/hoothin/ImgCodeCheck/ )), you can get the corresponding text and replace it in batches.
  ``` javascript
#list-chapterAll>dd>a>>item.href=item.href.replace(/.*book\('(\d+)','(\d+)'\).*/,"/go/$1 /$2.html");return item;@@@@@@let content=data.querySelector('h1~div');if(!content)return data.body.innerText;content.innerHTML=content.innerHTML. replace(/<br>/g,"\n");content=content.innerText;let pages=data.querySelectorAll(".chapterPages>a:not(.curr)");if(pages){let num= pages.length,cur=0;content=[content];[].forEach.call(pages, (page,i)=>{let url=page.href.replace(/.*'(\d+)', '([\d_]+)'.*/,"/go/$1/$2.html");fetch(url).then(r => r.text()).then(d => {let doc = document.implementation.createHTMLDocument(''); doc.documentElement.innerHTML = d;let c=doc.querySelector('h1~div');if(c){[].forEach.call(c.querySelectorAll(" img[src]"), img => { let imgTxt=`![img](${location.origin+img.getAttribute("src")})`; let imgTxtNode=document.createTextNode(imgTxt); img. parentNode.replaceChild(imgTxtNode, img); });c.innerHTML=c.innerHTML.replace(/<br>/g,"\n"); content[i+1]=c.innerText;if(++ cur>=num)cb(content.join("\n"));} }); });return false;}return content;
  ```
+ [📕Free Novel Network](http://www.huazhuangsheying.com/book/3659/)
> There is also pagination. Simply process it after fetch and it will be ok.
  ``` javascript
.section-box+h2+.section-box>.section-list>.book-item>a@@@@@@let content=data.querySelector('#content');if(!content)return data.body .innerText;if(content.children[0].tagName=='DIV')content.removeChild(content.children[0]);content.innerHTML=content.innerHTML.replace(/<br>/g,"\ n");content=content.innerText;let nextpage=data.querySelector(a[href$="_2.html"]);if(nextpage){fetch(nextpage.href).then(r => r.text ()).then(d => {let doc = document.implementation.createHTMLDocument(''); doc.documentElement.innerHTML = d;let c=doc.querySelector('#content');if(c){if (c.children[0].tagName=='DIV')c.removeChild(c.children[0]);c.innerHTML=c.innerHTML.replace(/<br>/g,"\n"); content+=c.innerText;}cb(content);});return false;}return content;
  ```
+ [📕Haitang Culture](https://haitbook.com/?act=showinfo&bookwritercode=EB20160922203907769482&bookid=67166&pavilionid=a)
> The token is in the page, get it directly through match and then request it.
  ``` javascript
.uk-list>li>a@@@@@@let contentMatch=data.body.innerHTML.match(/url: '\/showpapercolor.php',[\s\S]*?paperid:\s*' (\w+)',\s*vercodechk:\s*'(\w+)'/);if(!contentMatch)return "";$.ajax({url: '/showpapercolor.php',type: 'POST ',data: { paperid: `${contentMatch[1]}`, vercodechk: `${contentMatch[2]}`},error: function (xhr) {cb("");},success: function (colorresponse ) {cb(colorresponse.replace(/<img.*?>/,"").replace(/<br \/>/g,""))}});return false;
  ```
+ [📕乐文网](https://www.mylewen.com/book/183921/catalog/)
> Divided into two parts, the first half is plain text and the second half is encrypted. Call the page's own method to decrypt and then splice it.
  ``` javascript
.BCsectionTwo-top-chapter>a@@@@@@let content=doc.querySelector("#C0NTENT");let r="\n",ps=content.querySelectorAll("p");for(let i =0;i<ps.length;i++){let p=ps[i];if(p.style.cssText)break;else r+=p.innerText+"\n"};let script=content.nextElementSibling;let other=script.innerText.match(/html\(d\((".*?"), "(.*?)"\)\);/);let a=JSON.parse(other[1]) ,b=other[2];let cryptojs=document.createElement("script");cryptojs.src="/assets/js/cryptojs.min.js";cryptojs.charset="UTF-8";cryptojs.onload =()=>{function d(a, b) { b = CryptoJS.MD5(b).toString(); var d = CryptoJS.enc.Utf8.parse(b.substring(0, 16)); var e = CryptoJS.enc.Utf8.parse(b.substring(16)); return CryptoJS.AES.decrypt(a, e, { iv: d, padding: CryptoJS.pad.Pkcs7 }).toString(CryptoJS.enc.Utf8 ) };cb(r+d(a,b).replace(/<p>/g,"").replace(/<\/p>/g,"\n"));};document.head .appendChild(cryptojs);return false;
  ```
+ [📕Some flap](https://read.some flap.com/column/64079189/)
  ``` css
  a.chapter-item
  ```
> Due to legal issues, no specific rules will be given. Just because a friend asked, I analyzed it and gave relevant ideas for technical research. Please don't ask me for ready-made rules. If there are any changes later, we will not follow up. Please explore the specific operations by yourself and bear the consequences.
> First of all, only part of the content of a certain flap's inner pages is plain text, and the full text is encrypted. Every time it accesses an internal page, it will first search whether the ciphertext exists in the local storage. If it does not exist, it will grab the ciphertext. The ciphertext is encrypted by the sha256 of the digest.
> So the steps are as follows, first call article_v2/get_reader_data, provide the aid of the current chapter through the form (that is, the digital string after chapter), obtain json.data which is the cipher text, and then obtain the main text through the decryption method above. The main text is located in posts[0].contents. After traversing, data.text[0].content is read and spliced.
> The decryption method is as follows:
``` javascript
function decode(t) {
     const s = (new TextDecoder).decode(new Uint8Array([65, 69, 83, 45, 67, 66, 67]))
     , r = (new TextDecoder).decode(new Uint8Array([99, 114, 121, 112, 116, 111]))
     , o = (new TextDecoder).decode(new Uint8Array([115, 117, 98, 116, 108, 101]))
         , a = (new TextDecoder).decode(new Uint8Array([100, 105, 103, 101, 115, 116]))
         , h = (new TextDecoder).decode(new Uint8Array([83, 72, 65, 45, 50, 53, 54]))
         , l = (new TextDecoder).decode(new Uint8Array([105, 109, 112, 111, 114, 116, 75, 101, 121]))
         , c = (new TextDecoder).decode(new Uint8Array([100, 101, 99, 114, 121, 112, 116]))
         , u = (new TextDecoder).decode(new Uint8Array([105, 118]));
     const e = Uint8Array.from(window.atob(t), (t=>t.charCodeAt(0)))
         , i = e.buffer
         , d = e.length - 16 - 13
         , p = new Uint8Array(i,d,16)
         , f = new Uint8Array(i,0,d)
         , g = {};
     return g.name = s,
     g[u] = p,
     function() {
             const t = Ark.user
                 , e = t.isAnonymous ? document.cookie.replace(/.*bid=(\w+).*/,"$1") : t.id
                 , i = (new TextEncoder).encode(e);
             return window[r][o][a](h, i).then((t=>window[r][o][l]("raw", t, s, !0, [c])) )
         }().then((t=>window[r][o][c](g, t, f))).then((t=>JSON.parse((new TextDecoder).decode(t)) ))
}
```

+ [📕爱发电](https://爱电/album/afee5ce2462d11ee897e52540025c377)
> I am also a user who loves to generate electricity. I will not bully it when it is short-handed. Just to give you an idea, use the fourth level method to get the album_id and chapter id and go to /api/post to request data.
+ [📕Initial novel](https://m.touwz.net/dushi/yinhezhuiluo/)
> Simple paging, no difficulty. The only thing that needs attention is that the paging links are hidden in the js code and can be extracted using regular expressions.
``` javascript
.chapter>li>a@@@@@@let content="\n";let checkContent=(doc,over)=>{word=doc.querySelector('.content-div');if(!word) content+='\n'+doc.body.innerText;else {let ps=[];[].forEach.call(word.children, p=>{if(p.className!='moreinfo')ps.push (p.innerText)});content+=ps.join('\n');}let next=doc.querySelector("#pt_next");if(next){fetch(location.href+ doc.body.innerHTML. match(/'([^\|']+)\|[^']+'\.split/)[1]+".html").then(r => r.text()).then( d => {let _doc = document.implementation.createHTMLDocument('');_doc.documentElement.innerHTML = d;checkContent(_doc,over);});}else over();};checkContent(data,()= >{cb(content.replace(/\s* "If the chapter is missing, please exit # exit # reading # reading # mode"\s*|\s*This chapter is not finished, click the next page to continue reading. >>>\ s*/g,''))});return false;
```
+ [📕H Library](https://hlib.cc/s/sis-9732262)
> For simple paging, you can use simplified rules, pass in the selector of the inner paging, and let the script handle the rest automatically.
``` javascript
.list-group-item>div>a.text-decoration-none@@@@@@next:{[aria-label='Next page']+a}
```

### Test page
+ http://www.gulongbbs.com/zhentan/bdlr/plje/Index.html
+ http://www.jhshe.com/thread-1837-1-1.html
+ http://tieba.baidu.com/p/4871634479

### FAQ
- What should I do if the chapter does not have the words "which chapter and which section"? <br>
Just refer to the second-level mental method and enter one of the chapter names.
- What should I do if the crawling timeout fails after downloading a certain number of chapters? <br>
It may be that the website limits the number of concurrent threads. Just lower the number of execution threads in the settings. Set to a positive number to represent the number of execution threads, and a negative number to download a chapter every x seconds. For example, `-2` means to download a chapter every 2 seconds.
- What should I do if there is no response when pressing the shortcut keys? <br>
It may be that the shortcut keys have been taken over by other applications. Use the command menu in the script manager to download it.
- What should I do if there are irrelevant interfering characters? <br>
Just enter the jamming code css selector in the settings. Multiple selectors are separated by commas.
- What should I do if the order of chapters is wrong? <br>
The default is to sort by the position within the web page. Click Settings and try changing to "Reorder by URL" or "Reorder by Chapter Name"
- What should I do if the chapter title is wrong? <br>
The default is to take the chapter link text as the title. You can customize the chapter title in the settings. Enter title to crawl the title of the paginated page. Enter h1 to crawl the h1 level article title of the paginated page.
- What should I do if the downloaded content is incomplete? <br>
It may be because the text in the page is loaded dynamically. You can try to check "Open filter window before downloading" on the settings page, and then check "Use iframe to load content in the background"
- Why did the crawl fail? <br>
NETWORK ERROR indicates a network error, which may be caused by the current local network failure or the IP being blocked by the target website. TIMEOUT indicates that the access timeout may be because the current network speed is too slow or the target website traffic exceeds the limit.
- If you have any other questions, please contact me via email and I will help you solve it when I am available.

### Why write this script?
Mainly <img src="https://stickershop.line-scdn.net/stickershop/v1/product/8692/LINEStorePC/main.png;compress=true" width=50 alt="Lazy" title="Lazy Ah"/>
Because I want to download Chi Xingzhou's Drifting Street, but I found that the predecessor's wheel "[Novel] Download Script" cannot be used, and I don't want to write rules for this broken website🙃, and <del>I just don't like you, the overbearing president Xiu Xian Time Travel Bite me</del>Maybe it changes its version every three days. By writing a script with general rules, firstly, you don’t have to chase countless novel sites to adapt, modify and update it, and secondly, you can avoid legal risks.
This script will automatically find the main content and download it, without writing rules. Of course, if your website has more advertising content than the main text, I can’t help it.
When encountering special websites, it is recommended to use "[Novel] Download Script".
