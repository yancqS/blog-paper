---
title: 去哪吃
description: 去哪吃
date: '2021-05-13'
cover: 'http://img.up-4ever.site/20210513202439.png'
top_img: 'http://img.up-4ever.site/infinity-9198993.jpg'
tags:
  - node
  - idea
categories:
  - 其他
abbrlink: aa4b38e3
---

# 去哪吃

## 背景

看到这个题目，你也许会想『啥意思，什么去哪吃，想去哪吃去哪吃呗』。说得好，关键是不知道去哪吃，纠结啊。于是，就想着可不可以搞一个每天定时告诉我去哪吃的小脚本，定时发个邮件告诉我去哪吃，距离、价格最好也能一并告诉我。正所谓：『行胜于言』，搞起:rofl:

>其实这篇文章没啥技术含量，就是挺好玩的。:stuck_out_tongue:

## 准备工作

- axios：请求库
- nodemailer：用于发送邮件

```sh
mkdir getCating
cd getCanting
npm init --yes
npm i axios nodemailer
```

## 获取周边餐馆:v:

检索周边地点主要是调用了百度地图的Web服务API。

可参考[地点检索服务](https://lbsyun.baidu.com/index.php?title=webapi/guide/webservice-placeapi)服务文档。

![](http://img.up-4ever.site/20210513192625.png)

原型区域检索API(GET请求)：`http://api.map.baidu.com/place/v2/search?query=银行&location=39.915,116.404&radius=2000&output=xml&ak=您的密钥`。具体的请求参数也可参考[地点检索服务](https://lbsyun.baidu.com/index.php?title=webapi/guide/webservice-placeapi)服务文档。

这个query参数里面唯一需要自己去获取的就是经纬度(`location`)。这个具体的获取方法如下：

```js
navigator.geolocation.getCurrentPosition(position => {
    console.log(position.coords.latitude);
    console.log(position.coords.longitude)
})
```

>这个要打开浏览器的定位服务。

具体代码如下：

```js
const request = require('axios');
const fs = require('fs');
const path = require('path');
const ak = 'xxxxxxxxxxxxxxxxxxxx';
const query = encodeURI(`query=小吃$快餐$中餐&tag=小吃&location=40.04171,116.312820086&scope=2&radius_limit=true&page_size=20&radius=500&output=json&ak=${ak}`);

request.get(`http://api.map.baidu.com/place/v2/search?${query}`)
    .then(res => {
        fs.writeFileSync(path.resolve(__dirname, 'cantingList.json'), JSON.stringify(res.data, null, '\t'));
    })
```

返回的结果如下：

![](http://img.up-4ever.site/20210513193819.png)

## 发送邮件:dizzy:

发送邮件主要用了`nodemailer`这个第三方库，使用方法可参考[usage](https://nodemailer.com/usage/)以及[内置支持的服务](https://nodemailer.com/smtp/well-known/)。

```js
const nodemailer = require('nodemailer');

const transport = nodemailer.createTransport({
    service: 'QQ',
    auth: {
        user: 'xxxxxxx@qq.com',
        //这个密码不是qq密码，而是qq邮箱的授权码
        pass: 'xxxxxxxx'
    }
})

let mailOption = {
    from: '"xxxx" <xxxxxxxx@qq.com>',
    to: ['email-list'],
    subject: '中午/晚上吃什么? 我来告诉你',
}

function send(_item) {
    transport.sendMail(Object.assign(mailOption, {
        text: `
        名称：${_item.name}
        地址：${_item.address}
        距离：${_item.detail_info.distance}米
        人均价格：${_item.detail_info.price || '暂无数据'}
        评分：${_item.detail_info.overall_rating || '暂无数据'}
        经纬度：${_item.location.lat}, ${_item.location.lng}
        电话：${_item.telephone || '暂无电话'}
        在地图中查看：${_item.detail_info.detail_url}
        `
    }), (err, info) => {
        if (err) {
            return;
        }
        console.log('Message send successfully: %s', info.messageId);
    })
}
```

看这:eyes: [qq邮箱的授权码怎么获取？](https://service.mail.qq.com/cgi-bin/help?subtype=1&id=28&no=1001256)

最后的代码：

```js
const request = require('axios');
const nodemailer = require('nodemailer');
const fs = require('fs');
const path = require('path');
const ak = 'xxxxxxxxxxxxxxx';
const query = encodeURI(`query=小吃$快餐$中餐&tag=小吃&location=40.04171,116.312820086&scope=2&radius_limit=true&page_size=20&radius=500&output=json&ak=${ak}`)

let item;

const transport = nodemailer.createTransport({
    service: 'QQ',
    auth: {
        user: 'xxxxxx@qq.com',
        pass: 'xxxxxxxx'
    }
})

let mailOption = {
    from: '"name" <xxxxxx@qq.com>',
    to: ['email-list'],
    subject: '中午/晚上吃什么? 我来告诉你',
}

function send(_item) {
    transport.sendMail(Object.assign(mailOption, {
        text: `
        名称：${_item.name}
        地址：${_item.address}
        距离：${_item.detail_info.distance}米
        人均价格：${_item.detail_info.price || '暂无数据'}
        评分：${_item.detail_info.overall_rating || '暂无数据'}
        经纬度：${_item.location.lat}, ${_item.location.lng}
        电话：${_item.telephone || '暂无电话'}
        在地图中查看：${_item.detail_info.detail_url}
        `
    }), (err, info) => {
        if (err) {
            return;
        }
        console.log('Message send successfully: %s', info.messageId);
    })
}

request.get(`http://api.map.baidu.com/place/v2/search?${query}`)
    .then(res => {
        fs.writeFileSync(path.resolve(__dirname, 'cantingList.json'), JSON.stringify(res.data, null, '\t'));
        item = res.data.results[parseInt(Math.random() * 20)]
        send(item);
    })
```

## 定时执行 :clock11:

用的`crontab`,`crontab -e`直接编辑如下：

```sh
30 11,17 * * 1-5 /usr/local/bin/node /Users/{your path}/getCanting/getCanting.js
```

## 最后

感觉挺好玩的，虽然没啥含金量，但是感觉还是有那么「yinainai」（就是一点点的意思 :dog:）收获。

好啦 就到这吧。

土遁~~~~ :wave: :wave:
