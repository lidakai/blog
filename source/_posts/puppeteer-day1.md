---
title: 高级爬虫--Puppeteer初探
date: 2018-04-10 18:56:55
tags: 
    - NodeJs
cover: http://www.alibibi.top/image/lm.jpg
---
### 首先介绍Puppeteer
* Puppeteer是一个node库，他提供了一组用来操纵Chrome的API，理论上使用它可以做任何Chrome可以做的事
* 有点类似于PhantomJS，但Puppeteer由Chrome官方团队进行维护，前景更好
* Puppeteer的应用场景会非常多，就爬虫领域来说，远比一般的爬虫工具功能更丰富，性能分析、自动化测试也不在话下，今天先探讨爬虫相关
* [Puppeteer官方文档请猛戳这里](https://github.com/GoogleChrome/puppeteer/blob/master/docs/api.md#puppeteerlaunchoptions)

### Puppeteer 核心功能
1. 利用网页生成PDF、图片
2. 爬取SPA应用，并生成预渲染内容（即“SSR” 服务端渲染）
3. 可以从网站抓取内容
4. 自动化表单提交、UI测试、键盘输入等
5. 帮你创建一个最新的自动化测试环境（chrome），可以直接在此运行测试用例
6. 捕获站点的时间线，以便追踪你的网站，帮助分析网站性能问题

## 基本熟悉之后，接下来进行Puppeteer的爬虫教学：
1. 运行Puppeteer
```
puppeteer.launch().then(async browser => {
  ......
  what you want
  ......
})
```
2. 跳转至 [阮一峰老师的ES6博客](http://es6.ruanyifeng.com/#README)
```
let page = await browser.newPage();
await page.goto('http://es6.ruanyifeng.com/#README');
```
3. 分析博客左侧导航栏的dom结构，并拿到所有链接的href、title信息
```
let as = [...document.querySelectorAll('ol li a')];
return as.map((a) =>{
    return {
      href: a.href.trim(),
      name: a.text
    }
});
```
4. 使用Puppeteer打印当前页面的PDF
```
await page.pdf({path: `./es6-pdf/${aTags[0].name}.pdf`});
```
5. 完整代码在: [https://github.com/zhentaoo/puppeteer-deep](https://github.com/zhentaoo/puppeteer-deep)
6. 项目运行
* git clone https://github.com/zhentaoo/puppeteer-deep
* npm install (puppeteer在win下100+M、mac下70+M，请耐心等候)
* npm run es6

## 最终效果如下，不过要注意几个问题：
1. 如果在page go之后马上进行pdf抓取，此时页面还未完成渲染，只能抓到loading图（如下），所以需要用timeout做点简单处理
![示例](http://www.zhentaoo.com/img/puppeteer.png "示例")
2. 最终爬取效果如下，PDF的尺寸、预览效果、首页重复就不做过多整理， 预览效果如下,如果想要自己处理，可以设置一下chrome尺寸，打印页数
![示例](http://www.zhentaoo.com/img/es6-pdf.png "示例")
![示例](http://www.zhentaoo.com/img/es6.png "示例")

##### 最后声明，生成的PDF很粗糙，应该不会对阮老师产生什么影响，如有问题可以第一时间联系我….


ps:
文章来源自[javascript瞭望塔](http://www.zhentaoo.com/2017/08/17/Puppeteer/)