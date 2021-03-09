---

title: openEuler kernel 补丁合入规范
date: "2021-03-01"
author: openEuler kernel SIG
thumbnail:
blogexcerpt: openEuler kernel 是目前国内最具活力的的内核社区，自发布以来一直保持着比较高的热度. openEuler kernel 为了内核版本质量，在尽可能方便大家参与社区的前提下，加强了对合入补丁的管控，保证补丁安全、可信、可追溯，因此对 openEuler Kernel 合入补丁的方式和格式都做了一些约束和建议，本规范旨在为大家合入补丁提供必要的指导。

description: "openEuler kernel 是目前国内最具活力的的内核社区，自发布以来一直保持着比较高的热度. openEuler kernel 为了内核版本质量，在尽可能方便大家参与社区的前提下，加强了对合入补丁的管控，保证补丁安全、可信、可追溯，因此对 openEuler Kernel 合入补丁的方式和格式都做了一些约束和建议，本规范旨在为大家合入补丁提供必要的指导。"
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


阅读本规范之前，请先了解 [openEuler 内核的分支管理](https://gitee.com/openeuler/community/tree/master/sig/Kernel#kernel-的分支)。


当前版本 v1.0 @ 2021-03-01

| 更新日期 | 更新日志 |
|:-------:|:-------:|
| 2021/02/26 | 谢秀奇 完成 v0.9 版本 |
| 2021/03/03 | 成坚 完成 v1 版本    |


# 1 目的
-------

为规范和统一 openEuler 内核补丁的提交流程、方式、格式，方便对补丁/特性的分类、识别、统计、追溯以及移植，特制定openEuler kernel 补丁合入规范（以下简称“本规范”）。

# 2 适用范围

-------


本规范适用于 openEuler 在开发以及已布的所有版本的开发过程。

包括但不限于以下版本:

* openEuler kernel 4.19 内核开发；

* openEuler kernel 5.10 内核开发；

# 3  对补丁的基本要求

-------

openEuler 的目标是：

* 成为技术创新的平台，加速技术创新、成熟及落地应用

* 维护安全稳定可靠的、性能领先、生态丰富的的稳定内核，方便产业界快速应用

因此，满足这个原则的补丁或特性都可以提交给 openEuler 内核。

## 3.1 贡献openEuler社区之前需要签署贡献者协议 （CLA)

https://openeuler.org/zh/community/contribution/

## 3.2 补丁格式要求

【原则】

原则1：补丁的来源应当清晰、明确

原则2：代码的 licence 须满足开源要求（GPL v2)

原则3：补丁应遵循 Linux 内核社区的编码规范

原则4：补丁 Commit Message 应该清晰完整，有利于判断版本是否需要，有利于补丁在多版本间移植

原则5：补丁有适当的分类标识，便于识别和统计

原则6：补丁应有标识，能区分是发给哪个版本/分支的补丁



【规则】

规则1：补丁标题需要标识要推送到哪个分支，分支表示正确，才能触发 CI 检查。如

```[PATCH OLK-5.10]  表示是向 OLK-5.10 分支推送补丁```

规则2：如果补丁需要修改之后再次发送，需要在补丁标题中标注版本，如

```[PATCH OLK-5.10 v2]```

规则3：补丁需要有来源标识，比如 mainline inclusion, stable inclusion，dist inclusion 等。下表给出分类建议，不在列表中的，可以自行选择一个有辨识度的标识，或者使用 openEuler inclusion.

| 来源标识              | 说明                                |
| --------------------- | ----------------------------------- |
| mainline inclusion    | 主线 backport                       |
| stable inclusion      | 上游 stable 分支的补丁              |
| maillist inclusion    | 发到社区，但是还没进主线的补丁      |
| hulk inclusion        | openEuler kernel 团队开发的补丁     |
| virt inclusion        | 虚拟化相关的补丁                    |
| ascend inclusion      | 昇腾相关的补丁或特性                |
| kunpeng inclusion     | 鲲鹏相关的补丁或特性                |
| dist inclusion        | 从其他发行版移植的补丁              |
| raspberrypi inclusion | 树莓派相关的补丁合入                |
| openEuler inclusion   | 其他发送到openEuler社区的非上游补丁 |

规则4: 补丁应有分类标识，一般分为 bugfix，performance，feature，doc.

规则5：如果补丁来说上游主线社区，需要指定来自主线哪个版本，如 

```from mainline-5.11```

规则6：如果补丁来自上游 stable 分支，需要指定来自 stable 哪个版本，如

```from stable-5.10.18```

规则7：如果补丁来自上游社区，主线或stable分支，需要指定补丁的 commit id，如

```commit 0becc0ae5b42828785b589f686725ff5bc3b9b25```

