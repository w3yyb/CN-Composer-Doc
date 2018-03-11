# 库(资源包)

本章将告诉你如何让你的库通过作曲家可安装.

## 每一个项目都是一个包


一旦你有了`composer.json`在目录中, 该目录就是一个包。向项目添加[`require`](04-schema.md#require)时, 您正在制作一个依赖于其他包的包。项目和库之间的唯一区别是您的项目是一个没有名称的包.

为了使该软件包可安装, 您需要给它一个名称。可以通过在`composer.json`中添加 [`name`](04-schema.md#name) 属性来完成此操作:

```json
{
    "name": "acme/hello-world",
    "require": {
        "monolog/monolog": "1.0.*"
    }
}
```

在这种情况下, 项目名称是 `acme/hello-world`, 其中 `acme` 是供应商名称。提供供应商名称是必需的.

> **注意:**  如果您不知道使用什么作为供应商名称, 您的 GitHub 用户名通常是一个很好的选择。虽然包名称不区分大小写,但惯例是用小写和连字符分隔单词.

## 库版本控制

在绝大多数情况下, 您将使用某种版本控制系统 (如 git、svn、hg或fossil) 维护您的库。在这些情况下, Composer从 VCS 中推断出版本, 你**不应**在`composer.json` 文件中指定版本。(请参阅[版本文章](articles/versions.md)以了解Composer如何使用 VCS 分支和标记来解析版本约束。)

如果您是手动维护包 (即没有 VCS), 则需要通过在作`composer.json`文件中添加`version`值来显式指定版本:

```json
{
    "version": "1.0.0"
}
```

> **注意:** 将硬编码版本添加到 VCS 时, 该版本将与标记名称冲突。Composer将无法确定版本号。

### VCS 版本控制

Composer使用您的 VCS 的分支和标记功能来解决您在 `require` 字段中指定的特定文件集的版本约束。在确定有效的可用版本时, Composer将查看所有标记和分支, 并将其名称转换为一个内部选项列表, 然后将其与所提供的版本约束相匹配.

有关Composer如何处理标记和分支以及它如何解析包版本约束的更多内容, 请阅读[版本](articles/versions.md)文章。

## 锁文件

对于您的库, 您可以提交`composer.lock` 文件(如果需要)。这可以帮助您的团队始终针对相同的依赖项版本进行测试。但是, 此锁文件不会对依赖于它的其他项目产生任何影响。它只对主项目有影响.

如果不希望提交锁文件, 并且使用 git, 请将其添加到`.gitignore`。

## 发布到 VCS

一旦您拥有了一个包含`composer.json` 文件的 VCS 仓库 (版本控制系统, 例如 git), 您的库就已经可以由composer安装了。在此示例中, 我们将在`github.com/username/hello-world` 下的 GitHub 上发布`acme/hello-world` 库。

现在, 为了测试安装 `acme/hello-world`软件包, 我们在本地创建一个新项目。我们将称之为 `acme/blog`。这个博客将依赖`acme/hello-world`, 这反过来依赖`monolog/monolog`。我们可以通过在某个地方创建一个新的博客目录来实现这一点, 其中包含一个`composer.json`:

```json
{
    "name": "acme/blog",
    "require": {
        "acme/hello-world": "dev-master"
    }
}
```

在这种情况下不需要该名称, 因为我们不想将该博客发布为库。这里添加的是为了澄清哪个`composer.json` 正在被描述。

现在, 我们需要告诉博客应用程序在哪里可以找到 `hello-world`的依赖。我们通过向博客的`composer.json`添加一个软件包仓库说明来做到这一点:

```json
{
    "name": "acme/blog",
    "repositories": [
        {
            "type": "vcs",
            "url": "https://github.com/username/hello-world"
        }
    ],
    "require": {
        "acme/hello-world": "dev-master"
    }
}
```

有关包仓库的工作方式以及可用的其他类型的详细信息, 请参阅[仓库](05-repositories.md).

就这样。现在, 您可以通过运行Composer的[`install`](03-cli.md#install)命令来安装依赖项!

**回顾:** 任何 git/svn/hg/fossil仓库包含`composer.json` 可以通过指定包仓库并在 [`require`](04-schema.md#require) 字段中声明依赖项来添加到项目中。

## 发布到packagist

好了, 现在您可以发布软件包了。但是每次指定 VCS 仓库都很麻烦。你不想强迫所有的用户这样做.

您可能注意到的另一件事是, 我们没有为`monolog/monolog`指定一个包仓库。那是怎么做的？答案是 Packagist.

[Packagist](https://packagist.org/)是Composer的主要包库, 默认情况下是启用的。在 Packagist 上发布的任何内容都可以通过Composer自动获得。由于[Monolog 在Packagist](https://packagist.org/packages/monolog/monolog) 上, 我们可以依赖它, 而不必指定任何附加的仓库.

如果我们想与世界分享`hello-world`, 我们也会在 Packagist 上发布它。这样做是很容易的.

您只需访问 [Packagist](https://packagist.org) 并点击 "Submit" 按钮即可。这将提示您注册如果您尚未, 然后允许您提交 URL 到您的 VCS 仓库, 此时 Packagist 将开始抓取它。一旦完成, 您的包将可被任何人使用!

&larr; [基本用法](01-basic-usage.md) |  [命令行接口](03-cli.md) &rarr;
