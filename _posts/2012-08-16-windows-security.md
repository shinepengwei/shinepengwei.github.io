---
layout: post
title: Windows系统的安全体系结构
category: "理论及算法"
---
<strong>摘要：</strong>本文以Windows NT系列版本为基础介绍了Windows安全体系结构，并重点介绍了Windows系统中的身份认证、访问控制和安全审计机制。对于身份认证，文章主要介绍了交互式登录和网络身份认证，并介绍了他们所使用的身份认证协议NTLM和Kerberos协议，同时介绍了智能卡、指纹识别等认证技术。对于访问控制主要介绍了Windows中访问控制的原理、用户组的概念以及对文件的访问控制，同时对Vista中最新引入的用户账户控制UAC进行了简单介绍。对于审计主要介绍了windows的审计工作原理和审计策略。

<strong>关键字：</strong>Windows安全；安全体系结构；身份认证；访问控制；审计；
<h1>1 引言</h1>
1983年微软发布了第一代Windows到现在Windows 8的诞生，Windows操作系统已经有了近30年的历史。早期由于网络的未普及性，电脑大部分以孤岛的形式独立运行，人们未预见到安全的重要性，微软公司也未为早期Windows操作系统设计完整的安全机制。

随着计算机的发展，人类社会的大量工作及财富转移到计算机平台，同时由于互联网的爆发式增长，安全问题受到人们越来越多的关注。而作为所有应用的运行平台，操作系统的安全成为重中之重。为此，在Windows NT系列的系统中，微软为Windows操作系统引入了各种安全机制以保证操作系统的安全性。

根据美国国防部于1983年提出并于1985年批准的"可信计算机系统安全评价准则(TCSEC)，Windows NT系统达到了C2级别<sup>[1]</sup>。由于操作系统本身便是为了运行各种应用软件，以及用户的非专业性和对操作系统要求的方便操作等原因，操作系统不可能也无需达到完全地安全，相反，对安全过分追求可能导致操作的复杂从而降低用户体验。因此，如何平衡安全性和操作的简便性也是微软要考虑的问题。为此，微软也通过允许用户根据自身的使用要求进行个性化设置。

本文以Windows NT为核心首先介绍了Windows安全体系的基本思路和总体结构，在此基础上，重点介绍身份认证机制、访问控制机制和安全审计机制。
<h1>2 基本思路与总体结构</h1>
操作系统作为硬件和应用软件之间的连接桥梁和软件运行的平台，保证它的安全运行是信息安全的基础，操作系统所提供的安全服务主要包括<sup>[2]</sup>：认证、授权、隐私保护，数据完整性，不可否认性等。

本文以Windows Vista的安全体系结构为例，介绍Windows系统的安全体系结构。Vista的安全体系结构<sup>[3]</sup>如图1所示。

<img class="aligncenter" src="/wp-content/uploads/2012/08/081612_0558_Windows1.png" alt="" />
<p style="text-align: center;"><span style="font-size: 9pt;"><strong>图1 Vista的安全体系结构
</strong></span></p>
Windows安全体系结构主要分为三个层次，通用基础设施、安全基础设施和应用安全。
<ul style="margin-left: 42pt;">
	<li>
<div style="text-align: justify;">通用基础设施：此模块是操作系统的核心部分，主要为上层（包括安全模块但不仅限于此）提供计算、通信和存储等基本服务。</div></li>
	<li>
<div style="text-align: justify;">安全基础设施：此模块包括系统安全服务的主要模块<sup>[4]</sup>，为操作系统、应用程序和网络提供安全服务。包括以下模块：</div>
<ul>
	<li>
<div style="text-align: justify;">安全硬件服务：Windows所支持的硬件安全服务，如USB Key以及Vista中引入的智能卡和可信计算模块（Trusted Computing Module, TPM）<sup>[5]</sup>等。</div></li>
	<li>
<div style="text-align: justify;">密码服务：此模块主要为系统中其他模块提供加密解密、密钥管理等密码服务。包括公钥基础设施（PKI）和CryptoAPI、CNG密码模块。</div></li>
	<li>
<div style="text-align: justify;">认证：对用户身份进行鉴别，在此基础上给用户赋予不同的权限。</div></li>
	<li>
<div style="text-align: justify;">通信安全：此模块主要提供安全的网络、通讯服务，保护网络上传输数据的安全以及保证系统和服务免受非法用户的访问或攻击。主要包括IP安全协议（IPSec），安全套接字层（SSL）以及虚拟专用网络（VPN）。</div></li>
	<li>
<div style="text-align: justify;">安全存储：此模块在密码服务的基础上对文件系统进行加密，以防止数据的非法访问。</div></li>
	<li>
<div style="text-align: justify;">访问控制：在用户认证的基础上，给用户提供相应权限的服务，而防止非授权用户访问资源。</div></li>
	<li>
