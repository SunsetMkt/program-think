---
layout: default
title: '如何防止黑客入侵[7]&#65306;Web相关的防范&#65288;下&#65289;'
date: None
author:
    display_name: 
---

　　在[本系列前一篇](https://program-think.blogspot.com/2012/09/howto-prevent-hacker-attack-6.html)，俺介绍了三种隔离浏览器的方式（多种浏览器、同种浏览器多实例、多操作系统用户）。今天继续介绍第四种隔离方式——虚拟机，然后再推荐一些浏览器的安全扩展。  
  
　　本文提到的“虚拟机”，全称是“操作系统虚拟机”。

　　最近10年来，硬件水平显著提升，操作系统虚拟化的技术开始普及，出现了若干针对操作系统的虚拟化软件。这种软件可以让你在一台电脑上，同时运行【**多个操作系统**】（是不是很有趣？）。通过虚拟化软件来运行的操作系统，称之为“虚拟操作系统”；与之对应，你原先的操作系统称之为：真实操作系统或宿主操作系统。

　　由于“虚拟操作系统”是虚拟出来滴，你可以在里面为所欲为，而【不会】对真实操作系统产生实质性的影响。比方说，你可以在虚拟系统中把硬盘格式化，但不会影响到你的真实系统。同样的，如果某个虚拟系统被病毒感染了，也不会影响真实系统和其它虚拟系统。 　　所谓“多虚拟机”的方案，就是在你的电脑上创建多个虚拟机，分别用来实现【不同安全级别】的上网行为。 　　举个例子： 　　你可以创建虚拟机A，只用来访问网银（不访问其它网站）；然后创建虚拟机B，用来进行其它上网行为。那么，即使你在虚拟机B受到攻击，对虚拟机A也完全没有影响。这样一来，就可以彻底保证网银的安全。  
　　[前一篇博文](https://program-think.blogspot.com/2012/09/howto-prevent-hacker-attack-6.html)提到的三种隔离方案，“多用户”比前两种方案安全。这种方案是基于操作系统提供的用户壁垒——包括：不同用户的进程隔离性、文件系统的访问控制（ACL）、等等。 　　但是“多用户”方案还是有缺陷的。如果攻击者同时利用了未公开的浏览器漏洞和未公开的操作系统【提权】漏洞，就有可能攻破操作系统的用户壁垒。不过大伙儿别担心，实现这种攻击的难度比较大，只有足够牛B的入侵者能够做到这点。 　　为了满足列位看官的好奇心，稍微介绍一下所谓的牛逼黑客，大都是哪些人。

　　**御用骇客/御用高手**

　　所谓的“御用高手/御用骇客”，也就是官方资助的入侵者（类似于武侠小说中的大内高手）。这种类型的入侵者，往往不是一个人单干独斗，而是一个团队群殴。 　　“御用高手”的目标大致有如下： 　　1. 军事目标 军事目标主要有：外国重要的政府机构（比如：五角大楼）、外国重要的军工企业（比如：洛克希德-马丁公司）

俺在[上一篇](https://program-think.blogspot.com/2012/08/howto-prevent-hacker-attack-5.html)提到了 RSA 被入侵的案例：攻击者首先利用零日漏洞入侵某个 RSA 公司的雇员，经过深度渗透之后，入侵者搞到了 RSA 动态令牌产品（SecureID）的种子（种子是用来生成动态口令的）。由于 SecureID 产品被许多大公司采用，所以入侵者可以利用偷来的种子算出动态口令，进而实现对美国多家大型军工企业的入侵。

计划得如此严密的系列入侵，通常只有御用黑客团队能够干得出来。 　　2. 经济目标 所谓的经济目标，主要都是国外知名的大公司。 通过入侵这些公司，可以窃取商业机密，从而获得巨大的经济效益。 　　3. 政治目标 政治目标就比较杂，比如：知名的反共网站、某些知名的政治异议人士的电脑/手机/邮箱/IM、等等。 　　举个例子：

　　2009年底，Google 被来自中国大陆的攻击者深度渗透（这就是传说中的“[极光行动](https://zh.wikipedia.org/wiki/%E6%9E%81%E5%85%89%E8%A1%8C%E5%8A%A8)”）。此事直接导致 Google 愤而退出中国市场。根据事后分析，入侵者的注意力集中在某些 Gmail 邮箱的内容。而这些 Gmail 邮箱恰恰属于中国的持不同政见者。由此可见，入侵 Google 的人很可能是朝廷的走狗。

　　**民间高手**

　　除了御用黑客，也不排除民间有高手。甚至不排除某个御用黑客在业余时间干点脏活。 　　和御用高手不同，民间高手的目标相对比较单一，大部分人是为了获取经济利益。 　　综上所述，能被这2类人盯上的，往往是高价值目标。如果你只是一个普通网民，不用担心被高手盯上（也就是说，“多用户”的方案基本上可以满足你的安全需求）；反之，如果你自认为是一个高价值的目标，或者你对安全的要求非常非常高，不妨尝试一下“多虚拟机”的方案。 　　刚才已经说了“多虚拟机”的原理，还举了例子。 　　如果你已熟悉“虚拟化软件”（比如：VMware 系列、VirtualBox、等）的使用，那么本方案对你来说其实很简单——无非就是安装若干个虚拟操作系统，然后在虚拟系统中安装软件，仅此而已。

　　由于本文的重点是防范黑客入侵，所以俺就不再介绍虚拟化软件本身的安装和配置。**关于虚拟化软件的扫盲（包括原理、安装、配置、使用），俺已经另写了一个系列（在“[这里](https://program-think.blogspot.com/2012/10/system-vm-0.html)”）**。

　　在4种浏览器隔离方案中，多虚拟机的安全性最高。即使你的某个虚拟机被病毒感染或者被植入木马，也几乎不会影响到你的其它虚拟机和真实系统。 　　当然，绝对的安全是不存在滴。虚拟化软件也是软件，只要是软件，就有可能出现漏洞，只要出现漏洞，就有可能被利用。但是，想通过虚拟化软件的漏洞来突破虚拟机的壁垒，难度更大（远远大于突破操作系统的用户壁垒）。这方面的技术细节，说来话长，俺就不展开了。 　　对硬件的要求高（主要是物理内存和 CPU）。

　　具体要多高的硬件配置，取决于你**同时**开几个虚拟机。**同时运行**的虚拟机越多，就需要越大的物理内存和越多的CPU核心。

　　终于把浏览器的话题说完了。接下来再介绍几款安全方面的浏览器扩展。这些扩展可以帮你提高 Web 的安全性。  
　　**简介**  
　　NoScript 是一个名气很大、功能很强的 Firefox 扩展，主页在“[这里](https://addons.mozilla.org/en-US/firefox/addon/noscript/)”。 通过它，你可以定制网站白名单。只有当你浏览白名单内的网站时，才启用浏览器的 JavaScript 脚本功能和插件功能（比如 Flash 插件、Java 插件、PDF 插件、等）。 　　白名单只是它的功能之一，更多的功能介绍，请看它的主页。

　　**局限性**

　　这个扩展可以有效地避免【陌生网站】上的挂马。但是，如果你【经常访问】的网站出现跨站脚本的漏洞，NoScript 有可能帮不了你。

　　在[前面的博文](https://program-think.blogspot.com/2012/08/howto-prevent-hacker-attack-5.html)中，俺曾经举了一个跨站脚本攻击的例子。比如说，你经常上某个 BBS，并且该 BBS 的界面功能依赖于 JavaScript。那么，你就必须把这个 BBS 站点加入到 NoScript 的白名单中。假如这个 BBS 本身出现了基于 JS 的跨站脚本漏洞，那你还是有可能中招 :(

  
　　**简介** 　　某些用 Chrome 的同学，如果不喜欢刚才提到的 NoScript，可以考虑 Chrome 上的另外两款扩展——跟 NoScript 很像（不但功能很像，连名称也很像）。

　　一款叫做 NotScripts，主页在“[这里](https://chrome.google.com/webstore/detail/odjhifogjcknibkahlpidmdajjpkkcfn)”；另一款叫 ScriptNo，主页在“[这里](https://chrome.google.com/webstore/detail/oiigbmnaadbkfbmpbfijlflahbdbdgdf)”。

　　NotScripts 的用户数比 ScriptNo 略多。至于要选哪个，请大伙儿自行判断。

　　**局限性**

　　这两款扩展的局限性类似于前一个小节提到的 NoScript，俺就不再啰嗦了。  
　　这是著名的电子前线组织（EFF）发布的扩展，主页在“[这里](https://www.eff.org/https-everywhere)”，同时支持 Firefox 和 Chrome。说到 EFF，顺便提一下：Tor ＆ TorBrowser 也是该组织发布的产品。 　　如今，有很多网站都同时提供明文的 HTTP 协议和加密的 HTTPS 协议（比如维基百科）。装了 HTTPS Everywhere 扩展之后，如果你浏览的网站支持 HTTPS 协议，该扩展就会强制浏览器通过 HTTPS 协议访问该网站。从技术上讲，就是把所有针对该网站的 HTTP 请求都转换为 HTTPS 请求。

　　为啥要强制用 HTTPS 协议捏？因为 HTTPS 是加密协议，可以保护你免受入侵者的嗅探（关于“嗅探”的案例，[前面的博文](https://program-think.blogspot.com/2012/08/howto-prevent-hacker-attack-5.html)提到过）。

  
　　除了上述功能，HTTPS Everywhere 扩展还可以帮你侦测有问题的 CA 证书，降低“[中间人攻击](https://zh.wikipedia.org/wiki/%E4%B8%AD%E9%97%B4%E4%BA%BA%E6%94%BB%E5%87%BB)”（MITM）的风险。 　　引申阅读：

　　关于 CA 证书的扫盲，参见《[数字证书及 CA 的扫盲介绍](https://program-think.blogspot.com/2010/02/introduce-digital-certificate-and-ca.html)》。

　　**局限性**

　　如果某个网站只有 HTTP 连接，不提供 HTTPS 连接，那 HTTPS Everywhere 也帮不了你。  
　　**简介**  
　　LastPass 名气最大的在线口令管理工具。官网在“[这里](https://www.lastpass.com/)”，维基百科的介绍在“[这里](https://zh.wikipedia.org/wiki/LastPass)”。 　　该工具提供了针对所有主流浏览器的扩展（包括IE、Firefox、Chrome、Opera、Safari、等），帮你自动填写网站的登录口令，免除你记忆诸多口令的麻烦。你本人只需要记住一个【主密码】，LastPass 会利用主密码来加密本地的密码数据库——你的其它口令都存在在该数据库中。 　　为了确保安全性，LastPass 进行在线同步时，传输的是加密后的数据库。因此，即使 LastPass 网站被黑，入侵者拿到的也只是加密后的用户口令数据库。同样的，如果有人偷了你的电脑，但不知道你的主密码，也无法打开你的密码数据库。

　　**局限性**

　　LastPass 的做法相当于把鸡蛋都放在一个篮子里，有好处也有坏处。 　　最大的风险在于主密码被盗。一旦主密码被盗，密码数据库中的所有密码就都暴露了。什么情况下会发生主密码被盗捏？比如你的电脑被植入木马，并且此木马具有键盘记录的功能；比如你输入主密码的时候，有人在旁边偷窥；......

　　每个人都有很多密码。不过根据[二八原理](https://program-think.blogspot.com/2009/02/80-20-principle-0-overview.html)，真正重要的密码不到20%，大部分密码都不太重要。所以俺个人的建议是：少数特别重要的密码，还是靠自己脑子来记；大多数不太重要的密码，可以交给类似 LastPass 之类的口令管理软件。

  
　　**简介**  
　　BetterPrivacy 是一个侧重于隐私保护的 Firefox 扩展，主页在“[这里](https://addons.mozilla.org/en-US/firefox/addon/betterprivacy/)”。 　　当你浏览某些网站的时候，网站可能会在你的电脑上记录 cookie。通过这些 cookie，网站可以追踪你的上网行为（比如你多久访问一次这个网站）。 　　有了 BetterPrivacy，你就可以配置允许哪些网站记录 cookie。BetterPrivacy 的牛B之处在于：它不光可以控制传统的 cookie，还可以控制 Flash 的 cookie（LSO）。

　　**局限性**

　　此扩展只针对隐私保护，无法防范扩展脚本等攻击。

　　浏览器的安全类扩展，细分为很多领域，数量也很多。限于篇幅，俺仅挑选出每个领域最出名的代表。如果你还有补充的，可以到[本文留言](https://program-think.blogspot.com/2012/10/howto-prevent-hacker-attack-7.html)。

　　关于 Web 的安全防范，本来只想写一篇。谁曾想，东扯西扯，居然写了三篇。可能有同学会问，为啥没提到杀毒软件和个人防火墙？俺觉得：把这两个话题放到 Web 安全防范中聊，不太合适——还是单独拿出来聊比较好。在本系列后续的博文，会说说杀毒软件和个人防火墙的那些事儿。

**[回到本系列的目录](https://program-think.blogspot.com/2010/06/howto-prevent-hacker-attack-0.html#index)**
