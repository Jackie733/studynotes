# Web API

### JavaScript,API和其他JavaScript工具之间的关系

- JavaScript — 一种内置于浏览器的高级脚本语言，您可以用来实现Web页面/应用中的功能。
- 客户端API — 内置于浏览器的结构程序，位于JavaScript语言顶部，使您可以更容易的实现功能。
- 第三方API — 置于第三方普通的结构程序（例如Twitter，Facebook），使您可以在自己的Web页面中使用那些平台的某些功能（例如在您的Web页面显示最新的Tweets）。
- JavaScript库 — 通常是包含具有[特定功能](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Functions#Custom_functions)的一个或多个JavaScript文件，把这些文件关联到您的Web页以快速或授权编写常见的功能。例如包含jQuery和Mootools
- JavaScript框架 — 从库开始的下一步，JavaScript框架视图把HTML、CSS、JavaScript和其他安装的技术打包在一起，然后用来从头编写一个完整的Web应用。

### 常见浏览器API

- **操作文档的API**内置于浏览器中。最明显的例子是[DOM（文档对象模型）](https://developer.mozilla.org/zh-CN/docs/Web/API/Document_Object_Model)API，它允许您操作HTML和CSS — 创建、移除以及修改HTML，动态地将新样式应用到您的页面，等等。每当您看到一个弹出窗口出现在一个页面上，或者显示一些新的内容时，这都是DOM的行为。 您可以在在[Manipulating documents](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Client-side_web_APIs/Manipulating_documents)中找到关于这些类型的API的更多信息。
- **从服务器获取数据的API** 用于更新网页的一小部分是相当好用的。这个看似很小的细节能对网站的性能和行为产生巨大的影响 — 如果您只是更新一个股票列表或者一些可用的新故事而不需要从服务器重新加载整个页面将使网站或应用程序感觉更加敏感和“活泼”。使这成为可能的API包括[`XMLHttpRequest`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest)和[Fetch API](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API)。您也可能会遇到描述这种技术的术语**Ajax**。您可以在[Fetching data from the server](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Client-side_web_APIs/Fetching_data)找到关于类似的API的更多信息。
- **用于绘制和操作图形的API**目前已被浏览器广泛支持 — 最流行的是允许您以编程方式更新包含在HTML<canvas>元素中的像素数据以创建2D和3D场景的[Canvas](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API)和[WebGL](https://developer.mozilla.org/en-US/docs/Web/API/WebGL_API)。例如，您可以绘制矩形或圆形等形状，将图像导入到画布上，然后使用Canvas API对其应用滤镜（如棕褐色滤镜或灰度滤镜），或使用WebGL创建具有光照和纹理的复杂3D场景。这些API经常与用于创建动画循环的API（例如[`window.requestAnimationFrame()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/requestAnimationFrame)）和其他API一起不断更新诸如动画和游戏之类的场景。
- **音频和视频API**例如[`HTMLMediaElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLMediaElement)，[Web Audio API](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Audio_API)和[WebRTC](https://developer.mozilla.org/zh-CN/docs/MDN/Doc_status/API/WebRTC)允许您使用多媒体来做一些非常有趣的事情，比如创建用于播放音频和视频的自定义UI控件，显示字幕字幕和您的视频，从网络摄像机抓取视频，通过画布操纵（见上），或在网络会议中显示在别人的电脑上，或者添加效果到音轨（如增益，失真，平移等） 。
- **设备API**基本上是以对网络应用程序有用的方式操作和检索现代设备硬件中的数据的API。我们已经讨论过访问设备位置数据的地理定位API，因此您可以在地图上标注您的位置。其他示例还包括通过系统通知（参见[Notifications API](https://developer.mozilla.org/zh-CN/docs/Web/API/Notifications_API)）或振动硬件（参见[Vibration API](https://developer.mozilla.org/zh-CN/docs/Web/API/Vibration_API)）告诉用户Web应用程序有用的更新可用。
- **客户端存储API**在Web浏览器中的使用变得越来越普遍 - 如果您想创建一个应用程序来保存页面加载之间的状态，甚至让设备在处于脱机状态时可用，那么在客户端存储数据将会是非常有用的。例如使用[Web Storage API](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Storage_API)的简单的键 - 值存储以及使用[IndexedDB API](https://developer.mozilla.org/zh-CN/docs/Web/API/IndexedDB_API)的更复杂的表格数据存储。

### 常见第三方API

- The [Twitter API](https://dev.twitter.com/overview/documentation), 允许您在您的网站上展示您最近的推文等。
- The [Google Maps API](https://developers.google.com/maps/) 允许你在网页上对地图进行很多操作（这很有趣，它也是Google地图的驱动器）。现在它是一整套完整的，能够胜任广泛任务的API。其能力已经被[Google Maps API Picker](https://developers.google.com/maps/documentation/api-picker)见证。
- The [Facebook suite of API](https://developers.facebook.com/docs/) 允许你将很多Facebook生态系统中的功能应用到你的app，使之受益，比如说它提供了通过Facebook账户登录、接受应用内支付、推送有针对性的广告活动等功能。
- The [YouTube API](https://developers.google.com/youtube/), 允许你将Youtube上的视频嵌入到网站中去，同时提供搜索Youtube，创建播放列表等众多功能。
- The [Twilio API](https://www.twilio.com/), 其为您的app提供了针对语音通话和视频聊天的框架，以及从您的app发送短信息或多媒体信息等诸多功能。