<div style="text-align: justify;">审计：通过日志的形式对系统中有关安全的活动进行记录、检查和审核。</div></li>
	<li>
<div style="text-align: justify;">安全管理：包括安全策略管理和安全补丁管理。在windows 7中微软引入了Action Center，从中可以对系统安全特性以及备份、更新等功能进行配置<sup>[6]</sup>。</div></li>
</ul>
</li>
	<li>
<div style="text-align: justify;">安全应用：主要指基于安全基础设施构建的安全应用软件，其中安全机制是基于应用基础设施设计和开发的。</div></li>
</ul>
从本质上来讲，操作系统的安全便是在用户对资源和服务进行访问和使用时，对用户操作进行管理，阻止非法用户访问资源和服务。用户访问资源过程如图2 所示。

<img class="aligncenter" src="/wp-content/uploads/2012/08/081612_0558_Windows2.png" alt="" />
<p style="text-align: center;"><span style="font-size: 9pt;"><strong>图2 用户访问资源过程
</strong></span></p>
由图2 可以看出，在windows安全体系中，身份认证、访问控制和审计是三个核心模块，在windows安全中起着非常重要的作用。因此，本文将分别重点介绍windows安全框架中的身份认证、访问控制和审计模块。
<h1>3 身份认证</h1>
<h2>3．1 身份认证概述</h2>
身份认证<sup>[7]</sup>（又称身份鉴别）对用户身份进行识别和验证，防止攻击者假冒合法用户获取访问权限，一般涉及识别和验证两方面内容。

识别：识别访问者身份，并可区分不同用户。

验证：对访问者生成的身份进行确认。

身份认证主要使用以下几种依据：
<ul style="margin-left: 42pt;">
	<li>
<div style="text-align: justify;">验证他知道什么，如口令。</div></li>
	<li>
<div style="text-align: justify;">验证他拥有什么，如智能卡。</div></li>
	<li>
<div style="text-align: justify;">验证他的生物特征，如指纹、视网膜、语音甚至DNA。</div></li>
	<li>
<div style="text-align: justify;">验证他特有的能力，如签名。</div></li>
</ul>
<h2>3.2 Windows 2000的身份认证机制</h2>
Windows 2000的身份认证机制主要分为两部分：交互式登陆和网络身份认证<sup>[8]</sup>。交互式登录要求用户登录到域账户或者本地计算机账户，而网络身份认证则是向特定的网络服务提供对身份的证明。

早期Windows使用NTLM协议进行身份认证，在Windows 2000中，微软引入了更加安全的Kerberos v5认证协议，同时并向下兼容NTLM协议。此外，Windows 2000 还支持使用硬件令牌作为身份认证，如使用智能卡将密钥存储在硬件设备中，更有效地防止了网络上的假冒攻击。
<h3>3.2.1 交互式登陆</h3>
用户帐户分为本地用户和域用户，本地用户用于访问本地计算机资源，只在本地进行身份认证，用户信息存储在SAM中，域用户用以访问网络资源，用户信息存储与活动目录中。域是window NT 中数据安全和集中管理的基本单位。

在Windows系统中，用户可以交互式登录到本地计算机或者域账户。登录到本地计算机即可获得该机算上机上的资源的相应的使用权限。而登录到域账户则可同时获得本地计算机和域上资源和服务的相应访问权限。
<h4>3.2.1.1 交互式登陆框架</h4>
在Windows系统中，交互式登陆的框架如图3 <sup>[9]</sup>所示。

<img class="aligncenter" src="/wp-content/uploads/2012/08/081612_0558_Windows3.png" alt="" />
<p style="text-align: center;"><span style="font-size: 10pt;"><strong>图3 交互式登陆框架
</strong></span></p>
在交互式登录框架中，各组件的功能如下
<ul>
	<li>
<div style="text-align: justify;">Winlogon:负责管理登录相关的安全性工作，处理用户的登录与注销、启动用户 shell、输入口令、更改口令、锁定与解锁工作站等。此外，Winlogon还要向GINA发送事件通知消息，并提供可供GINA调用的各种函数。</div></li>
	<li>
<div style="text-align: justify;">GINA：此模块所提供的动态链接库实现了登陆进程的认证和身份认证，实现了Windows默认的登陆界面并提供了Winlogon用于标志和认证用户的输出函数。同时，GINA 收集用户的登录数据，然后集中传递到本地安全授权机构（LSA）</div></li>
	<li>
<div style="text-align: justify;">本地安全授权机构（LSA）：LSA是安全子系统的核心，负责在本地登录和远程登录中认证用户的身份，并维护本地安全策略。如图所示，LSA调用特定的身份认证程序包来完成以上任务。</div></li>
	<li>
