---
title: 转载-节选-科技爱好者周刊节选（171期）
date: '2021-08-13'
cover: 'http://img.up-4ever.site/20210813163942.jpeg'
top_img: 'http://img.up-4ever.site/infinity-1760239.jpg'
tags:
  - 趣谈
  - 转载
categories:
  - 其他
abbrlink: 6adebf65
---
# 科技爱好者周刊（第 171 期）：云服务流量有多贵？

## 前言

本文节选自阮一峰老师的《科技爱好者周刊》第171期。

《周刊》链接：[科技爱好者周刊171期](https://www.ruanyifeng.com/blog/2021/08/weekly-issue-171.html)

## 正文

*云服务流量有多贵* 部分结论：

小网站尽量不要托管在大型云服务商那里，流量费用实在吃不消。如果不得不放在上面，那就必须设法降低或转移流量，比如使用 CDN。

*科技动态* 部分：

- [本田电动平衡车](https://www.yankodesign.com/2021/07/22/this-self-balancing-honda-electric-scooter-redefines-futuristic-urban-commuting/)

日本摩托车厂商本田公司，推出了全新设计的单轮电动平衡车，令人耳目一新。这种车比摩托车成本更低、更节省空间。

![](http://img.up-4ever.site/20210813160250.jpeg)

![](http://img.up-4ever.site/20210813160342.jpeg)

> 别说这小车挺中我意的~~

- [人工智能的读图会意](https://zhuanlan.zhihu.com/p/398939467)

![](http://img.up-4ever.site/20210813163031.jpeg)

人工智能在图像学的应用，取得了新进展。

8月12日，阿里巴巴达摩院研发的 AliceMind 系统（已在 [GitHub](https://github.com/alibaba/AliceMind) 开源），在2021国际 VQA 挑战赛中，获得榜首成绩后，再次刷新了 VQA 得分纪录，首次超越人类的分数。

所谓 VQA（Visual Question Answering） 指的是视觉问答，简单说，就是根据图片回答问题，属于视觉-文本推理，也叫做"读图会意"，是更高层次的"图像识别"。

请看下图，图像识别工具会认出是一架飞机，而视觉问答工具则会尝试理解这张图片：它是什么型号的飞机？哪一年制造的？属于哪个国家？用什么材料做的？......

![](http://img.up-4ever.site/20210813163433.jpeg)

VQA 技术有着广阔的应用，可以使得图片搜索质量上一个台阶，并且在医疗诊断、智能驾驶等领域有着巨大的实用价值。据说，阿里巴巴内部已经把它用于店铺的智能客服，从商品图片提取信息，自动回答用户的咨询。

- [DIY 笔记本](https://frame.work/products/laptop-diy-edition)

美国 Framebook 公司推出了世界第一款 DIY 笔记本，用户收到的就是一堆零件，需要自己组装成一台 13.5寸的笔记本电脑。

![](http://img.up-4ever.site/20210813163942.jpeg)
![](http://img.up-4ever.site/20210813163943.jpeg)

![](http://img.up-4ever.site/20210813164104.jpeg)

> 这帮歪果仁脑洞是真滴大，但是我喜欢~~

*文章* 部分：

- [音频可视化入门](https://cjting.me/2021/08/07/fourier-transform-and-audio-visualization/)（中文）

作者介绍了音频处理的基本知识，如何进行采样，并通过傅立叶变换分离频率，最后实现音乐播放器的可视化音柱跳跃效果。

> 真不错~~

- [JavaScript 的浮点数格式](https://github.com/trekhleb/javascript-algorithms/tree/master/src/algorithms/math/binary-floating-point)（英文）

本文介绍了 JavaScript 的浮点数格式，JS 引擎底层到底是怎么存储一个数字的，图文并茂。

*工具* 部分：

- [Muse](https://www.muse.place/)

![](http://img.up-4ever.site/20210813174853.jpeg)

Muse 是一个无代码的网站生成器，但是生成的是 3D 虚拟网站。

这个工具还没有做完，现有的部分已经很酷了，全流程都是 3D 场景。可以裸眼使用，也可以配合 VR 眼镜，3D 网站的未来前景非常光明。

- [MacType](https://mactype.net/)

这个工具将 Mac 电脑的字体渲染方法，移植到 Windows 电脑。

> 先存一下，感觉有用~~

*资源* 部分：

- [FastAI book](https://github.com/fastai/fastbook)

![](http://img.up-4ever.site/20210813182428.jpeg)

FastAI 是一个深度学习库，这是它的教程书，介绍深度学习的基础知识，以及如何用这个库完成人工智能项目。

*言论* 部分：

- 抖音的推荐算法有一个最大的优势，就是它的内容很短，用户的观看历史以秒计算。用户使用一个小时，可以产生几百条观看记录。Netflix 需要等待一个月，才能获得相同数量的用户观看记录。

- 经常有人问我："未来 10 年会发生什么变化？" 但是，几乎从没人问我："未来 10 年什么不会改变？" 第二个问题实际上更重要，因为只有基于稳定的事物，我们才能制定业务战略。			-- 贝佐斯，亚马逊老板
- 我担心未来的技术会让美国的电影人以数字方式复制我的动作，再套上任何演员的脸。我一生都在训练，我们武者只会越来越老，他们却可以永远拥有我的动作，变成他们的知识产权，所以我拒绝与他们合作，不让他们使用数字技术捕捉我的动作。		-- [李连杰](https://www.scmp.com/abacus/culture/article/3028904/jet-li-says-he-rejected-matrix-because-he-didnt-want-his-kung-fu)，谈为什么拒绝出演《黑客帝国》续集

## 最后

感谢阮一峰老师的《周刊》，技术进步的速度总让人瞠目结舌~~

love & peace :kissing_heart:

PS: 最佳言论：经常有人问我："未来 10 年会发生什么变化？" 但是，几乎从没人问我："未来 10 年什么不会改变？" 第二个问题实际上更重要，因为只有基于稳定的事物，我们才能制定业务战略。

