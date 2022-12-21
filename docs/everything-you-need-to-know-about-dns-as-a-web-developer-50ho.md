# 作为一名 web 开发人员，您需要了解的关于 DNS 的一切

> 原文：<https://dev.to/rogercampos/everything-you-need-to-know-about-dns-as-a-web-developer-50ho>

本文旨在对有关 DNS 和域名的概念、工具和细节进行实用总结。对于有经验的开发人员来说，它们中的许多都不是新闻，但是对于第一次使用这种东西的人来说可能是有用的。您将不会在这里找到所涉及的协议的技术描述，但更多的是概念的入门级和平易近人的描述，以及如何将一切结合在一起。

## 注册商

### 它们是如何工作的

没有域名，DNS 就什么都不是，获得域名的第一步是从注册商那里购买。这些公司负责*正式记录*谁拥有什么，它们是关于域名的信任链的第一步。当你购买域名时，你会被要求提供一些联系信息，如姓名、电子邮件和地址。这将是该域名的 *whois* 记录的一部分。这只是关于该域名的官方信息，注册商需要拥有这些信息，并且还需要*更正*。这就是为什么有时你会收到来自你的注册商的电子邮件，要求*检查*他们关于你的域名的信息是否正确(你的名字、地址等等)。).然而，这些信息**并不要求公开**，只是注册服务商需要拥有。这很重要，因为一些超级便宜的注册商会在不告诉你的情况下让你购买域名，例如，你的信息会被公开！如果你以后想把它变成私有的，你会发现它是作为一个单独的产品出售的*。这是那些公司的典型伎俩，我建议你不要公开这些数据。*

当你购买一个域名时，你会购买一定的年限，当它到期时，你仍然有一个“宽限期”(通常为 40 天)，即使到期后你也可以重新购买。注册商总是会在您发给他们的电子邮件中通知您过期。

可以将一个域名转让给另一个注册商，但是，要这样做，你必须在新的注册商那里购买域名，支付费用，然后说这是现有域名的转让。你只需要提供一些**授权码**，你的旧注册商必须给你证明所有权，但由于你必须再次支付，通常你会想在当前注册即将到期时这样做。

我想澄清一下，以防万一，我们所说的“域”是指“something.com”、“foobar.gov”等。只是一个名字，后面跟着一个点，再后面跟着“com”什么的。一旦你购买了一个域名，**你就可以使用任意数量的子域名，甚至是嵌套的(foo.bar.something.com)。在注册商层面，子域名本身并不是实体，假设你也拥有所有的域名，并代表所有的域名做出回应。因此，对于与根域相关的东西(还有技术安全问题，我们将在后面看到)和你控制的东西，一定要使用子域。**

### TLD 法规

需要知道的一件重要事情是，关于域名的**规定根据 TLD** (顶级域名，任何域名的最后一部分都跟在点号后面，就像。com“，”。es”等。).而许多顶级域名是开放的(如“)。对于任何想购买的人来说，其他一些是受限制的顶级域名，这意味着他们会对买家进行额外的验证。比如，“。仅当您能够证明您在纽约市内拥有有效地址时，才能购买 nyc "(适用于纽约市)，或"。gov”域名只能由美国境内的联邦、州、地方和部落政府组织购买。鉴于此类差异和限制，这也意味着并非所有注册服务商都将在所有 TLD 中运营。

