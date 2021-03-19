---

title: 如何参与 openEuler linux 内核开发
date: "2021-03-04"
author: openEuler Kernel SIG
thumbnail:
blogexcerpt:
summary: "通过参与 openEuler Kernel 社区可以跟内核大牛讨论技术, 解决问题, 畅所欲言. 很多小伙伴都有想法, 但是之前一直不知道怎么入手, 本文就作为 openEuler Kernel SIG 的开篇之作, 旨在为大家提供一个参与 openEuler Kernel 开发的最基础的指导."
categories:
  - "开发指南"
tags:
  - "openEuler"
  - "Document"

# thumbnail: "img/placeholder.jpg" # Thumbnail image
# lead: "Example lead - highlighted near the title" # Lead text
comments: false # Enable Disqus comments for specific page
authorbox: true # Enable authorbox for specific page
pager: true # Enable pager navigation (prev/next) for specific page
toc: true # Enable Table of Contents for specific page
mathjax: true # Enable MathJax for specific page
sidebar: "right" # Enable sidebar (on the right side) per page
widgets: # Enable sidebar widgets in given order per page
  - "search"
  - "recent"
  - "taglist"

---


openEuler kernel 开发跟 linux 主线一样, 采用社区协作的方式, 我们发送补丁、咨询问题都是通过邮件进行交流的, 因此我们进行内核开发的标准流程和主线内核的开发流程是一样的.

1.	首先你需要配置好你的 git, 而且要能正常使用 git 发送补丁;

2.	下载 openEuler kernel 源代码;

3.	提交、验证你的补丁;

4.	补丁验证完成后, 发往 openEuler 邮件列表, 等待 Maintainer 回复;

5.	参与讨论, 与 Maintainer 和其他开发者充分沟通. 如果大家有意见修改你的补丁重复步骤 3;

6.	你的补丁经过充分的讨论和验证后, 得到了大牛和 Maintainer 的认可, 通过了门禁和测试用例的各项考验, 最终合入内核仓库.


当前版本 v1.3 @ 2021-03-15

| 版本 | 更新日期 | 作者 | 更新日志 |
|:----:|:--------:|:----:|:--------:|
| v0.9 | 2021/03/01 | 汪雄峰 | 完成 v0.9 版本						|
| v1.0 | 2021/03/01 | 成坚   | 完成了内容, 并将格式修改为 MARKDOWN			|
| v1.1 | 2021/03/03 | 汪雄峰 | 完善了 issue 提交帮助					|
| v1.2 | 2021/03/04 | 成坚   | 完善了沟通交流渠道一节的内容，添加技术讨论群添加渠道	|
| v1.3 | 2021/03/15 | 成坚   | 根据检视同学的进行, 进行了认真的修改 |

