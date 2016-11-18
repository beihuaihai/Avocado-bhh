# About Avovad

Avovado是帮助我们实现自动化测试的工具和库文件集。
它的优势在于它是一个测试框架。本地测试用python书写并且他们遵循*unittest*的模式，但任何可执行文件都可以做为一个测试。

Avovado的构成：
测试运行器允许您执行测试。这些测试文件要么是使用你选择的语言书写，要么是使用python和可用的库。在这两种情况下，你会获取诸如自动化的日志和系统信息收集等条件。
库文件帮助你使用简洁的却极具表现力和效率的方法写测试文件，你可以在库文件和APIs（应用程序编程接口）中发现更多有关用于测试的库的作者的信息。
*Plugins*（插件）可以扩展和添加新功能到Avocado框架。

Avocado尽可能的符合标准Python测试技术。测试文件使用来自于*unittest*类的Avocado API书写，而其他适合功能和性能的测试也被添加。测试运行器被设计去帮助人们运行测试文件，同时提供各种各样的系统和logging工具，并且如果你想要不用自己写就使用更多的功能特点，那么你可以逐步使用API的功能。



# Getting Started
要使用Avocado，第一步显而易见就是去安装它。

## Installing Avocado

### Installing from Packages

Avocado的RPM数据包中对Fedora和企业级linux系统是正式可用的。RPM在其它发行版本上的系统上可以打包和传输Avocado。DEB数据包在源代码树中是支持的（查看目录contrib/packages/debian）。

Avocado原本是一直在Fedora上开发的，不过比较好的进展是它正逐渐支持其他GUN/Linux平台。

## Fedora

首先，通过运行下面命令获取数据软件库的配置文件。

``` elixir
sudo curl https://repos-avocadoproject.rhcloud.com/static/avocado-fedora.repo -o /etc/yum.repos.d/avocado.repo
```


现在通过运行下面的命令检测你的Avocado和Avocado-lts软件库的配置：

``` fortran
sudo dnf repolist avocado avocado-lts
...
repo id      repo name                          status
avocado      Avocado                            50
avocado-lts  Avocado LTS (Long Term Stability)  disabled
```


一般的Avocado使用者想要使用标准的avocado软件库，可以通过追踪最新的Avocado发布信息找到。更多的关于长期稳定的版本的发布信息，请参考[Avocado Long Term Stability][1]和你的数据包管理文档中有关如何跳转到*avocado-lts*的件库。
最终，决定使用一般的发行版或长期稳定版，然后，你可以通过如下命令安装RPM包。

``` sql
sudo dnf install avocado
```

此外，其它两个Avocado数据包对Fedora来说也是可行的：

``` stylus
avocado-examples: contains example tests and other example files
avocado-plugins-output-html: HTML job report plugin
```

## Enterprise Linux

如果你使用的是企业级的linux或则其他的发行版如centos，可以通过如下的URL和命令安装：

``` elixir
# If not already, enable epel (for RHEL7 it's following cmd)
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
# Add avocado repository and install avocado
sudo curl https://repos-avocadoproject.rhcloud.com/static/avocado-el.repo -o /etc/yum.repos.d/avocado.repo
sudo yum install avocado
```
和Fedora一样，下面两个Avocado包对企业级的linux也是可用的：

``` stylus
avocado-examples: contains example tests and other example files
avocado-plugins-output-html: HTML job report plugin
```


The LTS (Long Term Stability) 发行版对企业级的linux来说是可行的，请参考链接[Avocado Long Term Stability][2]和你的如何跳转到*Avocado-lts*软件库的包管理文档。


## OpenSUSE

Avocado的[OpenSUSE][3]项目的稳定版。你可以通过如下命令安装：

``` sql
sudo zypper install avocado
```


## Generic installation from a GIT repository
首先确认你拥有基本的安装包，下面的命令应用于Fedora基本的发行版，请调整你的平台：

``` mel
sudo yum install -y git gcc python-devel python-pip libvirt-devel libyaml-devel redhat-rpm-config xz-devel
```


然后通过git软件库去安装Avocado：

``` vim
git clone git://github.com/avocado-framework/avocado.git
cd avocado
sudo make requirements
sudo python setup.py install
```


注意到python和pip应该指向python解释器的2.7.x版，如果安装遇到了问题，你可以再次尝试并使用命令行工具python2.7 和 pip2.7.


请注意：一些Avocado功能可以通过一些可选的插件实现，去安装 *HTML report plugin*，使用下面命令：

``` stylus
cd optional_plugins/html
sudo python setup.py install
```


如果你打算攻击Avocado，你可能想看看 [Hacking and Using Avocado][4] 。




## Installing from standard Python tools

Avocado可以通过标准的Python包工具pip去安装。在大多数python版本大于2.7的POSIX系统并且*pip*可以使用，安装通过以下命令执行：

``` sql
 pip install avocado-framework
```

注意：
软件的设计方案是，仅仅只有Avocado测试运行器核心的依赖文件被安装。你可能注意到，基于你的系统，一些插件下载失败，因为这些文件没有依赖关系。

如果你想安装要求的所有插件，你可以尝试运行下面的命令：

> pip install -r
> https://raw.githubusercontent.com/avocado-framework/avocado/master/requirements.txt

结果虽然非常依赖你的系统的设置，例如，拥有正确的编译器，头文件，可用的库文件。更多可预见的完整的Avocado经验可以在正式的RPM包中获取。

## Using Avocado
你应该首先通过使用测试运行器体验Avocado，命令行工具可以便捷的启动测试和收集结果。

### Running Tests

为了运行测试，请根据一个测试参考文件使用*run*的次命令运行*avocado*，最后结果会提供一个文件路径或者一个可识别的名字：

``` groovy
$ avocado run /bin/true
JOB ID    : 381b849a62784228d2fd208d929cc49f310412dc
JOB LOG   : $HOME/avocado/job-results/job-2014-08-12T15.39-381b849a/job.log
TESTS     : 1
 (1/1) /bin/true: PASS (0.01 s)
RESULTS    : PASS 1 | ERROR 0 | FAIL 0 | SKIP 0 | WARN 0 | INTERRUPT 0
TESTS TIME : 0.01 s
JOB HTML  : $HOME/avocado/job-results/job-2014-08-12T15.39-381b849a/html/results.html
```


你可能注意到我们使用/bin/true做一个测试文件，并且结果符合我们的预期，它通过了！这是已知的一个简单测试，但还有其他类型的测试，如*instrumented tests*。更多的信息可以查看链接[Test Types][6]，继续阅读：

Note：
尽管在大多数情况下，如下运行测试命令：
*avocado run $test1 $test3 ...* 是没有问题的，但可能导致参数碰撞测试名称冲突。最安全的执行测试的方式是 *avocado run --$argument1 --$argument2 -- $test1 $test2*。在  -  之后一切都将被当做位置参数，因此测试名称（ 假设*avocado run*）

### Listing tests

你有两种方式探索测试文件，你可以通过使用参数--dry-run 模拟执行。

``` groovy
avocado run /bin/true --dry-run
JOB ID     : 0000000000000000000000000000000000000000
JOB LOG    : /tmp/avocado-dry-runSeWniM/job-2015-10-16T15.46-0000000/job.log
TESTS      : 1
 (1/1) /bin/true: SKIP
RESULTS    : PASS 0 | ERROR 0 | FAIL 0 | SKIP 1 | WARN 0 | INTERRUPT 0
TESTS TIME : 0.00 s
JOB HTML   : /tmp/avocado-dry-runSeWniM/job-2015-10-16T15.46-0000000/html/results.html
```


支持所有的 *run* 参数，模拟运行甚至列出所有的测试参数。

其他方式是：不提供参数，使用*list*次命令列出所有被发现的测试文件，
Avocado列出了每个插件的默认测试文件，输出可能像这样：


``` stylus
$ avocado list
INSTRUMENTED /usr/share/avocado/tests/abort.py
INSTRUMENTED /usr/share/avocado/tests/datadir.py
INSTRUMENTED /usr/share/avocado/tests/doublefail.py
INSTRUMENTED /usr/share/avocado/tests/doublefree.py
INSTRUMENTED /usr/share/avocado/tests/errortest.py
INSTRUMENTED /usr/share/avocado/tests/failtest.py
INSTRUMENTED /usr/share/avocado/tests/fiotest.py
INSTRUMENTED /usr/share/avocado/tests/gdbtest.py
INSTRUMENTED /usr/share/avocado/tests/gendata.py
INSTRUMENTED /usr/share/avocado/tests/linuxbuild.py
INSTRUMENTED /usr/share/avocado/tests/multiplextest.py
INSTRUMENTED /usr/share/avocado/tests/passtest.py
INSTRUMENTED /usr/share/avocado/tests/sleeptenmin.py
INSTRUMENTED /usr/share/avocado/tests/sleeptest.py
INSTRUMENTED /usr/share/avocado/tests/synctest.py
INSTRUMENTED /usr/share/avocado/tests/timeouttest.py
INSTRUMENTED /usr/share/avocado/tests/trinity.py
INSTRUMENTED /usr/share/avocado/tests/warntest.py
INSTRUMENTED /usr/share/avocado/tests/whiteboard.py
...
```

这些python文件被Avocado用来去包含*INSTRUMENTED*的测试。

让我们现在列出可执行的shell脚本：

``` groovy
$ avocado list | grep ^SIMPLE
SIMPLE       /usr/share/avocado/tests/env_variables.sh
SIMPLE       /usr/share/avocado/tests/output_check.sh
SIMPLE       /usr/share/avocado/tests/simplewarning.sh
SIMPLE       /usr/share/avocado/tests/failtest.sh
SIMPLE       /usr/share/avocado/tests/passtest.sh
```

这边，如前所述，  *SIMPLE*标记代表了这些文件被当做简单测试文件对待去执行。你也可以使用参数 *--verbose*  或者 *-V* 标记去显示能被Avocado发现的文件，但这些文件没有被当做是Avocado的测试文件：


``` groovy
$ avocado list examples/gdb-prerun-scripts/ -V
Type       file
NOT_A_TEST examples/gdb-prerun-scripts/README
NOT_A_TEST examples/gdb-prerun-scripts/pass-sigusr1

SIMPLE: 0
INSTRUMENTED: 0
MISSING: 0
NOT_A_TEST: 2
```

注意到标记verbose也添加了概要信息。

## Writing a Simple Test

这个非常简单的简易测试事例，用shell脚本书写：


``` ruby
$ echo '#!/bin/bash' > /tmp/simple_test.sh
$ echo 'exit 0' >> /tmp/simple_test.sh
$ chmod +x /tmp/simple_test.sh
```
注意到文件被赋予执行权限，这对Avocado来说是做为一个简单测试文件的需要。也注意到脚本最后用编码0退出，这是用来发送一个成功的信号给Avocado。

## Running A More Complex Test Job

你可以用任意的顺序去运行任意数量的测试，就像混杂匹配的*instrumented and simple tests*一样：

``` groovy
$ avocado run failtest.py sleeptest.py synctest.py failtest.py synctest.py /tmp/simple_test.sh
JOB ID    : 86911e49b5f2c36caeea41307cee4fecdcdfa121
JOB LOG   : $HOME/avocado/job-results/job-2014-08-12T15.42-86911e49/job.log
TESTS     : 6
 (1/6) failtest.py:FailTest.test: FAIL (0.00 s)
 (2/6) sleeptest.py:SleepTest.test: PASS (1.00 s)
 (3/6) synctest.py:SyncTest.test: PASS (2.43 s)
 (4/6) failtest.py:FailTest.test: FAIL (0.00 s)
 (5/6) synctest.py:SyncTest.test: PASS (2.44 s)
 (6/6) /bin/true: PASS (0.00 s)
 (6/6) /tmp/simple_test.sh.1: PASS (0.02 s)
RESULTS    : PASS 2 | ERROR 2 | FAIL 2 | SKIP 0 | WARN 0 | INTERRUPT 0
TESTS TIME : 5.88 s
JOB HTML  : $HOME/avocado/job-results/job-2014-08-12T15.42-86911e49/html/results.html
```

## Interrupting The Job On First Failed Test (failfast)

Avocado的 *run* 命令有一个选项 *--failfast on* 用于在第一个失败的测试中退出工作。

``` groovy
$ avocado run --failfast on /bin/true /bin/false /bin/true /bin/true
JOB ID     : eaf51b8c7d6be966bdf5562c9611b1ec2db3f68a
JOB LOG    : $HOME/avocado/job-results/job-2016-07-19T09.43-eaf51b8/job.log
TESTS      : 4
 (1/4) /bin/true: PASS (0.01 s)
 (2/4) /bin/false: FAIL (0.01 s)
Interrupting job (failfast).
RESULTS    : PASS 1 | ERROR 0 | FAIL 1 | SKIP 2 | WARN 0 | INTERRUPT 0
TESTS TIME : 0.02 s
JOB HTML   : /home/apahim/avocado/job-results/job-2016-07-19T09.43-eaf51b8/html/results.html
```

选项 *--failfast* 接受到参数 *off* . 因为在该参数的默认值是disabled，当the original job用参数 *--failfast on*去 执行，参数 *off* 仅在重复的多个jobs中才有意义。

## Running Tests With An External Runner

在大多数软件项目中包含逐步开发成长的测试套件是很常见的，这些测试套件通常包含了一个定制的built，特定的test runner，用来指示如何发现并运行他们自己的测试文件。

仍然，由于种种原因运行这些嵌入到Avocado的工具的的测试可能是一个非常好的方式，涵盖了各种人类可读和机器可读的格式的结果，在这些测试工具旁（Avocado的系统信息功能）收集系统信息等等。

Avocado依靠"external runner"功能使其成为可能。使用它的最基本方式是：

``` elixir
$ avocado run --external-runner=/path/to/external_runner foo bar baz
```
在这个例子中，Avocado将汇报个别的测试结果给测试文件 *foo*，*bar* 和 *baz*。实际结果将基于各自执行文件 /path/to/external_runner foo, /path/to/external_runner bar 和最终的 /path/to/external_runner baz的返回代码。

换种方式解释这个功能如何工作的，想到“external runner”做为某些解释器，个别的测试文件可以被解释器识别并且能被执行。一个UNIX shell，说明/bin/sh 能够被当做一个*external runner*，并且带有shell code的文件可以被用作测试文件：

``` groovy
$ echo "exit 0" > /tmp/pass
$ echo "exit 1" > /tmp/fail
$ avocado run --external-runner=/bin/sh /tmp/pass /tmp/fail
JOB ID     : 4a2a1d259690cc7b226e33facdde4f628ab30741
JOB LOG    : /home/<user>/avocado/job-results/job-<date>-<shortid>/job.log
TESTS      : 2
(1/2) /tmp/pass: PASS (0.01 s)
(2/2) /tmp/fail: FAIL (0.01 s)
RESULTS    : PASS 1 | ERROR 0 | FAIL 1 | SKIP 0 | WARN 0 | INTERRUPT 0
TESTS TIME : 0.01 s
JOB HTML   : /home/<user>/avocado/job-results/job-<date>-<shortid>/html/results.html
```

