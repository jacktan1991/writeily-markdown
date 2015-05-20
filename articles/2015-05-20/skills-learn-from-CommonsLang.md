# 从commons-lang 学到的一些示范

## 使用Maven
尽管Gradle是更为适合Java项目的构建工具，但是在相当长的一段的时间内，Maven作为主流的构建工具，并被其他在线平台如在线CI、PASS部署使用实现相应的自动化动作。
此外Maven的很多插件可以实现自动化保障，比如用`CheckStyle`保证Java编码规范而不是靠程序员的自我约束，同时也可以做到自动化替代手工。

## Java编码规范
在两次的pull request过程中，没能一次提交成功，其中是一些很细微的本可规避错误导致了构建失败，如源码和commons-lang自己定制的Java编码规范冲突。这个过程也让我很直观地看到了一些这个优秀的开源项目所定制的规范，这里只列举我观察到的。

+ 4个空格_（基本上已是Java缩进的事实语法了）_
+ 命名规范
  - 严格的驼峰命名，针对变量，即便是临时变量
+ 空行不用缩进
+ 方法声明时，只保留`()`后面空格

    ```java
    public void method() {
        //TODO:some codes
    }
    ```
+ 语句块`()`前后空格都保留

    ```java
    if (condition) {
        //TODO:some codes
    }
    ```

## CheckStyle保证编码规范

### 流程解析

以前编码规范之类的一般以文档的形式给出，然后贴在墙上供团队成员编码时参考。这种方式的确定包括但不限于以下四种：

1. 是否遵守就全靠程序员职业道德的规范，**有漏洞**；
2. 即便是强制执行也是依靠自己或他人的手工操作，**费时费力**；
3. 而且贴在墙上的编码规范仿佛是程序员在coding时盘旋在头上的幽灵，时不时蹦出来在那絮絮叨叨，很**容易给程序员造成心理阴影和暴躁的情绪**；
4. 再高级一点就是依赖IDE了，团队都使用相同配置的IDE，从一定程度上减轻了手工量。但长此以往很容易导致**`依赖IDE慢性病`**，贻害深远；


### `checkstyle.xml`

CheckStyle插件使用名为`checkstyle.xml`配置文件来保证编码规范，checkstyle.xml文件同时也是编码规范文档。
commons-lang的[CheckStyle配置](https://raw.githubusercontent.com/apache/commons-lang/master/checkstyle.xml "checkstyle.xml")。

## Travis CI轻松持续构建
### 流程解析
一个像commons-lang这样的流行项目几乎每天都要接受pull request，每一个pull request在merge之前又会有多次commit动作。而github本身只是一个代码托管工具，一个用户在pull request前是否在本地通过maven构建不得而知，就需要管理员手动监管了。

### 自动化构建
Travis CI本身是一个云构建平台，提供包括Java、C、Android在内的25种项目类型构建所需的软、硬件环境，对于Java项目支持Maven、Gradle和Ant三种构建方式。它本身不需要你上传源码，而是通过授权可以读取github的项目，对每一次commit动作进行持续构建。

### `.travis.yml`
Travis使用名为`.travis.yml`配置文件，具体使用参考相应教程。commons-lang的[Travis配置](https://raw.githubusercontent.com/apache/commons-lang/master/.travis.yml ".travis.yml")。

### 构建勋章
Travis在每次对项目实施构建后，除了有详细的结果报告，还会有一个类似勋章之类的可爱图标。

比如commons-lang如果构建成功后，会颁发一个"build passing"勋章[![Build Status](https://travis-ci.org/apache/commons-lang.svg?branch=master)](https://travis-ci.org/apache/commons-lang)

## Coveralls反映测试覆盖率

## JIRA规范Issue管理
### 流程解析
一般而言，一个标配的github项目，默认开通了pull requsts和issues面板，而commons-lang关闭了issues面板。

后来发现该项目使用了一种名为Atlassian JIRA管理平台，将issues管理迁移至该平台。因为引入了额外的实体，这在一定程度上增加了开发的复杂度，这并不是一个可以**`拿来主义的示范`**，起码至今没有发现看出使用JIRA的奥妙。

### 集成到Maven
在pom.xml中添加一个一级子标签`issueManagement`，并填上项目在JIRA上的地址，如commons-lang的`issueManagement`标签内容如下：

```
<issueManagement>
  <system>jira</system>
  <url>http://issues.apache.org/jira/browse/LANG</url>
</issueManagement>
```

## 还是需要一个IDE

### 你用或者不用，IDE就在那里
相信大多数刚入门java的人，甚至在你还没能看到控制台打印出"Hello World"前，会被传授一种叫Eclipse的软件，然后一通的安装、配置、重启，打开Eclipse、变卡、关闭Eclipse、Eclipse无响应、卸载、重装、优化......

在你经过一番虐心的折腾，不敢卸载不敢重装系统不敢换电脑战战兢兢地享受这来之不易的果实的时候，你还得提防身后突然冒出的一句“竟然还用Eclispe？！快扔了还IDEA吧”，那种不屑语气让你内心几乎是崩溃的。

接着一转身，会被各种过来人、网友苦口婆心地劝说不要使用IDE，巴拉巴拉所一通回归记事本啦、Sublime才是程序员的标配、VIM是编辑器之王，你心里暗骂一句“卧槽，VIM是个什么鬼”。

曾几何时，你陷入编辑器之争的迷茫，**不用IDE+使用记事本/VIM编程**被看成是神秘大牛的标签，而你也开始了不用IDE的装逼之路。

### 如非必要，勿增实体

项目的配置仅仅是项目的元数据，协同开发中还需要一个IDE。元数据是团队协同开发的基石，但做为个人，你还需要一个趁手的IDE，让你开发时感到无痛。

## `Git Rebase`保证没有瞎提交

## 多线程单元测试
