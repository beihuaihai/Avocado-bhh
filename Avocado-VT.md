# About Avocado-VT
Avocado-VT是一个兼容的插件，可以让你执行虚拟化相关测试（正如所知的virt-test），Avocado为其提供了所有的便捷。

它的主要用途是给virt开发者提供一个自动化的回归分析测试服务，去做virt技术（在服务器测试基础设施上使用它）的定期的自动化测试

Avocado-VT目的是根据大多virt功能和性能测试需要来做一个集中的项目，我们总结如下：

 - 客户系统安装，window（winXP-win7），Linux（RHEL，Fedora，OpenSUSE，其他的通过步骤引擎机器的系统）
 - Linux客户的串行输出
 - Migration, networking, timedrift ，其它类型的测试

对于 qemu subtests，我们可以这样做：

 - Monitor control for both human and QMP protocols
 - Build and use qemu using various methods (source tarball, git repo, rpm)
 - 进行一定程度的性能测试
 - The KVM unit tests can be run comfortably from inside virt-test, we do have full integration with the unittest execution

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

 1. 你想在github中做出贡献的话，分叉出test provider
 https://help.github.com/articles/fork-a-repo
 
 2. 克隆分叉出的软件库。在这个例子中，我们假定你克隆的分叉的软件库在目录：


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