title: 【译】Fog Creek采访：Peteris Krumins
tags:
  - Fog Creek
  - Peteris Krumins
id: 420
categories:
  - 翻译
date: 2015-04-08 19:36:17
---

*   译注1：在我刚开始写[博客](http://www.legendtkl.com)的时候，发现了Peteris Krumins，特别高产的博主。博客地址是：[www.catonmat.net](http://www.catonmat.net)。我给他发了一封邮件，希望可以将他的一篇博文翻译成中文，他很开心的答应了。前段时候Peteris来信问我，有没有兴趣再翻译一篇。于是有了这篇译文。
*   译注2：关于Fog Creek公司。Fog Creek是一家致力于项目管理工具的公司。我所知道的Trello就是出自这家公司。让我印象比较深刻的还有办公环境，说其好于google也不为过。不信你去搜索一下。
*   注3：Fog Creek会定期对一些开发人员进行访谈。下面是采访译文。<!--more-->

* * *

![peter](http://ww4.sinaimg.cn/mw690/7cc829d3gw1eqz7p4ul34j203c03caa1.jpg)

在dev.life栏目中，我们会和一些开发者聊聊关于他们的编程热情：他们怎么喜欢上编程，以及喜欢什么样的工作。

本文的嘉宾是 [Peteris Krumins](https://twitter.com/pkrumins)，他是 [Browserling](https://www.browserling.com/) 公司的联合创始人兼 CEO。Browserling是一个网站多浏览器测试的在线工具，汇集了主流浏览器的网站，可以帮助网页设计师测试网站在不同浏览器下的兼容性。同时他还是书籍《Perl One-Liners》的作者。他经常在博客上写一些关于软件开发的文章。

**Fog Creek：你是怎么进入软件开发行业的？**

**Peteris：**我在6岁的时候第一次接触计算机，当时在妈妈工作的地方看到了一个386或者486的计算机。当时我就被深深吸引了。之后我就一起幻想能有一台自己的电脑。我试了很多方法，最后从一些朋友那儿得到了一台。为了蹭网，我甚至假装是大学生，尽管我还是一个小孩子。

我最开始从一个叫Zombie的朋友那儿得到了一台笔记本电脑。他是一个了不起的系统管理员，将一台闲置的笔记本送给我了。我现在还保存着：IBM Butterfly 笔记本（ThinkPad 701CS），8M内存，800M硬盘，我最开始用来跑Windows 95和OpenBSD。后来我将内存升级成了40M。它还有细缆PCMCIA网卡，我家里的网络最开始是10Mb/s。

![](http://ww1.sinaimg.cn/mw690/7cc829d3gw1eqz7p66mrkj20ju05tdi7.jpg)

再后来我在15岁的时候拥有了自己的计算机。当时它有着很不错的配置：400Mhz赛扬处理器，256M内存，8G硬盘驱动器，16M 3D Blaster Banshee显卡，17寸75Hz，分辨率1024×768的CRT显示器，原厂配置Windows 98操作系统。

计算机知识我是完全自学的。刚开始我学习了各种编程语言。当我拿到我赛扬计算机的时候，我已经知道自己想要做什么了。我想要做网页，于是开始学习HTML、JavaScript和CSS。最开始我并不知道网站是怎么工作的以及服务器相关的知识，一段时间才明白我需要一个web服务器去运行我的网站。我先是将我的网站托管在Angelfire上，后来用PHP和MySQL搭建了自己的Linux Slackware web服务器。我也想做一名黑客，所以我还学习了C和汇编语言。另外我花了很多时间在IRCNet上，所以我还学习了mIRC脚本并用VB搭建了自己的IRC客户端。（注：IRC是Internet Relay Chat的英文缩写，中文一般称为互联网中继聊天。由芬兰人JarkkOikarinen于1988年首创的一种网络聊天协议。mIRC是英国mIRC公司出品的IRC类客户端软件。mIRC脚本是其集成的脚本语言。）

**Fog Creek：谈一谈你现在的工作。**

**Peteris：**我现在是Browserling公司的CEO。2011年，我和一个朋友在旧金山湾区成立了Browerling公司。现在我每天都会写很多代码，管理服务器，和客户与雇员们工作。我很喜欢我现在的工作，实在无法想像如果我从事别的工作会是什么样。另外我还是Paul Graham的死粉，也是他的文章激励我创业。（注：Paul Graham就是特别火的《黑客与画家》的作者，没有读过的朋友，建议看一看。）

我现在全身心致力于扩展业务。我停止了所有副业、写书等任何可能分散我注意力的事情。创业成功的首要原则就是要百分百地投入。我现在已经实现了营收增长，并且正在建一个国际化远程的开发团队。我刚招聘了一个乌克兰的牛逼工程师，并向非英语市场上扩展业务。

我也解决了一些技术问题，比如如何将虚拟机上运行的浏览器分配给不同的客户，如何每秒钟抓取数以百计的浏览器截图。我喜欢服务器相关的工作，并且我打算将服务器从EC2和Rackspace云上迁到物理机上。云服务器是很适合初创公司，但是随着发展壮大，服务器迁到自己的地盘还是很有必要的：可以节省开支并提高性能。（注：EC2是Amazon云。Rackspace是全球三个云厂商之一。）

**Fog Creek:：什么时候是你最开心的编程时刻？**

**Peteris：**当我工作状态极好和搞定问题的时候是最开心的。我经常能进入状态，打算分享一下。晚上工作白天休息。晚上时间干扰很少能让你保持专心。另外一个秘诀是关闭Twitter、Facebook、Skype、GTalk、G+等社交工具。当你在工作状态时候不想被突然的消息提示打断吧。

![](http://ww1.sinaimg.cn/mw690/7cc829d3gw1eqz7p6y2uoj20ju05ttaw.jpg)

**Fog Creek：你的开发环境是什么样的？**

**Peteris：**我装了双操作系统。我在自己的工作站使用Windows 7，通过SSH登录Linux服务器。我上个月刚搭建了一个新的工作站：Intel i7 4790K 处理器并超频处理到 4.7Ghz。

另外我还有一个Linux 防火墙服务器、一个Linux文件服务器和一个Linux开发服务器。通过Samba将Linux文件服务器挂载到Windows上，然后在RAID6上运行驱动器。所有的这些Linux服务器都运行Slackware。我很喜欢Slackware的简洁。为了保证系统简洁，我一般只安装的需要的package。比如firewall服务器上只有Bash, Vim和Iptable。文件服务器上只有Bash, Vim, Cryptsetup和Samba。开发服务器上有我开发需要的一切工具。

我在Linux上使用Vim，在Windows上使用gVim和Visual Studio。Windows应用开发的时候，如果环境中没有IntelliSense，也很让人抓狂。我有一个高度定制化的Vim并有很多插件，比如：

*   surround.vim (quickly edit surrounding text)
*   repeat.vim (repeat surround commands)
*   matchit.vim (extend what % key matches)
*   snipmate.vim (code snippets)
*   nerd_tree.vim (explore filesystem from vim)
*   a.vim (alternate C and H files)
*   ragtag.vim (mappings for editing HTML)
*   tabular.vim (aligning text)
*   bufexplorer.vim (working with buffers)
*   python.vim (better python support)
*   exchange.vim (exchange text quickly)
*   abolish.vim (substitute words)
*   speeddating.vim (increment dates)
Windows下的工具：

*   Total Commander (file manager)
*   Visual Studi(can’t beat IntelliSense)
*   SQLyog (GUI manager for MySQL databases)
*   SQLiteSpy (GUI manager for SQLite databases)
*   pgAdmin (GUI manager for Postrgres databases)
*   WinSCP and SecureFX (secure FTP clients)
*   Putty and SecureCRT (SSH clients)
*   KeePass (password manager)
*   ClipX (clipboard manager)
*   Launchy (program launcher)
*   Locate32 (file indexer)
*   allSnap (window manager)
*   AutoHotkeys (automate tasks and programs)
*   Virtual CloneDrive (mount disk images)
*   IsoBuster (extract disk images)
*   ImgBurn (image burner)
*   Enounce MySpeed (speedup or slow down videos)
*   Hex Workshop (hex editor)
*   VMWare Workstation (virtual machines)
*   Cygwin (unix tools)
*   UltraMon (multi-screen support)
*   Beyond Compare (diffing tool)
*   Tclock2 (better clock)
*   Fineprint (printer proxy)
*   SumatraPDF (better PDF viewer)
*   AviSynth (edit videos programmatically)
*   ffmpeg (convert videos)
*   VirtualDub (convert and edit videos)
*   WinDirStat (disk space visualization)
*   clink (better cmd.exe)
*   IDA Pr(debugging)
*   Photoshop
*   Sysinternals tools
Linux下的工具：

*   samba (mounting Linux on Windows)
*   tmux and screen (persistent shell sessions)
*   all the standard UNIX utilities (awk, sed, grep, head, tail, uniq, sort, etc.)
*   perl (rapid prototyping, quick hacks, one-liners)
*   iptables and nftables (firewalling)
*   htop (better top)
*   mtr (better traceroute)
*   multitail (tail multiple files in multiple windows)
*   nc (netcat, TCP/IP swiss army knife)
*   iftop (bandwidth monitor)
*   ack (better grep)
*   ipcalc (network address calculator)
*   pv (pipe viewer – UNIX pipe progress bar)
*   rsync (backups)
*   ncdu (disk space visualization)
*   curl (http client)
*   nmap (network scanner)
*   tcpdump and wireshark (for network debugging)
*   sysdig (strace + lsof + tcpdump combined)
*   youtube-dl (downloading all online videos)
我坐着编程。我从来没有试过站着或者走着编程，我觉得那样太怪了。当我在工作状态时，我收听电台di.fm。如果不在，音乐往往让人分心。我已经用微软的Natural键盘写了十年代码了，除了年老色衰之外，它现在依然工作很好。

在我试着理解某些东西的时候，我会做很多的笔记。当我遇到复杂问题的时候，我一般将它们分解成小的易于解决的子问题，列在ToDlist上，一个一个解决。实际上我的ToDlist分成三种：长期的（未来1到2年）、中期的（未来几个月）和短期的。

**Fog Creek：关于开发，最喜欢的书或者资源？**

**Peteris：**我很喜欢计算机和科学书籍。每几个月我就花一天时间研究一下最新的出版物并买上一些最感兴趣的。下面我列出我最喜欢的5本书。

1\. 《[The New Turing Omnibus](http://www.amazon.cn/gp/product/0805071660/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&amp;camp=536&amp;creative=3200&amp;creativeASIN=0805071660&amp;linkCode=as2&amp;tag=vastwork-23)》
对计算机感兴趣的人必读。这本书共有66篇小文章，全都是最重要和最有趣的计算机相关知识，比如压缩、图灵机、正则方法和神经网络等。这本书的写作风格休闲，基本不含有数学相关的知识。一直是我的最爱。

2\. 《[The Little Book of Semaphores](http://www.amazon.cn/gp/product/1441418687/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&amp;camp=536&amp;creative=3200&amp;creativeASIN=1441418687&amp;linkCode=as2&amp;tag=vastwork-23)》
这本书告诉我们如何去思考多线程执行的问题以及如何解决同步问题。我强烈推荐这本书，如果你是自学的话更加推荐。这本书会引导读者一步一步去解决一系列的经典和非经典同步问题。解决问题的过程很有乐趣，我已经将它推荐给了我身边的每一个人。

3\. 《[编程珠玑](http://www.amazon.cn/gp/product/B00SFZH0DC/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&amp;camp=536&amp;creative=3200&amp;creativeASIN=B00SFZH0DC&amp;linkCode=as2&amp;tag=vastwork-23)》和《[编程](http://www.amazon.cn/gp/product/B004ZWBW5G/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&amp;camp=536&amp;creative=3200&amp;creativeASIN=B004ZWBW5G&amp;linkCode=as2&amp;tag=vastwork-23)[珠玑（续）](http://www.amazon.cn/gp/product/B004ZWBW5G/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&amp;camp=536&amp;creative=3200&amp;creativeASIN=B004ZWBW5G&amp;linkCode=as2&amp;tag=vastwork-23)》
经典编程书籍。Jon Bentley知道如何清楚、热情地写算法问题。这本书一直很好，它会教你如何分析问题、将问题拆开成小问题，以及高效实现解决方法。如果你读了这两本书，那么你会通过Google面试的。（注：我们都知道这是远远不够的：）。）

4\. 《[The Little Schemer](http://www.amazon.cn/gp/product/0262560992/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&amp;camp=536&amp;creative=3200&amp;creativeASIN=0262560992&amp;linkCode=as2&amp;tag=vastwork-23)》
这本书会以一种很有趣的方式教你一些关于LISP的知识。这本书就像你和作者之间关于数百个Scheme编程问题的对话，它会教学会递归思考。它会教你思考并锻炼你的思维能力。最有趣的编程书籍之一。

5\. 《The Elements of Style | [风格的要素](http://www.amazon.cn/gp/product/B008H0PQPE/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&amp;camp=536&amp;creative=3200&amp;creativeASIN=B008H0PQPE&amp;linkCode=as2&amp;tag=vastwork-23)》 和 《The Elements of Programming Style | [编程风格](http://www.amazon.cn/gp/product/B00UG7A7KA/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&amp;camp=536&amp;creative=3200&amp;creativeASIN=B00UG7A7KA&amp;linkCode=as2&amp;tag=vastwork-23)》
严格意义上来说，《The elements of Style》并不是完全的开发或者编程书籍，而是一本关于写作的书籍。为了成为一个牛逼的程序员，你需要和别人清楚的交流，写作技巧是必要的。这本书一共100页，你一个晚上就可以读完。

《The Elements of Programming Style | [编程风格](http://www.amazon.cn/gp/product/B00UG7A7KA/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&amp;camp=536&amp;creative=3200&amp;creativeASIN=B00UG7A7KA&amp;linkCode=as2&amp;tag=vastwork-23)》要一本经典的编程书籍。作者是Kernighan，写作风格深受到前一本书影响。这本书有点古老，但是基本所有的东西依然很有用。这本书共有70条编程规则，比如**清晰书写**。

![](http://ww2.sinaimg.cn/mw690/7cc829d3gw1eqz7p7szj5j20ju05tdjl.jpg)

**Fog Creek：你目前使用的技术能分享一下吗？**

**Peteris：**我是Visual Studio的死粉，刚下载了Visual Studi2015 Preview版本并且开始使用了。我还在虚拟机中装了Windows 10。由于我现在做多浏览器的测试工作，对微软的新浏览器Spartan还是很期待的。

Google刚开源了Kythe，这是一个很好的学习机会。我去年就从一个Googler朋友那儿听说了，早已经等不及了。这个周末我打算在Linux内核源代码上试试Kythe。

如果我有更多时间，我想把Oculus Rift和运动平台结合起来做一些虚拟现实的小东东。

**Fog Creek：不编程的时候喜欢做些什么？**

**Peteris：**我喜欢锻炼身体，做一些田径运动。我发现短路运动比咖啡更有用。我做10组短路之后，在接下来10到12个小时里都精力充沛，可以整个晚上编程像个怪物一样。我也喜欢田径比赛，特别是400米和800米。

**Fog Creek：对于年轻的自己，你有什么建议？**

**Peteris：**4条建议：

*   1\. 快速、高效解决问题，并继续
*   2\. 不要做没价值的事情
*   3\. 更早开始写技术博客
*   4\. 版本迭代尽量早尽量快