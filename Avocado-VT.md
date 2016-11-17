# About Avocado-VT
Avocado-VT是一个兼容的插件，可以让你执行虚拟化相关测试（正如所知的virt-test），Avocado为其提供了所有的便捷。

它的主要用途是给virt开发者提供一个自动化的回归分析测试服务，去做virt技术（在服务器测试基础设施上使用它）的定期的自动化测试

Avocado-VT目的是根据大多virt功能和性能测试需要来做一个集中的项目，我们总结如下：

 - 客户系统安装，window（winXP-win7），Linux（RHEL，Fedora，OpenSUSE，其他的通过步骤引擎机器的系统）
 - Linux客户的串行输出
 - Migration, networking, timedrift ，其它类型的测试

对于 qemu subtests，我们可以这样做：

 - 监视器控制human和QMP协议
 - 建立和使用qemu使用各种函数方法 (source tarball, git repo, rpm)
 - 进行一定程度的性能测试
 - KVM单元测试可以轻松从virt-test内部运行,我们有完整的集成的unittest执行

我们支持带有硬件虚拟化支持（AMD 和 Intel）的x84_64的主机，支持Intel 32和64位的客户机操作系统。

## About virt-test
Virt-test变成了Avocado-VT项目。以前在Autotest项目下面，在：http://github.com/autotest/virt-test
这个软件库现在被冻结了，并且因为一些历史性目的现在只有在本地可用。

# Getting Started
使用Avocado-VT的第一步就是去安装它。

## Installing Avocado

从[this link][1]的说明开始。

## Installing Avocado-VT
安装了Avocado之后，你应该已经使能了正确的软件库。

Note：
如果你是通过源码使用avocado，像[here][2]描述的那样做一个链接。

### Fedora and Enterprise Linux
在Fedora或者Enterprise Linux安装Avocado-VT最重要的就是安装avocado-plugins-vt数据包，安装命令：

``` stylus
$ yum install avocado-plugins-vt
```
### Bootstrapping Avocado-VT
在数据包之后，一个引导程序必须启动。选择你的测试后端（qemu，libvirt，v2v，openvswitch，etc）并且启动vt-bootstrap命令。例如：

``` stylus
$ avocado vt-bootstrap --vt-type qemu
```
输出应该类似于：

``` stylus
12:02:10 INFO | qemu test config helper
12:02:10 INFO |
12:02:10 INFO | 1 - Updating all test providers
12:02:10 INFO |
12:02:10 INFO | 2 - Checking the mandatory programs and headers
12:02:10 INFO | /bin/7za OK
12:02:10 INFO | /sbin/tcpdump OK
...
12:02:11 INFO | /usr/include/asm/unistd.h OK
12:02:11 INFO |
12:02:11 INFO | 3 - Checking the recommended programs
12:02:11 INFO | /bin/qemu-kvm OK
12:02:11 INFO | /bin/qemu-img OK
12:02:11 INFO | /bin/qemu-io OK
...
12:02:33 INFO | 7 - Checking for modules kvm, kvm-intel
12:02:33 DEBUG| Module kvm loaded
12:02:33 DEBUG| Module kvm-intel loaded
12:02:33 INFO |
12:02:33 INFO | 8 - If you wish, you may take a look at the online docs for more info
12:02:33 INFO |
12:02:33 INFO | http://avocado-vt.readthedocs.org/
```
如果这边有丢失的设备，请安装他们并且重新启动vt-bootstrap。

## First steps with Avocado-VT
检查Avocado插件列表是否正常：

``` stylus
$ avocado plugins
```

这个命令应该展示出加载的插件，希望没有错误。相关的行应该有：

``` stylus
Plugins that add new commands (avocado.plugins.cli.cmd):
vt-bootstrap Avocado VT - implements the 'vt-bootstrap' subcommand
...
Plugins that add new options to commands (avocado.plugins.cli):
vt      Avocado VT/virt-test support to 'run' command
vt-list Avocado-VT/virt-test support for 'list' command
```
然后让我们列出可用的测试：

``` stylus
$ avocado list --vt-type qemu --verbose
```
这应该会列出大量的测试文件（超过1900个虚拟的相关测试文件）：

``` stylus
ACCESS_DENIED: 0
BROKEN_SYMLINK: 0
BUGGY: 0
INSTRUMENTED: 49
MISSING: 0
NOT_A_TEST: 27
SIMPLE: 3
VT: 1906
```
现在运行一个虚拟测试：

