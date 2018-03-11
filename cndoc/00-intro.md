# 介绍

Composer是PHP的依赖管理工具。它允许您声明您的项目所依赖的库, 并且它将为您管理 (安装/更新) 它们.

## 依赖管理

Composer is **不是** 一个与Yum或 Apt 同样意义上的包管理器. 是的, 它处理 "包" 或库, 但它以每个项目为基础管理它们, 并将它们安装在项目内的目录 (如 `vendor`) 中. 默认情况下, 它不会在全局范围内安装任何内容。因此, 它是一个依赖关系管理器. 不过, 它为了方便通过[global](03-cli.md#global)命令确实支持"全局"项目.

这个想法并不新鲜,Composer强烈受node的[npm](https://www.npmjs.com/)和ruby的[bundler](https://bundler.io/)的启发.

假设:

1. 您有一个项目, 依赖于多个库.
1. 其中一些库依赖于其他库.

Composer:

1. 允许您声明所依赖的库.
1. 找出哪些软件包的哪个版本能及需要被安装, 并安装它们 (意味着将它们下载到您的项目中).

有关声明依赖项的更多详细信息, 请参见[基本用法](01-basic-usage.md)章节.

## 系统要求

Composer需要 PHP 5.3.2 以上才能运行。还需要一些敏感的 php 设置和编译标记, 但是当使用安装程序时, 您会收到有关任何不兼容的警告.

要从源码安装软件包而不是简单的 zip 存档, 您将需要git, svn, fossil或hg,这取决于软件包是如何受版本控制的.

Composer是多平台的, 我们努力使其运行在 Windows,Linux 和 OSX 上一样好.

## 在Linux / Unix / OSX上安装

### 下载 Composer 可执行文件

Composer提供了一个方便的安装程序,你可以直接从命令行执行。如果您希望了解有关安装程序的内部工作原理, 请随时[下载此文件](https://getcomposer.org/installer)或在[GitHub](https://github.com/composer/getcomposer.org/blob/master/web/installer)上进行审阅。源代码是纯 PHP。

有两种简单的方法来安装Composer。局部作为你的项目的一部分, 或在全局范围内作为全系统的可执行文件.

#### 局部安装


在局部安装Composer的问题只是在你的项目目录中运行安装程序。有关说明, 请参阅[下载页](http://composer.p2hp.com/download/).

安装程序将只检查几个PHP设置, 然后下载`composer.phar`到您的工作目录。这个文件是Composer的二进制文件。它是一个 PHAR (php存档), 一个 php 的存档格式, 除其他事项外,可以在命令行上运行.

现在运行 `php composer.phar` 执行 Composer.

您可以使用`--install-dir`选项将Composer安装到特定的目录, 另外(重新)将其命名使用`--filename`选项。当按照[下载页面说明](http://composer.p2hp.com/download/)运行安装程序时, 添加以下参数:

```sh
php composer-setup.php --install-dir=bin --filename=composer
```

现在运行 `php bin/composer` 执行 Composer.

#### 全局安装

你可以把Composer PHAR放到任何你想要的地方。如果你将它放在作为`PATH`一部分的目录中, 则可以全局访问它。在 unixy 系统上, 您甚至使它可执行并调用它, 而无需直接使用 `php`解释器。

当按照[下载页面说明](http://composer.p2hp.com/download/)运行安装程序之后, 您可以运行此操作来移动composer.phar 到你path中的目录:

```sh
mv composer.phar /usr/local/bin/composer
```

> **注意:**  如果上述操作由于权限而失败, 您可能需要使用sudo再执行一遍.

> **注意:** 在某些版本的 OSX 上, 默认情况下`/usr`目录不存在。如果您收到错误 "/usr/local/bin/composer: No such file or directory", 那么您必须手动创建目录, 然后再继续执行: `mkdir -p /usr/local/bin`.

> **注意:** 有关更改PATH路径的信息, 请阅读[百度百科文章](https://baike.baidu.com/item/%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F/1730949)和/或使用 百度/Google.

现在运行 `composer` 执行 Composer 而不是 `php composer.phar`.

## 在Windows上安装

### 使用安装程序

这是在你的机器上安装Composer最简单的方法.

下载并运行[Composer-Setup.exe](https://getcomposer.org/Composer-Setup.exe)。它将安装最新的Composer版本并设置您的PATH路径, 这样你可以从任何目录中在您的命令行调用`composer`.

> **注意:** 关闭你的当前终端。用新的终端测试使用: 这很重要, 因为只有当终端重新启动时才会加载PATH路径.

### 手动安装


更改目录到`PATH`路径中的目录, 并按照[下载页面说明](http://composer.p2hp.com/download/)运行安装程序以下载`composer.phar`.

在`composer.phar`旁边创建一个新的`composer.bat`文件 :

```sh
C:\bin>echo @php "%~dp0composer.phar" %*>composer.bat
```

将目录添加到 PATH 环境变量 (如果尚未)。有关更改 PATH 变量的信息, 请参阅[本文](https://jingyan.baidu.com/article/47a29f24610740c0142399ea.html)和/或使用 百度/Google.

关闭你的当前终端. 开一个新终端测试用法:

```sh
C:\Users\username>composer -V
Composer version 1.0.0 2016-01-10 20:34:53
```

## 使用 Composer

现在你已经安装了Composer, 你已经准备好使用它了! 请前往下一章查看简短和简单的演示。


[基本用法](01-basic-usage.md) &rarr;
