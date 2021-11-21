---
title: 【译】使用RAIL模型衡量性能
date: 2021-08-29 02:30:45
tags: 技术
index_img: https://z3.ax1x.com/2021/08/29/h8PNMn.png
---
原文：https://web.dev/rail/

RAIL是一种用户中心的性能的模型，它提供了一种衡量性能的结构。RAIL模型把用户体验分解为关键操作（例如点击、滚动、加载）并能帮助开发者对每个操作定义性能指标

RAIL代表Web App生命周期的四个不同方面：响应、动画、空闲、加载

[![h8PNMn.png](https://z3.ax1x.com/2021/08/29/h8PNMn.png)](https://z3.ax1x.com/2021/08/29/h8PNMn.png)

## 关注用户

将性能提升的重点放在用户上，下表是用户对不同性能延迟的感受指标：

| 0 to 16 ms       | 用户是非常擅长跟踪动态变化的，而且用户不喜欢不流畅的动画。动画每秒渲染 60 个新帧，用户就会认为很流畅。也就是16ms一帧，排除掉浏览器在屏幕渲染一帧的时间，app还剩大概10ms的时间去生成一帧 |
| :--------------- | :----------------------------------------------------------- |
| 0 to 100 ms      | 在这个时间窗口响应用户操作，用户会感到结果是即时的。如果超出这个时间，用户执行操作和app回应的联系就被打破了 |
| 100 to 1000 ms   | 在这个时间窗口中，发生的事情是任务执行过程中自然而然的一部分。对于大多数上网的用户来说，加载页面或改变视图是一个任务。 |
| 1000 ms or more  | 超过1s，用户就会丧失当前执行的任务的注意力                   |
| 10000 ms or more | 超过10s，，用户会感到沮丧，很可能放弃任务并不再回来          |

用户感受性能延迟是不同的，具体取决于网络条件和硬件。例如，功能强大的台式机用快速 Wi-Fi 加载站点通常会在 1 秒内完成，并且用户已经对此习以为常。在 慢速3G 的移动设备上加载站点需要更多时间，所以一般来说移动用户更具耐心，在移动设备上加载 5 秒是比较实际的目标。

## 目标和准则

在 RAIL 的上下文中，术语**目标**和**准则**有以下特定含义：

- **目标：**与用户体验相关的关键性能指标。例如，点击后100 毫秒内绘制。由于人类的感受是相对不变的，这些目标未来不大可能有变动。

- 准则：

  能帮助实现目标的建议。这些可能受限于当前的硬件和网络连接条件，因此可能会随着时间而改变。

  ## 响应（Response）50ms内处理事件

### **目标**

在 100 毫秒内完成从用户输入到初始化变动发生，让用户感觉交互是即时的。

### **准则**

- 为保证100ms内可见响应，需要app在50ms内处理用户输入事件。这个原则适用于大多数输入，比如点击按钮、切换表单控件或开启动画（不适用于触摸拖动或滚动）

- 尽管听起来违反直觉，总是立即响应用户输入并不是一个正确的做法，这100ms的窗口时间可以来做一些更重要的事，但是要注意不要阻塞用户，如果可以的话，可以在后台工作

- 对于需要50ms以上才能完成的动作，需要一直提供反馈

  ### 50 ms 还是 100 ms?

既然目标是在 100 毫秒内响应输入，为什么我们的预算只有 50 毫秒？这是因为除了处理输入之外，通常还有其他工作要做，并且这些工作占用了可接受响应的一部分时间。如果app在空闲时间内的50 毫秒块中执行工作，这意味着如果输入事件在这些工作块之一期间发生，则它最多可以排队 50 毫秒。考虑到这一点，我们可以假设只有剩余的 50 毫秒可用于实际输入处理是安全的。下图显示了这种效果，该图显示了在空闲任务期间收到的输入如何排队，从而减少了可用的处理时间：

[![h8Pvo8.png](https://z3.ax1x.com/2021/08/29/h8Pvo8.png)](https://z3.ax1x.com/2021/08/29/h8Pvo8.png)

空闲任务如何影响输入响应时间预算

## 动画（Animation）10ms内生成一帧

### **目标：**

- 在 10 毫秒或更短的时间内生成动画中的每一帧。从技术上讲，每帧的最大预算为 16 毫秒（1000 毫秒/每秒 60 帧≈16 毫秒），但浏览器需要大约 6 毫秒来渲染每帧，因此每帧 10 毫秒的准则。

- 以视觉流畅度为目标。用户会注意到帧率发生变化。

  ### **准则：**

- 在像动画这样的高压点上，关键是在你能做的地方什么都不做，在你不能做的地方绝对最少。只要有可能，就利用 100 毫秒响应预先计算昂贵的工作，以便最大限度地提高达到 60 fps 的机会。

- 有关各种动画优化策略，请参阅[渲染性能](https://developers.google.com/web/fundamentals/performance/rendering)。

要认清动画的种类。动画不仅仅是花哨的 UI 效果。这些交互中的每一个都被认为是动画：

- 视觉动画，例如出入页面、[补间动画](https://www.webopedia.com/TERM/T/tweening.html)和加载指示器。

- 滚动。这包括滑动事件，即用户开始滚动，然后放手，页面继续滚动。

- 拖拽。动画通常遵循用户交互，例如拖拽地图或捏合缩放。

  ## 空闲（Idle）最大化空闲时间

### **目标**

最大化空闲时间以增加页面在 50 毫秒内响应用户输入的几率。

### **准则**

- 利用空闲时间完成延期工作。例如，对于页面初加载，要加载尽可能少的数据，然后使用[空闲时间](https://developer.mozilla.org/en-US/docs/Web/API/Window/requestIdleCallback)加载其余的数据。

- 在 50 毫秒或更短的空闲时间内执行工作。如果时间更长，就有干扰应用程序在 50 毫秒内响应用户输入的风险。

- 如果用户在空闲时间与页面交互，则用户交互应始终具有最高优先级并中断空闲时间正在进行的工作。

  ## 加载（Loading）交付内容在5s内可互动

如果页面加载缓慢，用户注意力会转移，或认为任务已失败。加载速度快的网站具有[更长的平均会话时间、更低的跳出率和更高的广告可见度](https://www.thinkwithgoogle.com/intl/en-154/insights-inspiration/research-data/need-mobile-speed-how-mobile-latency-impacts-publisher-revenue/)。

### **目标**

- 优化与用户的设备和网络能力以快速加载性能。目前，首次加载的一个很好的目标是加载页面并在[5 秒或更短的时间内在 3G 连接速度较慢的中端移动设备上](https://web.dev/performance-budgets-101/#establish-a-baseline)进行[交互](https://web.dev/interactive/)。
- 对于后续加载，在 2 秒内加载页面比较好

**PS: 以上目标可能随时间改变**

### **准则**

- 在常见的用户移动设备和网络连接上测试App的负载性能。[Chrome 用户体验报告](https://web.dev/chrome-ux-report/)可以帮助了解用户的[连接分布](https://web.dev/chrome-ux-report-data-studio-dashboard/#using-the-dashboard)。如果网站的数据不可用，[GSMA - The mobile ecomomy](https://www.gsma.com/mobileeconomy/)建议中位线是中端 Android 手机，例如 Moto G4 的慢速 3G 网络（定义为 400 ms RTT 和 400 kbps 传输速度）。此组合在[WebPageTest](https://www.webpagetest.org/easy)上[可用](https://www.webpagetest.org/easy)。

- 需要记住，尽管大部分有代表性的移动用户设备可能声称它使用的是 2G、3G 或 4G 连接，但实际上，由于数据包丢失和网络差异，[有效连接速度](https://web.dev/adaptive-serving-based-on-network-quality/#how-it-works)通常要慢得多。

- [消除渲染阻塞资源](https://web.dev/render-blocking-resources/)。

- 不必在 5 秒内加载所有内容才产生完整加载的感觉。考虑[延迟加载图像](https://web.dev/browser-level-image-lazy-loading/)、[代码拆分 JavaScript 包](https://web.dev/reduce-javascript-payloads-with-code-splitting/)和[web.dev 上建议的](https://web.dev/fast/)其他[优化](https://web.dev/fast/)。

  ### 影响页面加载性能的因素

- 网络速度和延迟

- 硬件（例如，较慢的 CPU）

- Cache Eviction

- L2/L3 缓存的差异

- JavaScript解析

  ## 衡量RAIL的工具们

### 有一些工具可以帮助您自动执行 RAIL衡量：

Chrome DevTools

[Chrome DevTools](https://developers.google.com/web/tools/chrome-devtools)对页面加载或运行时发生的一切进行深入分析。请[参阅开始分析运行时性能](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance)来熟悉Performance面板 UI。

以下是 DevTools 提供的专门功能：

- [限制 CPU](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/reference#cpu-throttle)以模拟功能较弱的设备。

- [限制网络](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/reference#network-throttle)以模拟较慢的连接。

- [查看主线程活动](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/reference#main)以查看录制时主线程上发生的每个事件。

- [﻿查看表中的主线程活动](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/reference#activities)根据占用时间最多的活动对活动进行排序。

- [分析每秒帧数 (FPS)](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/reference#fps)以衡量您的动画是否真正流畅运行。

- [使用](https://developers.google.com/web/updates/2017/11/devtools-release-notes#perf-monitor)Performance Monitor实时[监控 CPU 使用率、JS 堆栈大小、DOM 节点、每秒布局等](https://developers.google.com/web/updates/2017/11/devtools-release-notes#perf-monitor)

- 使用网络部分[可视化](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/reference#network)录制时发生的[网络请求](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/reference#network)。

- [在录制时捕获屏幕截图](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/reference#screenshots)以准确回放页面加载时页面的外观，或动画触发等。

- [查看交互](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/reference#interactions)以快速识别用户与其交互后页面上发生的情况。

- 通过在潜在问题侦听器触发时突出显示页面[来实时查找滚动性能问题](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/reference#scrolling-performance-issues)。

- 实时查看绘制事件

  以识别可能损害动画性能的代价高昂的绘制事件。

  ### Lighthouse

[Lighthouse](https://developers.google.com/web/tools/lighthouse)可在 Chrome DevTools、[web.dev/measure](https://web.dev/measure/)、Chrome 扩展、Node.js 模块和 WebPageTest 中使用。它获取一个 URL，然后模拟一个 3G 连接速度较慢的中端设备，在页面上运行一系列检查，然后给你一份关于负载性能的报告，以及如何改进的建议。

以下检查需要特别关注：

#### 响应

- [Max Potential First Input Delay (FID) ](https://web.dev/lighthouse-max-potential-fid/)根据主线程空闲时间估计您的应用响应用户输入所需的时间。

- [没有用被动listener来提高滚动性能](https://web.dev/uses-passive-event-listeners/)。

- [总阻塞时间](https://web.dev/lighthouse-total-blocking-time/)。测量页面被阻止响应用户输入（例如鼠标点击、屏幕点击或按下键盘）的总时间。

- [TTI](https://developers.google.com/web/tools/lighthouse/audits/consistently-interactive)，衡量用户何时可以始终如一地与所有页面元素进行交互。

  #### 加载

- [未注册service workerk控制页面](https://web.dev/service-worker/)。Service Worker 可以缓存用户设备上的公共资源，从而减少通过网络获取资源所花费的时间。

- [移动网络上的页面加载速度不够快](https://web.dev/load-fast-enough-for-pwa/)。

- [消除渲染阻塞资源](https://developers.google.com/web/tools/lighthouse/audits/blocking-resources)。

- [推迟屏幕外图像](https://web.dev/offscreen-images/)。推迟加载屏幕外图像，直到需要它们。

- [适当大小的图像](https://web.dev/uses-responsive-images/)。不要提供明显大于移动视口中呈现的尺寸的图像。

- [避免链式关键请求](https://web.dev/critical-request-chains/)。

- [不为所有资源使用 HTTP/2](https://web.dev/uses-http2/)。

- [高效图片编码](https://web.dev/uses-optimized-images/)。

- [启用文本压缩](https://web.dev/uses-text-compression/)。

- [避免巨大的网络负载](https://web.dev/total-byte-weight/)。

- [避免过大的 DOM ](https://web.dev/dom-size/)。通过仅传送呈现页面所需的 DOM 节点来减少网络字节。

  ### WebPageTest

WebPageTest 是一个 Web 性能工具，它使用真实的浏览器来访问网页并收集计时指标。在[webpagetest.org/easy](https://webpagetest.org/easy)上输入一个URL，可以获取页面在慢速3G连接的真实Moto G4 设备上的负载性能的详细报告。它可以将其配置包含 Lighthouse审计报告。

## 总结

用户体验可以看成是一连串交互组成的，RAIL是一个用于了解这个旅程的透镜。它能了解用户是如何感受网站的，以便开发者设定对用户体验影响最大的性能目标。

- 以用户为中心。
- 在 100 毫秒内响应用户输入。
- 动画或滚动时，在 10 毫秒内生成一帧。
- 最大化主线程空闲时间。
- 在 5000 毫秒内加载交互式内容。