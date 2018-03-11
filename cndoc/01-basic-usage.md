# 基本用法

## 介绍

对于我们的基本使用介绍, 我们将安装`monolog/monolog`, 一个日志库。如果尚未安装Composer, 请参阅[介绍](00-intro.md)章节.

> **注意:** 为了简单起见, 本介绍将假定您已经执行了[局部](00-intro.md#locally)的Composer安装.

## `composer.json`: 项目设置

要开始在项目中使用Composer, 您需要的只是一个`composer.json`文件。此文件描述项目的依赖项, 并且可能还包含其他元数据.

### `require` 键

在`composer.json`中指定的第一个内容（通常是唯一的）是[`require`](04-schema.md#require)键。告诉Composer你的项目所依赖的软件包。

```json
{
    "require": {
        "monolog/monolog": "1.0.*"
    }
}
```

正如您所看到的, [`require`](04-schema.md#require)用一个对象, 将**包名** (例如`monolog/monolog`) 映射到**版本约束** (例如`1.0.*`)。

Composer 使用这些信息在您使用[`repositories`](04-schema.md#repositories)键注册的包“仓库（repositories）”, 或者在默认的包仓库 Packagist中 来寻找正确的文件。
。 在上面的示例中, 由于没有在`composer.json`文件中注册其他仓库(repositories), 因此假定`monolog/monolog`包在 Packagist 上注册。(有关[下面](#packagist) Packagist 的更多信息, 请参阅[此处](05-repositories.md)的有关仓库(repositories)的更多内容)。


### 包名称

包名称由供应商名称和项目名称组成。通常, 这些将是相同的-供应商名称只是为了防止命名冲突。例如, 它允许两个不同的人创建一个名为`json`的库。一个可能被命名为 `p2hp/json`, 而另一个可能是 `lenix/json`。

有关发布包和包命名的详细信息, 请参阅[此处](02-libraries.md)。(请注意, 您还可以将 "平台包" 指定为依赖项, 从而允许您要求某些版本的服务器软件。请参阅下面的[平台软件包](#platform-packages)。）

### 包版本约束

在我们的示例中, 我们要求具有版本约束[`1.0.*`](https://semver.mwl.be/#?package=monolog%2Fmonolog&version=1.0.*)的Monolog包。这意味着`1.0`开发分支中的任何版本, 或者大于或等于1.0 且小于 1.1 (`>=1.0 <1.1`) 的任何版本。

请阅读[版本](articles/versions.md), 了解有关版本、版本相互关联以及版本约束的更多详细信息。


> **Composer如何下载正确的文件？** 当你在`composer.json`中指定依赖项时, Composer首先获取您要求的包的名称, 并在使用[`repositories`](04-schema.md#repositories)键注册的任何仓库中搜索它。如果您尚未注册任何额外的仓库, 或者它在您指定的仓库中找不到具有该名称的包, 则它将返回到 Packagist 中寻找(更多 [在下面](#packagist))。
>
> 当Composer在 Packagist 或您指定的仓库中找到合适的包时, 它会使用包的 VCS (即分支和标记) 的版本控制功能来尝试找到您指定的版本约束的最佳匹配。请务必在[版本文章](articles/versions.md)中阅读有关版本和包解析的信息.

> **注意:**如果您试图请求一个包, 但Composer抛出一个关于包稳定性的错误, 您指定的版本可能无法满足默认的最低稳定性要求。默认情况下, 在您的 VCS 中搜索有效的包版本时, 只考虑稳定的版本.
>
> 如果您试图要求dev、alpha、beta 或 RC 版本的软件包, 则可能会遇到此问题。在 [结构页](04-schema.md)上阅读有关稳定性标记和最小稳定性`minimum-stability`键的更多信息。

## 安装依赖项

要安装项目的已在`composer.json`中定义的依赖项, 只需运行[`install`](03-cli.md#install)命令即可。

```sh
php composer.phar install
```

对于没有在`composer.json`中定义依赖项，还可以用`require`命令安装：
```sh
php composer.phar require monolog/monolog
```
运行`require`命令安装时，将自动将require键添加到`composer.json`文件中，并生成`composer.lock`文件：如：
```json
{
    "require": {
        "monolog/monolog": "1.0.*"
    }
}
```

运行以上命令时, 可能会发生以下两种情况之一:

### 不使用`composer.lock`进行安装

如果您以前从未运行过该命令, 也没有`composer.lock`文件存在, Composer只需解析您的`composer.json` 文件中列出的所有依赖项, 并将其文件的最新版本下载到项目中的`vendor`目录中。(`vendor`目录是项目中所有第三方代码的常规位置)。在我们上面的示例中, 您将最终得到`vendor/monolog/monolog/`源文件。如果Monolog列出了任何依赖关系, 这些依赖项也将位于 `vendor/` 下的文件夹中.

> **提示:** 如果您正在为项目使用 git, 您可能希望在`.gitignore`中添加`vendor`。您真的不想将所有第三方代码添加到您的版本控制库中.

当Composer完成安装后, 它会写入所有的包和他们的确切版本到下载的`composer.lock`文件中,将项目锁定到这些特定版本。 你应该提交`composer.lock`文件到你的项目仓库, 以便在此项目上工作的所有人都锁定在的依赖项的相同版本 (更多看下面)。

### 使用`composer.lock`进行安装

这就引出了第二种情况。如果已经有`composer.lock`文件以及`composer.json` 文件在运行`composer install`时, 它意味着您以前运行过`install`命令, 或者项目上的其他人运行了`install`命令并提交了`composer.lock`到项目 (这是好的).

无论哪种方式, 运行 `install`当`composer.lock` 文件解析并安装您在`composer.json` 中列出的所有依赖项, 但Composer使用`composer.lock`中列出的确切版本以确保包版本对每个人工作在您的项目中都一致
. 因此, 您将拥有`composer.json`文件所请求的所有依赖项, 但它们可能并非都位于最新的可用版本 (在`composer.lock`文件中列出的某些依赖项可能已在创建文件后发布了较新版本)。这是通过设计来确保项目不会因为依赖项中的意外更改而中断.

### 将`composer.lock`文件提交到版本控制

将此文件提交到 VC 是很重要的, 因为它将导致设置项目的任何人使用您所使用的依赖项的完全相同版本。您的 CI 服务器、生产机器、您的团队中的其他开发人员、所有内容和每个人都在相同的依赖项上运行, 这将减少仅影响部署某些部分的 bug 的可能性。即使您单独开发, 在重新安装项目的六月内, 您也可以对安装的依赖项仍可以工作有信心, 即使您的依赖项自那以后发布了许多新版本。(请参见下面有关使用`update`命令的说明。）

## 更新依赖到它们的最新版本

如上所述, `composer.lock`文件阻止您自动获取依赖项的最新版本。要更新到最新版本, 请使用 [`update`](03-cli.md#update)  命令。这将获取最新的匹配版本 (根据您的`composer.json`文件) 并使用新版本更新锁文件。(这相当于删除`composer.lock`文件并再次运行`install`。）

```sh
php composer.phar update
```
> **注意:** Composer将在执行`install`命令时显示警告, 如果`composer.lock`和作`composer.json`未同步.

如果只想安装或更新一个依赖项, 可以将其放入白名单:

```sh
php composer.phar update monolog/monolog [...]
```

> **注意:** 对于库, 不需要提交锁文件, 请参见: [库-锁文件](02-libraries.md#lock-file)。
 

## Packagist

[Packagist](https://packagist.org/)是主要的Composer仓库。Composer仓库基本上是一个包源: 一个可以从中获取包的地方。Packagist 的目标是成为每个人都使用的中央仓库。这意味着您可以自动`require`任何可用的包, 而无需进一步指定Composer应在何处查找包.

如果你去 [Packagist 网站](https://packagist.org/) (packagist.org), 你可以浏览和搜索包。
 

推荐任何开源项目 使用Composer在Packagist上发布它们的包.一个库不需要一定在Packagist上并由Composer使用, 但它使其他开发人员能够更快地发现和采用。

## 平台包

Composer有平台包, 这是在系统上安装一些东西的虚拟包, 但实际上不是由Composer安装的。这包括 php 本身、php 扩展和一些系统库.

* `php`表示用户的 PHP版本, 允许您应用约束, 例如 `>=5.4.0`。如果要求64位版本的 php, 您可以要求`php-64bit`包.

* `hhvm` 表示 HHVM 运行时的版本, 并允许您应用约束, 例如`>=2.3.3`。

* `ext-<name>` 允许您要求 PHP 扩展 (包括核心扩展)。在这里版本控制可能是相当不一致的, 所以把约束设置为`*`通常是个好主意。扩展包名称的一个示例是`ext-gd`.

* `lib-<name>`  允许对 PHP 使用的库版本进行约束。以下是可用的: `curl`, `iconv`, `icu`, `libxml`,
  `openssl`, `pcre`, `uuid`, `xsl`。

您可以使用 [`show --platform`](03-cli.md#show)获取本地可用平台包的列表。

## 自动加载

对于指定自动加载信息的库, Composer将生成一个`vendor/autoload.php`文件。您可以简单地包含此文件并开始使用这些库提供的类, 而无需任何额外的工作:

```php
require __DIR__ . '/vendor/autoload.php';

$log = new Monolog\Logger('name');
$log->pushHandler(new Monolog\Handler\StreamHandler('app.log', Monolog\Logger::WARNING));
$log->addWarning('Foo');
```

您甚至可以通过将[`autoload`](04-schema.md#autoload)字段添加到`composer.json`将自己的代码添加到自动加载器中.

```json
{
    "autoload": {
        "psr-4": {"Acme\\": "src/"}
    }
}
```

Composer将为 `Acme` 命名空间注册一个 [PSR-4](http://www.php-fig.org/psr/psr-4/) 自动加载器。

您可以定义从命名空间到目录的映射。`src`目录将位于项目根目录, 即与`vendor`目录相同的级别上。一个示例文件名是 `src/Foo.php` 包含一个 `Acme\Foo`  类.

添加[`autoload`](04-schema.md#autoload)字段后, 必须重新运行[`dump-autoload`](03-cli.md#dump-autoload)以重新生成`vendor/autoload.php` 文件.

包含该文件还将返回自动加载器实例, 因此您可以将包含调用的返回值存储在变量中, 并添加更多的命名空间。这对于测试套件中的自动加载类很有用，例如.

```php
$loader = require __DIR__ . '/vendor/autoload.php';
$loader->addPsr4('Acme\\Test\\', __DIR__);
```

除了 PSR-4 自动加载, Composer还支持 PSR-0、classmap 和文件自动加载。有关详细信息, 请参阅[`autoload`](04-schema.md#autoload)。

另请参阅[优化自动加载](articles/autoloader-optimization.md)的文档。

> **注意:** Composer提供了自己的自动加载器。如果你不想使用那个, 你可以只包含`vendor/composer/autoload_*.php` 文件, 它返回关联数组, 使您可以配置自己的自动加载器.

&larr; [介绍](00-intro.md)  |  [库（资源包）](02-libraries.md) &rarr;
