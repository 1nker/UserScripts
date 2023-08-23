[☯️](https://greasyfork.org/scripts/438684)東方永頁機 v.1.9.36.50
==
*Pagetual - Perpetual pages. Auto loading paginated web pages for 90% of all web sites ! [**Wiki**](https://pagetual.hoothin.com/en/)*

<a name="otherconfig" title="for restricted environment" href="https://pagetual.hoothin.com/en/rule.html"><strong>🔧Configuration page on wiki</strong></a>

<p name="click2import"></p>
<pre name="pagetual">
https://raw.githubusercontent.com/hoothin/UserScripts/master/Pagetual/pagetualRules.json
</pre>

---

[Rules example](https://pagetual.hoothin.com/en/rule.html)
==
``` json
[
  {
    "name":"beauty",
    "url":"^https://www\\.jpmn8\\.com/",
    "pageElement":"p>img"
  },
  {
    "name":"Expreview",
    "url":"^https://www\\.expreview\\.com/",
    "loadMore":"span.load"
  }
]
```

<table>
    <tr>
        <th colspan="3">Buy me a coffee if it helps you💞</th>
    </tr>
    <tr>
        <td><a href="https://paypal.me/hoothin">PayPal.Me</a></td><td><a href="https://ko-fi.com/hoothin">Ko-fi</a></td><td><a href="https://afdian.net/@hoothin">愛發電</a></td>
    </tr>
    <tr>
        <th colspan="3"><a href="mailto:rixixi@gmail.com">Send me an email for help</a></th>
    </tr>
</table>

![donate](https://s2.loli.net/2023/02/06/afTMxeASm48z5vE.jpg)

name
--
Name of the target site
```JSON
"name": "Site name"
```

author
--
Author of this rule
```JSON
"author": "Hoothin"
```

example
--
Example url of this rule
```JSON
"example": "https://abc.com"
```

[url](https://pagetual.hoothin.com/en/rules/url.html)
--
RegExp for the url of target site

[nextLink](https://pagetual.hoothin.com/en/rules/nextLink.html)
--
Selector or xpath of next page link, disable when set to 0, you can let it to be a array to contains multiple next links.

[pageElement](https://pagetual.hoothin.com/en/rules/pageElement.html)
--
Selector or xpath of main content which need to insert, you can let it to be a array to contains multiple page elements.

[enable](https://pagetual.hoothin.com/en/rules/enable.html)
--
0 means stop action when all matched

[include](https://pagetual.hoothin.com/en/rules/include.html)
--
Selector or xpath of the element which must include

[exclude](https://pagetual.hoothin.com/en/rules/exclude.html)
--
Selector or xpath of the element which must not include

[action](https://pagetual.hoothin.com/en/rules/action.html)
--
0 means load url and insert with static html, 1 means load by iframe so that dynamic javaScript code on page may effect, 2 means force insert iframe to bottom

[loadMore](https://pagetual.hoothin.com/en/rules/loadMore.html)
--
Selector of "load more" button you want to auto click
