# AngularJS入门教程00：引导程序 #

<div class="post-text">
<p>我们现在开始准备编写AngularJS应用——<strong>phonecat</strong>。这一步骤（步骤0），您将会熟悉重要的源代码文件，学习启动包含AngularJS种子项目的开发环境，并在浏览器端运行应用。</p>

<ol>
<li><p>进入angular-phonecat目录，运行如下命令：</p>

<pre class="prettyprint"><code><span class="pln">git checkout </span><span class="pun">-</span><span class="pln">f step</span><span class="pun">-</span><span class="lit">0</span></code></pre>

<p>该命令将重置phonecat项目的工作目录，建议您在每一学习步骤运行此命令，将命令中的数字改成您学习步骤对应的数字，该命令将清除您在工作目录内做的任何更改。</p></li>
<li><p>运行以下命令：</p>

<pre class="prettyprint"><code><span class="pln">node scripts</span><span class="pun">/</span><span class="pln">web</span><span class="pun">-</span><span class="pln">server</span><span class="pun">.</span><span class="pln">js</span></code></pre>

<p>来启动服务器，启动后命令行终端将会提示<strong><code>Http Server running at <a href="http://localhost:8000" target="_blank">http://localhost:8000</a></code></strong>,请不要关闭该终端，关闭该终端即关闭了服务器。在浏览器中输入<a href="http://localhost:8000/app/index.html" target="_blank">http://localhost:8000/app/index.html</a>来访问我们的<strong>phonecat</strong>应用。</p></li>
</ol>

<p>现在，在浏览器中您应该已经看到了我们的初始应用，很简单，但说明我们的项目已经可以运行了。</p>

<p>应用中显示的“Nothing here yet!”是由如下HTML代码构建而成，代码中包含了AngularJS的关键元素，正是我们需要学习的。</p>

<p><strong>app/index.html</strong></p>

<pre class="prettyprint"><code><span class="dec">&lt;!doctype html&gt;</span><span class="pln">
</span><span class="tag">&lt;html</span><span class="pln"> </span><span class="atn">lang</span><span class="pun">=</span><span class="atv">"en"</span><span class="pln"> </span><span class="atn">ng-app</span><span class="tag">&gt;</span><span class="pln">
</span><span class="tag">&lt;head&gt;</span><span class="pln">
    </span><span class="tag">&lt;meta</span><span class="pln"> </span><span class="atn">charset</span><span class="pun">=</span><span class="atv">"utf-8"</span><span class="tag">&gt;</span><span class="pln">
    </span><span class="tag">&lt;title&gt;</span><span class="pln">My HTML File</span><span class="tag">&lt;/title&gt;</span><span class="pln">
    </span><span class="tag">&lt;link</span><span class="pln"> </span><span class="atn">rel</span><span class="pun">=</span><span class="atv">"stylesheet"</span><span class="pln"> </span><span class="atn">href</span><span class="pun">=</span><span class="atv">"css/app.css"</span><span class="tag">&gt;</span><span class="pln">
    </span><span class="tag">&lt;link</span><span class="pln"> </span><span class="atn">rel</span><span class="pun">=</span><span class="atv">"stylesheet"</span><span class="pln"> </span><span class="atn">href</span><span class="pun">=</span><span class="atv">"css/bootstrap.css"</span><span class="tag">&gt;</span><span class="pln">
    </span><span class="tag">&lt;script</span><span class="pln"> </span><span class="atn">src</span><span class="pun">=</span><span class="atv">"lib/angular/angular.js"</span><span class="tag">&gt;&lt;/script&gt;</span><span class="pln">
</span><span class="tag">&lt;/head&gt;</span><span class="pln">
</span><span class="tag">&lt;body&gt;</span><span class="pln">
</span><span class="tag">&lt;p&gt;</span><span class="pln">Nothing here {{'yet' + '!'}}</span><span class="tag">&lt;/p&gt;</span><span class="pln">
</span><span class="tag">&lt;/body&gt;</span><span class="pln">
</span><span class="tag">&lt;/html&gt;</span></code></pre>

<h2>代码在做什么呢？</h2>

<h3><code>ng-app</code>指令：</h3>

<pre class="prettyprint"><code><span class="tag">&lt;html</span><span class="pln"> </span><span class="atn">lang</span><span class="pun">=</span><span class="atv">"en"</span><span class="pln"> </span><span class="atn">ng-app</span><span class="tag">&gt;</span></code></pre>

