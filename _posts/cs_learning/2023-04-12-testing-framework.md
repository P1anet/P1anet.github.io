<!--
 * @FilePath     : \workspace\P1anet.github.io\_posts\cs_learning\2023-04-12-testing-framework.md
 * @Author       : P1anet
 * @Date         : 2023-04-12 22:14:50
 * @Version      : V1.0.0
 * @LastEditors  : P1anet
 * @LastEditTime : 2023-04-14 11:13:06
 * @Description  : 测试框架学习，主要是unittest
 * 
 * Copyright (c) 2023 by P1anet, All Rights Reserved. 
-->
---
layout: post
title: "测试框架"
subtitle: "Testing Framework"
date: 2023-04-12
author: "Kenshin"
header-img: "img/post-bg/post-bg-default.png"
tags: 
  - LearnCS
---

# 测试框架

![自动化测试框架](https://img-blog.csdnimg.cn/c8f527c0a4ac47da9b1ec190d9b24d47.png)

## Outline

1. [Junit](https://junit.org/junit5/)
2. [Selenium](https://www.selenium.dev/)
3. [TestNG](https://testng.org/doc/index.html)
4. [Cucumber](https://cucumber.io/)
5. [Pytest](https://docs.pytest.org/en/6.2.x/)
6. [Robot Framework](https://robotframework.org/)
7. [UnitTest/PyUnit](https://github.com/python/cpython/tree/3.10/Lib/unittest/__init__.py)
8. [Behave]()
9. [Lettuce]()
10. [LoadRunner]()
11. [Jmeter]()
12. [Appium](http://appium.io/)
13. [ATX](https://github.com/NetEaseGame/ATX)

## 各测试框架简介

### Junit

Junit是一个面向Java编程语言的单元测试框架

单元测试：针对最小的功能单元编写测试代码。Java程序最小的功能单元是方法，对Java编程语言进行单元测试，说白了就是对Java的方法进行测试。

好处：
- 非常简单地组织测试代码，并随时运行它们。
- 可以自动生成测试报告，收集成功的测试用例和失败的测试用例，统计测试成功率和代码覆盖率。
- 几乎所有的IDE工具都集成了Junit。

场景：
- 常用于单元测试（白盒测试）
- 自动化测试的用例管理和用例执行框架（API自动化、UI自动化）。

### Selenium

Selenium 是使用最为广泛的 Web 自动化测试（UI自动化）框架之一。

Selenium 可以完全模拟用户对主流浏览器进行操作，主要包括鼠标事件和键盘事件。
- 鼠标事件：右击、双击、拖动、悬停。
- 键盘事件：Keys()类提供了键盘上几乎所有按键的方法，可以模拟各种键盘输入。

Selenium 支持八种元素定位方式：
- id定位： find_element_by_id()
- name定位： find_element_by_name()
- class定位：find_element_by_class_name()
- tag定位：find_element_by_tag_name()
- link定位：find_element_by_link_text()
- partial_link定位：find_element_by_partial_link_text()
- xpath定位：find_element_by_xpath()
- CSS定位：find_element_by_css_selector()

兼容性方面，Selenium 支持 Chrome、FireFox、Safari 等主流浏览器；
并且 Selenium 对 Java 和 Python 都提供了便捷的API调用。

场景:
- UI 自动化
- 爬虫

### TestNG

TestNG 是 Java中一个很流行实用的单元测试框架。

它的灵感来源于 Junit ( java 的单元测试框架) 和 Nunit ( .net 的单元测试框架)。

但是它又在此基础上引入了新的东西，使得它更加强大。

关于注解特性方面，可以参考下表：

<table data-draft-node="block" data-draft-type="table" data-size="normal" data-row-style="normal"><tbody><tr><td>特性</td><td>JUnit 4</td><td>TestNG</td></tr><tr><td>测试注解</td><td>@Test</td><td>@Test</td></tr><tr><td>测试套件在执行之前需要执行的</td><td>–</td><td>@BeforeSuite</td></tr><tr><td>测试套件在执行之后需要执行的</td><td>–</td><td>@AfterSuite</td></tr><tr><td>在测试之前需要执行的</td><td>–</td><td>@BeforeTest</td></tr><tr><td>在测试之后需要执行的</td><td>–</td><td>@AfterTest</td></tr><tr><td>在一个测试方法所属于的任意一个组的第一个方法被调用之前执行</td><td>–</td><td>@BeforeGroups</td></tr><tr><td>在一个测试方法所属于的任意一个组的最后一个方法被调用之后执行</td><td>–</td><td>@AfterGroups</td></tr><tr><td>在当前类的第一个测试方法调用之前执行</td><td>@BeforeClass</td><td>@BeforeClass</td></tr><tr><td>在当前类的最后一个测试方法调用之后执行</td><td>@AfterClass</td><td>@AfterClass</td></tr><tr><td>每个测试方法之前需要执行</td><td>@Before</td><td>@BeforeMethod</td></tr><tr><td>每个测试方法之后需要执行</td><td>@After</td><td>@AfterMethod</td></tr><tr><td>忽略</td><td>@ignore</td><td>@Test(enbale=false)</td></tr><tr><td>预期异常</td><td>@Test(expected = ArithmeticException.class)</td><td>@Test(expectedExceptions = ArithmeticException.class)</td></tr><tr><td>超时</td><td>@Test(timeout = 1000)</td><td>@Test(timeout = 1000)</td></tr></tbody></table>

TestNG 和 JUnit 还有两个比较明显的区别：
- 在Junit 4 中，如果我们需要在方法前面使用 @BeforeClass 和 @AfterClass ，那么该测试方法则必须是静态方法。TestNG 在方法定义部分则更加的灵活，它不需要类似的约束。
- estNG 中子类不会运行父类中的 @BeforeClass 和 @AfterClass， 而在Junit中会先运行父类的@BeforeClass，再运行自己的 @BeforeClass；而 @AfterClass 是先运行自己的，再运行父类的。

经过一番对比之后，TestNG 在参数化测试、依赖测试以及套件测试（组）方面功能比 Junit 更加强大，并且包含了几乎 Junit 的所有功能，所以建议优先选择 TestNG。

场景：
- 常用于单元测试（白盒测试）
- 自动化测试的用例管理和用例执行框架（API自动化、UI自动化）。

### Cucumber

面向“注释”编程

BDD:Behavior-Driven Development ，也就是行为驱动开发。

BDD使用的叫做Gherkin的语言，它的理念是使用自然语言来描述功能，而且强调的是使用例子来说明需求功能。是不是跟敏捷开发中的用户故事(User Story)很像？嗯，因为它们都是一个妈生的。

使用这种方法可以让非技术人员、客户可以参与到需求的确认与验收当中。

```
// Given 前置（预置）条件。（一般用于定义一个变量等）
@Given("today is Sunday")  
public void today_is_Sunday() {
    // Write code here that turns the phrase above into concrete actions
    throw new io.cucumber.java.PendingException();
}

// When 当xxx的时候（相当于 if ）
@When("I ask whether it's Friday yet") 
public void i_ask_whether_it_s_Friday_yet() {
    // Write code here that turns the phrase above into concrete actions
    throw new io.cucumber.java.PendingException();
}

// Then 那么将要做xxx操作。
@Then("I should be told {string}")
public void i_should_be_told(String string) {
    // Write code here that turns the phrase above into concrete actions
    throw new io.cucumber.java.PendingException();
}
```

场景：
- 自动化测试用例管理和用例执行（API自动化、UI自动化）。

### Pytest

Pytest 是 python 语言中一款强大的单元测试框架（也是最好用的单元测试框架，不服来战），用来管理和组织测试用例，可应用在单元测试、自动化测试工作中。

pytest是python的第三方测试框架，是基于unittest的扩展框架，比unittest更简洁，更高效。使用pytest编写用例，必须遵守以下规则：

1. 测试文件名必须以 test_ 开头或者 _test 结尾(如：test_ab.py)
2. 测试方法必须以 test 开头。
3. 测试类命名以 Test 开头。

Pytest 在 unittest 的基础上，丰富了不少功能，而且比 unittest 更简洁高效，pytest + allure 还可以输出更加美观的测试报告，pytest在数据驱动上的支持，也是略优于unittest。

- 简单灵活，容易上手。
- 支持参数化。
- 能够支持简单的单元测试和复杂的功能测试，还可以用来做 selenium / appnium 等自动化测试、接口自动化测试（pytest+requests)。
- pytest 不是 python 内置库，需要单独安装。

场景：
- 常用于单元测试（白盒测试）
- 自动化测试的用例管理和用例执行框架（API自动化、UI自动化）。

### Robot Framework

支持跨平台、多种应用测试。可以兼容：windos、MacOS、Linux等平台。

推荐使用Python3.6.4确保适当的注释能够被添加到diamagnetic中，并且可以跟踪更改。需要安装Python包管理器—pip。

学习的内容：

关键字、资源Resource、库Robot支持很多库，分为内置外置的。Python、Java携程的文件都可以；用例（多个关键字组合，顺序或者分支控制等组成的文件为Case）

### UnitTest/PyUnit

UnitTest/PyUnit是一种标准化的针对单元测试的Python类自动化测试框架。基类TestCase提供了各种断言方法、以及所有清理和设置的例程。因此TestCase子类中的每一种方法都是以“test”作为名词前缀，以标识它们能够被作为测试用例所运行。用户可以使用load方法和TestSuite类来分组、并加载各种测试。也可以通过联合使用来构建自定义的测试运行器。正如我们使用Junit去测试Selenium那样，UnitTest也会用到UnitTest-sml-reporting、并能生成各种XML类型的报告。

使用先决条件：由于UnitTest默认使用了Python，因此我们并不需要什么先决条件。除了需要具备Python框架的基本知识，也可以额外地安装pip、以及用于开发的IDE工具。

UnitTest编写Python单元测试代码，包括下面几个步骤，也就是我们应该学习的步骤。

写一个Python类，继承unittest模块中的testcase类，在编写测试类中定义测试方法（测试用例），在测试方法中调用被测试代码，校验测试结果。TsetCase类中提供了很懂校验的方法，最常见的就是assertEqual

### Behave

行为驱动开发(behavior-driven development，BDD)，是一种基于敏捷软件开发的方法。它能够鼓励开发人员、业务参与者和QA人员，三者之间的协作。作为另一种Python测试框架，Behave允许团队避开各种复杂的情况，去执行BDD测试。从本质上说，该框架与SpecFlow和Cucumber非常相似，常被用于执行自动化测试。用户可以通过简单易读的语言来编写测试用例，并能够在其执行期间粘贴到代码之中。而且，那些被设定的行为规范与步骤，也可以被重用到其他的测试方案中。

使用先决条件：任何具备Python基础知识的人都可以使用Behave。其他先决条件还包括：必须先安装Python 2.7.14及以上的版本。需要通过Python包管理器或pip来与Behave协作。大多数开发人员会选择Pycharm作为开发环境，当然也可以选用其他的IDE工具。

### Lettuce

Lettuce是另一种基于Cucumber和Python的行为驱动类自动化工具。Lettuce主要专注于那些具有行为驱动开发特征的普通任务。它不但简单易用，而且能够使得整个测试过程更流畅、甚至更有趣。

使用先决条件：需要安装带有IDE的Python 2.7.14、及以上的版本。当然，也可以使用Pycharm或任何其他IDE工具。同时，还需要安装Python包管理器。

五种自动化测试的Python框架中Pytest、Robot Framework和UnitTest可主要用于功能与单元测试，而Lettuce和Behave仅适用于行为驱动测试。

### Appium

在移动端的 UI自动化 测试领域，带头大哥当属 Appium。

Appium 是基于 Nodejs 的一款 UI自动化 测试框架。

支持多平台（Android、iOS等）

支持多语言（python、java、ruby、js、c#等）

Appium是跨平台的，可以用在OSX，Windows以及Linux桌面系统上运行。

场景：
- 移动端的UI自动化测试

一般会结合单元测试框架去开发UI自动化测试框架。

Java：TestNG + Appium + Jenkins

Python：Pytest + Appium + Allure + Jenkins

### ATX

ATX(AutomatorX) 是一款（网易）开源的自动化测试工具，支持测试iOS平台和Android平台的原生应用、游戏、Web应用。

使用Python来编写测试用例，混合使用图像识别，控件定位技术来完成游戏的自动化。

附加专用的IDE来完成脚本的快速编写。

ATX的生态圈：

[底层驱动]
- 安卓(Android) https://github.com/openatx/uiautomator2 简称u2
- 苹果(iOS) https://github.com/openatx/facebook-wda

[元素定位]
- Weditor https://github.com/openatx/weditor

[设备管理]
- atxserver2 https://github.com/openatx/atxserver2

[测试框架]
- ATX-Test pengchenglin/ATX-Test

场景：
- 移动端的UI自动化测试

一般会结合单元测试框架去开发UI自动化测试框架。

Python：Pytest + uiautomator2/wda + Allure + Jenkins

## unittest - 单元测试框架

### 面向对象方式支持的概念

- 测试脚手架
  - test fixture 表示为了开展一项或多项测试所需要进行的准备工作，以及所有相关的清理操作。举个例子，这可能包含创建临时或代理的数据库、目录，再或者启动一个服务器进程。

- 测试用例
  - 一个测试用例是一个独立的测试单元。它检查输入特定的数据时的响应。 unittest 提供一个基类： TestCase ，用于新建测试用例。

- 测试套件
  - test suite 是一系列的测试用例，或测试套件，或两者皆有。它用于归档需要一起执行的测试。

- 测试运行器
  - test runner 是一个用于执行和输出测试结果的组件。这个运行器可能使用图形接口、文本接口，或返回一个特定的值表示运行测试的结果。

### Basic Example

```python
import unittest

class TestStringMethods(unittest.TestCase): # 继承unittest.TestCase

    def test_upper(self): # 以test开头约定告诉测试运行者类的哪些方法表示测试
        self.assertEqual('foo'.upper(), 'FOO') # 调用 assertEqual() 来检查预期的输出

    def test_isupper(self):
        self.assertTrue('FOO'.isupper()) # 调用 assertTrue() 或 assertFalse() 来验证一个条件
        self.assertFalse('Foo'.isupper())

    def test_split(self):
        s = 'hello world'
        self.assertEqual(s.split(), ['hello', 'world'])
        # check that s.split fails when the separator is not a string
        with self.assertRaises(TypeError): # 调用 assertRaises() 来验证抛出了一个特定的异常
            s.split(2)

    # 使用这些方法而不是 assert 语句是为了让测试运行者能聚合所有的测试结果并产生结果报告
    # 通过 setUp() 和 tearDown() 方法，可以设置测试开始前与完成后需要执行的指令

if __name__ == '__main__':
    unittest.main() # unittest.main() 提供了一个测试脚本的命令行接口
```

### Command-line Interface

unittest 模块可以通过命令行运行模块、类和独立测试方法的测试:

```bash
python -m unittest test_module1 test_module2 # 模块 (>=3.2)
python -m unittest test_module.TestClass # 类 (>=3.2)
python -m unittest test_module.TestClass.test_method # 方法
```

测试模块可以通过文件路径指定:

```bash
python -m unittest tests/test_something.py
```

这样就可以使用 shell 的文件名补全指定测试模块。所指定的文件仍需要可以被作为模块导入。路径通过去除 '.py' 、把分隔符转换为 '.' 转换为模块名。若你需要执行不能被作为模块导入的测试文件，你需要直接执行该测试文件。

在运行测试时，你可以通过添加 -v 参数获取更详细（更多的冗余）的信息。

```bash
python -m unittest -v test_module
```

当运行时不包含参数，开始 探索性测试

```bash
python -m unittest
```

用于获取命令行选项列表：

```bash
python -m unittest -h
```

#### Command-line Options

unittest supports these command-line options:

**-b, --buffer** (>=3.2)

在测试运行时，标准输出流与标准错误流会被放入缓冲区。成功的测试的运行时输出会被丢弃；测试不通过时，测试运行中的输出会正常显示，错误会被加入到测试失败信息。

**-c, --catch** (>=3.2)

当测试正在运行时， Control-C 会等待当前测试完成，并在完成后报告已执行的测试的结果。当再次按下 Control-C 时，引发平常的 KeyboardInterrupt 异常。

See Signal Handling for the functions that provide this functionality.

**-f, --failfast** (>=3.2)

当出现第一个错误或者失败时，停止运行测试。

**--locals** (>=3.5)

在回溯中显示局部变量。

**-k** (>=3.7)

Only run test methods and classes that match the pattern or substring. This option may be used multiple times, in which case all test cases that match any of the given patterns are included.

包含通配符（*）的模式使用 fnmatch.fnmatchcase() 对测试名称进行匹配。另外，该匹配是大小写敏感的。

模式对测试加载器导入的测试方法全名进行匹配。

例如，-k foo 可以匹配到 foo_tests.SomeTest.test_something 和 bar_tests.SomeTest.test_foo ，但是不能匹配到 bar_tests.FooTest.test_something 。

命令行亦可用于探索性测试，以运行一个项目的所有测试或其子集。

### Test Discovery

Unittest支持简单的测试搜索。若需要使用探索性测试，所有的测试文件必须是 modules 或 packages （包括 namespace packages ）并可从项目根目录导入（即它们的文件名必须是有效的 identifiers ）。

探索性测试在 TestLoader.discover() 中实现，但也可以通过命令行使用。它在命令行中的基本用法如下：

```bash
cd project_directory
python -m unittest discover
# 方便起见， python -m unittest 与 python -m unittest discover 等价。
# 如果需要向探索性测试传入参数，必须显式地使用 discover 子命令。
```

discover 有以下选项：

- -v, --verbose
  - 更详细地输出结果。

- -s, --start-directory directory
  - 开始进行搜索的目录(默认值为当前目录 . )。

- -p, --pattern pattern
  - 用于匹配测试文件的模式（默认为 test*.py ）。

- -t, --top-level-directory directory
  - 指定项目的最上层目录（通常为开始时所在目录）。

-s ，-p 和 -t 选项可以按顺序作为位置参数传入。以下两条命令是等价的：

```bash
python -m unittest discover -s project_directory -p "*_test.py"
python -m unittest discover project_directory "*_test.py"
```

正如可以传入路径那样，传入一个包名作为起始目录也是可行的，如 myproject.subpackage.test 。你提供的包名会被导入，它在文件系统中的位置会被作为起始目录。

警告：探索性测试通过导入测试对测试进行加载。在找到所有你指定的开始目录下的所有测试文件后，它把路径转换为包名并进行导入。如 foo/bar/baz.py 会被导入为 foo.bar.baz 。
如果你有一个全局安装的包，并尝试对这个包的副本进行探索性测试，可能会从错误的地方开始导入。如果出现这种情况，测试会输出警告并退出。

如果你使用包名而不是路径作为开始目录，搜索时会假定它导入的是你想要的目录，所以你不会收到警告。

测试模块和包可以通过 load_tests protocol 自定义测试的加载和搜索。

在 3.4 版更改: 测试发现支持初始目录下的 命名空间包。注意你也需要指定顶层目录（例如：python -m unittest discover -s root/namespace -t root）。

### Organizing Test code

单元测试的构建单位是 test cases ：独立的、包含执行条件与正确性检查的方案。
在 unittest 中，测试用例表示为 unittest.TestCase 的实例。
通过编写 TestCase 的子类或使用 FunctionTestCase 编写你自己的测试用例。

一个 TestCase 实例的测试代码必须是完全自含的，因此它可以独立运行，或与其它任意组合任意数量的测试用例一起运行。

TestCase 的最简单的子类需要实现一个测试方法（例如一个命名以 test 开头的方法）以执行特定的测试代码：

```python
import unittest

class DefaultWidgetSizeTestCase(unittest.TestCase):
    def test_default_widget_size(self):
        widget = Widget('The widget')
        self.assertEqual(widget.size(), (50, 50))
```

可以看到，为了进行测试，我们使用了基类 TestCase 提供的其中一个 assert*() 方法。若测试不通过，将会引发一个带有说明信息的异常，并且 unittest 会将这个测试用例标记为测试不通过。任何其它类型的异常将会被当做错误处理。

可能同时存在多个前置操作相同的测试，我们可以把测试的前置操作从测试代码中拆解出来，并实现测试前置方法 setUp() 。在运行测试时，测试框架会自动地为每个单独测试调用前置方法。

```python
import unittest

class WidgetTestCase(unittest.TestCase):
    def setUp(self):
        self.widget = Widget('The widget')

    def test_default_widget_size(self):
        self.assertEqual(self.widget.size(), (50,50),
                         'incorrect default size')

    def test_widget_resize(self):
        self.widget.resize(100,150)
        self.assertEqual(self.widget.size(), (100,150),
                         'wrong size after resize')
```

注解：多个测试运行的顺序由内置字符串排序方法对测试名进行排序的结果决定。

在测试运行时，若 setUp() 方法引发异常，测试框架会认为测试发生了错误，因此测试方法不会被运行。

相似的，我们提供了一个 tearDown() 方法在测试方法运行后进行清理工作。

```python
import unittest

class WidgetTestCase(unittest.TestCase):
    def setUp(self):
        self.widget = Widget('The widget')

    def tearDown(self):
        self.widget.dispose()
```

若 setUp() 成功运行，无论测试方法是否成功，都会运行 tearDown() 。

这样的一个测试代码运行的环境被称为 test fixture 。一个新的 TestCase 实例作为一个测试脚手架，用于运行各个独立的测试方法。在运行每个测试时，setUp() 、tearDown() 和 \_\_init__() 会被调用一次。

推荐你根据用例所测试的功能将测试用 TestCase 分组。unittest 为此提供了 test suite：unittest 的 TestSuite 类是一个代表。通常情况下，调用 unittest.main() 就能正确地找到并执行这个模块下所有用 TestCase 分组的测试。

然而，如果你需要自定义你的测试套件的话，你可以参考以下方法组织你的测试：

```python
def suite():
    suite = unittest.TestSuite()
    suite.addTest(WidgetTestCase('test_default_widget_size'))
    suite.addTest(WidgetTestCase('test_widget_resize'))
    return suite

if __name__ == '__main__':
    runner = unittest.TextTestRunner()
    runner.run(suite())
```

你可以把测试用例和测试套件放在与被测试代码相同的模块中（比如 widget.py），但将测试代码放在单独的模块中（比如 test_widget.py）有几个优势。

- 测试模块可以从命令行被独立调用。
- 更容易在分发的代码中剥离测试代码。
- 降低没有好理由的情况下修改测试代码以通过测试的诱惑。
- 测试代码应比被测试代码更少地被修改。
- 被测试代码可以更容易地被重构。
- 对用 C 语言写成的模块无论如何都得单独写成一个模块，为什么不保持一致呢？
- 如果测试策略发生了改变，没有必要修改源代码。

### Reusing Old Test code

一些用户希望直接使用 unittest 运行已有的测试代码，而不需要把已有的每个测试函数转化为一个 TestCase 的子类。

因此， unittest 提供 FunctionTestCase 类。这个 TestCase 的子类可用于打包已有的测试函数，并支持设置前置与后置函数。

假定有一个测试函数：

```python
def testSomething():
    something = makeSomething()
    assert something.name is not None
    # ...
```

可以创建等价的测试用例如下，其中前置和后置方法是可选的。

```python
testcase = unittest.FunctionTestCase(testSomething,
                                     setUp=makeSomethingDB,
                                     tearDown=deleteSomethingDB)
```
注解：用 FunctionTestCase 可以快速将现有的测试转换成基于 unittest 的测试，但不推荐你这样做。花点时间继承 TestCase 会让以后重构测试无比轻松。
在某些情况下，现有的测试可能是用 doctest 模块编写的。 如果是这样， doctest 提供了一个 DocTestSuite 类，可以从现有基于 doctest 的测试中自动构建 unittest.TestSuite 用例。

### Skipping Tests & Expected Failures

Unittest 支持跳过单个或整组的测试用例。它还支持把测试标注成“预期失败”的测试。这些坏测试会失败，但不会算进 TestResult 的失败里。

要跳过测试只需使用 skip() decorator 或其附带条件的版本，在 setUp() 内部使用 TestCase.skipTest()，或是直接引发 SkipTest。

跳过测试的基本用法如下:

```python
class MyTestCase(unittest.TestCase):

    @unittest.skip("demonstrating skipping")
    def test_nothing(self):
        self.fail("shouldn't happen")

    @unittest.skipIf(mylib.__version__ < (1, 3),
                     "not supported in this library version")
    def test_format(self):
        # Tests that work for only a certain version of the library.
        pass

    @unittest.skipUnless(sys.platform.startswith("win"), "requires Windows")
    def test_windows_support(self):
        # windows specific testing code
        pass

    def test_maybe_skipped(self):
        if not external_resource_available():
            self.skipTest("external resource not available")
        # test code that depends on the external resource
        pass
```

跳过测试类的写法跟跳过测试方法的写法相似:

```python
@unittest.skip("showing class skipping")
class MySkippedTestCase(unittest.TestCase):
    def test_not_run(self):
        pass
```

TestCase.setUp() 也可以跳过测试。可以用于所需资源不可用的情况下跳过接下来的测试。

使用 expectedFailure() 装饰器表明这个测试预计失败。:

```python
class ExpectedFailureTestCase(unittest.TestCase):
    @unittest.expectedFailure
    def test_fail(self):
        self.assertEqual(1, 0, "broken")
```

你可以很容易地编写在测试时调用 skip() 的装饰器作为自定义的跳过测试装饰器。 下面这个装饰器会跳过测试，除非所传入的对象具有特定的属性:

```python
def skipUnlessHasattr(obj, attr):
    if hasattr(obj, attr):
        return lambda func: func
    return unittest.skip("{!r} doesn't have {!r}".format(obj, attr)) # {!r}即%r，表示用repr()处理，调用其__repr__函数
```

以下的装饰器和异常实现了跳过测试和预期失败两种功能：

- @unittest.skip(reason)
  - 跳过被此装饰器装饰的测试。 reason 为测试被跳过的原因。

- @unittest.skipIf(condition, reason)
  - 当 condition 为真时，跳过被装饰的测试。

- @unittest.skipUnless(condition, reason)
  - 跳过被装饰的测试，除非 condition 为真。

- @unittest.expectedFailure
  - 将测试标记为预期的失败或错误。 如果测试失败或在测试函数自身（而非在某个 test fixture 方法）中出现错误则将认为是测试成功。 如果测试通过，则将认为是测试失败。

- exception unittest.SkipTest(reason)
  - 引发此异常以跳过一个测试。
  - 通常来说，你可以使用 TestCase.skipTest() 或其中一个跳过测试的装饰器实现跳过测试的功能，而不是直接引发此异常。

被跳过的测试的 setUp() 和 tearDown() 不会被运行。被跳过的类的 setUpClass() 和 tearDownClass() 不会被运行。被跳过的模组的 setUpModule() 和 tearDownModule() 不会被运行。

### Distinguishing Test Iterations Using Subtests

当你的几个测试之间的差异非常小，例如只有某些形参不同时，unittest 允许你使用 subTest() 上下文管理器在一个测试方法体的内部区分它们。

例如，以下测试:

```python
class NumbersTest(unittest.TestCase):

    def test_even(self):
        """
        Test that numbers between 0 and 5 are all even.
        """
        for i in range(0, 6):
            with self.subTest(i=i):
                self.assertEqual(i % 2, 0)
```

可以得到以下输出:

```bash
======================================================================
FAIL: test_even (__main__.NumbersTest) (i=1)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "subtests.py", line 32, in test_even
    self.assertEqual(i % 2, 0)
AssertionError: 1 != 0

======================================================================
FAIL: test_even (__main__.NumbersTest) (i=3)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "subtests.py", line 32, in test_even
    self.assertEqual(i % 2, 0)
AssertionError: 1 != 0

======================================================================
FAIL: test_even (__main__.NumbersTest) (i=5)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "subtests.py", line 32, in test_even
    self.assertEqual(i % 2, 0)
AssertionError: 1 != 0
```
如果不使用子测试，程序遇到第一次错误之后就会停止。而且因为``i``的值不显示，错误也更难找。

```bash
======================================================================
FAIL: test_even (__main__.NumbersTest)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "subtests.py", line 32, in test_even
    self.assertEqual(i % 2, 0)
AssertionError: 1 != 0
```

### Classes & Functions

#### Test Cases

##### class unittest.TestCase(methodName='runTest')

TestCase 类的实例代表了 unittest 宇宙中的逻辑测试单元。 该类旨在被当作基类使用，特定的测试将由其实体子类来实现。 该类实现了测试运行器所需的接口以允许它驱动测试，并实现了可被测试代码用来检测和报告各种类型的失败的方法。

每个 TestCase 实例将运行一个单位的基础方法：即名为 methodName 的方法。 在使用 TestCase 的大多数场景中，你都不需要修改 methodName 或重新实现默认的 runTest() 方法。

在 3.2 版更改: TestCase 不需要提供 methodName 即可成功实例化。 这使得从交互式解释器试验 TestCase 更为容易。

TestCase 的实例提供了三组方法：一组用来运行测试，另一组被测试实现用来检查条件和报告失败，还有一些查询方法用来收集有关测试本身的信息。

---

第一组（用于运行测试的）方法是：

- setUp()
  - 为测试预备而调用的方法。 此方法会在调用测试方法之前被调用；除了 AssertionError 或 SkipTest，此方法所引发的任何异常都将被视为错误而非测试失败。 默认的实现将不做任何事情。

- tearDown()
  - 在测试方法被调用并记录结果之后立即被调用的方法。 此方法即使在测试方法引发异常时仍会被调用，因此子类中的实现将需要特别注意检查内部状态。 除 AssertionError 或 SkipTest 外，此方法所引发的任何异常都将被视为额外的错误而非测试失败（因而会增加总计错误报告数）。 此方法将只在 setUp() 成功执行时被调用，无论测试方法的结果如何。 默认的实现将不做任何事情。

- setUpClass()3.2 新版功能
  - 在一个单独类中的测试运行之前被调用的类方法。 setUpClass 会被作为唯一的参数在类上调用且必须使用 classmethod() 装饰器:
    ```python
    @classmethod
    def setUpClass(cls):
        ...
    ```

- tearDownClass()3.2 新版功能
  - 在一个单独类的测试完成运行之后被调用的类方法。 tearDownClass 会被作为唯一的参数在类上调用且必须使用 classmethod() 装饰器:
    ```python
    @classmethod
    def tearDownClass(cls):
        ...
    ```

- run(result=None)
  - 运行测试，将结果收集至作为 result 传入的 TestResult。 如果 result 被省略或为 None，则会创建一个临时的结果对象（通过调用 defaultTestResult() 方法）并使用它。 结果对象会被返回给 run() 的调用方。
  - 同样的效果也可通过简单地调用 TestCase 实例来达成。
  - 在 3.3 版更改: 之前版本的 run 不会返回结果。 也不会对实例执行调用。

- skipTest(reason)3.1 新版功能
  - 在测试方法或 setUp() 执行期间调用此方法将跳过当前测试。 详情参见 跳过测试与预计的失败。

- subTest(msg=None, **params)3.4 新版功能
  - 返回一个上下文管理器以将其中的代码块作为子测试来执行。 可选的 msg 和 params 是将在子测试失败时显示的任意值，以便让你能清楚地标识它们。
  - 一个测试用例可以包含任意数量的子测试声明，并且它们可以任意地嵌套。

- debug()
  - 运行测试而不收集结果。 这允许测试所引发的异常被传递给调用方，并可被用于支持在调试器中运行测试。

---

TestCase 类提供了一些断言方法用于检查并报告失败。 下表列出了最常用的方法（请查看下文的其他表来了解更多的断言方法）:

<div class="responsive-table__container"><table class="docutils align-default">
<colgroup>
<col style="width: 48%">
<col style="width: 34%">
<col style="width: 18%">
</colgroup>
<thead>
<tr class="row-odd"><th class="head"><p>方法</p></th>
<th class="head"><p>检查对象</p></th>
<th class="head"><p>引入版本</p></th>
</tr>
</thead>
<tbody>
<tr class="row-even"><td><p><a class="reference internal" href="#unittest.TestCase.assertEqual" title="unittest.TestCase.assertEqual"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertEqual(a,</span> <span class="pre">b)</span></code></a></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">a</span> <span class="pre">==</span> <span class="pre">b</span></code></p></td>
<td></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="#unittest.TestCase.assertNotEqual" title="unittest.TestCase.assertNotEqual"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertNotEqual(a,</span> <span class="pre">b)</span></code></a></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">a</span> <span class="pre">!=</span> <span class="pre">b</span></code></p></td>
<td></td>
</tr>
<tr class="row-even"><td><p><a class="reference internal" href="#unittest.TestCase.assertTrue" title="unittest.TestCase.assertTrue"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertTrue(x)</span></code></a></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">bool(x)</span> <span class="pre">is</span> <span class="pre">True</span></code></p></td>
<td></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="#unittest.TestCase.assertFalse" title="unittest.TestCase.assertFalse"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertFalse(x)</span></code></a></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">bool(x)</span> <span class="pre">is</span> <span class="pre">False</span></code></p></td>
<td></td>
</tr>
<tr class="row-even"><td><p><a class="reference internal" href="#unittest.TestCase.assertIs" title="unittest.TestCase.assertIs"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertIs(a,</span> <span class="pre">b)</span></code></a></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">a</span> <span class="pre">is</span> <span class="pre">b</span></code></p></td>
<td><p>3.1</p></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="#unittest.TestCase.assertIsNot" title="unittest.TestCase.assertIsNot"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertIsNot(a,</span> <span class="pre">b)</span></code></a></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">a</span> <span class="pre">is</span> <span class="pre">not</span> <span class="pre">b</span></code></p></td>
<td><p>3.1</p></td>
</tr>
<tr class="row-even"><td><p><a class="reference internal" href="#unittest.TestCase.assertIsNone" title="unittest.TestCase.assertIsNone"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertIsNone(x)</span></code></a></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">x</span> <span class="pre">is</span> <span class="pre">None</span></code></p></td>
<td><p>3.1</p></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="#unittest.TestCase.assertIsNotNone" title="unittest.TestCase.assertIsNotNone"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertIsNotNone(x)</span></code></a></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">x</span> <span class="pre">is</span> <span class="pre">not</span> <span class="pre">None</span></code></p></td>
<td><p>3.1</p></td>
</tr>
<tr class="row-even"><td><p><a class="reference internal" href="#unittest.TestCase.assertIn" title="unittest.TestCase.assertIn"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertIn(a,</span> <span class="pre">b)</span></code></a></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">a</span> <span class="pre">in</span> <span class="pre">b</span></code></p></td>
<td><p>3.1</p></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="#unittest.TestCase.assertNotIn" title="unittest.TestCase.assertNotIn"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertNotIn(a,</span> <span class="pre">b)</span></code></a></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">a</span> <span class="pre">not</span> <span class="pre">in</span> <span class="pre">b</span></code></p></td>
<td><p>3.1</p></td>
</tr>
<tr class="row-even"><td><p><a class="reference internal" href="#unittest.TestCase.assertIsInstance" title="unittest.TestCase.assertIsInstance"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertIsInstance(a,</span> <span class="pre">b)</span></code></a></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">isinstance(a,</span> <span class="pre">b)</span></code></p></td>
<td><p>3.2</p></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="#unittest.TestCase.assertNotIsInstance" title="unittest.TestCase.assertNotIsInstance"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertNotIsInstance(a,</span> <span class="pre">b)</span></code></a></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">not</span> <span class="pre">isinstance(a,</span> <span class="pre">b)</span></code></p></td>
<td><p>3.2</p></td>
</tr>
</tbody>
</table></div>

这些断言方法都支持 msg 参数，如果指定了该参数，它将被用作测试失败时的错误消息 (另请参阅 longMessage)。 请注意将 msg 关键字参数传给 assertRaises(), assertRaisesRegex(), assertWarns(), assertWarnsRegex() 的前提是它们必须被用作上下文管理器。

- assertEqual(first, second, msg=None)
  - 测试 first 和 second 是否相等。 如果两个值的比较结果是不相等，则测试将失败。
  - 此外，如果 first 和 second 的类型完全相同且属于 list, tuple, dict, set, frozenset 或 str 或者属于通过 addTypeEqualityFunc() 注册子类的类型则将会调用类型专属的相等判断函数以便生成更有用的默认错误消息 (另请参阅 类型专属方法列表)。
  - 在 3.1 版更改: 增加了对类型专属的相等判断函数的自动调用。
  - 在 3.2 版更改: 增加了 assertMultiLineEqual() 作为用于比较字符串的默认类型相等判断函数。

- assertNotEqual(first, second, msg=None)
  - 测试 first 和 second 是否不等。 如果两个值的比较结果是相等，则测试将失败。

- assertTrue(expr, msg=None) & assertFalse(expr, msg=None)
  - 测试 expr 是否为真值（或假值）。
  - 请注意这等价于 bool(expr) is True 而不等价于 expr is True (后者要使用 assertIs(expr, True))。 
  - 当存在更专门的方法时也应避免使用此方法 (例如应使用 assertEqual(a, b) 而不是 assertTrue(a == b))，因为它们在测试失败时会提供更有用的错误消息。

- assertIs(first, second, msg=None) & assertIsNot(first, second, msg=None)3.1 新版功能
  - 测试 first 和 second 是 (或不是) 同一个对象。

- assertIsNone(expr, msg=None) & assertIsNotNone(expr, msg=None)3.1 新版功能
  - 测试 expr 是 (或不是) None。

- assertIn(member, container, msg=None) & assertNotIn(member, container, msg=None)3.1 新版功能
  - 测试 member 是 (或不是) container 的成员。

- assertIsInstance(obj, cls, msg=None) & assertNotIsInstance(obj, cls, msg=None)3.2 新版功能
  - 测试 obj 是 (或不是) cls (此参数可以为一个类或包含类的元组，即 isinstance() 所接受的参数) 的实例。
  - 要检测是否为指定类型，请使用 assertIs(type(obj), cls)。

还可以使用下列方法来检查异常、警告和日志消息的产生:

<div class="responsive-table__container"><table class="docutils align-default">
<colgroup>
<col style="width: 53%">
<col style="width: 36%">
<col style="width: 11%">
</colgroup>
<thead>
<tr class="row-odd"><th class="head"><p>方法</p></th>
<th class="head"><p>检查对象</p></th>
<th class="head"><p>引入版本</p></th>
</tr>
</thead>
<tbody>
<tr class="row-even"><td><p><a class="reference internal" href="#unittest.TestCase.assertRaises" title="unittest.TestCase.assertRaises"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertRaises(exc,</span> <span class="pre">fun,</span> <span class="pre">*args,</span> <span class="pre">**kwds)</span></code></a></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">fun(*args,</span> <span class="pre">**kwds)</span></code> 引发了 <em>exc</em></p></td>
<td></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="#unittest.TestCase.assertRaisesRegex" title="unittest.TestCase.assertRaisesRegex"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertRaisesRegex(exc,</span> <span class="pre">r,</span> <span class="pre">fun,</span> <span class="pre">*args,</span> <span class="pre">**kwds)</span></code></a></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">fun(*args,</span> <span class="pre">**kwds)</span></code> 引发了 <em>exc</em> 并且消息可与正则表达式 <em>r</em> 相匹配</p></td>
<td><p>3.1</p></td>
</tr>
<tr class="row-even"><td><p><a class="reference internal" href="#unittest.TestCase.assertWarns" title="unittest.TestCase.assertWarns"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertWarns(warn,</span> <span class="pre">fun,</span> <span class="pre">*args,</span> <span class="pre">**kwds)</span></code></a></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">fun(*args,</span> <span class="pre">**kwds)</span></code> 引发了 <em>warn</em></p></td>
<td><p>3.2</p></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="#unittest.TestCase.assertWarnsRegex" title="unittest.TestCase.assertWarnsRegex"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertWarnsRegex(warn,</span> <span class="pre">r,</span> <span class="pre">fun,</span> <span class="pre">*args,</span> <span class="pre">**kwds)</span></code></a></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">fun(*args,</span> <span class="pre">**kwds)</span></code> 引发了 <em>warn</em> 并且消息可与正则表达式 <em>r</em> 相匹配</p></td>
<td><p>3.2</p></td>
</tr>
<tr class="row-even"><td><p><a class="reference internal" href="#unittest.TestCase.assertLogs" title="unittest.TestCase.assertLogs"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertLogs(logger,</span> <span class="pre">level)</span></code></a></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">with</span></code> 代码块在 <em>logger</em> 上使用了最小的 <em>level</em> 级别写入日志</p></td>
<td><p>3.4</p></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="#unittest.TestCase.assertNoLogs" title="unittest.TestCase.assertNoLogs"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertNoLogs(logger,</span> <span class="pre">level)</span></code></a></p></td>
<td><dl class="simple">
<dt><code class="docutils literal notranslate"><span class="pre">with</span></code> 代码块没有在</dt><dd><p><em>logger</em> 上使用最小的 <em>level</em> 级别写入日志</p>
</dd>
</dl>
</td>
<td><p>3.10</p></td>
</tr>
</tbody>
</table></div>

- assertRaises(exception, callable, *args, **kwds) & assertRaises(exception, *, msg=None)
  - 测试当 callable 附带任何同时被传给 assertRaises() 的位置或关键字参数被调用时是否引发了异常。 如果引发了 exception 则测试通过，如果引发了另一个异常则报错，或者如果未引发任何异常则测试失败。 要捕获一组异常中的任何一个，可以将包含多个异常类的元组作为 exception 传入。
  - 如果只给出了 exception 和可能的 msg 参数，则返回一个上下文管理器以便被测试的代码可以被写成内联形式而不是被写成函数:
    ```python
    with self.assertRaises(SomeException):
        do_something()
    ```
  - 当被作为上下文管理器使用时，assertRaises() 接受额外的关键字参数 msg。
  - 上下文管理器将把捕获的异常对象存入在其 exception 属性中。 这适用于需要对所引发异常执行额外检查的场合:
    ```python
    with self.assertRaises(SomeException) as cm:
        do_something()

    the_exception = cm.exception
    self.assertEqual(the_exception.error_code, 3)
    ```
  - 在 3.1 版更改: 添加了将 assertRaises() 用作上下文管理器的功能。
  - 在 3.2 版更改: 增加了 exception 属性。
  - 在 3.3 版更改: 增加了 msg 关键字参数在作为上下文管理器时使用。

- assertRaisesRegex(exception, regex, callable, *args, **kwds) & assertRaisesRegex(exception, regex, *, msg=None)
  - 与 assertRaises() 类似但还会测试 regex 是否匹配被引发异常的字符串表示形式。 regex 可以是一个正则表达式对象或包含正则表达式的字符串以提供给 re.search() 使用。 例如:
    ```python
    self.assertRaisesRegex(ValueError, "invalid literal for.*XYZ'$", int, 'XYZ')
    ```
  - 或者：
    ```python
    with self.assertRaisesRegex(ValueError, 'literal'):
        int('XYZ')
    ```
  - 3.1 新版功能: 以方法名``assertRaisesRegexp``添加。
  - 在 3.2 版更改: 重命名为 assertRaisesRegex()。
  - 在 3.3 版更改: 增加了 msg 关键字参数在作为上下文管理器时使用。

- assertWarns(warning, callable, *args, **kwds) & assertWarns(warning, *, msg=None)3.2 新版功能
  - 测试当 callable 附带任何同时被传给 assertWarns() 的位置或关键字参数被调用时是否触发了警告。 如果触发了 warning 则测试通过，否则测试失败。 引发任何异常则报错。 要捕获一组警告中的任何一个，可将包含多个警告类的元组作为 warnings 传入。
  - 如果只给出了 warning 和可能的 msg 参数，则返回一个上下文管理器以便被测试的代码可以被写成内联形式而不是被写成函数:
    ```python
    with self.assertWarns(SomeWarning):
        do_something()
    ```
  - 当被作为上下文管理器使用时，assertWarns() 接受额外的关键字参数 msg。
  - 上下文管理器将把捕获的警告对象保存在其 warning 属性中，并把触发警告的源代码行保存在 filename 和 lineno 属性中。 这适用于需要对捕获的警告执行额外检查的场合:
    ```python
    with self.assertWarns(SomeWarning) as cm:
        do_something()

    self.assertIn('myfile.py', cm.filename)
    self.assertEqual(320, cm.lineno)
    ```
  - 无论被调用时警告过滤器是否就位此方法均可工作。
  - 在 3.3 版更改: 增加了 msg 关键字参数在作为上下文管理器时使用。

- assertWarnsRegex(warning, regex, callable, *args, **kwds) & assertWarnsRegex(warning, regex, *, msg=None)3.2 新版功能
  - 与 assertWarns() 类似但还会测试 regex 是否匹配被触发警告的消息文本。 regex 可以是一个正则表达式对象或包含正则表达式的字符串以提供给 re.search() 使用。 例如:
    ```python
    self.assertWarnsRegex(DeprecationWarning,
                        r'legacy_function\(\) is deprecated',
                        legacy_function, 'XYZ')
    ```
  - 或者：
    ```python
    with self.assertWarnsRegex(RuntimeWarning, 'unsafe frobnicating'):
        frobnicate('/etc/passwd')
    ```
  - 在 3.3 版更改: 增加了 msg 关键字参数在作为上下文管理器时使用。

- assertLogs(logger=None, level=None)3.4 新版功能
  - 一个上下文管理器，它测试在 logger 或其子对象上是否至少记录了一条至少为指定 level 以上级别的消息。
  - 如果给出了 logger 则它应为一个 logging.Logger 对象或为一个指定日志记录器名称的 str。 默认为根日志记录器，它将捕获未被非传播型后继日志记录器所拦阻的所有消息。
  - 如果给出了 level 则它应为一个用数字表示的日志记录级别或其字符串形式 (例如 "ERROR" 或 logging.ERROR)。 默认为 logging.INFO。
  - 如果在 with 代码块内部发出了至少一条与 logger 和 level 条件相匹配的消息则测试通过，否则测试失败。
  - 上下文管理器返回的对象是一个记录辅助器，它会记录所匹配的日志消息。 它有两个属性:
    - records
      - 所匹配的日志消息 logging.LogRecord 对象组成的列表。
    - output
      - A list of str objects with the formatted output of matching messages.
  - 示例:
    ```python
    with self.assertLogs('foo', level='INFO') as cm:
    logging.getLogger('foo').info('first message')
    logging.getLogger('foo.bar').error('second message')
    self.assertEqual(cm.output, ['INFO:foo:first message',
                                'ERROR:foo.bar:second message'])
    ```

- assertNoLogs(logger=None, level=None)3.10 新版功能
  - A context manager to test that no messages are logged on the logger or one of its children, with at least the given level.
  - If given, logger should be a logging.Logger object or a str giving the name of a logger. The default is the root logger, which will catch all messages.
  - 如果给出了 level 则它应为一个用数字表示的日志记录级别或其字符串形式 (例如 "ERROR" 或 logging.ERROR)。 默认为 logging.INFO。
  - Unlike assertLogs(), nothing will be returned by the context manager.

---

There are also other methods used to perform more specific checks, such as:

<div class="responsive-table__container"><table class="docutils align-default">
<colgroup>
<col style="width: 46%">
<col style="width: 38%">
<col style="width: 16%">
</colgroup>
<thead>
<tr class="row-odd"><th class="head"><p>方法</p></th>
<th class="head"><p>检查对象</p></th>
<th class="head"><p>引入版本</p></th>
</tr>
</thead>
<tbody>
<tr class="row-even"><td><p><a class="reference internal" href="#unittest.TestCase.assertAlmostEqual" title="unittest.TestCase.assertAlmostEqual"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertAlmostEqual(a,</span> <span class="pre">b)</span></code></a></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">round(a-b,</span> <span class="pre">7)</span> <span class="pre">==</span> <span class="pre">0</span></code></p></td>
<td></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="#unittest.TestCase.assertNotAlmostEqual" title="unittest.TestCase.assertNotAlmostEqual"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertNotAlmostEqual(a,</span> <span class="pre">b)</span></code></a></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">round(a-b,</span> <span class="pre">7)</span> <span class="pre">!=</span> <span class="pre">0</span></code></p></td>
<td></td>
</tr>
<tr class="row-even"><td><p><a class="reference internal" href="#unittest.TestCase.assertGreater" title="unittest.TestCase.assertGreater"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertGreater(a,</span> <span class="pre">b)</span></code></a></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">a</span> <span class="pre">&gt;</span> <span class="pre">b</span></code></p></td>
<td><p>3.1</p></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="#unittest.TestCase.assertGreaterEqual" title="unittest.TestCase.assertGreaterEqual"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertGreaterEqual(a,</span> <span class="pre">b)</span></code></a></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">a</span> <span class="pre">&gt;=</span> <span class="pre">b</span></code></p></td>
<td><p>3.1</p></td>
</tr>
<tr class="row-even"><td><p><a class="reference internal" href="#unittest.TestCase.assertLess" title="unittest.TestCase.assertLess"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertLess(a,</span> <span class="pre">b)</span></code></a></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">a</span> <span class="pre">&lt;</span> <span class="pre">b</span></code></p></td>
<td><p>3.1</p></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="#unittest.TestCase.assertLessEqual" title="unittest.TestCase.assertLessEqual"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertLessEqual(a,</span> <span class="pre">b)</span></code></a></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">a</span> <span class="pre">&lt;=</span> <span class="pre">b</span></code></p></td>
<td><p>3.1</p></td>
</tr>
<tr class="row-even"><td><p><a class="reference internal" href="#unittest.TestCase.assertRegex" title="unittest.TestCase.assertRegex"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertRegex(s,</span> <span class="pre">r)</span></code></a></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">r.search(s)</span></code></p></td>
<td><p>3.1</p></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="#unittest.TestCase.assertNotRegex" title="unittest.TestCase.assertNotRegex"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertNotRegex(s,</span> <span class="pre">r)</span></code></a></p></td>
<td><p><code class="docutils literal notranslate"><span class="pre">not</span> <span class="pre">r.search(s)</span></code></p></td>
<td><p>3.2</p></td>
</tr>
<tr class="row-even"><td><p><a class="reference internal" href="#unittest.TestCase.assertCountEqual" title="unittest.TestCase.assertCountEqual"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertCountEqual(a,</span> <span class="pre">b)</span></code></a></p></td>
<td><p><em>a</em> and <em>b</em> have the same
elements in the same number,
regardless of their order.</p></td>
<td><p>3.2</p></td>
</tr>
</tbody>
</table></div>

- assertAlmostEqual(first, second, places=7, msg=None, delta=None) & assertNotAlmostEqual(first, second, places=7, msg=None, delta=None)
  - Test that first and second are approximately (or not approximately) equal by computing the difference, rounding to the given number of decimal places (default 7), and comparing to zero. Note that these methods round the values to the given number of decimal places (i.e. like the round() function) and not significant digits.
  - If delta is supplied instead of places then the difference between first and second must be less or equal to (or greater than) delta.
  - Supplying both delta and places raises a TypeError.
  - 在 3.2 版更改: assertAlmostEqual() automatically considers almost equal objects that compare equal. assertNotAlmostEqual() automatically fails if the objects compare equal. Added the delta keyword argument.

- assertGreater(first, second, msg=None) & assertGreaterEqual(first, second, msg=None) & assertLess(first, second, msg=None) & assertLessEqual(first, second, msg=None)3.1 新版功能
  - Test that first is respectively >, >=, < or <= than second depending on the method name. If not, the test will fail:
    ```>>>
    >>> self.assertGreaterEqual(3, 4)
    AssertionError: "3" unexpectedly not greater than or equal to "4"
    ```

- assertRegex(text, regex, msg=None) & assertNotRegex(text, regex, msg=None)
  - 测试一个 regex 搜索匹配（或不匹配） 文本。如果不匹配，错误信息中将包含匹配模式和 文本*（或部分匹配失败的 *文本）。regex 可以是正则表达式对象或能够用于 re.search() 的包含正则表达式的字符串。
  - 3.1 新版功能: 以方法名``assertRegexpMatches``添加。
  - 在 3.2 版更改: 方法 assertRegexpMatches() 已被改名为 assertRegex()。
  - 3.2 新版功能: assertNotRegex()
  - 3.5 新版功能: assertNotRegexpMatches 这个名字是 assertNotRegex() 的已被弃用的别名。

- assertCountEqual(first, second, msg=None)3.2 新版功能
  - Test that sequence first contains the same elements as second, regardless of their order. When they don't, an error message listing the differences between the sequences will be generated.
  - Duplicate elements are not ignored when comparing first and second. It verifies whether each element has the same count in both sequences. Equivalent to: assertEqual(Counter(list(first)), Counter(list(second))) but works with sequences of unhashable objects as well.

The assertEqual() method dispatches the equality check for objects of the same type to different type-specific methods. These methods are already implemented for most of the built-in types, but it's also possible to register new methods using addTypeEqualityFunc():

- addTypeEqualityFunc(typeobj, function)3.1 新版功能
  - Registers a type-specific method called by assertEqual() to check if two objects of exactly the same typeobj (not subclasses) compare equal. function must take two positional arguments and a third msg=None keyword argument just as assertEqual() does. It must raise self.failureException(msg) when inequality between the first two parameters is detected -- possibly providing useful information and explaining the inequalities in details in the error message.

以下是 assertEqual() 自动选用的不同类型的比较方法。一般情况下不需要直接在测试中调用这些方法。

<div class="responsive-table__container"><table class="docutils align-default">
<colgroup>
<col style="width: 49%">
<col style="width: 35%">
<col style="width: 17%">
</colgroup>
<thead>
<tr class="row-odd"><th class="head"><p>方法</p></th>
<th class="head"><p>用作比较</p></th>
<th class="head"><p>引入版本</p></th>
</tr>
</thead>
<tbody>
<tr class="row-even"><td><p><a class="reference internal" href="#unittest.TestCase.assertMultiLineEqual" title="unittest.TestCase.assertMultiLineEqual"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertMultiLineEqual(a,</span> <span class="pre">b)</span></code></a></p></td>
<td><p>字符串</p></td>
<td><p>3.1</p></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="#unittest.TestCase.assertSequenceEqual" title="unittest.TestCase.assertSequenceEqual"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertSequenceEqual(a,</span> <span class="pre">b)</span></code></a></p></td>
<td><p>序列</p></td>
<td><p>3.1</p></td>
</tr>
<tr class="row-even"><td><p><a class="reference internal" href="#unittest.TestCase.assertListEqual" title="unittest.TestCase.assertListEqual"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertListEqual(a,</span> <span class="pre">b)</span></code></a></p></td>
<td><p>列表</p></td>
<td><p>3.1</p></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="#unittest.TestCase.assertTupleEqual" title="unittest.TestCase.assertTupleEqual"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertTupleEqual(a,</span> <span class="pre">b)</span></code></a></p></td>
<td><p>元组</p></td>
<td><p>3.1</p></td>
</tr>
<tr class="row-even"><td><p><a class="reference internal" href="#unittest.TestCase.assertSetEqual" title="unittest.TestCase.assertSetEqual"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertSetEqual(a,</span> <span class="pre">b)</span></code></a></p></td>
<td><p>集合</p></td>
<td><p>3.1</p></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="#unittest.TestCase.assertDictEqual" title="unittest.TestCase.assertDictEqual"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertDictEqual(a,</span> <span class="pre">b)</span></code></a></p></td>
<td><p>字典</p></td>
<td><p>3.1</p></td>
</tr>
</tbody>
</table></div>

- assertMultiLineEqual(first, second, msg=None)3.1 新版功能
  - Test that the multiline string first is equal to the string second. When not equal a diff of the two strings highlighting the differences will be included in the error message. This method is used by default when comparing strings with assertEqual().

- assertSequenceEqual(first, second, msg=None, seq_type=None)3.1 新版功能
  - Tests that two sequences are equal. If a seq_type is supplied, both first and second must be instances of seq_type or a failure will be raised. If the sequences are different an error message is constructed that shows the difference between the two.
  - This method is not called directly by assertEqual(), but it's used to implement assertListEqual() and assertTupleEqual().

- assertListEqual(first, second, msg=None) & assertTupleEqual(first, second, msg=None)3.1 新版功能
  - Tests that two lists or tuples are equal. If not, an error message is constructed that shows only the differences between the two. An error is also raised if either of the parameters are of the wrong type. These methods are used by default when comparing lists or tuples with assertEqual().

- assertSetEqual(first, second, msg=None)3.1 新版功能
  - Tests that two sets are equal. If not, an error message is constructed that lists the differences between the sets. This method is used by default when comparing sets or frozensets with assertEqual().
  - Fails if either of first or second does not have a set.difference() method.

- assertDictEqual(first, second, msg=None)3.1 新版功能
  - Test that two dictionaries are equal. If not, an error message is constructed that shows the differences in the dictionaries. This method will be used by default to compare dictionaries in calls to assertEqual().

Finally the TestCase provides the following methods and attributes:

- fail(msg=None)
  - Signals a test failure unconditionally, with msg or None for the error message.

- failureException
  - This class attribute gives the exception raised by the test method. If a test framework needs to use a specialized exception, possibly to carry additional information, it must subclass this exception in order to "play fair" with the framework. The initial value of this attribute is AssertionError.

- longMessage3.1 新版功能
  - This class attribute determines what happens when a custom failure message is passed as the msg argument to an assertXYY call that fails. True is the default value. In this case, the custom message is appended to the end of the standard failure message. When set to False, the custom message replaces the standard message.
  - The class setting can be overridden in individual test methods by assigning an instance attribute, self.longMessage, to True or False before calling the assert methods.
  - The class setting gets reset before each test call.

- maxDiff3.2 新版功能
  - This attribute controls the maximum length of diffs output by assert methods that report diffs on failure. It defaults to 80*8 characters. Assert methods affected by this attribute are assertSequenceEqual() (including all the sequence comparison methods that delegate to it), assertDictEqual() and assertMultiLineEqual().
  - Setting maxDiff to None means that there is no maximum length of diffs.

Testing frameworks can use the following methods to collect information on the test:

- countTestCases()
  - Return the number of tests represented by this test object. For TestCase instances, this will always be 1.

- defaultTestResult()
  - Return an instance of the test result class that should be used for this test case class (if no other result instance is provided to the run() method).
  - For TestCase instances, this will always be an instance of TestResult; subclasses of TestCase should override this as necessary.

- id()
  - Return a string identifying the specific test case. This is usually the full name of the test method, including the module and class name.

- shortDescription()
  - Returns a description of the test, or None if no description has been provided. The default implementation of this method returns the first line of the test method's docstring, if available, or None.
  - 在 3.1 版更改: In 3.1 this was changed to add the test name to the short description even in the presence of a docstring. This caused compatibility issues with unittest extensions and adding the test name was moved to the TextTestResult in Python 3.2.

- addCleanup(function, /, *args, **kwargs)3.1 新版功能
  - Add a function to be called after tearDown() to cleanup resources used during the test. Functions will be called in reverse order to the order they are added (LIFO). They are called with any arguments and keyword arguments passed into addCleanup() when they are added.
  - If setUp() fails, meaning that tearDown() is not called, then any cleanup functions added will still be called.

- doCleanups()3.1 新版功能
  - This method is called unconditionally after tearDown(), or after setUp() if setUp() raises an exception.
  - It is responsible for calling all the cleanup functions added by addCleanup(). If you need cleanup functions to be called prior to tearDown() then you can call doCleanups() yourself.
  - doCleanups() pops methods off the stack of cleanup functions one at a time, so it can be called at any time.

- classmethod addClassCleanup(function, /, *args, **kwargs)3.8 新版功能
  - Add a function to be called after tearDownClass() to cleanup resources used during the test class. Functions will be called in reverse order to the order they are added (LIFO). They are called with any arguments and keyword arguments passed into addClassCleanup() when they are added.
  - If setUpClass() fails, meaning that tearDownClass() is not called, then any cleanup functions added will still be called.

- classmethod doClassCleanups()3.8 新版功能
  - This method is called unconditionally after tearDownClass(), or after setUpClass() if setUpClass() raises an exception.
  - It is responsible for calling all the cleanup functions added by addClassCleanup(). If you need cleanup functions to be called prior to tearDownClass() then you can call doClassCleanups() yourself.
  - doClassCleanups() pops methods off the stack of cleanup functions one at a time, so it can be called at any time.

##### class unittest.IsolatedAsyncioTestCase(methodName='runTest')3.8 新版功能

This class provides an API similar to TestCase and also accepts coroutines as test functions.

- coroutine asyncSetUp()
  - Method called to prepare the test fixture. This is called after setUp(). This is called immediately before calling the test method; other than AssertionError or SkipTest, any exception raised by this method will be considered an error rather than a test failure. The default implementation does nothing.

- coroutine asyncTearDown()
  - Method called immediately after the test method has been called and the result recorded. This is called before tearDown(). This is called even if the test method raised an exception, so the implementation in subclasses may need to be particularly careful about checking internal state. Any exception, other than AssertionError or SkipTest, raised by this method will be considered an additional error rather than a test failure (thus increasing the total number of reported errors). This method will only be called if the asyncSetUp() succeeds, regardless of the outcome of the test method. The default implementation does nothing.

- addAsyncCleanup(function, /, *args, **kwargs)
  - This method accepts a coroutine that can be used as a cleanup function.

- run(result=None)
  - Sets up a new event loop to run the test, collecting the result into the TestResult object passed as result. If result is omitted or None, a temporary result object is created (by calling the defaultTestResult() method) and used. The result object is returned to run()'s caller. At the end of the test all the tasks in the event loop are cancelled.

An example illustrating the order:
```python
from unittest import IsolatedAsyncioTestCase

events = []

class Test(IsolatedAsyncioTestCase):


    def setUp(self):
        events.append("setUp")

    async def asyncSetUp(self):
        self._async_connection = await AsyncConnection()
        events.append("asyncSetUp")

    async def test_response(self):
        events.append("test_response")
        response = await self._async_connection.get("https://example.com")
        self.assertEqual(response.status_code, 200)
        self.addAsyncCleanup(self.on_cleanup)

    def tearDown(self):
        events.append("tearDown")

    async def asyncTearDown(self):
        await self._async_connection.close()
        events.append("asyncTearDown")

    async def on_cleanup(self):
        events.append("cleanup")

if __name__ == "__main__":
    unittest.main()
```

After running the test, events would contain ["setUp", "asyncSetUp", "test_response", "asyncTearDown", "tearDown", "cleanup"].

##### class unittest.FunctionTestCase(testFunc, setUp=None, tearDown=None, description=None)

This class implements the portion of the TestCase interface which allows the test runner to drive the test, but does not provide the methods which test code can use to check and report errors. This is used to create test cases using legacy test code, allowing it to be integrated into a unittest-based test framework.

##### Deprecated aliases

For historical reasons, some of the TestCase methods had one or more aliases that are now deprecated. The following table lists the correct names along with their deprecated aliases:

<div class="responsive-table__container"><table class="docutils align-default">
<colgroup>
<col style="width: 40%">
<col style="width: 29%">
<col style="width: 31%">
</colgroup>
<thead>
<tr class="row-odd"><th class="head"><p>方法名</p></th>
<th class="head"><p>Deprecated alias</p></th>
<th class="head"><p>Deprecated alias</p></th>
</tr>
</thead>
<tbody>
<tr class="row-even"><td><p><a class="reference internal" href="#unittest.TestCase.assertEqual" title="unittest.TestCase.assertEqual"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertEqual()</span></code></a></p></td>
<td><p>failUnlessEqual</p></td>
<td><p>assertEquals</p></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="#unittest.TestCase.assertNotEqual" title="unittest.TestCase.assertNotEqual"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertNotEqual()</span></code></a></p></td>
<td><p>failIfEqual</p></td>
<td><p>assertNotEquals</p></td>
</tr>
<tr class="row-even"><td><p><a class="reference internal" href="#unittest.TestCase.assertTrue" title="unittest.TestCase.assertTrue"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertTrue()</span></code></a></p></td>
<td><p>failUnless</p></td>
<td><p>assert_</p></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="#unittest.TestCase.assertFalse" title="unittest.TestCase.assertFalse"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertFalse()</span></code></a></p></td>
<td><p>failIf</p></td>
<td></td>
</tr>
<tr class="row-even"><td><p><a class="reference internal" href="#unittest.TestCase.assertRaises" title="unittest.TestCase.assertRaises"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertRaises()</span></code></a></p></td>
<td><p>failUnlessRaises</p></td>
<td></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="#unittest.TestCase.assertAlmostEqual" title="unittest.TestCase.assertAlmostEqual"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertAlmostEqual()</span></code></a></p></td>
<td><p>failUnlessAlmostEqual</p></td>
<td><p>assertAlmostEquals</p></td>
</tr>
<tr class="row-even"><td><p><a class="reference internal" href="#unittest.TestCase.assertNotAlmostEqual" title="unittest.TestCase.assertNotAlmostEqual"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertNotAlmostEqual()</span></code></a></p></td>
<td><p>failIfAlmostEqual</p></td>
<td><p>assertNotAlmostEquals</p></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="#unittest.TestCase.assertRegex" title="unittest.TestCase.assertRegex"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertRegex()</span></code></a></p></td>
<td></td>
<td><p>assertRegexpMatches</p></td>
</tr>
<tr class="row-even"><td><p><a class="reference internal" href="#unittest.TestCase.assertNotRegex" title="unittest.TestCase.assertNotRegex"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertNotRegex()</span></code></a></p></td>
<td></td>
<td><p>assertNotRegexpMatches</p></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="#unittest.TestCase.assertRaisesRegex" title="unittest.TestCase.assertRaisesRegex"><code class="xref py py-meth docutils literal notranslate"><span class="pre">assertRaisesRegex()</span></code></a></p></td>
<td></td>
<td><p>assertRaisesRegexp</p></td>
</tr>
</tbody>
</table></div>

- 3.1 版后已移除: The fail* aliases listed in the second column have been deprecated.
- 3.2 版后已移除: The assert* aliases listed in the third column have been deprecated.
- 3.2 版后已移除: assertRegexpMatches and assertRaisesRegexp have been renamed to assertRegex() and assertRaisesRegex().
- 3.5 版后已移除: The assertNotRegexpMatches name is deprecated in favor of assertNotRegex().

#### Group Tests

##### class unittest.TestSuite(tests=())

This class represents an aggregation of individual test cases and test suites. The class presents the interface needed by the test runner to allow it to be run as any other test case. Running a TestSuite instance is the same as iterating over the suite, running each test individually.

If tests is given, it must be an iterable of individual test cases or other test suites that will be used to build the suite initially. Additional methods are provided to add test cases and suites to the collection later on.

TestSuite objects behave much like TestCase objects, except they do not actually implement a test. Instead, they are used to aggregate tests into groups of tests that should be run together. Some additional methods are available to add tests to TestSuite instances:

- addTest(test)
  - Add a TestCase or TestSuite to the suite.

- addTests(tests)
  - Add all the tests from an iterable of TestCase and TestSuite instances to this test suite.
  - This is equivalent to iterating over tests, calling addTest() for each element.

TestSuite shares the following methods with TestCase:

- run(result)
  - Run the tests associated with this suite, collecting the result into the test result object passed as result. Note that unlike TestCase.run(), TestSuite.run() requires the result object to be passed in.

- debug()
  - Run the tests associated with this suite without collecting the result. This allows exceptions raised by the test to be propagated to the caller and can be used to support running tests under a debugger.

- countTestCases()
  - Return the number of tests represented by this test object, including all individual tests and sub-suites.

- \_\_iter__()
  - Tests grouped by a TestSuite are always accessed by iteration. Subclasses can lazily provide tests by overriding __iter__(). Note that this method may be called several times on a single suite (for example when counting tests or comparing for equality) so the tests returned by repeated iterations before TestSuite.run() must be the same for each call iteration. After TestSuite.run(), callers should not rely on the tests returned by this method unless the caller uses a subclass that overrides TestSuite._removeTestAtIndex() to preserve test references.
  - 在 3.2 版更改: In earlier versions the TestSuite accessed tests directly rather than through iteration, so overriding __iter__() wasn't sufficient for providing tests.
  - 在 3.4 版更改: In earlier versions the TestSuite held references to each TestCase after TestSuite.run(). Subclasses can restore that behavior by overriding TestSuite._removeTestAtIndex().

In the typical usage of a TestSuite object, the run() method is invoked by a TestRunner rather than by the end-user test harness.

#### Loading and running tests

##### class unittest.TestLoader

The TestLoader class is used to create test suites from classes and modules. Normally, there is no need to create an instance of this class; the unittest module provides an instance that can be shared as unittest.defaultTestLoader. Using a subclass or instance, however, allows customization of some configurable properties.

TestLoader objects have the following attributes:

- errors3.5 新版功能
  - A list of the non-fatal errors encountered while loading tests. Not reset by the loader at any point. Fatal errors are signalled by the relevant a method raising an exception to the caller. Non-fatal errors are also indicated by a synthetic test that will raise the original error when run.

TestLoader objects have the following methods:

- loadTestsFromTestCase(testCaseClass)
  - Return a suite of all test cases contained in the TestCase-derived testCaseClass.
  - A test case instance is created for each method named by getTestCaseNames(). By default these are the method names beginning with test. If getTestCaseNames() returns no methods, but the runTest() method is implemented, a single test case is created for that method instead.

- loadTestsFromModule(module, pattern=None)
  - Return a suite of all test cases contained in the given module. This method searches module for classes derived from TestCase and creates an instance of the class for each test method defined for the class.
  - 注解: While using a hierarchy of TestCase-derived classes can be convenient in sharing fixtures and helper functions, defining test methods on base classes that are not intended to be instantiated directly does not play well with this method. Doing so, however, can be useful when the fixtures are different and defined in subclasses.
  - If a module provides a load_tests function it will be called to load the tests. This allows modules to customize test loading. This is the load_tests protocol. The pattern argument is passed as the third argument to load_tests.
  - 在 3.2 版更改: Support for load_tests added.
  - 在 3.5 版更改: The undocumented and unofficial use_load_tests default argument is deprecated and ignored, although it is still accepted for backward compatibility. The method also now accepts a keyword-only argument pattern which is passed to load_tests as the third argument.

- loadTestsFromName(name, module=None)
  - Return a suite of all test cases given a string specifier.
  - The specifier name is a "dotted name" that may resolve either to a module, a test case class, a test method within a test case class, a TestSuite instance, or a callable object which returns a TestCase or TestSuite instance. These checks are applied in the order listed here; that is, a method on a possible test case class will be picked up as "a test method within a test case class", rather than "a callable object".
  - For example, if you have a module SampleTests containing a TestCase-derived class SampleTestCase with three test methods (test_one(), test_two(), and test_three()), the specifier 'SampleTests.SampleTestCase' would cause this method to return a suite which will run all three test methods. Using the specifier 'SampleTests.SampleTestCase.test_two' would cause it to return a test suite which will run only the test_two() test method. The specifier can refer to modules and packages which have not been imported; they will be imported as a side-effect.
  - The method optionally resolves name relative to the given module.
  - 在 3.5 版更改: If an ImportError or AttributeError occurs while traversing name then a synthetic test that raises that error when run will be returned. These errors are included in the errors accumulated by self.errors.

- loadTestsFromNames(names, module=None)
  - Similar to loadTestsFromName(), but takes a sequence of names rather than a single name. The return value is a test suite which supports all the tests defined for each name.

- getTestCaseNames(testCaseClass)
  - Return a sorted sequence of method names found within testCaseClass; this should be a subclass of TestCase.

- discover(start_dir, pattern='test*.py', top_level_dir=None)3.2 新版功能
  - Find all the test modules by recursing into subdirectories from the specified start directory, and return a TestSuite object containing them. Only test files that match pattern will be loaded. (Using shell style pattern matching.) Only module names that are importable (i.e. are valid Python identifiers) will be loaded.
  - All test modules must be importable from the top level of the project. If the start directory is not the top level directory then the top level directory must be specified separately.
  - If importing a module fails, for example due to a syntax error, then this will be recorded as a single error and discovery will continue. If the import failure is due to SkipTest being raised, it will be recorded as a skip instead of an error.
  - If a package (a directory containing a file named __init__.py) is found, the package will be checked for a load_tests function. If this exists then it will be called package.load_tests(loader, tests, pattern). Test discovery takes care to ensure that a package is only checked for tests once during an invocation, even if the load_tests function itself calls loader.discover.
  - If load_tests exists then discovery does not recurse into the package, load_tests is responsible for loading all tests in the package.
  - The pattern is deliberately not stored as a loader attribute so that packages can continue discovery themselves. top_level_dir is stored so load_tests does not need to pass this argument in to loader.discover().
  - start_dir can be a dotted module name as well as a directory.
  - 在 3.4 版更改: Modules that raise SkipTest on import are recorded as skips, not errors.
  - 在 3.4 版更改: start_dir can be a namespace packages.
  - 在 3.4 版更改: Paths are sorted before being imported so that execution order is the same even if the underlying file system's ordering is not dependent on file name.
  - 在 3.5 版更改: Found packages are now checked for load_tests regardless of whether their path matches pattern, because it is impossible for a package name to match the default pattern.

The following attributes of a TestLoader can be configured either by subclassing or assignment on an instance:

- testMethodPrefix
  - String giving the prefix of method names which will be interpreted as test methods. The default value is 'test'.
  - This affects getTestCaseNames() and all the loadTestsFrom*() methods.

- sortTestMethodsUsing
  - Function to be used to compare method names when sorting them in getTestCaseNames() and all the loadTestsFrom*() methods.

- suiteClass
  - Callable object that constructs a test suite from a list of tests. No methods on the resulting object are needed. The default value is the TestSuite class.
  - This affects all the loadTestsFrom*() methods.

- testNamePatterns3.7 新版功能
  - List of Unix shell-style wildcard test name patterns that test methods have to match to be included in test suites (see -v option).
  - If this attribute is not None (the default), all test methods to be included in test suites must match one of the patterns in this list. Note that matches are always performed using fnmatch.fnmatchcase(), so unlike patterns passed to the -v option, simple substring patterns will have to be converted using * wildcards.
  - This affects all the loadTestsFrom*() methods.

##### class unittest.TestResult

This class is used to compile information about which tests have succeeded and which have failed.

A TestResult object stores the results of a set of tests. The TestCase and TestSuite classes ensure that results are properly recorded; test authors do not need to worry about recording the outcome of tests.

Testing frameworks built on top of unittest may want access to the TestResult object generated by running a set of tests for reporting purposes; a TestResult instance is returned by the TestRunner.run() method for this purpose.

TestResult instances have the following attributes that will be of interest when inspecting the results of running a set of tests:

- errors
  - A list containing 2-tuples of TestCase instances and strings holding formatted tracebacks. Each tuple represents a test which raised an unexpected exception.

- failures
  - A list containing 2-tuples of TestCase instances and strings holding formatted tracebacks. Each tuple represents a test where a failure was explicitly signalled using the TestCase.assert*() methods.

- skipped3.1 新版功能
  - A list containing 2-tuples of TestCase instances and strings holding the reason for skipping the test.

- expectedFailures
  - A list containing 2-tuples of TestCase instances and strings holding formatted tracebacks. Each tuple represents an expected failure or error of the test case.

- unexpectedSuccesses
  - A list containing TestCase instances that were marked as expected failures, but succeeded.

- shouldStop
  - Set to True when the execution of tests should stop by stop().

- testsRun
  - The total number of tests run so far.

- buffer3.2 新版功能
  - If set to true, sys.stdout and sys.stderr will be buffered in between startTest() and stopTest() being called. Collected output will only be echoed onto the real sys.stdout and sys.stderr if the test fails or errors. Any output is also attached to the failure / error message.

- failfast3.2 新版功能
  - If set to true stop() will be called on the first failure or error, halting the test run.

- tb_locals3.5 新版功能
  - If set to true then local variables will be shown in tracebacks.

- wasSuccessful()
  - Return True if all tests run so far have passed, otherwise returns False.
  - 在 3.4 版更改: Returns False if there were any unexpectedSuccesses from tests marked with the expectedFailure() decorator.

- stop()
  - This method can be called to signal that the set of tests being run should be aborted by setting the shouldStop attribute to True. TestRunner objects should respect this flag and return without running any additional tests.
  - For example, this feature is used by the TextTestRunner class to stop the test framework when the user signals an interrupt from the keyboard. Interactive tools which provide TestRunner implementations can use this in a similar manner.

The following methods of the TestResult class are used to maintain the internal data structures, and may be extended in subclasses to support additional reporting requirements. This is particularly useful in building tools which support interactive reporting while tests are being run.

- startTest(test)
  - Called when the test case test is about to be run.

- stopTest(test)
  - Called after the test case test has been executed, regardless of the outcome.

- startTestRun()3.1 新版功能
  - Called once before any tests are executed.

- stopTestRun()3.1 新版功能
  - Called once after all tests are executed.

- addError(test, err)
  - Called when the test case test raises an unexpected exception. err is a tuple of the form returned by sys.exc_info(): (type, value, traceback).
  - The default implementation appends a tuple (test, formatted_err) to the instance's errors attribute, where formatted_err is a formatted traceback derived from err.

- addFailure(test, err)
  - Called when the test case test signals a failure. err is a tuple of the form returned by sys.exc_info(): (type, value, traceback).
  - The default implementation appends a tuple (test, formatted_err) to the instance's failures attribute, where formatted_err is a formatted traceback derived from err.

- addSuccess(test)
  - Called when the test case test succeeds.
  - The default implementation does nothing.

- addSkip(test, reason)
  - Called when the test case test is skipped. reason is the reason the test gave for skipping.
  - The default implementation appends a tuple (test, reason) to the instance's skipped attribute.

- addExpectedFailure(test, err)
  - Called when the test case test fails or errors, but was marked with the expectedFailure() decorator.
  - The default implementation appends a tuple (test, formatted_err) to the instance's expectedFailures attribute, where formatted_err is a formatted traceback derived from err.

- addUnexpectedSuccess(test)
  - Called when the test case test was marked with the expectedFailure() decorator, but succeeded.
  - The default implementation appends the test to the instance's unexpectedSuccesses attribute.

- addSubTest(test, subtest, outcome)3.4 新版功能
  - Called when a subtest finishes. test is the test case corresponding to the test method. subtest is a custom TestCase instance describing the subtest.
  - If outcome is None, the subtest succeeded. Otherwise, it failed with an exception where outcome is a tuple of the form returned by sys.exc_info(): (type, value, traceback).
  - The default implementation does nothing when the outcome is a success, and records subtest failures as normal failures.

##### class unittest.TextTestResult(stream, descriptions, verbosity)

A concrete implementation of TestResult used by the TextTestRunner.

3.2 新版功能: This class was previously named _TextTestResult. The old name still exists as an alias but is deprecated.

##### unittest.defaultTestLoader

Instance of the TestLoader class intended to be shared. If no customization of the TestLoader is needed, this instance can be used instead of repeatedly creating new instances.

##### class unittest.TextTestRunner(stream=None, descriptions=True, verbosity=1, failfast=False, buffer=False, resultclass=None, warnings=None, *, tb_locals=False)

A basic test runner implementation that outputs results to a stream. If stream is None, the default, sys.stderr is used as the output stream. This class has a few configurable parameters, but is essentially very simple. Graphical applications which run test suites should provide alternate implementations. Such implementations should accept **kwargs as the interface to construct runners changes when features are added to unittest.

By default this runner shows DeprecationWarning, PendingDeprecationWarning, ResourceWarning and ImportWarning even if they are ignored by default. Deprecation warnings caused by deprecated unittest methods are also special-cased and, when the warning filters are 'default' or 'always', they will appear only once per-module, in order to avoid too many warning messages. This behavior can be overridden using Python's -Wd or -Wa options (see Warning control) and leaving warnings to None.

在 3.2 版更改: Added the warnings argument.

在 3.2 版更改: The default stream is set to sys.stderr at instantiation time rather than import time.

在 3.5 版更改: Added the tb_locals parameter.

- _makeResult()
  - This method returns the instance of TestResult used by run(). It is not intended to be called directly, but can be overridden in subclasses to provide a custom TestResult.
  - _makeResult() instantiates the class or callable passed in the TextTestRunner constructor as the resultclass argument. It defaults to TextTestResult if no resultclass is provided. The result class is instantiated with the following arguments:
    ```
    stream, descriptions, verbosity
    ```

- run(test)
  - This method is the main public interface to the TextTestRunner. This method takes a TestSuite or TestCase instance. A TestResult is created by calling _makeResult() and the test(s) are run and the results printed to stdout.

##### unittest.main(module='__main__', defaultTest=None, argv=None, testRunner=None, testLoader=unittest.defaultTestLoader, exit=True, verbosity=1, failfast=None, catchbreak=None, buffer=None, warnings=None)

A command-line program that loads a set of tests from module and runs them; this is primarily for making test modules conveniently executable. The simplest use for this function is to include the following line at the end of a test script:

```python
if __name__ == '__main__':
    unittest.main()
```

You can run tests with more detailed information by passing in the verbosity argument:

```python
if __name__ == '__main__':
    unittest.main(verbosity=2)
```

The defaultTest argument is either the name of a single test or an iterable of test names to run if no test names are specified via argv. If not specified or None and no test names are provided via argv, all tests found in module are run.

The argv argument can be a list of options passed to the program, with the first element being the program name. If not specified or None, the values of sys.argv are used.

The testRunner argument can either be a test runner class or an already created instance of it. By default main calls sys.exit() with an exit code indicating success or failure of the tests run.

The testLoader argument has to be a TestLoader instance, and defaults to defaultTestLoader.

main supports being used from the interactive interpreter by passing in the argument exit=False. This displays the result on standard output without calling sys.exit():

```>>>
>>> from unittest import main
>>> main(module='test_module', exit=False)
```

The failfast, catchbreak and buffer parameters have the same effect as the same-name command-line options.

The warnings argument specifies the warning filter that should be used while running the tests. If it's not specified, it will remain None if a -W option is passed to python (see Warning control), otherwise it will be set to 'default'.

Calling main actually returns an instance of the TestProgram class. This stores the result of the tests run as the result attribute.

在 3.1 版更改: The exit parameter was added.

在 3.2 版更改: The verbosity, failfast, catchbreak, buffer and warnings parameters were added.

在 3.4 版更改: The defaultTest parameter was changed to also accept an iterable of test names.

##### load_tests Protocol3.2 新版功能

Modules or packages can customize how tests are loaded from them during normal test runs or test discovery by implementing a function called load_tests.

If a test module defines load_tests it will be called by TestLoader.loadTestsFromModule() with the following arguments:

```
load_tests(loader, standard_tests, pattern)
```

where pattern is passed straight through from loadTestsFromModule. It defaults to None.

It should return a TestSuite.

loader is the instance of TestLoader doing the loading. standard_tests are the tests that would be loaded by default from the module. It is common for test modules to only want to add or remove tests from the standard set of tests. The third argument is used when loading packages as part of test discovery.

A typical load_tests function that loads tests from a specific set of TestCase classes may look like:

```python
test_cases = (TestCase1, TestCase2, TestCase3)

def load_tests(loader, tests, pattern):
    suite = TestSuite()
    for test_class in test_cases:
        tests = loader.loadTestsFromTestCase(test_class)
        suite.addTests(tests)
    return suite
```

If discovery is started in a directory containing a package, either from the command line or by calling TestLoader.discover(), then the package __init__.py will be checked for load_tests. If that function does not exist, discovery will recurse into the package as though it were just another directory. Otherwise, discovery of the package's tests will be left up to load_tests which is called with the following arguments:

```
load_tests(loader, standard_tests, pattern)
```

This should return a TestSuite representing all the tests from the package. (standard_tests will only contain tests collected from __init__.py.)

Because the pattern is passed into load_tests the package is free to continue (and potentially modify) test discovery. A 'do nothing' load_tests function for a test package would look like:

```python
def load_tests(loader, standard_tests, pattern):
    # top level directory cached on loader instance
    this_dir = os.path.dirname(__file__)
    package_tests = loader.discover(start_dir=this_dir, pattern=pattern)
    standard_tests.addTests(package_tests)
    return standard_tests
```

在 3.5 版更改: Discovery no longer checks package names for matching pattern due to the impossibility of package names matching the default pattern.

### Class and Module Fixtures

Class and module level fixtures are implemented in TestSuite. When the test suite encounters a test from a new class then tearDownClass() from the previous class (if there is one) is called, followed by setUpClass() from the new class.

Similarly if a test is from a different module from the previous test then tearDownModule from the previous module is run, followed by setUpModule from the new module.

After all the tests have run the final tearDownClass and tearDownModule are run.

Note that shared fixtures do not play well with [potential] features like test parallelization and they break test isolation. They should be used with care.

The default ordering of tests created by the unittest test loaders is to group all tests from the same modules and classes together. This will lead to setUpClass / setUpModule (etc) being called exactly once per class and module. If you randomize the order, so that tests from different modules and classes are adjacent to each other, then these shared fixture functions may be called multiple times in a single test run.

Shared fixtures are not intended to work with suites with non-standard ordering. A BaseTestSuite still exists for frameworks that don't want to support shared fixtures.

If there are any exceptions raised during one of the shared fixture functions the test is reported as an error. Because there is no corresponding test instance an _ErrorHolder object (that has the same interface as a TestCase) is created to represent the error. If you are just using the standard unittest test runner then this detail doesn't matter, but if you are a framework author it may be relevant.

#### setUpClass and tearDownClass

These must be implemented as class methods:

``` python
import unittest

class Test(unittest.TestCase):
    @classmethod
    def setUpClass(cls):
        cls._connection = createExpensiveConnectionObject()

    @classmethod
    def tearDownClass(cls):
        cls._connection.destroy()
```

If you want the setUpClass and tearDownClass on base classes called then you must call up to them yourself. The implementations in TestCase are empty.

If an exception is raised during a setUpClass then the tests in the class are not run and the tearDownClass is not run. Skipped classes will not have setUpClass or tearDownClass run. If the exception is a SkipTest exception then the class will be reported as having been skipped instead of as an error.

#### setUpModule and tearDownModule

These should be implemented as functions:

```python
def setUpModule():
    createConnection()

def tearDownModule():
    closeConnection()
```

If an exception is raised in a setUpModule then none of the tests in the module will be run and the tearDownModule will not be run. If the exception is a SkipTest exception then the module will be reported as having been skipped instead of as an error.

To add cleanup code that must be run even in the case of an exception, use addModuleCleanup:

##### unittest.addModuleCleanup(function, /, *args, **kwargs)3.8 新版功能

Add a function to be called after tearDownModule() to cleanup resources used during the test class. Functions will be called in reverse order to the order they are added (LIFO). They are called with any arguments and keyword arguments passed into addModuleCleanup() when they are added.

If setUpModule() fails, meaning that tearDownModule() is not called, then any cleanup functions added will still be called.

##### unittest.doModuleCleanups()3.8 新版功能

This function is called unconditionally after tearDownModule(), or after setUpModule() if setUpModule() raises an exception.

It is responsible for calling all the cleanup functions added by addCleanupModule(). If you need cleanup functions to be called prior to tearDownModule() then you can call doModuleCleanups() yourself.

doModuleCleanups() pops methods off the stack of cleanup functions one at a time, so it can be called at any time.

### 信号处理3.2 新版功能

The -c/--catch command-line option to unittest, along with the catchbreak parameter to unittest.main(), provide more friendly handling of control-C during a test run. With catch break behavior enabled control-C will allow the currently running test to complete, and the test run will then end and report all the results so far. A second control-c will raise a KeyboardInterrupt in the usual way.

The control-c handling signal handler attempts to remain compatible with code or tests that install their own signal.SIGINT handler. If the unittest handler is called but isn't the installed signal.SIGINT handler, i.e. it has been replaced by the system under test and delegated to, then it calls the default handler. This will normally be the expected behavior by code that replaces an installed handler and delegates to it. For individual tests that need unittest control-c handling disabled the removeHandler() decorator can be used.

There are a few utility functions for framework authors to enable control-c handling functionality within test frameworks.

- unittest.installHandler()
  - Install the control-c handler. When a signal.SIGINT is received (usually in response to the user pressing control-c) all registered results have stop() called.

- unittest.registerResult(result)
  - Register a TestResult object for control-c handling. Registering a result stores a weak reference to it, so it doesn't prevent the result from being garbage collected.
  - Registering a TestResult object has no side-effects if control-c handling is not enabled, so test frameworks can unconditionally register all results they create independently of whether or not handling is enabled.

- unittest.removeResult(result)
  - Remove a registered result. Once a result has been removed then stop() will no longer be called on that result object in response to a control-c.

- unittest.removeHandler(function=None)
  - When called without arguments this function removes the control-c handler if it has been installed. This function can also be used as a test decorator to temporarily remove the handler while the test is being executed:

    ```python
    @unittest.removeHandler
    def test_signal_handling(self):
        ...
    ```

## Conclusion

1）Java的白盒测试、API自动化、UI自动化：
- Junit（对Java的方法进行测试）；
- TestNG（相比Junit，参数化测试、依赖测试以及套件测试更好用，建议优先选这个）；

2）python的白盒测试、API自动化、UI自动化：
- pytest（功能更多，更简洁高效，优于unittest ）。

3）API自动化、UI自动化：
- Cucumber（面向“注释”编程）。

4）移动端的UI自动化测试：
- Appium（基于 Nodejs，多平台多语言，OSX、Win和Linux上也能用 ）
- ATX（只支持iOS、Android）

5）UI自动化、爬虫：
- Selenium（兼容性好，Web自动化必选）。