感谢 [张文博](https://github.com/ethercflow), [张明](https://github.com/zhangming-cloud) 两位同学对文档的检视. 针对大家的意见进行了修改, 感谢. 其中

* [张文博](https://github.com/ethercflow)同学, 检视的非常仔细, 发现了很多问题.

* [张明](https://github.com/zhangming-cloud)同学, 除了检视发现了不少问题, 还按照文档描述的流程, 进行了验证操作.


## 1 环境准备与配置
-------

我们需要一台安装了 `linux` 系统的机器, 由于你要编译内核, 最好是服务器, 当前 PC 也是可以的.

### 1.1 下载 openEuler linux 内核代码
-------

openEuler kernel 开源在 gitee 上面.

```
git clone git@gitee.com:openeuler/kernel.git
```

**强烈建议使用 ssh 协议**, 由于 kernel 仓库较大, 使用 https 协议下载, 可能会下载失败, 特别是当你辛辛苦苦下载一半然后失败的时候, 那种感觉....


![clone kernel 源代码](/kernel-portal/img/2021/03/0002-0001-clone_kernel.png)


默认会把仓库下载到和仓库同名的目录下.

![内核目录](/kernel-portal/img/2021/03/0002-0002-cloned_kernel.png)


### 1.2 git 配置
-------


配置 git 需要配置用户名和邮箱

```
git config --global user.name "Zhang San"
git config --global user.email "zhangsan@163.com"
```

这些信息都将作为以后你提交补丁的 "身份戳".

![Signed-off-by](/kernel-portal/img/2021/03/0002-0003-signed_off_by.png)

待你提交补丁合入后, 你的大名将显示在 git 的 log 中.

另外 git 默认配置的编辑器为 `EMACS`, 如果你更习惯用 `VIM`, 可以通过如下命令设置 GIT 默认编辑器.

```
git config --global core.editor vim
```

配置完成后的信息, 都存储在本地 `~/.gitconfig`, 你也可以直接查看和修改该文件.
你可以可以通过 `git config --list` 查看你的 GIT 配置.


### 1.3 配置 git 邮箱服务器
-------

我们向 openEuler 提交补丁以及和社区的小伙伴们探讨问题, 都是通过邮件进行交流的.


推荐大家使用 git send-email 来发送补丁, 直接使用 yum(或者 apt) 安装即可

```
sudo yum install git-email
```

> 注意:
> send-email 并不是 git 的必备组件, 你可以使用 "git send-email" 确认一下.
> 如果正确显示了 send-email 的 help 信息, 那么 send-email 已经安装在你的系统了.
> 否则如果显示结果类似于下面:
> ```
> # git send-email
>
> git: 'send-email' is not a git command. See 'git --help'.
> ```
>
> 你需要安装 send-email 命令.你的版本可能有一个 send-email 的安装包.
> 一般来说这个包的名字都是 "git-email", 当前不同发行版可能会有差异.


使用 `git send-email` 命令发送补丁是通过你邮箱的 SMTP 服务进行的, 因此我们必须正确的配置 SMTP 服务.
以 163 邮箱为例, 配置信息如下所示:

```
# cat ~/.gitconfig
[user]
	name = Zhang San
	email = zhangsan@163.com

[core]
	editor = vim

[sendemail]
	from = "Zhang San <zhangsan@163.com>"
	smtpserver = smtp.163.com
	smtpuser = zhangsan@163.com
	smtpserverport = 25
	suppresscc = all
	confirm = always
	smtppass = xxxxxxxxxxx
```


附常用邮箱的 `POP/SMTP` 服务.

| 邮箱服务商 |  POP3 服务 | SMTP 服务 |
|:--------:|:------------:|:------------:|
| 163免费邮 | pop.163.com | smtp.163.com |
| 126免费邮 | pop.126.com | smtp.126.com |
| yeah.net免费邮 | pop.yeah.net | smtp.yeah.net |

> 注意, 目前国内的大多数邮箱为了安全, 都是默认关闭了 POP3/SMTP 等服务的, 我们需要自己手动打开. 可以参见网易提供的帮助文档或者查阅其他网络资源, 比如这篇 [网易163,126邮箱如何开启POP3/SMTP/IMAP服务](https://jingyan.baidu.com/article/3f16e003e327772591c1039f.html)
>
> 另: 服务器地址也可以在设置页面找到
>
> 另外 163 邮箱过程中, 还是用了授权码作为 SMTP 服务的验证密码, 因此如果开启了授权码, 那么 smtppass 字段填写的就是你的授权码而不是登陆密码.

可以先简单发一封邮件测试下

```
git send-email -to zhangsan@163.com ./xxxx.patch
```

![发送邮件测试](/kernel-portal/img/2021/03/0002-0004-send_email_test.png)



## 2 补丁制作与提交
-------

### 2.1 修改内核代码解决 bug 并提交
-------

这个过程我们不详细描述了, 大家把问题解决掉之后, 提交到本地, 然后验证, 待测试验证完成之后, 就可以把自己提交的修改制作成补丁然后进行提交.

### 2.2 补丁描述
-------

#### 2.2.1 补丁基本格式
-------

![补丁的基本格式](/kernel-portal/img/2021/03/0002-0005-patch_format.png)

`commit` 信息如下图所示, 包括下面 4 部分内容 :

| 格式  | 描述 | 是否 openEuler 独有 |
|:----:|:----:|:-------------:|
| 标题 |  简要描述这个提交完成了什么功能或修复了什么问题 | N |
| commit 头 | 补丁头是 openEuler 追溯补丁来源和功能的手段 | Y |
| commit 内容 | 具体描述这个 commit 为什么有必要做, 怎么修改的, 有没有其他要注意的等等  | N |
| 引用 or 修复 | 非必须, 主要是改补丁引用的内容, 如果是个修复补丁, 那么列出来你修复的问题是哪个补丁引入的 | N |
| 签名 | 包含了 Signed-off-by/Reviewed-by/Suggested-by/Reported-by/Tested-by 等等信息 | N |

关于 **签名**

| 签名 | 描述 |
|:---:|:----:|
| Signed-off-by | 这个是我们最常使用的签名戳, 它包含了很多的含义, 表明你是这个补丁的作者或者你对这个修改进行了同步, 正常来说, 一些主线的补丁我们回合后, 要在最后面带上我们自己的签名, 表明这个补丁是你审视了作者的修改之后, 重新发布的. 的需要注意的是, 最后一个 Signed-off-by 必须是提交补丁的开发人员.  |
| Reviewed-by | openEuler 各个模块的 Committer/Maintainer 对你的补丁进行 review 完成之后, 就会签上这个戳, 表明他们认可了你的修改, 只有看到了这个戳, Commiter/Maintainer 才有可能将此补丁合入 |
| Reported-by | 一般用于别的小伙伴发现的 BUG 或者问题等, 当其他兄弟完成修复后, 出于对上报问题的兄弟工作的认可, 带上这个戳把补丁发出来. 毕竟发现问题也是需要花费时间的, 大家可能解决起来比较棘手的问题, 都可以发出来让大家一起讨论和修复, 修复问题的时候带上上报人的戳, 即是对其辛勤功劳的肯定, 也是一种荣誉和认可. 毕竟只有多多的发现问题, openEuler 才能不断发展和壮大. 所以也希望大家发现问题多发出来讨论, 多上报. 如果你发现的问题被其他兄弟修复之后, 没有带你 Reported-by, 你可可以直接指出, 辛勤的劳动值得获得的认可. |
| Suggested-by | 表明这个修改思路是这位兄弟提出的, 但是是你实现的. 同样提出解决问题的方法, 也是要付出的, 理所应当获得认可 |
| Tested-by | 表明这个补丁被这位兄弟测试过了, 没有问题. 一些修复补丁或者特性补丁, 如果你比较感兴趣, 欢迎大家多多测试, 测试完成后, 恢复邮件, 签上这个戳. Committer/Maintainer 在提交的时候就会把的戳带上 |
| Co-developed-by | 协同开发, 表明这个补丁是由多个兄弟共同开发的. 他的身份是等同于作者的, 一般来说 PATCH 邮件的 From 字段在提交之后, 会默认作为 Author, 但是如果你希望表明其他人也参与了开发, 请使用此签名. |

关于**签名**的详细信息, 可以参考内核文档 [Documentation/process/submitting-patches.rst](https://gitee.com/openeuler/kernel/blob/kernel-4.19/Documentation/process/submitting-patches.rst) 或者[主线内核手册](https://www.kernel.org/doc/html/latest/process/submitting-patches.html).



关于 **引用和修复**

如果你的补丁是一个修复补丁, 那么我们一般建议在提交补丁的时候, 找到引入问题的那个 commit, 然后填写 Fixes 字段.

#### 2.2.2 openEuler 补丁格式要求
-------

openEuler 的补丁格式沿用了社区的格式, 但是 openEuler Kernel 为了对补丁和问题的来源和定位信息进行跟踪, 因此在补丁的 `commit message` 之前要求大家添加一些补丁头.

| 字段 | 描述 |
|:---:|:---:|
| XXX inclusion | 表明这个补丁是哪个组织或者机构提交的或有谁维护, 如果是主线补丁请填 mainline, 如果是 LTS 补丁, 请填写 stable, 自研补丁填写对应机构的的缩写 |
| category | 表明这个补丁的类别, 一般如下字段可选 : bugfix/performance/feature/doc |
| bugzilla | 用于跟踪此问题的详细信息和定位日志, 通过关联网站查阅此信息, 可以清楚的了解该补丁修改的问题, 或者该特性所完成的工作, 以及需求来源等信息.<br>所有你关于此补丁想知道的可能都在这里, 一般 bugziila 号或者 issue 号任选其一或者其二<br>bugzilla 字段填写 bugzilla 号或者 issue 号即可, 不需要填全部 URL 链接, bugzilla 和 issue 的网站是固定的, 有编号就可以查询到. |
| CVE | 如果当前补丁是 CVE 修复补丁, 则必须填写, 否则请填 NA |



自研特性, 请使用此格式提交

```
hulk inclusion
category: bugfix
bugzilla: https://gitee.com/openeuler/kernel/issues/I3916O
CVE: NA
```

主线补丁, 请使用此格式提交.


```
mainline inclusion
from mainline-v5.2-rc1
commit 898490c010b5d2e499e03b7e815fc214209ac583
category: bugfix
bugzilla: https://gitee.com/openeuler/kernel/issues/I390TB
CVE: NA
```


关于补丁头的更详细信息, 请参照 [openEuler Kernel 补丁合入规范](https://openeuler.gitee.io/kernel-portal/post/2021/03/0001-openeuler_patch_format_specification), 信息如有出入, 以合入规范为准.


### 2.3 补丁制作
-------

使用 `git show` 可以查看最近提交的 `commit`


#### 2.3.1 生成 patch
-------

使用 format-patch 生成 PATCH

| 参数 | 描述 |
|:---:|:----:|
| -s/--signoff | 添加自己的 Signed-off-by 戳, 自己提交的补丁, 不管是不是自己自研的, 都是要带上自己的签名的, 使用 -s 自动在 patch 最后添加自己的签名 |
| --subject-prefix "PATCH openEuler-1.0-LTS" | 为补丁添加前缀, 我们的补丁必须显式告诉 Committer/Reviewer/Maintainer 们, 补丁是为哪个版本或者分支提供的 |
| -N | 将前 N 补丁生成 PATCH 文件, 比如 -65 将生成 65 个 PATCH, 我们可以指定开始生成的位置, 指定 tag/commit id 等都可以 |
| -o XXXX | 将 PATCH 生成到 XXXX 目录下 |

使用 git format-patch 生成补丁文件


*	生成单个 patch

我们针对某个 commit 生成一个 PATCH 文件出来

```
git format-patch -s -1 9ced0cc25946
```

![生成 PATCH](/kernel-portal/img/2021/03/0002-0006-format_patch.png)


*	如何生成补丁集


对于某些比较大的改动，涉及多个模块，我们需要提交多次commit，我们通常将这些提交生成一个补丁集，使用的命令如下图所示.

```
git format-patch -s --cover-letter --subject-prefix "PATCH v3 openEuler-1.0-LTS" -10 -o  mpam/20210301/v3
```


![cover-letter](/kernel-portal/img/2021/03/0002-0007-format_patchset.png)


上面的命令, 将当前 HEAD 位置开始, 往前的 10 个补丁生成 PATCH 文件, 并输出到当前目录 mpam/20210301/v1 路径下, 补丁的标题前缀 "PATCH v3 openEuler-1.0-LTS", 表示这个补丁是为 openEuler-20.03-LTS 提供的, 并且已经是第 3 个版本.

`HEAD~3` 表示补丁集包括最近的3次提交

一般一组 patchset 我们会使用 `--cover-letter` 生成补丁集封面. 这个封面中

这样我们将在patch目录下生成一个补丁集封面 0000-cover-letter.patch，编辑该文件

修改第 1 个框中(`*** SUBJECT HERE ***`)的内容为对该补丁集的简要描述
在第2个框中(`*** BLURB HERE ***`)具体描述该补丁集.

> 注意 将 `***` 一起替换掉, 要不然你的标题叫 `*** fix XXXXXX ***` 也是蛮奇怪的.
这个封面补丁只是作者呈现补丁集用的, 方便大家对整个补丁集有一个直观上的认识. 在 apply 之后这个补丁并不会出现在 git log 中.

![cover-letter](/kernel-portal/img/2021/03/0002-0008-cover_letter.png)


> 注意我们可以指定 commit id 为 patchset 的结束位置
> 如果没有指定要生成的 PATCH 结束位置的 COMMIT 号, 则默认使用 HEAD.


*	changelog 信息

一般软件发布的时候, changelog 用来说明与上一个版本的差异, 对于内核 PATCH 来, 我们的版本在经历了几次修改之后, 也建议用 changelog 描述下相比较之前版本的修改点. 这有助于 Committer 对补丁的修改和逻辑有一个大致的了解.

一组 patchset 的时候, 一般我们把 changelog 写到封面补丁中, 在补丁集描述之后, 就填写 changelog 信息. 由于这个补丁不会出现在 git log 中, 自然也不用担心 changelog 出现在版本的 git log 中. 我们以内核主线社区 patchwork 中的 patchset 举例, [参见](https://lore.kernel.org/patchwork/cover/1340764)

对于单个 patch, 如果 changelog 直接写到 commit message 后面, apply 之后就会体现在 git log 中, 这自然不是我们想要的. 此时 git 为我们提供了一种更简单的方式. 在最后一个 Signed-off-by 之后, git 打印了几个短杠 "---", 在这几个短杠之后, 修改文件列表之前, 我们可以在这里畅所欲言, 而不会体现在 git log 中. 这里也是我们可以填写 changelog 的地方. 同样以主线 patchwork 中的 patch 为例, [参见](https://lore.kernel.org/patchwork/patch/1328176/)

#### 2.3.3 checkpatch 检查
-------

内核提供了 [`scripts/checkpatch.pl`](https://gitee.com/openeuler/kernel/blob/kernel-4.19/scripts/checkpatch.pl) 脚本用来对代码进行静态扫描, 主要用来识别一些不规范的编码.

*   使用 `script/checkpatch.pl XXXX.patch` 对生成的单个 patch 进行检查.

![检查单个 patch](/kernel-portal/img/2021/03/0002-0010-checkpatch_patch.png)

> 注意
> 检查出的部分内容是由于我们追加的补丁头等信息报错的, 这部分内容不用修改
> 比如上面检查出的如下错误:
>
> ```
> ERROR: Please use git commit description style 'commit <12+ chars of sha1> ("<title line>")' - ie: 'commit fatal: bad o ("778836857adfba3f8")'
> #8: 
> commit 1a999d25ef536a14f6a7c25778836857adfba3f8
>
> ERROR: Please use git commit description style 'commit <12+ chars of sha1> ("<title line>")' - ie: 'commit fatal: bad o ("7a702e7819e62f04e")'
> #13: 
> commit 8310b77b48c5558c140e7a57a702e7819e62f04e upstream.
> ```
>
> 这些问题都是由于内核编码规范中要求内核 commit 按照固定的格式描述, 但是我们的补丁头明显不符合这些格式.
> 这类问题我们忽略就好了.

*   也可以加上 `-f` 选项, 直接对文件进行检查. 这样在编码过程中, 就可以直接边检查边改了, 再检查完事后, 再生成 patch.

![检查单个文件](/kernel-portal/img/2021/03/0002-0011-checkpatch_file.png)

除了一些补丁头等引入的问题不需要修改, 其他问题按照要求提示修改即可.

关于内核编码规范, 可以参考 [`Documentation/process/coding-style.rst`](https://gitee.com/openeuler/kernel/blob/kernel-4.19/Documentation/process/coding-style.rst), 或者中文版 [Linux 内核代码风格](https://gitee.com/openeuler/kernel/blob/kernel-4.19/Documentation/translations/zh_CN/coding-style.rst).


#### 2.3.4 发送补丁到 openEuler linux 内核社区
-------

使用git send-email 发送邮件到openEuler社区的邮件列表 kernel@openeuler.org. 发送邮件时可以CC自己的邮箱，这样自己可以看到发出来的邮件是什么样子.发送到邮件列表的邮件会被订阅该邮件列表的所有人收到，包括维护该模块的maintainer，maintainer如果认为补丁没问题就会回复Reviewed-by，补丁会在后续合入openEuler.

```
git send-email –cc zhangsan@163.com --to kernel@openeuler.org 0001-arm64-fix-compile-error-when-CONFIG_ACPI-is-not-enab.patch --suppress-cc=all
```

建议大家发送的邮件都抄送相关 Committer 一份.

## 3 附录
-------


### 3.1 openEuler Kernel 分支介绍
------

20.03 版本(基于 4.19 内核)

| 分支 | 描述 |
|:----:|:---:|
| openEuler-1.0-LTS | 20.03 商用分支 |
| kernel-4.19 | 20.03 NEXT 分支, 可能包含了较多没有合入商用分支的补丁 |
| openEuler-20.09 | 20.09 创新分支 |


21.03 版本(基于 5.10 内核)

| 分支 | 描述 |
|:----:|:---:|
| openEuler-21.03 | 21.03 创新分支 |
| OLK-5.10 | 21.03 NEXT 分支 |

> 2020/03/01 21.03 版本目前还没有发布, 因此目前没有商用分支

### 3.2 bugzilla
-------

Bugzilla 是一个开源的缺陷跟踪系统(Bug-Tracking System), 各个 linux 社区或者发行版基本都会使用这套平台跟踪开发过程中的错误信息.

openEuler bugzilla 目前已经上线了, 大家可以使用这个平台提交各类问题.
地址: https://bugzilla.openeuler.org

### 3.3 issue
-------

issue的类型一般选择 "缺陷，标题格式为 "[分支名 架构] 具体问题描述xxx".


https://gitee.com/openeuler/kernel/issues

我们可以向 openEuler linux内核提一个issue, 如下图所示.

![提交 issue](/kernel-portal/img/2021/03/0002-0009-create_issue.png)


**温馨提示**
issue 请在 **[openeuler/kernel](https://gitee.com/openeuler/kernel)** 仓库创建
**不要**在 **[src-openeuler/kernel](https://gitee.com/src-openeuler/kernel)** 仓库创建
src-openeuler/kernel 是 openEuler 用于构建 kernel 镜像和 RPM 包的 SPEC 仓库, 并不是内核仓库.

### 3.4 邮件列表
-------


为了更好更参与openEuler社区，我们可以通过下面的网址订阅openEuler社区邮件列表，点击 Kernel-discuss 或者 Kernel，填入名字和邮箱即可订阅.


订阅邮件列表后，我们也可以收到其他参与者发到openEuler linux内核社区的补丁，与其他人一起讨论补丁内容，协作开发.

| 邮件列表 | 地址 | 归档地址 | 描述 |
|:-------:|:---:|:-------:|:---:|
| openEuler Kernel | kernel@openeuler.org | [Archive](https://mailweb.openeuler.org/hyperkitty/list/kernel@openeuler.org) | kernel 的主邮件列表, 主要是 kernel patch 的提交和讨论. 其他关于 kernel 的所有信息都可以发送到此邮件列表 |
| Kernel-discuss | kernel-discuss@openeuler.org | NA | 主要用于讨论内核相关问题, 鉴于 kernel 主邮件列表邮件可能较多, 喜欢清静, 只想静静的讨论技术的可以订阅此邮件列表. |

查找 openEuler 更多邮件列表, [请前往](https://openeuler.org/zh/community/mailing-list)

## 4 参考资料
-------

[Contributions to openEuler Kernel project](https://gitee.com/openeuler/kernel/blob/kernel-4.19/README)

[openEuler Kernel SIG](https://gitee.com/openeuler/community/tree/master/sig/Kernel)
