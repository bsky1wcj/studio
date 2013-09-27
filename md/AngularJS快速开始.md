# AngularJS快速开始 #

<div class="post-text">
<h2>Hello World!</h2>

<p>开始学习AngularJS的一个好方法是创建经典应用程序“Hello World!”：</p>

<ol>
<li>使用您喜爱的文本编辑器，创建一个HTML文件，例如：helloworld.html。</li>
<li>将下面的源代码复制到您的HTML文件。</li>
<li>在web浏览器中打开这个HTML文件。</li>
</ol>

<p><strong>源代码</strong></p>

<pre class="prettyprint"><code><span class="dec">&lt;!doctype html&gt;</span><span class="pln">
</span><span class="tag">&lt;html</span><span class="pln"> </span><span class="atn">ng-app</span><span class="tag">&gt;</span><span class="pln">
    </span><span class="tag">&lt;head&gt;</span><span class="pln">
        </span><span class="tag">&lt;script</span><span class="pln"> </span><span class="atn">src</span><span class="pun">=</span><span class="atv">"http://code.angularjs.org/angular-1.0.1.min.js"</span><span class="tag">&gt;&lt;/script&gt;</span><span class="pln">
    </span><span class="tag">&lt;/head&gt;</span><span class="pln">
    </span><span class="tag">&lt;body&gt;</span><span class="pln">
        Hello {{'World'}}!
    </span><span class="tag">&lt;/body&gt;</span><span class="pln">
</span><span class="tag">&lt;/html&gt;</span></code></pre>

<p>请在您的浏览器中运行以上代码查看效果。</p>

<p>现在让我们仔细看看代码，看看到底怎么回事。 当加载该页时，标记<code>ng-app</code>告诉AngularJS处理整个HTML页并引导应用：</p>

<pre class="prettyprint"><code><span class="tag">&lt;html</span><span class="pln"> </span><span class="atn">ng-app</span><span class="tag">&gt;</span></code></pre>

<p>这行载入AngularJS脚本：</p>

<pre class="prettyprint"><code><span class="tag">&lt;script</span><span class="pln"> </span><span class="atn">src</span><span class="pun">=</span><span class="atv">"http://code.angularjs.org/angular-1.0.1.min.js"</span><span class="tag">&gt;&lt;/script&gt;</span></code></pre>

<p>（想了解AngularJS处理整个HTML页的细节，请看Bootstrap。）</p>

<p>最后，标签中的正文是应用的模板，在UI中显示我们的问候语：</p>

<pre class="prettyprint"><code><span class="typ">Hello</span><span class="pln"> </span><span class="pun">{{</span><span class="str">'World'</span><span class="pun">}}!</span></code></pre>

<p>注意，使用双大括号标记<code>{{}}</code>的内容是问候语中绑定的表达式，这个表达式是一个简单的字符串‘World’。</p>

<p>下面，让我们看一个更有趣的例子：使用AngularJS对我们的问候语文本绑定一个动态表达式。</p>

<h2>Hello AngularJS World!</h2>

<p>本示例演示AngularJS的双向数据绑定（bi-directional data binding）：</p>

<ol>
<li>编辑前面创建的helloworld.html文档。</li>
<li>将下面的源代码复制到您的HTML文件。</li>
<li>刷新浏览器窗口。</li>
</ol>

<p><strong>源代码</strong></p>

<pre class="prettyprint"><code><span class="dec">&lt;!doctype html&gt;</span><span class="pln">
</span><span class="tag">&lt;html</span><span class="pln"> </span><span class="atn">ng-app</span><span class="tag">&gt;</span><span class="pln">
    </span><span class="tag">&lt;head&gt;</span><span class="pln">
        </span><span class="tag">&lt;script</span><span class="pln"> </span><span class="atn">src</span><span class="pun">=</span><span class="atv">"http://code.angularjs.org/angular-1.0.1.min.js"</span><span class="tag">&gt;&lt;/script&gt;</span><span class="pln">
    </span><span class="tag">&lt;/head&gt;</span><span class="pln">
    </span><span class="tag">&lt;body&gt;</span><span class="pln">
        Your name: </span><span class="tag">&lt;input</span><span class="pln"> </span><span class="atn">type</span><span class="pun">=</span><span class="atv">"text"</span><span class="pln"> </span><span class="atn">ng-model</span><span class="pun">=</span><span class="atv">"yourname"</span><span class="pln"> </span><span class="atn">placeholder</span><span class="pun">=</span><span class="atv">"World"</span><span class="tag">&gt;</span><span class="pln">
        </span><span class="tag">&lt;hr&gt;</span><span class="pln">
        Hello {{yourname || 'World'}}!
    </span><span class="tag">&lt;/body&gt;</span><span class="pln">
</span><span class="tag">&lt;/html&gt;</span></code></pre>

<p>请在您的浏览器中运行以上代码查看效果。</p>

<p>该示例有一下几点重要的注意事项：</p>

<ul>
<li>文本输入指令<code>&lt;input ng-model="yourname" /&gt;</code>绑定到一个叫<code>yourname</code>的模型变量。</li>
<li>双大括号标记将<code>yourname</code>模型变量添加到问候语文本。</li>
<li>你不需要为该应用另外注册一个事件侦听器或添加事件处理程序！</li>
</ul>

<p>现在试着在输入框中键入您的名称，您键入的名称将立即更新显示在问候语中。 这就是AngularJS双向数据绑定的概念。 输入框的任何更改会立即反映到模型变量（一个方向），模型变量的任何更改都会立即反映到问候语文本中（另一方向）。</p>

<h2>AngularJS应用的解析</h2>

<p>本节描述AngularJS应用程序的三个组成部分，并解释它们如何映射到模型-视图-控制器设计模式：</p>

<h3>模板（Templates）</h3>

<p>模板是您用HTML和CSS编写的文件，展现应用的视图。 您可给HTML添加新的元素、属性标记，作为AngularJS编译器的指令。 AngularJS编译器是完全可扩展的，这意味着通过AngularJS您可以在HTML中构建您自己的HTML标记！</p>

<h3>应用程序逻辑（Logic）和行为（Behavior）</h3>

<p>应用程序逻辑和行为是您用JavaScript定义的控制器。AngularJS与标准AJAX应用程序不同，您不需要另外编写侦听器或DOM控制器，因为它们已经内置到AngularJS中了。这些功能使您的应用程序逻辑很容易编写、测试、维护和理解。</p>

<h3>模型数据（Data）</h3>

<p>模型是从AngularJS作用域对象的属性引申的。模型中的数据可能是Javascript对象、数组或基本类型，这都不重要，重要的是，他们都属于AngularJS作用域对象。</p>

<p>AngularJS通过作用域来保持数据模型与视图界面UI的双向同步。一旦模型状态发生改变，AngularJS会立即刷新反映在视图界面中，反之亦然。</p>

<h3>此外，AngularJS还提供了一些非常有用的服务特性：</h3>

<ol>
<li>底层服务包括依赖注入，XHR、缓存、URL路由和浏览器抽象服务。</li>
<li>您还可以扩展和添加自己特定的应用服务。</li>
<li>这些服务可以让您非常方便的编写WEB应用。</li>
</ol>
                </div>


**来源：**[http://www.ituring.com.cn/article/13472](http://www.ituring.com.cn/article/13472)