``` stylus
$ avocado run type_specific.io-github-autotest-qemu.migrate.default.tcp
JOB ID     : <id>
JOB LOG    : /home/<user>/avocado/job-results/job-2015-06-15T19.46-1c3da89/job.log
JOB HTML   : /home/<user>/avocado/job-results/job-2015-06-15T19.46-1c3da89/html/results.html
TESTS      : 1
(1/1) type_specific.io-github-autotest-qemu.migrate.default.tcp: PASS (95.76 s)
PASS       : 1
ERROR      : 0
FAIL       : 0
SKIP       : 0
WARN       : 0
INTERRUPT  : 0
TIME       : 95.76 s
```
如果你在提供的引导操作中遇到有问题的执行步骤，你有如下选择：

 - 发一封e-mail到 [the avocado mailing list][3]。
 - 在[the avocado-vt github area][4] 上开一个问题。
 - 我们也在[IRC (irc.oftc.net, #avocado)][5]上网络聊天。
 

# Writing Tests
这边文档主要是去帮助你写你自己的虚拟化测试文件。它有条理地简略的解释了源码的数据结构，然后写简单的测试文件，然后做做更复杂的测试，例如定义定制的客户机。

内容：

 - [Test Providers][6]
 - [Test Provider Layout][7]
 - [Types of Test Providers][8]
 - [Test Provider definition file][9]
 - [Development workflow after the Repository Split][10]
 - [Writing your own avocado VT test][11]
 - [Write our own ‘uptime’ test - Step by Step procedure][12]
 - [Defining New Guests][13]
 - [Linux Based Custom Guest][14]
 - [Windows Based Custom Guest][15]


## Test Providers
Test providers是Avocado-VT内置的一个可加载的模块的集合机制，能推送出一个目录，包含测试文件，配置文件和相关的依赖文件，以及其他的那些目录。 test providers背后的设计目标是：
 - 使其他组织机构可以在其他任意git软件库上维护测试库。
 - 稳定API并且强制分离核心Avocado-VT功能和测试文件。

test provider规范分为Provider Layout和Definition files。
### Test Provider Layout

``` stylus
.
|-- backend_1        -> Backend name. The actual name doesn't matter.
|   |-- cfg          -> Test config directory. Holds base files for the test runner.
|   |-- deps         -> Auxiliary files such as ELF files, Windows executables, images that tests need.
|   |-- provider_lib -> Shared libraries among tests.
|   `-- tests        -> Python test files.
|       `-- cfg      -> Config files for tests.
`-- backend_2
    |-- cfg
    |-- deps
    |-- provider_lib
    `-- tests
        `-- cfg
```
实际上，Avocado-VT能够智能地支持识别任意组织机构的python和‘test’目录里的配置文件。你不需要命名在backend names之后的顶层子目录，即使它确实可以使事情更简单。术语‘backend’常被Avocado-VT用来指支持虚拟化技术。在撰写这个文本时，backends被Avocado-VT认为是：

 - generic（运行在multiple backends中的测试）
 - qemu
 - openswitch
 - libvirt
 - v2v
 - libguestfs
 - lvsb

你不需要命名backend名字名称之后的目录的原因是你可以配置一个测试定义文件去指出任一目录名称。我们将进入。


### Types of Test Providers
每个test provider要么是一个本地的文件系统目录，要么是一个git软件库的子目录。当然，git软件库子目录可能是软件库的根目录，但是一点建议是人们能使用在其他项目中git软件库里的Avocado-VT providers。qemu希望维持它自己的provider，他们可以通过支持文件来做这，也就是说在qemu.git里在tests/avocado_vt子目录里。


### Test Provider definition file
主要的Avocado-VT套件需要一种方式知道有关test providers。他通过扫描在'test-providers.d'子目录里的definition files。 Definition files 是配置解析文件<http://docs.python.org/2/library/configparser.html> ，从一个test provider中编码信息。这有一有关test provider文件的数据结构的例子：

``` stylus
[provider]

# Test provider URI (default is a git repository, fallback to standard dir)
uri: git://git-provider.com/repo.git
#uri: /path-to-my-git-dir/repo.git
#uri: http://bla.com/repo.git
#uri: file://usr/share/tests

# Optional git branch (for git repo type)
branch: master

# Optionall git commit reference (tag or sha1)
ref: e44231e88300131621586d24c07baa8e627de989

# Pubkey: File containing public key for signed tags (git)
pubkey: example.pub

# What follows is a sequence of sections for any backends that this test
# provider implements tests for. You must specify the sub directories of
# each backend dir, reason why the subdir names can be arbitrary.

[qemu]
# Optional subdir (place inside repo where the actual tests are)
# This is useful for projects to keep virt tests inside their
# (larger) test repos. Defaults to ''.
subdir: src/tests/qemu/

[agnostic]
# For each test backend, you may have different sub directories
subdir: src/tests/generic/
```
一个默认的Avocado-VT  provider file的例子：

``` stylus
[provider]
uri: https://github.com/autotest/tp-qemu.git
[generic]
subdir: generic/
[qemu]
subdir: qemu/
[openvswitch]
subdir: openvswitch/
```

假设你想要在你的文件系统（/usr/share/tests/virt-test）中使用一个目录：

``` stylus
[provider]
uri: file://usr/share/tests/
[generic]
subdir: virt-test/generic/
[qemu]
subdir: virt-test/qemu/
[openvswitch]
subdir: virt-test/openvswitch/
```

## Development workflow after the Repository Split

 1. 把你想要贡献的测试提供者转移到github：


 https://help.github.com/articles/fork-a-repo
 
 2. 克隆分支存储库。 在这个例子中，我们假设你克隆了分叉的repo


``` stylus
/home/user/code/tp-libvirt
```

 3. 添加一个文件到目录：~/avocado/data/avocado-vt/test-providers.d，用你喜欢的名字命名文件。我们假定你选择的是：

``` stylus
user-libvirt.ini
```

 4. user-libvirt.ini的内容是：


``` stylus
[provider]
uri: file:///home/user/code/tp-libvirt
[libvirt]
subdir: libvirt/
[libguestfs]
subdir: libguestfs/
[lvsb]
subdir: lvsb/
[v2v]
subdir: v2v/
```


 5. 这应该足够了。现在当你使用 *--list-tests* ，你将能看见这样的入口：

``` stylus
...
1 user-libvirt.unattended_install.cdrom.extra_cdrom_ks.default_install.aio_native
2 user-libvirt.unattended_install.cdrom.extra_cdrom_ks.default_install.aio_threads
3 user-libvirt.unattended_install.cdrom.extra_cdrom_ks.perf.aio_native
...
```


 6. 修改测试文件，添加新的东西 到你的核心内容。当你对你的改变很满意时，你可以创建一个分支并且向我们推送请求。


## Writing your own avocado VT test
在这篇文章中，我们将谈论：

 1. 测试文件在哪边
 2. 写一个简单的测试文件
 3. 尝试你的新测试，发送它到mailing list

### Write our own ‘uptime’ test - Step by Step procedure
现在，让我们去写我们的uptime test，它唯一的目的就是 pick up a living guest，通过ssh连接它，并返回它的uptime。
1.首先我们需要定位我们的provider目录。它在Avocado data 目录（avocado config –datadir）中，通常在~/avocado/data/avocado-vt 。我们要去写一个一般的tp-qemu test，所以让我们移动到正确的git位置：

``` stylus
$ cd $AVOCADO_DATA/avocado-vt/test-providers.d/downloads/io-github-autotest-qemu
```


2.我们的uptime test不需要任何qemu特定功能。思考它，我们仅仅需要一个vm object并且establish an ssh session to it，所以我们能启动命令。所以我们能存储我们的分支新的测试在 *generic/tests* 目录下：

``` stylus
$ touch generic/tests/uptime.py
$ git add generic/tests/uptime.py
```


3.OK，那是个开始。这样的话，我们不得不实现至少一个功能 *run* 。让我们从它开始，仅仅放一个关键字pass，没有操作。我们的测试像这样：

``` stylus
def run(test, params, env):
    """
    Docstring describing uptime.
    """
    pass
```

 1. 现在，我们需要什么样的API从我们的测试环境中攫取一个VM？我们的env 对象有一个函数， *get_vm* ，它将获取存储在我们的环境变量中一个已给的vm 名字。他们中的一些有别名。main_vm包含了环境中现在的主要的vm名字，在很多时候，它是vm1 。env.get_vm返回一个vm对象，我们将存储在变量vm上。它将像这样：

``` stylus
def run(test, params, env):
    """
    Docstring describing uptime.
    """
    vm = env.get_vm(params["main_vm"])
```

 1. 一个vm对象有许多有趣的函数，我们打算更彻底的记录它们，但就目前而言，我们想要确保这个VM是alive和functional，至少在一个qemu进程的立场上。所以我们调用函数 *verify_alive()* ,它将确认qemu进程是否是functional并且如果monitors，如果any exist，是否是functional。如果由于任何问题这些条件中任何一个不支持，一个异常将会抛出并且测试将会失败。这个需求因为一些bug，vm进程可能死在水中，或者monitors没有响应：

``` stylus
def run(test, params, env):
    """
    Docstring describing uptime.
    """
    vm = env.get_vm(params["main_vm"])
    vm.verify_alive()
```

 1. 下一步，我们想要登录到vm。调用vm函数 *wait_for_login()* 返回一个远端会话对象，并且做为参数中一个，它允许你校正timeout，那是我们想要去看我们攫取一个ssh提示的等待时间。我们有顶级变量 *login_timeout* ，这是检索它并且传递它的值给 *wait_for_login()* 的一个很好的练习，所以如果因为一些原因我们运行在一个很慢的主机，某个变量的提高将影响所有的测试。注意因为这个函数有一个默认的timeout值，所以覆盖这个值或者不传递任何值给 *wait_for_login()* 是完全ok的。回到业务，从我们的参数字典中选择登陆超时时间：

``` stylus
def run(test, params, env):
    """
    Docstring describing uptime.
    """
    vm = env.get_vm(params["main_vm"])
    vm.verify_alive()
    timeout = float(params.get("login_timeout", 240))
```

 1. 现在我们调用 *wait_for_login()* 并且传递timeout值给它，在一个名为session的变量中存储结果会话对象：

``` stylus
def run(test, params, env):
    """
    Docstring describing uptime.
    """
    vm = env.get_vm(params["main_vm"])
    vm.verify_alive()
    timeout = float(params.get("login_timeout", 240))
    session = vm.wait_for_login(timeout=timeout)
```

 1. Avocado-VT将尽力去攫取这个会话，如果因为超时或者其他原因没能获取，它将抛出一个failure，测试失败。假设事情进展顺利，现在你有一个会话对象，你可以在你的客户机上输入命令和重新获得输出。所以在大多数时候，我们可以通过函数 *cmd()* 获取这些命令的输出。它将输入命令，攫取标准输入和输出，返回它们，所以你可以把它存在一个变量中，并且如果这个命令的退出码是 !=0，它将抛出一个 aexpect.ShellError? 。所以获取unix命令uptime的输出就像调用带有'uptime'参数的 *cmd()* 一样简单，并且存储结果在一个名为uptime的变量中：

``` stylus
def run(test, params, env):
    """
    Docstring describing uptime.
    """
    vm = env.get_vm(params["main_vm"])
    vm.verify_alive()
    timeout = float(params.get("login_timeout", 240))
    session = vm.wait_for_login(timeout=timeout)
    uptime = session.cmd('uptime')
```

 1. 如果你想要打印出这个值，让它能够在test logs总被看见，用logging库记录uptime的log值。既然那是我们想要做的所有，我们可以用函数 *close()* 关掉远程连接，去避免ssh/rss会话在你的测试机里。现在，注意这儿所有可能发生的失败都被函数调用隐式的处理了。如果一个测试从开始到结束都没有未处理的异常，autotest假设测试自动的通过，不需标记一个测试为显式的传递。如果你有失败的明确指向，对于更复杂的测试，你可以添加一些异常：
 

``` stylus
def run(test, params, env):
    """
    Docstring describing uptime.
    """
    vm = env.get_vm(params["main_vm"])
    vm.verify_alive()
    timeout = float(params.get("login_timeout", 240))
    session = vm.wait_for_login(timeout=timeout)
    uptime = session.cmd('uptime')
    logging.info("Guest uptime result is: %s", uptime)
    session.close()
```

 1. 现在，我故意在代码中引入一个bug，向各位展示如何使用一些工具去找到并且移除在你的代码中的琐碎的bugs。我强烈鼓励各位用inspektortool来检测代码。这个工作使用pylint在测试代码中捕捉bugs。你可以通过天剑COPR软件库 https://copr.fedoraproject.org/coprs/lmr/Autotest/ 来安装inspektor并且这么做：

``` stylus
$ yum install inspektor
```
在你安装了之后，你可以启动它：

``` stylus
$ inspekt lint generic/tests/uptime.py
************* Module generic.tests.uptime
E0602: 10,4: run: Undefined variable 'logging'
Pylint check fail: generic/tests/uptime.py
Syntax check FAIL
```

 1. Ouch。在代码的第10行有一个未定义的变量被调用。它是因为我忘记引入logging库了，它是一个用于处理信息，debug，警告信息的python库。让我处理他，代码变成：

``` stylus
import logging

def run(test, params, env):
    """
    Docstring describing uptime.
    """
    vm = env.get_vm(params["main_vm"])
    vm.verify_alive()
    timeout = float(params.get("login_timeout", 240))
    session = vm.wait_for_login(timeout=timeout)
    uptime = session.cmd("uptime")
    logging.info("Guest uptime result is: %s", uptime)
    session.close()
```

 1. 让我们重新启动 *inspektor*  去查看代码生成了什么：
 
 

``` stylus
$ inspekt lint generic/tests/uptime.py
Syntax check PASS
```

 2. So we’re good. Nice! 现在，好的排版对python很重要，inspekt indent将解决排版问题，去掉你代码中后面的空白部分。提交之前对你的测试的很好的整理：
 

``` stylus
$ inspekt indent generic/tests/uptime.py
```

 3. 现在，你可以测试你的代码。当列出所有的qemu tests时，你的新的测试应该出现在列表中（或者不应该？）：
 

``` stylus
$ avocado list uptime
```

 4. 还有事情要做。Avocado-VT不遍历目录，它使用Cartesian配置去定义测试和测试中所有可能的变量。把我们的测试添加到我们也需要的另一个文件的Cartesian配置中：
 

``` stylus
$ touch generic/tests/cfg/uptime.cfg
$ git add generic/tests/cfg/uptime.cfg
```

 5. 这个文件看起来应该像这样：

``` stylus
- uptime:
    virt_test_type = qemu libvirt
    type = uptime
```
virt_test_type指定了这个测试运行的backends，type 指定了测试文件。后缀.py将被添加并且它将在通常的位置中被搜索。


  6. 再一次，让我们尝试发现测试：
 

``` stylus
$ avocado list uptime
```

 7.  仍然还没有ok。我们需要通过启动vt-bootstrap来传播改变到实际的配置中：

``` stylus
$ avocado vt-bootstrap
```

 

 8. 现在你将最终看到测试：
 

``` stylus
$ avocado list uptime
```

 9. 现在，你可以运行你的测试区查看一切是否正常：
 
 

``` stylus
$ avocado run --vt-type qemu uptime
```

 10. OK,所以现在，我们有一些东西可以被git committed并且发送到mailing list （partial）：
 

``` stylus
diff --git a/generic/tests/uptime.py b/generic/tests/uptime.py
index e69de29..65d46fa 100644
--- a/tests/uptime.py
+++ b/tests/uptime.py
@@ -0,0 +1,13 @@
+import logging
+
+def run(test, params, env):
+    """
+    Docstring describing uptime.
+    """
+    vm = env.get_vm(params["main_vm"])
+    vm.verify_alive()
+    timeout = float(params.get("login_timeout", 240))
+    session = vm.wait_for_login(timeout=timeout)
+    uptime = session.cmd("uptime")
+    logging.info("Guest uptime result is: %s", uptime)
+    session.close()
```


 10.oh， 我忘记添加一个合适的docstring描述了，所以这样做：
 

``` stylus
import logging

def run(test, params, env):

    """
    Uptime test for virt guests:

    1) Boot up a VM.
    2) Establish a remote connection to it.
    3) Run the 'uptime' command and log its results.

    :param test: QEMU test object.
    :param params: Dictionary with the test parameters.
    :param env: Dictionary with test environment.
    """

    vm = env.get_vm(params["main_vm"])
    vm.verify_alive()
    timeout = float(params.get("login_timeout", 240))
    session = vm.wait_for_login(timeout=timeout)
    uptime = session.cmd("uptime")
    logging.info("Guest uptime result is: %s", uptime)
    session.close()
```


 1. git commit标记它，放置一个适当的描述，然后用git send-email 发送它。Profit!


## Defining New Guests 
假设你有一个精心准备的客户内核，并且jeOS just doesn’t cut it。这儿你如何添加新的客户：

### Linux Based Custom Guest
如果你的客户是一个基本的linux，你能添加一个配置文件片段描述你的测试（给linux的默认的配置中我们有一堆预置值）。
目录下降到：

``` stylus
shared/cfg/guest-os/Linux/LinuxCustom
```

你能添加，例如，foo.cfg  to that dir with the content:

``` stylus
FooLinux:
    image_name = images/foo-linux
```
这使得指定定制的客户使用成为可能：

``` stylus
$ avocado run migrate..tcp --vt-type qemu --vt-guest-os LinuxCustom.FooLinux
JOB ID     : 44a399b427c51530ba2fcc37087c100917e1dd8a
JOB LOG    : /home/lmr/avocado/job-results/job-2015-07-29T03.47-44a399b/job.log
JOB HTML   : /home/lmr/avocado/job-results/job-2015-07-29T03.47-44a399b/html/results.html
TESTS      : 3
(1/3) type_specific.io-github-autotest-qemu.migrate.default.tcp: PASS (31.34 s)
(2/3) type_specific.io-github-autotest-qemu.migrate.with_set_speed.tcp: PASS (26.99 s)
(3/3) type_specific.io-github-autotest-qemu.migrate.with_reboot.tcp: PASS (46.40 s)
RESULTS    : PASS 3 | ERROR 0 | FAIL 0 | SKIP 0 | WARN 0 | INTERRUPT 0
TIME       : 104.73 s
```
Provided that you have a file called images/foo-linux.qcow2，如果使用qcow2格式内核。
其它要被设置的有用的参数（不是一个详尽的列表）：

``` stylus
# shell_prompt is a regexp used to match the prompt on aexpect.
# if your custom os is based of some distro listed in the guest-os
# dir, you can look on the files and just copy shell_prompt
shell_prompt = [*]$
# If you plan to use a raw device, set image_device = yes
image_raw_device = yes
# Password of your image
password = 123456
# Shell client used (may be telnet or ssh)
shell_client = ssh
# Port were the shell client is running
shell_port = 22
# File transfer client
file_transfer_client = scp
# File transfer port
file_transfer_port = 22
```

### Windows Based Custom Guest
如果你的客户是基本的windows，你可以添加一个配置文件片段描述你的测试（对于默认windows配置我们有一堆预设置值）。
目录下降到：

``` stylus
shared/cfg/guest-os/Windows/WindowsCustom
```
你可以添加，例如foo.cfg to that dir with the content:

``` stylus
FooWindows:
    image_name = images/foo-windows
```
这使得指定定制的客户使用成为可能

``` stylus
$ avocado run migrate..tcp --vt-type qemu --vt-guest-os WindowsCustom.FooWindows
```
Provided that you have a file called images/foo-windows.qcow2.

其他有用的参数被设置（不是一个详尽的列表）：

``` stylus
# If you plan to use a raw device, set image_device = yes
image_raw_device = yes
# Attention: Changing the password in this file is not supported,
# since files in winutils.iso use it.
username = Administrator
password = 1q2w3eP
```
# Install Optional Packages
一些数据包在Avocado-VT的硬依赖关系中没有被设置，因为他们仅在一些特定的使用案例中需要。

如果你在运行特定的测试中遇到问题，请查证如果安装提到的数据包是否能解决问题。

## Fedora and EL
安装下列数据包：
1.在你的主机中安装一个工具链，你可以Fedora和RHEL中这样做：

``` stylus
$ yum groupinstall "Development Tools"
```

 1. 安装tcpdump，有必要自动的确定guest IPs
 

``` stylus
$ yum install tcpdump
```

 1. 安装nc，有必要从串行设备和其他qemu设备中获取输出

``` stylus
$ yum install nmap-ncat
```

 1. 安装p7zip文件压缩程式，你可以解压JeOS[2]内核
 

``` stylus
$ yum install p7zip
```

 1. 安装autotest-framework数据包，提供需要的autotest库。
 

``` stylus
$ yum install --enablerepo=updates-testing autotest-framework
```

 1. 安装fakeroot数据包，如果你想要从CD安装Ubuntu和Debian servers，没有需要的根：
 
 

``` stylus
$ yum install fakeroot
```

如果你没有安装autotest-framework数据包（例如，你的发行版仍然没有autotest数据包，或者你不想安装rpm），你将不得不克隆一份autotest树并且导出路径到AUTOTEST_PATH变量，对root和一般用户都可用。可以像下面那样把变量导入到他们的 ~/.bashrc文件：

``` stylus
$ export AUTOTEST_PATH="/path/to/autotest"
```
这个环境变量 AUTOTEST_PATH将引导运行的脚本去为所有的测试工作设置需要的库。

其他的数据包：

``` stylus
$ yum install git
```
你可以检测源码。如果你想要测试提供的qemu-kvm二进制发行版，你可以安装：

``` stylus
$ yum install qemu-kvm qemu-kvm-tools
```

为了运行libvirt测试，它需要安装virt-install utility，为了建立和克隆虚拟机的基本目的。

``` stylus
$ yum install virt-install
```
To run all tests that involve filedescriptor passing，你需要python-devel。原因是，这个测试套件和python2.4兼容，然而一个通过filedescriptors 的标准库仅仅引入了python3.2 。因此，我们不得不引入了一个编译需求的C python扩展

``` stylus
$ yum install python-devel
```

安装这个也是有用的：

``` stylus
$ yum install python-imaging
```

不是至关重要的，但非常便捷的是做imaging转换，从ppm到jpeg和png（允许最小的images）。


### Tests that are not part of the default JeOS set
如果你想要运行guest安装测试，你需要能创建floppies和isos去支持kickstart文件：

``` stylus
$ yum install mkisofs
```

为了更新的发行版，例如Fedora，你将需要：

``` stylus
$ yum install genisoimage
```
两个数据包都提供了相同的功能，需要去创造将在客户机安装进程期间使用的iso images。你也可以执行。

### Network tests
最后但并不是最不重要的，现在我们依靠libvirt给我们提供一个稳定的，工作桥。默认的，kvm测试使用用户网络，所以这不是完全必要的。 However, non root and user space networking make a good deal of the hardcode networking tests to not work.如果你最终像使用bridges：

``` stylus
$ yum install libvirt bridge-utils
```

确认libvirtd是启动的：

``` stylus
$ service libvirtd start
```

确保libvirt bridge出现在brctl shwo上的输出：

``` stylus
$ brctl show
bridge name bridge id       STP enabled interfaces
virbr0      8000.525400678eec   yes     virbr0-nic
```

## Debian
记住当前的autotest数据包是一个发展中的工作。就运行virt-tests的目的而言它是好的，但它需要许多的提高直到它能成为一个更 'official' 的数据包.

 autotest debian 数据包可以在 https://launchpad.net/~lmr/+archive/autotest 中找到，并且你能在你的系统上添加软件库，放置下面的东西到 /etc/apt/sources.list文件中：
 

``` stylus
$ deb http://ppa.launchpad.net/lmr/autotest/ubuntu raring main
$ deb-src http://ppa.launchpad.net/lmr/autotest/ubuntu raring main
```

然后更新你的软件列表：

``` stylus
$ apt-get update
```
这已经用Ubuntu 12.04,12.10和13.04测试过了。

安装下列数据包：

 1. 安装autotest-framework数据包，提供需要的autotest库。
 

``` stylus
$ apt-get install autotest
```

 1. 安装p7zip文件压缩程式，你可以解压jsOS[2] image。
 

``` stylus
$ apt-get install p7zip-full
```

 1. 安装tcpdump，有必要确定自动化地guest IPs

``` stylus
$ apt-get install tcpdump
```

 1. 安装nc，有必要获取串行设备和其他qemu设备的输出。
 
 

``` stylus
$ apt-get install netcat-openbsd
```

 1. 在你的主机中安装工具链，你可以在Debian和Ubuntu中使用下面命令：

``` stylus
$ apt-get install build-essential
```

 1. 安装fakeroot，如果你想要从CD上安装debian和ubuntu，不需要root：

``` stylus
$ apt-get install fakeroot
```
所以你安装了核心autotest库区运行测试。

如果你不想安装autotest-framework数据包（例如，你的发行版没有autotest数据包，或者你不想安装rpm），你将不得不克隆一份autotest tree树，并且导入路径到AUTOTEST_PATH变量，在root和你的一般用户上都有。可以使用下面明道导入到~/.bashrc文件中：

``` stylus
$ export AUTOTEST_PATH="/path/to/autotest"
```
环境变量AUTOTEST_PATH将引导运行脚本去设置所有测试工作需要的库。

其他的数据包：

``` stylus
$ apt-get install git
```
所以你可以检查源代码。如果你想要测试提供qemu-kvm二进制的发行版，你可以安装：

``` stylus
$ apt-get install qemu-kvm qemu-utils
```
为了运行libvirt测试，需要安装virt-install，因为建立和克隆虚拟机的基本目的。

``` stylus
$ apt-get install virtinst
```
去运行所有有关 filedescriptor passing的测试，你需要 python-all-dev。原因是，测试讨教和python2.4加绒，然而一个人通过filedescriptors 的标准库仅仅引入了python3.2.因此，我们不得不引入一个急需的编译了的C python的扩展。

``` stylus
$ apt-get install python-all-dev.
```

安装这个是有用的：

``` stylus
$ apt-get install python-imaging
```
不是很重要的，但是很便捷的去做imaging转换，从ppm到jpeg和png（考虑到更小的images）。

### Tests that are not part of the default JeOS set

当你想要运行客户拒安装测试，你需要能创建floppies和isos去支持kickstart文件：

``` stylus
$ apt-get install genisoimage
```

### Network tests
最后但并不是最不重要的，现在我们依赖libvirt去给我们提供一个稳定的，工作的桥，默认地，kvm测试使用用户网络，所以这不是完全必要的。然而，没有root和用户空间网络能很好的出路硬代码网络测试不工作。如果你最终像使用桥：

``` stylus
$ apt-get install libvirt-bin python-libvirt bridge-utils
```

确保libvirtd是工作的：

``` stylus
$ service libvirtd start
```
确保libvirt桥在brctl show的输出中出现如下信息：

``` stylus
$ brctl show
bridge name bridge id       STP enabled interfaces
virbr0      8000.525400678eec   yes     virbr0-nic
```

# Listing guests
如果你想看到所有定义的客户机，你可以使用：

``` stylus
$ avocado list --vt-type [test type] --vt-list-guests
```

这将生成一个能被测试使用的可能的客户机列表，提供他们的相关内核。列表将展示出客户机当前没有的可获得的内核。如果你执行了一般的引导程序，只有 JeOS.17.64可获得。

现在假设你拥有给另一个客户机的内核。假设你安装了 Fedora 17, 64 bits，选项 –list-guests将展示下载的：

``` stylus
$ avocado list --vt-type qemu --vt-list-guests
...
Linux.CentOS.6.6.i386.i440fx (missing centos66-32.qcow2)
Linux.CentOS.6.6.x86_64.i440fx (missing centos66-64.qcow2)
```

你可以列出所有对Fedora.17.64可用的测试（you must use the exact string printed by the test, minus obviously the index number, that’s there only for informational purposes）：

``` stylus
$ avocado list --vt-type qemu --vt-guest-os Linux.CentOS.6.6.i386.i440fx --verbose
...
VT           io-github-autotest-qemu.trans_hugepage.base
VT           io-github-autotest-qemu.trans_hugepage.defrag
VT           io-github-autotest-qemu.trans_hugepage.swapping
VT           io-github-autotest-qemu.trans_hugepage.relocated
VT           io-github-autotest-qemu.trans_hugepage.migration
VT           io-github-autotest-qemu.trans_hugepage.memory_stress
VT           io-github-autotest-qemu.ntpd
VT           io-github-autotest-qemu.clock_getres
VT           io-github-autotest-qemu.autotest_regression
VT           io-github-autotest-qemu.shutdown

ACCESS_DENIED: 0
BROKEN_SYMLINK: 0
BUGGY: 0
FILTERED: 0
INSTRUMENTED: 52
MISSING: 0
NOT_A_TEST: 27
SIMPLE: 3
VT: 2375
```

然后你可以特定的执行某个。它是相同的方法，仅仅拷贝出你想要的单独的测试，执行它：

``` stylus
$ avocado run balloon_check --vt-type qemu --vt-guest-os Fedora.21
```

并且他将在特别的测试中执行。

# How tests are run
当启动测试时， Avocado-VT将：

 1. 获得一个带有测试参数的字典
 2. 基于这些参数，准备环境-create or destroy vm instances, create/check disk images, among others。
 3. 执行测试自身，那将使用几个定义的参数去进行操作，那通常包含：-如果一个测试没有引发异常，它PASSed  -如果一个测试引发了一个TestFail 异常，它FAILed。  -如果一个测试引发一个TestNAError,ta SKIPPED 。 -否则，它ERRORed 。
 4. 基于在测试期间发生了什么，执行清除动作，例如杀死vms,移除没有使用的disk images 。

参数列表是通过解析一组配置文件获得。命令行选项通常会更进一步的修改解析文件，所以我们在配置集中引入新的数据。

# Cartesian Configuration
Cartesian Configuration是一种专业的提供不同类别的组合的键值对列表的方式。简化和凝结高度复杂的多维数组格式的测试参数平面列表。组合的结果可以筛选和调整前测试，用过滤器，依赖性，键值代替。

解析器依赖于缩进，并且对tab键和空格字符的误放非常敏感。强烈建议在能够把tab键转换到四个空格字符的编辑器中编辑或浏览Cartesian配置文件。不注意列间距的话会大大地影响输出。

##  Keys and values

Keys和values是格式提供的最基本的有用的工具。格式 \<key> = \<value>设置了\<key> 到 \<value> 的声明。values是字符串，终止于一个换行符，用可选的（but honored）引号完全包括。大多keys的一份参考描述被包含在配置参数参考空间里。The key will become part of all lower-level (i.e. further indented) variant stanzas (see section [variants][16])。然而，key的优先级是自上而下或者 ‘last defined’ 的顺序评估的。换句话说，最后解析的key优先级大于早期定义的。



## Variants
一个‘variants’ stanza 是用 ‘variants:’声明开始的。stanza 的内容必须比‘variants:’ 的声明向左缩进。每个variant stanza或者块定义一个一维输出数组。当一个Cartesian 配置文件包含两个 variants stanzas时，输出将尽可能的是两个变量内容的组合。变量可以被其他变量嵌套，用外面的数组单元可以有效的任意的嵌套复杂的数组。例如：

``` stylus
variants:
    - one:
        key1 = Hello
    - two:
        key2 = World
    - three:
variants:
    - four:
        key3 = foo
    - five:
        key3 = bar
    - six:
        key1 = foo
        key2 = bar
```


当组合时，每个基于预先的每个变量的结果的解析格式名称对应到一个列表。换句话说，第一个变量名字的解析将表现为 the left most name component.这些名称可能变得相当长，并且因为他们包含keys去区分结果，一个 ‘short-name’  key 也可以使用。例如，运行 *cartesian_config.py* 对上面内容产生下列组合和名称：

``` stylus
dict    1:  four.one
dict    2:  four.two
dict    3:  four.three
dict    4:  five.one
dict    5:  five.two
dict    6:  five.three
dict    7:  six.one
dict    8:  six.two
dict    9:  six.three
```
当结果被记录（看章节Job Names and Tags），变量的短名称表现为使用过的 \<TESTNAME>  value， 为了变量的便捷性，变量的名字用一个' @ '开始而不是预先考虑的‘short-name’名称，只有'name'。这允许创建‘shortcuts’来指定multiple sets或者改变成没有改变结果目录名称的键值对。例如，这经常对提供一个相关的基于其他测试组合的预配置测试带来很大的便捷。

## Named variants
variants 命名允许指定一个可解析的名字给一个变量集合。这使得整个变量集合可以在过滤器中使用。所有输出组合将继承自variant key名字，跟随指定的 variant name，例如：

``` stylus
variants var1_name:
     - one:
         key1 = Hello
     - two:
         key2 = World
     - three:
variants var2_name:
     - one:
         key3 = Hello2
     - two:
         key4 = World2
     - three:

only (var2_name=one).(var1_name=two)
```
当用 *cartesian_config.py -c*解析将导致下面的结果：

``` stylus
dict    1:  (var2_name=one).(var1_name=two)
      dep = []
      key2 = World         # variable key2 from variants var1_name and variant two.
      key3 = Hello2        # variable key3 from variants var2_name and variant one.
      name = (var2_name=one).(var1_name=two)
      shortname = (var2_name=one).(var1_name=two)
      var1_name = two      # variant name in same namespace as variables.
      var2_name = one      # variant name in same namespace as variables.
```

variants 的命名可以被当做普通变量使用：

``` stylus
variants guest_os:
     - fedora:
     - ubuntu:
variants disk_interface:
     - virtio:
     - hda:
```

这将产生下面的结果：

``` stylus
dict    1:  (disk_interface=virtio).(guest_os=fedora)
    dep = []
    disk_interface = virtio
    guest_os = fedora
    name = (disk_interface=virtio).(guest_os=fedora)
    shortname = (disk_interface=virtio).(guest_os=fedora)
dict    2:  (disk_interface=virtio).(guest_os=ubuntu)
    dep = []
    disk_interface = virtio
    guest_os = ubuntu
    name = (disk_interface=virtio).(guest_os=ubuntu)
    shortname = (disk_interface=virtio).(guest_os=ubuntu)
dict    3:  (disk_interface=hda).(guest_os=fedora)
    dep = []
    disk_interface = hda
    guest_os = fedora
    name = (disk_interface=hda).(guest_os=fedora)
    shortname = (disk_interface=hda).(guest_os=fedora)
dict    4:  (disk_interface=hda).(guest_os=ubuntu)
    dep = []
    disk_interface = hda
    guest_os = ubuntu
    name = (disk_interface=hda).(guest_os=ubuntu)
    shortname = (disk_interface=hda).(guest_os=ubuntu)
```

## Dependencies
经常有必要指示variants之间的关系。在这种方式中，the order of the resulting variant sets 将受影响。这通过在子variant名称之后将列出所有父辈的名称（按顺序）来完成。然而，依赖性的影响是 ‘weak’，在那后面的定义中，lower-level (higher indentation) definitions，nd/or filters (see section filters)可能修改或移除关联。例如，测试无人值守的安装，每个虚拟机在前面被引导，并且在后面被关机：

``` stylus
variants:
    - one:
        key1 = Hello
    - two: one
        key2 = World
    - three: one two
```
导致正确的variant sets顺序是： one, two, 然后 three。

## Filters
过滤器声明允许修改基于variant 集合名字（看章节 variants）的keys集合的结果。过滤器有3种使用方式：限制集合用仅包含组合的名称的集合去匹配一种模式。限制集合去排除不匹配的组合名称来匹配一种模式。修改集合或者带有匹配的组合名字的键值对内容。

名称通过解析一个variant名称组件来匹配，variant组件带有字符（集），','代表OR，'..'代表AND，'.'代表IMMEDIATELY-FOLLOWED-BY。当独自使用时，他们允许修改先前定义的键值列表。例如：

``` stylus
Linux..OpenSuse:
initrd = initrd
```

用 ‘OpenSuse’修改包含‘Linux’其后的任何地方的所有variants ，例如‘initrd’ key被值‘initrd’创建或者被覆盖。

当一个过滤器在前面用关键字'only'或者'no'，它限制了variant组合的选择。这被用于一个特别的集合，一个或更多的variant组合应该是有选择地或者唯一的。当给予一个非常大的矩阵variants，关键字'only'很便捷的限制了那些只有匹配了过滤器的结果集。反之关键字'no'能被用于移除特别的冲突的在其他variant组合名称下面的键值集合。例如：

``` stylus
only Linux..Fedora..64
```
将减少一个任意的大矩阵，只有这些variants，他们名字包含 Linux, Fedora, 和 64。

However, note that any of these filters may be used within named variants as well.在这个应用中，they are only evaluated when that variant name is selected for inclusion (implicitly or explicitly) by a higher-order.例如：

``` stylus
variants:
    - one:
        key1 = Hello
variants:
    - two:
        key2 = Complicated
    - three: one two
        key3 = World
variants:
    - default:
        only three
        key2 =

only default
```


导致以下结果：

``` stylus
name = default.three.one
key1 = Hello
key2 =
key3 = World
```

## Value Substitutions

值代替允许有选择地覆盖优先级和定义部分或者一个未来key值的所有。使用一个先前定义的key，它的值可以被代替或者做为另一个key的值。语法恰好和bash shell一样， 无论在哪里key的名字出现在字符'$'之后，一个key的值是被代替的。当在其他non-key-name文本内嵌入一个key，名字也应该被字符'{'和'}'包括。

替代值是内容敏感的，因此如果一个key被用同样的或者更高阶的块重定义，那个值将被用于未来的替换。如果一个key被用于替换，但还没有被定义，就不会起作用。换句话说，$key或者${key}字符串将出现字面上或内部的值。参照的嵌套是不支持的。（i.e. key substitutions within other substitutions.


例如，如果one = 1，two = 2，three = 3；那么order = ${one}${two}${three}导致order = 123。 This is particularly handy for rooting an arbitrary complex directory tree within a predefined top-level directory.

An example of context-sensitivity,

``` stylus
key1 = default value
key2 = default value

sub = "key1: ${key1}; key2: ${key2};"

variants:
    - one:
        key1 = Hello
        sub = "key1: ${key1}; key2: ${key2};"
    - two: one
        key2 = World
        sub = "key1: ${key1}; key2: ${key2};"
    - three: one two
        sub = "key1: ${key1}; key2: ${key2};"
```

导致如下结果：

``` stylus
dict    1:  one
    dep = []
    key1 = Hello
    key2 = default value
    name = one
    shortname = one
    sub = key1: Hello; key2: default value;
dict    2:  two
    dep = ['one']
    key1 = default value
    key2 = World
    name = two
    shortname = two
    sub = key1: default value; key2: World;
dict    3:  three
    dep = ['one', 'two']
    key1 = default value
    key2 = default value
    name = three
    shortname = three
    sub = key1: default value; key2: default value;
```


## Key sub-arrays
Parameters for objects like VM’s utilize array’s of keys specific to a particular object instance. In this way, values specific to an object instance can be addressed. For example, a parameter ‘vms’ lists the VM objects names to instantiate in the current frame’s test. Values specific to one of the named instances should be prefixed to the name:

``` stylus
vms = vm1 second_vm another_vm
mem = 128
mem_vm1 = 512
mem_second_vm = 1024
```
结果将是，3个虚拟机对象被创建。第三个虚拟机（another_vm）收到默认的'mem'值128.第二个虚拟机收到基于他们的名字指定的值。

这些卸载配置文件中的声明的顺序是不重要的；声明标记的一个单独的对象总是覆盖声明标记所有对象。注意：This is contrary to the way the Cartesian configuration file as a whole is parsed (top-down).


## Include statements
include声明在一个Cartesian 配置文件被利用去更好的组织相关内容。在解析时，当解析器遇到include语句，任何引用的文件的内容将被评估。The order in which files are included is relevant, and will carry through any key/value substitutions (see section [key_sub_arrays][17]) as if parsing a complete, flat file.

## Combinatorial outcome

解析器在python模块和命令行工具都是可用的，for examining the parsing results in a text-based listing.为了在命令行利用它，根据配置文件的解析路径运行模块。例如：

``` stylus
common_lib/cartesian_config.py tests/libvirt/tests.cfg
```
输出将仅仅是组合的结果集项目的名称（see short-names, section Variants）。然而， ‘--contents’参数可以指定在更深层次检查结果。 Internally, 键值数据的存储和访问和python目录实例类似。With the collection of dictionaries all being part of a python list-like object. Irrespective of the internals, running this module from the command-line is an excellent tool for both reviewing and learning about the Cartesian Configuration format.

一般来说，每个定义的variants独特的组合为一个单独的测试提供了参数。测试所得的，通过每个结果，通过测试代码传递keys和values的集合到日常工作。当检查Cartesian 配置文件时，有助于考虑做为 “defaults”定义的最早的key，然后查看文件尾部为了其他顶层覆盖这些值。如果有疑问在哪定义或设置一个key，把它放置在顶层缩进层，在文件尾部，将保证他被使用。


## Formal definition

 - 一个字典的列表被当做一个框架。
 - 解析器产生一个字典 (dicts)列表。每个字典包含一组键值对。
 - 每个字典包含至少三个keys：name，shortname和depend。name和shortname的值是字符串，depend值是一个字符串列表。
 - 最初的框架包含了一个单独的dict，name和shortname都是空字符串，并且depend是一个空列表。
 - 解析dict内容;
   - dict解析器在一个框架上操作，做为当前框架参考。
   - 格式 \<key> =  \<value> 的声明设置当前框架中所有字典中  \<key>的值到 \<value> 。如果一个字典缺少 \<key>，它会被创建。
   - 格式 \<key> += \<value> 的声明在当前框架的所有字典中追加 \<value>的值到 \<key> 。如果一个字典中缺少 \<key>，它将被创建。
   - 格式 \<key> <= \<value>的声明在当前框架的所有字典中  pre-pends \<value> 到 \<key>的值。如果一个字典缺少 \<key>，它将被创建。
   - 格式 \<key> ?= \<value> 的声明设置 \<key>的值到\<value>，在当前框架的所有字典中，但仅当\<key>在字典中存在。运算符?+=和 ?<= 也支持。
   - 格式 no \<regex>的声明移除来自当前框架中的所有名称字段匹配 \<regex>的列表。
   - 格式 only \<regex>的声明移除来自当前框架中的所有名称字段不匹配 \<regex>的列表。
 - Content exceptions
   - 一行异常有格式 \<regex>: \<key> \<operator> \<value>,其中 \<operator>是上面列出的任何一种操作（例如 =, +=, ?<=）。正则表达式 \<regex>的声明在当前框架中适用于字典中谁的名字部分匹配 \<regex>（例如，包含一个子串匹配 \<regex>）。
   - 一个多行的异常块被一行 \<regex>:格式打开。这行后面的文本应该被缩进。多行异常块的声明可能是赋值语句（例如 \<key> = \<value>）或者没有或者只有声明。嵌套的多行异常被允许。
 - Parsing Variants
   - 一个variants块通过variants: 声明打开。声明的缩进级别places the following set within the outer-most context-level when nested within other variant: blocks. The contents of the variants: block must be further indented.
   - 一个variant-name可选择的遵循variants关键字，在字符:之前。That name will be inherited by and decorate all block content as the key for each variant contained in it’s the block.
   - variants 的名字用 - \<variant_name>:指定。Each name is pre-pended to the name field of each dict of the variant’s frame, along with a separator dot (‘.’).
   - 每个variant 的内容可以使用格式 \<key> \<op> \<value>。他们也可以包含更近一步的variants:声明。
   - 如果variant的名字is not preceeded by a @ (i.e. - @<variant_name>:)，it is pre-pended to the shortname field of each dict of the variant’s frame. In other words, if a variant’s name is preceeded by a @, it is omitted from the shortname field.
   - 在一个variants块中的每个variant继承一份框架 in which the variants: statement appears. The ‘current frame’, which may be modified by the dict parser, becomes this copy.
   - 定义在块中的variants框架被连接成一个单一的框架。框架的内容替换外面包含的框架的内容（如果有一个）。
 - Filters
   - 过滤器可以有3种使用方式：
     - only \<filter>
     - no \<filter>
     - \<filter>: starts a conditional block (see section :ref:`filters_`)
   - Syntax:

``` stylus
.. means AND
. means IMMEDIATELY-FOLLOWED-BY
```


 - Example:

``` stylus
qcow2..Fedora.14, RHEL.6..raw..boot, smp2..qcow2..migrate..ide
```


``` stylus
means match all dicts whose names have:
(qcow2 AND (Fedora IMMEDIATELY-FOLLOWED-BY 14)) OR
((RHEL IMMEDIATELY-FOLLOWED-BY 6) AND raw AND boot) OR
(smp2 AND qcow2 AND migrate AND ide)
```


 - Note:


``` stylus
'qcow2..Fedora.14' is equivalent to 'Fedora.14..qcow2'.
```



``` stylus
'qcow2..Fedora.14' is not equivalent to 'qcow2..14.Fedora'.
'ide, scsi' is equivalent to 'scsi, ide'.
```

## Examples

 - A single dictionary:
 

``` stylus
key1 = value1
key2 = value2
key3 = value3

Results in the following::

Dictionary #0:
    depend = []
    key1 = value1
    key2 = value2
    key3 = value3
    name =
    shortname =
```

 - Adding a variants block:

``` stylus
key1 = value1
key2 = value2
key3 = value3

variants:
    - one:
    - two:
    - three:
```


 导致下面结果：
 

``` stylus
Dictionary #0:
    depend = []
    key1 = value1
    key2 = value2
    key3 = value3
    name = one
    shortname = one
Dictionary #1:
    depend = []
    key1 = value1
    key2 = value2
    key3 = value3
    name = two
    shortname = two
Dictionary #2:
    depend = []
    key1 = value1
    key2 = value2
    key3 = value3
    name = three
    shortname = three
```

 - 修改一个在variant内的字典：

``` stylus
key1 = value1
key2 = value2
key3 = value3

variants:
    - one:
        key1 = Hello World
        key2 <= some_prefix_
    - two:
        key2 <= another_prefix_
    - three:
```

导致如下结果：

``` stylus
Dictionary #0:
    depend = []
    key1 = Hello World
    key2 = some_prefix_value2
    key3 = value3
    name = one
    shortname = one
Dictionary #1:
    depend = []
    key1 = value1
    key2 = another_prefix_value2
    key3 = value3
    name = two
    shortname = two
Dictionary #2:
    depend = []
    key1 = value1
    key2 = value2
    key3 = value3
    name = three
    shortname = three
```

 - 添加依赖性：

``` stylus
key1 = value1
key2 = value2
key3 = value3

variants:
    - one:
        key1 = Hello World
        key2 <= some_prefix_
    - two: one
        key2 <= another_prefix_
    - three: one two
```


导致下面结果：

``` stylus
Dictionary #0:
    depend = []
    key1 = Hello World
    key2 = some_prefix_value2
    key3 = value3
    name = one
    shortname = one
Dictionary #1:
    depend = ['one']
    key1 = value1
    key2 = another_prefix_value2
    key3 = value3
    name = two
    shortname = two
Dictionary #2:
    depend = ['one', 'two']
    key1 = value1
    key2 = value2
    key3 = value3
    name = three
    shortname = three
```

 - 多重variant块：

``` stylus
key1 = value1
key2 = value2
key3 = value3

variants:
    - one:
        key1 = Hello World
        key2 <= some_prefix_
    - two: one
        key2 <= another_prefix_
    - three: one two

variants:
    - A:
    - B:
```

导致如下：

``` stylus
Dictionary #0:
    depend = []
    key1 = Hello World
    key2 = some_prefix_value2
    key3 = value3
    name = A.one
    shortname = A.one
Dictionary #1:
    depend = ['A.one']
    key1 = value1
    key2 = another_prefix_value2
    key3 = value3
    name = A.two
    shortname = A.two
Dictionary #2:
    depend = ['A.one', 'A.two']
    key1 = value1
    key2 = value2
    key3 = value3
    name = A.three
    shortname = A.three
Dictionary #3:
    depend = []
    key1 = Hello World
    key2 = some_prefix_value2
    key3 = value3
    name = B.one
    shortname = B.one
Dictionary #4:
    depend = ['B.one']
    key1 = value1
    key2 = another_prefix_value2
    key3 = value3
    name = B.two
    shortname = B.two
Dictionary #5:
    depend = ['B.one', 'B.two']
    key1 = value1
    key2 = value2
    key3 = value3
    name = B.three
    shortname = B.three
```

 - 过滤器，no 和 only：

``` stylus
key1 = value1
key2 = value2
key3 = value3

variants:
    - one:
        key1 = Hello World
        key2 <= some_prefix_
    - two: one
        key2 <= another_prefix_
    - three: one two

variants:
    - A:
        no one
    - B:
        only one,three
```
导致如下：

``` stylus
Dictionary #0:
    depend = ['A.one']
    key1 = value1
    key2 = another_prefix_value2
    key3 = value3
    name = A.two
    shortname = A.two
Dictionary #1:
    depend = ['A.one', 'A.two']
    key1 = value1
    key2 = value2
    key3 = value3
    name = A.three
    shortname = A.three
Dictionary #2:
    depend = []
    key1 = Hello World
    key2 = some_prefix_value2
    key3 = value3
    name = B.one
    shortname = B.one
Dictionary #3:
    depend = ['B.one', 'B.two']
    key1 = value1
    key2 = value2
    key3 = value3
    name = B.three
    shortname = B.three
```

 - Short-names:


``` stylus
key1 = value1
key2 = value2
key3 = value3

variants:
    - one:
        key1 = Hello World
        key2 <= some_prefix_
    - two: one
        key2 <= another_prefix_
    - three: one two

variants:
    - @A:
        no one
    - B:
        only one,three
```

导致如下：

 

``` stylus
Dictionary #0:
    depend = ['A.one']
    key1 = value1
    key2 = another_prefix_value2
    key3 = value3
    name = A.two
    shortname = two
Dictionary #1:
    depend = ['A.one', 'A.two']
    key1 = value1
    key2 = value2
    key3 = value3
    name = A.three
    shortname = three
Dictionary #2:
    depend = []
    key1 = Hello World
    key2 = some_prefix_value2
    key3 = value3
    name = B.one
    shortname = B.one
Dictionary #3:
    depend = ['B.one', 'B.two']
    key1 = value1
    key2 = value2
    key3 = value3
    name = B.three
    shortname = B.three
```

 - Exceptions:

``` stylus
key1 = value1
key2 = value2
key3 = value3

variants:
    - one:
        key1 = Hello World
        key2 <= some_prefix_
    - two: one
        key2 <= another_prefix_
    - three: one two

variants:
    - @A:
        no one
    - B:
        only one,three

three: key4 = some_value

A:
    no two
    key5 = yet_another_value
```

导致如下：

``` stylus
Dictionary #0:
    depend = ['A.one', 'A.two']
    key1 = value1
    key2 = value2
    key3 = value3
    key4 = some_value
    key5 = yet_another_value
    name = A.three
    shortname = three
Dictionary #1:
    depend = []
    key1 = Hello World
    key2 = some_prefix_value2
    key3 = value3
    name = B.one
    shortname = B.one
Dictionary #2:
    depend = ['B.one', 'B.two']
    key1 = value1
    key2 = value2
    key3 = value3
    key4 = some_value
    name = B.three
    shortname = B.three
```

## Default Configuration Files

测试配置文件通过指定每个测试参数用于控制框架。解析器产生一个键值组的列表，每组数据用于说明一个单独的测试。Variants被组织成单独的基于范围和适用性的文件。例如，客户操作系统的定义来源于一个共享位置，因为所有的虚拟化测试可以利用他们。

对于每个set/test，keys被测试调度系统，预处理程序，测试模块本身解释，然后被后处理程序解释。一些参数需要特定的部分，并且其它参数时可选择的。当需要时，parameters are often commented with possible values and/or their effect.在代码中有选择的地方，内存中的keys可以被修改，然而这个操作不被鼓励，除非有个好的原因。

当avocado vt-bootstrap --vt-type \[type] 命令被执行（看章节 [Bootstrapping Avocado-VT][18]），样例配置文件的副本被拷贝用于虚拟化技术特定目录backends/\[type]/cfg的子目录之下。例如，backends/qemu/cfg/base.cfg 。
 
 
 

| Relative Directory or File | Description                                                                                                                                                                                                                                                                                                   |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| cfg/tests.cfg              | The first file read that includes all other files, then the master set of filters to select the actual test set to be run. Normally this file never needs to be modified unless precise control over the test-set is needed when utilizing the autotest-client (only).                                        |
| cfg/tests-shared.cfg       | Included by tests.cfg to indirectly reference the remaining set of files to include as well as set some global parameters. It is used to allow customization and/or insertion within the set of includes. Normally this file never needs to be modified.                                                      |
| cfg/base.cfg               | Top-level file containing important parameters relating to all tests. All keys/values defined here will be inherited by every variant unless overridden. This is the first file to check for settings to change based on your environment                                                                     |
| cfg/build.cfg              | Configuration specific to pre-test code compilation where required/requested. Ignored when a client is not setup for build testing.                                                                                                                                                                           |
| cfg/subtests.cfg           | Automatically generated based on the test modules and test configuration files found when the avocado vt-bootstrap is used. Modifications are discouraged since they will be lost next time bootstrap is used.                                                                                                |
| cfg/guest-os.cfg           | Automatically generated when avocado vt-bootstrap is used from files within shared/cfg/guest-os/. Defines all supported guest operating system types, architectures, installation images, parameters, and disk device or image names.                                                                         |
| cfg/guest-hw.cfg           | All virtual and physical hardware related parameters are organized within variant names. Within subtest variants or the top-level test set definition, hardware is specified by Including, excluding, or filtering variants and keys established in this file.                                                |
| cfg/cdkeys.cfg             | Certain operating systems require non-public information in order to operate and or install properly. For example, installation numbers and license keys. None of the values in this file are populated automatically. This file should be edited to supply this data for use by the unattended install test. |
| cfg/virtio-win.cfg         |                                                                                                                                                                                                                                                                                                               |

 
 
 
#  Building test applications

这是描述如何从一个测试案列中建立一个测试应用。

## Dependencies

如果你写一个应用支持在测试目标上运行，把它放置到相对于你的测试案例的位置的目录../deps/\<name>/ ，获取这个目录的全路径的最早的方式是通过调用函数data_dir.get_deps_dir(“\<name>”)，不要忘记添加 from virttest import data_dir到你的测试案例中。

除了源代码文件之外，创建一个Makefile用于建立你的测试应用。下面的例子展示了timedrift测试案例的应用程序的Makefile。remote_build模块需要一个Makefile包含所有的测试应用。

``` stylus
CFLAGS+=-Wall
LDLIBS+=-lrt

.PHONY: clean

all: clktest get_tsc

clktest: clktest.o

get_tsc: get_tsc.o

clean:
        rm -f clktest get_tsc
```

## remote_build
为了简化在目标上应用的建立，并且当他们安装 pre-built，使用remote_build模块时去简化避免在目标上应用程序的建立。这个模块处理文件的传输和在目标上运行make。
一个简单的例子：

``` stylus
address = vm.get_address(0)
source_dir = data_dir.get_deps_dir("<testapp>")
builder = remote_build.Builder(params, address, source_dir)
full_build_path = builder.build()
```

在这个案子中，我们利用 .build()方法，去执行在builder中必要的方法去拷贝所有问价到目标，并且运行make（如果需要）。当做完后， .build()将返回在目标上的全路径到被刚刚建立的应用程序。当运行你的测试应用时确保使用这个路径，因为如果build参数改变了，路径也改变。例如：

``` stylus
session.cmd_status("%s --test" % os.path.join(full_build_path, "testapp"))
```


remote_build.Builder 类也能让你细微的控制你的构建过程。另一种方式去写上述的  .build() 调用：

``` stylus
builder = remote_build.Builder(params, address, source_dir)
if builder.sync_directories():
    builder.make()
full_build_path = builder.full_build_path
```

这种方式可能是有用的如果你举例在builder.make(),之前想要添加一个额外的命令去运行，也许可以去安装一些其他的依赖文件。
 
 
#  Networking
这儿我们有关于在Avocado-VT中networking安装的说明。

## Configuration
如何去配置可以允许所有传输通过virbr0的桥来转发：执行命令

``` stylus
$ echo "-I FORWARD -m physdev --physdev-is-bridged -j ACCEPT" > /etc/sysconfig/iptables-forward-bridged
$ lokkit --custom-rules=ipv4:filter:/etc/sysconfig/iptables-forward-bridged
$ service libvirtd reload
```

### Configure Static IP address in Avocado-VT

有时我们需要用带有静态IP地址的客户机来测试。

 - 例如，在测试环境中没有真实或者仿真的DHCP服务器
 - 例如，用我们不想改变网络配置的旧的内核测试
 - 例如，当DHCP存在问题时的测试。


在主机中创建一个桥（例如，'vbr'），配置它的IP为192.169.100.1，客户机能够通过它访问主机。并且在tests.cfg文件中分配 nic(s)’ ip，正常的执行测试。

tests.cfg：

``` stylus
ip_nic1 = 192.168.100.119
nic_mac_nic1 = 11:22:33:44:55:67
bridge = vbr
```

## TestCases


### Ntttcp

Ntttcp测试套件是一个用于windows的网络性能的测试，由Microsoft开发。它不是一个自由可再发行的二进制文件，所以你必须从网站下载，这边有直接下载的链接（记住它可能会改变）：

http://download.microsoft.com/download/f/1/e/f1e1ac7f-e632-48ea-83ac-56b016318735/NT%20Testing%20TCP%20Tool.msi

与它相关的基本文章知识：

http://msdn.microsoft.com/en-us/windows/hardware/gg463264


你需要添加数据包到winutils.iso，这个iso镜像带有测试windows的工具。首先下载这个镜像。[The get started documentation][19] 能帮助你下载如果你喜欢，但是直接下载的链接是这个：

http://assets-avocadoproject.rhcloud.com/static/winutils.iso


你需要把它的所有内容放在一个文件夹并且创建一个新的iso镜像。假设你想下载镜像到 /home/kermit/Downloads/winutils.iso 。你可以创建一个目录，进入它：

``` stylus
$ mkdir -p /home/kermit/Downloads
$ cd /home/kermit/Downloads
```


现在镜像，创建两个目录，一个用于安装，另一个用于内容：

``` stylus
$ wget http://people.redhat.com/mrodrigu/kvm/winutils.iso
$ mkdir original
$ sudo mount -o loop winutils.iso original
$ mkdir winutils
```
从original 拷贝所有内容到新架构：

``` stylus
$ cp -r original/* winutils/
```

在新架构上创建目标ntttcp目录：

``` stylus
$ mkdir -p winutils/NTttcp
```

下载安装和拷贝autoit脚本到新的架构，解除original的挂载：

``` stylus
$ cd winutils/NTttcp
$ wget http://download.microsoft.com/download/f/1/e/f1e1ac7f-e632-48ea-83ac-56b016318735/NT%20Testing%20TCP%20Tool.msi -O "winutils/NTttcp/NT Testing TCP Tool.msi"
$ cp /usr/local/autotest/client/virt/scripts/ntttcp.au3 ./
$ sudo umount original
```

备份旧的winutils.iso并且用mkisofs创建一个新的winutils.iso：

``` stylus
$ sudo mv winutils.iso winutils.iso.bak
$ mkisofs -o winutils.iso -max-iso9660-filenames -relaxed-filenames -D --input-charset iso8859-1 winutils
```

这就是它了。不要忘记把winutils放到能被Avocado-VT看见的合适的位置。


# Performance Testing

## Performance subtests

### network

 - [netperf (linux and windows)][20]
 - [ntttcp (windows)][21]


### block

 - [iozone (linux)][22]
 - [iozone (windows)][23]  (iozone has its own result analysis module)
 - iometer (windows) (not push upstream)
 - [ffsb (linux)][24]
 - [qemu_iotests (host)][25]
 - [fio (linux)][26]


## Environment setup

自动化测试已经支持性能测试的准备环境，客户或者主机因为一些配置需要重启。[setup script][27]


自动化测试支持numa pining。在tests.cfg文件中分配 “numanode=-1” ， then vcpu threads/vhost_net threads/VM memory will be pined to last numa node.如果你想 pin other processes to numa node,你可以使用numctl and taskset.

``` stylus
memory:
$ numactl -m $n $cmdline
cpu:
$ taskset $node_mask $thread_id
```
下面的内容是手动引导。


``` stylus
1.First level pinning would be to use numa pinning when starting the guest.
e.g.  $ numactl -c 1 -m 1 qemu-kvm  -smp 2 -m 4G <> (pinning guest memory and cpus to numa-node 1)

2.For a single instance test, it would suggest trying a one to one mapping of vcpu to pyhsical core.
e.g.
get guest vcpu threads id
$ taskset -p 40 $vcpus1  #(pinning vcpu1 thread to pyshical cpu #6 )
$ taskset -p 80 $vcpus2  #(pinning vcpu2 thread to physical cpu #7 )

3.To pin vhost on host. get vhost PID and then use taskset to pin it on the same soket.
e.g
$ taskset -p 20 $vhost #(pinning vcpu2 thread to physical cpu #5 )

4.In guest,pin the IRQ to one core and the netperf to another.
1) make sure irqbalance is off - `$ service irqbalance stop`
2) find the interrupts - `$ cat /proc/interrupts`
3) find the affinity mask for the interrupt(s) - `$ cat /proc/irq/<irq#>/smp_affinity`
4) change the value to match the proper core.make sure the vlaue is cpu mask.
e.g. pin the IRQ to first core.
   $ echo 01>/proc/irq/$virti0-input/smp_affinity
   $ echo 01>/proc/irq/$virti0-output/smp_affinity
5)pin the netserver to another core.
e.g.
$ taskset -p 02 netserver

5.For host to guest scenario. to get maximum performance. make sure to run netperf on different cores on the same numa node as the guest.
e.g.
$ numactl -m 1 netperf -T 4 (pinning netperf to physical cpu #4)
```

## Execute testing

 - 在Autotest服务器提交工作，仅仅执行 netperf.guset_exhost三次。


tests.cfg:

``` stylus
only netperf.guest_exhost
variants:
    - repeat1:
    - repeat2:
    - repeat3:
# vbr0 has a static ip: 192.168.100.16
bridge=vbr0
# virbr0 is created by libvirtd, guest nic2 get ip by dhcp
bridge_nic2 = virbr0
# guest nic1 static ip
ip_nic1 = 192.168.100.21
# external host static ip:
client = 192.168.100.15
```

结果文件：

``` stylus
$ cd /usr/local/autotest/results/8-debug_user/192.168.122.1/
$ find .|grep RHS
kvm.repeat1.r61.virtio_blk.smp2.virtio_net.RHEL.6.1.x86_64.netperf.exhost_guest/results/netperf-result.RHS
kvm.repeat2.r61.virtio_blk.smp2.virtio_net.RHEL.6.1.x86_64.netperf.exhost_guest/results/netperf-result.RHS
kvm.repeat3.r61.virtio_blk.smp2.virtio_net.RHEL.6.1.x86_64.netperf.exhost_guest/results/netperf-result.RHS
```

 - 在另一个环境（不同的数据包）用相同的配置提交相同的工作


``` stylus
$ cd /usr/local/autotest/results/9-debug_user/192.168.122.1/
$ find .|grep RHS
kvm.repeat1.r61.virtio_blk.smp2.virtio_net.RHEL.6.1.x86_64.netperf.exhost_guest/results/netperf-result.RHS
kvm.repeat2.r61.virtio_blk.smp2.virtio_net.RHEL.6.1.x86_64.netperf.exhost_guest/results/netperf-result.RHS
kvm.repeat3.r61.virtio_blk.smp2.virtio_net.RHEL.6.1.x86_64.netperf.exhost_guest/results/netperf-result.RHS
```


## Analysis result

Config file: perf.conf

``` stylus
[ntttcp]
result_file_pattern = .*.RHS
ignore_col = 1
avg_update =

[netperf]
result_file_pattern = .*.RHS
ignore_col = 2
avg_update = 4,2,3|14,5,12|15,6,13

[iozone]
result_file_pattern =
```

执行regression.py去对比两个结果：

``` stylus
login autotest server
$ cd /usr/local/autotest/client/tools
$ python regression.py netperf /usr/local/autotest/results/8-debug_user/192.168.122.1/ /usr/local/autotest/results/9-debug_user/192.168.122.1/
```

T-test:

 - scipy:http://www.scipy.org/
 - t-test: http://en.wikipedia.org/wiki/Student’s_t-test
 - 两个python模块 (scipy and numpy) 被需要
 - 用脚本在rhel6上自动安装numpy/scipy ：https://github.com/kongove/misc/blob/master/scripts/install-numpy-scipy.sh


Unpaired T-test被用于比较两个例子，用户可以检查p-value去知道是否存在 regression bug。如果统计学上两个例子的不同点被认为不是很显著（p<=0.05），它将在p-value前添加'+'或者'-'。（‘+’: avg_sample1 < avg_sample2, ‘-‘: avg_sample1 > avg_sample2）

 -  -只有超过95%的信任结果将会被在“Significance”部分添加"+/-"。
 -   + for cpu-usage means regression, “+” for throughput means improvement.”


## Regression results

 - [netperf.exhost_guest.html][28]
 - [fio.html][29]
 - Every Avg line represents the average value based on $n repetitions of the same test, and the following SD line represents the Standard Deviation between the $n repetitions.
 - 标准的偏差用平均的百分比来展示。
 - 两个平均值的不同之处的意义is calculated using unpaired T-test that takes into account the SD of the averages.
 - The paired t-test is computed for the averages of same category.
 - only over 95% confidence results will be added “+/-” in “Significance” part. “+” for cpu-usage means regression, “+” for throughput means improvement.

Highlight HTML result

 - green/red –> good/bad
 - Significance is larger than 0.95 –> green
 - dark green/red –> important (eg: cpu)
 - light green/red –> other
 - test time
 - version (only when diff)
 - other: repeat time, title
 - user light green/red to highlight small (< %5) DIFF
 - highlight Significance with same color in one raw
 - add doc link to result file, and describe color in doc


[netperf.avg.html][30]  -  Raw data that the averages are based on.


# Setup a virtual environment for multi host tests


## Problem:

对于多主机测试，多个物理系统经常被需要。可以使用两个虚拟客户机来进行大多数的自动化测试，除了Avocado-VTS（kvm，libvirt，....）。

然而，可以使用嵌套虚拟化去做为第一层客户机（L0）服务并且在它们里面运行嵌入的客户机。

这页解释了，如何设置（Fedora/RHEL）和对于这样的情况使用带有嵌套虚拟机的单独的电脑。小心这方面，嵌套虚拟机工作一般都是正常的，但一些案子可能会导致隐蔽的问题。不要使用嵌套虚拟机来做产品测试。

## Nested Virtualization:

 1. Emulated:

       qemu非常的慢
       
 

 2. Hardware accelerated:

Hardware for the accelerated nested virtualization AMD Phenom and never core extension (smv, NPT) Intel Nehalem and never core extension (vmx, EPT)

软件支持加速嵌入式虚拟机kvm，xen，vmware，.....几乎和本地客户机(1.0-0.1 of native quest performance)同样的速度。

性能依赖于加载的类型。加载IO可能很慢。 Without vt-d or AMD-Vi and network device pass through.

## Configuration for multi-host virt tests:


![enter description here][31]

## Config of host system

 - Intel CPU

options kvm_intel nested=1  在一些模块的配置文件的最后  /etc/modprobe.d/modules.conf

 - AMD CPU

options kvm_amd nested=1 在一些模块配置文件的最后 /etc/modprobe.d/modules.conf


## Config of Guest L0

 - Intel CPU
     - Virtual manager

    Procesor config->CPU Features->vmx set to require

     - Native qemu-kvm

    qemu-kvm -cpu core2duo,+vmx -enable-kvm .....

 - AMD CPU
     - Virtual manager

    Procesor config->CPU Features->svm set to require

     - Native qemu-kvm

    qemu-kvm -cpu qemu64,+svm -enable-kvm .....


## Config of Guest L0 System

在没有DHCP（dhcp和注意系统的dhcp冲突）的情况下用主机桥连接客户机L0桥。

 1. 破坏控制dhcp的libvirt桥。
 2. 使能网络服务的系统控制，使能network.service
 3. 手动添加新桥virbr0并且 insert to them L0 network adapter eth0 which is connected to host bridge


``` stylus
# interface connected to host system bridge
$ vi /etc/sysconfig/network-scripts/ifcfg-eth0
     NM_CONTROLLED="no"
     DEVICE="eth0"
     ONBOOT="yes"
     BRIDGE=virbr0

# Bridge has name virbr0 for compatibility with standard autotest settings.
$ vi /etc/sysconfig/network-scripts/ifcfg-virbr0
    DHCP_HOSTNAME="atest-guest"
    NM_CONTROLLED="no"
    BOOTPROTO="dhcp"
    ONBOOT="yes"
    IPV6INIT="no"
    DEVICE=virbr0
    TYPE=Bridge
    DELAY=0
```

确认关掉NetworkManager     systemctl disable NetworkManager.service

## Check Guest L0 System

modprobe kvm-intel 或者 modprobe kvm-amd应该工作。

## Start for multi-host virt tests:

## Manually from host machine

``` stylus
$ cd autotest/client/tests/virt/qemu/
$ sudo rm -rf results.*; sudo ../../../../server/autoserv -m guestL0_1,guestL0_2 multi_host.srv
```

## More details:

创立或者配置，根目录（这个git软件库的）也有简单的脚本去创建 L1 and L2 guests。并且L1，L2 libvirt 文件的参照也被添加 -  https://github.com/kashyapc/nvmx-haswell/blob/master/SETUP-nVMX.rst


# Multi Host Migration Tests

## Running Multi Host Migration Tests

Avocado-VT是我们的测试套件，但为了简便起见它只能在一个主机上运行。对于多主机测试，你将需要完整的 autotest + Avocado-VT package，并且程序将更复杂。我们将尽力让这个程序做为对象。


## Prerequesites

这个引导假定：

 1. 你拥有至少两个虚拟的能运行的机器，他们有共享的存储设置在[插入指定路径]。让我们称他们为 host1.foo.com 和 host2.foo.com。
 2. 你可以使用SSH无密码的以root用户登录这两台机器（意思是账户有一个SSH key设置，你可以用来运行测试）。
 3. 这些机器应该能够自由交流，注意防火墙的影响。在每个机器上你需要一个指定的NFS mount setup。





 - /var/lib/virt_test/isos
 - /var/lib/virt_test/steps_data
 - /var/lib/virt_test/gpg

他们需要一个NFS共享只读支持。为什么要只读？因为它更安全，我们排除删掉重要数据的可能性。除此之外上述数据是仅需要只读的。fstab例子：

``` stylus
myserver.foo.com:/virt-test/iso /var/lib/virt_test/isos nfs ro,nosuid,nodev,noatime,intr,hard,tcp 0 0
myserver.foo.com:/virt-test/steps_data  /var/lib/virt_test/steps_data nfs rw,nosuid,nodev,noatime,intr,hard,tcp 0 0
myserver.foo.com:/virt-test/gpg  /var/lib/virt_test/gpg nfs rw,nosuid,nodev,noatime,intr,hard,tcp 0 0
```

 - /var/lib/virt_test/images
 - /var/lib/virt_test/images_archive

这些都需要NFS共享读写支持（或者其他任何共享的存储你可以拥有）。这是必须的因为两个主机都需要看同样的一致的存储。fstab 例子：


``` stylus
myserver.foo.com:/virt-test/images_archive  /var/lib/virt_test/images_archive nfs rw,nosuid,nodev,noatime,intr,hard,tcp 0 0
myserver.foo.com:/virt-test/images /var/lib/virt_test/images  nfs rw,nosuid,nodev,noatime,intr,hard,tcp 0 0
```

images目录必须由你想要在上面运行你的测试的安装的客户机构成。他们必须匹配在Avocado-VT中客户机系统使用的文件名称。例如对于RHEL6.4而言，Avocado-VT使用的image名称是：

``` stylus
rhel64-64.qcow2
```
两次检查你文件在那儿：

``` stylus
$ ls /var/lib/virt_test/images
$ rhel64-64.qcow2
```


## Setup step by step
首先，递归的克隆autotest软件库。它是一个带有许多子模块的软件库，所以你可以看到许多输出：

``` stylus
$ git clone --recursive https://github.com/autotest/autotest.git
... lots of output ...
```

然后，编辑 global_config.ini 文件，改变key：

``` stylus
serve_packages_from_autoserv: True
```

to：

``` stylus
serve_packages_from_autoserv: False
```

然后你需要更新Avocado-VT的配置文件和sub tests（存在于分开的的软件库，不是git子模块）。这步你没必要下载JeOS文件，对于quest简单的回答'n'。

Note：下面描述的引导程序将在运行autoserv的命令上被自动的执行，触发测试。问题是，然后你将不能看到配置文件和修改优先的过滤器来真正的运行测试。因此这个文档将指导你手动的运行下面的步骤。

``` stylus
16:11:14 INFO | qemu test config helper
16:11:14 INFO |
16:11:14 INFO | 1 - Updating all test providers
16:11:14 INFO | Fetching git [REP 'git://github.com/autotest/tp-qemu.git' BRANCH 'master'] -> /var/tmp/autotest/client/tests/virt/test-providers.d/downloads/io-github-autotest-qemu
16:11:17 INFO | git commit ID is 6046958afa1ccab7f22bb1a1a73347d9c6ed3211 (no tag found)
16:11:17 INFO | Fetching git [REP 'git://github.com/autotest/tp-libvirt.git' BRANCH 'master'] -> /var/tmp/autotest/client/tests/virt/test-providers.d/downloads/io-github-autotest-libvirt
16:11:19 INFO | git commit ID is edc07c0c4346f9029930b062c573ff6f5433bc53 (no tag found)
16:11:20 INFO |
16:11:20 INFO | 2 - Checking the mandatory programs and headers
16:11:20 INFO | /usr/bin/7za
16:11:20 INFO | /usr/sbin/tcpdump
16:11:20 INFO | /usr/bin/nc
16:11:20 INFO | /sbin/ip
16:11:20 INFO | /sbin/arping
16:11:20 INFO | /usr/bin/gcc
16:11:20 INFO | /usr/include/bits/unistd.h
16:11:20 INFO | /usr/include/bits/socket.h
16:11:20 INFO | /usr/include/bits/types.h
16:11:20 INFO | /usr/include/python2.6/Python.h
16:11:20 INFO |
16:11:20 INFO | 3 - Checking the recommended programs
16:11:20 INFO | Recommended command missing. You may want to install it if not building it from source. Aliases searched: ('qemu-kvm', 'kvm')
16:11:20 INFO | Recommended command qemu-img missing. You may want to install it if not building from source.
16:11:20 INFO | Recommended command qemu-io missing. You may want to install it if not building from source.
16:11:20 INFO |
16:11:20 INFO | 4 - Verifying directories
16:11:20 INFO |
16:11:20 INFO | 5 - Generating config set
16:11:20 INFO |
16:11:20 INFO | 6 - Verifying (and possibly downloading) guest image
16:11:20 INFO | File JeOS 19 x86_64 not present. Do you want to download it? (y/n) n
16:11:30 INFO |
16:11:30 INFO | 7 - Checking for modules kvm, kvm-amd
16:11:30 WARNI| Module kvm is not loaded. You might want to load it
16:11:30 WARNI| Module kvm-amd is not loaded. You might want to load it
16:11:30 INFO |
16:11:30 INFO | 8 - If you wish, take a look at the online docs for more info
16:11:30 INFO |
16:11:30 INFO | https://github.com/autotest/virt-test/wiki/GetStarted
```

然后你需要拷贝multihost配置文件到适当的地方：

``` stylus
cp client/tests/virt/test-providers.d/downloads/io-github-autotest-qemu/qemu/cfg/multi-host-tests.cfg client/tests/virt/backends/qemu/cfg/
```

现在，修改文件：

``` stylus
server/tests/multihost_migration/control.srv
```

在那，你不得不改变 EXTRA_PARAMS去限制你想要在上面运行测试的客户机数目。在这个例子中，我们限制我们的测试到 RHEL 6.4。控制文件的额特别部分应该像：

``` stylus
EXTRA_PARAMS = """
only RHEL.6.4.x86_64
"""
```

重要的是要强调， the guests must be installed for this to work smoothly。然后最后一步是运行测试。对于机器主机名使用相同的约定，这儿有个命令你可以使用：

``` stylus
$ server/autotest-remote -m host1.foo.com,host2.foo.com server/tests/multihost_migration/control.srv
```

现在，你将能看到来自自动化测试远程输出的一堆输出。这是正常的，并且你应该有耐心指导所有的测试做完。


### Writing Multi Host Migration tests

#### Scheme:

![enter description here][32]


[Source file for the diagram above (LibreOffice file)][33]


#### Example:

``` stylus
class TestMultihostMigration(virt_utils.MultihostMigration):
    def __init__(self, test, params, env):
        super(testMultihostMigration, self).__init__(test, params, env)

    def migration_scenario(self):
        srchost = self.params.get("hosts")[0]
        dsthost = self.params.get("hosts")[1]

        def worker(mig_data):
            vm = env.get_vm("vm1")
            session = vm.wait_for_login(timeout=self.login_timeout)
            session.sendline("nohup dd if=/dev/zero of=/dev/null &")
            session.cmd("killall -0 dd")

        def check_worker(mig_data):
            vm = env.get_vm("vm1")
            session = vm.wait_for_login(timeout=self.login_timeout)
            session.cmd("killall -9 dd")

        # Almost synchronized migration, waiting to end it.
        # Work is started only on first VM.

        self.migrate_wait(["vm1", "vm2"], srchost, dsthost,
                          worker, check_worker)

        # Migration started in different threads.
        # It allows to start multiple migrations simultaneously.

        # Starts one migration without synchronization with work.
        mig1 = self.migrate(["vm1"], srchost, dsthost,
                            worker, check_worker)

        time.sleep(20)

        # Starts another test simultaneously.
        mig2 = self.migrate(["vm2"], srchost, dsthost)
        # Wait for mig2 finish.
        mig2.join()
        mig1.join()

mig = TestMultihostMigration(test, params, env)
# Start test.
mig.run()
```


当你调用：

``` stylus
mig = TestMultihostMigration(test, params, env)
```

发生了什么：

 1. VM的磁盘将被准备
 2. 同步服务将要开始
 3. 在VM创建磁盘后所有主机将同步。


当你调用函数：

``` stylus
migrate()
```

在图表中发生了什么：


| source                                   | destination            |
| ---------------------------------------- | ---------------------- |
| It prepare VM if machine is not started. |                        |
| Start work on VM.                        |                        |
| mig.migrate_vms_src()                    | mig.migrate_vms_dest() |
|                                          |                        |
| Wait for finish migration on all hosts.  |                        |


重要的是要注意，migrations 是使用tcp协议来完成，因为其他主机不知道多主机migration。

``` stylus
def migrate_vms_src(self, mig_data):
    vm = mig_data.vms[0]
    logging.info("Start migrating now...")
    vm.migrate(mig_data.dst, mig_data.vm_ports)
```

这个例子在migration中只migrates了第一个定义的机器。更好的例子在virt_utils.MultihostMigration.migrate_vms_src。这个函数migrates all machines defined for migration.


# Links with downloadable images for virt tests

这是可以用于测试的主要位置，我们想要维持iso文件位置的数据。

Update：现在我们有一个主要位置用于定义下载，在源代码树中：

``` stylus
shared/downloads/
```

包含一堆 .ini文件，每一个都带有download的定义。预计这将是比这页更新的数据。你可以查看一些有用的下载，下载文件使用：

``` stylus
scripts/download_manager.py
```

## Winutils ISO

windows工具现在可以在这找到：

http://assets-avocadoproject.rhcloud.com/static/winutils.iso

### How to update winutils.iso

这基本上是一些用于windows测试的文件。如果你想要更新这个文件，你将不得不选择这个iso文件，把它提取到一个目录中，进行更改，重新灌录iso并且上传会主要的位置。

## JeOS image


你可以在这找到一些JeOS images：

https://assets-avocadoproject.rhcloud.com/static/jeos-21-64.qcow2.7z


https://assets-avocadoproject.rhcloud.com/static/SHA1SUM_JEOS21

https://assets-avocadoproject.rhcloud.com/static/jeos-23-64.qcow2.7z

https://assets-avocadoproject.rhcloud.com/static/SHA1SUM_JEOS23


Unfortunately the host assets-avocadoproject.rhcloud.com is configured in such a way that exploring that base directory won’t give you a file listing, and you have to provide the exact urls of what you’re looking for.


### How to update JeOS

JeOS可以通过安装它来更新，就像一个普通的OS。你可以用avocado-vt来做它，选择一个无人值守的安装测试，在这个例子中，我们将使用无人值守的安装使用https安装和网络安装：

``` stylus
avocado run io-github-autotest-qemu.unattended_install.url.http_ks.default_install.aio_native
```
JeOS安装有一个技巧，用0填充两个qcow2 image，所以我们后面能用qemu img替换这些0。一旦image被安装，你可以使用我们的辅助脚本，位于scripts/package_jeos.py在avocado-vt源码树中。这个脚本使用qemu-img去修剪0 image，确保结果的qcow2 image是最小的。命令类似于：


``` stylus
$ qemu-img convert -f qcow2 -O qcow2 jeos-file-backup.qcow2 jeos-file.qcow2
```

然后它将使用7zip压缩它，未avocado-vt用户保存位置并加速下载。命令类似于：

``` stylus
$ 7za a jeos-file.qcow2.7z jeos-file.qcow2
```

正如提到的，脚本应该用程序帮助你。


# GlusterFS support

GlusterFS 是一个开源软件，能够扩展到几PB(actually, 72 brontobytes!)的分布式文件系统，处理成千上万的客户。GlusterFS 群集能够通过 Infiniband RDMA 或者TCP/IP相互联系把存储块连接到一起，在一个单独的全局命名空间中聚合磁盘，内存资源，管理数据。GlusterFS 是基于一个堆叠的用户空间设计并且能够传递异常的性能给不同的工作负载。

更多有关GlusterFS 的详细说明见下面的链接：
http://www.gluster.org/about/

GlusterFS 做为一个新的backend块添加在qemu上，为了利用这个特性我们需要下面组件。

更多有关 GlusterFS-QEMU集合的详细信息可以在下面找到：

http://raobharata.wordpress.com/2012/10/29/qemu-glusterfs-native-integration/

 1. Qemu- 1.3, 03Dec2012
 2. GlusterFS-3.4
 3. Libvirt-1.0.1, 15Dec2012

## How to use in Avocado-VT

你可以使用Avocado-VT去用下面的步骤去测试GlusterFS 支持。

 1. 用下面的修改去编辑qemu/cfg/tests.cfg

``` stylus
only glusterfs_support
remove ‘only no_glusterfs_support’ line from the file
```

 1. 有选择地，编辑shared/cfg/guest-hw.cfg，去修改gluster volume name 和 brick 路径，默认值是：

``` stylus
gluster_volume_name = test-vol
gluster_brick = /tmp/gluster
```

## How to use manually

下面仅仅是一个例子展示了如何创建一个 gluster volume，并且运行在那个volume上手动的运行一个客户机。


## Starting Gluster daemon

``` stylus
$ service glusterd start
```

## Gluster volume creation

``` stylus
$ gluster volume create [volume-name]  [hostname/host_ip]:/[brick_path]
```
例如： gluster volume create test-vol satheesh.ibm.com://home/satheesh/images_gluster

## Qemu Img creation

``` stylus
$ qemu-img create gluster://[hostname]:0/[volume-name]/[image-name] [size]
```

E.g.: qemu-img create gluster://satheesh.ibm.com:0/test-vol/test_gluster.img 10G


## Example of qemu cmd Line

``` stylus
$ qemu-system-x86_64 --enable-kvm -smp 4 -m 2048 -drive file=gluster://satheesh.ibm.com/test-vol/test_gluster.img,if=virtio -net nic,macaddr=52:54:00:09:0a:0b -net tap,script=/path/to/qemu-ifupVirsh
```


# Setting up a Regression Test Farm for KVM

你有所有的上行代码，并且你想知道KVM内置的Red Hat测试是否有许多秘密武器。不，它没有。

然而，它是一个复杂的尝试，因为有许多相关细节。Farm设置和维持是不容易的，考虑到大量可能会失败的事情（machines misbehave, 网络问题，git软件库不可用等等）。你已经得到警告。

对于刚刚说的所有，我们会分享我们已经做了什么。我们清空了配置文件和扩展，released them upstream，和这个程序一起，我们希望它会对你有用。这也将在一个单独的主机上覆盖KVM测试，做为关联许多主机的测试和Libvirt测试是一个正在进行中的工作。

基本的步骤是：

 1. 安装一个autotest服务。
 2. 添加机器到服务（测试节点）。这些机器是将要测试的虚拟主机。
 3. 准备虚拟化测试jobs并且安排他们。
 4. 在你的环境中设置cobbler，这样你能安装主机。
 5. 大量的试验和错误，直到你把所有小细节整理出来。

我们花几年时间重复上面的所有的步骤，来完善程序，我们愿意用文档记录所有使它达到最好的程度。然而我们害怕的是你将不得不做大量工作使程序适应你的环境。

## Some conventions

我们假设你将安装autotest到它的默认的上行位置。

/usr/local/autotest

因此这儿大量的参考路径把这当做基本目录。

## CLI vs Web UI

在这个文档期间，我们频繁使用术语CLI和Web UI。

通过CLI，意味着我们用特定的程序：

/usr/local/autotest/cli/autotest-rpc-client

这是位于autotest代码中的检测。

通过Web UI，意味着我们autotest的web界面，可能通过这被访问：

http://your-autotest-server.com/afe


## Step 1 - Install an autotest server

在你的测试实验室中拥有因特网，这应该是最简单的步骤。我们要么获取一个在实验室可用的VM或者一个基本机器（它不会起作用，这边我们使用VM）。从现在开始我们称它为“服务器”框。

服务器的硬盘应该有足够的空间来给测试结果。我们发现没超过6个月将会至少有250G的保存数据，条件是QEMU没有崩溃。

你可以遵循下面链接描述的步骤：

https://github.com/autotest/autotest/wiki/AutotestServerInstallRedHat

对于Red Hat派生物（例如Fedora和RHEL）

https://github.com/autotest/autotest/wiki/AutotestServerInstall

对于Debian的派生物（Debian，Ubuntu）。

注意到在使用在文档开始位置的正确的参考安装脚本是首选方法。如果你的实验室中有网络将可以更好的工作。假设你那没有网络，你需要依照在 ‘installing with the script’ 使用说明之后的说明。你有任何问题可以告知我们。


## Step 2 - Add test machines

它应该不言而喻，但你不得不添加的机器必须是支持虚拟化的（支持KVM）。

你能要么通过CLI要么Web UI添加机器，依照下面的文档：

https://github.com/autotest/autotest/wiki/ConfiguringHosts


如果你不想阅读那，我将尽力尽力写一个快速手记有关如何去做。

假设你有两台x86_64主机，一台AMD和其它。他们的主机名是：

foo-amd.bazcorp.com foo-intel.bazcorp.com

我将创建两个标签，amd64 和 intel64，我也将创建一个标签表明机器可以被cobbler分配。这是因为你能让autotest在任何能够比配已给标签的机器上运行一个job。

做为autotest用户记录：

``` stylus
$ /usr/local/autotest/cli/autotest-rpc-client label create -t amd64
Created label:
    'amd64'
$ /usr/local/autotest/cli/autotest-rpc-client label create -t intel64
Created label:
    'intel64'
$ /usr/local/autotest/cli/autotest-rpc-client label create hostprovisioning
Created label:
    'hostprovisioning'
```

然后我将用合适的标签创建每台机器。

``` stylus
$ /usr/local/autotest/cli/autotest-rpc-client host create -t amd64 -b hostprovisioning foo-amd.bazcorp.com
Added host:
    foo-amd.bazcorp.com

$ /usr/local/autotest/cli/autotest-rpc-client host create -t intel64 -b hostprovisioning foo-intel.bazcorp.com
Added host:
    foo-intel.bazcorp.com
```

## Step 3 - Prepare the test jobs

现在你不得不拷贝插件，插件我们开发用来扩展CLI去解析virt jobs中附加的信息：

``` stylus
$ cp /usr/local/autotest/contrib/virt/site_job.py /usr/local/autotest/cli/
```

这应该足够去使能所有的额外功能。

你也应该拷贝一份我们发行出来用作参考的的site-config.cfg文件，给qemu配置模块：

``` stylus
$ cp /usr/local/autotest/contrib/virt/site-config.cfg /usr/local/autotest/client/tests/virt/qemu/cfg
```
意识到你需要很好的阅读这个文件，并且后面，根据你的测试需求配置它。我们特别的强调你可以创建一个你想要测试的git软件库的私有git mirrors，so you tax the upstream mirrors less，并且增加了可靠性。


现在，它能在Fedora 18，upstream kvm上运行 regression testing ， 如果你有一个cobbler功能的实例，带有一个名为f18-autotest-kvm的配置文件能被恰当的安装在你的机器上。Having that properly set up may open another can of worms.

我们在我们的服务器上安排jobs的一种简单的方式是使用cron去安排你想测试东西的每天测试jobs。这儿有一个例子在'out of box'工作。如果你有一个你以收到邮件通知为目的创建的因特网邮件列表，叫做autotest-virt-jobs@foocorp.com ，你可以把它放在服务器中用户自动测试的crontab上：


``` stylus
07 00 * * 1-7 /usr/local/autotest/cli/autotest-rpc-client job create -B never -a never -s -e autotest-virt-jobs@foocorp.com -f "/usr/local/autotest/contrib/virt/control.template" -T --timestamp -m '1*hostprovisioning' -x 'only qemu-git..sanity' "Upstream qemu.git sanity"
15 00 * * 1-7 /usr/local/autotest/cli/autotest-rpc-client job create -B never -a never -s -e autotest-virt-jobs@foocorp.com -f "/usr/local/autotest/contrib/virt/control.template" -T --timestamp -m '1*hostprovisioning' -x 'only f18..sanity' "Fedora 18 koji sanity"
07 01 * * 1-7 /usr/local/autotest/cli/autotest-rpc-client job create -B never -a never -s -e autotest-virt-jobs@foocorp.com -f "/usr/local/autotest/contrib/virt/control.template" -T --timestamp -m '1*hostprovisioning' -x 'only qemu-git..stable' "Upstream qemu.git stable"
15 01 * * 1-7 /usr/local/autotest/cli/autotest-rpc-client job create -B never -a never -s -e autotest-virt-jobs@foocorp.com -f "/usr/local/autotest/contrib/virt/control.template" -T --timestamp -m '1*hostprovisioning' -x 'only f18..stable' "Fedora 18 koji stable"
```

那应该足够有一个 sanity 和 stable job：

 - Fedora 18.
 - qemu.git userspace and kvm.git kernel.


这些 ‘sanity’ 和 ‘stable’ jobs做了什么？简要说：

 - 主机操作系统通过cobbler安装
 - 主机操作系统最新的kernel安装（要么建立在Fedora的最新的kernel，要么检测，编译，安装kvm.git）

## sanity job

 - 安装最新的Fedora 18 qemu-kvm，或者编译最新的 qemu.git
 - 安装一个带有Fedora 18 的VM，启动，重启，用所有支持的协议做简单的单独的主机迁移
 - 花大概两小时。实际上，我们内部测试了更多的客户机，但他们不是可以广泛可用的（RHEL 6 和 Windows 7），所以我们用Fedora 18替换他们。

## stable job

 - 和上面一样，但有更多的网络工作，timedrift 和其他测试。


## Setup cobbler to install hosts

Cobbler是一个安装服务，控制你的x86_64基本虚拟化主机的DHCP ，（或者）PXE boot。通过下面的资源，你能学习如何设置它;


https://github.com/cobbler/cobbler/wiki/Start%20Here

你设置它来进行简单的安装，并且你可能只需要导入一个Fedora 18 DVD到它里面，所以它能用于安装你的主机。跟随着导入进程，你将拥有一份创建的‘profile’，它是一个描述一个操作系统能被安装在你的虚拟化主机的标签。我们选择的标签是上面已经提到过的 f18-autotest-kvm。如果你想要改变名字，相应地你将不得不改变 site-config.cfg文件。

你也将不得不添加你的测试机到你的cobbler服务器，并且将设置远程控制他们（开关电源）。

下面的内容是重要的：

在autotest服务中你的机器的主机名必须是在cobbler中你的系统的名字。

所以，对于假设的例子，你将不得不在cobbler中用名字foo-amd.bazcorp.com foo-intel.bazcorp.com设置系统。那完全正确，系统的‘名字’必须是‘主机名’。否则autotest将询问cobbler并且cobbler不知道autotest谈论的是哪台机器。


我们的其他的设想在这儿：

1）我们有一个（只读的，避免人们错误的删除了系统）NFS共享，里面有Fedora 18 DVD和其他ISOS。基本目录的数据结构可能看上去像这样：

``` stylus
.
|-- linux
|   `-- Fedora-18-x86_64-DVD.iso
`-- windows
    |-- en_windows_7_ultimate_x64_dvd_x15-65922.iso
    |-- virtio-win.iso
    `-- winutils.iso
```
这是为了防止您合法地有权下载和使用Windows 7，例如。


2）我们有另一个带有qcow2 images备份空间的共享的NFS以防测试期间出现损坏，并且你想要人们去分析它们。数据结构将是：


``` stylus
.
|-- foo-amd
`-- bar-amd
```

那是，在你的网格上你的每个主机机器都有的一个目录。确保它们最终在kickstart中正确配置。

现在这里是kickstart的一个摘录，我们学到了一些经验。一些注意点：

 - 这不是一个完全形成的，功能的kickstart，仅仅为了防止你没注意到。
 - 这是希望你阅读它，理解它，并适应你的需要。如果你把它粘贴到你的kickstart，告诉我它不工作，我会默认忽略你的电子邮件，如果你坚持，你的电子邮件将被过滤掉，去垃圾桶。


``` stylus
install

text
reboot
lang en_US
keyboard us
rootpw --iscrypted [your-password]
firewall --disabled
selinux --disabled
timezone --utc America/New_York
firstboot --disable
services --enabled network --disabled NetworkManager
bootloader --location=mbr
ignoredisk --only-use=sda
zerombr
clearpart --all --drives=sda --initlabel
autopart
network --bootproto=dhcp --device=eth0 --onboot=on

%packages
@core
@development-libs
@development-tools
@virtualization
wget
dnsmasq
genisoimage
python-imaging
qemu-kvm-tools
gdb
iasl
libvirt
python-devel
ntpdate
gstreamer-plugins-good
gstreamer-python
dmidecode
popt-devel
libblkid-devel
pixman-devel
mtools
koji
tcpdump
bridge-utils
dosfstools
%end

%post

echo "[nfs-server-that-holds-iso-images]:[nfs-server-that-holds-iso-images]/base_path/iso /var/lib/virt_test/isos nfs ro,defaults 0 0" >> /etc/fstab
echo "[nfs-server-that-holds-iso-images]:[nfs-server-that-holds-iso-images]/base_path/steps_data  /var/lib/virt_test/steps_data nfs rw,nosuid,nodev,noatime,intr,hard,tcp 0 0" >> /etc/fstab
echo "[nfs-server-that-has-lots-of-space-for-backups]:/base_path/[dir-that-holds-this-hostname-backups] /var/lib/virt_test/images_archive nfs rw,nosuid,nodev,noatime,intr,hard,tcp 0 0" >> /etc/fstab
mkdir -p /var/lib/virt_test/isos
mkdir -p /var/lib/virt_test/steps_data
mkdir -p /var/lib/virt_test/images
mkdir -p /var/lib/virt_test/images_archive

mkdir --mode=700 /root/.ssh
echo 'ssh-dss [the-ssh-key-of-the-Server-autotest-user]' >> /root/.ssh/authorized_keys
chmod 600 /root/.ssh/authorized_keys

ntpdate [your-ntp-server]
hwclock --systohc

systemctl mask tmp.mount
%end
```

## Painful trial and error process to adjust details

毕竟，你可以开始运行你的测试工作，看看什么东西需要修复。 通过使用自动测试用户登录到服务器，可以轻松地运行作业，并使用命令：

``` stylus
$ /usr/local/autotest/cli/autotest-rpc-client job create -B never -a never -s -e autotest-virt-jobs@foocorp.com -f "/usr/local/autotest/contrib/virt/control.template" -T --timestamp -m '1*hostprovisioning' -x 'only f18..sanity' "Fedora 18 koji sanity"
```

正如你可能已经猜到的，这将安排一个Fedora 18 sanity job。 所以理解它，并一步一步地解决。 如果有什么问题，你可以看看这：


https://github.com/autotest/autotest/wiki/DiagnosingFailures

看看它是否有帮助，你可以在邮件列表上询问，但在你让我们一步一步的引导你通过所有过程之前，请先相当好的完成你的工作。这已经是一步步的步骤了。
完成了，好运，开心的测试吧！


# Installing Windows virtio drivers with Avocado-VT

所以，你想要使用Avocado-VT去安装windows guests。您还希望它们与为Windows开发的半虚拟化驱动程序一起安装。你来到了正确的地方。


## A bit of context on windows virtio drivers install

到目前为止，这种安装方法涵盖存储（viostor）和网络（NetKVM）驱动程序。 Avocado-VT使用带有Windows应答文件的启动软盘，以便执行Windows客户端的无人值守安装。 对于winXP和win2003，无人参与文件是简单的.ini文件，而对于win2008和更高版本，无人参与文件是XML文件。

为了在客户机安装过程中安装virtio驱动程序，KVM自动测试必须通知windows安装程序在哪查找驱动程序。 因此，我们从以下假设开始工作：

 1. 您已经有一个iso文件，其中包含netikvm和viostor的windows virtio驱动程序（inf文件）。 如果你不确定如何生成该iso，在kvm test目录下有一个在contrib下的脚本示例。这里是一个例子，如何组织这个cd中的文件，假设iso映像安装在/ tmp / virtio-win下（实际的cd有更多的文件，但我们只采取了关于示例的部分，win7 64位）。


``` stylus
/tmp/virtio-win/
/tmp/virtio-win/vista
/tmp/virtio-win/vista/amd64
/tmp/virtio-win/vista/amd64/netkvm.cat
/tmp/virtio-win/vista/amd64/netkvm.inf
/tmp/virtio-win/vista/amd64/netkvm.pdb
/tmp/virtio-win/vista/amd64/netkvm.sys
/tmp/virtio-win/vista/amd64/netkvmco.dll
/tmp/virtio-win/vista/amd64/readme.doc
/tmp/virtio-win/win7
/tmp/virtio-win/win7/amd64
/tmp/virtio-win/win7/amd64/balloon.cat
/tmp/virtio-win/win7/amd64/balloon.inf
/tmp/virtio-win/win7/amd64/balloon.pdb
/tmp/virtio-win/win7/amd64/balloon.sys
/tmp/virtio-win/win7/amd64/blnsvr.exe
/tmp/virtio-win/win7/amd64/blnsvr.pdb
/tmp/virtio-win/win7/amd64/vioser.cat
/tmp/virtio-win/win7/amd64/vioser.inf
/tmp/virtio-win/win7/amd64/vioser.pdb
/tmp/virtio-win/win7/amd64/vioser.sys
/tmp/virtio-win/win7/amd64/vioser-test.exe
/tmp/virtio-win/win7/amd64/vioser-test.pdb
/tmp/virtio-win/win7/amd64/viostor.cat
/tmp/virtio-win/win7/amd64/viostor.inf
/tmp/virtio-win/win7/amd64/viostor.pdb
/tmp/virtio-win/win7/amd64/viostor.sys
/tmp/virtio-win/win7/amd64/wdfcoinstaller01009.dll
...
```

如果您计划安装WinXP或Win2003，您还应该有一个预制的带有virtio 驱动和配置文件的软盘映像，安装程序将从中读取获取正确的驱动。不幸的是，我没有太多的信息有关如何建立该文件，如果你愿意测试这些客户操作系统，你可能会有已经组装好的内核。


所以你必须映射您的包含驱动程序的cd路径到配置变量中。我们希望通过与virtio驱动团队的合作来提高它。


## Step by step procedure

我们假定你已经有了virtio cd的正确地装配，以及windows iso确实匹配了我们在test_base.cfg例子中的提供。不要担心，我们尽可能多地使用MSDN中的文件，来标准化。

我们将使用win7 64位（非sp1）为例，所以你需要的CD是：

``` stylus
cdrom_cd1 = isos/windows/en_windows_7_ultimate_x86_dvd_x15-65921.iso
sha1sum_cd1 = 5395dc4b38f7bdb1e005ff414deedfdb16dbf610
```


这个文件可以从MSDN网站下载，所以你可以验证它的SHA1总和是否匹配。

 1. git克隆autotest到一个便捷的位置，例如$HOME/Code/autotest。看[the download source documentation][34]，请使用git并克隆repo到提到的位置。
 2. 执行get_started.py脚本（see the get started documentation <../GetStartedGuide>）。它将创建我们期望cd文件可用的目录。你不需要下载Fedora 14 DVD，但是你需要下载winutils.iso cd （在下面的例子，我跳过了下载，因为我有这个文件，所以我可以将其复制到预期的位置，这是/ tmp / kvm_autotest_root / isos / windows）。请阅读脚本中提到的文档，以避免缺少安装的软件包和其他配置错误。
 3. 在 /tmp/kvm_autotest_root/isos 目录下创建windows 目录。
 4. 拷贝你的windows 7 iso到 /tmp/kvm_autotest_root/isos/windows
 5. 编辑文件cdkeys.cfg，并将windows 7 64位的密钥放在该文件上
 6. 编辑文件win-virtio.cfg并验证路径是否正确。你可以通过这个会话看到


``` stylus
64:
    unattended_install.cdrom, whql.support_vm_install:
        # Look at your cd structure and see where the drivers are
        # actually located (viostor and netkvm)
        virtio_storage_path = 'F:\win7\amd64'
        virtio_network_path = 'F:\vista\amd64'

        # Uncomment if you have a nw driver installer on the iso
        #virtio_network_installer_path = 'F:\RHEV-Network64.msi'
```

 7. 如果您使用带有本文开头提到的布局的cd，路径已经是正确的。然而，如果他们不同（更有可能），你必须调整路径。不要忘记阅读并在win-virtio.cfg文件中根基评论的指示做出所有的配置。
 8. 在 tests.cfg文件中，你必须启动windows 7的virtio的安装。在下面的块中，你不得不改变  *only rtl8139* 到 *only virtio_net* ， *only ide* 到 *only virtio-blk* 。你通知自动测试，你只想要一个带有virtio硬盘和网络设备的vm安装。

``` stylus
# Runs qemu-kvm, Windows Vista 64 bit guest OS, install, boot, shutdown
- @qemu_kvm_windows_quick:
    # We want qemu-kvm for this run
    qemu_binary = /usr/bin/qemu-kvm
    qemu_img_binary = /usr/bin/qemu-img
    # Only qcow2 file format
    only qcow2
    # Only rtl8139 for nw card (default on qemu-kvm)
    only rtl8139
    # Only ide hard drives
    only ide
    # qemu-kvm will start only with -smp 2 (2 processors)
    only smp2
    # No PCI assignable devices
    only no_pci_assignable
    # No large memory pages
    only smallpages
    # Operating system choice
    only Win7.64
    # Subtest choice. You can modify that line to add more subtests
    only unattended_install.cdrom, boot, shutdown
```



 9. 你必须改变tests.cfg的底部使其看起来像下面，这意味着你通知自动测试只运行上面提到的测试集，而不是默认，安装Fedora 15。

``` stylus
only qemu_kvm_windows_quick
```

 10. 根据get_started.py的输出，可以执行以运行自动测试的命令是（请以root用户运行或者使用sudo）

``` stylus
$ $HOME/Code/autotest/client/bin/autotest $HOME/Code/autotest/client/tests/kvm/control
```

 11. Profit！你自动安装带有virtio驱动的windows 7将被执行。

如果你想安装其它客户机，你可以想象，你可以用其它客户机改变 *only Win7.64* ，例如 *only Win2008.64.sp2* 。现在，在第一次执行安装时，最好观察安装以查看是否没有问题，例如一个错误的cd秘钥阻止安装发生。Avocado-VT打印所使用的qemu命令行，因此您可以查看可以连接到哪个vnc显示器以观察正在安装的vm。

# Running QEMU kvm-unit-tests

当前有两种方式去运行 kvm-unit-tests。每个测试结果更新的一种方法是[Run kvm-unit-tests in avocado][35]，稍旧的一种方法是[Run kvm-unit-tests in avocado-vt][36] 这需要手动修改并且只报告整体结果。

## Run kvm-unit-tests in avocado
有一个contrib脚本可以使用外部运行器Avocado功能运行kvm-unit-test。它可选择从Git下载最新的kvm-unit-test，编译它并在avocado中运行测试。

contrib 脚本位于$AVOCADO/contrib/testsuites/run-kvm-unit-test.sh或者可以从这下载： https://raw.githubusercontent.com/avocado-framework/avocado/master/contrib/testsuites/run-kvm-unit-test.sh

然后你可以简单的运行它并等待结果：

``` stylus
$ ./contrib/testsuites/run-kvm-unit-test.sh
JOB ID     : ca00d570f03b4942368ec9c407d69a881d98eb9d
JOB LOG    : /home/medic/avocado/job-results/job-2016-07-04T09.38-ca00d57/job.log
TESTS      : 38
 (01/38) access: PASS (4.77 s)
 (02/38) apic: PASS (5.10 s)
 (03/38) debug: PASS (1.82 s)
 (04/38) emulator:  ^C
Interrupt requested. Waiting 2 seconds for test to finish (ignoring new Ctrl+C until then)

INTERRUPTED (0.84 s)
RESULTS    : PASS 3 | ERROR 0 | FAIL 0 | SKIP 34 | WARN 0 | INTERRUPT 1
JOB HTML   : /home/medic/avocado/job-results/job-2016-07-04T09.38-ca00d57/html/results.html
TESTS TIME : 12.54 s
```

Note：

您可以指定各种选项，包括avocado参数。使用 -h 去查看所有（例如：通配符指定测试模式或路径以避免（重新）下载kvm-unit-test）


## Run kvm-unit-tests in avocado-vt

一段时间以来，qemu-kvm包含一个单元测试套件，可用于评估一些KVM子系统的行为。理想情况下，如果您运行最新的qemu和最新的Linux KVM树，它们应该是PASS。Avocado-VT很长一段时间都支持以自动化的方式运行它们。这是一个很好的机会，把你的git分支加入到unittest，从一个干净的状态开始（KVM自动测试将从您的git树获取，保留您的实际开发树完好无损和从头做的事情，这是不太可能掩盖问题）。


### A bit of context on Avocado-VT build tests

人们通常不知道Avocado-VT支持从许多不同的软件源中构建和安装用于测试目的的QEMU / KVM。你可以：

 1. 从一个git软件库构建qemu-kvm（最常见的选择是为开发者黑客代码）
 2. 用rpm文件安装qemu-kvm（人们测试一个新建的rpm包）
 3. 直接从Red Hat构建系统安装qemu-kvm（Koji是Fedora构建系统的实例，Brew是相同的，但是对于RHEL而言不同。有了这个，我们可以对Fedora和RHEL包执行质量控制，试图在软件包打断用户之前预料到破坏的包）


对于本文，我们将专注于git的基本构建。 此外，我们专注于Fedora和RHEL。我们将尝试用一个非常通用的方式写文章，欢迎改进这一点，如何在你最喜欢的linux发行版上做同样的细节。


### Before you start

您需要验证您是否可以从源代码以及单元测试套件实际构建qemu-kvm。

 1. 确保已安装了相应的软件包。你可以阅读[the install prerequesite packages (client) section][37]获取更多信息。

### Step by step procedure

 1. Git克隆一份autotest到方便的位置，例如 $HOME/Code/autotest 。查看[the download source documentation][38] ，请使用git并克隆软件库到提到的位置。
 2. 执行get_started.py脚本（查看 [the get started documentation][39] ，如果你只是想运行单元测试，你可以安全地跳过每一个iso下载可能，因为qemu-kvm将直接引导小内核镜像（单元测试），而不是完全的操作系统安装。）
 3. 由于运行单元测试是相当独立于其他你可以做的Avocado-VT测试的，它是人们感兴趣的东西，我们为它准备了一个特殊的控制文件和一个特殊的配置文件。在kvm目录中，您可以看到文件unittests.cfg control.unittests。 您只需要编辑unittests.cfg。
 4. 文件unittests.cfg是用于运行单元测试的独立配置。 它由一个build变量和unittests变量组成。 编辑文件，它将如下所示：

``` stylus
... bunch of params needed for the Avocado-VT preprocessor
# Tests
variants:
    - build:
        type = build
        vms = ''
        start_vm = no
        # Load modules built/installed by the build test?
        load_modules = no
        # Save the results of this build on test.resultsdir?
        save_results = no
        variants:
            - git:
                mode = git
                user_git_repo = git://git.kernel.org/pub/scm/virt/kvm/qemu-kvm.git
                user_branch = next
                user_lbranch = next
                test_git_repo = git://git.kernel.org/pub/scm/virt/kvm/kvm-unit-tests.git

    - unittest:
        type = unittest
        vms = ''
        start_vm = no
        unittest_timeout = 600
        testdev = yes
        extra_params += " -S"
        # In case you want to execute only a subset of the tests defined on the
        # unittests.cfg file on qemu-kvm, uncomment and edit test_list
        #test_list = idt_test hypercall vmexit realmode

only build.git unittest
```

 5. 正如你可以看到上面，你有些地方既指定用户空间git repo也指定了单元测试 git repo。你可以随意用你自己的git repo替换user_git_repo。 它可以是一个远程git位置，或者它可以简单地是一个克隆的树在您的开发机器的路径。
 6. 对于Fedora 15，与gcc 4.6.0一起，编译更严格，所以代码中有未使用的变量这类事件将导致构建失败。您可以通过为qemu-kvm用户空间构建提供额外的配置脚本选项来禁用此严格级别。在user_git_repo行下面，您可以设置变量extra_configure_options去包括--disable-werror。假设你还希望Avocado-VT从我的本地树中获取/ home / lmr / Code / qemu-kvm，master分支，与kvm-unit-tests repo相同。如果您进行这些更改，您的构建版本将如下所示：


``` stylus
- git:
    mode = git
    user_git_repo = /home/lmr/Code/qemu-kvm
    extra_configure_options = --disable-werror
    user_branch = master
    user_lbranch = master
    test_git_repo = /home/lmr/Code/kvm-unit-tests
```



 7. 现在你可以像往常一样运行Avocado-VT，你只需要改变主控制文件（called  *control*  with the unittest one  *control.unittests* ）


``` stylus
$ $HOME/Code/autotest/client/bin/autotest $HOME/Code/autotest/client/tests/kvm/control.unittests
```

 8. 典型的单元测试执行的输出看起来像。 请注意，自动测试通知您每个单元测试的日志所在的位置，因此您也可以检查它。


``` stylus
07/14 18:49:44 INFO |  unittest:0052| Running apic
07/14 18:49:44 INFO |    kvm_vm:0782| Running qemu command:
/usr/local/autotest/tests/kvm/qemu -name 'vm1' -nodefaults -vga std -monitor unix:'/tmp/monitor-humanmonitor1-20110714-184944-6ms0',server,nowait -qmp unix:'/tmp/monitor-qmpmonitor1-20110714-184944-6ms0',server,nowait -serial unix:'/tmp/serial-20110714-184944-6ms0',server,nowait -m 512 -smp 2 -kernel '/usr/local/autotest/tests/kvm/unittests/apic.flat' -vnc :0 -chardev file,id=testlog,path=/tmp/testlog-20110714-184944-6ms0 -device testdev,chardev=testlog  -S -cpu qemu64,+x2apic
07/14 18:49:46 INFO |  unittest:0096| Waiting for unittest apic to complete, timeout 600, output in /tmp/testlog-20110714-184944-6ms0
07/14 18:59:46 ERROR|  unittest:0108| Exception happened during apic: Timeout elapsed (600s)
07/14 18:59:46 INFO |  unittest:0113| Unit test log collected and available under /usr/local/autotest/results/default/kvm.qemu-kvm-git.unittests/debug/apic.log
07/14 18:59:46 INFO |  unittest:0052| Running smptest
07/14 19:00:15 INFO |   aexpect:0783| (qemu) (Process terminated with status 0)
07/14 19:00:16 INFO |    kvm_vm:0782| Running qemu command:
/usr/local/autotest/tests/kvm/qemu -name 'vm1' -nodefaults -vga std -monitor unix:'/tmp/monitor-humanmonitor1-20110714-184944-6ms0',server,nowait -qmp unix:'/tmp/monitor-qmpmonitor1-20110714-184944-6ms0',server,nowait -serial unix:'/tmp/serial-20110714-184944-6ms0',server,nowait -m 512 -smp 2 -kernel '/usr/local/autotest/tests/kvm/unittests/smptest.flat' -vnc :0 -chardev file,id=testlog,path=/tmp/testlog-20110714-184944-6ms0 -device testdev,chardev=testlog  -S
07/14 19:00:17 INFO |  unittest:0096| Waiting for unittest smptest to complete, timeout 600, output in /tmp/testlog-20110714-184944-6ms0
07/14 19:00:17 INFO |   aexpect:0783| (qemu) (Process terminated with status 0)
07/14 19:00:18 INFO |  unittest:0113| Unit test log collected and available under /usr/local/autotest/results/default/kvm.qemu-kvm-git.unittests/debug/smptest.log
07/14 19:00:18 INFO |  unittest:0052| Running smptest3
07/14 19:00:18 INFO |    kvm_vm:0782| Running qemu command:
/usr/local/autotest/tests/kvm/qemu -name 'vm1' -nodefaults -vga std -monitor unix:'/tmp/monitor-humanmonitor1-20110714-184944-6ms0',server,nowait -qmp unix:'/tmp/monitor-qmpmonitor1-20110714-184944-6ms0',server,nowait -serial unix:'/tmp/serial-20110714-184944-6ms0',server,nowait -m 512 -smp 3 -kernel '/usr/local/autotest/tests/kvm/unittests/smptest.flat' -vnc :0 -chardev file,id=testlog,path=/tmp/testlog-20110714-184944-6ms0 -device testdev,chardev=testlog  -S
07/14 19:00:19 INFO |  unittest:0096| Waiting for unittest smptest3 to complete, timeout 600, output in /tmp/testlog-20110714-184944-6ms0
07/14 19:00:19 INFO |   aexpect:0783| (qemu) (Process terminated with status 0)
07/14 19:00:20 INFO |  unittest:0113| Unit test log collected and available under /usr/local/autotest/results/default/kvm.qemu-kvm-git.unittests/debug/smptest3.log
07/14 19:00:20 INFO |  unittest:0052| Running vmexit
07/14 19:00:20 INFO |    kvm_vm:0782| Running qemu command:
/usr/local/autotest/tests/kvm/qemu -name 'vm1' -nodefaults -vga std -monitor unix:'/tmp/monitor-humanmonitor1-20110714-184944-6ms0',server,nowait -qmp unix:'/tmp/monitor-qmpmonitor1-20110714-184944-6ms0',server,nowait -serial unix:'/tmp/serial-20110714-184944-6ms0',server,nowait -m 512 -smp 2 -kernel '/usr/local/autotest/tests/kvm/unittests/vmexit.flat' -vnc :0 -chardev file,id=testlog,path=/tmp/testlog-20110714-184944-6ms0 -device testdev,chardev=testlog  -S
07/14 19:00:21 INFO |  unittest:0096| Waiting for unittest vmexit to complete, timeout 600, output in /tmp/testlog-20110714-184944-6ms0
07/14 19:00:31 INFO |   aexpect:0783| (qemu) (Process terminated with status 0)
07/14 19:00:31 INFO |  unittest:0113| Unit test log collected and available under /usr/local/autotest/results/default/kvm.qemu-kvm-git.unittests/debug/vmexit.log
07/14 19:00:31 INFO |  unittest:0052| Running access
07/14 19:00:31 INFO |    kvm_vm:0782| Running qemu command:
/usr/local/autotest/tests/kvm/qemu -name 'vm1' -nodefaults -vga std -monitor unix:'/tmp/monitor-humanmonitor1-20110714-184944-6ms0',server,nowait -qmp unix:'/tmp/monitor-qmpmonitor1-20110714-184944-6ms0',server,nowait -serial unix:'/tmp/serial-20110714-184944-6ms0',server,nowait -m 512 -smp 2 -kernel '/usr/local/autotest/tests/kvm/unittests/access.flat' -vnc :0 -chardev file,id=testlog,path=/tmp/testlog-20110714-184944-6ms0 -device testdev,chardev=testlog  -S
07/14 19:00:32 INFO |  unittest:0096| Waiting for unittest access to complete, timeout 600, output in /tmp/testlog-20110714-184944-6ms0
07/14 19:01:02 INFO |   aexpect:0783| (qemu) (Process terminated with status 0)
07/14 19:01:03 INFO |  unittest:0113| Unit test log collected and available under /usr/local/autotest/results/default/kvm.qemu-kvm-git.unittests/debug/access.log
07/14 19:01:03 INFO |  unittest:0052| Running emulator
07/14 19:01:03 INFO |    kvm_vm:0782| Running qemu command:
/usr/local/autotest/tests/kvm/qemu -name 'vm1' -nodefaults -vga std -monitor unix:'/tmp/monitor-humanmonitor1-20110714-184944-6ms0',server,nowait -qmp unix:'/tmp/monitor-qmpmonitor1-20110714-184944-6ms0',server,nowait -serial unix:'/tmp/serial-20110714-184944-6ms0',server,nowait -m 512 -smp 2 -kernel '/usr/local/autotest/tests/kvm/unittests/emulator.flat' -vnc :0 -chardev file,id=testlog,path=/tmp/testlog-20110714-184944-6ms0 -device testdev,chardev=testlog  -S
07/14 19:01:05 INFO |  unittest:0096| Waiting for unittest emulator to complete, timeout 600, output in /tmp/testlog-20110714-184944-6ms0
07/14 19:01:06 INFO |   aexpect:0783| (qemu) (Process terminated with status 0)
07/14 19:01:07 INFO |  unittest:0113| Unit test log collected and available under /usr/local/autotest/results/default/kvm.qemu-kvm-git.unittests/debug/emulator.log
07/14 19:01:07 INFO |  unittest:0052| Running hypercall
07/14 19:01:07 INFO |    kvm_vm:0782| Running qemu command:
/usr/local/autotest/tests/kvm/qemu -name 'vm1' -nodefaults -vga std -monitor unix:'/tmp/monitor-humanmonitor1-20110714-184944-6ms0',server,nowait -qmp unix:'/tmp/monitor-qmpmonitor1-20110714-184944-6ms0',server,nowait -serial unix:'/tmp/serial-20110714-184944-6ms0',server,nowait -m 512 -smp 2 -kernel '/usr/local/autotest/tests/kvm/unittests/hypercall.flat' -vnc :0 -chardev file,id=testlog,path=/tmp/testlog-20110714-184944-6ms0 -device testdev,chardev=testlog  -S
07/14 19:01:08 INFO |  unittest:0096| Waiting for unittest hypercall to complete, timeout 600, output in /tmp/testlog-20110714-184944-6ms0
07/14 19:01:08 INFO |   aexpect:0783| (qemu) (Process terminated with status 0)
07/14 19:01:09 INFO |  unittest:0113| Unit test log collected and available under /usr/local/autotest/results/default/kvm.qemu-kvm-git.unittests/debug/hypercall.log
07/14 19:01:09 INFO |  unittest:0052| Running idt_test
07/14 19:01:09 INFO |    kvm_vm:0782| Running qemu command:
/usr/local/autotest/tests/kvm/qemu -name 'vm1' -nodefaults -vga std -monitor unix:'/tmp/monitor-humanmonitor1-20110714-184944-6ms0',server,nowait -qmp unix:'/tmp/monitor-qmpmonitor1-20110714-184944-6ms0',server,nowait -serial unix:'/tmp/serial-20110714-184944-6ms0',server,nowait -m 512 -smp 2 -kernel '/usr/local/autotest/tests/kvm/unittests/idt_test.flat' -vnc :0 -chardev file,id=testlog,path=/tmp/testlog-20110714-184944-6ms0 -device testdev,chardev=testlog  -S
07/14 19:01:10 INFO |  unittest:0096| Waiting for unittest idt_test to complete, timeout 600, output in /tmp/testlog-20110714-184944-6ms0
07/14 19:01:10 INFO |   aexpect:0783| (qemu) (Process terminated with status 0)
07/14 19:01:11 INFO |  unittest:0113| Unit test log collected and available under /usr/local/autotest/results/default/kvm.qemu-kvm-git.unittests/debug/idt_test.log
07/14 19:01:11 INFO |  unittest:0052| Running msr
07/14 19:01:11 INFO |    kvm_vm:0782| Running qemu command:
/usr/local/autotest/tests/kvm/qemu -name 'vm1' -nodefaults -vga std -monitor unix:'/tmp/monitor-humanmonitor1-20110714-184944-6ms0',server,nowait -qmp unix:'/tmp/monitor-qmpmonitor1-20110714-184944-6ms0',server,nowait -serial unix:'/tmp/serial-20110714-184944-6ms0',server,nowait -m 512 -smp 2 -kernel '/usr/local/autotest/tests/kvm/unittests/msr.flat' -vnc :0 -chardev file,id=testlog,path=/tmp/testlog-20110714-184944-6ms0 -device testdev,chardev=testlog  -S
07/14 19:01:12 INFO |  unittest:0096| Waiting for unittest msr to complete, timeout 600, output in /tmp/testlog-20110714-184944-6ms0
07/14 19:01:13 INFO |   aexpect:0783| (qemu) (Process terminated with status 0)
07/14 19:01:13 INFO |  unittest:0113| Unit test log collected and available under /usr/local/autotest/results/default/kvm.qemu-kvm-git.unittests/debug/msr.log
07/14 19:01:13 INFO |  unittest:0052| Running port80
07/14 19:01:13 INFO |    kvm_vm:0782| Running qemu command:
/usr/local/autotest/tests/kvm/qemu -name 'vm1' -nodefaults -vga std -monitor unix:'/tmp/monitor-humanmonitor1-20110714-184944-6ms0',server,nowait -qmp unix:'/tmp/monitor-qmpmonitor1-20110714-184944-6ms0',server,nowait -serial unix:'/tmp/serial-20110714-184944-6ms0',server,nowait -m 512 -smp 2 -kernel '/usr/local/autotest/tests/kvm/unittests/port80.flat' -vnc :0 -chardev file,id=testlog,path=/tmp/testlog-20110714-184944-6ms0 -device testdev,chardev=testlog  -S
07/14 19:01:14 INFO |  unittest:0096| Waiting for unittest port80 to complete, timeout 600, output in /tmp/testlog-20110714-184944-6ms0
07/14 19:01:31 INFO |   aexpect:0783| (qemu) (Process terminated with status 0)
07/14 19:01:32 INFO |  unittest:0113| Unit test log collected and available under /usr/local/autotest/results/default/kvm.qemu-kvm-git.unittests/debug/port80.log
07/14 19:01:32 INFO |  unittest:0052| Running realmode
07/14 19:01:32 INFO |    kvm_vm:0782| Running qemu command:
/usr/local/autotest/tests/kvm/qemu -name 'vm1' -nodefaults -vga std -monitor unix:'/tmp/monitor-humanmonitor1-20110714-184944-6ms0',server,nowait -qmp unix:'/tmp/monitor-qmpmonitor1-20110714-184944-6ms0',server,nowait -serial unix:'/tmp/serial-20110714-184944-6ms0',server,nowait -m 512 -smp 2 -kernel '/usr/local/autotest/tests/kvm/unittests/realmode.flat' -vnc :0 -chardev file,id=testlog,path=/tmp/testlog-20110714-184944-6ms0 -device testdev,chardev=testlog  -S
07/14 19:01:33 INFO |  unittest:0096| Waiting for unittest realmode to complete, timeout 600, output in /tmp/testlog-20110714-184944-6ms0
07/14 19:01:33 INFO |   aexpect:0783| (qemu) (Process terminated with status 0)
07/14 19:01:34 INFO |  unittest:0113| Unit test log collected and available under /usr/local/autotest/results/default/kvm.qemu-kvm-git.unittests/debug/realmode.log
07/14 19:01:34 INFO |  unittest:0052| Running sieve
07/14 19:01:34 INFO |    kvm_vm:0782| Running qemu command:
/usr/local/autotest/tests/kvm/qemu -name 'vm1' -nodefaults -vga std -monitor unix:'/tmp/monitor-humanmonitor1-20110714-184944-6ms0',server,nowait -qmp unix:'/tmp/monitor-qmpmonitor1-20110714-184944-6ms0',server,nowait -serial unix:'/tmp/serial-20110714-184944-6ms0',server,nowait -m 512 -smp 2 -kernel '/usr/local/autotest/tests/kvm/unittests/sieve.flat' -vnc :0 -chardev file,id=testlog,path=/tmp/testlog-20110714-184944-6ms0 -device testdev,chardev=testlog  -S
07/14 19:01:35 INFO |  unittest:0096| Waiting for unittest sieve to complete, timeout 600, output in /tmp/testlog-20110714-184944-6ms0
07/14 19:02:05 INFO |   aexpect:0783| (qemu) (Process terminated with status 0)
07/14 19:02:05 INFO |  unittest:0113| Unit test log collected and available under /usr/local/autotest/results/default/kvm.qemu-kvm-git.unittests/debug/sieve.log
07/14 19:02:05 INFO |  unittest:0052| Running tsc
07/14 19:02:05 INFO |    kvm_vm:0782| Running qemu command:
/usr/local/autotest/tests/kvm/qemu -name 'vm1' -nodefaults -vga std -monitor unix:'/tmp/monitor-humanmonitor1-20110714-184944-6ms0',server,nowait -qmp unix:'/tmp/monitor-qmpmonitor1-20110714-184944-6ms0',server,nowait -serial unix:'/tmp/serial-20110714-184944-6ms0',server,nowait -m 512 -smp 2 -kernel '/usr/local/autotest/tests/kvm/unittests/tsc.flat' -vnc :0 -chardev file,id=testlog,path=/tmp/testlog-20110714-184944-6ms0 -device testdev,chardev=testlog  -S
07/14 19:02:06 INFO |  unittest:0096| Waiting for unittest tsc to complete, timeout 600, output in /tmp/testlog-20110714-184944-6ms0
07/14 19:02:06 INFO |   aexpect:0783| (qemu) (Process terminated with status 0)
07/14 19:02:07 INFO |  unittest:0113| Unit test log collected and available under /usr/local/autotest/results/default/kvm.qemu-kvm-git.unittests/debug/tsc.log
07/14 19:02:07 INFO |  unittest:0052| Running xsave
07/14 19:02:07 INFO |    kvm_vm:0782| Running qemu command:
/usr/local/autotest/tests/kvm/qemu -name 'vm1' -nodefaults -vga std -monitor unix:'/tmp/monitor-humanmonitor1-20110714-184944-6ms0',server,nowait -qmp unix:'/tmp/monitor-qmpmonitor1-20110714-184944-6ms0',server,nowait -serial unix:'/tmp/serial-20110714-184944-6ms0',server,nowait -m 512 -smp 2 -kernel '/usr/local/autotest/tests/kvm/unittests/xsave.flat' -vnc :0 -chardev file,id=testlog,path=/tmp/testlog-20110714-184944-6ms0 -device testdev,chardev=testlog  -S
07/14 19:02:08 INFO |  unittest:0096| Waiting for unittest xsave to complete, timeout 600, output in /tmp/testlog-20110714-184944-6ms0
07/14 19:02:09 INFO |   aexpect:0783| (qemu) (Process terminated with status 0)
07/14 19:02:09 INFO |  unittest:0113| Unit test log collected and available under /usr/local/autotest/results/default/kvm.qemu-kvm-git.unittests/debug/xsave.log
07/14 19:02:09 INFO |  unittest:0052| Running rmap_chain
07/14 19:02:09 INFO |    kvm_vm:0782| Running qemu command:
/usr/local/autotest/tests/kvm/qemu -name 'vm1' -nodefaults -vga std -monitor unix:'/tmp/monitor-humanmonitor1-20110714-184944-6ms0',server,nowait -qmp unix:'/tmp/monitor-qmpmonitor1-20110714-184944-6ms0',server,nowait -serial unix:'/tmp/serial-20110714-184944-6ms0',server,nowait -m 512 -smp 2 -kernel '/usr/local/autotest/tests/kvm/unittests/rmap_chain.flat' -vnc :0 -chardev file,id=testlog,path=/tmp/testlog-20110714-184944-6ms0 -device testdev,chardev=testlog  -S
07/14 19:02:11 INFO |  unittest:0096| Waiting for unittest rmap_chain to complete, timeout 600, output in /tmp/testlog-20110714-184944-6ms0
07/14 19:02:12 INFO |   aexpect:0783| (qemu) (Process terminated with status 0)
07/14 19:02:13 INFO |  unittest:0113| Unit test log collected and available under /usr/local/autotest/results/default/kvm.qemu-kvm-git.unittests/debug/rmap_chain.log
07/14 19:02:13 INFO |  unittest:0052| Running svm
07/14 19:02:13 INFO |    kvm_vm:0782| Running qemu command:
/usr/local/autotest/tests/kvm/qemu -name 'vm1' -nodefaults -vga std -monitor unix:'/tmp/monitor-humanmonitor1-20110714-184944-6ms0',server,nowait -qmp unix:'/tmp/monitor-qmpmonitor1-20110714-184944-6ms0',server,nowait -serial unix:'/tmp/serial-20110714-184944-6ms0',server,nowait -m 512 -smp 2 -kernel '/usr/local/autotest/tests/kvm/unittests/svm.flat' -vnc :0 -chardev file,id=testlog,path=/tmp/testlog-20110714-184944-6ms0 -device testdev,chardev=testlog  -S -enable-nesting -cpu qemu64,+svm
07/14 19:02:13 INFO |   aexpect:0783| (qemu) qemu: -enable-nesting: invalid option
07/14 19:02:13 INFO |   aexpect:0783| (qemu) (Process terminated with status 1)
07/14 19:02:13 ERROR|  unittest:0108| Exception happened during svm: VM creation command failed:    "/usr/local/autotest/tests/kvm/qemu -name 'vm1' -nodefaults -vga std -monitor unix:'/tmp/monitor-humanmonitor1-20110714-184944-6ms0',server,nowait -qmp unix:'/tmp/monitor-qmpmonitor1-20110714-184944-6ms0',server,nowait -serial unix:'/tmp/serial-20110714-184944-6ms0',server,nowait -m 512 -smp 2 -kernel '/usr/local/autotest/tests/kvm/unittests/svm.flat' -vnc :0 -chardev file,id=testlog,path=/tmp/testlog-20110714-184944-6ms0 -device testdev,chardev=testlog  -S -enable-nesting -cpu qemu64,+svm"    (status: 1,    output: 'qemu: -enable-nesting: invalid option\n')
07/14 19:02:13 ERROR|  unittest:0115| Not possible to collect logs
07/14 19:02:13 INFO |  unittest:0052| Running svm-disabled
07/14 19:02:13 INFO |    kvm_vm:0782| Running qemu command:
/usr/local/autotest/tests/kvm/qemu -name 'vm1' -nodefaults -vga std -monitor unix:'/tmp/monitor-humanmonitor1-20110714-184944-6ms0',server,nowait -qmp unix:'/tmp/monitor-qmpmonitor1-20110714-184944-6ms0',server,nowait -serial unix:'/tmp/serial-20110714-184944-6ms0',server,nowait -m 512 -smp 2 -kernel '/usr/local/autotest/tests/kvm/unittests/svm.flat' -vnc :0 -chardev file,id=testlog,path=/tmp/testlog-20110714-184944-6ms0 -device testdev,chardev=testlog  -S -cpu qemu64,-svm
07/14 19:02:14 INFO |  unittest:0096| Waiting for unittest svm-disabled to complete, timeout 600, output in /tmp/testlog-20110714-184944-6ms0
07/14 19:02:15 INFO |   aexpect:0783| (qemu) (Process terminated with status 0)
07/14 19:02:16 INFO |  unittest:0113| Unit test log collected and available under /usr/local/autotest/results/default/kvm.qemu-kvm-git.unittests/debug/svm-disabled.log
07/14 19:02:16 INFO |  unittest:0052| Running kvmclock_test
07/14 19:02:16 INFO |    kvm_vm:0782| Running qemu command:
/usr/local/autotest/tests/kvm/qemu -name 'vm1' -nodefaults -vga std -monitor unix:'/tmp/monitor-humanmonitor1-20110714-184944-6ms0',server,nowait -qmp unix:'/tmp/monitor-qmpmonitor1-20110714-184944-6ms0',server,nowait -serial unix:'/tmp/serial-20110714-184944-6ms0',server,nowait -m 512 -smp 2 -kernel '/usr/local/autotest/tests/kvm/unittests/kvmclock_test.flat' -vnc :0 -chardev file,id=testlog,path=/tmp/testlog-20110714-184944-6ms0 -device testdev,chardev=testlog  -S --append "10000000 `date +%s`"
07/14 19:02:17 INFO |  unittest:0096| Waiting for unittest kvmclock_test to complete, timeout 600, output in /tmp/testlog-20110714-184944-6ms0
07/14 19:02:33 INFO |   aexpect:0783| (qemu) (Process terminated with status 0)
07/14 19:02:34 INFO |  unittest:0113| Unit test log collected and available under /usr/local/autotest/results/default/kvm.qemu-kvm-git.unittests/debug/kvmclock_test.log
07/14 19:02:34 ERROR|       kvm:0094| Test failed: TestFail: Unit tests failed: apic svm
```


你可以看看unittests.cfg配置文件选项做一些你可能喜欢的调整，例如使超时考虑单元测试作为失败时可以较小以及其他事情。




# Parallel Jobs

Avocado-VT附带一个插件，可在已知的公共位置（默认是/tmp,但可以配置）创建锁定文件，以防止包含VT测试的多个作业运行。

原因是，默认情况下，在同一个运行的多个作业可以访问相同的数据文件，并导致损坏。 数据文件的示例是客户机images，通常由测试直接或间接修改。


## Checking Installation

默认情况下安装并注册vt-joblock。 要确保它是活动的，请运行：

``` stylus
$ avocado plugins
```

应列出的 VT job锁插件：

``` stylus
Plugins that run before/after the execution of jobs (avocado.plugins.job.prepost):
...
vt-joblock Avocado-VT Job Lock/Unlock
...
```

## Configuration


vt-joblock插件的配置可以在/etc/avocado/conf.d/vt_joblock.conf找到。配置文件内容示例如下：

``` stylus
[plugins.vtjoblock]
# Directory where the lock file will be located. Avocado should have permission
# to write to this directory.
dir=/tmp
```

配置键dir可让您设置Avocado在运行之前查找现有锁文件的目录，如果尚不存在，则创建一个。


## Running Parallel Jobs


假设在单个机器上有多个用户，使用不同的数据目录，您可以通过为每个用户设置不同的锁目录来允许并行VT作业。

为此，您可以将自定义锁目录添加到用户自己的Avocado配置文件中。 从创建锁目录开始：

``` stylus
[user1@localhost] $ mkdir ~/avocado/data/avocado-vt/lockdir
```

然后修改用户自己的配置，使其指向新创建的锁目录：


``` stylus
[user1@localhost] $ cat >> ~/.config/avocado/avocado.conf <<EOF
[plugins.vtjoblock]
dir=/home/user1/avocado/data/avocado-vt/lockdir
EOF
```


然后验证：

``` stylus
[user1@localhost] $ avocado config | grep plugins.vtjoblock
...
plugins.vtjoblock.dir          /home/user1/avocado/data/avocado-vt/lockdir
...
```

对其他用户做同样的事情，他们的工作不会被彼此锁定。


# Contribution and Community Guide

Contents:

## Contact information

 - [Virt-test-devel mailing list (new mailing list hosted by red hat)][40]
 - [IRC channel: irc.oftc.net #virt-test][41]


## Downloading the Source


主要来源在git上维护，可以克隆如下：

``` stylus
git clone https://github.com/avocado-framework/avocado-vt.git
```

如果你想学习如何使用git作为一个有效的贡献工具，请考虑阅读[git工作流程自动测试文档][42]

## Avocado VT Development Workflow

http://avocado-framework.readthedocs.org/en/latest/ContributionGuide.html


## Contributions Guidelines and Tips


### Code

外加的测试和代码的贡献总是欢迎的。如果有疑问（或）关于处理特定问题的建议，在提交代码之前请联系项目成员（请参见\_collaboration部分），请查看[git repository configuration guidelines][43]。

要提交更改，请按照以下[说明操作][44]。 维护人员最多需要两周时间才能提取并查看您的更改。 虽然，如果你想在任何阶段的帮助，请随时在邮件列表上发表并引用你的pull请求。


### Docs

请直接编辑文档以纠正任何细微的不准确或澄清项目。 首选标记语法是[ReStructuredText][45]，与现有文档中的约定和样式保持一致。 对于任何图形或图表，应使用web-friendly的格式，如PNG或SVG。

避免使用'you'，'we'，'they'，因为它们在参考文档中可能含糊不清。 它在交流和电子邮件工作正常，但在参考资料看起来很奇怪。 同样，避免使用‘unnecessary’,，off-topic或其它语言。 例如在美国英语中， “Rinse and repeat”是一个有趣的短语，但当翻译成其他语言时可能会导致问题。 基本上，尽量避免任何减慢读者从事实发现的事情。

# virttest

 - [virttest package][46]
   - [Subpackages][47]
     - [virttest.libvirt_xml package][48]
       - [Subpackages][49]
       - [Submodules][50]
       - [virttest.libvirt_xml.accessors module][51]
       - [virttest.libvirt_xml.base module][52]
       - [virttest.libvirt_xml.capability_xml module][53]
       - [virttest.libvirt_xml.network_xml module][54]
       - [virttest.libvirt_xml.nodedev_xml module][55]
       - [virttest.libvirt_xml.nwfilter_xml module][56]
       - [virttest.libvirt_xml.pool_xml module][57]
       - [virttest.libvirt_xml.secret_xml module][58]
       - [virttest.libvirt_xml.snapshot_xml module][59]
       - [virttest.libvirt_xml.sysinfo_xml module][60]
       - [virttest.libvirt_xml.vm_xml module][61]
       - [virttest.libvirt_xml.vol_xml module][62]
       - [virttest.libvirt_xml.xcepts module][63]
       - [Module contents][64]
     - [virttest.qemu_devices package][65]
       - [Submodules][66]
       - [virttest.qemu_devices.qbuses module][67]
       - [virttest.qemu_devices.qcontainer module][68]
       - [virttest.qemu_devices.qdevices module][69]
       - [virttest.qemu_devices.utils module][70]
       - [Module contents][71]
     - [virttest.remote_commander package][72]
       - [Submodules][73]
       - [virttest.remote_commander.messenger module][74]
       - [virttest.remote_commander.remote_interface module][75]
       - [virttest.remote_commander.remote_master module][76]
       - [virttest.remote_commander.remote_runner module][77]
       - [Module contents][78]
     - [virttest.staging package][79]
       - [Subpackages][80]
       - [Submodules][81]
       - [virttest.staging.lv_utils module][82]
       - [virttest.staging.service module][83]
       - [virttest.staging.utils_cgroup module][84]
       - [virttest.staging.utils_koji module][85]
       - [virttest.staging.utils_memory module][86]
       - [Module contents][87]
     - [virttest.tests package][88]
       - [Submodules][89]
       - [virttest.tests.unattended_install module][90]
       - [Module contents][91]
     - [virttest.unittest_utils package][92]
       - [Submodules][93]
       - [virttest.unittest_utils.mock module][94]
       - [Module contents][95]
     - [virttest.utils_test package][96]
       - [Subpackages][97]
       - [Submodules][98]
       - [virttest.utils_test.libguestfs module][99]
       - [virttest.utils_test.libvirt module][100]
       - [Module contents][101]
   - [Submodules][102]
   - [virttest.RFBDes module][103]
   - [virttest.arch module][104]
   - [virttest.asset module][105]
   - [virttest.base_installer module][106]
   - [virttest.bootstrap module][107]
   - [virttest.build_helper module][108]
   - [virttest.cartesian_config module][109]
   - [virttest.ceph module][110]
   - [virttest.data_dir module][111]
   - [virttest.defaults module][112]
   - [virttest.element_path module][113]
   - [virttest.element_tree module][114]
   - [virttest.env_process module][115]
   - [virttest.error_context module][116]
   - [virttest.funcatexit module][117]
   - [virttest.gluster module][118]
   - [virttest.guest_agent module][119]
   - [virttest.http_server module][120]
   - [virttest.installer module][121]
   - [virttest.iscsi module][122]
   - [virttest.libvirt_storage module][123]
   - [virttest.libvirt_vm module][124]
   - [virttest.lvm module][125]
   - [virttest.lvsb module][126]
   - [virttest.lvsb_base module][127]
   - [virttest.lvsbs module][128]
   - [virttest.nfs module][129]
   - [virttest.openvswitch module][130]
   - [virttest.ovirt module][131]
   - [virttest.ovs_utils module][132]
   - [virttest.passfd module][133]
   - [virttest.passfd_setup module][134]
   - [virttest.postprocess_iozone module][135]
   - [virttest.ppm_utils module][136]
   - [virttest.propcan module][137]
   - [virttest.qemu_installer module][138]
   - [virttest.qemu_io module][139]
   - [virttest.qemu_monitor module][140]
   - [virttest.qemu_qtree module][141]
   - [virttest.qemu_storage module][142]
   - [virttest.qemu_virtio_port module][143]
   - [virttest.qemu_vm module][144]
   - [virttest.remote module][145]
   - [virttest.remote_build module][146]
   - [virttest.rss_client module][147]
   - [virttest.scan_autotest_results module][148]
   - [virttest.scheduler module][149]
   - [virttest.ssh_key module][150]
   - [virttest.standalone_test module][151]
   - [virttest.step_editor module][152]
   - [virttest.storage module][153]
   - [virttest.syslog_server module][154]
   - [virttest.test_setup module][155]
   - [virttest.utils_config module][156]
   - [virttest.utils_conn module][157]
   - [virttest.utils_disk module][158]
   - [virttest.utils_env module][159]
   - [virttest.utils_gdb module][160]
   - [virttest.utils_libguestfs module][161]
   - [virttest.utils_libvirtd module][162]
   - [virttest.utils_misc module][163]
   - [virttest.utils_net module][164]
   - [virttest.utils_netperf module][165]
   - [virttest.utils_params module][166]
   - [virttest.utils_sasl module][167]
   - [virttest.utils_selinux module][168]
   - [virttest.utils_spice module][169]
   - [virttest.utils_v2v module][170]
   - [virttest.utils_virtio_port module][171]
   - [virttest.version module][172]
   - [virttest.versionable_class module][173]
   - [virttest.video_maker module][174]
   - [virttest.virsh module][175]
   - [virttest.virt_vm module][176]
   - [virttest.xml_utils module][177]
   - [virttest.yumrepo module][178]
   - [Module contents][179]


# Cartesian Config

笛卡尔配置格式的参考文档。

内容：

 - [Parameters][180]
   - [Addressing objects (VMs, images, NICs etc)][181]
   - [Parameters used by the test dispatching system][182]
   - [Parameters used by the preprocessor][183]
   - [Parameters used by the postprocessor][184]
   - [Test parameters][185]
   - [Real world example][186]
 - [Cartesian Config Reference][187]
   - [bridge][188]
   - [cdroms][189]
   - [cd_format][190]
   - [check_image][191]
   - [convert_ppm_files_to_png][192]
   - [create_image][193]
   - [display][194]
   - [drive_cache][195]
   - [drive_format][196]
   - [drive_index][197]
   - [drive_werror][198]
   - [drive_serial][199]
   - [file_transfer_client][200]
   - [file_transfer_port][201]
   - [force_create_image][202]
   - [guest_port][203]
   - [guest_port_remote_shell][204]
   - [guest_port_file_tranfer][205]
   - [guest_port_unattended_install][206]
   - [images][207]
   - [images_good][208]
   - [image_format][209]
   - [image_name][210]
   - [image_raw_device][211]
   - [image_size][212]
   - [keep_ppm_files][213]
   - [keep_screendumps][214]
   - [kill_unresponsive_vms][215]
   - [kill_vm][216]
   - [kill_vm_gracefully][217]
   - [kill_vm_timeout][218]
   - [login_timeout][219]
   - [main_monitor][220]
   - [main_vm][221]
   - [mem][222]
   - [migration_mode][223]
   - [monitors][224]
   - [monitor_type][225]
   - [nic_mode][226]
   - [nics][227]
   - [pre_command][228]
   - [pre_command_timeout][229]
   - [pre_command_noncritical][230]
   - [profilers][231]
   - [post_command][232]
   - [post_command_timeout][233]
   - [post_command_noncritical][234]
   - [qemu_binary][235]
   - [qemu_img_binary][236]
   - [qxl_dev_nr][237]
   - [qxl][238]
   - [redirs][239]
   - [remove_image][240]
   - [spice][241]




  [1]: http://avocado-framework.readthedocs.io/en/latest/GetStartedGuide.html#installing-avocado
  [2]: http://avocado-framework.readthedocs.io/en/latest/ContributionGuide.html#hacking-and-using-avocado
  [3]: https://www.redhat.com/mailman/listinfo/avocado-devel
  [4]: https://github.com/avocado-framework/avocado-vt/issues/new
  [5]: irc://irc.oftc.net/#avocado
  [6]: http://avocado-vt.readthedocs.io/en/latest/WritingTests/TestProviders.html
  [7]: http://avocado-vt.readthedocs.io/en/latest/WritingTests/TestProviders.html#test-provider-layout
  [8]: http://avocado-vt.readthedocs.io/en/latest/WritingTests/TestProviders.html#types-of-test-providers
  [9]: http://avocado-vt.readthedocs.io/en/latest/WritingTests/TestProviders.html#test-provider-definition-file
  [10]: http://avocado-vt.readthedocs.io/en/latest/WritingTests/DevelEnvSetup.html
  [11]: http://avocado-vt.readthedocs.io/en/latest/WritingTests/WritingSimpleTests.html
  [12]: http://avocado-vt.readthedocs.io/en/latest/WritingTests/WritingSimpleTests.html#write-our-own-uptime-test-step-by-step-procedure
  [13]: http://avocado-vt.readthedocs.io/en/latest/WritingTests/DefiningNewGuests.html
  [14]: http://avocado-vt.readthedocs.io/en/latest/WritingTests/DefiningNewGuests.html#linux-based-custom-guest
  [15]: http://avocado-vt.readthedocs.io/en/latest/WritingTests/DefiningNewGuests.html#windows-based-custom-guest
  [16]: http://avocado-vt.readthedocs.io/en/latest/CartesianConfig.html#variants
  [17]: http://avocado-vt.readthedocs.io/en/latest/CartesianConfig.html#key-sub-arrays
  [18]: http://avocado-vt.readthedocs.io/en/latest/GetStartedGuide.html#run-bootstrap
  [19]: http://avocado-vt.readthedocs.io/en/latest/GetStartedGuide.html
  [20]: https://github.com/autotest/autotest/tree/master/client/virt/tests/netperf.py
  [21]: https://github.com/autotest/autotest/tree/master/client/virt/tests/ntttcp.py
  [22]: https://github.com/autotest/autotest/tree/master/client/tests/iozone/
  [23]: https://github.com/autotest/autotest/tree/master/client/virt/tests/iozone_windows.py
  [24]: https://github.com/autotest/autotest/tree/master/client/tests/ffsb/
  [25]: https://github.com/autotest/autotest-client-tests/tree/master/qemu_iotests
  [26]: https://github.com/autotest/autotest-client-tests/tree/master/fio
  [27]: https://github.com/autotest/virt-test/blob/master/shared/scripts/rh_perf_envsetup.sh
  [28]: https://i-kvm.rhcloud.com/static/pub/netperf.exhost_guest.html
  [29]: http://i-kvm.rhcloud.com/static/pub/fio.html
  [30]: https://github.com/kongove/misc/blob/master/html/netperf.avg.html
  [31]: ./images/nested-virt.png "nested-virt.png"
  [32]: ./images/multihost-migration.png "multihost-migration.png"
  [33]: http://avocado-vt.readthedocs.io/en/latest/_downloads/multihost-migration.odg
  [34]: http://avocado-vt.readthedocs.io/en/latest/contributing/DownloadSource.html
  [35]: http://avocado-vt.readthedocs.io/en/latest/RunQemuUnittests.html#run-kvm-unit-tests-in-avocado
  [36]: http://avocado-vt.readthedocs.io/en/latest/RunQemuUnittests.html#run-kvm-unit-tests-in-avocado-vt
  [37]: http://avocado-vt.readthedocs.io/en/latest/InstallOptionalPackages.html
  [38]: git%20clone%20https://github.com/avocado-framework/avocado-vt.git
  [39]: http://avocado-vt.readthedocs.io/en/latest/GetStartedGuide.html
  [40]: http://www.redhat.com/mailman/listinfo/virt-test-devel
  [41]: irc://irc.oftc.net/#virt-test
  [42]: https://github.com/autotest/autotest/wiki/GitWorkflow
  [43]: http://github.com/autotest/autotest/wiki/GitWorkflow
  [44]: https://github.com/autotest/autotest/wiki/SubmissionChecklist
  [45]: http://en.wikipedia.org/wiki/ReStructuredText
  [46]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html
  [47]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#subpackages
  [48]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.libvirt_xml.html
  [49]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.libvirt_xml.html#subpackages
  [50]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.libvirt_xml.html#submodules
  [51]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.libvirt_xml.html#virttest-libvirt-xml-accessors-module
  [52]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.libvirt_xml.html#virttest-libvirt-xml-base-module
  [53]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.libvirt_xml.html#virttest-libvirt-xml-capability-xml-module
  [54]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.libvirt_xml.html#virttest-libvirt-xml-network-xml-module
  [55]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.libvirt_xml.html#virttest-libvirt-xml-nodedev-xml-module
  [56]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.libvirt_xml.html#virttest-libvirt-xml-nwfilter-xml-module
  [57]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.libvirt_xml.html#virttest-libvirt-xml-pool-xml-module
  [58]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.libvirt_xml.html#virttest-libvirt-xml-secret-xml-module
  [59]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.libvirt_xml.html#virttest-libvirt-xml-snapshot-xml-module
  [60]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.libvirt_xml.html#virttest-libvirt-xml-sysinfo-xml-module
  [61]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.libvirt_xml.html#virttest-libvirt-xml-vm-xml-module
  [62]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.libvirt_xml.html#virttest-libvirt-xml-vol-xml-module
  [63]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.libvirt_xml.html#virttest-libvirt-xml-xcepts-module
  [64]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.libvirt_xml.html#module-contents
  [65]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.qemu_devices.html
  [66]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.qemu_devices.html#submodules
  [67]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.qemu_devices.html#virttest-qemu-devices-qbuses-module
  [68]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.qemu_devices.html#virttest-qemu-devices-qcontainer-module
  [69]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.qemu_devices.html#virttest-qemu-devices-qdevices-module
  [70]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.qemu_devices.html#module-virttest.qemu_devices.utils
  [71]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.qemu_devices.html#module-virttest.qemu_devices
  [72]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.remote_commander.html
  [73]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.remote_commander.html#submodules
  [74]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.remote_commander.html#module-virttest.remote_commander.messenger
  [75]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.remote_commander.html#module-virttest.remote_commander.remote_interface
  [76]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.remote_commander.html#module-virttest.remote_commander.remote_master
  [77]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.remote_commander.html#module-virttest.remote_commander.remote_runner
  [78]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.remote_commander.html#module-virttest.remote_commander
  [79]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.staging.html
  [80]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.staging.html#subpackages
  [81]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.staging.html#submodules
  [82]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.staging.html#virttest-staging-lv-utils-module
  [83]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.staging.html#virttest-staging-service-module
  [84]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.staging.html#virttest-staging-utils-cgroup-module
  [85]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.staging.html#virttest-staging-utils-koji-module
  [86]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.staging.html#virttest-staging-utils-memory-module
  [87]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.staging.html#module-contents
  [88]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.tests.html
  [89]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.tests.html#submodules
  [90]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.tests.html#virttest-tests-unattended-install-module
  [91]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.tests.html#module-contents
  [92]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.unittest_utils.html
  [93]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.unittest_utils.html#submodules
  [94]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.unittest_utils.html#module-virttest.unittest_utils.mock
  [95]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.unittest_utils.html#module-virttest.unittest_utils
  [96]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.utils_test.html
  [97]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.utils_test.html#subpackages
  [98]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.utils_test.html#submodules
  [99]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.utils_test.html#virttest-utils-test-libguestfs-module
  [100]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.utils_test.html#virttest-utils-test-libvirt-module
  [101]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.utils_test.html#module-contents
  [102]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#submodules
  [103]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-rfbdes-module
  [104]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-arch-module
  [105]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-asset-module
  [106]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-base-installer-module
  [107]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-bootstrap-module
  [108]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-build-helper-module
  [109]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#module-virttest.cartesian_config
  [110]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-ceph-module
  [111]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-data-dir-module
  [112]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#module-virttest.defaults
  [113]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#module-virttest.element_path
  [114]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#module-virttest.element_tree
  [115]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-env-process-module
  [116]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#module-virttest.error_context
  [117]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-funcatexit-module
  [118]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-gluster-module
  [119]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-guest-agent-module
  [120]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#module-virttest.http_server
  [121]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-installer-module
  [122]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-iscsi-module
  [123]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-libvirt-storage-module
  [124]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-libvirt-vm-module
  [125]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-lvm-module
  [126]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#module-virttest.lvsb
  [127]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#module-virttest.lvsb_base
  [128]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-lvsbs-module
  [129]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-nfs-module
  [130]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-openvswitch-module
  [131]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-ovirt-module
  [132]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-ovs-utils-module
  [133]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-passfd-module
  [134]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-passfd-setup-module
  [135]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-postprocess-iozone-module
  [136]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#module-virttest.ppm_utils
  [137]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#module-virttest.propcan
  [138]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-qemu-installer-module
  [139]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-qemu-io-module
  [140]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-qemu-monitor-module
  [141]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-qemu-qtree-module
  [142]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-qemu-storage-module
  [143]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-qemu-virtio-port-module
  [144]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-qemu-vm-module
  [145]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-remote-module
  [146]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-remote-build-module
  [147]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#module-virttest.rss_client
  [148]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#module-virttest.scan_autotest_results
  [149]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-scheduler-module
  [150]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-ssh-key-module
  [151]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-standalone-test-module
  [152]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-step-editor-module
  [153]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-storage-module
  [154]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#module-virttest.syslog_server
  [155]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-test-setup-module
  [156]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#module-virttest.utils_config
  [157]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-utils-conn-module
  [158]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-utils-disk-module
  [159]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-utils-env-module
  [160]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-utils-gdb-module
  [161]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-utils-libguestfs-module
  [162]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-utils-libvirtd-module
  [163]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-utils-misc-module
  [164]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-utils-net-module
  [165]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-utils-netperf-module
  [166]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-utils-params-module
  [167]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-utils-sasl-module
  [168]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-utils-selinux-module
  [169]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-utils-spice-module
  [170]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-utils-v2v-module
  [171]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-utils-virtio-port-module
  [172]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-version-module
  [173]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#module-virttest.versionable_class
  [174]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#module-virttest.video_maker
  [175]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-virsh-module
  [176]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#virttest-virt-vm-module
  [177]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#module-virttest.xml_utils
  [178]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#module-virttest.yumrepo
  [179]: http://avocado-vt.readthedocs.io/en/latest/api/virttest.html#module-virttest
  [180]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigParametersIntro.html
  [181]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigParametersIntro.html#addressing-objects-vms-images-nics-etc
  [182]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigParametersIntro.html#parameters-used-by-the-test-dispatching-system
  [183]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigParametersIntro.html#parameters-used-by-the-preprocessor
  [184]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigParametersIntro.html#parameters-used-by-the-postprocessor
  [185]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigParametersIntro.html#test-parameters
  [186]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigParametersIntro.html#real-world-example
  [187]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference.html
  [188]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-bridge.html
  [189]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-cdroms.html
  [190]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-cd_format.html
  [191]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-check_image.html
  [192]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-convert_ppm_files_to_png.html
  [193]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-create_image.html
  [194]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-display.html
  [195]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-drive_cache.html
  [196]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-drive_format.html
  [197]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-drive_index.html
  [198]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-drive_werror.html
  [199]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-drive_serial.html
  [200]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-file_transfer_client.html
  [201]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-file_transfer_port.html
  [202]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-force_create_image.html
  [203]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-guest_port.html
  [204]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-guest_port_remote_shell.html
  [205]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-guest_port_file_transfer.html
  [206]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-guest_port_unattended_install.html
  [207]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-images.html
  [208]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-images_good.html
  [209]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-image_format.html
  [210]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-image_name.html
  [211]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-image_raw_device.html
  [212]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-image_size.html
  [213]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-keep_ppm_files.html
  [214]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-keep_screendumps.html
  [215]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-kill_unresponsive_vms.html
  [216]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-kill_vm.html
  [217]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-kill_vm_gracefully.html
  [218]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-kill_vm_timeout.html
  [219]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-login_timeout.html
  [220]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-main_monitor.html
  [221]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-main_vm.html
  [222]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-mem.html
  [223]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-migration_mode.html
  [224]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-monitors.html
  [225]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-monitor_type.html
  [226]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-nic_mode.html
  [227]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-nics.html
  [228]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-pre_command.html
  [229]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-pre_command_timeout.html
  [230]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-pre_command_noncritical.html
  [231]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-profilers.html
  [232]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-post_command.html
  [233]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-post_command_timeout.html
  [234]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-post_command_noncritical.html
  [235]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-qemu_binary.html
  [236]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-qemu_img_binary.html
  [237]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-qxl_dev_nr.html
  [238]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-qxl.html
  [239]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-redirs.html
  [240]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-remove_image.html
  [241]: http://avocado-vt.readthedocs.io/en/latest/cartesian/CartesianConfigReference-KVM-spice.html