规则8：每个补丁或补丁集需要指定 bugzilla 链接，用例说明该补丁解决了什么问题，或新增了什么特性。如果一个特性补丁集，或回合 stable 的补丁，可以使用相同的bugzilla链接标识一组补丁集。如

```bugzilla: https://bugzilla.openeuler.org/show_bug.cgi?id=6```

规则9：补丁需要有分类标识，如 category: bugfix, category: feature 等

规则10：如果补丁是 CVE 的修复补丁，需要指定 CVE 编号， 如 CVE: CVE: CVE-2020-36158

规则11：如果补丁是来自内核社区还没进主线的补丁，需要给出补丁或补丁集链接，如：

```Reference: https://lwn.net/Articles/839107/```

建议12：回合社区Bugfix补丁时，可以在原补丁commit message 之前，增加额外的描述，一遍能更好的理解为什么需要合入该补丁。

规则13：回合社区补丁时，commit message 中新增的部分，与原始部分，需要有分割线，如

```mainline inclusion      [M]
from $mainline-version
commit $id
category: $category
bugzilla: $bug-id
CVE: $cve-id

additional changelog

--------------------------------

original changelog

Signed-off-by: $yourname <$yourname@huawei.com>   [M]
```

规则14: 不论是自己开发的补丁，还是从社区回合的补丁，还是帮团队其他人员转发的补丁，都需要增加自己的签名。下面说明摘自内核补丁提交说明（文档位置：Documentation/process/submitting-patches.rst）。

```
Sign your work - the Developer’s Certificate of Origin
   As the same of upstream kernel community, you also need to sign your patch.

   See: https://www.kernel.org/doc/html/latest/process/submitting-patches.html

   The sign-off is a simple line at the end of the explanation for the patch,
   which certifies that you wrote it or otherwise have the right to pass it
   on as an open-source patch. The rules are pretty simple: if you can certify
   the below:

   Developer’s Certificate of Origin 1.1
   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

     By making a contribution to this project, I certify that:

     (a) The contribution was created in whole or in part by me and I have
         the right to submit it under the open source license indicated in
         the file; or

     (b  The contribution is based upon previous work that, to the best of
         my knowledge, is covered under an appropriate open source license
         and I have the right under that license to submit that work with
         modifications, whether created in whole or in part by me, under
         the same open source license (unless I am permitted to submit under
         a different license), as indicated in the file; or

     (c) The contribution was provided directly to me by some other person
         who certified (a), (b) or (c) and I have not modified it.

     (d) I understand and agree that this project and the contribution are
         public and that a record of the contribution (including all personal
         information I submit with it, including my sign-off) is maintained
         indefinitely and may be redistributed consistent with this project
         or the open source license(s) involved.

   then you just add a line saying:

     Signed-off-by: Random J Developer <random@developer.example.org>

   using your real name (sorry, no pseudonyms or anonymous contributions.)
```

规则15: 自己开发的补丁，需要经过 ./scripts/checkpatch.pl 工具检查，不能有 Error 项。

建议16: 你可以在补丁的 commit message 中标记补丁要回合到的维护分支，如果补丁没有冲突，maintainer 会自动将其合到对应的分支。

  * 例如，下面这一行信息，表示该补丁需要回合到 openEuler-20.03-LTS-SP1，openEuler-20.03-LTS-SP2两个分支中。

    ```backport: openEuler-20.03-LTS-SP1, openEuler-20.03-LTS-SP2```

## 3.3 补丁的维护与生命周期

-------

1. 少数遇到以下情况，已经合入openEuler 内核的补丁或特性可能会被回退或删除：

  * 发现 license 不合规，或代码来源不合规等问题

  * 有严重安全问题，经提醒，仍没有整改

  * 后续有补丁的新版本，或其他特性有冲突的，经决策后，可能会被替代或删除

2. 内核升级大版本，如从 4.19 升级到 5.10，上游主线没有包含的补丁，需要原补丁开发团队重新分析合入。

3. LTS 维护分支 （openEuler-xx.xx)，通常不直接接受补丁，只从开发主干（OLK-X.XX)回合的补丁。
4. 补丁只合入了创新版本，不一定能进入 LTS 版本和主干。并且，如果补丁要合入主干和LTS分支，需要重新适配发送补丁。

# 4 流程要求

-------

1.  新特性、新驱动、补丁集回合等，需要现在 [bugzilla](https://bugzilla.openeuler.org/enter_bug.cgi) 上提交 request，对特性及改动做简单的介绍。
2.  明显的、少量的bugfix、独立的bugfix，可以在 bugzilla 上提交问题之后，描述补丁分析情况或验证情况，直接按前面要求发补丁。
