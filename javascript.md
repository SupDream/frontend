# [JavaScript 的性能优化：加载和执行](http://www.ibm.com/developerworks/cn/web/1308_caiys_jsload/index.html)
JavaScript 在浏览器中的性能因JavaScript的阻塞特性变的复杂,也就是说当浏览器在执行js代码时，不能同时做其它事情，即<script>每次出现都会让页面等待脚本的解析和执行（不论JS是内嵌的还是外链的），JS代码执行完成后，才继续渲染页面。浏览器在下载和执行脚本时出现阻塞的原因在于，脚本可能会改变页面或 JavaScript 的命名空间，它们对后面页面内容造成影响。
##脚本位置
推荐将所有<script>标签尽可能放到<body>标签的底部，以尽量减少对整个页面下载的影响。
##组织脚本
通常一个大型网站或应用需要依赖数个JavaScript文件，可以把多个文件合并成一个，这样只需要引用一个<script>标签，就可以减少性能消耗。
##无阻塞的脚本
在页面加载完成后才加载 JavaScript 代码。这就意味着在 window 对象的 onload事件触发后再下载脚本。
###延迟加载脚本
 IE 4 和 Firefox 3.5 更高版本的浏览器
 Defer 属性指明本元素所含的脚本不会修改 DOM，因此代码能安全地延迟执行。
 ###动态脚本元素
 ```javascript
 var script = document.createElement ("script");
   script.type = "text/javascript";
   script.src = "script1.js";
   document.getElementsByTagName("head")[0].appendChild(script);
 ```
 ###使用 XMLHttpRequest(XHR)对象
 ```javascript
 var xhr = new XMLHttpRequest();
xhr.open("get", "script1.js", true);
xhr.onreadystatechange = function(){
    if (xhr.readyState == 4){
        if (xhr.status >= 200 && xhr.status < 300 || xhr.status == 304){
            var script = document.createElement ("script");
            script.type = "text/javascript";
            script.text = xhr.responseText;
            document.body.appendChild(script);
        }
    }
};
xhr.send(null);
```
这种方法的主要优点是，您可以下载不立即执行的 JavaScript 代码。限制是：JavaScript 文件必须与页面放置在同一个域内，不能从 CDN 下载
