# [译]怎样使用Node.js开发一个交互式的命令行应用

> [原文链接](https://www.smashingmagazine.com/2017/03/interactive-command-line-application-node-js/)

## 概览

过去的五年中，Node.js给软件开发带来了一致性（原文：uniformity）。你可以用Node.js做任何事，无论是前端开发、服务端脚本、跨平台座面应用、跨平台移动应用还是物联网，任何你能说出来的。因为Node.js，写一个命令行工具也比以前容易得多——不仅是任何命令行工具，而且是一个可交互的、有用的、更容易开发的工具。

如果你是一个前端开发者，你一定听说过Gulp、Angular CLI、Cordova、Yeoman等，你是否曾经想象过他们怎么工作的呢？例如，在`Angular CLI`中，通过允许类似`ng new <project-name>`的命令，你将创建一个基于基本配置的Angular项目。

而例如`Yeoman`的工具，则可以在运行时让你输入，最终帮你自定义你项目的配置。一些`yeoman`的生成器帮助你在生产环境中部署你的项目。

这正是我们今天将要学习的。

> 在这个教程中，我们将开发一个命令行应用，这个应用接收一个包含客户信息的CSV格式文件，然后使用`SendGrid API`给他们发邮件，下面是这篇教程的一个大致目录：

## Hello World

这篇教程中假设你已经在你的系统中安装了`Node.js`，如果你还没有安装，请先安装它。`Node.js`同样可以使用`NPM`这个包管理器来安装。使用`NPM`，你可以安装很多开源的包。你可以通过`npm`官网获取所有的包信息。在本项目中，我们将使用多个开源模块（以后会用得更多）。现在，让我们使用`npm`创建一个`Node.js`项目吧。

```bash
$ npm init
name: broadcast
version: 0.0.1
description: CLI utility to broadcast emails
entry point: broadcast.js
```

我创建了一个名为`broadcast`的目录，在该目录下运行`npm init`命令。正如你看见的，我提供了一些这个项目的基本信息，比如名字、描述、版本、入口等。这个入口是一个无论在哪里执行都会以它开始的主要`js`文件。Node.js默认使用index.js作为入口文件，然而在本例中，我们改成了`broadcast.js`。当你运行`npm init` 命令，你将获得一些选项，例如：git仓库、license和作者。你既可以提供值也可以留空。

如上操作，成功执行`npm init`后，你将在根目录下面找到一个被创建的`package.json`文件，这就是我们的配置文件。它包含了我们在创建项目的时候提供的配置信息。你可以在`npm`的官方文档中浏览更多关于`package.json`的信息。

现在我们的项目创建了，让我们创建一个 helllo world 程序。作为开始，我们需要先在项目中创建一个`broadcast.js`文件，这将是你的主文件，在文件中写入以下代码片段：

```js
console.log('hello world');
```

现在，让我们运行一下。

```bash
$ node broadcast
hello world
```

正如你所见，`hello world`被打印到`console`中。你既可以允许`node broadcast.js`也可以运行`node broadcast`，Node.js能够识别其中的不同。

根据`package.json`文档，其中有一个名为`dependencies`的选项，该选项可以维护所有你计划在项目中使用的第三方模块，根据它们的版本号。正如 所提到的，我们将使用很多第三方开源模块来开发这个工具。在我们的例子中，`package.json`看起来像这样：

```js
{
  "name": "broadcast",
  "version": "0.0.1",
  "description": "CLI utility to broadcast emails",
  "main": "broadcast.js",
  "license": "MIT",
  "dependencies": {
    "async": "^2.1.4",
    "chalk": "^1.1.3",
    "commander": "^2.9.0",
    "csv": "^1.1.0",
    "inquirer": "^2.0.0",
    "sendgrid": "^4.7.1"
  }
}
```

你需要特别注意的是，我们将使用 [Async](http://caolan.github.io/async/docs.html)、[Chalk](https://github.com/chalk/chalk)、[Commander](https://www.npmjs.com/package/commander)、[CSV](https://www.npmjs.com/package/csv)、[Inquirer.js](https://github.com/SBoudrias/Inquirer.js/)和[SendGrid](https://github.com/sendgrid/sendgrid-nodejs)。随着本教程的进行，这些模块的用法我们将详细解释。

## 操作命令行参数

读命令行参数并不困难，你可以简单地使用[process.argv](https://nodejs.org/docs/latest/api/process.html)读取它们。然而，解析它们的值和选项是一个复杂的工作。所以，取代重复造轮子，我们将使用`Commander`模块。`Commander`是一个开源的Node.js模块，它是一个实现可交互命令行的工具。它自带非常有趣的功能用于解析命令行选项，并且它有着类`git`的子指令，





















 