<div style="text-align: justify;">身份认证程序包：身份认证程序包接受输入的登陆证书，并通过自己的认证程序来约定是否允许用户登录。Windows 2000 默认安装了两个认证程序包，它们分别是MSV1_0 和Kerberos v5。</div></li>
</ul>
当身份认证程序验证了用户的可登录性以后，将结果通过LSA返回给GINA，GINA向用户显示验证是否成功，同时，GINA还将结果返回给Winlogon。如果验证成功，Winlogon会为用户创建一个Windows工作桌面，并启动用户与计算机交互的相关进程。
<h4>3.2.1.2 登陆到本地计算机</h4>
在Windows2000中，每个用户都在系统安全账户管理器（Security Account Manager, SAM）中存储一个用户信息。当用户登录到本地计算机时，系统将登陆信息和SAM数据库中的条目进行比较以判断用户提交的信息是否有效。

<img class="aligncenter" src="/wp-content/uploads/2012/08/081612_0558_Windows4.png" alt="" />
<p style="text-align: center;"><span style="font-size: 9pt;"><strong>图4 交互登陆到本地计算机
</strong></span></p>
登录到本地计算机的交互式登陆过程如图4 所示，具体的步骤如下：
<ol style="margin-left: 42pt;">
	<li>
<div style="text-align: justify;">用户启动登陆过程。Winlogon切换至Winlogon桌面，并调用GINA显示的登陆对话框。</div></li>
	<li>
<div style="text-align: justify;">用户输入用户名和密码，GINA将登录信息发送给LSA进行认证。</div></li>
	<li>
<div style="text-align: justify;">LSA调用认证程序包（如MSV1_0）。</div></li>
	<li>
<div style="text-align: justify;">身份认证程序包根据用户的登录信息在SAM数据库中寻找匹配信息。</div></li>
	<li>
<div style="text-align: justify;">如果找到匹配的用户信息，SAM就向身份认证程序包返回用户的SID和用户所在组的SID。</div></li>
	<li>
<div style="text-align: justify;">认证程序包向LSA返回用户ID和用户所在组的SID。</div></li>
	<li>
<div style="text-align: justify;">LSA使用这些SID创建安全访问令牌，并将令牌和登陆确认信息返回给Winlogon。</div></li>
</ol>
通过以上步骤，用户完成了登录到本地计算机的过程并获得了系统对该用户提供的访问资源权限。
<h4>3.2.1.3 登录到域账户的交互式登陆过程</h4>
域用户账户信息存储在域的活动目录服务中。本质上来讲，当用户从一台 Windows计算机登录到域账户时，用户实质上是在请求允许使用那台计算机上的本地系统服务。域账户的登录和身份认证使用Kerberosv5协议，该协议是一个基于Ticket的协议，为客户和服务器之间提供双向的身份认证。

用户启动登陆过程如下：
<ol style="margin-left: 42pt;">
	<li>
<div style="text-align: justify;">切换至Winlogon桌面，并调用GINA显示登陆对对话框。</div></li>
	<li>
<div style="text-align: justify;">GINA将用户输入的账户信息发送给LSA进行验证。</div></li>
	<li>
<div style="text-align: justify;">LSA接收到用户的登陆信息后对口令进行散列函数转换，并通过Kerberos身份认证程序包发送给域控制器的密钥分发中心（Key Distribution Center, KDC）。</div></li>
	<li>
<div style="text-align: justify;">KDC收到验证服务请求后，利用自己的密钥对信息解密以判断用户是否正确。</div></li>
	<li>
<div style="text-align: justify;">KDC证实用户的身份后为用户返回一个登录会话密钥并向Kerberos认证程序包返回一个TGT（Ticket Granting Ticket，票据授予票据）。</div></li>
	<li>
<div style="text-align: justify;">LSA根据用户特有的信息为用户创建会话令牌，并将其和登陆确认信息返回给Winlogon。</div></li>
</ol>
至此，用户完成了域用户的登录并进入了Windows桌面。交互式域登陆过程<sup>[9]</sup>如图5 所示。

<img class="aligncenter" src="/wp-content/uploads/2012/08/081612_0558_Windows5.png" alt="" />
<p style="text-align: center;"><span style="font-size: 9pt;"><strong>图5 交互登陆到域账户
</strong></span></p>
通过使用KDC返回给用户的TGT，用户可以在TGT的有效期内使用KDC的票据授予服务获得ST（Session Ticket，会话票据），用户持有ST便可以访问相应的资源和服务。
<h3>3.2.2 网络身份验证</h3>
在Windows2000系统中，网络身份验证依赖于成功的交互式域账户登录。网络身份验证如图所示。通过 Kerberos 验证程序包，用户会话将预先建立的TGT 提交给 KDC上的票据授予服务TGS，TGS向用户生成一个可用于该项服务或资源的会话票据。然后，用户把这个服务票据（Service Ticket）提交给所请求的资源，就可以被授予相应的访问权限。

