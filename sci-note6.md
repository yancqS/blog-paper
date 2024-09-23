---
title: 转载-节选-科技爱好者周刊节选（173期）
date: '2021-08-28'
cover: 'http://img.up-4ever.site/20210828145215.jpeg'
top_img: 'http://img.up-4ever.site/infinity-9198993.jpg'
tags:
  - 趣谈
  - 转载
categories:
  - 其他
abbrlink: fd41ae4c
---
# 科技爱好者周刊（第 173 期）：网络收音机的设计

## 前言

本文节选自阮一峰老师的《科技爱好者周刊》第173期。

《周刊》链接：**[科技爱好者周刊173期](https://www.ruanyifeng.com/blog/2021/08/weekly-issue-173.html)**

## 正文

*科技动态* 部分

- [Facebook 虚拟会议软件](https://www.axios.com/facebook-vr-zuckerberg-meetings-7b8b78fb-5345-42a6-a338-2abe19079c9d.html)

Facebook 公司推出虚拟会议软件 Horizon Workrooms，将虚拟会议提升到一个新的境界。

![](http://img.up-4ever.site/20210828132336.jpeg)

只要用户戴上 VR 头盔，打开软件，就会进入一间虚拟会议室。软件会自动为你的分身配上脸部动作和表情，同时使用 VR 头盔的摄像头，捕捉你双手的动作，让软件里面的分身也做出同样的动作。

比如，如果你在打字，虚拟空间里面也会显示你的分身在打字。

与会者可以在虚拟白板上进行协作，也可以共享照片、文档和电脑屏幕。Facebook 放出了马克·扎克伯格虚拟会议的[演示视频](https://v.qq.com/x/page/f3269m4u87c.html)，他的表情和双手的动作，以及背后白板上的板书，令人印象深刻。

<iframe frameborder="0" width="100%" height="460px" src="https://v.qq.com/txp/iframe/player.html?vid=f3269m4u87c" allowFullScreen="true"></iframe>

- [电子驾照](http://china.qianlong.com/2021/0820/6179805.shtml)

今年9月1日开始，公安部在全国28个城市，全面推行电子驾照，效力与纸质驾照完全相同。电子驾照通过"交管12123"APP发放，领取后只要携带手机，就不需要携带纸质驾照了。

电子驾照下方还有实时生成的二维码和条形码信息，交警通过扫描二维码，能够快速获取车主的身份证信息和车辆登记信息。

- [VR 透视头盔](https://www.sohu.com/a/481676198_255990)

随着头戴显示装置越来越普及，逐渐浮现出一个问题。戴头盔的人可以看到外部，但是外部的人无法看到戴头盔者的脸，双方无法进行面对面的交流。

Facebook 的研究人员想出了一个解决方法，就是在头盔外部的眼睛位置，放置显示屏，实时展示戴头盔者的眼部表情和动作。未来，人们有可能彼此戴着头盔，当面交流。

![](http://img.up-4ever.site/20210828133144.jpeg)

> 确定这样面对面交流 晚上不害怕吗 🐶

---

*文章* 部分:

- [man 命令的奇怪输出](https://unix.stackexchange.com/questions/405783/why-does-man-print-gimme-gimme-gimme-at-0030)（英文）

![](http://img.up-4ever.site/20210828133719.jpeg)

有人发现，如果在半夜 00:30 分执行`man`命令，会得到一条奇怪的报错信息"gimme gimme gimme"。

他在网上提问：有人知道这是怎么回事吗？

被采纳的答案指出是`man`命令的彩蛋。

![](http://img.up-4ever.site/20210828134050.png)

- [CSS 检测支持悬停的设备](https://css-irl.info/detecting-hover-capable-devices/)（英文）

![](http://img.up-4ever.site/20210828141443.jpeg)

鼠标有悬停（hover）事件，移动设备由于不使用鼠标，自然就没有这个动作。因此就可以通过它判断，用户是否正在使用移动设备。

本文介绍如何使用 CSS 来侦测支持悬停事件的设备，并可以设置特殊的样式。

- [CSS transform 属性详解](https://www.joshwcomeau.com/css/transforms/)（英文）

![](http://img.up-4ever.site/20210828142212.jpeg)

一篇写得很好的 CSS 入门教程，介绍如何使用 transform 属性，实现网页元素的变形。文中有大量互动式示例。

---

*工具* 部分

- [Web Captioner](https://webcaptioner.com/)

![](http://img.up-4ever.site/20210828142601.jpeg)

一个网页的语音转文字工具，使用 Chrome 浏览器的 Web Speech API，支持中文，[代码](https://github.com/bensonruan/Chrome-Web-Speech-API)开源，这里还有一篇[介绍文章](https://bensonruan.com/voice-to-text-with-chrome-web-speech-api/)。

> 好玩~~，开启了新世界的大门

---

*资源* 部分

- [物联网 IoT 初学者教程](https://github.com/microsoft/IoT-For-Beginners)

![](http://img.up-4ever.site/20210828144836.jpeg)

微软发布的英文教程，详细介绍什么是物联网，以及物联网的应用。

---

*图片* 部分

- [人工智能图像](https://ml.berkeley.edu/blog/posts/clip-art/)

人工智能公司 OpenAI 最近推出了 CLIP 模型，经过训练以后，可以根据用户提供的文字描述，自动生成图像。

研究人员已经用它生成了许多非常酷的图像，有些堪称是艺术杰作。如果不说明，你根本想象不到这是软件生成的。

下面就是其中一些作品，标题就是给出的文字描述，图像是软件返回的结果。

《城市的夜》

![](http://img.up-4ever.site/20210828145052.jpeg)

《起风了》

![](http://img.up-4ever.site/20210828145115.jpeg)

《山顶的暴风雪中有一个小木屋，黄昏时亮着一盏灯》

![](http://img.up-4ever.site/20210828145142.jpeg)

《午夜，山中的房子，飞舞的萤火虫（使用吉卜力工作室的风格）》

![](http://img.up-4ever.site/20210828145215.jpeg)

---

*文摘* 部分

- **门把手的设计**

一扇门既可以向外拉，也可以向里推，人们经常搞错。应该怎么设计门把手，才能使大家一眼就看出来，到底应该推还是拉，而不会搞错？

一个最简单的方法，就是在门上贴文字说明，写明应该"拉"或"推"。

但是，这种方法需要用户先看到文字标签，而且对于外国用户，或者不能理解说明的人就没有用。那么能不能不使用文字说明，只凭人类直觉来区分"推"和"拉"呢？

《设计心理学》的作者建议，为了解决这个问题，可以约定，竖的门把手表示拉，横的门把手表示推。

![](http://img.up-4ever.site/20210828145530.jpeg)

或者，对于需要推的门，索性取消门把手，表示这个门只能推。

![](http://img.up-4ever.site/20210828145636.jpeg)

但是，作者也认为，这些设计都不是特别理想。门把手的推拉设计，是一个很难的问题，目前还没有完美的解决方案。

他开玩笑说，有一回他用绳子当作家里的门把手，表示这扇门只能拉，这个设计可能是最好的设计。

![](http://img.up-4ever.site/20210828145707.jpeg)

> 讲真的，最后一个设计真的让我明白了一个道理：less is more。这个设计确实绝了。佩服

---

*言论* 部分

- 你的代码应该是写给下一个开发者的情书。		-- [AgriWebb](https://www.agriwebb.com/au/your-code-should-be-a-love-letter-to-the-next-developer/)

## 最后

感谢阮一峰老师的《周刊》

love & peace :kissing_heart:

最喜欢言论：你的代码应该是写给下一个开发者的情书。