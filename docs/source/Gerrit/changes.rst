提交一个变更到Gerrit
=============================

在向Hyperledger Fabric代码库提交代码变更之前，请仔细阅读下述内容。
这些指南适用于开源新手，同样也适用于经验丰富的开源开发人员。

变更要求
-------------------

这节包含了提交用于审核的代码变更的指南。关于如何使用Gerrit提交变更的信息，请参考
:doc:`使用Gerrit进行工作 <gerrit>` 。

所有对Hyperledger Fabric的变更都是利用Gerrit通过Git commits进行提交的。
每个提交必须包括：

-  一个简短的描述性的主题行，不超过55个字符，后面跟上一行空行。
-  包含逻辑的描述或者变更的原因说明，后面再跟一行空行。
-  一行Signed-off-by，跟在(Signed-off-by:)后面。
-  一行Change-Id标识行，跟在(Change-Id:)后面。Gerrit不会接受没有这个标识的行。

一个具备以上格式的提交被认为是一个良好的提交。

.. note:: 你不需要为新的提交提供Change-Id标识符； ``commit-msg`` 仓库关联的
          Git钩子会自动添加。如果你接下来修改了之前的提价并且重新提交了，
          然后使用与初始提交相同的Change-Id将保证Gerrit识别后续提交为与之前提交相关的修改提交。

发送给Gerrit的所有变更和主题必须是符合上述格式的。
除了上述的强制的格式之外，一个提交消息需要包括：

-  **what** 变更做了什么，
-  **why** 为什么你选择这种方法，
-  **how** 你怎么知道这种方法可行---例如，你运行了哪些测试。

提交必须 :doc:`build cleanly <../dev-setup/build>` ，从而避免破坏可分割性。
每个提交都需要有一个JIRA标识，在逻辑上独立。举个例子，一个提交可能会修复一个空白问题，
另一个提交可能会重命名一个函数，第三个提交可能会修改一些代码的功能。

下面详细说明了一个格式良好的提交：

::

    [FAB-XXXX] purpose of commit, no more than 55 characters

    You can add more details here in several paragraphs, but please keep
    each line less than 80 characters long.

    Change-Id: IF7b6ac513b2eca5f2bab9728ebd8b7e504d3cebe1
    Signed-off-by: Your Name <commit-sender@email.address>


``Signed-off-by:`` 行中的名字和邮箱必须和变更的作者信息相匹配。
请确保你的 ``.gitconfig`` 文件是正确的。

如果变更中包含了用以其他变更的代码，但是现在还不是最终的集合，请将这些信息告知审核者。

检查你的变更是否在CI校验通过了
-------------------------------------------------------------

为了保证代码的稳定性并限制可能的回归，我们使用基于Jenkins的持续集成（CI）流程，
该流程触发多个平台上的构建，并针对变更请求运行测试。你也有责任检查你的CR是否通过了测试。
在没有通过测试的前提下CR是不会被合并的。
在CRs通过CI的测试之前，你不应该期望任何人对你的CR作出回应。

为了检查CI的进度，只需要按照上一中推送给你的URL，在Gerrit上进行查看。
页面底部的“历史记录”会显示正在执行的CI过程对应的“Hyperledger Jobbuilder”所采取的一组操作。

完成后，如果成功了 "Hyperledger Jobbuilder" 会添加一个 a *+1 投票* ，否则则添加一个 *-1 投票* 。

如果失败了，请浏览CR历史记录的链接。如果你发现CR有问题，那么请修改你的提交，并将它进行更新，
这样它会再次启动CI流程。

如果你发现你的CR没有问题，那么可能是CI过程因为某些和你的CR无关的原因导致的失败。
在这种情况下，你可能希望使用简单的 "reverify" 来重新启动CI过程。
有关此问题的更多信息和选项，请查看
 `CI 管理页面
<https://github.com/hyperledger/ci-management/blob/master/docs/source/fabric_ci_process.rst>`__ 。

.. Licensed under Creative Commons Attribution 4.0 International License
   https://creativecommons.org/licenses/by/4.0/