网络身份验证过程<sup>[9]</sup>如图6 所示。

<img class="aligncenter" src="/wp-content/uploads/2012/08/081612_0558_Windows6.png" alt="" />
<p style="text-align: center;"><span style="font-size: 9pt;"><strong>图6 网络身份验证
</strong></span></p>

<h2>3.3 身份认证协议</h2>
<h3>3.3.1 NTLM身份认证协议</h3>
NTLM（NT局域网管理器）是Windows NT早期版本的标准安全协议，由于NTLM容易破解，自Windows 2000开始操作系统的默认协议使用Kerberos，但是为了向前兼容性，Windows仍支持NTLM。

NTLM主要存在以下问题：
<ul style="margin-left: 42pt;">
	<li>
<div style="text-align: justify;">NTLM是微软开发的非开放性协议，对它支持的厂商较少。</div></li>
	<li>
<div style="text-align: justify;">NTLM不提供双向的认证，只能服务器认证客户端，而客户端无法认证服务器。因此，容易被攻击者使用伪装的服务器认证而获得系统的访问权限。</div></li>
</ul>
<h3>3.3.2 Kerberos身份认证协议</h3>
Kerberos <sup>[10]</sup>协议是麻省理工学院设计的一种用户双方进行验证的认证协议，是Windows 2000默认的身份认证协议。
<p style="background-color: white; background-position: initial initial; background-repeat: initial initial;"><img class="aligncenter" src="/wp-content/uploads/2012/08/081612_0558_Windows7.png" alt="" /></p>
<p style="text-align: center; background: white;"><span style="font-size: 9pt;"><strong>图7 Kerberos身份认证过程
</strong></span></p>
Kerberos身份认证过程如图7所示，此协议包括两个部分：

1 .身份认证

用户向密钥分发中心KDC发送自己的身份信息，KDC的身份认证服务（Authentication Service ,AS）对用户信息进行鉴别，鉴别成功后为用户生成TGT， 并用用户的密钥将TGT加密回复给用户。

此时只有真正的用户才能利用它与KDC之间的密钥将加密后的TGT解密，从而获得TGT。

2.票据授予

当用户需要访问相应的网络资源时，用户把TGT和希望访问的服务名称发送给票据授予服务（Ticket Granting Service，TGS），一经批准，TGS为用户发行一个会话票据ST，客户收到ST后可持有ST访问相应的网络资源。

相比于NTLM协议，Kerberos协议更加灵活高效，且具有更好的安全性。Kerberos主要具有以下优点：
<ul style="margin-left: 42pt;">
	<li>
<div style="text-align: justify;">服务器验证过程更加高效。</div></li>
	<li>
<div style="text-align: justify;">可以提供双向验证。</div></li>
	<li>
<div style="text-align: justify;">简化了信任管理，使信任验证通过默认的双向和传递。</div></li>
	<li>
<div style="text-align: justify;">Kerberos是开放的标准，可以与其他网络协同工作。</div></li>
</ul>
<h2>3.4 其他身份认证技术</h2>
<h4>3.4.1 智能卡</h4>
智能卡通常和信用卡大小一样，它所提供的抗篡改存储能保证用户的证书和私钥。Windows 2000系统引入了智能卡验证，它是基于用户持有凭证的身份认证技术，通过智能卡进行身份验证相比传统的用户名&amp;口令的认证方式在安全性上有了极大的提高。

目前，使用智能卡及关联个人标记（PIN）的组合验证形式越来越受到人们的关注并广泛应用于金融、军事等安全级别要求较高的环境，它主要有以下优势<sup>[11]</sup>：
<ul style="margin-left: 42pt;">
	<li>
<div style="text-align: justify;">提高了攻击者的破解难度。由于智能卡使用难以伪造的电子证书作为凭证，黑客必须窃取智能卡并同时获取PIN才可以通过身份验证。</div></li>
	<li>
<div style="text-align: justify;">无法抵赖。由于智能卡能识别登录用户信息，所以降低了用户抵赖责任的可能性。</div></li>
</ul>
<h4>3.4.2 生物特征认证</h4>
利用人类特征进行身份认证也是一种安全性较高的认证方法。口令和信物都有泄漏和复制的可能，但是生物特征具有无法复制的特点。因此，在对于安全性要求较高的系统，可使用这种费用相对比较昂贵的身份鉴别系统。这些鉴别系统利用授权用户的人类特征作为鉴别依据，主要有语音、指纹、视网膜及DNA等。