这个例子非常明显，能够通过给予的测试文本 /tmp/pass 和 /tmp/fail ，壳shell “shebangs” (#!/bin/sh)当做解释器，赋予测试文件执行权限 (chmod +x /tmp/pass /tmp/fail)，并且把他们做为“SIMPLE”测试文件执行。

但是现在考虑下面的例子：

``` groovy
$ avocado run --external-runner=/bin/curl http://local-avocado-server:9405/jobs/ \
                    http://remote-avocado-server:9405/jobs/
JOB ID     : 56016a1ffffaba02492fdbd5662ac0b958f51e11
JOB LOG    : /home/<user>/avocado/job-results/job-<date>-<shortid>/job.log
TESTS      : 2
(1/2) http://local-avocado-server:9405/jobs/: PASS (0.02 s)
(2/2) http://remote-avocado-server:9405/jobs/: FAIL (3.02 s)
RESULTS    : PASS 1 | ERROR 0 | FAIL 1 | SKIP 0 | WARN 0 | INTERRUPT 0
TESTS TIME : 3.04 s
JOB HTML   : /home/<user>/avocado/job-results/job-<date>-<shortid>/html/results.html
```
有效地使 /bin/curl 成为一个“external test runner”，负责去获取这些URLs，并且报告他们每个的成功或者失败。

## Debugging tests
当开发一些新的测试，你经常性地想直接查看工作中的log，无需转换屏幕并且无需到尾端查看job log。

为了做到这点，你可以使用 *avocado --show test run ...* 或者 *avocado run --show-job-log ...*  选项：

``` stylus
$ avocado --show test run examples/tests/sleeptest.py
...
Job ID: f9ea1742134e5352dec82335af584d1f151d4b85

START 1-sleeptest.py:SleepTest.test

PARAMS (key=timeout, path=*, default=None) => None
PARAMS (key=sleep_length, path=*, default=1) => 1
Sleeping for 1.00 seconds
PASS 1-sleeptest.py:SleepTest.test

Test results available in $HOME/avocado/job-results/job-2015-06-02T10.45-f9ea174
```

正如你所看见到的，界面输出被抑制并且只有工作log被显示，对于开发和debug而言这是一个非常有用的功能。

# Writing Avocado Tests
我们将用python写一个Avocado测试，它继承自avocado.test。这样的测试被称作*instrumented test*。

## Basic example
让我们重建一个前面的喜欢用的测试文件，*sleeptest* 。它非常简单，除了睡眠一会其他什么也不做。

``` stylus
import time

from avocado import Test

class SleepTest(Test):

    def test(self):
        sleep_length = self.params.get('sleep_length', default=1)
        self.log.debug("Sleeping for %.2f seconds", sleep_length)
        time.sleep(sleep_length)
```

这是你可以为Avocado编写的最简单测试，同时还利用它的API功能。

## What is an Avocado Test
正如上面的例子中所看到的，一个Avocado test可以用继承自avocado.Test类的*test*函数来开始。


## Multiple tests and naming conventions
在一个单独的类中，可以有多种test函数。
为了这样做，可以选择一种命名方式，如以*test*开头，可以有*test_foo*，*test_bar*等等。我们建议你遵循  [PEP8 Function Names][7] 章节定义的那样的命名方式。

对于类的名字，你可以选择任何你喜欢的，但我们仍建议你依照CamelCase约定，大家耳熟能详的*CapWords*定义在 [Class Names][8]下面的PEP 8文档里。

## Convenience Attributes

测试类给我们提供了许多便捷的属性：

 - 打算在测试中使用log工具，那可能需要通过访问*self.log*的方式，让你可以查看debug的日志，信息，错误和警告信息。
 - 一个参数的 passing system（和fetching system）可能会通过*self.params*的方式去访问。这是一种*Multiplexer*技术，你可以在[Test variants-Mux][9]中发现更多的信息。


## Saving test generated (custom) data
每个测试事例都会提供一个被称之为 *whiteboard*的东西。它可以通过*self.whiteboard*去访问。这种白板（whiteboard）就是一个会被自动保存进测试结果的一个简单字符串（只要输出格式支持）。如果你选择保存二进制数据进白板，你首先需要修改编码方式（base64是一个明显的选择）。

在之前演示的*sleeptest*上建立一个测试，假设你想通过使用一些其他的脚本或者数据分析工具去保存sleep length：

``` stylus
def test(self):
    sleep_length = self.params.get('sleep_length', default=1)
    self.log.debug("Sleeping for %.2f seconds", sleep_length)
    time.sleep(sleep_length)
    self.whiteboard = "%.2f" % sleep_length
```
whiteboard应该能够通过可用的测试结果的插件产生的文件去显示。*result.json*文件已经对它的每个测试文件都涵盖了whiteboard。此外，我们将保存一个原始副本的whiteboard内容在名为*whiteboard*文件中，它和*result.json*文件在同一层，这将提供很大的便捷（可能你想直接使用benchmark的结果，用你定制的脚本去分析详细的benchmark结果）。

## Accessing test parameters
每个test都有一套参数集可以通过*self.params.get($name, $path=None, $default=None)*访问。Avocado用你在一个Multiplex配置文件（见 *Test variants-Mux*）中定义的所有参数去发现和填充*self.paras*。思考下面给出的有关sleeptest参数复用的文件：

``` stylus
sleeptest:
    type: "builtin"
    length: !mux
        short:
            sleep_length: 0.5
        medium:
            sleep_length: 1
        long:
            sleep_length: 5
```
当通过命令*avocado run $test --mux-yaml $file.yaml*启动这个例子时，三个可变类型被执行，并且内容被加入到*/run*的命名空间（详见 [Test variants - Mux][10]）。每个变体包含可变的"type"和"sleep_length"。为了获得现在的值，你需要有名称（"sleep_length"）和它的路径。路径不同于每个变体，所以需要路径中最合适的部分，在这个例子中： /run/sleeptest/length/\*  或者也许  sleeptest/\*可能就足够了。它依赖于你是怎样设置的。

默认值是可供选择的，但总要考虑去很好的处理他们。一些人可能用不同的参数或者什么参数也没有使用去执行你的测试文件。它应该都能完美的支持。

所以这个完整的事例，用于如何去访问"sleep_length"的语句应该是下面这样：

``` stylus
self.params.get("sleep_length", "/*/sleeptest/*", 1)
```

有一种方式可以让它更简单。它可能定义了解析顺序，那么对于简单查询你可以省略路径：

``` stylus
self.params.get("sleep_length", None, 1)
self.params.get("sleep_length", '*', 1)
self.params.get("sleep_length", default=1)
```

我们应该总是尽量的避开参数冲突（multiple参数变量匹配已给不同源路径的关键字）。如果不可能（例如：当你使用 multiple yaml文件去配置时），你可以通过修改--mux-path参数来修改默认的路径。它切分了参数并通过路径去一个个遍历他们。当在第一个切分中有一个匹配，它返回匹配到的值，无需再尝试其他的切分。尽管相关的查询仅仅匹配--mux-path的切分。

有许多种使用路径来分离参数冲突的方式，或者仅仅让你查询的参数的路径更具体些。通常在测试中，使用' * '号就足够了，没必要使用命名空间，但命名空间可以使用法更清晰，更容易使用。

考虑到路径总是要考虑到所有使用者。通常使用附加的变量去扩展默认配置，或者组合不同的路径产生一个使用者需要的正确的情境。使用者可以在其它地方引入值（例如：/run/sleeptest => /upstream/sleeptest）或者合并其它冲突文件到默认路径，这将避免冲突，不过将返回的是他们的替代值。然后你需要阐明路径（例如:'\*' = sleeptest/\*）。

更多的信息在[Test variants - Mux][11]。

## Using a multiplex file
可以使用带有 multiplex文件的Avocado运行器去提供参数和产生矩阵给sleeptest，如下：

``` stylus
$ avocado run sleeptest.py --mux-yaml examples/tests/sleeptest.py.data/sleeptest.yaml
JOB ID     : d565e8dec576d6040f894841f32a836c751f968f
JOB LOG    : $HOME/avocado/job-results/job-2014-08-12T15.44-d565e8de/job.log
TESTS      : 3
 (1/3) sleeptest.py:SleepTest.test;1: PASS (0.50 s)
 (2/3) sleeptest.py:SleepTest.test;2: PASS (1.00 s)
 (3/3) sleeptest.py:SleepTest.test;3: PASS (5.00 s)
RESULTS    : PASS 3 | ERROR 0 | FAIL 0 | SKIP 0 | WARN 0 | INTERRUPT 0
TESTS TIME : 6.50 s
JOB HTML   : $HOME/avocado/job-results/job-2014-08-12T15.44-d565e8de/html/results.html
```
选项 *--mux-yaml*要么从 *$FILE_LOCATION* 要么从*$INJECT_TO:$FILE_LOCATION*获取值。就像在[Test variants - Mux ][12]解释的那样，没有任何路径被加入到 /run，去做默认的相对路径。*$INJECT_TO*要么给出相对路径，那么它的位置 */run/$INJECT_TO*，要么给出绝对路径（以'/'开始），那么它就直接使用指定的位置并且直到测试或者框架的开发者从定位中获取值（使用路径或者添加路径是到 *mux-path*）。要明白执行这些命令的不同之处：

``` stylus
$ avocado multiplex -t -m examples/tests/sleeptest.py.data/sleeptest.yaml
$ avocado multiplex -t -m duration:examples/tests/sleeptest.py.data/sleeptest.yaml
$ avocado multiplex -t -m /my/location:examples/tests/sleeptest.py.data/sleeptest.yaml
```
注意到，当你的 multiplex文件为sleeptest指定所有参数时，你不能让test ID为空：

``` stylus
$ scripts/avocado run --mux-yaml examples/tests/sleeptest/sleeptest.yaml
Empty test ID. A test path or alias must be provided
```

你也可以用同样的 *multiplex*文件执行多个测试：

``` stylus
$ avocado run sleeptest.py synctest.py --mux-yaml examples/tests/sleeptest.py.data/sleeptest.yaml
JOB ID     : cd20fc8d1714da6d4791c19322374686da68c45c
JOB LOG    : $HOME/avocado/job-results/job-2016-05-04T09.25-cd20fc8/job.log
TESTS      : 8
 (1/8) sleeptest.py:SleepTest.test;1: PASS (0.50 s)
 (2/8) sleeptest.py:SleepTest.test;2: PASS (1.00 s)
 (3/8) sleeptest.py:SleepTest.test;3: PASS (5.01 s)
 (4/8) sleeptest.py:SleepTest.test;4: PASS (10.00 s)
 (5/8) synctest.py:SyncTest.test;1: PASS (2.38 s)
 (6/8) synctest.py:SyncTest.test;2: PASS (2.47 s)
 (7/8) synctest.py:SyncTest.test;3: PASS (2.46 s)
 (8/8) synctest.py:SyncTest.test;4: PASS (2.45 s)
RESULTS    : PASS 8 | ERROR 0 | FAIL 0 | SKIP 0 | WARN 0 | INTERRUPT 0
TESTS TIME : 26.26 s
JOB HTML   : $HOME/avocado/job-results/job-2016-05-04T09.25-cd20fc8/html/results.html
```

## Advanced logging capabilities
Avocado在测试运行时提供高级的logging能力。在测试时这些可能和标准的Python库的APIs结合。

在更长期和更复杂的测试中，一个普通的例子需要遵循特定的发展。让我们查看一个非常简单的测试例子，在一个单独的测试中有多个层级：

``` stylus
import logging
import time

from avocado import Test

progress_log = logging.getLogger("progress")

class Plant(Test):

    def test_plant_organic(self):
        rows = self.params.get("rows", default=3)

        # Preparing soil
        for row in range(rows):
            progress_log.info("%s: preparing soil on row %s",
                              self.name, row)

        # Letting soil rest
        progress_log.info("%s: letting soil rest before throwing seeds",
                          self.name)
        time.sleep(2)

        # Throwing seeds
        for row in range(rows):
            progress_log.info("%s: throwing seeds on row %s",
                              self.name, row)

        # Let them grow
        progress_log.info("%s: waiting for Avocados to grow",
                          self.name)
        time.sleep(5)

        # Harvest them
        for row in range(rows):
            progress_log.info("%s: harvesting organic avocados on row %s",
                              self.name, row)
```

从现在开始，你可以让Avocado去显示log流，这些流要么是专有的要么是其他内建指令的添加：

``` stylus
$ avocado --show app,progress run plant.py
```
结果应该和下面类似：

``` stylus
JOB ID     : af786f86db530bff26cd6a92c36e99bedcdca95b
JOB LOG    : /home/cleber/avocado/job-results/job-2016-03-18T10.29-af786f8/job.log
TESTS      : 1
 (1/1) plant.py:Plant.test_plant_organic: progress: 1-plant.py:Plant.test_plant_organic: preparing soil on row 0
progress: 1-plant.py:Plant.test_plant_organic: preparing soil on row 1
progress: 1-plant.py:Plant.test_plant_organic: preparing soil on row 2
progress: 1-plant.py:Plant.test_plant_organic: letting soil rest before throwing seeds
-progress: 1-plant.py:Plant.test_plant_organic: throwing seeds on row 0
progress: 1-plant.py:Plant.test_plant_organic: throwing seeds on row 1
progress: 1-plant.py:Plant.test_plant_organic: throwing seeds on row 2
progress: 1-plant.py:Plant.test_plant_organic: waiting for Avocados to grow
\progress: 1-plant.py:Plant.test_plant_organic: harvesting organic avocados on row 0
progress: 1-plant.py:Plant.test_plant_organic: harvesting organic avocados on row 1
progress: 1-plant.py:Plant.test_plant_organic: harvesting organic avocados on row 2
PASS (7.01 s)
RESULTS    : PASS 1 | ERROR 0 | FAIL 0 | SKIP 0 | WARN 0 | INTERRUPT 0
TESTS TIME : 7.01 s
JOB HTML   : /home/cleber/avocado/job-results/job-2016-03-18T10.29-af786f8/html/results.html
```

定制的*progress*流和应用输出相结合，这种方式可能适合也可能不适合你的需要和选择。如果你想要*progress*流被发送进一个分离的文件，清楚透明持久，你可以这样运行Avocado：

``` stylus
$ avocado run plant.py --store-logging-stream progress
```
结果是，除了所有其他的log文件产生外，将产生一个名为*progres.INFO*的log文件在log结果目录中。在测试运行期间，将可以看到这样的进程：

``` stylus
$ tail -f ~/avocado/job-results/latest/progress.INFO
10:36:59 INFO | 1-plant.py:Plant.test_plant_organic: preparing soil on row 0
10:36:59 INFO | 1-plant.py:Plant.test_plant_organic: preparing soil on row 1
10:36:59 INFO | 1-plant.py:Plant.test_plant_organic: preparing soil on row 2
10:36:59 INFO | 1-plant.py:Plant.test_plant_organic: letting soil rest before throwing seeds
10:37:01 INFO | 1-plant.py:Plant.test_plant_organic: throwing seeds on row 0
10:37:01 INFO | 1-plant.py:Plant.test_plant_organic: throwing seeds on row 1
10:37:01 INFO | 1-plant.py:Plant.test_plant_organic: throwing seeds on row 2
10:37:01 INFO | 1-plant.py:Plant.test_plant_organic: waiting for Avocados to grow
10:37:06 INFO | 1-plant.py:Plant.test_plant_organic: harvesting organic avocados on row 0
10:37:06 INFO | 1-plant.py:Plant.test_plant_organic: harvesting organic avocados on row 1
10:37:06 INFO | 1-plant.py:Plant.test_plant_organic: harvesting organic avocados on row 2
```
一样的 *progress*  logger，可能被用在跨多文件测试函数和多文件模块的测试。在给出的例子中，测试名称被用来给出其他的内容。

## *unittest.TestCase* heritage

既然Avocado测试继承自*unittest.TestCase*，你可以使用它所有的继承自父类的方法。

代码示例使用了 *assertEqual*，*assertTrue* 和 *assertIsInstace*：

``` stylus
from avocado import Test

class RandomExamples(Test):
    def test(self):
        self.log.debug("Verifying some random math...")
        four = 2 * 2
        four_ = 2 + 2
        self.assertEqual(four, four_, "something is very wrong here!")

        self.log.debug("Verifying if a variable is set to True...")
        variable = True
        self.assertTrue(variable)

        self.log.debug("Verifying if this test is an instance of test.Test")
        self.assertIsInstance(self, test.Test)
```

## Running tests under other *unittest* runners

*nose*是另一个python测试框架，也兼容*unittest*。
因为这样，你可以用*nosetests*应用程序运行avocado测试：

``` stylus
$ nosetests examples/tests/sleeptest.py
.
----------------------------------------------------------------------
Ran 1 test in 1.004s

OK
```
相反的是，你也可以使用标准的*unittest.main()*函数的进入点去运行Avocado测试。检查下面的代码，用*dummy.py*去保存：

``` stylus
from avocado import Test
from unittest import main

class Dummy(Test):
    def test(self):
        self.assertTrue(True)

if __name__ == '__main__':
    main()
```
它可以这样被启动：

``` stylus
$ python dummy.py
.
----------------------------------------------------------------------
Ran 1 test in 0.000s

OK
```

## Setup and cleanup methods

在测试前后如果你需要执行setup动作，你可以在*setup*和*tearDown*函数中分别这样做。我们会在下面的段落中给出例子。

## Running third party test suites
在测试自动化的负载中使用第三方开发的测试套件是很常见的。通过包裹执行代码进一个Avocado测试模块中，你可以访问由框架提供的设施和API。假设你想使用由C语言编写的经过打包，解压，编译的测试套件来执行测试。这儿有个例子：

``` stylus
#!/usr/bin/env python

import os

from avocado import Test
from avocado import main
from avocado.utils import archive
from avocado.utils import build
from avocado.utils import process


class SyncTest(Test):

    """
    Execute the synctest test suite.
    """
    default_params = {'sync_tarball': 'synctest.tar.bz2',
                      'sync_length': 100,
                      'sync_loop': 10}

    def setUp(self):
        """
        Set default params and build the synctest suite.
        """
        # Build the synctest suite
        self.cwd = os.getcwd()
        tarball_path = os.path.join(self.datadir, self.params.sync_tarball)
        archive.extract(tarball_path, self.srcdir)
        self.srcdir = os.path.join(self.srcdir, 'synctest')
        build.make(self.srcdir)

    def test(self):
        """
        Execute synctest with the appropriate params.
        """
        os.chdir(self.srcdir)
        cmd = ('./synctest %s %s' %
               (self.params.sync_length, self.params.sync_loop))
        process.system(cmd)
        os.chdir(self.cwd)


if __name__ == "__main__":
    main()
```
这边我们有一个例子*setup*函数在起作用：这边我们通过* avocado.Test.datadir()*函数获得测试套件代码（tarball）的位置，然后通过*avocado.utils.archive.extract()*函数去解压打包的文件，API解压测试套件tarball，按照*avocado.utils.build.make()*函数，建立一个套件。

*setup*函数是Avocado中你被允许调用*skip*方法的唯一地方，已知，如果一个测试开始被执行，按照定义它不能忽略任何东西。Avocado将尽力去强调这个范围，所以如果你在*setup*之外使用*skip*，执行的测试将被标记为错误的状态，并且错误信息将指示你修改你的测试代码。

在这个例子中，*test*函数将使用合理的参数，使用接口函数*avocado.utils.process.system()*去获取到编译套件和执行*./synctest*命令的基本目录。

## Fetching asset files

去运行上面提到的第三方测试套件，或者为了其他目的，我们提供一个asset fetcher做为Avocado测试类的函数。asset函数在*cache_dirs*键值和配置文件里的*[datadir.paths]*段落里寻找目录列表。只读的目录也支持。
当asset文件不在任何已提供的目录中时，我们尽力的从提供的位置中下载文件，把他拷贝到可写的缓存目录中。
例子：

``` stylus
cache_dirs = ['/usr/local/src/', '~/avocado/cache']
```
在上面的例子中，*/usr/local/src/*是只读目录。既然那样，当我们需要从这个位置上获取有用的东西时，它将被拷贝到*~/avocado/cache*目录中。

如果你没有提供一个*cache_dirs*，我们将在avocado的*data_dir*的位置创建一个*cache*目录来放入获取到的文件。

 - 使用案例1：在配置文件中没有*cache_dirs*键，仅仅有用的名字被提供在完整的url格式中：
 

``` stylus
...
    def setUp(self):
        stress = 'http://people.seas.harvard.edu/~apw/stress/stress-1.0.4.tar.gz'
        tarball = self.fetch_asset(stress)
        archive.extract(tarball, self.srcdir)
...
```

在这个案子中，*fetch_asset()*函数将下载url提供的文件，并拷贝到*$data_dir/cache*目录中。*tarball*变量将包含例如*/home/user/avocado/data/cache/stress-1.0.4.tar.gz*的文件。

 - 使用案例2：只读缓存目录的提供。*cache_dirs = ['/mnt/files']* ：
 

``` stylus
...
    def setUp(self):
        stress = 'http://people.seas.harvard.edu/~apw/stress/stress-1.0.4.tar.gz'
        tarball = self.fetch_asset(stress)
        archive.extract(tarball, self.srcdir)
...
```
在这个案子中，我们尽力在*/mnt/files*目录中找到*stress-1.0.4.tar.gz*文件。如果不在，因为/mnt/files是只读的，我们将尽力下载asset文件到*$data_dir/cache*目录。

 - 使用案例3：可写的缓存目录被提供，连同本地的列表一起。*cache_dirs = ['~/avocado/cache']*：
 
 

``` stylus
...
    def setUp(self):
        st_name = 'stress-1.0.4.tar.gz'
        st_hash = 'e1533bc704928ba6e26a362452e6db8fd58b1f0b'
        st_loc = ['http://people.seas.harvard.edu/~apw/stress/stress-1.0.4.tar.gz',
                  'ftp://foo.bar/stress-1.0.4.tar.gz']
        tarball = self.fetch_asset(st_name, asset_hash=st_hash,
                                   locations=st_loc)
        archive.extract(tarball, self.srcdir)
...
```
在这个案子中，我们尽力从已提供的位置列表中去下载文件*stress-1.0.4.tar.gz*（如果不在*~/avocado/cache*文件夹中）。哈希值也被提供，所以我们将确认哈希值。这样做，首先我们在相同的目录中去寻找名为*stress-1.0.4.tar.gz.sha1*的哈希文件。如果哈希文件不存在，我们计算哈希值，创造hash文件给将来使用。

结果打包变量内容是*~/avocado/cache/stress-1.0.4.tar.gz*。如果我们下载失败将会发生一个异常去确认文件。

列举*fetch_asset()*的属性：

 - *name：*名称用来命名获取的文件。它可以包含完整的URL，URL将被做为第一位置去尝试获取（在缓存目录搜索之后）。
 - *asset_hash：*（可选的）预期的文件的哈希值。如果没有，我们将跳过检测。如果提供，在计算哈希值前，我们将寻找hash文件去确认可用。如果hash文件不存在，我们将计算哈希值，并在同一个缓存目录中创建hash文件给将来使用。
 - *algorithm：*（可选的）提供哈希算法的格式。默认是sha1.
 - *locations：*（可选的）列出经常获取文件的位置。支持的方案有：*http://* ，*https://* ，*ftp://* 和 *file://* 。你需要把完整的url提供给文件，包含文件的名称。第一次成功的话将忽略下面的位置。注意到*file://* ，我们将在缓存目录中创建一个象征性地连接，指向文件原始的位置。
 - *expire：*（可选的）缓存文件有效的时间周期。周期之后，文件将再次被下载。值可能是一个整数或者一个包含时间和单位的字符串。例如：‘10d’（10天）。合法的单位是 *s*（秒），m（分），h（小时）和 d（天）。
 
 期待的*return*是一个有用的文件路径或者一个异常。
 
##  Test Output Check and Output Record Mode
 在许多场合，你想要更简单：检测应用程序的输出是否符合你的期待。为了帮助处理这些常用的用例，我们提供了选项： *--output-check-record [mode]* 给测试运行器：
 

``` stylus
--output-check-record OUTPUT_CHECK_RECORD
                      Record output streams of your tests to reference files
                      (valid options: none (do not record output streams),
                      all (record both stdout and stderr), stdout (record
                      only stderr), stderr (record only stderr). Default:
                      none
```

如果这个选项被使用，它将存储被执行程序的标准输出或者标准错误（或者两者都有，如果你指定了 *all* ）到相关文件中：*stdout.expected* 和 *stderr.expected*。这些文件被记录在测试数据目录中。这个数据目录和测试源文件目录相同，名称是：*[source_file_name.data]*。让我们把测试文件*synctest.py*做为一个例子。在一次Avocado的刷新检测中，你可以看到：

``` stylus
examples/tests/synctest.py.data/stderr.expected
examples/tests/synctest.py.data/stdout.expected
```
从这两个文件中发现，仅仅stdout.expected是非空的：

``` stylus
$ cat examples/tests/synctest.py.data/stdout.expected
PAR : waiting
PASS : sync interrupted
```
输出文件原本通过使用测试运行器和通过测试运行器的命令选项 –output-check-record all来获取数据的：

``` stylus
$ scripts/avocado run --output-check-record all synctest.py
JOB ID    : bcd05e4fd33e068b159045652da9eb7448802be5
JOB LOG   : $HOME/avocado/job-results/job-2014-09-25T20.20-bcd05e4/job.log
TESTS     : 1
 (1/1) synctest.py:SyncTest.test: PASS (2.20 s)
RESULTS    : PASS 1 | ERROR 0 | FAIL 0 | SKIP 0 | WARN 0 | INTERRUPT 0
TESTS TIME : 2.20 s
```
在参考文件被添加后，校验流程是透明的，就此而言你不需要再给测试运行器添任何的标签。现在每次测试被执行，在运行之后，在考虑到测试通过前，它将检测输出是否正确。如果你想修改默认的行为并且完全地忽略检测，你可以提供标记 *--output-check=off*给测试运行器。

*avocado.utils.process*  APIs有一个参数*allow_output_check* (defaults to *all*)，所以你可以选择哪个程序输出到相关文件，你可以选择记录他们。你可以选择*all*,这样标准输出和标准错误都可以输出到相关文件，*stdout*，这样仅对标准输出，*stderr*，这样仅对标准错误，或者*none*，这样所有输出都不允许被记录和检测。

这个带有简单测试的程序运行良好，程序或者shell脚本返回0（PASSed）或者！=0（FAILed）。让我们思考我们伪造的例子：


``` stylus
$ cat output_record.sh
#!/bin/bash
echo "Hello, world!"
```

让我们为这个测试记录输出：

``` stylus
$ scripts/avocado run output_record.sh --output-check-record all
JOB ID    : 25c4244dda71d0570b7f849319cd71fe1722be8b
JOB LOG   : $HOME/avocado/job-results/job-2014-09-25T20.49-25c4244/job.log
TESTS     : 1
 (1/1) output_record.sh: PASS (0.01 s)
RESULTS    : PASS 1 | ERROR 0 | FAIL 0 | SKIP 0 | WARN 0 | INTERRUPT 0
TESTS TIME : 0.01 s
```

在这个命令之后，你将注意到在shell脚本的同级目录出现了一个测试数据目录，包含了两个文件：

``` stylus
$ ls output_record.sh.data/
stderr.expected  stdout.expected
```
让我们查看他们的每一个都是什么？

``` stylus
$ cat output_record.sh.data/stdout.expected
Hello, world!
$ cat output_record.sh.data/stderr.expected
$
```
现在，每次这个测试运行，期待的文件都会被记录，不需要做其他的任何事，只要运行测试。让我们看看如果我们更改了文件*stdout.expected*的内容为*Hello, Avocado!*会发生什么：

``` stylus
$ scripts/avocado run output_record.sh
JOB ID    : f0521e524face93019d7cb99c5765aedd933cb2e
JOB LOG   : $HOME/avocado/job-results/job-2014-09-25T20.52-f0521e5/job.log
TESTS     : 1
 (1/1) output_record.sh: FAIL (0.02 s)
RESULTS    : PASS 0 | ERROR 0 | FAIL 1 | SKIP 0 | WARN 0 | INTERRUPT 0
TESTS TIME : 0.02 s
```
确认失败原因：

``` stylus
$ cat $HOME/avocado/job-results/job-2014-09-25T20.52-f0521e5/job.log
20:52:38 test       L0163 INFO | START 1-output_record.sh
20:52:38 test       L0164 DEBUG|
20:52:38 test       L0165 DEBUG| Test instance parameters:
20:52:38 test       L0173 DEBUG|
20:52:38 test       L0176 DEBUG| Default parameters:
20:52:38 test       L0180 DEBUG|
20:52:38 test       L0181 DEBUG| Test instance params override defaults whenever available
20:52:38 test       L0182 DEBUG|
20:52:38 process    L0242 INFO | Running '$HOME/Code/avocado/output_record.sh'
20:52:38 process    L0310 DEBUG| [stdout] Hello, world!
20:52:38 test       L0565 INFO | Command: $HOME/Code/avocado/output_record.sh
20:52:38 test       L0565 INFO | Exit status: 0
20:52:38 test       L0565 INFO | Duration: 0.00313782691956
20:52:38 test       L0565 INFO | Stdout:
20:52:38 test       L0565 INFO | Hello, world!
20:52:38 test       L0565 INFO |
20:52:38 test       L0565 INFO | Stderr:
20:52:38 test       L0565 INFO |
20:52:38 test       L0060 ERROR|
20:52:38 test       L0063 ERROR| Traceback (most recent call last):
20:52:38 test       L0063 ERROR|   File "$HOME/Code/avocado/avocado/test.py", line 397, in check_reference_stdout
20:52:38 test       L0063 ERROR|     self.assertEqual(expected, actual, msg)
20:52:38 test       L0063 ERROR|   File "/usr/lib64/python2.7/unittest/case.py", line 551, in assertEqual
20:52:38 test       L0063 ERROR|     assertion_func(first, second, msg=msg)
20:52:38 test       L0063 ERROR|   File "/usr/lib64/python2.7/unittest/case.py", line 544, in _baseAssertEqual
20:52:38 test       L0063 ERROR|     raise self.failureException(msg)
20:52:38 test       L0063 ERROR| AssertionError: Actual test sdtout differs from expected one:
20:52:38 test       L0063 ERROR| Actual:
20:52:38 test       L0063 ERROR| Hello, world!
20:52:38 test       L0063 ERROR|
20:52:38 test       L0063 ERROR| Expected:
20:52:38 test       L0063 ERROR| Hello, Avocado!
20:52:38 test       L0063 ERROR|
20:52:38 test       L0064 ERROR|
20:52:38 test       L0529 ERROR| FAIL 1-output_record.sh -> AssertionError: Actual test sdtout differs from expected one:
Actual:
Hello, world!

Expected:
Hello, Avocado!

20:52:38 test       L0516 INFO |
```
正如预期的那样，测试失败是因为我们改变了期望值。


## Test log, stdout and stderr in native Avocado modules

如果需要，你可以从本地测试范围直接写到期望的标准输出和标准错误文件。区分以下实体非常重要：

 - The test logs
 - The test expected stdout
 - The test expected stderr
 
 第一个是用来调试和收集信息为目的。此外写到self.log.warning将会导致测试被标记为脏，并且当其他一切运转正常，测试以WARN结尾。这意味着测试通过但有不相关的意外的状态被描述在警告的log中。
 
 你可以使用*avocado.Test.log*类属性的方法记录一些东西到测试日志中。
 思考例子：
 

``` stylus
class output_test(Test):

    def test(self):
        self.log.info('This goes to the log and it is only informational')
        self.log.warn('Oh, something unexpected, non-critical happened, '
                      'but we can continue.')
        self.log.error('Describe the error here and don't forget to raise '
                       'an exception yourself. Writing to self.log.error '
                       'won't do that for you.')
        self.log.debug('Everybody look, I had a good lunch today...')
```
如果你需要直接写测试标准输出和错误流，Avocado使用两个可用的预配置记录器来达到目的，记录器的名字是：*avocado.test.stdout*和*avocado.test.stderr*。你可以使用Python标准的logging API去写他们。例子：

``` stylus
import logging

class output_test(Test):

    def test(self):
        stdout = logging.getLogger('avocado.test.stdout')
        stdout.info('Informational line that will go to stdout')
        ...
        stderr = logging.getLogger('avocado.test.stderr')
        stderr.info('Informational line that will go to stderr')
```
Avocado将自动保存测试在输出上产生的一切信息到*stdout*文件，在测试结果目录中可以找到。同样应用于测试中的标准错误的信息，它被保存在同样位置中的*stderr*文件中。

此外，当使用运行器的输出记录特点，选项名为：*--output-check-record *，参数值有：*stdout*，*stderr* 或者 *all*，给予这些记录器的一切信息都被保存进在测试数据目录中的文件*stdout.expected*和*stderr.expected*（不同于job/test结果的目录）。

## Avocado Tests run on a separate process
在使用Avocado运行器主进程的环境中，为了避免测试进入混乱，测试做为一用子进程来运行。这将给测试带来更好的稳健性（测试不容易进入混乱或者被打断）和一些好的功能，例如可以设置测试的超时设定。

## Setting a Test Timeout
有时你的测试套件或者测试可能陷入永久的僵局，并且这可能影响到你的测试布局。你可能因为各种可能性，给你的测试建立一个*timeout*参数。测试的超时时间可以通过两种方式设置，按照下面的优先顺序：

 - 多元变量参数。你可以只设置超时时间参数，就像下面的过分简单的例子一样：
 

``` stylus
sleep_length = 5
sleep_length_type = float
timeout = 3
timeout_type = float
```

``` stylus
$ avocado run sleeptest.py --mux-yaml /tmp/sleeptest-example.yaml
JOB ID    : 6d5a2ff16bb92395100fbc3945b8d253308728c9
JOB LOG   : $HOME/avocado/job-results/job-2014-08-12T15.52-6d5a2ff1/job.log
TESTS     : 1
 (1/1) sleeptest.py:SleepTest.test: ERROR (2.97 s)
RESULTS    : PASS 0 | ERROR 1 | FAIL 0 | SKIP 0 | WARN 0 | INTERRUPT 0
TESTS TIME : 2.97 s
JOB HTML  : $HOME/avocado/job-results/job-2014-08-12T15.52-6d5a2ff1/html/results.html
```

``` stylus
$ cat $HOME/avocado/job-results/job-2014-08-12T15.52-6d5a2ff1/job.log
15:52:51 test       L0143 INFO | START 1-sleeptest.py
15:52:51 test       L0144 DEBUG|
15:52:51 test       L0145 DEBUG| Test log: $HOME/avocado/job-results/job-2014-08-12T15.52-6d5a2ff1/sleeptest.1/test.log
15:52:51 test       L0146 DEBUG| Test instance parameters:
15:52:51 test       L0153 DEBUG|     _name_map_file = {'sleeptest-example.yaml': 'sleeptest'}
15:52:51 test       L0153 DEBUG|     _short_name_map_file = {'sleeptest-example.yaml': 'sleeptest'}
15:52:51 test       L0153 DEBUG|     dep = []
15:52:51 test       L0153 DEBUG|     id = sleeptest
15:52:51 test       L0153 DEBUG|     name = sleeptest
15:52:51 test       L0153 DEBUG|     shortname = sleeptest
15:52:51 test       L0153 DEBUG|     sleep_length = 5.0
15:52:51 test       L0153 DEBUG|     sleep_length_type = float
15:52:51 test       L0153 DEBUG|     timeout = 3.0
15:52:51 test       L0153 DEBUG|     timeout_type = float
15:52:51 test       L0154 DEBUG|
15:52:51 test       L0157 DEBUG| Default parameters:
15:52:51 test       L0159 DEBUG|     sleep_length = 1.0
15:52:51 test       L0161 DEBUG|
15:52:51 test       L0162 DEBUG| Test instance params override defaults whenever available
15:52:51 test       L0163 DEBUG|
15:52:51 test       L0169 INFO | Test timeout set. Will wait 3.00 s for PID 15670 to end
15:52:51 test       L0170 INFO |
15:52:51 sleeptest  L0035 DEBUG| Sleeping for 5.00 seconds
15:52:54 test       L0057 ERROR|
15:52:54 test       L0060 ERROR| Traceback (most recent call last):
15:52:54 test       L0060 ERROR|   File "$HOME/Code/avocado/tests/sleeptest.py", line 36, in action
15:52:54 test       L0060 ERROR|     time.sleep(self.params.sleep_length)
15:52:54 test       L0060 ERROR|   File "$HOME/Code/avocado/avocado/job.py", line 127, in timeout_handler
15:52:54 test       L0060 ERROR|     raise exceptions.TestTimeoutError(e_msg)
15:52:54 test       L0060 ERROR| TestTimeoutError: Timeout reached waiting for sleeptest to end
15:52:54 test       L0061 ERROR|
15:52:54 test       L0400 ERROR| ERROR 1-sleeptest.py -> TestTimeoutError: Timeout reached waiting for sleeptest to end
15:52:54 test       L0387 INFO |
```
如果你通过了multiplex 对于 runner multiplexer，这将通过发送一个*signal.SIGTERM*信号给测试来强制注册一个3秒的超时设置在Avocado结束测试之前，使其提高*avocado.core.exceptions.TestTimeoutError*。

 - 默认参数属性。思考下面的例子：
 
 

``` stylus
import time

from avocado import Test
from avocado import main


class TimeoutTest(Test):

    """
    Functional test for Avocado. Throw a TestTimeoutError.
    """
    default_params = {'timeout': 3.0,
                      'sleep_time': 5.0}

    def test(self):
        """
        This should throw a TestTimeoutError.
        """
        self.log.info('Sleeping for %.2f seconds (2 more than the timeout)',
                      self.params.sleep_time)
        time.sleep(self.params.sleep_time)


if __name__ == "__main__":
    main()
```
这对定义在那的multiplex setup达到了相似的作用。

``` stylus
$ avocado run timeouttest.py
JOB ID    : d78498a54504b481192f2f9bca5ebb9bbb820b8a
JOB LOG   : $HOME/avocado/job-results/job-2014-08-12T15.54-d78498a5/job.log
TESTS     : 1
 (1/1) timeouttest.py:TimeoutTest.test: INTERRUPTED (3.04 s)
RESULTS    : PASS 0 | ERROR 1 | FAIL 0 | SKIP 0 | WARN 0 | INTERRUPT 0
TESTS TIME : 3.04 s
JOB HTML  : $HOME/avocado/job-results/job-2014-08-12T15.54-d78498a5/html/results.html
```


``` stylus
$ cat $HOME/avocado/job-results/job-2014-08-12T15.54-d78498a5/job.log
15:54:28 test       L0143 INFO | START 1-timeouttest.py:TimeoutTest.test
15:54:28 test       L0144 DEBUG|
15:54:28 test       L0145 DEBUG| Test log: $HOME/avocado/job-results/job-2014-08-12T15.54-d78498a5/timeouttest.1/test.log
15:54:28 test       L0146 DEBUG| Test instance parameters:
15:54:28 test       L0153 DEBUG|     id = timeouttest
15:54:28 test       L0154 DEBUG|
15:54:28 test       L0157 DEBUG| Default parameters:
15:54:28 test       L0159 DEBUG|     sleep_time = 5.0
15:54:28 test       L0159 DEBUG|     timeout = 3.0
15:54:28 test       L0161 DEBUG|
15:54:28 test       L0162 DEBUG| Test instance params override defaults whenever available
15:54:28 test       L0163 DEBUG|
15:54:28 test       L0169 INFO | Test timeout set. Will wait 3.00 s for PID 15759 to end
15:54:28 test       L0170 INFO |
15:54:28 timeouttes L0036 INFO | Sleeping for 5.00 seconds (2 more than the timeout)
15:54:31 test       L0057 ERROR|
15:54:31 test       L0060 ERROR| Traceback (most recent call last):
15:54:31 test       L0060 ERROR|   File "$HOME/Code/avocado/tests/timeouttest.py", line 37, in action
15:54:31 test       L0060 ERROR|     time.sleep(self.params.sleep_time)
15:54:31 test       L0060 ERROR|   File "$HOME/Code/avocado/avocado/job.py", line 127, in timeout_handler
15:54:31 test       L0060 ERROR|     raise exceptions.TestTimeoutError(e_msg)
15:54:31 test       L0060 ERROR| TestTimeoutError: Timeout reached waiting for timeouttest to end
15:54:31 test       L0061 ERROR|
15:54:31 test       L0400 ERROR| ERROR 1-timeouttest.py:TimeoutTest.test -> TestTimeoutError: Timeout reached waiting for timeouttest to end
15:54:31 test       L0387 INFO |
```
## Test Tags

这个需求用于提升更复杂的测试，使用了更先进的Python功能如继承。事实上由于Avocado使用了一个安全测试内省方式，更加的限制了实际上测试类的加载。Avocado可能需要你帮助确认这些测试。例如，假设你定义了一个新的测试类，它继承自Avocado的基本的测试类，并把它放到文件*mylibrary.py*中：


``` stylus
from avocado import Test


class MyOwnDerivedTest(Test):
    def __init__(self, methodName='test', name=None, params=None,
                 base_logdir=None, job=None, runner_queue=None):
        super(MyOwnDerivedTest, self).__init__(methodName, name, params,
                                               base_logdir, job,
                                               runner_queue)
        self.log('Derived class example')
```
那么使用派生类来实现你实际上的测试，在*mytest.py*文件中：

``` stylus
import mylibrary


class MyTest(mylibrary.MyOwnDerivedTest):

    def test1(self):
        self.log('Testing something important')

    def test2(self):
        self.log('Testing something even more important')
```

如果你尝试在那个文件中列出测试文件，那么你将获得：

``` stylus
scripts/avocado list mytest.py -V
Type       Test
NOT_A_TEST mytest.py

ACCESS_DENIED: 0
BROKEN_SYMLINK: 0
EXTERNAL: 0
FILTERED: 0
INSTRUMENTED: 0
MISSING: 0
NOT_A_TEST: 1
SIMPLE: 0
VT: 0
```
你需要通过添加文档字符串标记给Avocado一些帮助。文档字符串标记是*:avocado: enable*。这个标记让Avocado安全测试探测代码无论如何都把它看做一个avocado测试。让我们看看那是如何解决的。添加文档字符串，正如你所看到的下面的例子：

``` stylus
import mylibrary


class MyTest(mylibrary.MyOwnDerivedTest):
    """
    :avocado: enable
    """
    def test1(self):
        self.log('Testing something important')

    def test2(self):
        self.log('Testing something even more important')
```
现在，尝试再次在*mytest.py*文件上列出测试文件：

``` stylus
scripts/avocado list mytest.py -V
Type         Test
INSTRUMENTED mytest.py:MyTest.test1
INSTRUMENTED mytest.py:MyTest.test2

ACCESS_DENIED: 0
BROKEN_SYMLINK: 0
EXTERNAL: 0
FILTERED: 0
INSTRUMENTED: 2
MISSING: 0
NOT_A_TEST: 0
SIMPLE: 0
VT: 0
```
你也可以使用*：avocado：disable*标记，以相反的方式工作：某些东西看起来像一个Avocado测试，但我们强制的不让他做为一个Avocado test列出。

## Python *unittest* Compatibility Limitations And Caveats

当执行测试，Avocado使用不同于大多数其他Python unittest runners的方式。这带来了一些兼容性的限制，Avocado使用者必须知道。

## Execution Model
一个主要的不同是Avocado设计方案的结果，测试中应该包含self并且和其他的测试隔离。此外，Avocado test runner在分开的进程中运行每个测试。

如果你在许多测试方法中有unittest类并且用大多测试启动器来运行他们，你将发现所有的测试函数都在同样的进程下运行。检测你添加setup函数的行为：

``` stylus
def setUp(self):
    print("PID: %s", os.getpid())
```

如果你在Avocado下运行相同的测试，你将发现每个测试都在分开的进程中。

Class Level *setUp* and *tearDown*

因为Avocado测试执行模式（每个测试都运行在分开的进程中），它没有意义去支持unittest的*unittest.TestCase.setUpClass()* 和 *unittest.TestCase.tearDownClass()*。对每个测试而言，测试类被新实例化了，所以用这些方法去运行代码没有意义，因为在测试之间他们应该保持类状态。

如果你需要一个普通的setup去做许多测试，目前推荐的方法是去写一个正规的setup和tearDown代码去检查状态是否被设置。一个需要通过数据包来二进制安装的测试的例子：

 

``` stylus
from avocado import Test

from avocado.utils import software_manager
from avocado.utils import path as utils_path
from avocado.utils import process


class BinSleep(Test):

    """
    Sleeps using the /bin/sleep binary
    """
    def setUp(self):
        self.sleep = None
        try:
            self.sleep = utils_path.find_command('sleep')
        except utils_path.CmdNotFoundError:
            software_manager.install_distro_packages({'fedora': ['coreutils']})
            self.sleep = utils_path.find_command('sleep')

    def test(self):
        process.run("%s 1" % self.sleep)
```
如果你的测试setup是某种行为可以跨流程维持，像前面的例子中给的一个软件包的安装一样，这儿你可以几乎全部覆盖。

如果你想跨越测试执行去保留一个其他类型的一个类的数据，你将不得不经常的保存并从外部源中（如一个“pickle”文件）恢复数据。找到并使用一个可靠的安全的位置去保存现在不在Avocado 支持使用的案例中的数据。

## Environment Variables for Simple Tests
Avocado用Avocado变量和多元变量去配置BASH环境来运行测试。这些变量和简单测试相关，因为他们不能通过python直接使用Avocado的API，可以做一些本地测试和修改测试参数。

这儿有一些现在的变量，Avocado用来配置测试环境：

| Environemnt Variable    | Meaning                          | Example                                                                                       |
| ----------------------- | -------------------------------- | --------------------------------------------------------------------------------------------- |
| AVOCADO_VERSION         | Version of Avocado test runner   | 0.12.0                                                                                        |
| AVOCADO_TEST_BASEDIR    | Base directory of Avocado tests  | $HOME/Downloads/avocado-source/avocado                                                        |
| AVOCADO_TEST_DATADIR    | Data directory for the test      | $AVOCADO_TEST_BASEDIR/my_test.sh.data                                                         |
| AVOCADO_TEST_WORKDIR    | Work directory for the test      | /var/tmp/avocado_Bjr_rd/my_test.sh                                                            |
| AVOCADO_TEST_SRCDIR     | Source directory for the test    | /var/tmp/avocado_Bjr_rd/my-test.sh/src                                                        |
| AVOCADO_TEST_LOGDIR     | Log directory for the test       | $HOME/logs/job-results/job-2014-09-16T14.38-ac332e6/test-results/$HOME/my_test.sh.1           |
| AVOCADO_TEST_LOGFILE    | Log file for the test            | $HOME/logs/job-results/job-2014-09-16T14.38-ac332e6/test-results/$HOME/my_test.sh.1/debug.log |
| AVOCADO_TEST_OUTPUTDIR  | Output directory for the test    | $HOME/logs/job-results/job-2014-09-16T14.38-ac332e6/test-results/$HOME/my_test.sh.1/data      |
| AVOCADO_TEST_SYSINFODIR | The system information directory | $HOME/logs/job-results/job-2014-09-16T14.38-ac332e6/test-results/$HOME/my_test.sh.1/sysinfo   |
| .                       | All variables from –mux-yaml    | TIMEOUT=60; IO_WORKERS=10; VM_BYTES=512M; ...                                                 |

 
##  Simple Tests BASH extensions

为了强调简单测试，可以使用我们创建的被支持的库的集合。使用的唯一需要是：

``` stylus
PATH=$(avocado "exec-path"):$PATH
```
在shell PATH中给Avocado工具加一个路径。看一看命令*avocado exec-path*，查看可用的函数列表。从文件examples/tests/simplewarning.sh获得启发。


## Wrap Up

我们推荐你看一看在*examples/tests*目录中的一些现在测试的例子，包含了一些可以让你找到灵感的例子。那个目录除了包含一些例子，也被用于Avocado self测试套件去做一些Avocado自身的一些功能测试。

推荐看看[API Reference][13]。有更多的可能性。

# Result Formats
一个测试运行器必须提供各种各样的方法和感兴趣的部分包括人类或者机器去交流结果。

## Results for human beings

为了人类可读Avocado有两种不同的结果格式：

 - 它的默认的UI，基于命令行，基本文本，UI显示现场测试执行的结果。
 - 网页报告，在测试工作完成运行后产生。
 
### Avocado command line UI
一次正规的运行Avocado将以一种生活方式提供测试结果，所谓的生活方式是指，它的job和测试结果在不断的更新：

``` stylus
$ avocado run sleeptest.py failtest.py synctest.py
JOB ID    : 5ffe479262ea9025f2e4e84c4e92055b5c79bdc9
JOB LOG   : $HOME/avocado/job-results/job-2014-08-12T15.57-5ffe4792/job.log
TESTS     : 3
 (1/3) sleeptest.py:SleepTest.test: PASS (1.01 s)
 (2/3) failtest.py:FailTest.test: FAIL (0.00 s)
 (3/3) synctest.py:SyncTest.test: PASS (1.98 s)
RESULTS    : PASS 1 | ERROR 1 | FAIL 1 | SKIP 0 | WARN 0 | INTERRUPT 0
TESTS TIME : 3.17 s
JOB HTML  : $HOME/avocado/job-results/job-2014-08-12T15.57-5ffe4792/html/results.html
```
最重要的事是记住程序永远不需要人类去解析输出，去理解一个测试运行时发生了什么。

### HTML report
正如在前面的例子中所见到的，Avocado一旦完成测试就会展示产生的HTML报告的路径：

``` stylus
$ avocado run sleeptest.py failtest.py synctest.py
...
JOB HTML  : $HOME/avocado/job-results/job-2014-08-12T15.57-5ffe4792/html/results.html
...
```
你可以使用选项*--open-browser*来请求自动打开报告。

例如：

``` stata
$ avocado run sleeptest --open-browser
```
在sleeptest测试完成运行之后，你将展示一份漂亮的网页结果报告。

## Machine readable results
另一种测试结果的类型是那些可以用于被其他应用程序检测。几个在标准测试中存在的社区，并且Avocado在理论上能支持几乎所有结果的标准。

创造性的，Avocado支持许多机器可读结果。他们总是产生和存储在结果目录结果类型文件中，但你也可以请求存储到另一个位置。


### xunit

在Avocado中默认机器可读输出是*xunit*。

xunit是一个XML格式文件，它以结构体形式包含测试结果，并且也被其他测试自动化项目使用，例如jenkins。如果你想在标准的运行器输出中让Avocado产生xunit输出，简单的使用方式是：

``` stylus
$ avocado run sleeptest.py failtest.py synctest.py --xunit -
<?xml version="1.0" encoding="UTF-8"?>
<testsuite name="avocado" tests="3" errors="0" failures="1" skipped="0" time="3.5769162178" timestamp="2016-05-04 14:46:52.803365">
        <testcase classname="SleepTest" name="1-sleeptest.py:SleepTest.test" time="1.00204920769"/>
        <testcase classname="FailTest" name="2-failtest.py:FailTest.test" time="0.00120401382446">
                <failure type="TestFail" message="This test is supposed to fail"><![CDATA[Traceback (most recent call last):
  File "/home/medic/Work/Projekty/avocado/avocado/avocado/core/test.py", line 490, in _run_avocado
    raise test_exception
TestFail: This test is supposed to fail
]]></failure>
                <system-out><![CDATA[14:46:53 ERROR|
14:46:53 ERROR| Reproduced traceback from: /home/medic/Work/Projekty/avocado/avocado/avocado/core/test.py:435
14:46:53 ERROR| Traceback (most recent call last):
14:46:53 ERROR|   File "/home/medic/Work/Projekty/avocado/avocado/examples/tests/failtest.py", line 17, in test
14:46:53 ERROR|     self.fail('This test is supposed to fail')
14:46:53 ERROR|   File "/home/medic/Work/Projekty/avocado/avocado/avocado/core/test.py", line 585, in fail
14:46:53 ERROR|     raise exceptions.TestFail(message)
14:46:53 ERROR| TestFail: This test is supposed to fail
14:46:53 ERROR|
14:46:53 ERROR| FAIL 2-failtest.py:FailTest.test -> TestFail: This test is supposed to fail
14:46:53 INFO |
]]></system-out>
        </testcase>
        <testcase classname="SyncTest" name="3-synctest.py:SyncTest.test" time="2.57366299629"/>
</testsuite>
```
注意：

在选项中的破折号-  *-xunit*，它的意思是xunit结果应该标准输出。

### json
[JSON][14]被广泛用于数据格式转换。Avocado插件json输出job信息，类似于xunit的输出插件：

``` stylus
$ avocado run sleeptest.py failtest.py synctest.py --json -
{
    "debuglog": "/home/cleber/avocado/job-results/job-2016-08-09T13.53-10715c4/job.log",
    "errors": 0,
    "failures": 1,
    "job_id": "10715c4645d2d2b57889d7a4317fcd01451b600e",
    "pass": 2,
    "skip": 0,
    "tests": [
        {
            "end": 1470761623.176954,
            "fail_reason": "None",
            "logdir": "/home/cleber/avocado/job-results/job-2016-08-09T13.53-10715c4/test-results/1-sleeptest.py:SleepTest.test",
            "logfile": "/home/cleber/avocado/job-results/job-2016-08-09T13.53-10715c4/test-results/1-sleeptest.py:SleepTest.test/debug.log",
            "start": 1470761622.174918,
            "status": "PASS",
            "test": "1-sleeptest.py:SleepTest.test",
            "time": 1.0020360946655273,
            "url": "1-sleeptest.py:SleepTest.test",
            "whiteboard": ""
        },
        {
            "end": 1470761623.193472,
            "fail_reason": "This test is supposed to fail",
            "logdir": "/home/cleber/avocado/job-results/job-2016-08-09T13.53-10715c4/test-results/2-failtest.py:FailTest.test",
            "logfile": "/home/cleber/avocado/job-results/job-2016-08-09T13.53-10715c4/test-results/2-failtest.py:FailTest.test/debug.log",
            "start": 1470761623.192334,
            "status": "FAIL",
            "test": "2-failtest.py:FailTest.test",
            "time": 0.0011379718780517578,
            "url": "2-failtest.py:FailTest.test",
            "whiteboard": ""
        },
        {
            "end": 1470761625.656061,
            "fail_reason": "None",
            "logdir": "/home/cleber/avocado/job-results/job-2016-08-09T13.53-10715c4/test-results/3-synctest.py:SyncTest.test",
            "logfile": "/home/cleber/avocado/job-results/job-2016-08-09T13.53-10715c4/test-results/3-synctest.py:SyncTest.test/debug.log",
            "start": 1470761623.208165,
            "status": "PASS",
            "test": "3-synctest.py:SyncTest.test",
            "time": 2.4478960037231445,
            "url": "3-synctest.py:SyncTest.test",
            "whiteboard": ""
        }
    ],
    "time": 3.4510700702667236,
    "total": 3
}
```
注意
选项中的破折号- *-json*，它的意思是xunit结果应该标准输出。

记住Avocado JSON结果的格式没有记录的标准。这意味着它将可能发展为接受更新的Avocado特点。一个合理的进展是将会有一个不会打破向后兼容的应用程序，它用于检测它的JSON结果的格式。

### TAP
提供了基本的TAP（Test Anything Protocol）结果，目前是版本12。不像大多数存在的avocado机器可读的输出，这个是改进的（每个测试结果）：

``` stylus
$ avocado run sleeptest.py --tap -
1..1
# debug.log of sleeptest.py:SleepTest.test:
#   12:04:38 DEBUG| PARAMS (key=sleep_length, path=*, default=1) => 1
#   12:04:38 DEBUG| Sleeping for 1.00 seconds
#   12:04:39 INFO | PASS 1-sleeptest.py:SleepTest.test
#   12:04:39 INFO |
ok 1 sleeptest.py:SleepTest.test
```

### Silent result
虽然不是一个想象中的结果格式，一个应用程序可以不做任何事，但从一个Avocado测试job运行中存在了状态代码。例子：

``` stylus
$ avocado --silent run failtest.py
$ echo $?
1
```
在实践中，这通常被用于轮流运行Avocado和检测结果的脚本中：

``` stylus
#!/bin/bash
...
$ avocado --silent run /path/to/my/test.py
if [ $? == 0 ]; then
   echo "great success!"
elif
   ...
```
更多相关信息在段落：*Exit Codes*。

## Multiple results at once

只要他们中一个使用标准输出，你可以立刻拥有多个结果格式。例如，它可以使用在标准输出上使用xunit结果和JSON结果的输出到一个文件中：

``` stylus
$ avocado run sleeptest.py synctest.py --xunit - --json /tmp/result.json
<?xml version="1.0" encoding="UTF-8"?>
<testsuite name="avocado" tests="2" errors="0" failures="0" skipped="0" time="3.64848303795" timestamp="2016-05-04 17:26:05.645665">
        <testcase classname="SleepTest" name="1-sleeptest.py:SleepTest.test" time="1.00270605087"/>
        <testcase classname="SyncTest" name="2-synctest.py:SyncTest.test" time="2.64577698708"/>
</testsuite>

$ cat /tmp/result.json
{
     "debuglog": "/home/cleber/avocado/job-results/job-2016-08-09T13.55-1a94ad6/job.log",
     "errors": 0,
     ...
}
```
但是没有 *-json* 标记的话，你不能做同样的事来通过编程：

``` stylus
$ avocado run sleeptest.py synctest.py --xunit - --json -
Options --json --xunit are trying to use stdout simultaneously
Please set at least one of them to a file to avoid conflicts
```
这基本上是唯一的一个法则，做为一个理智的人，你需要去遵循。


## Exit Codes

Avocado退出代码尽力去表示在执行过程中发生的不同的事情。那意味着退出代码可能是一个组合的代码，是做为一个和ORed一起的独立的退出代码。最终的退出代码可能是没有捆绑的，所以使用者应该对job中发生的事有个很好的认知。

单个独立的退出代码：

 - AVOCADO_ALL_OK(0)
 - AVOCADO_TEST_FAIL(1)
 - AVOCADO_JOB_FAIL(2)
 - AVOCADO_FAIL(4)
 - AVOCADO_JOB_INTERRUPTED(8)
 
 如果一个job以退出编码9完成，例如，它意味着我们至少有一个测试失败并且我们也在某种程度上有工作中断，可能由于超时或者CTRL+c。
 
##  Implementing other result formats
如果你想实现一个新的用于机器或者人类可读的输出格式，你可以参考*avocado.core.plugins.xunit*，用它做为一个开始。

如果你的结果是基于完整的工作结果立刻产生的，你应该创建一个新类，继承自*avocado.core.plugin_interfaces.Result*并且实现*avocado.core.plugin_interfaces.Result.render()*的方法。

但是，如果你的结果实现是在每个测试结果前后连续的输出信息，不得不实现老式接口。创造一个类继承自*avocado.core.result.Result*并且实现所有的公共函数，为每个测试阶段执行动作（写到一个文件或流中）。

你可以查看[Plugin System][15]获取更多的信息，关于如何编写一个插件用于激活和执行新的结果格式。
 
#  Configuration
 Avocado工具有一定的基于指导的默认行为，合理的（我们希望是）猜测关于用户喜欢如何使用他们的系统。当然，不同的人会有不同的需求，并且可能不喜欢我们的默认值，那也是为什么一个配置系统用来处理这些情况。
 
 Avocado配置文件的格式是基于INI（informal）文件设置的，[ INI file ‘specification’][16]，由Python的*configparser*来实施。这个格式是简单而直接的，由许多的键值对构成。以一个基本的Avocado配置文件为例：
 
 

``` stylus
[datadir.paths]
base_dir = ~/avocado
test_dir = /$HOME/Code/avocado/examples/tests
data_dir = /usr/share/avocado/data
logs_dir = ~/avocado/job-results
```
*datadir.paths*部分包含了许多键，他们中所有的都和测试运行器使用的目录有关。*base_dir*是其他重要Avocado目录的基本目录，例如log，数据和测试目录。你也可以通过变量*test_dir*，*data_dir*和*logs_dir*设置这些其他的重要目录。你简单的编辑可用的配置文件就可以做到了。

## Config file parsing order
Avocado开始检测系统配置文件，这是传递给所有Avocado用户的系统根目录，*/etc/avocado/avocado.conf*。然后它会确认是否有一个本地用户配置文件，它通常位于 *~/.config/avocado/avocado.conf* 。解析的顺序很重要，所有系统根目录文件被解析，然后用户配置文件最后被解析，这样用户可以覆盖值。有另一个目录将被其它配置文件扫描， */etc/avocado/conf.d* 。这个目录可以包含插件配置文件，额外的附加配置文件，系统管理员或者avocado开发者可以判断是否有必要放在那。

请注意，对于那些基本目录，如果你选择了一个目录不能被Avocado正确使用（一些目录要求能够读访问，以及其他的，读和写访问），Avocado将退回到一些缺省值。所以如果你的普通用户想要写logs到目录 */root/avocado/logs*，Avocado不会使用这个目录，因为他不能写文件到那个地方。一个新的位置，将会使用默认的目录  *~/avocado/job-results* 代替。

本节中描述的文件顺序仅在安装有avocado的系统中有效。因为人们从git软件库中使用avocado（通常是avocado开发者），没有安装在系统中，记住avocado将在git软件库中读取配置文件，并且忽略系统的宽的配置文件。用命令 *avocado config* 可以让你知道哪个文件真正的被使用。

## Plugin config files

插件也能被配置文件配置。为了不弄乱Avocado的主配置文件，这些插件，如果他们希望如此，可以安装附加的配置文件到  */etc/avocado/conf.d/[pluginname].conf*，这个配置文件在系统宽的配置文件之后会被解析。使用者可以使用本地配置文件覆盖这些值。考虑为假设的插件*salad*配置：

``` stylus
[salad.core]
base = ceasar
dressing = ceasar
```
如果你想，你可以在你的配置文件中修改 *dressing*，在你的本地配置文件中简单的添加 *[salad.core]*新部分，并且为 *dressing* 在那设置一个不同的值。


## Parsing order recap

所以文件解析顺序是：

 - /etc/avocado/avocado.conf
 - /etc/avocado/conf.d/*.conf
 - ~/.config/avocado/avocado.conf
 
 在这个顺序中，意味着你在本地配置文件中的设置将覆盖你在系统宽文件中的定义。
 
 注意：
 请注意如果avocado是从git软件库中启动，在树的配置文件中这些文件将被忽略。这通常会影响人们开发avocado，如果你怀疑，avocado config命令将告诉你在任何情况下哪个文件是正确被使用的。
 
##  Order of precedence for values used in tests

因为你可以使用配置系统去改变测试中的行为和使用的值（思考测试编程的路径，举例），我们确定了下列变量的优先级排序（从最低优先级到最高）：

 - default value (from library or test code)
 - global config file
 - local (user) config file
 - command line switch
 - multiplexer
 
 所以最不重要的值来自于库和测试代码的默认值，经过所有的方式直到multiplexing系统。
 
##  Config plugin
一个配置插件被提供给所有使用者，希望能够快速的查看在所有的Avocado配置中什么被定义了，在所有文件在他们的正确决议的排序被解析之后。例子;

 

``` stylus
$ avocado config
Config files read (in order):
    /etc/avocado/avocado.conf
    $HOME/.config/avocado/avocado.conf

    Section.Key     Value
    runner.base_dir /usr/share/avocado
    runner.test_dir $HOME/Code/avocado/examples/tests
    runner.data_dir /usr/share/avocado/data
    runner.logs_dir ~/avocado/job-results
```
这个命令也展示了你的配置文件被解析的顺序，让你更好地理解正在发生什么。术语Section.Key受启发于*git config --list*的输出。

## Avocado Data Directories
当启动测试时，我们时常留心这些：

 - Locate tests
 - Write logs to a given location
 - Grab files that will be useful for tests, such as ISO files or VM disk images
 
 Avocado有一个模块专门用于查找这些路径，为了避免繁琐的路径起作用，人们不得不提前测试框架。
 
 如果你想为你的测试列出所有的相关目录，你可以使用avocado config –datadir命令去列出这些目录，执行它将给你一个和下面所见的相似的输出：
 

``` stylus
$ avocado config --datadir
Config files read (in order):
    /etc/avocado/avocado.conf
    $HOME/.config/avocado/avocado.conf

Avocado replaces config dirs that can't be accessed
with sensible defaults. Please edit your local config
file to customize values

Avocado Data Directories:
    base  $HOME/avocado
    tests $HOME/Code/avocado/examples/tests
    data  $HOME/avocado/data
    logs  $HOME/avocado/job-results
```

注意到，尽管Avocado会尽力使你在配置文件中提供的配置值，如果它不能写值到提供的位置，它将退回到（我们希望的）合理的默认值，并且我们通知用户有关命令的输出。

相关API文档和这些数据目录的每个意图在 *avocado.data_dir*中，所以强烈推荐你看一看。

 你可以通过修改Avocado配置文件设置你偏爱的数据目录，重要数据目录中唯一例外的是Avocad的tmp目录，用于放置测试使用临时文件。这个目录正常情况下是 */var/tmp/avocado_XXXXX*，（XXXXX实际上是一个随机的字符串）安全的创建在目录 */var/tmp/* 中，除非使用者修改了环境变量 *$TMPDIR*，因为这在unix编程中很常见。
 
下一节的文档解释了如何查看和设置修改Avocado工具和插件行为的配置值。

# Test discovery

 在这个章节中你将学到测试文件是如何被发现和怎么影响过程的。
 
##  The order of test loaders

Avocado支持不同类型的测试，从简单测试开始，简单测试是简单的可执行文件，然后是名为INSTRUMENTED的unittest-like的测试，直到像avocado-vt的测试，它从没有直接映射到存在文件的配置文件中使用复杂矩阵测试。鉴于加载器的数量，从映射在命令行上的测试名称到执行测试可能不总是唯一的。此外，一些人可能总是（或者对于给定运行）想执行单一类型的唯一测试。

为了适应这种行为，你要么可以调整*plugins.loaders*在Avocado中的设置（/etc/avocado/），要么临时使用 *--loaders*（avocado run的选项）选项。
 
 这个选项允许你指定可用的测试加载器的排序和一些参数。你可以指定 loader_name (*file*), loader_name + TEST_TYPE (*file.SIMPLE*)，并且甚至对一些加载器添加一些附加的通过的参数在：之后（*external:/bin/echo -e*）。你也可以提供 *@DEFAULT*，注入到所有保留未使用的加载器的位置。


为了获得关于 *--loaders*的帮助：

``` stylus
$ avocado run --loaders ?
$ avocado run --loaders external:?
```


例子中*--loaders*如何影响产品测试的（手动收集他们中一些错误结果）：

``` stylus
$ avocado run passtest.py boot this_does_not_exist /bin/echo
    > INSTRUMENTED passtest.py:PassTest.test
    > VT           io-github-autotest-qemu.boot
    > MISSING      this_does_not_exist
    > SIMPLE       /bin/echo
$ avocado run passtest.py boot this_does_not_exist /bin/echo --loaders @DEFAULT "external:/bin/echo -e"
    > INSTRUMENTED passtest.py:PassTest.test
    > VT           io-github-autotest-qemu.boot
    > EXTERNAL     this_does_not_exist
    > SIMPLE       /bin/echo
$ avocado run passtest.py boot this_does_not_exist /bin/echo --loaders file.SIMPLE file.INSTRUMENTED @DEFAULT external.EXTERNAL:/bin/echo
    > INSTRUMENTED passtest.py:PassTest.test
    > VT           io-github-autotest-qemu.boot
    > EXTERNAL     this_does_not_exist
    > SIMPLE       /bin/echo
```

# Logging system
这个章节描述在Avocado和Avocado测试中使用的logging系统。

## Tweaking the UI
Avocado使用python的logging系统去产生UI并存储测试的输出。系统非常的灵活并且允许你根据需要调整输出，你可以使用内置流集，或者直接使用流的名字。为了调整他们，你能使用

``` stylus
avocado –show STREAM[:LEVEL][,STREAM[:LEVEL],...]
```
内置流的描述（依照相关的python流列表）：


``` stylus
app:	The text based UI (avocado.app)
test:	Output of the executed tests (avocado.test, “”)
debug:	Additional messages useful to debug avocado (avocado.app.debug)
remote:	Fabric/paramiko debug messages, useful to analyze remote execution (avocado.fabric, paramiko)
early:	Early logging before the logging system is set. It includes the test output and lots of output produced by used libraries. (“”, avocado.test)
```
此外，你可以指定“all”或者“none”去开关所有预定义的流，并且你也能提供定制的python日志流，他们将通过标准的输出。

Warning：
“avocado.app”中更重要的信息或者平等警告日志流总是开启的，并且他们走标准错误。

## Storing custom logs
当你运行一个测试，你也能通过命令：

``` stylus
avocado run –store-logging-stream [STREAM[:LEVEL] [STREAM[:LEVEL] ...]],
```
存储定制的日志流到结果目录，在测试结果目录中每个（独特的）条目将产生$STREAM.$LEVEL文件。

注意：

你不得不指定日志流分离。在这个函数中你不能使用内置流。

注意：

现在定制的流被存储在每个job中，不是每个独立的测试中。


## Paginator

一些子命令（list，plugins，....）支持"paginator"（分页处理）,在兼容的终端上，基本上管道的彩色输出少于简化的浏览生成的输出。使用者可以通过命令*–paginator {on|off}.*来关掉它。


# Test variants - Mux

*Mux*是一种特别的机制，用不同的参数在同样的测试中来产生多元变量。有必要获得一个合适的覆盖率，并且avocado允许使用多种方式定义这些参数，从简单的列举键值对到复杂的树，树是用一种简单的管理方式把所有的可能的变量定义成测试矩阵。

这听起来和Jenkins的稀疏矩阵相似，不同的地方是：avocado替换了filters，但也是可以使用的，avocado允许指定*mux domains*，它是表现数据的一种更好的方式。把数据用树来表示，树创建于每个域中所有可能的变量并且把他们结合在一起。它听起来有点复杂，但实际上它遵循人们经常定义的依赖关系的方式，因此，它使用起来非常容易，甚至在复杂的案子中也很清晰明了。

最好的解释通常来自例子，所以随意的向下滚动到段落[yaml_to_mux plugin][17]，你可以使用默认的mux插件去学习Mux。

## Mux internals
*Mux*是Avocado的核心部分，使用者可以把它看做一个*multiplexed*数据库，我们研究树中这些关联路径的键值对，我们称这些路径为节点*Nodes*。

Mux允许遍历存储在这个数据库中的所有可能组合，这被称作*multiplexation*。Mux找到*variants*，是他们这些带有值的叶节点的列表，然后被加工成*AvocadoParams*。这些参数在测试中可以使用 *self.params* 获得，使用者可以查询当前参数：

``` stylus
self.params.get(key="my_key", path="/some/location/*",
                default="default_value")
```
让我们回到Mux一会。正如前面提到的，它是一个允许存储测试参数的多元变量的数据库。填写数据库，你可以使用的几个命令：

 1. *--mux-inject*  -直接引入命令行参数（看 *avocado multiplex -h*）的值 [path:]key:node
 2. *yaml_to_mux plugin*  -允许解析*yaml*文件到Mux数据库（看[yaml_to_mux plugin][18]）
 3. 定制的插件使用简单的*Mux*API（看mux_api）
 
##  Mux API
Warning
API是内置的，我们可以在任何时候改变它。另一方面我们维持*avocado-virt*插件，使用这个API，所以在这种情况下，我们会提供一个补丁展示必要的改变。


Mux对象被定义在*avocado/core/multiplexer.py*，总是在*avocado.core.parser.py*实例化，并且在*args.mux*中可用。基本的工作流程是：

 - 在*args.mux*中初始化*Mux*
 - 填入数据（*plugins*或者*job*）
 - Multiplex it（多元化）（在*job*中）
 - 遍历所有job测试的所有变量。
 
 一旦对象*Mux*被multiplexed (3)，他将被限制更改数据，为了避免改变已经产生的数据。
 
 你的插件所需的主要的API，我们会尝试尽可能的保持稳定性的是：
 
 
 - mux.is_parsed()  -查明对象是否已经被解析。
 - data_inject(key, value, path=None)  - 选择性地引入键值对到已给的路径（默认是‘/’）
 - data_merge(tree)  - 去合并*avocado.core.tree.TreeNode*像树一样到数据库。
 
 鉴于这些，你应该可以实现任何形式的解析或者参数给予，你应该需要一个。我们喜欢*yaml*并且因此我们实现了一个*yaml_to_mux*插件在文件*avocado/plugins/yaml_to_mux.py*中，并且在它上面我们也描述了*Mux*的工作方式：yaml_to_mux plugin


### Yaml_to_mux plugin

为了有一个好的覆盖率，使用者总是需要使用不同的参数或者各种环境去执行相同测试。Avocado使用术语*Multiplexation*或者*Mux*去形成在不同值的相同测试的多元变量。为了 定义这些变量和值，（[YAML][19]）文件被使用。使用YAML文件的好处是不同范围里的可见分离。甚至每个高级设置仍然是可读的，不像传统稀疏，多空间矩阵的参数。

让我们用一个事例开始（第一列的行号仅仅是为了文档，他们不是muktiplex文件格式的一部分）：


``` stylus
1  hw:
 2      cpu: !mux
 3          intel:
 4              cpu_CFLAGS: '-march=core2'
 5          amd:
 6              cpu_CFLAGS: '-march=athlon64'
 7          arm:
 8              cpu_CFLAGS: '-mabi=apcs-gnu -march=armv8-a -mtune=arm8'
 9      disk: !mux
10          scsi:
11              disk_type: 'scsi'
12          virtio:
13              disk_type: 'virtio'
14  distro: !mux
15      fedora:
16          init: 'systemd'
17      mint:
18          init: 'systemv'
19  env: !mux
20      debug:
21          opt_CFLAGS: '-O0 -g'
22      prod:
23          opt_CFLAGS: '-O2'
```
有键值对key=>value（行：4,6,8,11,13，....），有命名节点（行1,2,3,5,7,9，...）定义范围.也有附加的标记（行：2,9,14,19）用于修改行为。


## Nodes

他们定义key=>value键值对的内容让我们可以轻易辨别这个值是给什么使用的，并且它也可能用对应不同范围的相同键去定义多元的值。

鉴于他们的目的，节点名称的YAML自动化类型转换是关闭的，所以节点名称的值总是被写在yaml文件中（不像values，这边yes转换成true等）。

节点被组织在父子关系中并且他们一起创建一个树。用 *avocado multiplex --tree -m \<file>* 去查看结构体：

``` stylus
┗━━ run
     ┣━━ hw
     ┃    ┣━━ cpu
     ┃    ┃    ╠══ intel
     ┃    ┃    ╠══ amd
     ┃    ┃    ╚══ arm
     ┃    ┗━━ disk
     ┃         ╠══ scsi
     ┃         ╚══ virtio
     ┣━━ distro
     ┃    ╠══ fedora
     ┃    ╚══ mint
     ┗━━ env
          ╠══ debug
          ╚══ prod
```

你能看到*hw*有两个子节点*cpu*和*disk*。所有定义在父节点的参数继承给孩子并且他们的值被扩展和覆盖直到叶节点。叶节点（*intel*，*amd*，*arm*，*scsi*，.....）是最重要的，在multiplexation之后，他们是测试中可用的参数。

## Keys and Values
dict（4,6,8,11）之外的每个值可以被做为祖先节点使用。

每个节点能定义键值对（行4,6,8,11，.....）。此外，每个子节点继承他父辈的值并且结果被节点*environment*调用。

考虑下面的节点结构：

``` stylus
devtools:
    compiler: 'cc'
    flags:
        - '-O2'
    debug: '-g'
    fedora:
        compiler: 'gcc'
        flags:
            - '-Wall'
    osx:
        compiler: 'clang'
        flags:
            - '-arch i386'
            - '-arch x86_64'
```

并且定义的规则是：

 - 标量（Booleans，Numbers 和 Strings）被从根到最后一个节点的值覆盖。
 - 列出附加（到尾部），每当我们从根走到最后一个节点。
 
 
 
 为节点*fedora*和*osx*创建的环境是：
 
 - Node *//devtools/fedora*    environment  *compiler: 'gcc'*   *flags: ['-O2', '-Wall']*
 - Node  *//devtools/osx*        environment   *compiler: 'clang'*  *flags: ['-O2', '-arch i386', '-arch x86_64']*

 注意到在环境中由于键和值得不同的使用方法，我们关闭了键值转换，同事保持它对值的支持。这意味着值可能是任何YAML支持的值，例如，bool，None，list或者自定义的类型，即使键总是一个字符串。
 
 
##  Variants
最后所有的叶被聚集并且转换为参数，更具体到*AvocadoParams*：

``` stylus
setup:
    graphic:
        user: "guest"
        password: "pass"
    text:
        user: "root"
        password: "123456"
```
产物 *[graphic, text]* 。在测试代码中你可以查询的只有那些页。媒介或者根节点是可用的。

上面的例子生成了一个带有路径隔离参数的测试执行。但最强大的multiplexer 功能是它能生成一个multiple变量。这样做的话，你需要标记一个节点，它的后代是被multiplexed标记的。它只有效地返回那时一个子节点的叶。为了生成所有可能的变量，multiplexer创建了这些变量的笛卡尔积：

``` stylus
cpu: !mux
    intel:
    amd:
    arm:
fmt: !mux
    qcow2:
    raw:
```

产生6个变量：

``` stylus
/cpu/intel, /fmt/qcow2
/cpu/intel, /fmt/raw
...
/cpu/arm, /fmt/raw
```
！mux求值是递归的，所以一个变量可以扩展到多个：

``` stylus
fmt: !mux
    qcow: !mux
        2:
        2v3:
    raw:
```

结果：

``` stylus
/fmt/qcow2/2
/fmt/qcow2/2v3
/raw
```
## Resolution order

你能看到叶是测试参数的一部分。它可能会发生这种情况：这些叶中的一些包含了同键不同值的情况。那么你需要确保你的查询能够通过路径区分他们。当路径匹配不同源的multiple结果时，一个异常发生，它不可能去猜测哪个键值是原本想要的。

为了避开这些问题，推荐在测试参数中尽可能使用唯一的名字去避免这些冲突。这样也使得测试中扩展或者混合multiple YAML文件更简单。

因为multiplex YAML文件是框架的一部分，包含了默认的配置，或者像插件配置和高级设置那样服务，它可能通常需要使用非唯一名称。不过记住这些点并且提供合理的路径。

Multiplexer也支持默认路径。默认的是 */run/\** 但能被 *--mux-path* 选项值覆盖，它接收multiple参数。通过提供的路径去分开这些叶。每个查询遍历这些子树并且返回第一个匹配结果。它可能没有解决问题，但它帮助整合你的测试中已经存在的YAML文件：

``` stylus
qa:         # large and complex read-only file, content injected into /qa
    tests:
        timeout: 10
    ...
my_variants: !mux        # your YAML file injected into /my_variants
    short:
        timeout: 1
    long:
        timeout: 1000
```

你想要使用一个已经存在的测试，它使用了 *params.get('timeout', '\*')*。那么你可以使用：*--mux-path '/my_variants/\*' '/qa/\*'*,它将首先查看你的变量。如果没有匹配到，那么将到 */qa/\** 中继续查询。

记住定义在mux-path的总切片路径被用来做相对路径（从\*开始的那些）。


## Injecting files

你可以用任何的YAML文件启动测试：

``` stylus
avocado run sleeptest.py --mux-yaml file.yaml
```
放置*file.yaml*的内容到 */run* 位置，正如前面提到的额，是默认的 *mux-path* 路径。对于大多数简单测试来说，希望的行为是：在默认路径中你的文件是可用的并且你可以安全的使用 *params.get(key)* 。

当你需要放置一个文件到不同的路径，例如当你有两个文件并且你不想内容被合并到一个地方成为一个独立的对象(blob)，你可以通过给一个名字到yaml文件：

``` stylus
avocado run sleeptest.py --mux-yaml duration:duration.yaml
```
*duraion.yaml*的内容被加入到 */run/duration* 。当来自其他文件的keys没有冲突，你仍可以使用*params.get(key)* 并且从默认的路径中重新获取值，唯一的扩展是*duration*中间节点。另一个好处是你可以通过使用相同或者不同的名字或者甚至一个复杂的（相对）路径来合并或者分开multiple文件。

最后但不是最重要的是，高级用户可以加入文件到任何他们想要的路径：

``` stylus
avocado run sleeptest.py --mux-yaml /my/variants/duration:duration.yaml
```
简单的 *params.get(key)* 不会查询这个位置，这可能是测试作者想要的。有几种方式去访问这个值：

 - 绝对路径 *params.get(key, '/my/variants/duration')*
 - 带有通配符的绝对路径*params.get(key, '/my/\*)*  (或者 */\*/duration/\** ....)
 - 设置mux-path  *avocado run ... --mux-path /my/\** 并且使用相对路径
 
 建议给单独的YAML文件使用简单的引入，多个简单的YAML文件使用相对路径的引入并且最终的选项是用来做高级设置的，当你既不能修改YAML文件并且也没有指定测试参数，你需要指定自定义的解析顺序，例如你的插件的参数，需要和你的测试参数分开。
 
##  Multiple files
你可以提供multiple文件。在这样的场景中，最终的树是提供的文件的组合，相同名字的最新节点覆盖前面对应的节点。新节点被添加到一个新的子位置：

 

``` stylus
file-1.yaml:
    debug:
        CFLAGS: '-O0 -g'
    prod:
        CFLAGS: '-O2'

file-2.yaml:
    prod:
        CFLAGS: '-Os'
    fast:
        CFLAGS: '-Ofast'
```
结果：

``` stylus
debug:
    CFLAGS: '-O0 -g'
prod:
    CFLAGS: '-Os'       # overriden
fast:
    CFLAGS: '-Ofast'    # appended
```
包含存在的文件到另一个文件中的另一个给予节点是可能的。通过命令 *!include : $path directive* 可以做到：

``` stylus
os:
    fedora:
        !include : fedora.yaml
    gentoo:
        !include : gentoo.yaml
```

Warning：
由于YAML的特性，在 !include和冒号(:)之间放一个空格死强制的必须遵循。

在!incude中被调用（甚至被嵌套）的YAML文件的定位可以是绝对路径或者相对路径。
整个文件被合并到定义的节点中。

## Advanced YAML tags

有YAML文件相关的附加功能。他们中的大多数需要":"来分隔。再次，在所有这种案子中，在tag和":"之间加空格" "是强制的，否则":"被当做是tag名称的部分，解析失败。

## !include
包含其他文件并注入到节点中，它的规定是：

``` stylus
my_other_file:
    !include : other.yaml
```
*/my_other_file*的内容是解析自文件 *other.yaml* 。它硬编码和 *-m $using:$path*  等效。

相对路径从源文件目录开始。

## !using

预先的节点路径，定义为：

``` stylus
!using : /foo
bar:
    !using : baz
```

*bar* 被放进 *baz* 变成 */baz/bar* ，然后一切都被放进 */foo* 。所以*bar*的最终路径是 */foo/baz/bar* 。


## !remove_node

如果在合并中它已经存在，移除掉。它能用于扩展不相容的YAML文件：

``` stylus
os:
    fedora:
    windows:
        3.11:
        95:
os:
    !remove_node : windows
    windows:
        win3.11:
        win95:
```

从结构中移除windows节点。它不同于filter-out，它是真正从树中移除节点（在所有子节点中），并且它可以被新结构替换就像例子中展示的那样。它移除了windows的所有子节点，然后用新版本的结构替换。

当!remove_node在合并中执行时，当你颠倒顺序，windows没有移除并且你以节点/windows/{win3.11,win95,3.11,95} 结尾。

## !remove_value
它和!remove_node相似，只是仅作用于值。

## !mux
这个节点的子节点将被 multiplexed。这意味着它将返回第一个子叶做为第一个变量，返回第二个子叶做为第二个变量等等。Variants章节中有例子。

## Complete example
让我们再次查看第一个例子：

``` stylus
 1    hw:
 2        cpu: !mux
 3            intel:
 4                cpu_CFLAGS: '-march=core2'
 5            amd:
 6                cpu_CFLAGS: '-march=athlon64'
 7            arm:
 8                cpu_CFLAGS: '-mabi=apcs-gnu -march=armv8-a -mtune=arm8'
 9        disk: !mux
10            scsi:
11                disk_type: 'scsi'
12            virtio:
13                disk_type: 'virtio'
14    distro: !mux
15        fedora:
16            init: 'systemd'
17        mint:
18            init: 'systemv'
19    env: !mux
20        debug:
21            opt_CFLAGS: '-O0 -g'
22        prod:
23            opt_CFLAGS: '-O2'
```
在过滤器被使用后（简单地移除没有匹配的变量），叶节点被集中，并且所有的变量生成：

``` stylus
$ avocado multiplex -m examples/mux-environment.yaml
Variants generated:
Variant 1:    /hw/cpu/intel, /hw/disk/scsi, /distro/fedora, /env/debug
Variant 2:    /hw/cpu/intel, /hw/disk/scsi, /distro/fedora, /env/prod
Variant 3:    /hw/cpu/intel, /hw/disk/scsi, /distro/mint, /env/debug
Variant 4:    /hw/cpu/intel, /hw/disk/scsi, /distro/mint, /env/prod
Variant 5:    /hw/cpu/intel, /hw/disk/virtio, /distro/fedora, /env/debug
Variant 6:    /hw/cpu/intel, /hw/disk/virtio, /distro/fedora, /env/prod
Variant 7:    /hw/cpu/intel, /hw/disk/virtio, /distro/mint, /env/debug
Variant 8:    /hw/cpu/intel, /hw/disk/virtio, /distro/mint, /env/prod
Variant 9:    /hw/cpu/amd, /hw/disk/scsi, /distro/fedora, /env/debug
Variant 10:    /hw/cpu/amd, /hw/disk/scsi, /distro/fedora, /env/prod
Variant 11:    /hw/cpu/amd, /hw/disk/scsi, /distro/mint, /env/debug
Variant 12:    /hw/cpu/amd, /hw/disk/scsi, /distro/mint, /env/prod
Variant 13:    /hw/cpu/amd, /hw/disk/virtio, /distro/fedora, /env/debug
Variant 14:    /hw/cpu/amd, /hw/disk/virtio, /distro/fedora, /env/prod
Variant 15:    /hw/cpu/amd, /hw/disk/virtio, /distro/mint, /env/debug
Variant 16:    /hw/cpu/amd, /hw/disk/virtio, /distro/mint, /env/prod
Variant 17:    /hw/cpu/arm, /hw/disk/scsi, /distro/fedora, /env/debug
Variant 18:    /hw/cpu/arm, /hw/disk/scsi, /distro/fedora, /env/prod
Variant 19:    /hw/cpu/arm, /hw/disk/scsi, /distro/mint, /env/debug
Variant 20:    /hw/cpu/arm, /hw/disk/scsi, /distro/mint, /env/prod
Variant 21:    /hw/cpu/arm, /hw/disk/virtio, /distro/fedora, /env/debug
Variant 22:    /hw/cpu/arm, /hw/disk/virtio, /distro/fedora, /env/prod
Variant 23:    /hw/cpu/arm, /hw/disk/virtio, /distro/mint, /env/debug
Variant 24:    /hw/cpu/arm, /hw/disk/virtio, /distro/mint, /env/prod
```
第一个变量包含：

``` stylus
/hw/cpu/intel/  => cpu_CFLAGS: -march=core2
/hw/disk/       => disk_type: scsi
/distro/fedora/ => init: systemd
/env/debug/     => opt_CFLAGS: -O0 -g
```

第二个包含：

``` stylus
/hw/cpu/intel/  => cpu_CFLAGS: -march=core2
/hw/disk/       => disk_type: scsi
/distro/fedora/ => init: systemd
/env/prod/      => opt_CFLAGS: -O2
```
从这个例子中，你能看到对 */env/debug* 的查询工作仅在第一个变量中，但在第二个变量中什么都没返回。记住当你使用 *!mux* 标记时，查询pre-mux路径时，在这个例子中是 */env/\** 。


## Job Replay

为了使用相同的数据再现一个已给的job，可以在 *run* 命令上使用 *--replay* 选项，通知原本job的hash id被重新执行。hash id可能是局部的，只要对应原本job id的初始字符的提供部分，并且也是独一无二的。或者替代job id，你可以使用字符串 *latest* ，avocado将重新执行最新的job。

让我们看一个例子。首先，用两个urls启动一个简单的job：

``` stylus
$ avocado run /bin/true /bin/false
JOB ID     : 825b860b0c2f6ec48953c638432e3e323f8d7cad
JOB LOG    : $HOME/avocado/job-results/job-2016-01-11T16.14-825b860/job.log
TESTS      : 2
 (1/2) /bin/true: PASS (0.01 s)
 (2/2) /bin/false: FAIL (0.01 s)
RESULTS    : PASS 1 | ERROR 0 | FAIL 1 | SKIP 0 | WARN 0 | INTERRUPT 0
TESTS TIME : 0.02 s
JOB HTML   : $HOME/avocado/job-results/job-2016-01-11T16.14-825b860/html/results.html
```
现在我们能通过命令重新执行job：

``` stylus
$ avocado run --replay 825b86
JOB ID     : 55a0d10132c02b8cc87deb2b480bfd8abbd956c3
SRC JOB ID : 825b860b0c2f6ec48953c638432e3e323f8d7cad
JOB LOG    : $HOME/avocado/job-results/job-2016-01-11T16.18-55a0d10/job.log
TESTS      : 2
 (1/2) /bin/true: PASS (0.01 s)
 (2/2) /bin/false: FAIL (0.01 s)
RESULTS    : PASS 1 | ERROR 0 | FAIL 1 | SKIP 0 | WARN 0 | INTERRUPT 0
TESTS TIME : 0.01 s
JOB HTML   : $HOME/avocado/job-results/job-2016-01-11T16.18-55a0d10/html/results.html
```

replay功能将重新获取原本job的urls，multiplex 树和配置。让我们看另一个例子，现在使用multiplex文件：

``` stylus
$ avocado run /bin/true /bin/false --mux-yaml mux-environment.yaml
JOB ID     : bd6aa3b852d4290637b5e771b371537541043d1d
JOB LOG    : $HOME/avocado/job-results/job-2016-01-11T21.56-bd6aa3b/job.log
TESTS      : 48
 (1/48) /bin/true;1: PASS (0.01 s)
 (2/48) /bin/true;2: PASS (0.01 s)
 (3/48) /bin/true;3: PASS (0.01 s)
 (4/48) /bin/true;4: PASS (0.01 s)
 (5/48) /bin/true;5: PASS (0.01 s)
 (6/48) /bin/true;6: PASS (0.01 s)
 (7/48) /bin/true;7: PASS (0.01 s)
 (8/48) /bin/true;8: PASS (0.01 s)
 (9/48) /bin/true;9: PASS (0.01 s)
 (10/48) /bin/true;10: PASS (0.01 s)
 (11/48) /bin/true;11: PASS (0.01 s)
 (12/48) /bin/true;12: PASS (0.01 s)
 (13/48) /bin/true;13: PASS (0.01 s)
 (14/48) /bin/true;14: PASS (0.01 s)
 (15/48) /bin/true;15: PASS (0.01 s)
 (16/48) /bin/true;16: PASS (0.01 s)
 (17/48) /bin/true;17: PASS (0.01 s)
 (18/48) /bin/true;18: PASS (0.01 s)
 (19/48) /bin/true;19: PASS (0.01 s)
 (20/48) /bin/true;20: PASS (0.01 s)
 (21/48) /bin/true;21: PASS (0.01 s)
 (22/48) /bin/true;22: PASS (0.01 s)
 (23/48) /bin/true;23: PASS (0.01 s)
 (24/48) /bin/true;24: PASS (0.01 s)
 (25/48) /bin/false;1: FAIL (0.01 s)
 (26/48) /bin/false;2: FAIL (0.01 s)
 (27/48) /bin/false;3: FAIL (0.01 s)
 (28/48) /bin/false;4: FAIL (0.01 s)
 (29/48) /bin/false;5: FAIL (0.01 s)
 (30/48) /bin/false;6: FAIL (0.01 s)
 (31/48) /bin/false;7: FAIL (0.01 s)
 (32/48) /bin/false;8: FAIL (0.01 s)
 (33/48) /bin/false;9: FAIL (0.01 s)
 (34/48) /bin/false;10: FAIL (0.01 s)
 (35/48) /bin/false;11: FAIL (0.01 s)
 (36/48) /bin/false;12: FAIL (0.01 s)
 (37/48) /bin/false;13: FAIL (0.01 s)
 (38/48) /bin/false;14: FAIL (0.01 s)
 (39/48) /bin/false;15: FAIL (0.01 s)
 (40/48) /bin/false;16: FAIL (0.01 s)
 (41/48) /bin/false;17: FAIL (0.01 s)
 (42/48) /bin/false;18: FAIL (0.01 s)
 (43/48) /bin/false;19: FAIL (0.01 s)
 (44/48) /bin/false;20: FAIL (0.01 s)
 (45/48) /bin/false;21: FAIL (0.01 s)
 (46/48) /bin/false;22: FAIL (0.01 s)
 (47/48) /bin/false;23: FAIL (0.01 s)
 (48/48) /bin/false;24: FAIL (0.01 s)
RESULTS    : PASS 24 | ERROR 0 | FAIL 24 | SKIP 0 | WARN 0 | INTERRUPT 0
TESTS TIME : 0.29 s
JOB HTML   : $HOME/avocado/job-results/job-2016-01-11T21.56-bd6aa3b/html/results.html
```
我们可以重新执行工作，使用 *$ avocado run --replay latest* ，或则忽略multiplex 文件来重新执行工作，像下面的例子那样：

``` stylus
$ avocado run --replay bd6aa3b --replay-ignore mux
Ignoring multiplex from source job with --replay-ignore.
JOB ID     : d5a46186ee0fb4645e3f7758814003d76c980bf9
SRC JOB ID : bd6aa3b852d4290637b5e771b371537541043d1d
JOB LOG    : $HOME/avocado/job-results/job-2016-01-11T22.01-d5a4618/job.log
TESTS      : 2
 (1/2) /bin/true: PASS (0.01 s)
 (2/2) /bin/false: FAIL (0.01 s)
RESULTS    : PASS 1 | ERROR 0 | FAIL 1 | SKIP 0 | WARN 0 | INTERRUPT 0
TESTS TIME : 0.02 s
JOB HTML   : $HOME/avocado/job-results/job-2016-01-11T22.01-d5a4618/html/results.html
```
他也可能只重新执行给定结果的变量，使用选项 *--replay-test-status* ，看下面的例子：

``` stylus
$ avocado run --replay bd6aa3b --replay-test-status FAIL
JOB ID     : 2e1dc41af6ed64895f3bb45e3820c5cc62a9b6eb
SRC JOB ID : bd6aa3b852d4290637b5e771b371537541043d1d
JOB LOG    : $HOME/avocado/job-results/job-2016-01-12T00.38-2e1dc41/job.log
TESTS      : 48
 (1/48) /bin/true;1: SKIP
 (2/48) /bin/true;2: SKIP
 (3/48) /bin/true;3: SKIP
 (4/48) /bin/true;4: SKIP
 (5/48) /bin/true;5: SKIP
 (6/48) /bin/true;6: SKIP
 (7/48) /bin/true;7: SKIP
 (8/48) /bin/true;8: SKIP
 (9/48) /bin/true;9: SKIP
 (10/48) /bin/true;10: SKIP
 (11/48) /bin/true;11: SKIP
 (12/48) /bin/true;12: SKIP
 (13/48) /bin/true;13: SKIP
 (14/48) /bin/true;14: SKIP
 (15/48) /bin/true;15: SKIP
 (16/48) /bin/true;16: SKIP
 (17/48) /bin/true;17: SKIP
 (18/48) /bin/true;18: SKIP
 (19/48) /bin/true;19: SKIP
 (20/48) /bin/true;20: SKIP
 (21/48) /bin/true;21: SKIP
 (22/48) /bin/true;22: SKIP
 (23/48) /bin/true;23: SKIP
 (24/48) /bin/true;24: SKIP
 (25/48) /bin/false;1: FAIL (0.01 s)
 (26/48) /bin/false;2: FAIL (0.01 s)
 (27/48) /bin/false;3: FAIL (0.01 s)
 (28/48) /bin/false;4: FAIL (0.01 s)
 (29/48) /bin/false;5: FAIL (0.01 s)
 (30/48) /bin/false;6: FAIL (0.01 s)
 (31/48) /bin/false;7: FAIL (0.01 s)
 (32/48) /bin/false;8: FAIL (0.01 s)
 (33/48) /bin/false;9: FAIL (0.01 s)
 (34/48) /bin/false;10: FAIL (0.01 s)
 (35/48) /bin/false;11: FAIL (0.01 s)
 (36/48) /bin/false;12: FAIL (0.01 s)
 (37/48) /bin/false;13: FAIL (0.01 s)
 (38/48) /bin/false;14: FAIL (0.01 s)
 (39/48) /bin/false;15: FAIL (0.01 s)
 (40/48) /bin/false;16: FAIL (0.01 s)
 (41/48) /bin/false;17: FAIL (0.01 s)
 (42/48) /bin/false;18: FAIL (0.01 s)
 (43/48) /bin/false;19: FAIL (0.01 s)
 (44/48) /bin/false;20: FAIL (0.01 s)
 (45/48) /bin/false;21: FAIL (0.01 s)
 (46/48) /bin/false;22: FAIL (0.01 s)
 (47/48) /bin/false;23: FAIL (0.01 s)
 (48/48) /bin/false;24: FAIL (0.01 s)
RESULTS    : PASS 0 | ERROR 0 | FAIL 24 | SKIP 24 | WARN 0 | INTERRUPT 0
TESTS TIME : 0.19 s
JOB HTML   : $HOME/avocado/job-results/job-2016-01-12T00.38-2e1dc41/html/results.html
```

当重新执行工作是在 *--failfast on* 选项上执行，你可以在重新执行的工作中使用选项 *--failfast off* 关掉 *failfast* 。

为了重新执行一个job，avocado在同样的工作结果目录中记录工作数据，在一个内部被命名为 *replay* 的子目录中。如果一个给定的job中没有使用默认路径去记录log，当重新执行时间到来时，我们需要告知log在哪。看下面的例子：

 

``` stylus
$ avocado run /bin/true --job-results-dir /tmp/avocado_results/
JOB ID     : f1b1c870ad892eac6064a5332f1bbe38cda0aaf3
JOB LOG    : /tmp/avocado_results/job-2016-01-11T22.10-f1b1c87/job.log
TESTS      : 1
 (1/1) /bin/true: PASS (0.01 s)
RESULTS    : PASS 1 | ERROR 0 | FAIL 0 | SKIP 0 | WARN 0 | INTERRUPT 0
TESTS TIME : 0.01 s
JOB HTML   : /tmp/avocado_results/job-2016-01-11T22.10-f1b1c87/html/results.html
```

尝试重新执行工作，失败：

``` stylus
$ avocado run --replay f1b1
can't find job results directory in '$HOME/avocado/job-results'
```
在这个案子中，我们不得不通知job结果目录在哪边：


 

``` stylus
$ avocado run --replay f1b1 --replay-data-dir /tmp/avocado_results
JOB ID     : 19c76abb29f29fe410a9a3f4f4b66387570edffa
SRC JOB ID : f1b1c870ad892eac6064a5332f1bbe38cda0aaf3
JOB LOG    : $HOME/avocado/job-results/job-2016-01-11T22.15-19c76ab/job.log
TESTS      : 1
 (1/1) /bin/true: PASS (0.01 s)
RESULTS    : PASS 1 | ERROR 0 | FAIL 0 | SKIP 0 | WARN 0 | INTERRUPT 0
TESTS TIME : 0.01 s
JOB HTML   : $HOME/avocado/job-results/job-2016-01-11T22.15-19c76ab/html/results.html
```

# Job Diff
Avocado Diff插件允许使用者简单地比较两个给定工作的几个方面。基本的用法是：

``` stylus
$ avocado diff 7025aaba 384b949c
--- 7025aaba9c2ab8b4bba2e33b64db3824810bb5df
+++ 384b949c991b8ab324ce67c9d9ba761fd07672ff
@@ -1,15 +1,15 @@

 COMMAND LINE
-/usr/bin/avocado run sleeptest.py
+/usr/bin/avocado run passtest.py

 TOTAL TIME
-1.00 s
+0.00 s

 TEST RESULTS
-1-sleeptest.py:SleepTest.test: PASS
+1-passtest.py:PassTest.test: PASS

 ...
```

Avocado Diff 能比较和创造一个统一的diff：

 - Command line.
 - Job time.
 - Variants and parameters.
 - Tests results.
 - Configuration.
 - Sysinfo pre and post.
 
 仅仅不同内容的部分被包含在结果中。你可以使用 *--diff-filter* 打开或关闭这些内容。请用命令 *avocado diff --help* 查看更多信息。
 
 Jobs的识别可以是Job ID,结果目录，或者键 *latest* 。例如：
 

``` stylus
$ avocado diff ~/avocado/job-results/job-2016-08-03T15.56-4b3cb5b/ latest
--- 4b3cb5bbbb2435c91c7b557eebc09997d4a0f544
+++ 57e5bbb3991718b216d787848171b446f60b3262
@@ -1,9 +1,9 @@

 COMMAND LINE
-/usr/bin/avocado run perfmon.py
+/usr/bin/avocado run passtest.py

 TOTAL TIME
-11.91 s
+0.00 s

 TEST RESULTS
-1-test.py:Perfmon.test: FAIL
+1-examples/tests/passtest.py:PassTest.test: PASS
```

根据同一标准的diff，你也可以生成html（选项--html）diff文件，有选择的，打开你首选的浏览器（选项：*--open-browser*）：

``` stylus
$ avocado diff 7025aaba 384b949c --html /tmp/myjobdiff.html
/tmp/myjobdiff.html
```
如果选项 *--open-browser* 被使用，没有带上 *--html* ，我们将创建一个临时的html文件。

想要用定制的diff工具代替Avocado Diff工具，我们提供选项 *--create-reports* ，然后会创建了两个具有相关内容的临时文件。两个文件名字被打印出来并且使用者能将其拷贝粘贴到定制的diff工具的命令行：

``` stylus
$ avocado diff 7025aaba 384b949c --create-reports
/var/tmp/avocado_diff_7025aab_zQJjJh.txt /var/tmp/avocado_diff_384b949_AcWq02.txt

$ diff -u /var/tmp/avocado_diff_7025aab_zQJjJh.txt /var/tmp/avocado_diff_384b949_AcWq02.txt
--- /var/tmp/avocado_diff_7025aab_zQJjJh.txt    2016-08-10 21:48:43.547776715 +0200
+++ /var/tmp/avocado_diff_384b949_AcWq02.txt    2016-08-10 21:48:43.547776715 +0200
@@ -1,250 +1,19 @@

 COMMAND LINE
 ============
-/usr/bin/avocado run sleeptest.py
+/usr/bin/avocado run passtest.py

 TOTAL TIME
 ==========
-1.00 s
+0.00 s

...
```
# Running Tests Remotely

## Running Tests on a Remote Host

Avocado可以让你在远程机器上使用SSH链接启动测试，你可以在安装Avocado来设置它。

你可以通过命令检测这个功能（一个插件）是否被启用：

``` stylus
$ avocado plugins
...
remote  Remote machine options for 'run' subcommand
...
```

假设这个功能是启用的，在Avocado命令行工具中使用 *run* 命令时，你应该能通过下面的选项：

``` stylus
--remote-hostname REMOTE_HOSTNAME
                      Specify the hostname to login on remote machine
--remote-port REMOTE_PORT
                      Specify the port number to login on remote machine.
                      Default: 22
--remote-username REMOTE_USERNAME
                      Specify the username to login on remote machine
--remote-password REMOTE_PASSWORD
                      Specify the password to login on remote machine
```
假使你用password-less SSH链接（通过SSH keys）设置了虚拟机，你可以正常的使用这些选项：–remote-hostname和–remote-username。


###  Remote Setup
确保你拥有：

 1. Avocado数据包安装。你可以在Getting Start章节中看到更多信息。
 2. 远程机器的IP地址或者完全限定的主机名和SSH端口号。
 3. 在远程机器中你测试的所有先决条件都被安装（gcc,make和其他例如你想编译一个第三方用C写的测试套件）。
 
 有选择地，你可以打开你远程机器的免密码SSH登录。
 
###  Running your test

 一旦远程机器被正确的登录，你可以运行你的测试。例如：
 
 

``` stylus
$ scripts/avocado run --remote-hostname 192.168.122.30 --remote-username fedora examples/tests/sleeptest.py examples/tests/failtest.py
REMOTE LOGIN  : fedora@192.168.122.30:22
JOB ID    : 60ddd718e7d7bb679f258920ce3c39ce73cb9779
JOB LOG   : $HOME/avocado/job-results/job-2014-10-23T11.45-a329461/job.log
TESTS     : 2
 (1/2) examples/tests/sleeptest.py: PASS (1.00 s)
 (2/2) examples/tests/failtest.py: FAIL (0.00 s)
RESULTS    : PASS 1 | ERROR 0 | FAIL 1 | SKIP 0 | WARN 0 | INTERRUPT 0
TESTS TIME : 1.01 s
```

正如你看到的，Avocado将拷贝你的测试文件到远程机器并执行他们。一点额外的logging信息被添加到job概览，主要去区分远端的常规执行。注意到我们没有需要–remote-password 因为SSH key已经被设置。


##  Running Tests on a Virtual Machine
有时你不想直接在你自己的机器上运行一个已给的测试（可能这个测试是危险的，可能你需要在在另一个linux发行版伤，等等）。


 因为这些情境，Avocado让你直接在你的系统上用libvirt定义的虚拟机上运行，你需要正确设置那些已经提供的。
 
 你可以检查功能（一个插件）是否已经被running开启：
 
``` stylus
$ avocado plugins
...
vm      Virtual Machine options for 'run' subcommand
...
```
确保这个功能是开启的，当你在Avocado命令行工具中使用 *run* 命令时，你应该能通过下面的选项：

``` stylus
--vm                  Run tests on Virtual Machine
--vm-hypervisor-uri VM_HYPERVISOR_URI
                      Specify hypervisor URI driver connection
--vm-domain VM_DOMAIN
                      Specify domain name (Virtual Machine name)
--vm-hostname VM_HOSTNAME
                      Specify VM hostname to login. By default Avocado
                      attempts to automatically find the VM IP address.
--vm-username VM_USERNAME
                      Specify the username to login on VM
--vm-password VM_PASSWORD
                      Specify the password to login on VM
--vm-cleanup          Restore VM to a previous state, before running the
                      tests
```

从这些选项中，假设你没有设置虚拟机的password-less SSH链接（通过SSH keys），你可以正常使用 –vm-domain， –vm-hostname 和–vm-username 。

如果你的虚拟机已经安装了 *qemu-guest-agent* ，你可以忽略这个选项 *--vm-hostname* 。Avocado将通过agent探测VM IP。

## Virtual Machine Setup

确保你拥有这些：

 1. 一个带有Avocado数据包的libvirt domain被安装。你可以从*Getting Started*章节查看如何去做的更多信息。
 2. 域IP地址或者完全限定的主机名称。
 3. 在虚拟机中所有测试安装要求（gcc，make和例如你想编译第三方用C写的测试套件）。
 
有选择地，你可以使用短密码SSH登录你的虚拟机。


## Running your test

 一旦虚拟机正确运行，你可以启动你的测试。例如：
 
 

``` stylus
$ scripts/avocado run --vm-domain fedora20 --vm-username autotest --vm examples/tests/sleeptest.py examples/tests/failtest.py
VM DOMAIN : fedora20
VM LOGIN  : autotest@192.168.122.30
JOB ID    : 60ddd718e7d7bb679f258920ce3c39ce73cb9779
JOB LOG   : $HOME/avocado/job-results/job-2014-09-16T18.41-60ddd71/job.log
TESTS     : 2
 (1/2) examples/tests/sleeptest.py:SleepTest.test: PASS (1.00 s)
 (2/2) examples/tests/failtest.py:FailTest.test: FAIL (0.01 s)
RESULTS    : PASS 1 | ERROR 0 | FAIL 1 | SKIP 0 | WARN 0 | INTERRUPT 0
TESTS TIME : 1.01 s
```
正如你能看见的，Avocado将拷贝你的测试文件到libvirt域中并执行他们。一点额外的logging信息将被添加到job概览中，主要来区分在远端的常规执行。注意到我们没有需要–vm-password，因为SSH key已经设置。

## Docker container images

Avocado需要在集成的image中被显示，为了测试执行被正确的执行。在默认的image软件库（*docker.io*）中有一个已经使用的image（*ldoktor/fedora-avocado*）：

``` stylus
$ sudo docker pull ldoktor/fedora-avocado
Using default tag: latest
Trying to pull repository docker.io/ldoktor/fedora-avocado ...
latest: Pulling from docker.io/ldoktor/fedora-avocado
...
Status: Downloaded newer image for docker.io/ldoktor/fedora-avocado:latest
```

## Use custom docker images

使用（开发）Avocado的一种可能的方式是去创建一个带有发展树的docker image 。有一种好的方式可以不打断你的系统来测试你的开发分支。
这样做，你可以遵循一些简单的步骤。从正常获取源码开始：

``` stylus
$ git clone github.com/avocado-framework/avocado.git avocado.git
```
你可以对Avocado做一些改变：

``` stylus
$ cd avocado.git
$ patch -p1 < MY_PATCH
```
最终建立一个docker image：

``` stylus
$ docker build -t fedora-avocado-custom -f contrib/docker/Dockerfile.fedora .
```
并且你可以用你在集成中修改的Avocado运行测试：

``` stylus
$ avocado run --docker fedora-avocado-custom examples/tests/passtest.py
```
## Running your test
确保你的系统被正确设置去启动Docker，包含带有Avocado的一个image，你可以使用一个相似的命令在容器中运行测试：

``` stylus
$ avocado run passtest.py warntest.py failtest.py --docker ldoktor/fedora-avocado --docker-cmd "sudo docker"
DOCKER     : Container id '4bcbcd69801211501a0dde5926c0282a9630adbe29ecb17a21ef04f024366943'
JOB ID     : db309f5daba562235834f97cad5f4458e3fe6e32
JOB LOG    : $HOME/avocado/job-results/job-2016-07-25T08.01-db309f5/job.log
TESTS      : 3
 (1/3) /avocado_remote_test_dir/$HOME/passtest.py:PassTest.test: PASS (0.00 s)
 (2/3) /avocado_remote_test_dir/$HOME/warntest.py:WarnTest.test: WARN (0.00 s)
 (3/3) /avocado_remote_test_dir/$HOME/failtest.py:FailTest.test: FAIL (0.00 s)
RESULTS    : PASS 1 | ERROR 0 | FAIL 1 | SKIP 0 | WARN 1 | INTERRUPT 0
TESTS TIME : 0.00 s
JOB HTML   : $HOME/avocado/job-results/job-2016-07-25T08.01-db309f5/html/results.html
```

## Environment Variables

Avocado操作系统运行远程实例，例如使用远程或者VM插件，远程的环境有一组不同的环境变量。如果你想远程的变量在本地环境中也是可用的，你可以使用run选项 –env-keep。看下面的例子：

``` stylus
$ export MYVAR1=foobar
$ env MYVAR2=foobar2 avocado run passtest.py --env-keep MYVAR1,MYVAR2 --remote-hostname 192.168.122.30 --remote-username fedora
```
通过这么做，MYVAR1和MYVAR2在远端环境中将会可以使用。

 
#  Debugging with GDB
Avocado有两种相互补充的GDB支持：

 - 透明可执行文件在GUN Debugger内可执行。这把标准的和可能未修改的使用了*avocado.utils.proces*APIs的测试文件当做运行进程。通过使用命令行选项，可执行文件在GDB上运行。这允许使用者和GDB相互交互，但是对于测试本身而言，所有事情都非常透明。
 - APIs *avocado.utils.gdb* 允许测试和GDB相互交互，包括设置一个可执行文件去运行，设置断点或者其他类型的命令。这需要一个测试方法和API。

Tip
即使这个章节描述了Avocado GDB功能的使用，允许在Avocado测试中进行线上的二进制调试，它也可能通过使用工具[rr][20]线下调试应用程序。Avocado使用wrapper脚本（使用 *--wrapper* ）的例子传输达到目的。

## Transparent Execution of Executables

 这个功能添加了一些命令行选项到Avocado*run*命令：
 
``` stylus
$ avocado run --help
...
GNU Debugger support:

  --gdb-run-bin EXECUTABLE[:BREAKPOINT]
                        Run a given executable inside the GNU debugger,
                        pausing at a given breakpoint (defaults to "main")
  --gdb-prerun-commands EXECUTABLE:COMMANDS
                        After loading an executable in GDB, but before
                        actually running it, execute the GDB commands in the
                        given file. EXECUTABLE is optional, if omitted
                        COMMANDS will apply to all executables
  --gdb-coredump {on,off}
                        Automatically generate a core dump when the inferior
                        process received a fatal signal such as SIGSEGV or
                        SIGABRT
...
```

从你希望使用的 *--gdb-run-bin* 开始，像下面例子展示的那样。

### Example
最简单的方式是运行：*avocado run --gdb-run-bin=doublefree examples/tests/doublefree.py*，用在GDB server里的*doublefree*包裹了每个执行的可执行文件并且停在执行的进入点。

你可以随意的使用 *--gdb-run-bin=doublefree:$breakpoint*（例如：*doublefree:1*）指定单一的断点或者仅仅*doublefree:*，当中断发生时就停止。（例如：SIGABRT）。

值得一提的是当断点没有达到，测试没有任何中断结束。这是有帮助的，在的代码中当你识别到你不应该获得的区域，或者有些地方让你感兴趣并且你能在产品和GDB变量中运行你的代码。如果在很长一段时间之后，你到达了这个地点，测试通知你并且你能研究这个问题。这在*examples/tests/doublefree_nasty.py*测试中演示。为了显示Avocado的功能，运行这个测试：

``` stylus
avocado run --gdb-run-bin=doublefree: examples/tests/doublefree_nasty.py --gdb-prerun-commands examples/tests/doublefree_nasty.py.data/gdb_pre --mux-yaml examples/tests/doublefree_nasty.py.data/iterations.yaml
```
这个命令对测试做了100次迭代执行，而你在文件*examples/tests/doublefree_nasty.py.data/gdb_pre*（你可以指定GDB支持的任何东西，不只有中断点）中设置了所有中断点。

正如你能看见的，这个测试正常通过，但一旦进入有问题的区域就通不过。想象以下，这是很难发现的（依赖于HW注册....）并且这是一种组合常规测试的方式并且调试你代码中很难获得部分的可能性。

### Caveats
现在，当使用Avocado GDB插件时，也就是当你使用 *–gdb-run-bin* 选项时，有一些说明你应该知道：

 - 目前和Avocado的–output-check-record功能不兼容
 - 没有方式去执行适当的输入过程，也就是去控制基本输入
 - 基本错误的内容和gdbserver产生的基本错误内容混合了（因为他们实际上是同一件事）
 
 但是，你仍然可以依赖基本输出进程，以这个虚构的测试为例：
 

``` stylus
from avocado import Test
from avocado.utils import process

class HelloOutputTest(Test):

    def test(self):
        result = process.run("/path/to/hello", ignore_status=True)
        self.assertIn("hello\n", result.stdout)
```

是否在GDB下执行，result.stdout 的行为和内容和期待的一样。



### Reasons for the caveats
有两个基本的原因需要提示说明：

 - Avocado 的GDB特征的架构
 - GDB的自身行为和局限

当使用Avocado GDB插件时，即使用选项， –gdb-run-bin，Avocado透明地运行一个gdbserver实例并且通过一个gdb进程控制它。当一个给定的事件发生，说明一个断点到达，它断开了它自身来自server上的gdb连接，并且允许使用者去使用一个标准的gdb去连接gdbserver。这提供了一种自然的无缝的用户体验。

但是，gdbserver在这点上有些限制，包括：

 - 无法设置一个控制tty
 - 不能区分它自身的错误内容和应用程序运行产生的
 
 这些限制在Avocado和GDB上都在解决，并且将来在Avocado的版本中被解决。
 
##  Workaround
如果你正在运行的应用程序做为你测试的一部分，可以从可供选择的来源（包括设备，文件或者网卡）读取输入并且同样产生输出，那么不应该被进一步限制。

## GDB support and avocado-virt
另一个目前的限制是avocado-virt的使用和avocado GDB支持。

透明调试支持的API目前被限制到[avocado.utils.process.run()][21]并且没有覆盖[ avocado.utils.process.SubProcess][22]类的高级使用。avocado-virt扩展，虽然使用了 avocado.utils.process.SubProcess类在后台去执行qemu。

这些限制将在avocado和avocado-virt的未来版本中被解决。

## *avocado.utils.gdb* APIs

Avocado的GDB模块，提供了三个主要的类来让一个测试作者和gdb进程，gdbserver进程交互，并且使用GDB远端协议和一个远程目标交互。


请参考[avocado.utils.gdb][23]来获取更多信息。

### Example
看一看*examples/tests/modify_variable.py*测试：

``` stylus
def test(self):
    """
    Execute 'print_variable'.
    """
    path = os.path.join(self.srcdir, 'print_variable')
    app = gdb.GDB()
    app.set_file(path)
    app.set_break(6)
    app.run()
    self.log.info("\n".join(app.read_until_break()))
    app.cmd("set variable a = 0xff")
    app.cmd("c")
    out = "\n".join(app.read_until_break())
    self.log.info(out)
    app.exit()
    self.assertIn("MY VARIABLE 'A' IS: ff", out)
```

你能看见我们调用[avocado.utils.gdb.GDB][24]替换了运行执行使用的*process.run*。这让我能够与GDB进行自动化的交互，设置断点，执行命令，查询输出。

当你检查输出（*--show-job-log*），你可以看到尽管声明变量为0，ff被引入并且被替代打印。

# Wrap executables run by tests
Avocado允许可执行的instrumentation以一种透明地方式通过一个测试被运行。使用者指定一个脚本（"the wrapper"）用来运行一个被测试调用的实际程序。
如果instrumentation 脚本能够被正确的实现，它不应该干涉测试行为。也就是wrapper应该避免改变原始执行文件的返回状态，标准输出和错误信息。

用户应该明确哪一个程序被包裹（用一个shell-like glob），或者被略去，一个全局的wrapper将应用到被测试调用的所有程序。


## Usage

这个功能做为一个插件来实施，添加 -wrapper选项到Avocado run命令。想要更详细的解释，请查阅Avocado man page。
 
以透明方式运行strace做为一个wrapper的例子

``` stylus
#!/bin/sh
exec strace -ff -o $AVOCADO_TEST_LOGDIR/strace.log -- $@
```
让所有程序从被 *~/bin/my-wrapper.sh* 包裹的 *test.py* 开始：

``` stylus
$ scripts/avocado run --wrapper ~/bin/my-wrapper.sh tests/test.py
```

只拥有被 *~/bin/my-wrapper.sh* 包裹的 *my-binary* ：

``` stylus
$ scripts/avocado run --wrapper ~/bin/my-wrapper.sh:*my-binary tests/test.py
```
## Caveats

 - 不能同时使用GDB调试（–gdb-run-bin）和wrappers（–wrapper）。这两个选项相互排斥。
 - 你只能设置一个（全局）wrapper。如果你需要的功能在两个wrappers中，你不得不把这些组合进一个单独的wrapper脚本中。
 - 仅用[avocado.utils.process][25]APIs（并且其它的API模块利用它，像mod：avocado.utils.build）运行的可执行文件被这个功能影响。

# Plugin System
Avocado有一个插件系统，可以被用来以一种纯净的方式来扩展avocado。
## Listing plugins
avocado命令行工具有一个內建的命令*plugins*，可以让你列出所有可用的plugins。使用方法非常简单：

``` stylus
$ avocado plugins
Plugins that add new commands (avocado.plugins.cli.cmd):
exec-path Returns path to avocado bash libraries and exits.
run       Run one or more tests (native test, test alias, binary or script)
sysinfo   Collect system information
...
Plugins that add new options to commands (avocado.plugins.cli):
remote  Remote machine options for 'run' subcommand
journal Journal options for the 'run' subcommand
...
```

既然插件（通常很小）和python代码捆绑在一起，如果python代码因为任何原因损坏，插件可能加载失败。例子：

``` stylus
$ avocado plugins
Failed to load plugin from module "avocado.plugins.exec_path": ImportError('No module named foo',)
Plugins that add new commands (avocado.plugins.cli.cmd):
run       Run one or more tests (native test, test alias, binary or script)
sysinfo   Collect system information
...
```
## Writing a plugin
想要更好的理解Avocado插件时如何工作的那么可以创建一个。让我们使用另一个以前的最爱，主题“print hello world”。

### Code example
假设你想写一个插件为了添加一个新的次命令到测试运行器，*hello*。这是你该如何做的：

``` stylus
from avocado.plugins.base import CLICmd


class HelloWorld(CLICmd):

    name = 'hello'
    description = 'The classical Hello World! plugin example.'

    def run(self, args):
        print(self.description)
```
正如你看见的，这个插件继承自*avocado.plugins.base.CLICmd*。这个特定的基类允许为Avocado CLI工具创建新命令。唯一被强制实施的方法是*run*并且它是插件的主要进入点。在这个代码例子中，它将简单的打印插件的描述。

### Registering Plugins

Avocado利用[Stevedore][26]库区加载激活插件。Stevedore自身使用[setuptools][27]和它的[entry points ][28]去注册并找到python对象。所以让你的新插件对Avocado可见，你需要基于文件setup.py添加你的setuptools，像这样：

``` stylus
setup(name='mypluginpack',
...
entry_points={
   'avocado.plugins.cli': [
      'hello = mypluginpack.hello:HelloWorld',
   ]
}
...
```
然后，通过运行 *$ python setup.py install* 或者 *$ python setup.py develop* ，你的插件应该对Avocado可见了。

### Fully qualified named for a plugin
前面提到的插件注册表，（setuptools 和它的 entry points）对于给定的python安装时全局的。Avocado使用命名空间前缀 *avocado.plugins.* 来避免名字和其它软件冲突。现在，在Avocado自身内部，不需要保留使用前缀 *avocado.plugins.* 。

举个例子，Job Pre/Post 插件被定义在 *setup.py* 文件中：

``` stylus
'avocado.plugins.job.prepost': [
   'jobscripts = avocado.plugins.jobscripts:JobScripts'
]
```
setuptools entry point命名空间由提到的*avocado.plugins.*组成，它后面跟着Avocado插件的类型，在这个案子中是：*job.prepost*。

在avocado自身内，一个插件的完全限定名称是一个插件类型，例如*job.prepost*串联到在 entry point自身定义使用的名字，在这个案子中是，*jobscripts*。
总结，仍然使用相同的例子，完全限定的Avocado插件名称将是*job.prepost.jobscripts*。

### Disabling a plugin

即使一个插件在 *setuptools entry points* 下可以被安装注册，在Avocado中它能明确的被关闭。
要这样做的可用机制是在Avocado配置文件的 *plugins* 空间中添加一个入口到 *disable* 键。例子：

``` stylus
[plugins]
disable = ['cli.hello', 'job.prepost.jobscripts']
```
Avocado禁用插件的确切影响取决于插件的类型。例如，禁用的插件类型是 *cli.cmd*，在Avocado命令行应用中插件的命令实现将不再可用。现在禁用一个 *job.prepost* 插件，在jobs执行前后这些都不将执行。

### Wrap Up

我们已经简要的讨论了Avocado插件的制作。我们推荐阅读[ Stevedore documentation][29]并且看一看各种插件接口定义的*avocado.plugins.base*模块。

一些插件例子在[ Avocado source tree][30]中是可用的，在*examples/plugins*目录下。

最终，探索真正的插件附带[avocado.plugins][31]中的Avocado，是最终的“documentation”源。

# Reference Guide
本指南提供Avocado基本设计后它的内部构建的信息。

## Job, test and identifiers
### Job ID
Job ID是一个随机的SHA1字符串，用来唯一标识一个已给的工作。

SHA1字符串的完整形式是用于一个job的最好参照。

``` stylus
$ avocado run sleeptest.py
JOB ID     : 49ec339a6cca73397be21866453985f88713ac34
...
```
但在一些地方一个更短的版本也在使用，例如job results的定位：

``` stylus
JOB LOG    : $HOME/avocado/job-results/job-2015-06-10T10.44-49ec339/job.log
```
### Test References

一个测试参考是一个字符串，通过Avocado Test 解释器被进一步分解为（解释为）一个或者多个测试。一个给定的解释器插件被用来免费的解释一个test reference，它对Avocado的其它组件是完全抽象的。
Note：
 Test References 到 tests的映射可能被命令行开关类似 –external-runner影响，它完全改变了已给字符串的意思。
 
### Test Name
一个测试名称是一个随意的长字符串，明确的指向一个单一的测试来源。换句话说Avocado Test Resolver，配置一个特定的工作，应该返回一个且只有一个测试做为这个名字的解释。

这个名字可能有必要特定的让它唯一。因此它能包含任意数目的变量，前缀，后缀，标签等等。它所有都依赖于用户的参数选择，通过测试解析器和job的内容都被Avocado支持。

当解决Test References，Test Resolver的输出应该总是一个明确的测试名称（对一个特定的job）的列表。

注意到虽然测试名字必须是独一无二的，在一个job内一个测试可能不止一次的运行。

按照定义，一个Test Name是一个Test Reference，但是彼此不一定是正确的，后者可以代表不止一次测试。

###  Variant IDs

multiplexer组件创建不同的变量（被称为“variants”）集，允许测试在他们每个中单独运行。

一个Variant ID是被multiplexer创建来标识每个变量的一个随机的抽象的字符串。在一个集合内对每个变量来说他应该是唯一的。换句话说，multiplexer生成一个变组量，被独一无二的IDs标识。

更简单的实现方式是multiplexer使用连续的整数做为变量IDs。一个复杂的实现能产生带有更多语义，可能表示其内容的变量IDs。

Note
multiplexer 仅支持序列化的变量IDs。

### Test ID
一个test  ID是一个字符串，在一个job的内容中唯一的标识一个测试。当考虑一个单一的job，没有两个相同ID的测试。
一个test ID应该概述测试名称和变量ID，允许直接的标识一个测试。换句话说，通过查看测试ID，应该能识别出：

 - 测试名字是什么
 - 被用于运行测试的变量是什么（如果有）
 
 
当考虑一个特定的job之外的东西时，Test IDs不需要保持它的独特属性，但两个运行在完全相同环境中的相同jobs应该产生一组相同的Test IDs。

语法：

``` stylus
<unique-id>-<test-name>[;<variant-id>]
```
测试名字的例子：

``` stylus
'/bin/true'
'/bin/grep foobar /etc/passwd'
'passtest.py:Passtest.test'
'file:///tmp/passtest.py:Passtest.test'
'multiple_tests.py:MultipleTests.test_hello'
'type_specific.io-github-autotest-qemu.systemtap_tracing.qemu.qemu_free'
```
## Test Types
Avocado在它的最简单配置中能运行两种不同类型的测试。你可以在一个单独的job中混合搭配这些。

### Instrumented

这些用python或者BASH编写的测试，用带有Avocado帮助的Avocado测试API。
更确切的说，python文件必须包含一个继承自*avocado.test.Test*的类。这意味着用python书写的可执行文件不总是一个instrumented 测试，也可能是一个简单测试。

instrumented 测试允许作者更好的控制进程包括logging，测试结果状态，和其它更复杂测试的API。

测试状态*PASS*, *WARN*, *START* 和 *SKIP*被认为是成功的建立。*ABORT*, *ERROR*, *FAIL*, *ALERT*, *RUNNING*, *NOSTATUS* 和 *INTERRUPTED*被认为是失败的。

### Simple

在你盒子中的任何可执行文件。PASS/FAIL的标准是可执行文件的返回代码。如果他返回0，则测试PASS，如果返回其它任何值，则FAIL。


## Test Statuses
Avocado坚持下面测试状态的定义：

 - `PASS`  ：测试通过，意味着测试的所有条件都通过。
 - `FAIL`    ：测试失败，意味着测试中至少一个条件是失败的。理想的是，意味着软件中的一个问题被测试发现。
 - `ERROR`：在测试执行期间一个错误发生。这可能发生的是，例如，如果在测试运行器，在它的库文件中有一个bug或者如果一个资源意外中断。测试中未捕获的异常也会导致这种状态。
 - `SKIP`    ：测试运行器决定要求一个测试不运行。这可能发生的一些情况，例如在测试环境中缺少要求或者一个job超时。
##  Libraries and APIs

 Avocado的库文件和APIs是Avocado的很大一部分。
 
 但为了避免有任何问题，你应该理解Avocado的库文件部分是测试文件作者和他们各自的API稳定性的承诺。
###  Test APIs
基本上当用python写测试文件并且打算利用任何实用的库时，你应该使用测试的APIs。

Test APIs在avocado的主模块中，并且它的最重要的成员是 *avocado.Test* 类。通过继承来符合 *avocado.Test* 的API，你可以使用所有的工具库。

Test  APIs 是一个Avocado主要版本稳定性的保证。那意味着，因为测试API的改变，对于一个已经给定版本的Avocado的测试的书写不应该破坏后面的小版本。


### Utility Libraries
在 avocado.utils的命名空间中有许多实用工具库。这些都非常普遍，可以帮助加快你的测试的开发。
工具库在许多小版本中可能有不兼容的改变，但在稳定版中都是好的。如果一个已给的工具库的改变导致测试中断，它将首先被记录和弃用，并且在随后的小版本中它将被改变。
这意味着Avocado更新到小版本后，你应该查看Avocado的发布信息，看哪些改变对你的测试有影响。

### Core (Application) Libraries
最终，在 avocado.core下的一切是应用程序基础设置的一部分，并且不应该被测试使用。

扩展和插件能使用核心库，但API的稳定性不能保证稳定在任何级别。

## Test Resolution
当你使用测试运行器，你将频繁的给文件提供路径，检查，根据内容才去行动。下图显示了Avocado分析一个文件并且决定怎么处理：

![tupian][32]

重要的是要注意检查机制是安全的（也就是在发现和检查阶段，python类和文件没有真正加载和执行）。由于实际上Avocado没有真正的加载代码和类，自省是简单的并且不会去调试测试模块，在代码中你想要列出和运行丢失的加载和各种调试。我们推荐仅仅运行你信任的源码的测试，在你的测试开发进程中静态检测和复查的使用。


由于简单测试的检查机制，avocado不会识别出继承自avocado.Test的类，请参照* Writing Avocado Tests*的文档，使用标签功能去把派生类标记为avocado测试类。

## Results Specification

在一个机器上执行测试，job结果在 *[job-results]/job-[timestamp]-[short job ID]* 下是可用的， *logdir* 被配置为Avocado的log目录（看数据目录插件），并且目录名字包含一个时间戳，例如 *job-2014-08-12T15.44-565e8de* 。一个典型的结果目录如下所见：

``` stylus
$HOME/avocado/job-results/job-2014-08-13T00.45-4a92bc0/
├── id
├── jobdata
│   ├── args
│   ├── cmdline
│   ├── config
│   ├── multiplex
│   ├── pwd
│   └── urls
├── job.log
├── results.json
├── results.xml
├── sysinfo
│   ├── post
│   │   ├── brctl_show
│   │   ├── cmdline
│   │   ├── cpuinfo
│   │   ├── current_clocksource
│   │   ├── df_-mP
│   │   ├── dmesg_-c
│   │   ├── dmidecode
│   │   ├── fdisk_-l
│   │   ├── gcc_--version
│   │   ├── hostname
│   │   ├── ifconfig_-a
│   │   ├── interrupts
│   │   ├── ip_link
│   │   ├── ld_--version
│   │   ├── lscpu
│   │   ├── lspci_-vvnn
│   │   ├── meminfo
│   │   ├── modules
│   │   ├── mount
│   │   ├── mounts
│   │   ├── numactl_--hardware_show
│   │   ├── partitions
│   │   ├── scaling_governor
│   │   ├── uname_-a
│   │   ├── uptime
│   │   └── version
│   ├── pre
│   │   ├── brctl_show
│   │   ├── cmdline
│   │   ├── cpuinfo
│   │   ├── current_clocksource
│   │   ├── df_-mP
│   │   ├── dmesg_-c
│   │   ├── dmidecode
│   │   ├── fdisk_-l
│   │   ├── gcc_--version
│   │   ├── hostname
│   │   ├── ifconfig_-a
│   │   ├── interrupts
│   │   ├── ip_link
│   │   ├── ld_--version
│   │   ├── lscpu
│   │   ├── lspci_-vvnn
│   │   ├── meminfo
│   │   ├── modules
│   │   ├── mount
│   │   ├── mounts
│   │   ├── numactl_--hardware_show
│   │   ├── partitions
│   │   ├── scaling_governor
│   │   ├── uname_-a
│   │   ├── uptime
│   │   └── version
│   └── profile
└── test-results
    └── tests
        ├── sleeptest.py.1
        │   ├── data
        │   ├── debug.log
        │   └── sysinfo
        │       ├── post
        │       └── pre
        ├── sleeptest.py.2
        │   ├── data
        │   ├── debug.log
        │   └── sysinfo
        │       ├── post
        │       └── pre
        └── sleeptest.py.3
            ├── data
            ├── debug.log
            └── sysinfo
                ├── post
                └── pre

22 directories, 65 files
```
从中你看到了什么，结果目录有：

 1. 一个人类可读的*id*在顶层，值是job SHA1。
 2. 一个人类可读的*job.log*在顶层，用人类可读的任务记录表示。
 3. 带有子目录的*jobdata*，包含了有关job的机器可读数据。
 4. 一个机器可读的*results.xml*和*results.json*在顶层，是用xUnit/json格式的job信息总结。
 5. 一个顶层*sysinfo*目录，带有子目录*pre*，*post*和*profile*，分别的存储信息文件pre/post/在工作期间。
 6. 带有子目录的*test-results*，包含了大量的子目录（系统友好的测试文件ids）。这些测试ids代表了测试执行结果的实例。

 
 ###  Test execution instances specification

 实例应该有：
 

 1. 一个顶层的人类可读的*job.log*，带有调试信息。
 2. 一个*sysinfo*子目录，带有子目录*pre*和*post*，分别存储系统信息文件pre test和post test。
 3. 一个*data*子目录，如有必要测试可以输出许多文件。


## Job Pre and Post Scripts

Avocado用插件（默认安装）传输，允许在job的实际执行前后运行脚本。使用者可以确认，当一个给定的"pre"脚本去运行，job中没有测试在运行，并且当"post"脚本在运行，一个给定的job中所有测试都完成了运行。

### Configuration
默认情况下，脚本目录位置是：

``` stylus
/etc/avocado/scripts/job
```
在那个目录中，有一个目录给pre-job脚本：

``` stylus
/etc/avocado/scripts/job/pre.d
```
并且一个给post-job脚本：

``` stylus
/etc/avocado/scripts/job/post.d
```

有关Pre/Post Job脚本的所有配置都被放在*avocado.plugins.jobscripts*配置部分。为了改变pre-job脚本的位置，你的配置文件应该像这个一样：

``` stylus
[plugins.jobscripts]
pre = /my/custom/directory/for/pre/job/scripts/
```
因此，为了改变post-job脚本的目录，你的配置文件应该看起来像这样：

``` stylus
[plugins.jobscripts]
post = /my/custom/directory/for/post/scripts/
```
两个其他配置选项可用相同的部分：

 - *warn_non_existing_dir* ：给予警告，如果配置（或者默认）目录不存在，为pre或者post脚本设置了。
 - *warn_non_zero_status* ：如果一个给定的脚本（pre或者post）以非0状态退出，给予警告。
 
###  Script Execution Environment

 所有脚本用一些环境变量设置都运行在单独的进程中。在你的脚本中这些可以被你用你希望的任何方式使用：
 

 - *AVOCADO_JOB_UNIQUE_ID*：唯一的job-id。
 - *AVOCADO_JOB_STATUS*：目前的job状态
 - *AVOCADO_JOB_LOGDIR*：文件系统位置，把logs和各种其他文件给一个job运行。
 

Note：即使这些变量都应该被设置，对脚本而言它是检测的一个很好的实践，如果在使用它们的值之前它们都被设置。这可以防止意外的行为，例如，写到当前工作目录替代写到*AVOCADO_JOB_LOGDIR*，如果这个值没有设置。

最终，在Pre/Post脚本中的任何错误不会改变相应的工作状态。

## Job Cleanup
它可能注册了一个回调函数，当所有测试完成时将被调用。这有效的允许了一个测试job去清除一些它可能遗留的状态。

目前，这个功能没有打算让测试文件编写者使用，但它看上去相识做为一个Avocado的扩展被利用。

为了注册一个回调函数，你的代码应该用一种特别的格式把一个信息放到 "runner queue"。Avocado测试运行器代码会理解这个信息包含了一个（序列化）函数，一旦测试完成运行将会被调用。
例子：

``` stylus
from avocado import Test

def my_cleanup(path_to_file):
   if os.path.exists(path_to_file):
      os.unlink(path_to_file)

class MyCustomTest(Test):
...
   cleanup_file = '/tmp/my-custom-state'
   self.runner_queue.put({"func_at_exit": self.my_cleanup,
                          "args": (cleanup_file),
                          "once": True})
...
```
在*my_cleanup*函数中结果被用位置参数*cleanup_file*调用。

因为*once*被设置为*True*，只有一个独一无二的函数，位置参数和关键字参数的组合被注册，不管多少次他们在试图注册。获得更多的信息可查看[avocado.utils.data_structures.CallbackRegister.register()][33]。

# Contribution and Community Guide
有用的指引如何参加Avocado社区和贡献。
## Hacking and Using Avocado

 从版本0.31.0，我们的插件系统需要Setuptools entry points去注册。如果你袭击Avocado并且想要使用相同的，可能修改的，源代码去运行你的测试和实验，你可能需要做一个附加的步骤：

``` stylus
$ make develop
```
在POSIX系统上，这将创建一个"egg link"到你的在 “$HOME/.local/lib/pythonX.Y/site-packages”目录下的原始的源代码树。然后，在你的源代码树上，一个"egg info"目录将被创建，除此之外，包含前面提到的Setuptools entry points。这像一个符号链接一样工作，所以你只需要运行一次（除非你添加了一个新的进入点，那么你需要重新运行它并让它起作用）。

Avocado支持各种插件，分布出来做为一个独立的项目，例如"avocado-vt"和"avocado-virt"。为了用来自源码（开箱即用的安装版本）的Avocado正确的工作，这些需要被布置和链接。为了简化这个，你可以使用来自Avocado主要项目的requirements-plugins去安装插件的需求，使链接连接，并且开发插件。工作流程是：

``` stylus
$ cd $AVOCADO_PROJECTS_DIR
$ git clone $AVOCADO_GIT
$ git clone $AVOCADO_PROJECT2
$ # Add more projects
$ cd avocado    # go into the main avocado project dir
$ make requirements-plugins
$ make link
```

你应该看到每个目录的进程和状态。

## Contact information

 - Avocado-devel邮件列表：https://www.redhat.com/mailman/listinfo/avocado-devel
 - Avocado  IRC 频道：irc://irc.oftc.net/#avocado

##  Contributing to Avocado
Avocado使用github和github pull请求开发模块。你可以在这儿[here][34]找到一个如何使用github pull requests的引导。你发送的每个推送请求将被[Travis CI][35]测试并且复审你的推送请求。

对于不喜欢用github开发模块的人来说，你可以选择发送补丁到Mailing List，遵循一个在开源开发社区更传统的工作流程。补丁会被Mailing List复审，你应该会选择这种方式。然后维护者将收集补丁，把他们集成在一个分支上，然后这些分支会被做为一个github推送请求递交。这个过程尽力的确保每个贡献的补丁在被认为是一个好的之前经过CI jobs。

### Git workflow

 - 在github上分叉软件库
 - 在你的分支中克隆：

``` stylus
$ git clone git@github.com:<username>/avocado.git
```

 - 进入目录：

``` stylus
$ cd avocado
```

 - 创建一个*remote*，指向upstream：

 

``` stylus
$ git remote add upstream git@github.com:avocado-framework/avocado.git
```

 - 在git中配置你的名字和邮件：
 
``` stylus
$ git config --global user.name "Your Name"
$ git config --global user.email email@foo.bar
```
 - Golden tip：永远不要工作在本地分支master上。创建一个本地分支并且检查它：

``` stylus
$ git checkout -b my_new_local_branch
```

 - 代码并且提交你的改变：

``` stylus
$ git add new-file.py
$ git commit -s
# or "git commit -as" to commit all changes
```

 - 写一个好的提交信息，指出动机，你解决的问题。通常你应该在提交的信息中解释三点：动机，方法，影响：

``` stylus
header          <- Limited to 72 characters. No period.
                <- Blank line
message         <- Any number of lines, limited to 72 characters per line.
                <- Blank line
Reference:      <- External references, one per line (issue, trello, ...)
Signed-off-by:  <- Signature (created by git commit -s)
```

 - 确保你的代码在工作（安装你的avocado版本，测试你的改变，运行*make check*确保你没有引入任何回归）。
 - 在pastebin服务中粘贴上一步的*job.log*内容，像 fpaste.org。如果你已经安装了*fpaste*，你可以简单的运行：

``` stylus
$ fpaste ~/avocado/job-results/latest/job.log
```

 - 在upstream master顶端重定你的本地分支的基底：

``` stylus
$ git fetch
$ git rebase upstream/master
(resolve merge conflicts, if any)
```

 - 推送你的提交到你的fork上：

``` stylus
$ git push origin my_new_local_branch
```

 - 在github上创建推送请求。添加相关信息到你的推送请求描述上。
 - 在推送请求讨论页，链接到job.log输出/文件的评论。
 - 检查你的推送请求是否通过 CI (travis)。你的推送请求可能被忽略直到它都变成绿色。
 
 现在你在github的推送请求页等待反馈。一旦你得到了回馈，参加讨论，回答问题，明确如果你想要基于一些复审的基础上改变代码，如果不，为什么。不同意审查员，他们可能有不同的用例和意见，这正是所期待的。如有必要，尽力描述你的想法并且建议其他解决方案。
 
 你的代码的新版本不会被强制更新（除非显式地请求的代码审查）。反而，你应该：
 
 - 在你先前的分支之外创建一个新的分支

``` stylus
$ git checkout my_new_local_branch
$ git checkout -b my_new_local_branch_v2
```

 - 下载代码，修改提交并且/或者创建新的提交。如果你在项目中有多个提交，你将可能需要变基交互地去修改正确的提交。*git cola*或者*git citool*命令在这可能很方便。
 - 变基你在upstream master顶层上的本地分支：

``` stylus
$ git fetch
$ git rebase upstream/master
(resolve merge conflicts, if any)
```

 - 推送你的修改：

``` stylus
$ git push origin my_new_local_branch_v2
```

 - 对你的新分支创建一个新的推送请求。在推送请求描述中，指出前面的推送请求和相比于之前推送请求现在推送请求引导的改变。
 - 关掉github上之前的推送请求。


在你的项目合并后，你可以在你的本地软件库同步master分支，传播在github上同步你的fork软件库到master分支：

``` stylus
$ git checkout master
$ git pull upstream master
$ git push
```

有时，你可以删掉旧的分支避免污染：

``` stylus
# To list branches along with time reference:
$ git for-each-ref --sort='-authordate:iso8601' --format=' %(authordate:iso8601)%09%(refname)' refs/heads
# To remove branches from your fork repository:
$ git push origin :my_old_branch
```

### Signing commits

你可以选择使用GPG签名签署提交。这样做是简单的，它帮助未经授权的代码不用通知就合并。

所有你需要的是一个有效的GPG签名，git配置，使用签名稍微修改的工作流程，最终甚至在github上的设置，受益于github的友好界面。

获得一个GPG签名：

``` stylus
# Google for howto, but generally it works like this
$ gpg --gen-key  # defaults are usually fine (using expiration is recommended)
$ gpg --send-keys $YOUR_KEY    # to propagate the key to outer world
```

在git上打开它：

``` stylus
$ git config --global user.signingkey $YOUR_KEY
```
（有选择的）用你的GH账户链接key：

``` stylus
1. Login to github
2. Go to settings->SSH and GPG keys
3. Add New GPG key
4. run $(gpg -a --export $YOUR_EMAIL) in shell to see your key
5. paste the key there
```
使用它：

``` stylus
# You can sign commits by using '-S'
$ git commit -S
# You can sign merges by using '-S'
$ git merge -S
```


Warning
你不能在github上使用合并按钮去发送合并信号，当github没有你的私有key。

## Tests Repositories

我们鼓励你和你的公司创建公共的Avocado测试软件库，这样社区也能受益于你的测试。我们非常开心的在我们的文档中推广你的软件库。

已知的社区列表和第三方软件库维护：

 - https://github.com/avocado-framework-tests/avocado-misc-tests ：社区维护Avocado多方面的测试软件库。在那你可以发现，包括其他人的。像*lmbench*,*stress*那样的性能测试，像*ebizzy*这样的CPU测试和像*ltp*一样的一般测试。他们中的一些是从 Autotest Client Tests软件库移植的。
 - https://github.com/scylladb/scylla-cluster-tests ：给 Scylla Clusters的Avocado测试。这些测试可能自动的创建一个 scylla cluster，一些加载机器并且运行被测试作者定义的操作，例如数据库工作负载。

# Avocado development tips

##  Interrupting test

如果您想“暂停”正在运行的测试，你可以使用SIGTSTP（ctrl+z）信号发送到avocado主进程。这个信号被转发给测试，它是一个子进程。你发送同样的信号恢复测试。

注意：在停止的进程上，job/test超时仍然是使能的。

## In tree utils

 在avocado.utils.debug中你可以找到方便的工具。
 
###  measure_duration
Decorator 可以被用来打印当前执行函数的持续时间，积累这个 decorated函数的执行时间。当最优化时它非常便捷。

使用方法：

 

``` stylus
from avocado.utils import debug
...
@debug.measure_duration
def your_function(...):
```
执行期间查找：

``` stylus
PERF: <function your_function at 0x29b17d0>: (0.1s, 11.3s)
PERF: <function your_function at 0x29b17d0>: (0.2s, 11.5s)
```
## Line-profiler

你可以使用line_profiler逐字逐句的估量性能。你可以使用pip安装它：

``` stylus
pip install line_profiler
```
然后用@profile （不需要从其他地方加载它）简单的标记想要的功能。然后执行：

``` stylus
kernprof -l -v ./scripts/avocado run ...
```
当进程完成时，你将看到配置信息。（有时二进制被调用 kernprof.py）

## Remote debug with Eclipse
Eclipse是一个很好的调试前端，允许远程调试。它非常简单。你需要做的唯一的事是给Eclipse安装pydev插件。那么你需要定位pydevd路径（通常是$INSTALL_LOCATION/plugins/org.python.pydev\_\*/pysrc 或者 ~/.eclipse/plugins/org.python.pydev\_\*/pysrc）。然后你通过下面设置断点：

``` stylus
import sys
sys.path.append("$PYDEV_PATH")
import pydevd
pydevd.settrace("$IP_ADDR_OF_ECLIPSE_MACHINE")
```
二选一地，你可以导出 PYTHONPATH=$PYDEV_PATH ，或只使用最后两行。

在你运行代码之前，你需要开始Eclipse的调试服务。转向调试视角（你可能需要首先打开它 Window->Perspective->Open Perspective）。然后从 Pydev->Start Debug Server开启服务。

现在无论何时pydev.settrace()的代码在执行，它接触Eclipse调试服务（默认端口800，不要忘了打开它）并且你可以在执行中轻松的继续。这个工作每个远程的机器会访问你的Eclipse端口8000（你可以覆盖它）。

## Using Trello cards in Eclipse

Eclipse允许我们创建任务。他们非常酷正如你看到的状态（not started, started, current, done）并且通过切换任务从你先前完成的地方恢复（打开文件....）

Avocado打算使用Trello，Eclipse还没有支持。无论如何,至少有一种方法可以得到你提交的只读列表。这个用标签和描述的没有做好的引导是基于https://docs.google.com/document/d/1jvmJcCStE6QkJ0z5ASddc3fNmJwhJPOFN7X9-GLyabM/ 。唯一的不同是你需要使用Query Pattern：

``` stylus
\"url\":\"https://trello.com/[^/]*/[^/]*/({Id}[^\"]+)({Description})\"
```
安装trello key：

 1.创建一个Trello账户
 2.在这儿获取（developer_key）：https://trello.com/1/appKey/generate
 3.从下述地址（replace key with your key）获得user_token： https://trello.com/1/authorize?key=$developer_key&name=Mylyn%20Tasks&expiration=never&response_type=token
 4.你的分配的任务（task_addr）的地址： https://trello.com/1/members/my/cards?key=developer_key&token=$user_token ，在web浏览器中打开它并且你应该看到[]或者[$list_of_cards] 的无密码登录。



配置Eclipse：

 1. 我们需要web模板，它还不是上行。我们需要孵化模板。
 2. Help->Install New Software...
 3. -> Add
 4. Name: Incubator
 5. Location: http://download.eclipse.org/mylyn/incubator/3.10
 6. -> OK
 7. 选择Mylyn Tasks Connector: Web Templates (Advanced) (Incubation) (use filter text to find it)
 8. 安装它 (Next->Agree->Next...)
 9. 重启Eclipse
 10. 打开mylyn Team Repositories  Window->Show View->Other...->Mylyn->Team Repositories
 11. 右击Team Repositories 并且选择New->Repository
 12. 使用Task Repository -> Next
 13. 使用Web Template (Advanced) -> Next
 14. 在任务软件库对话框的属性中，进入 https://trello.com
 15. 在服务领域并且给软件库一个标签（例如：Trello API）。
 16. 在附加的设置部分设置applicationkey = $developer_key 和userkey = $user_token.
 17. 在高级配置设置Task URL到 https://trello.com/c/
 18. 设置 New Task URL to https://trello.com
 19. 设置 Query Request URL (不需要更改): https://trello.com/1/members/my/cards?key=${applicationkey}&token=${userkey}
 20. 为 the Query Pattern enter “url”:”https://trello.com/[^/]*/[^/]*/({Id}[^”]+)({Description})”
 21. -> Finish


创建任务查询：

 1. 通过打开 Mylyn Task List.创建一个查询。
 2. 右击 pane 并且选择 New Query。
 3. 选择 Trello API作为一个软件库
 4. -> Next
 5. 输入你查询的名称。
 6. 展开Advanced Configuration并且确认Query Pattern已经填入职。
 7. 点击预览确认没有错误。
 8. 点击完成
 9. 分配给你的Trello任务将出现在Mylyn 的任务列表中。

现在你能通过点击名字前面的小气泡开始使用任务。关掉所有的编辑。
尝试打开一些然后再次点击气泡。他们应该被关闭。当你点击气泡三次，他应该恢复之前所有打开的编辑。

我的正常工作流程是：

 1. git checkout $branch
 2. Eclipse: select task
 3. git commit ...
 4. Eclipse: unselect task
 5. git checkout $other_branch
 6. Eclipse: select another_task

这样你总是拥有提供的所有文件，你可以很简单的恢复你的工作。


# Releasing avocado

所以你所有的项目都已批准，Sprint会议做了，现在Avocado准备发布。太好了，让我们仔细检查你需要注意的（大多）数据。


## Bump the version number
通过avocado代码库，更新发行号。这次撰写时，用diff查看不同：

``` stylus
diff --git a/avocado.spec b/avocado.spec
index eb910e8..21313ca 100644
--- a/avocado.spec
+++ b/avocado.spec
@@ -1,7 +1,7 @@
 Summary: Avocado Test Framework
 Name: avocado
-Version: 0.28.0
-Release: 2%{?dist}
+Version: 0.29.0
+Release: 0%{?dist}
 License: GPLv2
 Group: Development/Tools
 URL: http://avocado-framework.github.io/
@@ -104,6 +104,9 @@ examples of how to write tests on your own.
 %{_datadir}/avocado/wrappers

 %changelog
+* Wed Oct 7 2015 Lucas Meneghel Rodrigues <lmr@redhat.com> - 0.29.0-0
+- New upstream release 0.29.0
+
 * Wed Sep 16 2015 Lucas Meneghel Rodrigues <lmr@redhat.com> - 0.28.0-2
 - Add pystache, aexpect, psutil, sphinx and yum/dnf dependencies for functional/unittests

diff --git a/avocado/core/version.py b/avocado/core/version.py
index c927b19..a555af5 100755
--- a/avocado/core/version.py
+++ b/avocado/core/version.py
@@ -18,7 +18,7 @@ __all__ = ['MAJOR', 'MINOR', 'RELEASE', 'VERSION']


 MAJOR = 0
-MINOR = 28
+MINOR = 29
 RELEASE = 0

 VERSION = "%s.%s.%s" % (MAJOR, MINOR, RELEASE)
diff --git a/setup.cfg b/setup.cfg
index 76953b9..5cf90e9 100644
--- a/setup.cfg
+++ b/setup.cfg
@@ -1,6 +1,6 @@
 [metadata]
 name = avocado
-version = 0.28.0
+version = 0.29.0
 summary = Avocado Test Framework
 description-file =
     README.rst
```
你可以在git上诸如提交命令上找到，这将帮助你获得其他版本库。

## Which repositories you should pay attention to
一般，一个avocado的发行包括查看和最终的发行内容在下面的软件库中：

 - *avocado*
 - *avocado-vt*

## Tag all repositories
当一切都处于一个好的状态时，提交改变的版本和标记，在master上提交：

``` stylus
$ git tag -u $(GPG_ID) -s $(RELEASE) -m 'Avocado Release $(RELEASE)'
```
然后tag应该被推送到Git软件库：

``` stylus
$ git push --tags
```
## Build RPMs
到源目录并且做：

``` stylus
$ make rpm
...
+ exit 0
```

这将是所有。它将使用mock建立数据包，针对你默认的配置。那通意味着你在相同的平台上。

## Sign Packages
为了更安全的公共消费，所有数据包都应该签名。这个进程当然基于私有key，基于运行：

``` stylus
$ rpm --resign
```
查看*rpmsign(8)* man page可以获得更多信息。

## Upload packages to repository

现在的分布方法是基于通过HTTP的服务内容。也就是说软件库元数据在本地创建并且同步到大家都知道的公共Web服务器。一个相似的进程：

``` stylus
$ cd $REPO_ROOT && for DIR in epel-?-noarch fedora-??-noarch; \
do cd $DIR && createrepo -v . && cd ..; done;
```

本地创建软件库数据。那么一个类似的命令：

``` stylus
$ rsync -va $REPO_ROOT user@repo_web_server:/path
```
用于复制内容。


## Write release notes

版本注释告知在一个给定的开发周期中有哪些改变。版本注释中的好地方是：

 1. Git logs
 2. Trello Cards (查找已做的列表)
 3. Github compare views: https://github.com/avocado-framework/avocado/compare/0.28.0...0.29.0


去那写一个文本表示发行包含的改变。

## Upload package to PyPI

使用者可能想要从PyPI软件库中获得Avocado，所以请也上传到那。协助这个过程，请运行：

``` stylus
$ make pypi
```

按照给定的URL和简短说明。

## Send e-mails to avocado-devel and other places
发送带有版本注释的邮件到avocado-devel 和 virt-test-devel。


# Test APIs

这是使用者应该使用的，能信赖的最小一组APIs合集，当在写测试的时候。

## Module contents

**avocado.main**
   TestProgram的别名
class avocado.Test(methodName='test', name=None, params=None, base_logdir=None, job=None, runner_queue=None)

Bases: unittest.case.TestCase
测试类的基本实现。
你将继承这个去写你自己的测试。有代表性的在你自己的测试中你将想要实现setUP(),test\*()和tearDown()方法。
初始化测试。
**Parameters** ：

 -  **methodName** -要去运行的main方法的名字。为了兼容原有的unittest类，你不应该设置这个。
 - **name(avocado.core.test.TestName)** - 漂亮的test name的名称。使用avocado API的一般测试，这个不应该被设置。被保留给内部Avocado使用，例如运行随机的可执行文件来测试。
 - **base_logdir** -test logs将要放置的目录。如果值是None，它将使用avocado.data_dir.create_job_logs_dir()。
 - **job** -这个测试的job部分。

 
**Raises** ： [avocado.core.test.NameNotTestNameError][36]

 
 **basedir**
 这个测试（当返回一个文件）位于的目录。
 
 
 **cache_dirs = None**


 **datadir**
 返回包含测试数据文件目录的路径。
 
 **default_params = {}**
 
 **error(message=None)**
 当前运行测试的错误。
 
 当调用了这个方法之后，测试将被终止并且有一个错误的状态。
     Parameters:	message (str) -一个选择的信息将会被记录在logs中
     
**fail(message=None)**
当前运行的测试失败。

 当调用了这个方法之后，测试将被终止并且有一个失败的状态。
 
    Parameters:	message (str) –一个选择的信息将会被记录在logs中
   
 **fetch_asset(name, asset_hash=None, algorithm='sha1', locations=None, expire=None)**
 
   为了获取utils.asset的调用方法和支持hash检测的资产文件，缓存，和多个定位。
   
   **Parameters** ：
  
 - **name** - 资产文件名或者URL
 - **assert_hash** - assert hash（可选的）
 - **algorithm**  - hash算法（可选的，默认是sha1）
 - **locations** - 获取自残文件的URLs列表（可选择的）
 - **expire** - 资产文件的到期时间


**Raises** ： 	**EnvironmentError** – 当获取资产文件失败

**return** ： 资产文件位置路径

 **filename**
 返回当前测试的文件（路径）的名称
 
 **get_state()**
 
 序列化可选的属性代表了测试状态
 
 Reruns ： 一个包含相关测试状态数据的目录
 Return type ： dict（字典）
 
 **report_state()**
 发送当前的测试状态到测试运行器进程
 
 **run_avocado()**
 
 包裹运行方法，为了avocado运行器内部的执行。
     Result： 未使用参数，和*unittest.TestCase*兼容。
     

**skip(message=None)**
忽略当前的运行测试。
这个方法应该仅在一个测试的setUP()方法中被调用，没有其他任何地方，因为根据定义，如果一个测试执行了，它不能忽略任何东西。如果你在setUP()之外调用这个方法，avocado将给你的状态标记为ERROR，并且通知你解决你测试中的错误信息。
    Parameters:	message (str) -一个可选择的信息，会被记录在logs中。
   

 **srcdir = None**

**workdir = None**
 
 
 
 
 **avocado.fail_on(exceptions=None)**
 
 当指定类型的修饰函数进程异常，测试失败。
 
 （例如：我们的方法可以提高测试软件错误上的indexError，我们要么可以try/catch它，要么使用这个修饰函数替代）
  
  
Parameters:	exceptions – 元组或者单个异常被假定为测试失败[Exception]
Note： self.error 和 self.skip行为保持完整。
Note：允许简单的使用，参数"异常"不能被调用。
 
#  Utilities APIs

 这是utility APIs的合集，Avocado给测试作者提供添加的值。
 
##  Subpackages
### [avocado.utils.external package][37]

 - [avocado.utils.external package][38]
 - [avocado.utils.external.gdbmi_parser module][39]
 - [avocado.utils.external.spark module][40]
 - [Module contents][41]

##  Submodules
## avocado.utils.archive module
模块是帮助提取和创建压缩archives。
 *exception* **avocado.utils.archive.ArchiveException**
Bases:[exceptions.Exception][42]
所有Archive错误的基本异常。
 
 *class* **avocado.utils.archive.ArchiveFile***(filename, mode='r')*
 Bases:[object][43]
 类代表了一个Archive文件。
 Archive是ZIP文件或者Tarballs。
 创建一个Archive实例。
 
 **Parameters:**
 - **filename** - archive文件名
 - **mode**   - 文件模式，r  读，w 写

**add**(filename, arcname=None)
添加文件到archive。
**Parameters:**

 - **filename**  - 到archive的文件。
 - **arcname**  - 在archive中文件的通用交叉名称

 **close()**
关掉存档。

**extract(path='.')**
从存档中提取所有文件
 **Paremeters： path**  - 目的路径
 
 **list()**
 列出标准输出的文件。
 
 **classmethod open(filename, mode='r')**
 创建一个[ArchiveFile][44]实例。
 **parameters**：
 
 - **filename**  - 存档文件名称
 - **mode**  -文件模式， r 读，w 写。


**avocado.utils.archive.compress***(filename, path)*
压缩文件到一个存档里。

 **Parameters:**
 
 - **filename**  -存档文件名称
 - **path**  - 文件压缩的原本路径。不允许个别的文件

 **avocado.utils.archive.create***(filename, path)*
 压缩文件到一个存档。
 **Parameters**:
 

 - filename  -  存档文件名称。
 - path  - 文件压缩的原本路径。不允许个别的文件。


**avocado.utils.archive.extract***(filename, path)*

 从存档里提取文件。
 **Parameters**：
 
 - filename  -存档文件名称
 - path   - 提取文件的目标路径
 
 **avocado.utils.archive.is_archive***(filename)*
测试已给的文件是否是一个存档。
**Parameters**： filename -  要测试的文件
**Returns**： 如果是一个存档返回True


**avocado.utils.archive.uncompress***(filename, path)*
从一个存档中提取文件。

 **Parameters**：

 - filename  -  存档文件名称
 - path   -  提取的目的路径

## avocado.utils.asset module
Asset从多个位置提取
*class* **avocado.utils.asset.Asset***(name, asset_hash, algorithm, locations, cache_dirs, expire=None)*
Bases：[object][45]

尽力地从多重位置中去获取或确认一个asset文件。

初始化Asset()类
**Parameters**：

 - name  -   asset文件名，url是支持的。
 - asset_hash  -   asset hash
 - algorithm  -   hash算法
 - locations  - 获取asset文件位置列表
 - cache_dirs  -   缓存目录列表
 - expire   -   asset到期时间

fetch()
获取asset，首先在提供的缓存目录列表上找到asset。然后尽力从提供的列表位置上下载asset。

Raises  ：	EnvironmentError   -  当获取asset失败时
Returns：   缓存目录上的文件路径。


## avocado.utils.astring module

字符串操作（转换和sanitation）
 与众不同的名字是为了避免使用标准库模块字符串时名字冲突。甚至使用符号点,人们尽力这样做：
 import string ... from avocado.utils import string
 
 直到他们的代码开始失败才通知。
 **avocado.utils.astring.bitlist_to_string***(data)*
 从bit list转换到ASCII字符串。
 Parameters： data  -   将被转换的Bit list
 
**avocado.utils.astring.iter_tabular_output***(matrix, header=None)*
 产生一个相当好的整齐的字符串代表一个nxm矩阵。
 这个代表能被用于打印任何的扁平的数据，例如数据库结果。它通过扫描每个列中每个元素的长度工作，并且动态的确定字符串格式。
 
 Parameters：
 
 - matrix  - 矩阵代表（n行m列的列表）
 - header - 可选择的元组或者头元素显示的列表
 
 **avocado.utils.astring.shell_escape***(command)*

来自一个命令的转义特殊字符，它在(ba)sh命令中 用一个双引号（" "）通过。

Parameters ： command  -  要去转义的命令字符串
Returns ： 转义命令字符串，需要的被摄入的双引号没有添加，在一些点通过调用被添加。

查看：http://www.tldp.org/LDP/abs/html/escapingsection.html

**avocado.utils.astring.string_safe_encode***(string)*
 人们倾向于将编码字符串混合到Unicode流中。这个函数尽力用一个合法的utf-8编码ascii流替换任何输入。
 
 **avocado.utils.astring.string_to_bitlist***(data)*
 转换ASCII字符串到bit list。
 Parameters：  data - 将要转换的字符串。
 
 **avocado.utils.astring.string_to_safe_path***(string)*
 将字符串转换为一个有效的文件或者目录名字。：参数字符串：被转换的字符串：返回：做为一个文件或者目录名字（在最近的文件系统上）能够安全通过的字符串。
 
 **avocado.utils.astring.strip_console_codes***(output, custom_codes=None)*

移除linux控制台转义并且控制控制台的输出序列。使得输出可读并且可以用于结果检测。现在仅仅在引导期间移除一些基本的控制台编码的使用。

Parameters：

 - output(string)  -  从linux控制台输出
 - custom_codes  - 被添加到控制台编码的编码，不在默认编码中
 
Returns： 没有任何特殊编码的字符串
Return type ： string

  **avocado.utils.astring.tabular_output***(matrix, header=None)*
  
 很好的对齐的字符串代表一个nxm矩阵。
 
 这个表示可能用于打印一些元组数据，例如数据库结果。他通过扫描每个列中每个元素的长度工作，并且动态的决定字符串格式。
 
 Parameters:

 - matrix  -  矩阵代表（n行m列的列表）
 - header - 可选的元组或则头部元素可显示的列表
 
 Returns：扁平输出的字符串，行和unix行输入隔开
 Return type：str
 
##  avocado.utils.aurl module

 URL相关功能。
 
 奇怪的名字是为了避免代码中意外的命名冲突。
 **avocado.utils.aurl.is_url***(path)*
 
 如果path看上去像一个URL则返回True。
 Parameters： path  -  待检测的路径
 Return type ： 布尔值
 
##  avocado.utils.build module

 **avocado.utils.build.make***(path, make='make', env=None, extra_args='', ignore_status=None, allow_output_check=None, process_kwargs=None)*
 
 运行make，添加 MAKEOPTS到选项列表。
 Parameters：

 - make - 要使用的make命令名称
 - env    -  在调用make之前将要被设置的环境变量目录（例如：CFLAGS）
 - extra  -  将要通过make的额外命令行参数
 - allow_output_check(str)  -  是否记录在测试流文件中make进程的命令流输出（标准输出和标准错误）。有效的值：'stdout'，只允许标准输出，'stderr'，只允许标准错误，'all'，允许标准输出和错误，'none'，不记录任何东西（默认）。这儿的默认是'none'，因为通常在测试中我们不想使用会变输出做为参考。
 

Returns： make进程的退出状态。

 **avocado.utils.build.run_make***(path, make='make', env=None, extra_args='', ignore_status=None, allow_output_check=None, process_kwargs=None)*
 
 运行make，添加MAKEOPTS 到选项列表。
 Parameters：

 - make  - 使用的make命令名称
 - env  -  在调用make之前设置的环境变量目录（例如：CFLAGS）
 - extra  - 需要通过make的额外命令行参数
 - allow_output_check(str) - 是否记录在测试流文件中make进程的命令流输出（标准输出和标准错误）。有效的值：'stdout'，只允许标准输出，'stderr'，只允许标准错误，'all'，允许标准输出和错误，'none'，不允许记录任何（默认）。这儿的默认值是'none'，因为通常在测试中我们不想使用汇编输出做为参考。
 - process_kwargs  -  附加参数给make的底层进程运行。
 

Returns： make命令结果对象

## avocado.utils.cpu module
从当前机器的CPU获取信息。

**avocado.utils.cpu.cpu_has_flags***(flags)*
 
 检查标签列表在当前CPU信息中是否可用
 Parameters： flags(list)  - 在当前CPU上必须存在的cpu标签列表。
 Returns： 如果所有标签都被发现返回True，否则返货False
 
 Return type： list
 
 **avocado.utils.cpu.cpu_online_list***()*
 报告一份在线cpus索引的列表。
 
 **avocado.utils.cpu.get_cpu_arch***()*
 
 算出当前正在运行的CPU的架构
 
 
 **avocado.utils.cpu.get_cpu_vendor_name***()*
 
 获取当前CPU供应商的名字
 Returns： 取决于当前CPU架构的字符串'intel' 或者 'amd'或则'power 7'。
 Return type：string
 
##  avocado.utils.crypto module
**avocado.utils.crypto.hash_file***(filename, size=None, algorithm='md5')*
 计算filename的hash值。
 如果size不是None，限制第一个文件字节大小。如果文件有错误抛出异常。也能在bash上用一行命令实现（确保size%1024==0）
 

``` stylus
dd if=filename bs=1024 count=size/1024 | sha1sum -
```
Parameters：

 - filename  -  将被计算hash值得文件路径
 - method  -  计算hash值得使用方法。支持方法：* md5 * sha1
 - size  -  如果提供，第一个文件大小用于hash计算

Returns：文件的hash值，如果出现错误，返回None。

**avocado.utils.crypto.hash_wrapper***(algorithm='md5', data=None)*

 返回一个要么使用mds或者sha1的数据的hash对象。
 Parameters： input  -  可选的输出字符串，用于更新hash值
 Returns： hash对象。
 
##  avocado.utils.data_factory module
为avocado框架和测试生有用的数据。

``` stylus
avocado.utils.data_factory.generate_random_string(length, ignore='!"#$%&\'()*+, -./:;<=>?@[\\]^_`{|}~', convert='')
```
使用字母数字字符生成随机字符串。
Parameters：

 - length(int)  -  生成字符串长度
 - ignore(str) -  在生成的字符串中不包括的字符
 - convert(str) - 需要转义的字符（预先考虑'""）。

Returns：生成的随机字符串

**avocado.utils.data_factory.make_dir_and_populate***(basedir='/tmp')*

 在basedir创建一个目录，并用一定数目的文件填充。
 文件中只有随机的文本内容。
 
 Parameters： basedir(str)  -  生成目录的基本目录
 Returns： 被创建和填充的目录路径
 Return type：str
 
##  avocado.utils.data_structures module
 这个模块包含了便捷的类，可在avocado核心代码和插件中使用
 
 *class* avocado.utils.data_structures.Borg
 
 这个类的多个实例共享相同的状态。
 这在python中被认为是比普通模式更好的模式，例如单例模式。灵感来源于Alex Martelli的文章：
 查看	http://www.aleax.it/5ep.html
 
  *class* avocado.utils.data_structures.CallbackRegister(name, log)
 Bases: object
 
 在后面执行的寄存器挑选功能。
 Parameters：name - 人类可读的寄存器表示符。
 
 register(func, args, kwargs, once=False)
 Register function/args被self.destroy()调用：参数 func：挑选的功能：参数 args：挑选的位置参数：参数 kwargs：挑选的关键字参数：参数once：添加唯一的一次组合（(func,args,kwargs）。
 run()
 调用所有车次注册函数。
 unregister(func,args,kwargs)
 Unregister (func,args,kwargs)组合：参数func：挑选的函数：参数args：挑选的位置参数：参数kwargs：挑选的关键字参数
 
  *class* avocado.utils.data_structures.LazyProperty(f_get)
 Bases: object
 延迟实例化属性。
 当你想要设置一个只有首次访问时被评估的属性，使用这个decorator 。灵感来源于Stack Overflow thread 的讨论：
 查看http://stackoverflow.com/questions/15226721/
 
 **avocado.utils.data_structures.compare_matrices***(matrix1, matrix2, threshold=0.05)*
 
 比较两个矩阵nxm，返回一个带有比较数据和状态的矩阵nxm。当第一列匹配，他们被认为是页眉并且包含在完整的结果中。
 Parameters：

 - matrix1 - 引用浮点型矩阵；第一列可能是header
 - matrix2 -  用于对比的矩阵；第一列可能是header
 - threshold - 任何差异大一这个百分比的阈值将会被记录。

Returns：对比中不同部分的矩阵，改进的数量，回归的数量，对比的总数目。

**avocado.utils.data_structures.geometric_mean***(values)*
评估一个数字值列表的集合方式。这个实现是慢的当时没有数字值得限制：参数Values：列出了值。：return：代表值列表的几何方式的单个值。查看： http://en.wikipedia.org/wiki/Geometric_mean

**avocado.utils.data_structures.ordered_list_unique***(object_list)*
返回一个独特的对象列表，带着他们保留的原本的顺序。
 
 **avocado.utils.data_structures.time_to_seconds***(time)*
 转换时间，把分钟，小时，天转换成秒：参数time：时间，可选择的带上单位（例如：'10d'）。
 
##  avocado.utils.debug module

 这个文件包含了Avocado开发的工具。
 **avocado.utils.debug.log_calls***(length=None, cls_name=None)*
 使用这个decorator 去记录带有所有参数的函数。：参数 length：最大的信息长度：参数cls_name：可选择的类名前缀
 
 **avocado.utils.debug.log_calls_class***(length=None)*
 使用这个decorator 记录函数方法调用。：参数length：最大的信息长度。
 
  **avocado.utils.debug.measure_duration***(func)*
 使用这个decorator 区测量函数执行的时间。输出是：“Function $name: ($current_duration, $accumulated_duration)”
 
## avocado.utils.disk module
 磁盘工具
 **avocado.utils.disk.freespace***(path)*
 
##  avocado.utils.distro module

 
# 保留1

# Internal (Core) APIs
内部的APIs，对Avocado袭击有意义。

## Subpackages
### [avocado.core.remote package][46]

 - [Submodules][47]
 - [avocado.core.remote.result module][48]
 - [avocado.core.remote.runner module][49]
 - [avocado.core.remote.test module][50]
 - [Module contents][51]
 

### [avocado.core.restclient package][52]

 - [Subpackages][53]
   - [avocado.core.restclient.cli package][54]
     - [Subpackages][55]
       - [avocado.core.restclient.cli.actions package][56]
         - [Submodules][57]
         - [avocado.core.restclient.cli.actions.base module][58]
         - [avocado.core.restclient.cli.actions.server module][59]
         - [Module contents][60]
       - [avocado.core.restclient.cli.args package][61]
         - [Submodules][62]
         - [avocado.core.restclient.cli.args.base module][63]
         - [avocado.core.restclient.cli.args.server module][64]
         - [Module contents][65]
     - [Submodules][66]
     - [avocado.core.restclient.cli.app module][67]
     - [avocado.core.restclient.cli.parser module][68]
     - [Module contents][69]
 - [Submodules][70]
 - [avocado.core.restclient.connection module][71]
 - [avocado.core.restclient.response module][72]
 - [Module contents][73]


## Submodules
## avocado.core.app module
 Avocado核心应用程序。
 *class* avocado.core.app.AvocadoApp
 Bases: object
Avocado 应用程序
run()


## avocado.core.data_dir module
库用于让avocado测试在系统中找到重要的路径。
找到路径的一般推理是：

 - 当在树上运行时，不使用avocado.conf。我们也在树中运行展示事例测试来表达。
 - 当avocado.conf在/etc/avocado或者~/.config/avocado中，那么在那获取值是可能的。如果他们只想的位置我们不能写到，那么使用下一个可用的最好位置。
 - 最好的下一个位置是默认的系统根目录的那个。
 - 最好的下一个位置是默认的用户指定的那个。

 **avocado.core.data_dir.clean_tmp_files***()*
通过移除尽力去清空临时目录。
对在tests\jobs完成后avocado进入去清空目录来说这个一个非常有用的函数。如果系统错误提高，悄然地忽略错误。

**avocado.core.data_dir.create_job_logs_dir***(logdir=None, unique_id=None)*
为一个job或者一个独立执行的测试创建一个log目录，
 Parameters：

 - logdir  -  基本的log目录，如果是None，使用配置的中值
 - unique_id  -  独一无二的标识。如果是None，创建一个。

Return type：basestring
**avocado.core.data_dir.get_base_dir***()*
获取最合适的基本目录。

对于大多avocado的其它重要目录而言，基本目录是父位置。
Examples：

 - Log目录
 - Data目录
 - Tests目录
**avocado.core.data_dir.get_data_dir***()*
获得最合适的数据目录的位置。
数据目录是在job的任何必要数据位置和测试操作的定位。
Examples：
 - ISO文件
 - GPG文件
 - VM内核
 - 参考位图
**avocado.core.data_dir.get_datafile_path***(\*args)*
获取相关数据目录的路径。
Parameters：
args  -  一个通过os.path.join. Ex (‘images’, ‘jeos.qcow2’)的参数

**avocado.core.data_dir.get_logs_dir***()*
获取最合适的log目录位置
log目录一般存储job/test的日志。
**avocado.core.data_dir.get_test_dir***()*
获取最合适的测试位置。
测试位置是存储我们写的带有avocado API的测试文件。

确定使用的启发式测试目录是：1）如果一个明确的测试目录被设置在配置系统中，它被使用。2）如果使用者在源代码树之外运行Avocado，测试用例目录被使用。3）系统根目录被使用。4）用户默认测试目录(~/avocado/tests) 被使用。
**avocado.core.data_dir.get_tmp_dir***()*
获取最合适的临时目录的位置。
临时目录是人为产生在测试时保留的。
Examples：

 - 一个测试套件的源代码拷贝
 - 编译测试套件源代码

## avocado.core.dispatcher module


 
#  保留2
后期补充

#  Extension (plugin) APIs
扩展APIs可以用于插件的书写。
## Submodules
## avocado.plugins.archive module
存档插件结果
*class* **avocado.plugins.archive.Archive**
Bases: avocado.core.plugin_interfaces.Result
**description** = 'Result archive (ZIP) support'
**name** = 'zip_archive'
**render**(result, job)

*class* **avocado.plugins.archive.ArchiveCLI**
Bases: avocado.core.plugin_interfaces.CLI
**configure**(parser)
**description** = 'Result archive (ZIP) support to run command'
**name** = 'zip_archive'
**run**(args)

## avocado.plugins.config module

*class* **avocado.plugins.config.Config**
Bases: avocado.core.plugin_interfaces.CLICmd

实现avocado 'config'子命令

**configure**(parser)
**description** = 'Shows avocado config keys'
**name** = 'config'
**run**(args)

























# 保留3
后期补充

# Release Notes

 以下页是Avocado中新发布的总结：
 

 - [42.0 Stranger Things][74]
 - [41.0 Outlander][75]
 - [40.0 Dr Who][76]
 - [39.0 The Hateful Eight][77]
 - [38.0 Love, Ken][78]
 - [37.0 Trabant vs. South America][79]
 - [36.0 LTS][80]
 - [35.0 Mr. Robot][81]
 - [0.34.0 The Hour of the Star][82]
 - [0.33.0 Lemonade Joe or Horse Opera][83]
 - [0.32.0 Road Runner][84]
 - [0.31.0 Lucky Luke][85]
 - [0.30.0 Jimmy’s Hall][86]
 - [0.29.0 Steven Universe][87]
 - [0.28.0 Jára Cimrman, The Investigation of the Missing Class Register][88]
 - [0.27.1][89]
 - [0.27.0 Terminator: Genisys][90]
 - [0.26.0 The Office][91]
 - [0.25.0 Blade][92]

 
 
 
 
 
 
 
 
 


  [1]: https://www.redhat.com/archives/avocado-devel/2016-April/msg00038.html
  [2]: https://www.redhat.com/archives/avocado-devel/2016-April/msg00038.html
  [3]: https://build.opensuse.org/package/show/Virtualization:Tests/avocado
  [4]: http://avocado-framework.readthedocs.io/en/latest/ContributionGuide.html#hacking-and-using
  [5]: bhh
  [6]: http://avocado-framework.readthedocs.io/en/latest/ReferenceGuide.html#test-types
  [7]: https://www.python.org/dev/peps/pep-0008/#function-names
  [8]: https://www.python.org/dev/peps/pep-0008/
  [9]: http://avocado-framework.readthedocs.io/en/latest/Mux.html
  [10]: http://avocado-framework.readthedocs.io/en/latest/Mux.html
  [11]: http://avocado-framework.readthedocs.io/en/latest/Mux.html
  [12]: http://avocado-framework.readthedocs.io/en/latest/Mux.html
  [13]: http://avocado-framework.readthedocs.io/en/latest/index.html#api-reference
  [14]: http://www.json.org/
  [15]: http://avocado-framework.readthedocs.io/en/latest/Plugins.html
  [16]: https://en.wikipedia.org/wiki/INI_file
  [17]: http://avocado-framework.readthedocs.io/en/latest/Mux.html#yaml-to-mux-plugin
  [18]: http://avocado-framework.readthedocs.io/en/latest/Mux.html#yaml-to-mux-plugin
  [19]: http://www.yaml.org/
  [20]: http://rr-project.org/
  [21]: http://avocado-framework.readthedocs.io/en/latest/api/utils/avocado.utils.html#avocado.utils.process.run
  [22]: http://avocado-framework.readthedocs.io/en/latest/api/utils/avocado.utils.html#avocado.utils.process.SubProcess
  [23]: http://avocado-framework.readthedocs.io/en/latest/api/utils/avocado.utils.html#module-avocado.utils.gdb
  [24]: http://avocado-framework.readthedocs.io/en/latest/api/utils/avocado.utils.html#avocado.utils.gdb.GDB
  [25]: http://avocado-framework.readthedocs.io/en/latest/api/utils/avocado.utils.html#module-avocado.utils.process
  [26]: https://github.com/openstack/stevedore
  [27]: https://pythonhosted.org/setuptools/
  [28]: https://pythonhosted.org/setuptools/pkg_resources.html#entry-points
  [29]: http://docs.openstack.org/developer/stevedore/index.html
  [30]: https://github.com/avocado-framework/avocado/tree/master/examples/plugins
  [31]: http://avocado-framework.readthedocs.io/en/latest/api/plugins/avocado.plugins.html#module-avocado.plugins
  [32]: ./images/diagram.png "diagram.png"
  [33]: http://avocado-framework.readthedocs.io/en/latest/api/utils/avocado.utils.html#avocado.utils.data_structures.CallbackRegister.register
  [34]: https://help.github.com/articles/about-pull-requests/
  [35]: https://travis-ci.org/avocado-framework/avocado
  [36]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.html#avocado.core.test.NameNotTestNameError
  [37]: http://avocado-framework.readthedocs.io/en/latest/api/utils/avocado.utils.external.html
  [38]: http://avocado-framework.readthedocs.io/en/latest/api/utils/avocado.utils.external.html#submodules
  [39]: http://avocado-framework.readthedocs.io/en/latest/api/utils/avocado.utils.external.html#module-avocado.utils.external.gdbmi_parser
  [40]: http://avocado-framework.readthedocs.io/en/latest/api/utils/avocado.utils.external.html#module-avocado.utils.external.spark
  [41]: http://avocado-framework.readthedocs.io/en/latest/api/utils/avocado.utils.external.html#module-avocado.utils.external
  [42]: https://docs.python.org/3/library/exceptions.html#exceptions.Exception
  [43]: https://docs.python.org/3/library/functions.html#object
  [44]: http://avocado-framework.readthedocs.io/en/latest/api/utils/avocado.utils.html#avocado.utils.archive.ArchiveFile
  [45]: https://docs.python.org/3/library/functions.html#object
  [46]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.remote.html
  [47]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.remote.html#submodules
  [48]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.remote.html#module-avocado.core.remote.result
  [49]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.remote.html#module-avocado.core.remote.runner
  [50]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.remote.html#module-avocado.core.remote.test
  [51]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.remote.html#module-avocado.core.remote
  [52]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.restclient.html
  [53]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.restclient.html#subpackages
  [54]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.restclient.cli.html
  [55]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.restclient.cli.html#subpackages
  [56]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.restclient.cli.actions.html
  [57]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.restclient.cli.actions.html#submodules
  [58]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.restclient.cli.actions.html#module-avocado.core.restclient.cli.actions.base
  [59]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.restclient.cli.actions.html#module-avocado.core.restclient.cli.actions.server
  [60]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.restclient.cli.actions.html#module-avocado.core.restclient.cli.actions
  [61]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.restclient.cli.args.html
  [62]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.restclient.cli.args.html#submodules
  [63]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.restclient.cli.args.html#module-avocado.core.restclient.cli.args.base
  [64]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.restclient.cli.args.html#module-avocado.core.restclient.cli.args.server
  [65]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.restclient.cli.args.html#module-avocado.core.restclient.cli.args
  [66]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.restclient.cli.html#submodules
  [67]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.restclient.cli.html#module-avocado.core.restclient.cli.app
  [68]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.restclient.cli.html#module-avocado.core.restclient.cli.parser
  [69]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.restclient.cli.html#module-avocado.core.restclient.cli
  [70]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.restclient.html#submodules
  [71]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.restclient.html#module-avocado.core.restclient.connection
  [72]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.restclient.html#module-avocado.core.restclient.response
  [73]: http://avocado-framework.readthedocs.io/en/latest/api/core/avocado.core.restclient.html#module-avocado.core.restclient
  [74]: http://avocado-framework.readthedocs.io/en/latest/release_notes/42_0.html
  [75]: http://avocado-framework.readthedocs.io/en/latest/release_notes/41_0.html
  [76]: http://avocado-framework.readthedocs.io/en/latest/release_notes/40_0.html
  [77]: http://avocado-framework.readthedocs.io/en/latest/release_notes/39_0.html
  [78]: http://avocado-framework.readthedocs.io/en/latest/release_notes/38_0.html
  [79]: http://avocado-framework.readthedocs.io/en/latest/release_notes/37_0.html
  [80]: http://avocado-framework.readthedocs.io/en/latest/release_notes/36_0.html
  [81]: http://avocado-framework.readthedocs.io/en/latest/release_notes/35_0.html
  [82]: http://avocado-framework.readthedocs.io/en/latest/release_notes/0_34_0.html
  [83]: http://avocado-framework.readthedocs.io/en/latest/release_notes/0_33_0.html
  [84]: http://avocado-framework.readthedocs.io/en/latest/release_notes/0_32_0.html
  [85]: http://avocado-framework.readthedocs.io/en/latest/release_notes/0_31_0.html
  [86]: http://avocado-framework.readthedocs.io/en/latest/release_notes/0_30_0.html
  [87]: http://avocado-framework.readthedocs.io/en/latest/release_notes/0_29_0.html
  [88]: http://avocado-framework.readthedocs.io/en/latest/release_notes/0_28_0.html
  [89]: http://avocado-framework.readthedocs.io/en/latest/release_notes/0_27_1.html
  [90]: http://avocado-framework.readthedocs.io/en/latest/release_notes/0_27_0.html
  [91]: http://avocado-framework.readthedocs.io/en/latest/release_notes/0_26_0.html
  [92]: http://avocado-framework.readthedocs.io/en/latest/release_notes/0_25_0.html