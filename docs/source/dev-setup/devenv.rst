设置开发环境
--------------------------------------

前置工作
~~~~~~~~~~~~~

-  `Git 客户端 <https://git-scm.com/downloads>`__
-  `Go <https://golang.org/dl/>`__ - 版本 1.11.x
-  (macOS)
   `Xcode <https://itunes.apple.com/us/app/xcode/id497799835?mt=12>`__
   必须安装
-  `Docker <https://www.docker.com/get-docker>`__ - 17.06.2-ce 或者更高
-  `Docker Compose <https://docs.docker.com/compose/>`__ - 1.14.0 或者更高
-  (macOS) 你必须安装gnutar,macOS默认使用bsdtar，
   但是构建使用了一些gnutar的标识。 
   你可以使用Homebrew来安装它:

::

    brew install gnu-tar --with-default-names

-  (macOS) `Libtool <https://www.gnu.org/software/libtool/>`__ 。
    你可以使用Homebrew来安装它：

::

    brew install libtool

-  (只有使用Vagrant的需要安装) - `Vagrant <https://www.vagrantup.com/>`__ -
   1.9 或者更高
-  (只有使用Vagrant的需要安装) -
   `VirtualBox <https://www.virtualbox.org/>`__ - 5.0 或者更高
-  BIOS 支持虚拟化 - 因硬件而异

-  注意: BIOS虚拟化设置可能在BIOS的CPU或者Security setting里


步骤
~~~~~

设置GOPATH
^^^^^^^^^^^^^^^

确保你已经设置了你主机的 
`GOPATH 环境变量 <https://github.com/golang/go/wiki/GOPATH>`__ 。
只有设置了环境变量才能在你的主机或者虚拟机中编译构建。

如果你安装的Go不在默认位置，确保你设置了
 `GOROOT环境变量 <https://golang.org/doc/install#install>`__ 。

Windows用户请注意
^^^^^^^^^^^^^^^^^^^^^

如果你用的是Windows系统，在运行 ``git clone`` 命令之前，请先运行下列命令。

::

    git config --get core.autocrlf

如果 ``core.autocrlf`` 设置 ``true``, 你必须将它设置为 ``false`` 

::

    git config --global core.autocrlf false

如果你继续设置 ``core.autocrlf`` 为 ``true`` ，
``vagrant up`` 命令将会报错:

``./setup.sh: /bin/bash^M: bad interpreter: No such file or directory``

克隆Hyperledger Fabric项目源代码
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Since Hyperledger Fabric is written in ``Go``, you'll need to
clone the source repository to your $GOPATH/src directory. If your $GOPATH
has multiple path components, then you will want to use the first one.
There's a little bit of setup needed:

因为Hyperledger Fabric是用 ``Go`` 写的，所以你需要将它克隆到$GOPATH/src目录。
如果你的$GOPATH有多个，那么请选择第一个$GOPATH。
这里有一些需要设置：

::

    cd $GOPATH/src
    mkdir -p github.com/hyperledger
    cd github.com/hyperledger

Recall that we are using ``Gerrit`` for source control, which has its
own internal git repositories. Hence, we will need to clone from
:doc:`Gerrit <../Gerrit/gerrit>`.
For brevity, the command is as follows:

重申一下，我们使用 ``Gerrit`` 来做代码的控制， ``Gerrit`` 内部有自己的git仓库。
因此我们需要从
:doc:`Gerrit <../Gerrit/gerrit>` 来克隆。

::

    git clone ssh://LFID@gerrit.hyperledger.org:29418/fabric && scp -p -P 29418 LFID@gerrit.hyperledger.org:hooks/commit-msg fabric/.git/hooks/

**注意:** 当然你要用将 ``LFID`` 替换为你的
:doc:`Linux Foundation ID <../Gerrit/lf-account>` 。

.. Licensed under Creative Commons Attribution 4.0 International License
   https://creativecommons.org/licenses/by/4.0/

