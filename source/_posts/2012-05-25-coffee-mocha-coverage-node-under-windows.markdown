---
layout: post
title: "在Windows上使用mocha对基于Node.JS的服务器端CoffeeScript进行测试并通过jscoverage生成覆盖率报告"
date: 2012-05-26 22:00
comments: true
published: true
categories: 
- coffeescript
- coffee
- javascript
- mocha
- jscoverage
- windows
- testing
- nodejs
---
最近在捣鼓一个基于[Node.JS][]的东西，语言自然是JavaScript了。但后来发现了[CoffeeScript][]，发现确实简便很多，于是变节到了[Coffee][]阵营。

写了两个小模块，忽然想到要测试。最初找到了[Jasmine][]，后来又发现了[mocha][]。经过一番比较斟酌，觉得既然配咖啡（[Coffee][]），自然还是得摩卡（[mocha][]）。所以最终决定使用[mocha][]来做测试工具。

好吧，实际原因是在[mocha][]的主页上看到它支持代码覆盖率检查。后来经过各种折腾才总算搞定了这个覆盖率检查以及报告的查看问题。

其实，这一切在Linux上应该是非常简单的，但我手上只有Windows，所以一波三折。介于网上相关的信息有些零散，并且不够傻瓜，这里做个总结，也算给自己留个笔记吧。

<!-- more -->
## 安装必需品
要做这一切，首先上面提到的各种需要的软件是一定要先安装的，下面就一个一个来介绍一下。

### Node.JS
这个不消说，一切都是基于[Node.JS][]之上的，所以绝对是第一等必需品。[Node.JS][]发展到今天，安装也很简单，从主页上下载Windows版，安装即可。

安装后，打开命令行窗口，执行

	node --version

看看是不是正确输出了版本号？如果是，恭喜，安装成功了！如果没有……自己上网找找解决方案吧。这个不在本文的讨论范围之内。

#### NPM
另外，[Node.JS][]的包管理工具[NPM][]也应该随着上面的安装一同安装好。可以执行以下命令来验证。

查看版本号

	npm --version

或者，查看帮助

	 npm --help

### CoffeeScript
[CoffeeScript][]是一种对JavaScript的简化，它极大程度上地减少了JavaScript里面的各种括号和分号。最终还是要编译成JavaScript执行的。

