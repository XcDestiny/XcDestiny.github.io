# spin-the-wheel

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210331225350388.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0MjczNDI5,size_16,color_FFFFFF,t_70)


演示链接：[http://haiyongcsdn.gitee.io/spin-the-wheel/](http://haiyongcsdn.gitee.io/spin-the-wheel/)

我非常感谢您对改进它的任何反馈，我已经盯着它看了一个星期👀

我一直在为棋盘游戏玩家建立一个简单工具的网站。由于各种原因，有时您需要在线掷骰子，翻转卡片或旋转微调器。

该网站适用于诸如此类的简单操作。

我希望网站取得成功，所以我首先看了看比赛，并出于以下原因（出于我将在另一篇文章中详述的原因）决定：

 - 该网站应尽可能地易于访问
 - 任何页面都不得大于 30kB
 - 每个工具都应该有一个`no javascript` 后退

对于此工具，我们需要克服一些有趣的障碍😐😐😐：

---

## 满意请点击

旋转器在旋转时必须具有令人满意的咔嗒声，这一点很重要。

我找到了一个mp3咔嗒声🔉，但是即使在不到1秒的时间里，它仍然很大7kB。使用它会使我超出30kB预算。

我敢肯定，有很多巧妙的方法可以减小音频文件的大小。但取而代之的是，我选择使用JavaScript和Web Audio API生成点击，这是我之前从未听说过的。

幸运的是，我认识一位合成器爱好者，他向我解释了一些术语。

我找到了有关合成鼓声🥁的教程，并调整了踩example示例以使其适合。

这结束了刚刚~1.2kB的js-有空间进一步优化。

---

## 创建一个无JavaScript版本

使微调器不工作js就非常简单。

如果浏览器已禁用JavaScript，则无需提交并旋转`client-side`，请点击`spin the wheel`提交表格…

然后服务器：

 - 使用用户的自定义值构建微调器
 - 随机选择一名获胜者
 - 先进地生成使车轮旋转的css动画
 - 将html发送回客户端

它出奇地好。

我是使用Netlify Functions来完成这个工作的，我没有为一小部分将不使用js使用该网站的人运行整个服务器。

## SVG动画

虽然对SVG设置动画效果通常不错，但某些浏览器确实为此感到困扰（Safari）。经过大量的修补，事实证明，最好的解决方法是为每个动画组件使用单独的SVG，然后将每个SVG放置在自己的SVG中，然后对它们`<div>`进行动画处理`<div>`。

## 定时点击

旋转器以不同的速度，持续时间旋转，并具有随机的旋转次数-这样就保持了令人惊讶和戏剧化的效果。

为了使旋转真正令人满意，它需要在顶部有一点点滴答声🔉。（例如在游戏节目“命运之轮”上）。

这意味着在车轮的轮辋上有“销钉”，并在每次“击中”“销钉”时对自动收报器进行动画处理。

出于性能方面的考虑，我认为最好预先计算动画的时间（并单击声音）。

事实证明，这是一项非常复杂的任务，在学习了3天的微积分后，我放弃了。

相反，它使用`requestAnimationFrame`并测量微调器的当前旋转。如果旋转在特定范围之间，则会产生咔嗒声。

可以，但是确实会出错。

这也意味着该`no-javascript`版本没有自动收录器动画。

## 旋转1000s的值

一个问题是允许人们向微调器添加1000的值。

我发现有一个用例，有人可能想要粘贴整个值的电子表格以随机选择一个。

因此，我决定使用一个`<textarea>`作为输入，并为每个新值添加新行。然后，如果用户粘贴逗号分隔的列表，它将在生成转轮之前重新格式化它。

### 这里最大的问题是性能。

为了使其工作，微调器`<svg>`会随着添加更多的值而变得不那么复杂。

 - 图案被删除。
 - 车轮轮缘上的销钉数量上限为 60
 - 文本路径变得更简单。

我只在我的新电脑上测试过它，但是它可以正常工作到接近6000某个值。随时进行测试，让我知道！

---

## 还需要改善

整体外观可以通过抛光来完成，特别是在其他颜色方案上。
点击声音可能与调整有关。
找到一种精确的高级测量点击动画的方法将是惊人的。
使自定义车轮可嵌入`<iframe>`是很酷的。

欢迎关注作者微信公众号`啦啦啦好想biu点什么` 保存识别，也可在公众号上向我反馈遇到的问题，看到我会第一时间回复😁
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210123111950829.jpg#pic_center)


后面我还会持续更新类似免费好玩的H5小游戏、Java小游戏、好玩、实用的项目和软件等等

## 相关内容

 - [我是怎样制作这个逼真的红色开关的（纯CSS）](https://blog.csdn.net/qq_44273429/article/details/115148871)
 - [勇敢的兔子疯狂奔跑小游戏](https://mp.weixin.qq.com/s/h-F1Yx3LAMVKsvDpTTKjbg)
 - [基于HTML/CSS/JS的酷炫登陆注册表单](https://blog.csdn.net/qq_44273429/article/details/114490266)
 - [用HTML实现简单的下雪特效](http://mp.weixin.qq.com/s?__biz=Mzg5OTU2NTQ4MQ==&mid=2247484065&idx=1&sn=17a958a2a8df8458bf316fac55f865f3&chksm=c0501067f7279971add268cff499833f173c5d7368040c8c902da698eb72bdba23d10aca385a#rd)
 - [基于HTML/CSS/JS的动态元素周期表](https://blog.csdn.net/qq_44273429/article/details/114296024)
 - [基于HTML/CSS/JS的爱吹风的狮子小游戏](https://blog.csdn.net/qq_44273429/article/details/113792583)
 - [100个最常问的JavaScript面试问答](https://blog.csdn.net/qq_44273429/article/details/114240168)
 - [java五子棋小游戏含免费源码](https://mp.weixin.qq.com/s?__biz=Mzg5OTU2NTQ4MQ==&amp;mid=2247483719&amp;idx=1&amp;sn=8c86ff28a782c838a184a4050c695d26&amp;chksm=c0501381f7279a97df8f4467203e8de05ef7fd34f1d9be51a11c591c8e37e244cb010c55afda&token=925595149&lang=zh_CN#rd)
 - [一个炫光效果的酷炫登录表单](https://blog.csdn.net/qq_44273429/article/details/113797520)

感谢您阅读至最后❤️这是您的勋章
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210309222038491.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0MjczNDI5,size_16,color_FFFFFF,t_70)


最后，不要忘了❤️或📑支持一下哦