对于生物特征的认证程序目前大部分都是由第三方开发的，微软在Windows Vista中内置了指纹识别的认证程序<sup>[1]</sup>。
<h1>4 访问控制</h1>
<h2>4.1 访问控制概述</h2>
访问控制是在保障授权用户能取所需资源的同时拒绝非授权用户的安全机制。在用户身份认证和授权之后，访问控制机制根据用户身份将根据预先设定的规则对用户访问某项资源进行控制，只有规则允许时才能访问，违反预定的安全规则的访问行为将被拒绝。访问控制的是为了根据权限限制访问主体对访问客体的访问，从而使计算机系统安全的运行。

为了实现访问控制的目的，访问控制机制需要完成识别确认用户和拒绝或允许用户访问资源的任务。前者主要由身份认证完成，因此，本章主要介绍访问控制如何根据权限限制用户访问资源。

访问控制一般包括三种类型<sup>[7]</sup>：
<ul style="margin-left: 43pt;">
	<li>
<div style="text-align: justify;">自主访问控制</div>
<p style="margin-left: 1pt;">自主访问控制基于对主体和主体所属的主体组的识别来限制对客体的访问，它是一种比较宽松的访问控制机制。</p>
</li>
	<li>
<div style="text-align: justify;">强制访问控制</div>
<p style="margin-left: 1pt;">强制访问控制是一种比较强硬的访问控制机制，系统为主题和客体分别制定安全级别，不同级别标记了不同的重要程度和访问能力，不同级别的主题对不同级别的客体的访问在强制的安全策略下实现。</p>
</li>
	<li>
<div style="text-align: justify;">基于角色的访问控制</div>
<p style="margin-left: 1pt;">在基于角色的访问控制模式中，用户以一定的角色访问资源，不同的角色被赋予不同的访问权限。</p>
</li>
</ul>
其中，Windows操作系统是基于自主访问控制实现的访问控制机制。
<h2>4.2 windows访问控制工作原理</h2>
<p style="margin-left: 36pt;">对于访问控制的主体（即用户），Windows在用户登陆的时候为用户创建一个访问令牌，该令牌包含用户的SID、用户组的SID和用户访问权限。对于访问控制的客体（即被访问对象），Windows为访问资源提供了安全描述，定义了客体的安全信息。</p>
<p style="margin-left: 36pt;">访问令牌把用户权限通知安全子系统，而安全子系统通过安全描述获知被访问对象是否允许用户访问和如何访问。</p>
<p style="margin-left: 36pt;">访问令牌和安全描述关系<sup>[9]</sup>如图8 所示。</p>
<img class="aligncenter" src="/wp-content/uploads/2012/08/081612_0558_Windows8.png" alt="" />
<p style="text-align: center;"><span style="font-size: 9pt;"><strong>图8 访问令牌和安全描述的关系
</strong></span></p>
<p style="margin-left: 36pt;">图8 中各模块分别介绍：</p>

<ul style="margin-left: 43pt;">
	<li>
<div style="text-align: justify;">安全标识（Security Identifier，SID）。</div>
<p style="margin-left: 1pt;">Windows使用SID唯一标识用户和用户组，在用户和用户组创建的时候生成。</p>
</li>
	<li>
<div style="text-align: justify;">访问令牌。</div>
<p style="margin-left: 1pt;">当用户登录系统时，本地安全授权机构LSA在登陆过程中获得该用户和用户组的SID，并以此为用户创建访问令牌，其中包括了与用户有关的标识和特权信息。在此后代表该用户工作的每个进程和线程中都有访问令牌的一个副本。</p>
</li>
	<li>
<div style="text-align: justify;">访问控制列表和访问控制项。</div>
<p style="margin-left: 1pt;">访问控制项ACE包含了用户或组的SID以及对象的权限，访问控制项有两种，允许访问和拒绝访问。访问控制列表ACL是ACE的有序列表，定义了主体对对象和对象属性的访问限制和保护措施。ACL包括自主访问控制列表DACL和系统访问控制列表SACL，DACL规定了哪些安全主体可以访问对象以及以何种方式访问，而SACL定义了操作系统将产生何种类型的审计信息。</p>
</li>
	<li>
<div style="text-align: justify;">安全描述。</div>
<p style="margin-left: 1pt;">Windows为访问对象创建安全描述，其中包含了此对象的一组安全属性。其中主要包括所有者信息、自由访问控制列表和系统访问控制列表。安全描述由对象所有者创建。</p>
</li>
</ul>
Windows系统中访问控制工作原理如下：
<ol>
	<li>
<div style="text-align: justify;">当用户成功登陆后，系统根据用户SID为用户生成访问令牌，包括用户名、所在组、安全标识等信息。</div></li>
	<li>
<div style="text-align: justify;">当用户生成新进程和线程时，系统为其复制一个访问令牌的副本。</div></li>
	<li>
