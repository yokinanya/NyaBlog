---
title: '萌娘百科上的黑幕实现'
date: 2024-12-22 23:38:45
---

引用自 [萌娘百科 MediaWiki:Gadget-site-styles.css](https://zh.moegirl.org.cn/MediaWiki:Gadget-site-styles.css)
在css中添加如下代码：
```css
/* 萌娘百科 “黑幕” */
/* 以下内容引用自萌娘百科 https://zh.moegirl.org.cn/MediaWiki:Gadget-site-styles.css，感谢！ */
/* MoeGirl Start */

.heimu,
.heimu rt {
  background-color: #252525;
}

.heimu,
.heimu a,
a .heimu,
a.new .heimu,
span.heimu a.new,
span.heimu a.external,
span.heimu a.external:visited,
span.heimu a.extiw,
span.heimu a.extiw:visited,
span.heimu a.mw-disambig,
span.heimu a.mw-redirect {
  transition: color 0.13s linear;
  color: #252525;
  text-shadow: none;
}

span.heimu:hover,
span.heimu:active {
  color: white;
}

span.heimu:hover a,
a:hover span.heimu {
  color: lightblue;
}

span.heimu:hover a:visited,
a:visited:hover span.heimu {
  color: #c5cae9;
}

span.heimu:hover a.new,
a.new:hover span.heimu {
  color: #fcc;
}

span.heimu a.new:hover:visited,
a.new:hover:visited span.heimu {
  color: #ef9a9a;
}

span.heimu:hover a.extiw:visited,
a.extiw:visited:hover span.heimu {
  color: #d1c4e9;
}

/* MoeGirl End */
```

黑幕格式实现如下：
```html
<span class="heimu" title="你知道的太多了">黑幕</span>
```

效果如下：
<span class="heimu" title="你知道的太多了">黑幕</span>

