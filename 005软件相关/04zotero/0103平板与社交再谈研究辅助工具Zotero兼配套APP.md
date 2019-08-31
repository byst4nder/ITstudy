# Zotero（3）：平板与社交：再谈研究辅助工具Zotero兼配套APP

阳志平的网志

原文链接：[Zotero（3）：平板与社交：再谈研究辅助工具Zotero兼配套APP - 阳志平的网志](https://www.yangzhiping.com/tech/zotero3.html)

继续小白文风格，多图，高手慎入。很多亲朋好友们，都是卡在上手工具的那么几个关键小步骤。导致完全回避好的方法与工具，使用上个世纪的研究理念干苦力活。

人有时会念旧，十多年前，互联网可不是今天这样子。那时，我们谈论 R 语言、数据挖掘与文献管理，我们在谈论什么？昨晚，偶尔翻出当年担任版主的统计论坛：百岛潮统计论坛，时光如水，百岛潮当年单纯的气质已默默成为每人青春回忆中美好的一部分。

如今这个时代，知识看似垂手可得，研究日益变为职业与市场交易，研究的技术也跟随演化。继续前两天话题，将研究辅助工具再次说透彻点。这次侧重社交与移动。

## 01. 移动的Zotero

以 iPad 的 Safari 为例，安卓的 Chrome 步骤大同小异。说说如何给 Safari 增加保存到 Zotero 文献库的选项。

步骤1：随便选一个页面，将它加入到 Safari 书签。如下图所示：

步骤2，打开 iPad 的设置，找到 Safari，如下图所示，将【总是显示书签栏】给选为【on】。

步骤3，打开 iPad 的 Safari，然后触摸书签，再触摸【书签栏】，如下图所示：

步骤4：好的，关键点到了！将之前在步骤 1 保存的 Zotero，点击编辑，将网址更改为 Zotero 书签的内容。从这里复制：

http://www.zotero.org/downloadbookmarklet

如下图所示：

复制之后，效果如图：

步骤5：好的，大功告成，在平板上的 Zotero.org 网站，登陆一下你的账号，记住选择记住密码。然后随便挑选个论文库或者豆瓣读书试试看，我们会发现成功保存到云端的文献库中了。

## 02. 平板的Zotero

一般习惯用 Kindle 读电子书，读 PDF 还是习惯平板设备。主要是批注速度、阅读速度较快。以下以 iPad 为例。同样，Zotero 也支持安卓设备。

先从神器1——Notability 讲起。

Notability 是一个好评如潮的 PDF 阅读与批注 APP。如何与 Zotero 配合？关键点在第一篇文中的小技巧，将 Zotero 的条目与存储相互分离。Zotero 的数据库仅仅用于保存条目等；而 Dropbox 用于保存 PDF、网页快照等。

这么一来，通过 Notability 自带的与 Dropbox 自动同步的功能，就可以非常愉快与轻松地使用手指或者触控笔手写笔记。

步骤如下，

步骤1：购买并安装 Notability

它非常物美价廉。安装后打开。

步骤2：在 dropbox 下面的 zotero 目录创建 note 目录

请记住！在老文文献管理软件 Zotero 基础及进阶示范，第一步建立的 Dropbox 下面的 Zotero 新建一个笔记目录！这是用来存放自动同步的笔记的，与存放 PDF、电子书、网页快照的 storage 目录平级！如下图所示：

步骤3：设置 Dropbox 自动同步

回到 Notability 的设置里面，继续在 import dropbox 里面设置。如下图所示，选择设置：

打开之后，选择自动同步选项：

一步一步往下设置，选择 Dropbox。

这是个比较关键的步骤，未来很多技巧会用到这个路径。所以，请务必选择刚才步骤 2 的[note]目录。

接下去，选择同步格式了。又是个关键步骤，一般建议选择 rtfd 格式。这是富文档格式，这样才能够直接保存录音、图片等。

步骤4：导入 Zotero 里面保存的 PDF 文档，并写批注

好的，大功告成。现在点击 import，然后导入 dropbox=>storage 目录下面的 pdf 即可。如下图所示：

导入时，会询问你是否创建新笔记，选择是即可。打开 pdf，开始阅读，并做批注。这里有很多不同于上个世纪的技巧。说三个我常用的。

1. 使用录音代替打字做笔记！尤其是给别人讲 Paper 的时候，使用录音的效果会更好！

2. 对于部分外部配套图书或者好资料，及时拍照存档。

3. 买一只电容笔 ，来在 iPad 上手写批注与笔记。

如下图所示：

神器1「Notability」与 Zotero 的配合，已经非常完美了，不过查找 PDF 的时候，还是略有不便。对于难以忍受这些不便的极端完美主义者来说，可以自己去编译或者购买开源的 iPad APP：Zotpad，它的 github 网址是：https://github.com/mronkko/ZotPad

好的，现在云端 Dropbox 会自动同步多媒体格式的笔记成功。我们解压 rtfd 格式之后，打开文档看看，如下图所示：

哈哈，直接点击播放声音，声音、笔记都在一起了！接下去的扫尾工作，可以将文档以附件形式归到相应 Zotero 的条目下。

## 03. 社交的Zotero

有很多需要拉人讨论或者实验室、课题组共享一批文献。则可以在 Zotero 里面，创建一个私密或者公开的群组。如下图所示：

## 04. 两大辅助神器：Altmetric 与 Utopia

### 1. Altmetric

刚才这个办法，仅仅是自己实验室内部的文献共享。那么，如何知道全世界关于这篇文献的意见呢？Altmetric 就是拿来干这个的！将这个页面的书签拖到 Chrome 菜单栏上即可。

http://www.altmetric.com/bookmarklet.php

---

备注：进入上面的网页后，直接按住蓝色的「Altmetric it!」，拖到浏览器顶部的菜单栏里。

---

然后，当你在网页上读到任何一篇文献，让我们 以 @Zwai 在《心智简报》中翻译的这篇文章「Is working memory training effective? A meta-analytic review」为例。它的原文地址是：http://www.ncbi.nlm.nih.gov/pubmed/22612437

打开网页，然后点击 Chrome 书签栏上，我们在前一个步骤创建的书签：Altmetric。奇迹发生了！我们获得了一个分数，以及获得了全世界主要的媒体、博客、推特们对这篇文献的讨论。它是社交媒体中非常热门的一个讨论话题。（当然，在中国并不热门）如下图所示：

然后，我们查看详情，更酷了，获得了非常深入与专业的来源分析。如下图所示：

http://altmetric.com/details.php?citation_id=770688

我们了解到，有 92 人在推特上讨论过这篇文章，有 2 位博客作者写过对它的介绍。在研究社交网站上，有 79 位 Mendeley 读者下载与阅读过这篇文献，有 1 位 CiteULike 读者下载与阅读过这篇文献。

### 2. Utopia

但是，很多 PDF 文档，事先下载好了，我们一篇一篇这么去查它的社交媒体影响力，岂不是累死？专为研究 2.0 时代而生的 Utopia 来了！同样是免费、跨平台。下载地址是：

http://getutopia.com/download.php

[Utopia Documents](http://utopiadocs.com/download)

这是个专注 PDF 阅读的神器。由老友@鍾智敏 wiswood 介绍。utopia 文档（以下简称utopia）是一款令人惊奇，免费、跨平台好软件，界面简洁优雅。它专注于学术 PDF 阅读，实现了什么？对 PDF 的笔记分享与数据加载！类似于 kindle 加载其他读者写过的批注！还可加载第三方数据，比如维基百科、生物化学数据等。同样，更酷的是，它的 online 模式，默认会将该篇 PDF 文档的社交媒体信息读取出来，比如上文 altmetric 分数。同样，不仅仅是 altmetric 分数，utopia 还会调用大量第三方高质量信息源的 API，读出它的授权信息等。

仍然以「Is working memory training effective? A meta-analytic review」这篇文献为例，我们下载过来，http://www.apa.org/pubs/journals/releases/dev-49-2-270.pdf

通过 utopia 打开：

当我们默认是 online 模式的时候，utopia 这个 PDF 阅读器会自动加载它的 altmetric 分数等信息。

当然，utopia 的功能不仅仅局限于此，它支持大量第三方高质量信息源，如它的入门文档描述一样：

http://getutopia.com/quickstart.php

感兴趣的读者，可以去阅读 utopia 团队发表的学术论文：

http://getutopia.com/publications.php

目前，《生物化学杂志》已经使用它，通过与第三方命科学的数据的协作，让每一篇论文都变得动态起来。如何与 Zotero 配套使用，将 utopia 变为默认 PDF 阅读器即可。更进一步的做法则是，调用 altmetric，整理文献。

## 05. mendeley与Zotero的同步

软件不是非此即彼的互斥关系。mendeley 善于识别 PDF 元数据信息，大量处理 PDF 时，交给它好了。mendeley 提供了 Zotero 同步功能。如下图所示：

## 小结

今天的信息日益海量，通过各类社会化过滤器，使得我们能够与同行保持更密切的联系，比如专注科学文献社交媒体影响力评估的 altmetric 等。同样，时代的变迁也正在诞生大量优秀的神器，如专注于平板笔记的神器1「Notability」与专注于学术 PDF 阅读的神器2「utopia」。这些神器，在以 Zotero 作为主文献库的配合之下，将发挥更强大的战斗力。

最新的: April 10, 2013

© 2019 阳志平. 技术来自于 Jekyll & Minimal Mistakes.