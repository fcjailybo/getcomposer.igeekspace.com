# 简介

Composer是PHP的库依赖关系管理工具。您可以在您的项目中声明依赖的库，Composer能帮您安装这些库。

## 依赖关系管理

Composer不是包管理工具，它处理“包”或库，却是基于每个项目进行管理，它将“包”和库安装在您项目中的一个文件夹（例如“vendor”）下。默认情况下，它不会全局安装任何东西。因此，它是一个依赖关系管理工具。

这种想法并不新鲜，Composer受到了node的[npm](http://npmjs.org/)和ruby的[bundler](http://gembundler.com/)的强烈启发.但是没有一款针对于PHP的类似的工具。

Composer能解决以下几个问题:

a) 您有一个项目依赖于若干个库。

b) 其中一些库又依赖于其他库。

c) 声明您所依赖的东西。

d) Composer 会找到要安装的那些库对应的版本，然后完成安装（即将对应的库下载到您的项目中）。

## 声明依赖关系

比方说，您正在创建一个需要日志库的项目，您决定使用 [monolog](https://github.com/Seldaek/monolog)。想要将它添加到您的项目中，你只需要创建一个文件名为“composer.json”的 文件，在该文件中定义您项目中库的依赖关系。

```json
{
    "require": {
        "monolog/monolog": "1.2.*"
    }
}
```

我们简单的定义，我们的项目需要版本号为1.2的`monolog/monolog`库。

## 系统要求

Composer需要PHP 5.3.2以上的版本才能正常运行。同时必须进行一些敏感的 PHP 设置和编译标志，不过不用担心，任何不兼容项，Composer安装程序都会抛出警告。

通过源码安装而不是zip包安装库的话，您可以需要git，svn或者hg工具，用哪个工具取决于您要安装的库使用的是哪种版本控制系统。

Composer能在多个操作系统上运行，我们也致力于让它能在Windows，Linux和OSX上运行得同样出色。

## 在Linux / Unix / OSX下安装 -

### 下载Composer可执行文件

简而言之,有两种方法安装Composer：作为项目的局部安装；作为系统全局可执行的全局安装；

#### 局部安装

局部安装Composer只需要在您项目文件夹中运行安装脚本：

```sh
curl -sS http://getcomposer.igeekspace.com/installer | php
```

> **注意:** 如果按照上面的方法安装失败，您可以通过php下载安装脚本进行安装

```sh
php -r "readfile('http://getcomposer.igeekspace.com/installer');" | php
```

安装脚本将会检测一些PHP设置，然后下载`composer.phar`到您当前工作目录。这是Composer二进制文件，它是一个 PHAR (PHP archive)，它是一个PHP归档包，能够在命令行中直接运行。

您可以通过`--install-dir`选项指定一个路径（可以是绝对路径或者相对路径）将composer安装到指定路径。

```sh
curl -sS http://getcomposer.igeekspace.com/installer | php -- --install-dir=bin
```

#### 全局安装

您可以将Composer文件放在任何您想要的位置，只要您将这个位置添加到 `PATH`变量中,您便可以在任何位置使用composer命令，在类Unix系统中，您甚至可以直接运行它而不需要在前面加`php`。

通过运行下面的命令，你可以在任何位置通过输入 `composer` 命令执行Composer：

```sh
curl -sS http://getcomposer.igeekspace.com/installer | php
mv composer.phar /usr/local/bin/composer
```

> **注意:** 如果上述命令执行失败，用管理员的身份执行mv命令。

> **注意:** 在有些版本的OSX中，`/usr`目录默认不存在。如果您收到"/usr/local/bin/composer: No such file or directory"的错误提示，您可以在执行命令前手动创建`/usr/local/bin/`目录。

执行完后，您只需运行`composer`命令便能执行Composer，而不需要再输入`php composer.phar`。

## 在Windows下安装

### 通过安装包安装

最简单的方法便是下载安装包文件到您电脑上.

下载并运行[Composer-Setup.exe](http://getcomposer.igeekspace.com/Composer-Setup.exe),它将安装最新版本的Composer并且配置PATH环境变量，安装完成后，在命令行的任何目录中，您只需执行`composer`命令，便可调用Composer。

> **注意:** 关闭当前终端，新建个终端用于测试composer的功能，因为只有新建个终端，安装包设置的PATH变量才能生效。
### 手动安装

配置好您系统中的 `PATH`环境变量然后运行安装脚本来下载composer.phar：

```sh
C:\Users\username>cd C:\bin
C:\bin>php -r "readfile('(http://getcomposer.igeekspace.com/installer');" | php
```

> **注意:** 如果readfile命令执行失败, 可以使用`http`协议的URL，或者在php.ini配置文件中启用php_openssl.dll。

在`composer.phar`所属的目录下创建一个`composer.bat`脚本文件:

```sh
C:\bin>echo @php "%~dp0composer.phar" %*>composer.bat
```

关闭当前终端，新建个终端，输入下面的命令测试composer是否可用:

```sh
C:\Users\username>composer -V
Composer version 27d8904
```

## 使用Composer

现在我们能够使用Composer来安装项目依赖文件了。如果您当前目录没有`composer.json`文件 ，请跳至[基本用法](01-basic-usage.md)章节.

要安装依赖文件, 只需运行`install`命令：

```sh
php composer.phar install
```

如果您当初选择的是全局安装，并且当前文件夹下没有composer.phar文件，可以运行下面的命令：

```sh
composer install
```

依照上面例子中的配置[声明依赖关系](#声明依赖关系),它将会把monolog下载到`vendor/monolog/monolog`目录.

## 自动加载

除了下载库文件之外，Composer还准备了一个自动加载文件，它能够用来加载下载的所有的库中的所有的类。要想使用这个自动加载文件，只需要在您项目的引导文件中下面的代码。

```php
require 'vendor/autoload.php';
```

哇! 现在可以使用monolog了!想要学习更多的Composer知识, 请继续阅读"基本用法"章节。

[基本用法](01-basic-usage.md) &rarr;