具体的区别和例子，可以到[Coffee][]的主页上去瞅瞅。一目了然。至于安装，在那里也写得[一清二楚](http://coffeescript.org/#installation)。介于都是英文，这里还是做一下简单的介绍吧。

首先，[Coffee][]是[Node.JS][]的一个包，所以就可以用[NPM][]来很方便地进行安装。命令如下：

	npm install -g coffee-script

这里为什么有个`-g`？`-g`的意思是`install globally`，也就是全局安装。其实说白了就是安装后的包中所带的命令可以直接在命令行执行而不需要去指定所在的目录。比如，按照上面所写的带有`-g`参数的命令安装后，可以在命令行的任意目录下执行

	coffee --version

来查看版本。如果上面命令执行成功了，恭喜你！[CoffeeScript][]已经安装好了。

### mocha
[mocha][]是针对JavaScript的一个测试工具。具体细节请自行参看主页。英文啃不动，请自行寻找辞典。外语学习等有空我另撰文来讨论。

言归正传，[mocha][]也是[Node.JS][]的一个包，所以……对了，你很聪明，用[NPM][]来解决！（什么？你没想到？没关系，跟我一样有前途。）

	npm install -g mocha

因为[mocha][]也是个需要执行命令的工具，所以我们继续加上`-g`。

这里顺便说一下**`-g`什么时候用**。如果所要安装的包是以命令的方式被使用，也就是会通过命令行调用其中已经做好的工具命令，就最好加上`-g`。这样，在命令行下随时可以使用那些命令。如果所要安装的包是需要在代码中被包含，也就是`require('xxx')`，则不要加`-g`，而且必须在你的程序所在的目录或者上级的某个目录下执行`npm install`。

跑题了，现在跑回来。验证[mocha][]安装成功的方法……或许你已经猜到了，就是下面这个带有万能验证参数`--version`的命令。

	mocha --version

至此，你已经可以写[Coffee][]代码并进行测试了。执行

	mocha --compilers coffee:coffee-script

[mocha][]就会自动运行`test`目录下的测试用例代码进行测试，并给出漂亮的结果报告。

对不起，我说早了。如果你写了测试用例，或者手快，从这个博客的下面的内容里复制了测试用例，那么运行上述命令后，十有八九是会出现类似下面这样的错误。

	node.js:201
        	throw e; // process.nextTick error, or 'error' event on first tick
              	  ^
	Error: Cannot find module 'chai'
    	at Function._resolveFilename (module.js:332:11)
    	at Function._load (module.js:279:25)
    	at Module.require (module.js:354:17)
    	at require (module.js:370:17)
    	........（以下省略若干行）

原因是我们缺少测试时必须的检验库（assertion library），下面就来说一下检验包的安装。（如果你没有写任何测试用例，上面的命令不会出错，但为了后面不折腾，还是跟着下面的内容安装一下为好。）

### should.js / expect.js / chai
使用[mocha][]进行测试，需要自备数据检验库。数据检验库的作用是用来验证期待值与实际值一致与否的。[mocha][]支持的检验库有如下三种：

* [should.js](https://github.com/visionmedia/should.js)：使用 _被检验值_.should._检验方法_(_期待值_) 的语法进行验证。
* [expect.js](https://github.com/LearnBoost/expect.js)：使用 expect(_被检验值_).to._检验方法_(_期待值_) 的语法进行验证。
* [chai](http://chaijs.com/)：支持以上两种语法和 assert._检验方法_(_被检验值_, _期待值_) 的语法。

在网上查找的结果发现should.js有诸多不便，又因为chai包罗万象，所以我决定采用chai来进行自己的测试。

安装方法在各个检验库的主页中都有所描述。而且，既然是基于[Node.JS][]的库，自然需要祭出[NPM][]这一神器。以chai为例，命令如下：

	npm install chai

因为我们在测试用例代码中要引入这个库（或者说包），所以这里没有加`-g`参数。另外，执行此命令的目录要在项目的根目录下，这样就可以在项目下的任何目录的代码中引入它了。

安装成功后，会在目录下建一个名为`node_modules`的目录。这个目录是专门用来放各种依赖包的。要确认安装了哪些包，可以用

	npm list

来查看。执行上面命令，如果你看到类似`chai@1.0.1`的内容，说明安装成功了。`@`后面是版本号。

另外，为了便于管理依赖包，也可以使用`package.json`文件进行记录和管理，然后使用不带参数的`npm install`命令自动安装所有缺失的包。

我们这个例子的`package.json`文件如下：

{% codeblock package.json lang:json https://github.com/programus/coffee-mocha-nodejs-coverage-windows-example/blob/master/package.json github-source %}
{
    "name":          "coffee-mocha-nodejs-coverage-windows-example"
  , "version":       "0.0.1"
  , "private":       true
  , "dependencies": {
      "chai":        "1.0.1"
  }
}
{% endcodeblock %}

其中`dependencies`中就记录了依赖的包和版本号。

装好了`chai`，测试应该就不成问题了。

**然而！**

然而我们的重点在于覆盖率如何得到。我最初就是卡在这里了。[mocha][]的文档中虽然提到一个`html-cov`的**reporter**，但执行`mocha --compiler coffee:coffee-script -R html-cov`得到的覆盖率永远都是0%，而且所执行过的代码也没有标记。仔细看看文档，发现有这么一段话：

> The library being tested should first be instrumented by [node-jscoverage][], this allows Mocha to capture the coverage information necessary to produce a single-page HTML report.

也就是说，要想导出覆盖率报告，那么被测试的代码必须是被[node-jscoverage][]“蹂躏”过的。跟着链接看了一眼node-jscoverage，居然是个需要在Linux下面编译的东西。这可让我这个在Windows下忍辱负重的家伙如何是好？

没关系，[node-jscoverage][]再强大，充其量不过是[jscoverage][]的加强版。所以，我们直接去找它的本家[jscoverage][]去。到下载页一瞧，本家果然够意思，已经有了编译好的Windows压缩包。那么现在说一下下一个需要安装的软件——

### JSCoverage
仔细读读文档，会发现[JSCoverage][]的工作原理是把你写好的JavaScript程序给加一层“壳”，加壳的程序的执行结果与原本的程序相同，但这层壳会在代码执行时记录执行过的代码，从而最终统计出代码覆盖率。

又跑题了，拉回来，继续说安装的事儿。在[下载页](http://siliconforks.com/jscoverage/download.html)的下半部分可以找到编译好的Windows执行程序。下载之，然后解压缩到一个适当的目录（为了减少未来的麻烦，不建议目录名中带有空格）。再接下来，为了可以在命令行中执行`jscoverage`，将可执行文件所在的目录加入环境变量的`PATH`里面。系统环境变量的修改，请参考[这里](http://baike.baidu.com/view/95930.htm)的[这个图](http://baike.baidu.com/albums/95930/95930/0/0.html#0$d019d2bf676f0a3418d81f09)。

做好上述工作后，在命令行窗口里执行我们的万能验证命令

	jscoverage --version

什么？出错了？我没说修改了环境变量之后，需要重新打开新的命令行窗口才有效吗？没说？真的没说？那好吧，现在重新启动命令行窗口再试试。

## 组织代码

### 业务代码
至此，需要的软件都安装好了。下面就该写我们的代码了。为了今后处理方便，建议将代码写在`lib`目录下。原因？据说就是一种约定俗成（据说是因为从网上查到的，不是我说的）。

比如，我写了如下代码（一个问候者）：
{% codeblock lib/models/index.coffee lang:coffeescript https://github.com/programus/coffee-mocha-nodejs-coverage-windows-example/blob/master/lib/models/index.coffee github-source %}
class Greeter
  constructor: (@lang) ->
    if !@lang?
      @lang = 'en'
  
  sayHello: (name) ->
    switch @lang
      when 'jp'
        # nameさん、こんにちは。
        "#{if name? then "#{name}\u3055\u3093\u3001" else ""}\u3053\u3093\u306B\u3061\u306F\u3002"
      when 'zh'
        # name，你好！吃了没？
        "#{if name? then "#{name}\uFF0C" else ""}\u4F60\u597D\uFF01\u5403\u4E86\u6CA1\uFF1F"
      else
        "Hello#{if name? then ", #{name}" else ""}!"

exports.Greeter = Greeter
{% endcodeblock %}
放在`lib/models`下，并命名为`index.coffee`。命名为index，在引入模块的时候可以直接以目录名来引入。

### 测试用例代码
代码写好了，接下来写一下测试用例的代码。之前提过，[mocha][]会自动处理`test`目录下的内容，所以测试用例代码我们都放在`test`目录下。

我们的`Greeter`类有两个函数：构造函数和`sayHello()`函数。而`sayHello()`函数的参数又是可选的。因此测试代码如下：
{% codeblock test/models.test.coffee lang:coffeescript https://github.com/programus/coffee-mocha-nodejs-coverage-windows-example/blob/master/test/models.test.coffee github-source %}
# Test suites for all models
chai = require 'chai'
should = chai.should()

models = require('../req') 'models'
Greeter = models.Greeter

langs = [
  'zh'
  'en'
  'jp'
]

hellos =
  zh: '你好！吃了没？'
  en: 'Hello!'
  jp: 'こんにちは。'

hellosWithName =
  zh:
    name: '老舍'
    greet: '老舍，你好！吃了没？'
  en:
    name: 'Jack'
    greet: 'Hello, Jack!'
  jp:
    name: '武蔵'
    greet: '武蔵さん、こんにちは。'

describe 'Greeter', ->
  describe '#constructor()', ->
    it 'should have language set', ->
      new Greeter(l).lang.should.eql(l) for l in langs
    it 'should become default language, en', ->
      new Greeter().lang.should.eql('en')

  describe '#sayHello()', ->
    it 'should greet without name', ->
      new Greeter(l).sayHello().should.eql(hellos[l]) for l in langs
    it 'should greet with name', ->
      new Greeter(l).sayHello(hellosWithName[l].name).should.eql(hellosWithName[l].greet) for l in langs
{% endcodeblock %}
细心的人可能发现我最上面写的测试目标的部分和常规写法不太一样。通常会写作
{% codeblock lang:coffeescript %}
models = require '../lib/models'
{% endcodeblock %}
而我这里写成了
{% codeblock lang:coffeescript %}
models = require('../req') 'models'
{% endcodeblock %}
为什么呢？这就涉及到我为[mocha][]的测试写的一个小工具——

### 测试辅助工具
这个工具其实很简单，没几行：
{% codeblock req.coffee lang:coffeescript https://github.com/programus/coffee-mocha-nodejs-coverage-windows-example/blob/master/req.coffee github-source %}
module.exports = (path)->
  try
    require "./lib-cov/#{path}"
  catch e
    require "./lib/#{path}"
{% endcodeblock %}
作用就是检查在`./lib-cov`目录下是否有我们所需的模块，如果有，则导入之，如果没有，则导入`./lib`下的响应模块。

刚才说过，要把代码组织到`lib`目录（因为req.coffee放在项目根目录下，`.`又是当前目录，所以这里的`lib`跟上文的`./lib`是一会儿事儿）下。我们又说过，[JSCoverage][]计算覆盖率的做法是将代码加个壳，然后让测试程序调用加壳后的代码。这个加壳后的代码就被存储在`lib-cov`目录下（当然是可以自己指定的目录）。所以，使用这个`req.coffee`就可以让[mocha][]在有`lib-cov`目录的情况下使用`lib-cov`下的内容进行测试，从而为生成覆盖率报告做好准备。

在网上搜索，可以看到大多数解决方案是让设置一个环境变量`XXX_COV`为1来达到上述目的。而且是以类似下面的样子，将其写到`makefile`里面
{% codeblock lang:make %}
	XXX_COV=1 mocha --compilers coffee:coffee-script -R html-cov > coverage.html
{% endcodeblock %}
同时在测试代码中检查环境变量
{% codeblock lang:coffeescript %}
	models = require if process.env.XXX_COV then '../lib-cov/models' else '../lib/models'
{% endcodeblock %}
但经试验，如上`makefile`中设定环境变量的语句在Windows下无效。故此追加了这样一个测试辅助工具。同时，在测试时保证每次生成完覆盖率报告都删除`lib-cov`目录，就可以正常使用了。

## 进行测试
到这里，我们的准备工作都基本完成了，下一步就是开始进行测试了。

比起上面的各种折腾，测试倒是显得非常容易。

### 单纯测试
先说一下单纯的测试。

其实上面也曾提到过，只需要执行如下一条命令，就可以得到结果：

	mocha --compilers coffee:coffee-script

用这条命令测试[Coffee][]的代码，不需要格外进行编译。（据说老版本的[mocha][]里自动支持[Coffee][]，不需要指定后面的一长串参数。）

测试后，应该会有如下输出：

<div style="background-color:black;font-family:monospace;font-size:13px;">
<code>
<div></div>
<div style="color:#848284">&#160;&#160;....</div>
<div></div>
<div>&#160;&#160;<span style="color:#0f0">✔</span><span style="color:#008200"> 4 tests complete </span><span style="color:#848284">(6ms)</span></div>
<div></div>
</code>
</div>

测试成功！

### 代码覆盖率报告
接下来说一下重头戏——如何产生代码覆盖率的报告。

[mocha][]生成报告只需要指定相应的reporter即可。但前提是需要使用[JSCoverage][]处理一下代码。

[JSCoverage][]，顾名思义，只能处理JavaScript的代码，对[CoffeeScript][]目前还是视而不见的。故而，我们首先要将[CoffeeScript][]编译成JavaScript：

	coffee -o lib-js -c lib

这条命令，会递归地将`lib`目录下的所有[coffee][]代码编译成js文件，并以同样的目录结构存储到`lib-js`目录中。

比如，我的`lib`目录中的文件目前如下（使用`tree /f`命令生成的结果，根目录内容有部分删节）：

	\coffee-mocha-nodejs-coverage-windows-example\lib
	└─models
        	index.coffee

编译后的`lib-js`目录则如下：

	\coffee-mocha-nodejs-coverage-windows-example\lib-js
	└─models
        	index.js

当然，你也可以去掉`-o lib-js`的部分，将js文件编译到[coffee][]文件的身边。我倾向于把他们分清楚，所以分了一下目录。

有了js文件，就可以使用[JSCoverage][]了。命令如下：

	jscoverage --no-highlight lib-js lib-cov

这样就将`lib-js`目录里的js文件处理好并放进了`lib-cov`目录。至于那个`--no-highlight`参数，是为了去掉代码高亮的。如果不去掉，控制高亮的HTML代码会在最终报告中被转义，导致代码严重不可读。（不服你自己试试就知道了。）

现在，我们生成覆盖率报告的材料都准备好了，执行最终的测试命令：

	mocha --compilers coffee:coffee-script -R html-cov > coverage.html

结束后，打开`coverage.html`文件，就可以看到最终的报告了。

如果你的代码是从上面的博客内容中复制的，那么覆盖率应该是100%。覆盖率报告的样子应该与此大致相同：[coverage.html](/downloads/pages/coverage.html)

如果你对报告有所怀疑，可以将测试用例中的一部分`it`改成`xit`（`xit`代表此条不进行测试），然后再次运行上述命令试试看。

至此，我们实现了在Windows上使用[mocha][]对基于[Node.JS][]的服务器端[CoffeeScript][]进行测试并通过[JSCoverage][]生成覆盖率报告。

**但是……**

会不会觉得上面那么多命令，每次敲来敲去会让手指发疼，关节发麻，有腱鞘炎倾向？没关系，我们仍然有更好的解决方案——

## 使用make
`make`是Linux下面的著名命令。用来方便地批量地完成编译、安装等一系列工作。说实话，我对`make`也不是了解太多，虽然以前有所接触，但基本都是这次研究这个测试问题才开始真正学习了一些。

如果你要搜索[mocha][]的覆盖率测试，估计很多结果中都提到了在`makefile`中如何动手脚的文章。由于不懂`make`到底是什么，被误导了不少，后来才明白，文中提到的`makefile`其实都是他们自己写的文件。有了那神奇的`makefile`，就可以用非常简单的命令来批量执行上面的各种或长或短的命令了。

### 安装make
我使用的是[GNU Make for Windows](http://gnuwin32.sourceforge.net/packages/make.htm)。

为了使用[Octopress][]这套博客系统，之前安装了[Ruby][]，里面自带了`make`，所以我就没有安装。

如果你要独立安装`make`，可以到[主页](http://gnuwin32.sourceforge.net/packages/make.htm)上寻找安装程序，然后安装之。再然后，用万能确认命令确认：

	make --version

如果出错，很可能是环境变量的`PATH`没有设置好，手动设置一下即可。

### 撰写makefile
安装好`make`后，就需要撰写`makefile`了。这是个挺麻烦的事儿。因为很多Windows的命令都不被支持。好在你看到这篇文章时，已经有我这个大善人写好了一份现成的`makefile`了。你只需复制或者下载即可，内容如下：
{% codeblock makefile lang:make https://github.com/programus/coffee-mocha-nodejs-coverage-windows-example/blob/master/makefile github-source %}
	REPORT_FILE=coverage.html
	RMJS=rmdir.js
	RMV=node $(RMJS)
	# change this if using linux or others
	BROWSE=start

	all: | test-all coverage

	# use this to cross platform
	$(RMJS):
		@echo 'var f=require("fs"),t=require("path");var r=function(a){var b=f.readdirSync(a);for(var c=0;c<b.length;c++){var d=b[c];if(d!=="."&&d!=".."){d=t.join(a,d);if(f.statSync(d).isDirectory()){r(d)}else{try{f.unlinkSync(d)}catch(e){}}}}try{f.rmdirSync(a)}catch(e){}};try{r(process.argv[2])}catch(e){}' > $(RMJS)

	# use this to cross platform
	rmtools:
		@coffee -e "require('fs').unlink 'rmdir.js'"

	test-all:
		@echo 'Testing...'
		@mocha --compilers coffee:coffee-script

	compile-coffee: $(RMJS)
		@$(RMV) lib-js
		@echo 'CoffeeScript -> JavaScript compiling...'
		@coffee -o lib-js -c lib
		@$(RMV) -p

	clean-compile:
		@$(RMV) -p

	jscoverage:
		@jscoverage --no-highlight lib-js lib-cov

	mocha-html-cov:
		@echo 'Testing and generating coverage report...'
		@mocha --compilers coffee:coffee-script -R html-cov > $(REPORT_FILE)

	clean-coverage:
		@$(RMV) lib-js
		@$(RMV) lib-cov

	open-coverage:
		@echo 'Openning report in your default browser...'
		@$(BROWSE) $(REPORT_FILE)

	clean-report:
		@$(RMV) $(REPORT_FILE)

	compile: | $(RMJS) compile-coffee rmtools

	coverage: | $(RMJS) compile-coffee jscoverage mocha-html-cov clean-coverage rmtools open-coverage

	clean: | $(RMJS) clean-compile clean-coverage clean-report rmtools

	.PHONY: all test-all compile-coffee clean-compile jscoverage mocha-html-cov clean-coverage open-coverage clean-report compile coverage clean rmtoos
{% endcodeblock %}
虽说我是个大善人，但毕竟也是第一次写`makefile`，如果你是懂行的，发现了不足之处，请一定留言告诉我。

这里，有几点简单说明一下。

* 一个是`RMJS`这个东西。这是一个递归删除目录的js文件，在`makefile`中通过`echo`命令将内容写到文件中，并在用完后删除。之所以这么做，是因为无论是Windows的`rd`命令还是Linux的`rm`命令（当然是for Windows版本）都无法很好地删除目录。Windows的`rd`命令的问题是，`make`貌似不识别它，即便识别了，也会在`/s /q`（静默递归删除）参数上出现错误。而`rm`命令的问题更有趣。其它目录都没有问题，但在编译[Coffee][]的命令执行后，如果在编译期间创建了新的目录则会产生一个名为`-p`的目录。估计是内部使用了Linux下的`mkdir -p`命令造成的。当使用`rm -rf -p`来删除时，`-p`会被识别为`rm`命令的参数而导致失败。故而，制造了这个`RMJS`。同时还可以达到跨平台的目的。同样，删除文件也用的是[coffee][]脚本完成的。

* 另一个是`BROWSE`，在Windows下`start`命令可以使用默认打开方式打开文件，所以将查看HTML报告的命令定义为`start`。基本来讲，这里的`makefile`是可以跨平台移植的，当移植到其他平台时，需要修改此命令的定义。

* 最后一点，也是最重要的一点：**文件名必须是小写的`makefile`**。如果大小写不对，会导致`make`无法识别。

### 使用make
那么，写好了这个`makefile`，怎么用呢？

只要到有`makefile`的目录下，执行`make`命令即可。

单纯的测试，执行：

	make test-all

编译[CoffeeScript][]，执行：

	make compile

代码覆盖率报告，执行：

	make coverage

先测试，然后出覆盖率报告：

	make

清理现场（删除中间生成的目录、文件）：

	make clean

## 全部内容
本文中提到的所有代码，都被提交到了[GitHub][]上，工程名字叫[coffee-mocha-nodejs-coverage-windows-example](https://github.com/programus/coffee-mocha-nodejs-coverage-windows-example)。（好吧，我知道名字长了点……）

有兴趣的话，可以到那里取得所有的代码。

{% include links %}
