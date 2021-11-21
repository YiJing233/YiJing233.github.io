---
title: 前端/Web学习Roadmap
date: 2021-01-30 20:38:36
tags: 
  - 技术
  - 百宝箱
index_img: https://s3.ax1x.com/2021/01/30/yAVp6I.png
---
前面讲了一些“鸡汤”，接下来就是站在一个前端工程师的视角上给出的学习Roadmap

**提前声明：书籍（包括电子书）、网站等仅供参考，是我自己的入门路径，大家不必“完美复刻”，不要忘记自己探索的重要性哦**

### 第一步：爬虫

是的，你没有看错，一个前端工程师的最开始要学习的是爬虫。

理由（新人可能看不懂，可以学会一些东西之后回头看）：

- 爬虫包含了最基本的知识，包括网络请求处理、数据交互与存储、网页的组成等等
- 现代Web技术以编程语言而不是HTML等静态标记语言为核心，且常见的爬虫语言Python、Go、JS等可以作为C语言知识之外的补充
- 信软科班教学，从科班出身的前端人员是重应用、重开发而不是重视觉、重交互

爬虫的教学有很多，个人的入门是通过这本书：

[Python网络爬虫权威指南（第2版）](https://www.ituring.com.cn/book/1980)

（里面的代码，我之前曾一行一行敲了两三遍，现在回想起来也是很有意义的事）

如果你还不会Python，可以考虑用这本书入门：

[Python编程：从入门到实践（第2版）](https://www.ituring.com.cn/book/2784)

如果只是入门Python，你可以只看第一部分，你感兴趣可以顺便学习后面的Django（一个后端框架）和小游戏。

网络上的课程方面，[阿里云的Python](https://developer.aliyun.com/learning/roadmap/python?spm=a2c6h.21110250.9515527460.1.12613c67ONV9BY)感觉节奏不错，如果有其他py入门课程可以回复，让大家都看到

另外，[廖雪峰的Python课程](https://www.liaoxuefeng.com/wiki/1016959663602400)可以作为进阶课程

### 第二步：前端两件套

当你用BeautifulSoap库获取数据的时候，你有没有发现网页的构成是什么样的？它是一个一个结构化的标签组成的，我们看到的所有网页都不例外，“盒子模型”贯穿始终。那么它们都是什么呢？接下来就是学习HTML了

而我们在筛选标签的过程中，用到了很多标签的特性，比如颜色，它是和标签放在一起的还是分离的？相同的的标签，比如div是靠什么让我们在视觉上区分开？这些都是CSS的作用，那么，CSS也要安排一下咯

网络资源推荐（CSS也是同样的平台去学）：

[w3school-HTML](https://www.w3school.com.cn/html/index.asp)

[菜鸟教程-HTML](https://www.runoob.com/html/html-tutorial.html)

书籍推荐：

[HTML5与CSS3基础教程](https://www.ituring.com.cn/book/1199)（进阶书籍，先看HTML再看HTML5哦）

### 第三步：综合运用

恭喜你，凭借以上的知识，你已经可以完成一个网站的搭建了，不信？快回去看看Python的Web教程（阿里云和廖雪峰都有），开始学习（或者重新学习）里面的课程吧，如果你之前学习了一些却没太理解，现在的你一定会茅塞顿开

给个传送门：

[阿里云-Python阶段2 Web开发](https://developer.aliyun.com/learning/roadmap/python?spm=a2c6h.21110250.9515527460.1.12613c67ONV9BY)

[廖雪峰-Python Web APP](https://www.liaoxuefeng.com/wiki/1016959663602400/1018138095494592)

如果你喜欢书本上的课程，我推荐这一本：

[Flask Web开发：基于Python的Web应用开发实战（第2版）](https://www.ituring.com.cn/book/2463)

这个时候，你已经是一个合格的前端/Web入门者了，你已经有了知识体系的初步构建和动手能力

### 番外篇

无论是爬虫还是Web App，少不了的一个部分就是数据存储。你可能会想，存储有什么呢？只要保存和读取就够了。那么如何做好保存和读取，它的形式是和我们在电脑里的文件一样吗？如果不一样，它的优势在哪里呢？

抱着这些问题，你可以开始入门数据库和SQL了，如果这个时候你已经是大二下学期，那么我很推荐你去抱着课程的书本学习，如果你还是一个小白，这本书应该可以帮助你入门

[SQL基础教程（第2版）](http://xn--sql(2)-hd5ru79j6e9abqr0egh2e/)

当然，这个时候也少不了廖雪峰

[廖雪峰的SQL课程](https://www.liaoxuefeng.com/wiki/1177760294764384)

如果你已经有了数据库基础，也很对数据库技术很感兴趣，论坛里的[另一篇关于数据库的帖子](https://d.jotang.club/t/topic/547)可能会帮到你哦（有种RPG游戏里从基础角色转职的感觉呢）

### 第四步：网络初入门

这个阶段，你已经对整个知识体系有了初步的认识。但是我们之前做的大多数工作都是在自己的计算机上进行的，有想过把自己写的Web应用给其他人使用吗？该怎么做呢？还有一个新的问题是为什么我们，为什么很多事情只有连接上网络（WiFi或是宽带、流量）才能进行？“互联网”的本质是什么？

能回答以上问题的是计算机网络的知识，而作为初入门，你可以先看这本书：

[图解HTTP](https://www.ituring.com.cn/book/1229)

（有很多图片，读起来相对简单）

如果你学有余力，还想了解更深层的计算机网络的知识，就去看这本书吧（同时它也是大二下计网课程的教材）

[计算机网络](https://book.douban.com/subject/30280001/)

### 第五步，JavaScript

JS，现代Web无法离开的语言，它随着web的浪潮起势，在它成长的过程中有了Ajax异步技术，正式宣告了前后端分离时代的诞生，09年Nodejs的诞生让JS的能量爆发，到了今天，GitHub上JS依然是最活跃的语言（甚至没有之一）

JS是极其重要的，强大的JS功底也是一个优秀的前端工程师必备的条件，那么让我们开始吧！

JS从es6（js的标准）开始突变，因此有教程是专讲es5及之前的语法，有些教程不区分es6和es5，在下面的资源推荐中会标明白

JS基础教程：

[网道-JavaScript](https://wangdoc.com/javascript/)（es5）

[现代JavaScript教程](https://zh.javascript.info/)（包含es6和es5）

[阮一峰ES6 入门教程](https://es6.ruanyifeng.com/) （只讲es6）

JS书籍

[JavaScript高级程序设计（第4版）](https://www.ituring.com.cn/book/2472)（经典红宝书，第四版应该有es6内容，存疑，非常推荐读）

[你不知道的JavaScript](http://xn--javascript-sj2p90k0z2ogbek99i/)（主讲es6，很喜欢的一套书，共分为上中下三卷，像一本武林秘籍）

[JavaScript DOM编程艺术（第二版）](https://www.ituring.com.cn/book/42)（可以读读）

但是以上这些还只是入门的标准，（其实我现在都不敢说自己会JS）所以，持续学习吧！

### 第六步，但也不是第六步

这个小标题是什么意思呢？是说你可以不用同步地去学习了，接下来的知识可以并发学习（那么问题来了，什么是并行什么是并发呢？）

#### Vue

如果让我推荐初学者学习哪种前端框架，我一定会说是Vue，因为它足够新手友好

网络资源推荐：

[Vue中文官网](https://cn.vuejs.org/v2/guide/)，内部有教程和规范等等，生态很好，中文文档很优秀

[被删的Vue学习教程](https://godbasin.github.io/front-end-playground/vue/) 来自勇聪、原寒的推荐，质量很有保证

而进阶知识，有本书推荐：

[深入浅出Vue.js](https://www.ituring.com.cn/book/2675)

以上的这些都是Vue2的教程，Vue3即将来到，那么该如何获取知识呢？

从这个时候开始，你会发现你来到了瞬息万变的现代Web时代，你会发现在本部分之后，书籍的推荐减少了，更多是在网络上的知识。因为很多有趣有用的教程、课程是你自己搜索到的，包括但不限于B站的课程、各种在线编程培训班视频资源、CSDN、知乎、掘金等社区的优秀文章等等

很多事情是自己搜索到的，是其他人推荐的，这就是探索能力的体现

除了Vue，我更想向大家介绍Vue的作者尤雨溪，可以看看这个：[尤雨溪的访谈视频](https://www.bilibili.com/video/BV1iE411H71U?from=search&seid=9938655123101131915)

PS：之前听同事和大牛推荐过国外的前端课程[Frontend Master](https://frontendmasters.com/)，如果你足够有💰和精力，可以选购

### 

#### Nodejs

因为你有过用Python开发Web应用的经验，所以入门Node对你来说并不难，如果你想了解更多Node的优势，可以看看进阶知识里推荐的那本书

教程推荐：

[廖雪峰-Nodejs](https://www.liaoxuefeng.com/wiki/1022910821149312/1023025235359040)（为什么总是你）

进阶知识：

[深入浅出Nodejs](https://www.ituring.com.cn/book/1290)

#### Linux知识

如果你没有在学校上Unix操作系统基础这门课，我比较推荐你看这本书：

[Linux程序设计(第4版)](https://www.ituring.com.cn/book/171)

#### React

前端两大框架里，另一个就是React，它在国外被使用的更多，而在国内没有Vue火（有多方面的原因导致，可以再开一篇帖子分析了）但这并不影响React也是一个优秀的前端“框架”，React和Vue在相互抄袭借鉴中都获得了成长

入门推荐；

[React小书](http://huziketang.mangojuice.top/books/react/)

[React中文网](https://react.docschina.org/docs/getting-started.html)

进阶知识？快去自己探索（其实是自己也并没有太多好的教程啦）推荐阅读国外的论坛或博客，讲解React的会很多

PS：如果大家有好的React教程、资料，可以在下面回复！

#### 网络安全

如果你是网络安全方向，这块可以跳过。

Web安全是网络安全的一个重要部分，虽然现在的各大公司工种里已经将安全工程师和前后端开发区分开来，但是一本的安全知识素养还是要有，如XSS、CSRF等攻击的原理

书籍推荐：

[白帽子讲Web安全](https://book.douban.com/subject/10546925/)

#### 回答一个问题

你已经学习到很多了，那么有这样一个问题：

从浏览器里的地址栏里输入一串域名并回车，到一个页面展现在你面前都发生了什么事情？

如果你能回答出50字，说明你已经入门了

如果你能回答出400字，说明你已经有了Web的整体脉络

如果你能用这个题目写一篇长长的文章，事无巨细滔滔不绝，那么我要给你竖起大拇指👍

每学习一段时间，就更新一下这个问题的回答吧！这个问题可以贯穿你的学习始终

#### Docker与服务端知识

你知道什么是LAMP/LNMP嘛？

它是Linux + Apache/Nginx + MySQL + PHP的缩写，是非常经典的一套动态网站的开源技术选型，你可以尝试用这一套东西搭建起自己的网站

（非常抱歉，这个部分没有推荐的教程，因为我自己也是零散着学习的😭 ，读者朋友们有推荐的可以在下面回复）

而Docker的出现，给前端朋友们一个快乐的方式部署自己的Web App，只需要一台云服务器即可，那么，尝试把自己写的Web App（无论是Vue项目还是React项目）部署起来，并在网络上访问它吧！

#### 小程序 + Serverless

如今，各家互联网公司都推出了自己的小程序应用，从微信小程序到支付宝小程序、头条小程序甚至快手小程序等等，在这样一个繁杂的“小程序”世界里，和前端是脱不开关系的，如何开发小程序，小程序的实现是什么样的，小程序的未来如何？

书籍推荐

[微信小程序开发入门和实践](https://book.douban.com/subject/27661869/)（缺点是这本书比较老了，是2017年的版本，但是可以看看）

Serverless在近年是一个很热门的话题，将“服务端”细化简化，为前端服务，Serverless在各大公司都有技术落地，且在国外也有创业公司专门做Serverless服务，代表的有[Netlify](https://www.netlify.com/)，而微信小程序的开发也和腾讯云Serverless有很深的融合，可以多多了解

#### 其他前端/Web技术

- Webpack，打包工具
- TypeScript，我认为会成为未来的JS技术标准（爆论）
- Babel，都快成为JS APP的基石了
- Electron（桌面端）
- React Native、Flutter（移动端）
- ES7、8、9与TC39 （当你了解了JS之后，就知道关注这些了）
- Deno （Node作者最近的新作）
- SSR （服务端渲染）
- V8 （V8引擎，与编译技术相关）
- WebAssembly（如何用其他语言开发Web）
- jquery （看看曾经的王者，可以学到很多）
- Wordpress/CMS （一个至今都很有价值的建一个网站的方式？）
- Low-code （前端未来的技术趋势之一）
- 前端智能化（前端未来的技术趋势之一）
- 前端工程化（前端是怎么成为一个工种的）

·······

## 一些很值得推荐的教程/Book/博文链接/会议

- 现代Web开发https://zhuanlan.zhihu.com/p/88616149
- [JSConfChina](https://2019.jsconfchina.com/) 看到上面的文章发现的，也很有意思
- [D2前端论坛](https://www.alibabaf2e.com/) 阿里组织的论坛
- [一年一度的前端技术趋势可视化](https://2019.stateofjs.com/)
- [Jamstack Conf ](https://jamstackconf.com/)，讲web的
- [RoadMap for FrontEnd](https://dev.to/rahxuls/ultimate-roadmap-with-so-many-resources-for-front-end-development-597f)
- [另一个RoadMap](https://dev.to/theme_selection/how-to-become-a-pro-front-end-developer-5gbo)
- [The psychology of web performance](https://blog.uptrends.com/web-performance/the-psychology-of-web-performance/)
- [一年一度的CSS技术趋势可视化](https://stateofcss.com/)
- [现代web魔法指南](https://github.com/dexteryy/spellbook-of-modern-webdev)
- [HOPL-IV一个研究编程语言历史的会议](https://hopl4.sigplan.org/)
- [打造舒适的前端开发环境](https://github.com/ykfe/fe-dev-playbook)
- [Build-your-own-react](https://pomb.us/build-your-own-react/)
- [TypeScript Deep Dive](https://basarat.gitbook.io/typescript/)

## 后记

对于这个Roadmap，我深知上文罗列的资源教程并不会是全网最好的，但确确实实是我自己学习的历程，大家主要可以采取和而不同的策略，拿这个做个路径参考。

而学习的时候也不要心急，因为这些看似“课外”的知识会最终与你学习的课内知识结合起来，你的知识体系是不断扩充和完善的，相信我，大二结束的时候才是你的质变。