<div style="text-align: justify;">当进程要访问某个对象时，访问控制监视器将进程中的访问令牌中的SID和对象的安全描述中自主访问控制表遍历比较，从而决定是否允许进程访问对象。</div></li>
</ol>
<h2>4.3 用户组</h2>
组是用户账户的一种容器。通过用户组的设置可以允许将权限分配给一组用户而不是某个用户，从而大大的简化了用户和用户权限分配的管理。Windows2000中包括两种类型的组：安全组和分布组。安全组用于授权，可用来分配访问资源的访问权限。分布组用于与安全不相关的列表。

Windows 2000具有一些内置的组，他们是预定义的用户集合，同时具有不同的权限。Windows系统中默认的用户组如表1所示。

<span style="font-size: 9pt;"><strong>表1 Windows系统中默认的用户组
</strong></span>
<div>
<table style="border-collapse: collapse;" border="0"><colgroup> <col style="width: 186px;" /> <col style="width: 524px;" /></colgroup>
<tbody valign="top">
<tr>
<td style="padding-left: 9px; padding-right: 9px; border: solid black 0.5pt;"><span style="font-size: 10pt;">组名</span></td>
<td style="padding-left: 9px; padding-right: 9px; border-top: solid black 0.5pt; border-left: none; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;"><span style="font-size: 10pt;">注释</span></td>
</tr>
<tr>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: solid black 0.5pt; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;"><span style="font-size: 10pt;">Administrators</span></td>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: none; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;"><span style="font-size: 10pt;">成员具有所有的权利，可以执行所有操作系统提供的功能</span></td>
</tr>
<tr>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: solid black 0.5pt; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;"><span style="font-size: 10pt;">Users</span></td>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: none; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;"><span style="font-size: 10pt;">成员权限较低，只能运行经过认证的一些程序</span></td>
</tr>
<tr>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: solid black 0.5pt; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;"><span style="font-size: 10pt;">Guests</span></td>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: none; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;"><span style="font-size: 10pt;">成员和用户组成员具有相同权限，但是限制更多</span></td>
</tr>
<tr>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: solid black 0.5pt; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;"><span style="font-size: 10pt;">Authenticated Users</span></td>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: none; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;"><span style="font-size: 10pt;">隐藏组，包括所有的已登录账户</span></td>
</tr>
<tr>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: solid black 0.5pt; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;"><span style="font-size: 10pt;">Backup Operators</span></td>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: none; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;"><span style="font-size: 10pt;">备份操作员为备份或还原文件可以代替安全限制</span></td>
</tr>
<tr>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: solid black 0.5pt; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;"><span style="font-size: 10pt;">Replicator</span></td>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: none; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;"><span style="font-size: 10pt;">用于域中的文件复制</span></td>
</tr>
<tr>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: solid black 0.5pt; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;"><span style="font-size: 10pt;">Server Operators</span></td>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: none; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;"><span style="font-size: 10pt;">成员可以管理域服务器</span></td>
</tr>
<tr>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: solid black 0.5pt; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;"><span style="font-size: 10pt;">AccountOperators</span></td>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: none; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;"><span style="font-size: 10pt;">成员可以管理域用户和组账户</span></td>
</tr>
<tr>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: solid black 0.5pt; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;"><span style="font-size: 10pt;">LINTERATIVE</span></td>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: none; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;"><span style="font-size: 10pt;">包括物理控制台和终端服务登录到本地系统的所有用户</span></td>
</tr>
<tr>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: solid black 0.5pt; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;"><span style="font-size: 10pt;">Everyone</span></td>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: none; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;"><span style="font-size: 10pt;">当前网络的所有用户</span></td>
</tr>
<tr>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: solid black 0.5pt; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;"><span style="font-size: 10pt;">Network</span></td>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: none; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;"><span style="font-size: 10pt;">通过网络访问指定资源的用户</span></td>
</tr>
<tr>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: solid black 0.5pt; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;"><span style="font-size: 10pt;">Print Operators</span></td>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: none; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;"><span style="font-size: 10pt;">成员可以管理域打印机。</span></td>
</tr>
</tbody>
</table>
</div>
<h2>4.4 文件系统的访问控制</h2>
NTFS是一种高效、安全的文件系统<sup>[12]</sup>。在Windows2000中引入，他提供了对文件系统的访问控制，可以通过设置文件或者目录的ACL来调整文件对用户的访问权限。

在NTFS文件系统中，文件可以设置的权限从低到高分别为：无、读取、更改和完全控制，对目录的访问权限包括"无"、"读取"、"写入"、"列目录"、"读取和写入"、"修改"和"完全控制"。

由于文件夹是子文件夹和文件的容器，因此文件夹的权限是默认继承的，即分配给文件夹的权限将自动应用于该文件夹内的子文件夹和文件。

