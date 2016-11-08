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
Test providers是Avocado-VT内置的一个可加载的模块的集合机制，能推送出一个目录，包含测试文件，配置文件和相关的依赖文件，以及其他的那些目录. test providers背后的设计目标是：
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
每个test provider要么是一个本地的文件系统目录，要么是一个git软件库的子目录。当然，git软件库子目录可能是软件库的根目录，但是一点建议是人们能使用在其他项目中git软件库里的Avocado-VT providers。qemu希望位置它自己的provider，他们可以通过支持文件来做这，也就是说在qemu.git里在tests/avocado_vt子目录里。
### Test Provider definition file
主要的Avocado-VT套件需要一种方式知道有关test providers。他通过扫描在'test-providers.d'子目录里的definition files。 Definition files 是配置解析文件<http://docs.python.org/2/library/configparser.html> ，从一个test provider钟编码信息。这有一有关test provider文件的数据结构的例子：

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


 6. 修改而是，添加新的东西 到你的核心内容。当你对你的改变很满意时，你可以创建一个分支并且向我们推送请求。


## Writing your own avocado VT test
在这片文章中，我们将谈论：

 1. 测试文件在哪边
 2. 写一个简单的测试文件
 3. 尝试你的新测试，发送它到mailing list

### Write our own ‘uptime’ test - Step by Step procedure
现在，让我们去写我们的uptime test，它一生中唯一的目的就是 pick up a living guest，通过ssh连接它，并返回它的uptime。
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

 1. 一个vm对象有许多有趣的函数，我们打算更彻底的记录它们，但就目前而言，我们想要确认这个VM是alive和functional，至少在一个qemu进程的立场上。所以我们调用函数 *verify_alive()* ,它将确认qemu进程是否是functional并且如果minutors，如果any exist，是否是functional。如果由于任何问题这些条件中任何一个不支持，一个异常将会抛出并且测试将会失败。这个需求因为一些bug，vm进程可能死在水中，或者monitors没有响应：

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