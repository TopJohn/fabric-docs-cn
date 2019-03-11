Gerrit 最佳实践
============================

本文档介绍了一些帮助您更有效地使用Gerrit的最佳实践。目的是展示如何轻松提交内容。
使用建议的做法可以减少故障排查的实践，提高社区参与度。

浏览Git Tree
---------------------

访问
`Gerrit <https://gerrit.hyperledger.org/r/#/admin/projects/fabric>`__
然后选择 ``Projects --> List --> SELECT-PROJECT --> Branches`` 。
选择你感兴趣的分支，点击右边的 ``gitweb`` 。 
现在， ``gitweb`` 将你的选择载入到了Git Web界面上来了。

关注一个项目
------------------

浏览
`Gerrit <https://gerrit.hyperledger.org/r/#/admin/projects/fabric>`__ ，
选择右上角的 ``Settings`` 。选择
``Watched Projects`` 然后添加你感兴趣的项目。

提交信息
---------------

Gerrit遵循Git提交消息的格式。确保头部位于底部，并且彼此之间不包含空行。
以下示例显示了提交信息中预期的格式和内容：


简短的（不超过55个字符）的一行描述。

精简表明为什么（动机）的描述，变更做了什么，以及怎么样测试的。
任何对文档所做的变更需要与代码保持一致，每行需要在72个字符以内。


| Jira: FAB-100
| Change-Id: LONGHEXHASH
| Signed-off-by: Your Name your.email\@example.org
| AnotherExampleHeader: An Example of another Value

Gerrit服务器提供了一个提交前的钩子来自动生成使用的Change-Id。

**推荐阅读:** `如何编写Git提交消息 <https://chris.beams.io/posts/git-commit/>`__

避免将未经测试的代码推送到Gerrit服务器
----------------------------------------------

避免将未经测试的代码推送到Gerrit。

在将更改推送到Gerrit之前，请至少确认三遍。注意你要提交的信息。

更踪变更
------------------------

-  设置Gerrit给你发电子邮件：

-  如果一个开发者把你添加为审查者或者你评论了某个补丁，Gerrit会将你添加到变更的邮件列表中。

-  在Gerrit审核页面打开一个变更是一种快速跟踪变更的方法。

-  在 ``Gerrit`` 上关注Gerrit上的项目，至少选择一个 *New Changes, New Patch Sets, All Comments* and *Submitted
   Changes* 其中一个。

始终追踪你正在进行的项目；同时关注邮件列表里的反馈和评论，以此来学习护着帮助他人提升。

主题分支
--------------

主题分支是临时分支，你可以推送一组逻辑依赖的提交：

将更改从 ``REMOTE/master`` 树推送到Gerri 图,以便在 **TopicName** 中作为主题进行审核，
请使用以下命令作为示例：

::

    $ git push REMOTE HEAD:refs/for/master/TopicName

该主题将会出现在评论 ``UI`` 和 ``Open Changes List`` 中。
主题分支在被合并到master分支之后将会消失。

为主题创建一个说明信
-----------------------------------

你可以决定说明信说否出现在历史中。

1. 要让说明信显示在历史中，请使用下述命令：

::

    git commit --allow-empty

编辑提交信息，这个信息将成为说明信。使用的命令不改变源树中的任何文件。

2. 如果不需要说明信出现在历史中，请参照下述步骤：

-  | 将空提交放在提交列表的末尾，以便可以忽略它
   | 不需要任何修改。

-  现在添加你的提交

::

    git commit ...
    git commit ...
    git commit ...

-  最终，提交推送到主题分支。以下命令是一个示例：

::

    git push REMOTE HEAD:refs/for/master/TopicName

如果你已经有提交但是你想设置说明信，
请为说明信创建一个空提交并移动提交，使其成为最后一个提交。使用下面命令作为示例：

::

    git rebase -i HEAD~#Commits

移动之前请小心取消注释。 ``#Commits`` 是你评论的总和加上你的说明信。


查找可用的主题
------------------------

::

       $ ssh -p 29418 gerrit.hyperledger.org gerrit query \ status:open project:fabric branch:master \
       | grep topic: | sort -u

-   `gerrit.hyperledger.org <https://gerrit.hyperledger.org>`__ 是当前项目托管的URL。

-  *status* 指示主题的当前状态：打开，合并，放弃，草稿，合并冲突。

-  *project* 指示了项目的当前名字，在本例中是fabric。

-  *branch* 在branch中搜索主题。

-  *topic* 特定主题的名字，如果为空则表示所有主题。

-  *sort* Sorts the found topics, in this case by update (-u).

-  *sort* 对找到的主题进行排序，在本例中为update(-u)。

下载或者检出变更
------------------------------------

在审查界面中的右上角， **Download** 链接提供了一系列的命令和超级链接来检出和下载差异或文件。

我们建议使用 *git review* 插件。安装git review的步骤超出了本文档的范围。
有关安装过程，请参阅 `git review
文档 <https://wiki.openstack.org/wiki/Documentation/HowTo/FirstTimers>`__ 。