同时，Windows提供了对文件的加密服务。早期Windows系统主要使用加密文件系统（EFS），但由于该系统只能对文件或目录进行加密，无法做到对整个磁盘卷的加密，因此微软在Vista系统引入了BitLocker，它可以加密整个Windows卷，包括页面文件、注册表数据文件等系统文件。在Windows7中，BitLocker功能获得了进一步的提升<sup>[6]</sup>，已经可以为移动硬盘等移动存储介质进行加密。
<h2>4.5 用户账户控制UAC</h2>
在Windows系统中，很多用户都有管理特权，这对计算机安全管理是一个很大的隐患。因此，微软在Windows Vista和Windows Server 2008中引入了UAC（User Account Control，用户账户可控制）。用户账户控制（UAC）<sup>[13]</sup>也称作用户账户保护，是最小特权原则在Vista中的体现，即通过控制使用户在执行任务时使用尽可能少的特权。其设计目的是尽量减少用户权限为管理员的时间，从而帮助用户更好的保护系统安全，防止恶意软件的攻击。

UAC允许用户验证系统行为，从而组织未经认证的计算机系统的使用和变动。当用户以管理员身份登录到Windows后，会得到两个访问令牌：完全访问令牌和标准受限访问令牌。标准受限的访问令牌并没有系统的完全管理权，而系统中程序默认都是以标准受限的访问令牌运行。因此，当程序运行需要管理员权限的操作时，必须通过特定的用户界面（UAC对话框）确定之后才能将权限提升为管理员。

但是，用户账户的引入大大增加了系统操作的复杂性，降低了用户体验，因此在Windows 7中微软提供给用户更多的选择<sup>[6]</sup>，可以使用户根据自己的使用环境对其进行配置甚至关闭。
<h1>5 安全审计机制</h1>
审计是对信息系统访问控制的必要补充，它会对用户使用何种信息资源、使用的时间，以及如何使用 (执行何种操作) 进行记录与监控，这样就可以在安全事故发生后帮助管理员发现发生事故的原因并提供相应的证据。

审计可以实现多种安全相关目标<sup>[14]</sup>，包括：个人职能、事件重建、入侵检测、故障分析。
<h2>5.1 Windows审计工作原理</h2>
Windows审计子系统通过和安全监控和安全策略组件联合工作，记录用户的操作和系统的安全事件。安全监控根据审记策略监控系统的安全事件，当有相关的安全事件发生时，监视器就会通知安全审计子系统，并将事件信息发送给审计子系统，审计子系统将这些信息格式化为事件日志，并通过安全日志引擎存储与安全日志中。Windows审计工作原理<sup>[15]</sup>如图9 所示。

<img class="aligncenter" src="/wp-content/uploads/2012/08/081612_0558_Windows9.png" alt="" />
<p style="text-align: center;"><span style="font-size: 9pt;"><strong>图9 Windows审计模块工作原理
</strong></span></p>
同时，Windows根据不同的事件定义了三种日志，每种日志记录不同的事件。应用程序日志记录由应用程序或一般程序产生的事件信息。系统日志记录系统本身活动产生的事件信息。安全日志记录安全事件的信息，包括监视系统、用户和进程相关的安全活动的信息。

在以上基础上，Windows还提供了另外三种日志供用户选择使用：目录服务日志、文件复制日志和DNS服务日志。
<h2>5.2 审计策略</h2>
Windows审计子系统是根据审计策略来选择要记录的事件类型，对于每一个可审核的事件，又可以选择事件执行成功或执行者失败时跟踪记录事件信息。

Windows中可以审计的事件类型<sup>[9]</sup>有以下九种：
<ul style="margin-left: 43pt;">
	<li>
<div style="text-align: justify;">登陆事件</div>
<p style="margin-left: 1pt;">每次用户在计算机上登录或者注销时，审计系统会在安全日志中生成一个事件</p>
</li>
	<li>
<div style="text-align: justify;">账户登录事件</div>
<p style="margin-left: 1pt;">当某用户登录到域时，需要在域控制器上对登陆进行处理，这是域控制器上会记录用户对账户进行验证的登陆尝试。</p>
</li>
	<li>
<div style="text-align: justify;">账户管理</div>
<p style="margin-left: 1pt;">当用户对账户进行管理时，如创建、更改或操作用户或组的操作，该审计策略会记录此操作的相关信息。</p>
</li>
	<li>
<div style="text-align: justify;">对象访问</div>
<p style="margin-left: 1pt;">在windows系统中，每个被访问对象都有一个系统访问控制列表SACL，它包含一个将要审计的对对象进行操作的用户和租的列表，基于SACL可以实现对对象访问的审计。</p>
</li>
	<li>
<div style="text-align: justify;">目录服务访问</div>
<p style="margin-left: 1pt;">活动目录同样具有SACL，因此可以对他们进行审计。</p>
</li>
	<li>
<div style="text-align: justify;">特权使用</div>
<p style="margin-left: 1pt;">用户具有自己的权限，并在权限的限制下进行操作，审计系统也会对某些特权的使用活动进行记录。</p>
</li>
	<li>