<p><code>ng-app</code>指令标记了AngularJS脚本的作用域，在<code>&lt;html&gt;</code>中添加<code>ng-app</code>属性即说明整个<code>&lt;html&gt;</code>都是AngularJS脚本作用域。开发者也可以在局部使用<code>ng-app</code>指令，如<code>&lt;div ng-app&gt;</code>，则AngularJS脚本仅在该<code>&lt;div&gt;</code>中运行。</p>

<h3>AngularJS脚本标签：</h3>

<pre class="prettyprint"><code><span class="tag">&lt;script</span><span class="pln"> </span><span class="atn">src</span><span class="pun">=</span><span class="atv">"lib/angular/angular.js"</span><span class="tag">&gt;&lt;/script&gt;</span></code></pre>

<p>这行代码载入angular.js脚本，当浏览器将整个HTML页面载入完毕后将会执行该angular.js脚本，angular.js脚本运行后将会寻找含有<code>ng-app</code>指令的HTML标签，该标签即定义了AngularJS应用的作用域。</p>

<h3>双大括号绑定的表达式：</h3>

<pre class="prettyprint"><code><span class="tag">&lt;p&gt;</span><span class="pln">Nothing here {{'yet' + '!'}}</span><span class="tag">&lt;/p&gt;</span></code></pre>

<p>这行代码演示了AngularJS模板的核心功能——绑定，这个绑定由双大括号<code>{{}}</code>和表达式<code>'yet' + '!'</code>组成。</p>

<p>这个绑定告诉AngularJS需要运算其中的表达式并将结果插入DOM中，接下来的步骤我们将看到，DOM可以随着表达式运算结果的改变而实时更新。</p>

<p>AngularJS表达式<a href="http://angularjs.cn/A00s" target="_blank">Angular expression</a>是一种类似于JavaScript的代码片段，AngularJS表达式仅在AngularJS的作用域中运行，而不是在整个DOM中运行。</p>

<h2>引导AngularJS应用</h2>

<p>通过ngApp指令来自动引导AngularJS应用是一种简洁的方式，适合大多数情况。在高级开发中，例如使用脚本装载应用，您也可以使用<a href="http://angularjs.cn/A00o" target="_blank">bootstrap</a>手动引导AngularJS应用。</p>

<p><strong>AngularJS应用引导过程有3个重要点：</strong></p>

<ol>
<li><a href="http://docs.angularjs.org/api/AUTO.%24injector" target="_blank">注入器(injector)</a>将用于创建此应用程序的依赖注入(dependency injection)；</li>
<li>注入器将会创建根作用域作为我们应用模型的范围；</li>
<li>AngularJS将会链接根作用域中的DOM，从用ngApp标记的HTML标签开始，逐步处理DOM中指令和绑定。</li>
</ol>

<p>一旦AngularJS应用引导完毕，它将继续侦听浏览器的HTML触发事件，如鼠标点击事件、按键事件、HTTP传入响应等改变DOM模型的事件。这类事件一旦发生，AngularJS将会自动检测变化，并作出相应的处理及更新。</p>

<p>上面这个应用的结构非常简单。该模板包仅含一个指令和一个静态绑定，其中的模型也是空的。下一步我们尝试稍复杂的应用！</p>

<p><img src="http://docs.angularjs.org/img/tutorial/tutorial_00.png" alt="img_tutorial_00"></p>

<h2>我工作目录中这些文件是干什么的？</h2>

<p>上面的应用来自于AngularJS种子项目，我们通常可以使用<a href="https://github.com/angular/angular-seed">AngularJS种子项目</a>来创建新项目。种子项目包括最新的AngularJS代码库、测试库、脚本和一个简单的应用程序示例，它包含了开发一个典型的web应用程序所需的基本配置。</p>

<p>对于本教程，我们对AngularJS种子项目进行了下列更改：</p>

<ol>
<li>删除示例应用程序；</li>
<li>添加手机图像到app/img/phones/；</li>
<li>添加手机数据文件(JSON)到app/phones/；</li>
<li>添加<a href="http://twitter.github.com/bootstrap/" target="_blank">Twitter Bootstrap</a>文件到app/css/ 和app/img/。</li>
</ol>

<h2>练习</h2>

<p>试试把关于数学运算的新表达式添加到index.html：</p>

<pre class="prettyprint"><code><span class="tag">&lt;p&gt;</span><span class="pln">1 + 2 = {{ 1 + 2 }}</span><span class="tag">&lt;/p&gt;</span></code></pre>

<h2>总结</h2>

<p>现在让我们转到<a href="http://www.ituring.com.cn/article/13475">步骤1</a>，将一些内容添加到web应用程序。</p>
                </div>


**来源：**  [http://www.ituring.com.cn/article/13474](http://www.ituring.com.cn/article/13474)