使用Git检出特定变更，请参照下述命令：

::

    git review -d CHANGEID

如果你没有安装Git-review，下面的命令可以做相同的事：

::

    git fetch REMOTE refs/changes/NN/CHANGEIDNN/VERSION \ && git checkout FETCH_HEAD

举个例子，对于第四版更改 2464，NN是前两位数（24）：

::

    git fetch REMOTE refs/changes/24/2464/4 \ && git checkout FETCH_HEAD

使用草稿分支
--------------------

你可以在发布更改之前使用草稿分支添加特定的审阅者。
草稿分支被推送到 ``refs/drafts/master/TopicName`` 。

下一个命令是确保创建了本地分支：

::

    git checkout -b BRANCHNAME

下一个命令将你的更改推送到 **TopicName** 下的草稿分支：

::

    git push REMOTE HEAD:refs/drafts/master/TopicName

使用沙箱分支
----------------------

你可以创建你自己的分支来开发新功能。
分支被推送到 ``refs/sandbox/USERNAME/BRANCHNAME`` 。

这些命令确保在Gerrit服务器上创建了分支。

::

    git checkout -b sandbox/USERNAME/BRANCHNAME
    git push --set-upstream REMOTE HEAD:refs/heads/sandbox/USERNAME/BRANCHNAME


通常，创建内容的过程是：

-  开发代码，
-  把信息分成小的提交，
-  提交变更，
-  等待反馈，
-  rebase。

下面的命令在没有审阅的情况下，强制推送了提交：

::

    git push REMOTE sandbox/USERNAME/BRANCHNAME

你也可以通过审查进行提交:

::

    git push REMOTE HEAD:ref/for/sandbox/USERNAME/BRANCHNAME

更新变更的版本
--------------------------------

在审查的过程中，你可能会被要求更新你的变更。可以对同一个变更提交多个版本。
每个变更的版本叫做补丁集。

Always maintain the **Change-Id** that was assigned. For example, there
is a list of commits, **c0...c7**, which were submitted as a topic
branch:

始终保持分配的 **Change-Id** 。举个例子，有一系列提交， **c0...c7** ，
作为主题分支的提交：


::

    git log REMOTE/master..master

    c0
    ...
    c7

    git push REMOTE HEAD:refs/for/master/SOMETOPIC

在获得审查者的反馈后， **c3** 和 **c4** 中的2个提交必须被修复。
如果修复需要rebase，rebase改变了提交Id，参考
`rebasing <https://git-scm.com/book/en/v2/Git-Branching-Rebasing>`__ 
获取更多信息。但是，你必须保持相同的Change-Id，并再次推送更改：

::

    git push REMOTE HEAD:refs/for/master/SOMETOPIC

这个新的推送创建了一个补丁修订，然后清除你本地的历史记录。
但是，对于每个更改，你可以在 ``review UI`` 中访问你的变更历史。

在推送新版本的时候，同样允许添加更多提交。

Rebasing
--------

Rebasing通常是杂i 推送变更到Gerrit的最后一步；这允许你进行必要的 *Change-Ids* 。
*Change-Ids* 一定要保持一致。

-  **squash:** 合并2个或多个提交为一个提交。
-  **reword:** 改变提交信息。
-  **edit:** 改变提交内容。
-  **reorder:** 允许你改变提交顺序。
-  **rebase:**将提交堆叠到主分支上。

在Pull的时候进行Rebasing
----------------------

在推送rebase到主分支之前，确保历史有一个连续的排序。

例如，你的 ``REMOTE/master`` 有从  **a0** 到  **a4**的提交， **c0...c7** 在  **a4** 上面；
从而：

::

    git log --oneline REMOTE/master..master

    a0
    a1
    a2
    a3
    a4
    c0
    c1
    ...
    c7

 如果 ``REMOTE/master``  上收到了 **a5**, **a6** and **a7** 。
 可以使用以下方式进行rebase：

::

    git pull --rebase REMOTE master

这会先拉取 **a5-a7** 然后重新应用 **c0-c7** 在它们的顶部：

::

       $ git log --oneline REMOTE/master..master
       a0
       ...
       a7
       c0
       c1
       ...
       c7

从Git获取更好的日志
----------------------------

使用这些命令更改Git的配置，从而产生更好的日志：

::

    git config log.abbrevCommit true

这个命令将命令日志设置为缩写提交的哈希值。

::

    git config log.abbrev 5

The command above sets the abbreviation length to the last 5 characters
of the hash.

这个命令设置缩写的长度为哈希值的最后5个字母。

::

    git config format.pretty oneline

上面的命令避免在作者行之前插入不必要的行。


要为当前用户进行这些配置更改，必须为 ``config`` 添加path选项 ``--global`` ，如下所示：

::

    git config --global

.. Licensed under Creative Commons Attribution 4.0 International License
   https://creativecommons.org/licenses/by/4.0/

