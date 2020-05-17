---
title: Vue曝光统计指令
abbrlink: b373
date: 2020-05-17 14:28:02
categories: 前端
tags: [Javascript,Vue]
---

![beach](https://cloudgrin.oss-cn-shanghai.aliyuncs.com/images/big_intersection_observer.jpeg?x-oss-process=image/resize,w_600,limit_0)

<section id="nice" data-tool="mdnice编辑器" data-website="https://www.mdnice.com" style="font-size: 16px; padding: 0 10px; word-spacing: 0px; word-break: break-word; word-wrap: break-word; text-align: left; margin-top: -10px; line-height: 1.25; color: #2b2b2b; font-family: Optima-Regular, Optima, PingFangTC-Light, PingFangSC-light, PingFangTC-light; letter-spacing: 2px; background-image: linear-gradient(90deg, rgba(50, 0, 0, 0.04) 3%, rgba(0, 0, 0, 0) 3%), linear-gradient(360deg, rgba(50, 0, 0, 0.04) 3%, rgba(0, 0, 0, 0) 3%); background-size: 20px 20px; background-position: center center;">
<h2 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 22px; display: block; border-bottom: 4px solid #40B8FA;"><span class="prefix" style="display: flex; width: 20px; height: 20px; background-size: 20px 20px; background-image: url(https://imgkr.cn-bj.ufileos.com/15fdfb3c-b350-4da9-928e-5f8c506ec325.png); margin-bottom: -22px;"></span><span class="content" style="display: flex; color: #40B8FA; font-size: 20px; margin-left: 25px;">什么时候需要埋点：</span><span class="suffix" style="display: flex; box-sizing: border-box; width: 200px; height: 10px; border-top-left-radius: 20px; background: RGBA(64, 184, 250, .5); color: rgb(255, 255, 255); font-size: 16px; letter-spacing: 0.544px; justify-content: flex-end; float: right; margin-top: -10px; box-sizing: border-box !important; overflow-wrap: break-word !important;"></span></h2>
<ul data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; font-size: 15px; color: #595959; list-style-type: circle;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">当一款新产品规划完成需要看数据时；</section></li><li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">某个功能改版规划完成需要看数据时；</section></li><li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">ABTest需要判断哪种方案更好时；</section></li><li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">需要看H5活动页的统计数据时；</section></li><li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">需要看第三方投放的引流效果时；</section></li><li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">以及今天我们将谈论的广告位曝光埋点；</section></li><li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">...</section></li></ul>
<h2 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 22px; display: block; border-bottom: 4px solid #40B8FA;"><span class="prefix" style="display: flex; width: 20px; height: 20px; background-size: 20px 20px; background-image: url(https://imgkr.cn-bj.ufileos.com/15fdfb3c-b350-4da9-928e-5f8c506ec325.png); margin-bottom: -22px;"></span><span class="content" style="display: flex; color: #40B8FA; font-size: 20px; margin-left: 25px;">需求</span><span class="suffix" style="display: flex; box-sizing: border-box; width: 200px; height: 10px; border-top-left-radius: 20px; background: RGBA(64, 184, 250, .5); color: rgb(255, 255, 255); font-size: 16px; letter-spacing: 0.544px; justify-content: flex-end; float: right; margin-top: -10px; box-sizing: border-box !important; overflow-wrap: break-word !important;"></span></h2>
<p data-tool="mdnice编辑器" style="padding-top: 8px; padding-bottom: 8px; line-height: 26px; color: #2b2b2b; margin: 10px 0px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px;">我司主要是做电商业务，最近卖出去了一些广告位，想要统计这些广告位的曝光情况，然后就可以理直气壮的拿出数据向商家收钱了。</p>
<p data-tool="mdnice编辑器" style="padding-top: 8px; padding-bottom: 8px; line-height: 26px; color: #2b2b2b; margin: 10px 0px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px;">产品提出需求方案是广告位漏出 1/4 且连续时间达到 2 秒，广告位类型主要是轮播和 banner。</p>
<h2 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 22px; display: block; border-bottom: 4px solid #40B8FA;"><span class="prefix" style="display: flex; width: 20px; height: 20px; background-size: 20px 20px; background-image: url(https://imgkr.cn-bj.ufileos.com/15fdfb3c-b350-4da9-928e-5f8c506ec325.png); margin-bottom: -22px;"></span><span class="content" style="display: flex; color: #40B8FA; font-size: 20px; margin-left: 25px;">方案</span><span class="suffix" style="display: flex; box-sizing: border-box; width: 200px; height: 10px; border-top-left-radius: 20px; background: RGBA(64, 184, 250, .5); color: rgb(255, 255, 255); font-size: 16px; letter-spacing: 0.544px; justify-content: flex-end; float: right; margin-top: -10px; box-sizing: border-box !important; overflow-wrap: break-word !important;"></span></h2>
<ul data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; font-size: 15px; color: #595959; list-style-type: circle;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">监听滚动（节流），实时计算广告位与视窗的位置。</section></li><li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">使用定时器定时计算页面广告位与视窗的位置。</section></li></ul>
<p data-tool="mdnice编辑器" style="padding-top: 8px; padding-bottom: 8px; line-height: 26px; color: #2b2b2b; margin: 10px 0px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px;">但这两个方案有一个弊端就是都需要调用 Element.getBoundingClientRect()，这个API在主线程上运行且会引起页面回流，极易拖慢整个网站的性能。</p>
<p data-tool="mdnice编辑器" style="padding-top: 8px; padding-bottom: 8px; line-height: 26px; color: #2b2b2b; margin: 10px 0px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px;">其实统计曝光和图片懒加载一样，非常适合使用 <a href="https://developer.mozilla.org/zh-CN/docs/Web/API/Intersection_Observer_API" style="text-decoration: none; word-wrap: break-word; color: #40B8FA; font-weight: normal; border-bottom: 1px solid #3BAAFA;">IntersectionObserver</a> 这个原生API。</p>
<p data-tool="mdnice编辑器" style="padding-top: 8px; padding-bottom: 8px; line-height: 26px; color: #2b2b2b; margin: 10px 0px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px;">Intersection Observer API 会注册一个回调方法，每当期望被监视的元素进入或者退出另外一个元素的时候(或者浏览器的视口)该回调方法将会被执行，或者两个元素的交集部分大小发生变化的时候回调方法也会被执行。通过这种方式，网站将不需要为了监听两个元素的交集变化而在主线程里面做任何操作，并且浏览器可以帮助我们优化和管理两个元素的交集变化。</p>
<p data-tool="mdnice编辑器" style="padding-top: 8px; padding-bottom: 8px; line-height: 26px; color: #2b2b2b; margin: 10px 0px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px;">根据我们的需要只要针对性的再增加时间的统计就可以了。</p>
<h2 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 22px; display: block; border-bottom: 4px solid #40B8FA;"><span class="prefix" style="display: flex; width: 20px; height: 20px; background-size: 20px 20px; background-image: url(https://imgkr.cn-bj.ufileos.com/15fdfb3c-b350-4da9-928e-5f8c506ec325.png); margin-bottom: -22px;"></span><span class="content" style="display: flex; color: #40B8FA; font-size: 20px; margin-left: 25px;">实现思路</span><span class="suffix" style="display: flex; box-sizing: border-box; width: 200px; height: 10px; border-top-left-radius: 20px; background: RGBA(64, 184, 250, .5); color: rgb(255, 255, 255); font-size: 16px; letter-spacing: 0.544px; justify-content: flex-end; float: right; margin-top: -10px; box-sizing: border-box !important; overflow-wrap: break-word !important;"></span></h2>
<p data-tool="mdnice编辑器" style="padding-top: 8px; padding-bottom: 8px; line-height: 26px; color: #2b2b2b; margin: 10px 0px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px;">封装vue指令，如果你使用react，也可以修改为hoc。</p>
<p data-tool="mdnice编辑器" style="padding-top: 8px; padding-bottom: 8px; line-height: 26px; color: #2b2b2b; margin: 10px 0px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px;">由于Intersection Observer API<a href="https://www.caniuse.com/#search=interS" style="text-decoration: none; word-wrap: break-word; color: #40B8FA; font-weight: normal; border-bottom: 1px solid #3BAAFA;">兼容性</a>不好，尤其是IOS 12.2以上才支持，所以需要使用官方的 <a href="https://www.npmjs.com/package/intersection-observer" style="text-decoration: none; word-wrap: break-word; color: #40B8FA; font-weight: normal; border-bottom: 1px solid #3BAAFA;">polyfill垫片</a>。</p>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">除了完成现有的需求之外，我们在开发时也要考虑到实际使用，以及业务拓展的情况。比如产品经理要求有的广告位只上报一次，有的曝光一次上报一次，下拉刷新重置当前页面所有统计，还有我猜会不会需要支持第一次曝光时间是2秒，之后修改时间的情况，毕竟产品经理的脑回路和开发不一样，我们最好的是在第一次开发的时候把这些想到的情况都自己过一遍，合理的就一次开发掉。</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<h2 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 22px; display: block; border-bottom: 4px solid #40B8FA;"><span class="prefix" style="display: flex; width: 20px; height: 20px; background-size: 20px 20px; background-image: url(https://imgkr.cn-bj.ufileos.com/15fdfb3c-b350-4da9-928e-5f8c506ec325.png); margin-bottom: -22px;"></span><span class="content" style="display: flex; color: #40B8FA; font-size: 20px; margin-left: 25px;">代码实现</span><span class="suffix" style="display: flex; box-sizing: border-box; width: 200px; height: 10px; border-top-left-radius: 20px; background: RGBA(64, 184, 250, .5); color: rgb(255, 255, 255); font-size: 16px; letter-spacing: 0.544px; justify-content: flex-end; float: right; margin-top: -10px; box-sizing: border-box !important; overflow-wrap: break-word !important;"></span></h2>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px;"><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; background: #fafafa; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; border-radius: 0px; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px;"><span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// observe-exposure.js</span>
<span/>
<span/><span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">/**
<span/> * v-exposure="callback"
<span/> * v-exposure="{callback,time:2000,disabled:false,once:false,intersection:{root:null,rootMargin:'0',threshold:0}}"
<span/> * @description 曝光指令（漏出面积比例 and 达到多长时间）
<span/> * @param {function} callback(entry) 曝光回调
<span/> *****************************@param {object} entry IntersectionObserverEntry对象
<span/> * @param {number} time @default 2000 曝光持续时间
<span/> * @param {boolean} disabled @default false 是否禁用（true-&gt;false 可以重新开启曝光检测，使用方式：在callback时置为true，当需要重新观察时改为false）
<span/> * @param {boolean} once @default false 只曝光一次
<span/> * @param {object} intersection IntersectionObserver 的 option
<span/> */</span>
<span/>
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'intersection-observer'</span> <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 兼容 polyfill</span>
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> { processCallbackOptions, deepEqual } <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'./util.js'</span>
<span/>IntersectionObserver.prototype[<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'THROTTLE_TIMEOUT'</span>] = <span class="hljs-number" style="color: #986801; line-height: 26px;">300</span> <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 兼容时 节流 300ms触发一次</span>
<span/>
<span/><span class="hljs-class" style="line-height: 26px;"><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">class</span> <span class="hljs-title" style="color: #c18401; line-height: 26px;">Exposure</span> </span>{
<span/>  <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">constructor</span> (el, options, vnode) {
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._el = el <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 被观察元素</span>
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._options = <span class="hljs-literal" style="color: #0184bb; line-height: 26px;">null</span>
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._observer = <span class="hljs-literal" style="color: #0184bb; line-height: 26px;">null</span> <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 观察者</span>
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._frozen = <span class="hljs-literal" style="color: #0184bb; line-height: 26px;">false</span> <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 只触发一次曝光回调（设置了once）</span>
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._timer = <span class="hljs-literal" style="color: #0184bb; line-height: 26px;">null</span> <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 定时器ID</span>
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._callback = <span class="hljs-literal" style="color: #0184bb; line-height: 26px;">null</span> <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 曝光回调</span>
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._oldResult = <span class="hljs-literal" style="color: #0184bb; line-height: 26px;">undefined</span> <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 出现 or 隐藏 (对于root)</span>
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>.createObserver(options, vnode) <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 创造观察者</span>
<span/>  }
<span/>
<span/>  <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 曝光交集边界值</span>
<span/>  <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">get</span> _threshold () {
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">return</span> <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._options.intersection?.threshold ?? <span class="hljs-number" style="color: #986801; line-height: 26px;">0</span>
<span/>  }
<span/>
<span/>  <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">/**
<span/>   * 创造观察者
<span/>   * 1. 初始化
<span/>   * 2. 指令入参更新
<span/>   */</span>
<span/>  createObserver (options, vnode) {
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">if</span> (<span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._observer) {
<span/>      <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 销毁之前的观察者</span>
<span/>      <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>.destroyObserver()
<span/>    }
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">if</span> (<span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._frozen) {
<span/>      <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 设置了once，且已经曝光</span>
<span/>      <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">return</span>
<span/>    }
<span/>    <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 处理option（直传callback和对象模式）</span>
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._options = processCallbackOptions(options)
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._callback = <span class="hljs-function" style="line-height: 26px;">(<span class="hljs-params" style="line-height: 26px;">entry</span>) =&gt;</span> {
<span/>      <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 曝光回调</span>
<span/>      <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._options.callback(entry)
<span/>      <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">if</span> (<span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._options.once) {
<span/>        <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 设置了once，停止观察</span>
<span/>        <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>.destroyObserver()
<span/>        <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._frozen = <span class="hljs-literal" style="color: #0184bb; line-height: 26px;">true</span>
<span/>      }
<span/>    }
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._oldResult = <span class="hljs-literal" style="color: #0184bb; line-height: 26px;">undefined</span>
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._observer = <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">new</span> IntersectionObserver(<span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">entries</span> =&gt;</span> {
<span/>      <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">let</span> entry = entries[<span class="hljs-number" style="color: #986801; line-height: 26px;">0</span>]
<span/>      <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> result = entry.isIntersecting &amp;&amp; entry.intersectionRatio &gt;= <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._threshold
<span/>      <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">if</span> (result !== <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._oldResult) {
<span/>        <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._oldResult = result
<span/>        <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">if</span> (result) {
<span/>          <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 显示（对于root）切换</span>
<span/>          <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._timer = setTimeout(<span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">()</span> =&gt;</span> {
<span/>            <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._callback(entry)
<span/>          }, <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._options.time ?? <span class="hljs-number" style="color: #986801; line-height: 26px;">2000</span>)
<span/>        } <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">else</span> {
<span/>          <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 隐藏（对于root）切换</span>
<span/>          clearTimeout(<span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._timer)
<span/>          <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._timer = <span class="hljs-literal" style="color: #0184bb; line-height: 26px;">null</span>
<span/>        }
<span/>      }
<span/>    }, <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._options.intersection)
<span/>    <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 渲染完成后观察</span>
<span/>    vnode.context.$nextTick(<span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">()</span> =&gt;</span> {
<span/>      <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._observer.observe(<span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._el)
<span/>    })
<span/>  }
<span/>  <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 销毁观察者</span>
<span/>  destroyObserver () {
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">if</span> (<span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._observer) {
<span/>      <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._observer.disconnect()
<span/>      <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._observer = <span class="hljs-literal" style="color: #0184bb; line-height: 26px;">null</span>
<span/>    }
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">if</span> (<span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._timer) {
<span/>      clearTimeout(<span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._timer)
<span/>      <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._timer = <span class="hljs-literal" style="color: #0184bb; line-height: 26px;">null</span>
<span/>    }
<span/>  }
<span/>
<span/>  <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">/**
<span/>   * 解冻
<span/>   * 可以使设置了once的已曝光元素重新接受曝光检测
<span/>   */</span>
<span/>  resetFrozen () {
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">this</span>._frozen = <span class="hljs-literal" style="color: #0184bb; line-height: 26px;">false</span>
<span/>  }
<span/>}
<span/>
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">export</span> <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">default</span> {
<span/>  bind (el, { value }, vnode) {
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">if</span> (!value || value?.disabled) <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">return</span>
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">if</span> (<span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">typeof</span> value === <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'function'</span> || ((value <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">instanceof</span> <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">Object</span>) &amp;&amp; <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">typeof</span> value.callback === <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'function'</span>)) {
<span/>      <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> observer = <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">new</span> Exposure(el, value, vnode)
<span/>      el._vue_exposure = observer
<span/>    } <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">else</span> {
<span/>      <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">console</span>.warn(<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'[v-exposure] You should pass in a value of type function or an object containing a callback of type function. '</span>)
<span/>    }
<span/>  },
<span/>  update (el, { value, oldValue }, vnode) {
<span/>    <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 值未改变（vnode更新）</span>
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">if</span> (deepEqual(value, oldValue)) <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">return</span>
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">let</span> observer = el._vue_exposure
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">if</span> (observer &amp;&amp; (!value || value?.disabled)) {
<span/>      observer.destroyObserver()
<span/>      <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">return</span>
<span/>    }
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">if</span> (<span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">typeof</span> value === <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'function'</span> || ((value <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">instanceof</span> <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">Object</span>) &amp;&amp; <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">typeof</span> value.callback === <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'function'</span>)) {
<span/>      <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">if</span> (observer) {
<span/>        <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 更新指令传值</span>
<span/>        <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">if</span> (value?.disabled === <span class="hljs-literal" style="color: #0184bb; line-height: 26px;">false</span> &amp;&amp; oldValue.disabled === <span class="hljs-literal" style="color: #0184bb; line-height: 26px;">true</span>) {
<span/>          <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 如果设置了once，只有disabled被修改为false，才会解除冻结</span>
<span/>          <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 之前未曝光的不受disabled修改为false影响，会被createObserver()更新曝光设置（重新生成新的观察者）</span>
<span/>          <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 之前已经曝光的会重新接受曝光检测，也会更新新的option创建观察者</span>
<span/>          observer.resetFrozen()
<span/>        }
<span/>        observer.createObserver(value, vnode)
<span/>      } <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">else</span> {
<span/>        observer = <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">new</span> Exposure(el, value, vnode)
<span/>        el._vue_exposure = observer
<span/>      }
<span/>    } <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">else</span> {
<span/>      <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">console</span>.warn(<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'[v-exposure] You should pass in a value of type function or an object containing a callback of type function. '</span>)
<span/>      observer &amp;&amp; observer.destroyObserver()
<span/>    }
<span/>  },
<span/>  unbind (el) {
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> observer = el._vue_exposure
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">if</span> (observer) {
<span/>      observer.destroyObserver()
<span/>      <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">delete</span> el._vue_exposure
<span/>    }
<span/>  }
<span/>}
<span/>
<span/></code></pre>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px;"><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; background: #fafafa; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; border-radius: 0px; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px;"><span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// util.js</span>
<span/>
<span/><span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 处理option（直传callback和对象模式）</span>
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">export</span> <span class="hljs-function" style="line-height: 26px;"><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">function</span> <span class="hljs-title" style="color: #4078f2; line-height: 26px;">processCallbackOptions</span> (<span class="hljs-params" style="line-height: 26px;">value</span>) </span>{
<span/>  <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">let</span> options
<span/>  <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">if</span> (<span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">typeof</span> value === <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'function'</span>) {
<span/>    options = {
<span/>      <span class="hljs-attr" style="color: #986801; line-height: 26px;">callback</span>: value
<span/>    }
<span/>  } <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">else</span> {
<span/>    options = value
<span/>  }
<span/>  <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">return</span> options
<span/>}
<span/>
<span/><span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 判断两值是否相等</span>
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">export</span> <span class="hljs-function" style="line-height: 26px;"><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">function</span> <span class="hljs-title" style="color: #4078f2; line-height: 26px;">deepEqual</span> (<span class="hljs-params" style="line-height: 26px;">val1, val2</span>) </span>{
<span/>  <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">if</span> (val1 === val2) <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">return</span> <span class="hljs-literal" style="color: #0184bb; line-height: 26px;">true</span>
<span/>  <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">if</span> (<span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">typeof</span> val1 === <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'object'</span>) {
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">for</span> (<span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> key <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">in</span> val1) {
<span/>      <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">if</span> (!deepEqual(val1[key], val2[key])) {
<span/>        <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">return</span> <span class="hljs-literal" style="color: #0184bb; line-height: 26px;">false</span>
<span/>      }
<span/>    }
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">return</span> <span class="hljs-literal" style="color: #0184bb; line-height: 26px;">true</span>
<span/>  }
<span/>  <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">return</span> <span class="hljs-literal" style="color: #0184bb; line-height: 26px;">false</span>
<span/>}
<span/></code></pre>
<h2 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 22px; display: block; border-bottom: 4px solid #40B8FA;"><span class="prefix" style="display: flex; width: 20px; height: 20px; background-size: 20px 20px; background-image: url(https://imgkr.cn-bj.ufileos.com/15fdfb3c-b350-4da9-928e-5f8c506ec325.png); margin-bottom: -22px;"></span><span class="content" style="display: flex; color: #40B8FA; font-size: 20px; margin-left: 25px;">未支持功能</span><span class="suffix" style="display: flex; box-sizing: border-box; width: 200px; height: 10px; border-top-left-radius: 20px; background: RGBA(64, 184, 250, .5); color: rgb(255, 255, 255); font-size: 16px; letter-spacing: 0.544px; justify-content: flex-end; float: right; margin-top: -10px; box-sizing: border-box !important; overflow-wrap: break-word !important;"></span></h2>
<ul data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; font-size: 15px; color: #595959; list-style-type: circle;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">目前指令做的是满足条件后调用传入的callback，你也可以根据需要改成指令直接调用API，把需要上报的数据传入指令或者dataset</section></li><li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">对于ToC项目，由于用户体量巨大不可能曝光一次就上报一次，这样对服务器压力🍐会很大，所以我们需要缓存，定时批量上报。</section></li></ul>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">以上第二点，定时批量上报会涉及到一种特殊场景，就是待上报队列还未清空，结果用户关闭了浏览器/webview，导致代码没法执行的情况。目前想到几种方案：<p>1. 不考虑时效性的情况下，存储到localStorage，下次应用启动时上报；</p><p>2. webview下让native支持在后台进程中上报 </p><p>3. 使用 <a href="https://developer.mozilla.org/zh-CN/docs/Web/API/Navigator/sendBeacon" style="text-decoration: none; word-wrap: break-word; color: #40B8FA; font-weight: normal; border-bottom: 1px solid #3BAAFA;">sendBeacon</a> API</p></p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<h2 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 22px; display: block; border-bottom: 4px solid #40B8FA;"><span class="prefix" style="display: flex; width: 20px; height: 20px; background-size: 20px 20px; background-image: url(https://imgkr.cn-bj.ufileos.com/15fdfb3c-b350-4da9-928e-5f8c506ec325.png); margin-bottom: -22px;"></span><span class="content" style="display: flex; color: #40B8FA; font-size: 20px; margin-left: 25px;">参考文献</span><span class="suffix" style="display: flex; box-sizing: border-box; width: 200px; height: 10px; border-top-left-radius: 20px; background: RGBA(64, 184, 250, .5); color: rgb(255, 255, 255); font-size: 16px; letter-spacing: 0.544px; justify-content: flex-end; float: right; margin-top: -10px; box-sizing: border-box !important; overflow-wrap: break-word !important;"></span></h2>
<ul data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; font-size: 15px; color: #595959; list-style-type: circle;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;"><p style="padding-top: 8px; padding-bottom: 8px; line-height: 26px; color: #2b2b2b; margin: 10px 0px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px;"><a href="https://developer.mozilla.org/zh-CN/docs/Web" style="text-decoration: none; word-wrap: break-word; color: #40B8FA; font-weight: normal; border-bottom: 1px solid #3BAAFA;">MDN</a></p>
</section></li><li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;"><p style="padding-top: 8px; padding-bottom: 8px; line-height: 26px; color: #2b2b2b; margin: 10px 0px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px;"><a href="https://cn.vuejs.org/v2/guide/custom-directive.html" style="text-decoration: none; word-wrap: break-word; color: #40B8FA; font-weight: normal; border-bottom: 1px solid #3BAAFA;">Vue 指令</a></p>
</section></li></ul>
<p id="nice-suffix-juejin-container" class="nice-suffix-juejin-container" data-tool="mdnice编辑器" style="padding-top: 8px; padding-bottom: 8px; line-height: 26px; color: #2b2b2b; margin: 10px 0px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin-top: 20px !important;">本文使用 <a href="https://mdnice.com" style="text-decoration: none; word-wrap: break-word; color: #40B8FA; font-weight: normal; border-bottom: 1px solid #3BAAFA;">mdnice</a> 排版</p></section>