近年来，特定顶级域名数量激增。很多好奇又奢侈的喜欢”。黑色星期五。啤酒”，但也有一些，现在属于一个具体的公司，如。苹果”只能由苹果公司拥有的域名。关于安全的争论被用来证明这一点，但实际上这只不过是互联网滥用的又一个例子。关于 TLD 转让的其他冲突在过去也发生过，例如[看起来亚马逊将最终拥有。亚马逊 TLD](https://www.lexology.com/library/detail.aspx?g=ed56ae56-c0ea-47de-8f3a-afaf670cdfcc) 代替它的是开放代表亚马逊地区。

### 注册员级别的设置

从技术上来说，在注册器中只有几件事情是您必须管理的。第一个也是最重要的是域名的域名服务器，即 NS 记录。这些字段必须指向一个 DNS 服务器，该服务器实际上是管理该域所有 DNS 记录的第一权威。你必须在这里输入至少两个名字，只是为了冗余，如 [RFC 1304](https://tools.ietf.org/html/rfc1034) 所述。别担心，所有的 DNS 服务器都会为你提供至少 2 个域名。

DNS 服务器有许多免费的选择:Netlify、Cloudflare 等等。举例来说，你甚至可以使用你的自托管 DNS 服务器，使用 [bind](https://bind9.net/) ，但是我真的建议使用一些已经可以用于任何面向互联网的东西。

配置域名服务器有时真的会令人困惑，因为**所有的注册商通常也是 DNS 服务器**，默认情况下，他们会用他们的 DNS 服务器来配置你的新域名。他们可能会向您显示一个配置“DNS 记录”的界面，好像这就是要做的全部工作，而实际的 NS 记录配置可能有些隐藏。找找看，一般叫“DNS 服务器”。通常注册商的 DNS 服务器不是很好，所以我的建议是改变它们，把它指向你信任的地方，并提供我们稍后会看到的所有功能。

您只能在注册器中进行的另一项技术配置是 DNSSEC 的设置。这是 DNS 规范中最近增加的内容，目的是验证 DNS 服务器的权威性。我们之前说过，注册商是唯一可以证明你是一个给定域的所有者，通过这种机制，注册商还验证你选择的 DNS 服务器是唯一一个被授权为你的域提供 DNS 条目的服务器。点击这里查看关于 DNSSEC 的更完整的解释。

### `whois`工具

要了解与某个域相关的所有信息，可以使用命令行程序`whois`，例如:

```
~ > whois microsoft.com
   Domain Name: MICROSOFT.COM
   Registry Domain ID: 2724960_DOMAIN_COM-VRSN
   Registrar WHOIS Server: whois.markmonitor.com
   Registrar URL: http://www.markmonitor.com
   Updated Date: 2014-10-09T16:28:25Z
   Creation Date: 1991-05-02T04:00:00Z
   Registry Expiry Date: 2021-05-03T04:00:00Z
   Registrar: MarkMonitor Inc.
   Registrar IANA ID: 292
   Registrar Abuse Contact Email: abusecomplaints@markmonitor.com
   Registrar Abuse Contact Phone: +1.2083895740
   Domain Status: clientDeleteProhibited https://icann.org/epp#clientDeleteProhibited
   Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited
   Domain Status: clientUpdateProhibited https://icann.org/epp#clientUpdateProhibited
   Domain Status: serverDeleteProhibited https://icann.org/epp#serverDeleteProhibited
   Domain Status: serverTransferProhibited https://icann.org/epp#serverTransferProhibited
   Domain Status: serverUpdateProhibited https://icann.org/epp#serverUpdateProhibited
   Name Server: NS1.MSFT.NET
   Name Server: NS2.MSFT.NET
   Name Server: NS3.MSFT.NET
   Name Server: NS4.MSFT.NET
   DNSSEC: unsigned
   URL of the ICANN Whois Inaccuracy Complaint Form: https://www.icann.org/wicf/

   [...] 
```

我们可以看到一些日期，比如更重要的创建和到期日期，以及关于谁是所有者的一些信息。这是我们之前谈到的一件事，如果你的注册服务商提供“私人 whois”，你不会在这里看到你的名字和电子邮件。否则，你的电子邮件会暴露，所以...你可以得到一些额外的垃圾邮件。

在“名称服务器”下，我们还可以看到该域正在使用的 DNS 服务器。在例 4 中，它们都做同样的事情，这只是为了冗余。

正如我们之前所说的，由于关于域名的实际规定取决于每个 TLD，因此一些信息或全部信息可能不可用。比如”。es " TLDs 根本不显示任何内容:

```
~ > whois google.es

    Conditions of use for the whois service via port 43 for .es domains

    Access will only be enabled for  IP addresses  authorised  by Red.es.  A maximum of one  IP address per
    user/organisation is permitted.

    [...] 
```

## [DNA](#dns)

### 什么事？

首先，基本的定义每个人可能都知道，DNS 是一种存储与域名 T2 相关的信息的方式。最典型的例子就是搜索网站。任何网站都是托管在服务器上的(是的，甚至是无服务器的)，你的浏览器必须知道那个服务器的 IP 地址才能连接到它并请求那个网页。要知道，以前，它使用 DNS 来发现，例如，“microsoft.com”翻译成`40.76.4.15`(现在，特别是从我的位置)。

这是 DNS 在域“microsoft.com”中存储信息的一个例子，该信息可以在`40.76.4.15`找到其网站。

DNS 系统由分布在世界各地的数以千计的 DNS 服务器组成，它本质上是一个全球分布式数据库。详细解释这是如何工作的已经超出了本文的范围，[，但是这无疑是一篇有趣的文章。您应该知道，作为一个如此大的数据库，新信息传播到整个网络需要时间。在实践中，当您在自己的领域中做一些更改时，您实际上可以在大约 1 小时后看到更改生效，但为了安全起见，请始终等待 24-48 小时，然后再将更改视为全局完成。](https://howdns.works/)

### DNS 识别服务器？还是网站？

有些人可能认为，根据我们刚才所说的，DNS 提供的将域名转换为 IP 地址的解析机制也用于识别该服务器上托管的网站。换句话说，我们需要一个域名来让网站运行。不是这样的，**网站独立于 DNS** 也能很好地工作，它们是独立的系统。

如果一个服务器(ip = `40.76.4.15`的机器)托管 2 个不同的网站，决定哪一个作为对 HTTP 请求的响应将完全取决于 HTTP 请求本身。

这个决定是基于客户端**必须发送给**的`Host` HTTP 报头，其目的正是为了识别客户端请求的网站。此标题是强制性的，不能省略。

我们可以通过一个简单的实验来检查这一点，使用`curl -I`只显示标题。首先我们检查正常响应:

```
~ > curl -I http://microsoft.com
HTTP/1.1 301 Moved Permanently
Date: Fri, 14 Jun 2019 18:58:42 GMT
Server: Kestrel
Location: https://www.microsoft.com/ 
```

我们看到微软用 301 来回应 https 版本。现在如果我们直接问 IP:

```
~ > curl -I http://40.76.4.15
HTTP/1.1 404 Not Found
Date: Fri, 14 Jun 2019 18:58:51 GMT
Server: Kestrel 
```

这是 404。他们的网络服务器知道我们没有发送`Host`报头，在这种情况下，逻辑响应是 404。现在，我们可以再次尝试使用 IP，但是手动提供`Host`报头:

```
~ > curl -I http://40.76.4.15 -H "Host: microsoft.com"
HTTP/1.1 301 Moved Permanently
Date: Fri, 14 Jun 2019 18:58:59 GMT
Server: Kestrel
Location: https://www.microsoft.com/ 
```

我们得到了相同的初始反应，很好！。

这里的技巧是 http 客户端，比如 curl，会自动将`Host`头设置为与域相同的值。我们可以通过启用更多详细信息来检查这一点:

```
~ > curl -v -I http://microsoft.com
* Rebuilt URL to: http://microsoft.com/
*   Trying 40.76.4.15...
* TCP_NODELAY set
* Connected to microsoft.com (40.76.4.15) port 80 (#0)
> HEAD / HTTP/1.1
> Host: microsoft.com
> User-Agent: curl/7.58.0
> Accept: */*
>
< HTTP/1.1 301 Moved Permanently
HTTP/1.1 301 Moved Permanently
< Date: Fri, 14 Jun 2019 18:59:08 GMT
Date: Fri, 14 Jun 2019 18:59:08 GMT
< Server: Kestrel
Server: Kestrel
< Location: https://www.microsoft.com/
Location: https://www.microsoft.com/

<
* Connection #0 to host microsoft.com left intact 
```

这通常是一个容易忘记的细节。如果你没有明确地配置它，当通过直接 IP 访问时，许多 web 服务器将默认地服务于一个“随机”的网站。这也可能导致谷歌和搜索引擎普遍认为这是重复内容，因为同一个网站将由两个不同的 T2 域名提供服务。

然而，如果你的网站只使用 https(应该如此)，这个问题就解决了。因为如果我们用 https 尝试这个实验，我们会看到 curl 抱怨说:

`curl: (51) SSL: no alternative certificate subject name matches target host name '40.76.4.15'`

服务器没有为该 IP 颁发的 SSL 证书。这是意料之中的，SSL 证书中的通用名称必须与我们正在连接的**域**完全匹配，而不是 http 主机。第一个原因是，否则这将是一个令人难以置信的安全问题(任何人都可以伪造它)，第二，更严格地说，这是不可能的。实际的 HTTP 协议在最先进行的 TLS 连接的“内部”。这就是 https 的目的，加密连接，所以在 TLS 协商成功完成之前，服务器看不到任何关于实际 HTTP 请求的信息。这是一个协议套一个协议。

为一个 IP 地址颁发 SSL 证书在技术上是可能的，但这不是任何正常人会做的事情。

### DNS 作为信息存储

我们应该讨论的第一个概念是，在 DNS 系统中，附加到域的信息总是按照记录类型进行分类。您不能存储“任何东西”，它必须是允许的记录类型之一，并且每种类型都有特殊的格式和特定的用途。我们说我们在 DNS 中存储“记录”,并且每个记录属于一种记录类型。您可以有许多相同记录类型的记录，这没有问题。

其次，DNS 系统中的每个记录还必须有一个“主机”属性。一些提供者称之为“主机”，另一些提供者称之为“名称”，还有一些提供者甚至使用了更有想象力的名称。也许你听说过[给事物命名是这个行业有史以来最困难的问题之一](https://hilton.org.uk/blog/why-naming-things-is-hard)，这是绝对正确的，这里我们还有另一个例子。不管它在你的提供者中的名字是什么，你必须知道它代表这个记录将被附加到的子域(或根域)的**。每个子域(或根域)都有自己的记录。**

如果记录属于一个子域，只需介绍子域的名称，但如果是根域，则必须使用特殊的语法，不能留空。许多提供商使用“@”来指代根域，但其他提供商会使用“.”，您必须查看提供商的文档或示例。

除此之外，根据记录类型，您必须输入更多或更少的信息，并且会应用一些额外的验证。

### 如何可视化 DNS 记录

调试和试验时最重要的事情之一是拥有一个**快速可靠的反馈回路**。一些你可以信赖的东西。而且查 DNS 记录的时候，`dig`是你的朋友。这是一个简单的命令行工具，使用方法如下:

```
~ > dig google.es

; <<>> DiG 9.8.3-P1 <<>> google.es
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 8004
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;google.es.         IN  A

;; ANSWER SECTION:
google.es.      155 IN  A   172.217.16.227

;; Query time: 23 msec
;; SERVER: 80.58.61.254#53(80.58.61.254)
;; WHEN: Fri Jun 14 20:59:21 2019
;; MSG SIZE  rcvd: 43 
```

这是在告诉我们“google.es”有一条指向`172.217.168.163`的类型为“A”的记录。为了使事情更简洁，有一个名为“+short”的选项可以让`dig`只显示有用的响应，其他什么都不显示，我们从现在开始使用这个选项:

```
~ > dig google.es +short  
172.217.168.163 
```

一个有趣的技巧是，你可以强迫`dig`向一个 DNS 服务器询问你的问题。如果您刚刚更改了 DNS 服务器中的一些 DNS 信息，请直接询问该服务器以验证信息是否已更新。您的 DNS 服务器地址与您在注册机构中使用的地址相同。例如，使用`whois`我们可以发现`microsoft.com`由`NS1.MSFT.NET`管理。我们可以直接问:

```
~ > dig @NS1.MSFT.NET microsoft.com +short
13.77.161.179
40.76.4.15
40.112.72.205
40.113.200.201
104.215.148.63 
```

这样，您就不需要等待 DNS 信息传播来验证您的更改！

### 有哪些记录类型？

让我们快速回顾一下 DNS 可以使用的不同记录类型。用的最多的只有 4 个:A，CNAME，TXT，MX。其他类型从一开始就已经存在了，比如 SOA 和 NS，但是它们纯粹是技术性的，我们在这里就不讨论了。同样的事情也发生了，随着时间的推移，为了不同的目的，你可以在这里看到一个完整的列表。

#### 一

“A”记录类型是最基本的记录类型，它们将名称转换为 IP 地址。要翻译的“名称”将是我们之前讨论过的“主机”属性，对于根域通常是“@”和任何想要创建子域的内容。在这种情况下，实际数据将只是指向的 IP。我们有两种变体，“A”代表 ipv4，“AAAA”代表 ipv6。即使你没有给出一个明确的“AAAA”版本，通常 DNS 服务器会自动处理来自“A”记录的转换。

我们之前说过，同一类型可以创建许多不同的记录，通常对于大公司的“A”记录来说也是如此。它被用作在机器之间分配负载的一种方式(但是**它不是一个负载平衡器**，我们稍后会看到)。

```
~ > dig amazon.com +short
176.32.98.166
176.32.103.205
205.251.242.103 
```

出于某种原因，谷歌只有 1，但我们可以看到亚马逊定义了 3。

这种机制主要服务于两个目的，首先，对于我们人类来说，记住品牌名称(或产品、服务等)比记住一系列数字更容易也更好(IPv6 甚至更好，但那是另一个故事)。有人可能会说，现在你可以在谷歌(或任何其他搜索引擎)中搜索品牌名称，并通过它访问网站，所以域名和整个 url 是无用的。几年前[谷歌试图这样做](https://www.cnet.com/news/google-chromes-plan-to-hide-urls-hits-a-snag/)，但由于安全问题，主要是网络钓鱼攻击，很快又恢复了。我认为，在相当长的一段时间内，我们仍将拥有域名和完整网址。

第二个目的是技术性的，使用域来访问资源意味着使用一个额外的抽象层，所以可以找到资源的真实位置(服务器)可以被替换，而世界上的其他人不知道。

#### MX

“MX”记录相当于“A”记录，但用于电子邮件。

在 A 的情况下，我们有:

1.  要求[网络浏览器]访问“google.es”
2.  它询问 DNS 系统:google.es 的 A 记录是什么？”]
3.  获取一个 IP 作为响应
4.  浏览器连接到该 IP 并继续运行。

对于“MX ”,我们有:

1.  要求[电子邮件客户端]向" [jon@google.es](mailto:jon@google.es) "发送电子邮件
2.  它询问 DNS 系统:google.es 的 MX 记录是什么？”]
3.  获取 IP 地址或域作为响应
4.  连接到它(如果响应是一个域，这里可能会有一个额外的 A 分辨率)并继续发送电子邮件。

有一些额外的技术差异，但本质上就是这样。为什么不把两者混合呢？好吧，这样我们就不会被迫使用同一个服务器进行网络托管和电子邮件托管。网站托管在我们管理的服务器上(或者像 Netlify 这样的平台上)是很常见的，但是电子邮件是由谷歌管理的。

#### [t1 网络 NAME](#cname)

Cnames 很有趣，它们就像别名。CNAME 记录只有一个信息价值，而且必须是另一个域名，而不是 IP。但它可以是任何东西，不一定是来自同一个域的另一个“子域”，任何东西。意思是说:*你正在寻找的这个域实际上指的是另一个域*，然后客户端必须继续搜索。当您想要公开使用一个域，但在内部与另一个域相关联时，这是非常有用的。你可以改变这个“另一个域”，世界上的其他地方不会受到影响。

一个典型的用例是处理网站的“www”版本。你直接在根域下建立你的网站(有一个 A 记录)，然后“www”子域被分配一个 CNAME 给根域。这里有一点很重要，我们**只是在谈论 DNS 解析机制**，与 HTTP 或其他任何东西无关。设置好“CNAME www”后，`ping www.mywebsite.com`会起作用，但仅此而已。另外，如果你将你的网络服务器配置为**也**监听你的域名的 www 版本，那么你将能够从子域浏览你的网站**也**。然而，在这种情况下，`mywebsite.com`和`www.mywebsite.com`都可以正常工作，显示相同的网站，但是这样你就会遇到重复内容的搜索引擎优化问题。一般来说，你不希望你的内容出现在互联网的不同网址上。为了最终解决这个问题，您需要创建一个从 www 版本到根域的`301`重定向。

你可以看到 CNAMEs 在 Netlify、Zendesk、Heroku 等公司中使用的另一个例子。所有这些公司都有一个共同点，那就是他们需要为客户提供一个网站作为他们服务的一部分。

如果你创建一个新的 Heroku 网站，他们会给你一个 URL `https://<my_name>.herokuapp.com/`，一个新的 Netlify 应用程序会出现在`https://zen-shockley-<whatever>.netlify.com`下，类似的事情也会发生在 Zendesk 上。这是他们给你的“规范”域名。如果你想在那些网站上使用你自己的域名，你通常需要设置一个 CNAME 来指向它们(**警告**:在这样做之前，请看后面关于 [CNAME 扁平化](#alias-or-cname-flattening))。

最后，我们还应该提到，CNAMEs 与 SSL 没有任何关系！。按照前面的例子，如果你的网络生活网站是`zen-shockley-1412a2.netlify.com`并且你在`www.mywebsite.com`中设置了一个指向该网站的 CNAME，那么网络服务器必须有一个覆盖`www.mywebsite.com`的 SSL 证书。Netlify 和其他服务会在你配置好你想要使用的域名后自动处理，但是也许其他服务会强迫你自己上传 SSL 证书。

如果你正在创建一个有这个功能的应用程序，那么你将需要管理自动 SSL 发布，例如，让我们加密。花点时间来欣赏一下[我们有像“让我们加密吧”这样的东西是多么幸运！](https://letsencrypt.org/donate/)。几年前，这是一个真正的痛苦，总是需要人工干预。

#### TXT

这种 DNS 记录无非就是一个**明文信息**。有一些技术会要求您使用一个(如设置 DKIM 进行电子邮件传递，我们稍后会看到)，它也通常被用作证明域名所有权的一种方式。

例如，Google analytics 或任何其他与域名相关的服务通常会要求你确认你是域名的所有者。他们可以证明的方法之一是让你创建一个新的 CNAME 或 TXT 记录。很久以前，通常的做法是添加一个他们给你的特殊 html 文件。这对于静态网站来说很好，但对于动态网站来说，通常最好使用 TXT 或 CNAME 来验证这种东西。

### 别名 CNAME 或拉平

在看了最常见的 DNS 记录类型之后，现在我们将讨论一种“虚拟类型”。这个概念在一些 DNS 服务器中被称为“别名记录类型”，或者“ANAME”，或者有时被称为“cname 扁平化”，但它是相同的概念。

这就像一个 CNAME 记录，但直接解决了。我们看到 CNAMEs 充当别名，将一个域指向另一个域。但这只在 DNS 级别有效。当一个程序在解析“foo.bar.com”时，DNS 服务器会回应*，这实际上与“bar.com”相同*，然后程序会再次询问“bar.com”。换句话说，所有客户端软件都知道 CNAMEs。

然而，使用这种别名类型，客户机将直接接收 IP 地址(如果是 A 记录)。客户端会认为这是一个正常的 A 记录，这是 DNS 服务器做的把戏。**它手动短路别名**，它是 dns 服务器解析目的地并返回给用户最终信息。

这为什么有用？因为根域中不允许有 CNAME 记录，所以发明了这个解决方案。如果你在 Netlify，Heroku 等网站上主持，这是你应该做的。你希望你的网站在你的自定义根域。不能使用 CNAME，请使用别名。

但是为什么不允许在根域中有 CNAME 呢？有趣的事情。我们之前已经知道**任何 DNS 记录必须属于一个子域或根域**，包括任何 CNAME 声明。我们已经看到，CNAME 就像是客户端软件必须遵循的别名。这实际上是一个矛盾，假设你有两个域:

**foo.website.com**有两个 DNS 条目:

*   值为“alice”的 TXT 记录
*   值为“[www.bar.com](http://www.bar.com)”的 CNAME 记录

www.bar.com 有一个 DNS 条目:

*   值为“bob”的 TXT 记录

当客户询问“foo.website.com”中的 TXT 记录时，应该如何回应？有一个 TXT 记录，但也有一个 CNAME，客户端应该遵循 CNAME 还是在第一个域上保留 TXT？

为了解决这种不一致，DNS 标准增加了一条规则:

> CNAME 记录只能作为单个记录存在，不能与任何其他资源记录组合(DNSSEC SIG、NXT 和关键 RR 记录除外)

因此，如果有一个给定子域或根域的 CNAME，没有其他 DNS 记录可以添加到那里。然而，有两种特殊的 DNS 记录类型，名为 NS 和 SOA，规范强制它们出现在根域中！我们不会详细讨论这些，但这意味着这两件事不能同时为真，所以 CNAME 不能在根域。

不久前，我遇到了一个 DNS 服务器，它可以让你这样做，但请不要这样做。这导致来自某些提供商的电子邮件丢失，如果你不遵守这条规则，可能会发生严重的问题。

### 缓存和 TTL

你必须考虑到 DNS 解析一直在发生，非常非常频繁。解析域名的 IP 地址不是免费的。DNS 使用非常快速的 UDP，但是仍然需要一些时间。如果每次都必须完成整个解决方案，互联网将是不可行的。为了解决这个问题， **DNS 响应被缓存在许多不同的级别**。在您的浏览器中，在您的家用路由器中，以及在中间网络硬件中。这是唯一快速的方法。

如果有缓存，也有一种方法使缓存过期，那就是 TTL(生存时间)。任何 DNS 记录都将有一个关联的 TTL 值，这就是任何相关方可以存储(缓存)其值的时间**。您可以随时从 DNS 服务器更改此设置。典型值是 1 小时，但您可以为 MX 或 CNAME 记录设置更长的时间(通常它们是最稳定的)。**

 **DNS 信息被缓存显然意味着你设置的 TTL 将是你在改变某些东西时期望等待的最小时间**。如果你想把你的网站迁移到另一个服务器，你必须改变一个 A 记录，如果这个 A 记录有 1 个小时的 TTL，你必须让旧的服务器运行至少 1 个小时(我仍然会等 24 小时)。如果你有一个非常大的值，提前检查是很重要的。你可以用`dig` :
来检查这个**

```
~ > dig google.es                                                                                                                                                                                    

; <<>> DiG 9.8.3-P1 <<>> google.es
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 46680
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;google.es.         IN  A

;; ANSWER SECTION:
google.es.      272 IN  A   172.217.17.3

;; Query time: 28 msec
;; SERVER: 80.58.61.254#53(80.58.61.254)
;; WHEN: Sun Jun 16 17:49:27 2019
;; MSG SIZE  rcvd: 43 
```

TTL 就是回答部分的那个“272”数(这次不能用`+short`)。

如果你有一个太大的 TTL，只需将其更改为 1 小时，并等待前一个 TTL 时间到期。

### DNS 作为负载均衡器

我们之前已经看到，您可以设置许多相同类型的 DNS 记录，这通常也适用于 A 记录，如下所示:

```
~ > dig microsoft.com +short                                                                                                                                                                                    
104.215.148.63
13.77.161.179
40.76.4.15
40.112.72.205
40.113.200.201 
```

在这种情况下，当被询问时，dns 服务器将以循环方式返回一个或另一个 IP 地址。

看到这一点，有些人可能会说，你可以只使用 DNS 作为负载平衡器，因为不同的客户端将收到不同的 IP 为您的服务器，有效地分配负载均匀。但是请不要:

*   DNS 解析将由中间解析器和客户端本身进行缓存，因此，最终，您将无法获得良好的负载分配，因为只要 TTL 允许，客户端就会继续重用其之前的服务器。

*   这不是故障转移。如果您的一些服务器停止响应，它仍然会显示在 DNS 响应中。即使您在检测到故障后手动将其配置为从轮换中删除该 IP，客户端和中间解析器也不会接受更改，直到 TTL 过期，通常至少需要 5 分钟。

对于负载平衡，你可以使用 HAProxy，或者使用某种已经提供的基础设施作为服务(Amazon，Linode 等等)。).

最后要注意的是，这种机制实际上可以在受控环境中工作**。如果 DNS 在内部被用作服务发现的机制，那么使用非常短的 TTL 来获得几乎实时的 DNS 响应是有意义的。这是可行的，因为这里性能不是问题，一切都在本地网络中。例如， [consul](https://www.consul.io/intro/index.html) 就是一个使用 DNS 进行发现的服务管理的例子。**

### 所有权在 DNS 服务器提供商内部，他们怎么知道是我？

在开始时，我们看到了如何设置 DNS 服务器，你想为您的域使用。你必须去你的注册商那里更改 NS 记录，将它们指向你想使用的 DNS 服务器。例如，如果您想使用 linode，您可以将它们设置为`ns1.linode.com`和`ns2.linode.com`。然后去 linode.com，登录，并点击“添加新域”。

但是现在...Linode 如何知道**你是那个域名**的所有者？它没有！。在那个时候，其他人能去 linode 添加你的域名吗？是啊！这行得通吗？是啊！

所以这是可能发生的。它是不受管理的，所有的 DNS 服务器提供商都会信任第一个在他们的服务中添加域的提供商。他们都有“客户支持政策”来解决冲突，以防这种情况发生在你身上，但那只是在事后。更重要的是，任何人现在都可以在 Linode 中注册成百上千个域名，只需等待他们中的任何一个被指向那里并获得管理能力的那一刻！

这真的是个问题吗？没有那么多。如果这种情况发生在你身上，你只需要回到你的注册服务商那里，将 ns 记录更改为另一个 DNS 服务器提供商，或者设置默认记录(你的注册服务商肯定会提供给你)，直到冲突得到解决。

攻击面的设置也很复杂，攻击者应该知道你将获得“这个确切的”域名，并且你将把它指向“那个确切的”DNS 服务器。如果他们知道所有这些具体的数据，就可能发生这种情况，但对于一般的攻击来说，这太普通了:要尝试太多的域名，太多的 DNS 服务器。

此外，DNSSEC 将**避免这成为任何人使用你的域名**的实际问题。

正如我们最初看到的，DNSSEC 是一种安全机制，通过这种机制，注册商(只有您能控制的东西)只授权且只授权一个 DNS 服务器为该域提供 DNS 记录。如果无法访问注册器，攻击者将无法正确设置 DNSSEC。请始终为您的域配置 DNSSEC！。如果这种“域管理劫持”发生在你身上，但是你的域已经启用了 DNSSEC，任何客户端都会发现提供记录的 DNS 服务器没有得到注册商的授权，并将中止连接。

### 局部分辨率

还有其他方法可以将域名解析为 IP、“A”DNS 记录类型。所有操作系统都有一个名为`hosts`的特殊文件，在 Linux 和 MacOs 中，它位于`/etc/hosts`，在 Windows 中位于`C:\Windows\System32\Drivers\etc\hosts`。它包含这样的内容:

```
##
127.0.0.1   localhost
255.255.255.255 broadcasthost
::1             localhost 
```

每行以一个 IP 地址开始，在这之后你可以写将解析它的域，一个或多个用空格分开。如果一个域名列在这里，这将优先，正常的 DNS 机制将不会生效。

这是一个很好的方法来检查您的新服务器，而不需要公开更改任何东西，只需编辑这个文件并手动设置您的域指向您的新服务器的 IP 地址，您就可以用浏览器真正测试它了。

在一些特殊情况下，这是行不通的，例如，如果您在自定义服务器上使用 Cloudflare。在这种情况下，cloudflare 充当反向代理以及 DNS 服务器。如果你设置了一个指向你的服务器的 A 记录(例如:`213.24.1.1`)，那么公开解析的 IP 就不会是那个！它将是属于 Cloudflare 服务器的另一个 IP。它们接收所有的流量，添加 SSL 证书管理等功能，并将所有请求代理到真正的服务器。您的服务器只能从 Cloudflare 访问。

在这种情况下，您可以将您的本地`hosts`文件指向您的新服务器，但是您将没有 SSL，例如，如果这是您依赖 Cloudflare 提供的功能。

还值得一提的是特殊的 TLD `.localhost`。它是保留的(不能购买)并且将总是解析到本地主机。当请求的主机对您很重要时，这是一种使用您想要的任何定制域进行本地开发的简单方法。

### 电子邮件和 DNS

电子邮件是另一个需要修改 DNS 条目才能正常工作的大问题。

我们已经看到，要在您的自定义域中接收电子邮件(例如:" [my_name@my_branding.com](mailto:my_name@my_branding.com) ")，您只需配置 MX 记录。这很简单，就像一个记录，这些记录将只用于知道在哪里处理收到的电子邮件。

要正确发送**外发**电子邮件，那就更复杂了。电子邮件标准是开放的，这意味着任何人都可以发送电子邮件，这是一件好事。你可以在本地安装一个电子邮件服务器，用它来发送任意数量的电子邮件。更重要的是，你可以让它看起来像是从你想要的任何地址发送到**。**

自然，这是一个很大的安全问题。为了减少垃圾邮件，托管您的电子邮件的服务器有责任采取措施来最大限度地减少不想要的垃圾邮件。然而不幸的是，这是一个无法解决的问题，因为你永远不知道用户是否真的希望收到新联系人的邮件。

为了打击垃圾邮件，电子邮件提供商使用了许多技术来试图预测你刚刚收到的电子邮件是否“合法”。分析内容，对照已知黑名单检查发件人等。

但至少有一件重要的事情是可以知道的，那就是给你发邮件的人是否真的是他使用的域名的所有者。也就是说，如果你从“【jon@google.com】”收到的电子邮件确实来自谷歌。

这类似于我们之前所说的 dnsSEC，注册服务商“验证”为您的域名提供 DNS 记录的 DNS 服务器。这里，该技术包括验证发送的电子邮件是否与某些 DNS 条目中的配置相匹配。你应该使用的技术叫做 **DKIM** 和 **SPF** ，你可以在 Fastmail 的这篇帮助文章中掌握它们[。](https://www.fastmail.com/help/receive/domains-advanced.html)

### 工具

为了结束这篇关于 DNS 的长文，这里有一些免费的工具可以用来检查你的配置:

*   [https://www.ssllabs.com/ssltest/analyze.html](https://www.ssllabs.com/ssltest/analyze.html)
*   [https://dnsspy.io](https://dnsspy.io)

sslabs 的工具是我所发现的最全面的工具。你应该争取 A+的成绩。它还会报告您可能遇到的与使用的密码、TLS 版本等相关的漏洞。

Dnsspy 更侧重于安全性。它可能会报告您的 DNS 记录是一个服务器接一个服务器的，但这是非常标准的。**