 # performance

mdn相关链接   https://developer.mozilla.org/zh-CN/docs/Web/API/Performance

[bolg]https://www.cnblogs.com/libin-1/p/6501951.html


[`window.performance`](https://www.w3.org/TR/2014/WD-navigation-timing-2-20140325/) 是W3C性能小组引入的新的API，目前IE9以上的浏览器都支持。

在浏览器 控制台打印  `window.performance`
或者 ` performance.toJSON()`

`timing` API


![](performance.timing.png)

 计算性能指标

可以使用`Navigation.timing` 统计到的时间数据来计算一些页面性能指标，比如DNS查询耗时、白屏时间、domready等等。如下：
-   DNS查询耗时 = domainLookupEnd - domainLookupStart
-   TCP链接耗时 = connectEnd - connectStart
-   request请求耗时 = responseEnd - responseStart
-   解析dom树耗时 = domComplete - domInteractive
-   白屏时间 = domloadng - fetchStart
-   domready时间 = domContentLoadedEventEnd - fetchStart
-   onload时间 = loadEventEnd - fetchStart


相关代码

```js
	;
(function() {

    handleAddListener('load', getTiming)

    function handleAddListener(type, fn) {
        if(window.addEventListener) {
            window.addEventListener(type, fn)
        } else {
            window.attachEvent('on' + type, fn)
        }
    }

    function getTiming() {
        try {
            var time = performance.timing;
            var timingObj = {};

            var loadTime = (time.loadEventEnd - time.loadEventStart) / 1000;

            if(loadTime < 0) {
                setTimeout(function() {
                    getTiming();
                }, 200);
                return;
            }

            timingObj['重定向时间'] = (time.redirectEnd - time.redirectStart) / 1000;
            timingObj['DNS解析时间'] = (time.domainLookupEnd - time.domainLookupStart) / 1000;
            timingObj['TCP完成握手时间'] = (time.connectEnd - time.connectStart) / 1000;
            timingObj['HTTP请求响应完成时间'] = (time.responseEnd - time.requestStart) / 1000;
            timingObj['DOM开始加载前所花费时间'] = (time.responseEnd - time.navigationStart) / 1000;
            timingObj['DOM加载完成时间'] = (time.domComplete - time.domLoading) / 1000;
            timingObj['DOM结构解析完成时间'] = (time.domInteractive - time.domLoading) / 1000;
            timingObj['脚本加载时间'] = (time.domContentLoadedEventEnd - time.domContentLoadedEventStart) / 1000;
            timingObj['onload事件时间'] = (time.loadEventEnd - time.loadEventStart) / 1000;
            timingObj['页面完全加载时间'] = (timingObj['重定向时间'] + timingObj['DNS解析时间'] + timingObj['TCP完成握手时间'] + timingObj['HTTP请求响应完成时间'] + timingObj['DOM结构解析完成时间'] + timingObj['DOM加载完成时间']);

            for(item in timingObj) {
                console.log(item + ":" + timingObj[item] + '毫秒(ms)');
            }

            console.log(performance.timing);

        } catch(e) {
            console.log(timingObj)
            console.log(performance.timing);
        }
    }
})();


```