<div style="text-align: justify;">进程跟踪</div>
<p style="margin-left: 1pt;">当系统要求审计计算机上的某些进程的运行信息时，审计系统会记录创建进程和结束进程的尝试。</p>
</li>
	<li>
<div style="text-align: justify;">系统事件</div>
<p style="margin-left: 1pt;">当用户或进程改变计算机环境的某些方面时，比如更改事件、清空日志等操作，系统会记录这些操作的事件信息。</p>
</li>
	<li>
<div style="text-align: justify;">策略更改</div>
<p style="margin-left: 1pt;">当用户修改审计策略本身时，系统也会对他们的更改操作进行审计。</p>
</li>
</ul>
在windows Vista之前，所有安全事件都属于9中审记策略类别之一。通过启用该类别的成功或失败的审计，就启用了该类别的所有审计事件，审计策略都是平级的<sup>[15]</sup>。

在Windows Vista和Windows Server2008中，审计策略是分层的，所有的安全事件都属于一个审计策略自类别。当启用某子类别的审计策略时，就启用了所有属于该类别的审计策略。审计策略组织形式如图10 所示。
<p style="margin-left: 21pt; text-align: center;"><img class="aligncenter" src="/wp-content/uploads/2012/08/081612_0558_Windows10.png" alt="" /><span style="font-size: 9pt;"><strong>图10 Windows审计策略层次组织形式</strong></span></p>

<h1>6 总结</h1>
本文以Windows NT系列版本为基础首先介绍了Windows安全体系结构，并重点介绍了Windows系统中的身份认证、访问控制和安全审计机制。对于身份认证，文章主要介绍了交互式登录和网络身份验证，并介绍了他们所使用的身份验证协议NTLM和Kerberos协议，同时介绍了智能卡、指纹识别等验证技术。对于访问控制主要介绍了Windows中访问控制的原理、用户组的概念以及对文件的访问控制，同时对Vista中最新引入的用户账户控制UAC进行了一些介绍。对于审计主要介绍了windows的审计工作原理和审计策略。

除此以外，在Windows安全体系中，还有其他安全模块也是保证Windows正常安全的为用户提供服务的重要手段，如网络传输安全、应用服务安全以及安全管理等。
<h1>参考文献</h1>
[1] 谭彬.Windows Vista操作系统的安全关键技术分析[D] .上海交通大学.2008.

[2] 贾春福，郑鹏. 操作系统安全 [M] .武汉：武汉大学出版社，2006.

[3] 卿斯汉，沈晴霓. 操作系统安全 [M] . 北京：清华大学出版社，2011.

[4] <a href="javascript:;">Vista安全技术纵览</a>[EB/OL]. <a href="http://blog.chinaunix.net/uid-20197215-id-1729356.html">http://blog.chinaunix.net/uid-20197215-id-1729356.html</a>.

[5] Larry Chaffin, Scott Granneman. Microsoft Vista for IT Security Professionals [M] .Syngress，2007.

[6] Windows 7安全机制十大革新[EB/<em>O</em>L].http://netsecurity.51cto.com/art/200912/167515.htm

[7] 龚俭，吴桦，杨望. 计算机网络安全导论 [M]. 南京：东南大学出版社，2007.

[8] windows系统安全机制[EB/OL]. <a href="http://wenku.baidu.com/view/b303108cd0d233d4b14e6%20999.html">http://wenku.baidu.com/view/b303108cd0d233d4b14e6 999.html</a>.

[9] 薛质，王轶骏，李建华. Windows系统安全原理与技术 [M]. 北京：清华大学出版社，2005.

[10] 维基百科. Kerberos协议 [EB/OL]. <a href="http://en.wikipedia.org/wiki/Kerberos_(protocol)">http://en.wikipedia.org/wiki/Kerberos_(protocol)</a>.

[11] 潘晓恒. WINDOWS域智能卡认证实施方案设计[D].内蒙古大学.2008.

[12] 维基百科.NTFS [EB/OL]. <a href="http://zh.wikipedia.org/zh/NTFS">http://zh.wikipedia.org/zh/NTFS</a>.

[13] <a href="javascript:authorHref('作者','彭爱华',2,1,'作者代码|单位-机构-学位授予单位-作者机构-作者单位')">彭爱华</a>. WindowsVista安全机制分析[J] .程序员.2007,(7).

[14] Windows网络安全审计的四部曲 [EB/OL]. <a href="http://cio.ccw.com.cn/research/info/htm2009/%202009">http://cio.ccw.com.cn/research/info/htm2009/ 2009</a>0217_589158.shtml.

[15] Jesper M. Johansson. Windows Server 2008安全技术详解[M].北京：人民邮电出版社，2010.
