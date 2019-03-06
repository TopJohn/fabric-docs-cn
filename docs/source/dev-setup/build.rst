构建Hyperledger Fabric
---------------------------

下面的指南假设你已经设置好了你的
:doc:`开发环境 <devenv>`  。

来构建Hyperledger Fabric：

::

    cd $GOPATH/src/github.com/hyperledger/fabric
    make dist-clean all

运行单元测试
~~~~~~~~~~~~~~~~~~~~~~

在运行单元测试之前，需要配置安装PKCS #11的加密令牌套件。
PKCS #11的API在Fabric的bccsp组件中使用，像硬件安全模块（HSMs），用于存储和加密计算。
在测试环境中，SoftHSM可以满足需求。

SoftHSM可以通过如下命令安装：

::

    sudo apt install libsofthsm2 # Ubuntu
    sudo yum install softhsm     # CentOS
    brew install softhsm         # macOS

一旦SoftHSM安装了，可能需要其他配置。
举个例子，默认的配置文件在用户无法写入的系统文件路径中存储了令牌数据。

配置典型地包括复制 ``/etc/softhsm2.conf`` 到``$HOME/.config/softhsm2/softhsm2.conf`` 
改变 ``directories.tokendir``
到合适的位置。请查看 ``softhsm2.conf`` 的man页面来获取更详细的信息。

在SoftHSM配置好了之后，下面的命令可以用于初始化需要的令牌：

::
    softhsm2-util --init-token --slot 0 --label "ForFabric" --so-pin 1234 --pin 98765432

如果测试在你的环境中找不到libsofthsm2.so，请通过环境变量指定令牌的路径，PIN以及label。
举个例子，在macOS中：

::
    export PKCS11_LIB="/usr/local/Cellar/softhsm/2.5.0/lib/softhsm/libsofthsm2.so"
    export PKCS11_PIN=98765432
    export PKCS11_LABEL="ForFabric"

使用下列的顺序来运行所有的单元测试：

::

    cd $GOPATH/src/github.com/hyperledger/fabric
    make unit-test

如果只需要测试一部分单元测试，请设置TEST_PKGS环境变量。
指定了一部分包（根据空间划分），举个例子：

::

    export TEST_PKGS="github.com/hyperledger/fabric/core/ledger/..."
    make unit-test

为了运行具体的单元测试，请使用 ``-run RE`` 标识符，RE是一个正则表达式，可以匹配一些用例的名字。
要显示详细的过程，请使用 ``-v`` 标识。举个例子，要运行 ``TestGetFoo`` 测试用例，
移动到包含 ``foo_test.go``文件的目录，然后执行

::

    go test -v -run=TestGetFoo


运行 Node.js 客户端SDK的单元测试
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

你需要运行Node.js的单元测试，以此来保证 Node.js client SDK没有因为你的修改而崩溃。
要运行Node.js的单元测试，请遵循下述指南
`here <https://github.com/hyperledger/fabric-sdk-node/blob/master/README.md>`__ 。

配置
-------------

配置采用 `viper <https://github.com/spf13/viper>`__
和 `cobra <https://github.com/spf13/cobra>`__ 库来实现。

peer包括一个 **core.yaml** 配置文件。
很多配置可以被带有 *'CORE\_'* 前缀的环境变量覆盖。
举个例子，日志等级通过环境变量操作：

::

    CORE_PEER_LOGGING_LEVEL=CRITICAL peer

.. Licensed under Creative Commons Attribution 4.0 International License
   https://creativecommons.org/licenses/by/4.0/
