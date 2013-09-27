# AngularJS入门教程02：AngularJS 模板 #

<div class="post-text">
<p>是时候给这些网页来点动态特性了——用AngularJS！我们这里为后面要加入的控制器添加了一个测试。</p>

<p>一个应用的代码架构有很多种。对于AngularJS应用，我们鼓励使用<a href="http://en.wikipedia.org/wiki/Model%E2%80%93View%E2%80%93Controller" target="_blank">模型-视图-控制器（MVC）模式</a>解耦代码和分离关注点。考虑到这一点，我们用AngularJS来为我们的应用添加一些模型、视图和控制器。</p>

<p>请重置工作目录：</p>

<pre class="prettyprint"><code><span class="pln">git checkout </span><span class="pun">-</span><span class="pln">f step</span><span class="pun">-</span><span class="lit">2</span></code></pre>

<p>我们的应用现在有了一个包含三部手机的列表。</p>

<p>步骤1和步骤2之间最重要的不同在下面列出。，你可以到<a href="https://github.com/angular/angular-phonecat/compare/step-1...step-2">GitHub</a>去看完整的差别。</p>

<h3>视图和模板</h3>

<p>在AngularJS中，一个<strong>视图</strong>是模型通过HTML**模板**渲染之后的映射。这意味着，不论模型什么时候发生变化，AngularJS会实时更新结合点，随之更新视图。</p>

<p>比如，视图组件被AngularJS用下面这个模板构建出来：</p>

<pre class="prettyprint"><code><span class="tag">&lt;html</span><span class="pln"> </span><span class="atn">ng-app</span><span class="tag">&gt;</span><span class="pln">
</span><span class="tag">&lt;head&gt;</span><span class="pln">
  ...
  </span><span class="tag">&lt;script</span><span class="pln"> </span><span class="atn">src</span><span class="pun">=</span><span class="atv">"lib/angular/angular.js"</span><span class="tag">&gt;&lt;/script&gt;</span><span class="pln">
  </span><span class="tag">&lt;script</span><span class="pln"> </span><span class="atn">src</span><span class="pun">=</span><span class="atv">"js/controllers.js"</span><span class="tag">&gt;&lt;/script&gt;</span><span class="pln">
</span><span class="tag">&lt;/head&gt;</span><span class="pln">
</span><span class="tag">&lt;body</span><span class="pln"> </span><span class="atn">ng-controller</span><span class="pun">=</span><span class="atv">"PhoneListCtrl"</span><span class="tag">&gt;</span><span class="pln">

  </span><span class="tag">&lt;ul&gt;</span><span class="pln">
    </span><span class="tag">&lt;li</span><span class="pln"> </span><span class="atn">ng-repeat</span><span class="pun">=</span><span class="atv">"phone in phones"</span><span class="tag">&gt;</span><span class="pln">
      {{phone.name}}
    </span><span class="tag">&lt;p&gt;</span><span class="pln">{{phone.snippet}}</span><span class="tag">&lt;/p&gt;</span><span class="pln">
    </span><span class="tag">&lt;/li&gt;</span><span class="pln">
  </span><span class="tag">&lt;/ul&gt;</span><span class="pln">
</span><span class="tag">&lt;/body&gt;</span><span class="pln">
</span><span class="tag">&lt;/html&gt;</span></code></pre>

<p>我们刚刚把静态编码的手机列表替换掉了，因为这里我们使用<a href="http://docs.angularjs.org/api/ng.directive%3angRepeat" target="_blank">ngRepeat指令</a>和两个用花括号包裹起来的AngularJS表达式——<code>{{phone.name}}</code>和<code>{{phone.snippet}}</code>——能达到同样的效果。</p>

<ul>
<li>在<code>&lt;li&gt;</code>标签里面的<code>ng-repeat="phone in phones"</code>语句是一个AngularJS迭代器。这个迭代器告诉AngularJS用第一个<code>&lt;li&gt;</code>标签作为模板为列表中的每一部手机创建一个<code>&lt;li&gt;</code>元素。</li>
<li>正如我们在第0步时学到的，包裹在<code>phone.name</code>和<code>phone.snippet</code>周围的花括号标识着数据绑定。和常量计算不同的是，这里的表达式实际上是我们应用的一个数据模型引用，这些我们在<code>PhoneListCtrl</code>控制器里面都设置好了。</li>
</ul>

<p><img src="http://docs.angularjs.org/img/tutorial/tutorial_02.png" alt="tutorial_02.png"></p>

<h3>模型和控制器</h3>

<p>在<code>PhoneListCtrl</code><strong>控制器</strong>里面初始化了<strong>数据模型</strong>（这里只不过是一个包含了数组的函数，数组中存储的对象是手机数据列表）：</p>

<p><strong>app/js/controller.js</strong>:</p>

<pre class="prettyprint"><code><span class="kwd">function</span><span class="pln"> </span><span class="typ">PhoneListCtrl</span><span class="pun">(</span><span class="pln">$scope</span><span class="pun">)</span><span class="pln"> </span><span class="pun">{</span><span class="pln">
  $scope</span><span class="pun">.</span><span class="pln">phones </span><span class="pun">=</span><span class="pln"> </span><span class="pun">[</span><span class="pln">
    </span><span class="pun">{</span><span class="str">"name"</span><span class="pun">:</span><span class="pln"> </span><span class="str">"Nexus S"</span><span class="pun">,</span><span class="pln">
     </span><span class="str">"snippet"</span><span class="pun">:</span><span class="pln"> </span><span class="str">"Fast just got faster with Nexus S."</span><span class="pun">},</span><span class="pln">
    </span><span class="pun">{</span><span class="str">"name"</span><span class="pun">:</span><span class="pln"> </span><span class="str">"Motorola XOOM™ with Wi-Fi"</span><span class="pun">,</span><span class="pln">
     </span><span class="str">"snippet"</span><span class="pun">:</span><span class="pln"> </span><span class="str">"The Next, Next Generation tablet."</span><span class="pun">},</span><span class="pln">
    </span><span class="pun">{</span><span class="str">"name"</span><span class="pun">:</span><span class="pln"> </span><span class="str">"MOTOROLA XOOM™"</span><span class="pun">,</span><span class="pln">
     </span><span class="str">"snippet"</span><span class="pun">:</span><span class="pln"> </span><span class="str">"The Next, Next Generation tablet."</span><span class="pun">}</span><span class="pln">
  </span><span class="pun">];</span><span class="pln">
</span><span class="pun">}</span></code></pre>

<p>尽管控制器看起来并没有起到什么控制的作用，但是它在这里起到了至关重要的作用。通过给定我们数据模型的语境，控制器允许我们建立模型和视图之间的数据绑定。我们是这样把表现层，数据和逻辑部件联系在一起的：</p>

<ul>
<li><code>PhoneListCtrl</code>——控制器方法的名字（在JS文件<code>controllers.js</code>中）和<code>&lt;body&gt;</code>标签里面的<a href="http://docs.angularjs.org/api/ng.directive%3angController" target="_blank">ngController</a>指令的值相匹配。</li>
<li>手机的数据此时与注入到我们控制器函数的<strong>作用域</strong>（<code>$scope</code>）相关联。当应用启动之后，会有一个根作用域被创建出来，而控制器的作用域是根作用域的一个典型后继。这个控制器的作用域对所有<code>&lt;body ng-controller="PhoneListCtrl"&gt;</code>标记内部的数据绑定有效。</li>
</ul>

<p>AngularJS的作用域理论非常重要：一个作用域可以视作模板、模型和控制器协同工作的粘接器。AngularJS使用作用域，同时还有模板中的信息，数据模型和控制器。这些可以帮助模型和视图分离，但是他们两者确实是同步的！任何对于模型的更改都会即时反映在视图上；任何在视图上的更改都会被立刻体现在模型中。</p>

<p>想要更加深入理解AngularJS的作用域，请参看<a href="http://docs.angularjs.org/api/ng.%24rootScope.Scope" target="_blank">AngularJS作用域文档</a>。</p>

<h3>测试</h3>

<p>“AngularJS方式”让开发时代码测试变得十分简单。让我们来瞅一眼下面这个为控制器新添加的单元测试：</p>

<p><strong>test/unit/controllersSpec.js:</strong></p>

<pre class="prettyprint"><code><span class="pln">describe</span><span class="pun">(</span><span class="str">'PhoneCat controllers'</span><span class="pun">,</span><span class="pln"> </span><span class="kwd">function</span><span class="pun">()</span><span class="pln"> </span><span class="pun">{</span><span class="pln">

  describe</span><span class="pun">(</span><span class="str">'PhoneListCtrl'</span><span class="pun">,</span><span class="pln"> </span><span class="kwd">function</span><span class="pun">(){</span><span class="pln">

    it</span><span class="pun">(</span><span class="str">'should create "phones" model with 3 phones'</span><span class="pun">,</span><span class="pln"> </span><span class="kwd">function</span><span class="pun">()</span><span class="pln"> </span><span class="pun">{</span><span class="pln">
      </span><span class="kwd">var</span><span class="pln"> scope </span><span class="pun">=</span><span class="pln"> </span><span class="pun">{},</span><span class="pln">
      ctrl </span><span class="pun">=</span><span class="pln"> </span><span class="kwd">new</span><span class="pln"> </span><span class="typ">PhoneListCtrl</span><span class="pun">(</span><span class="pln">scope</span><span class="pun">);</span><span class="pln">

      expect</span><span class="pun">(</span><span class="pln">scope</span><span class="pun">.</span><span class="pln">phones</span><span class="pun">.</span><span class="pln">length</span><span class="pun">).</span><span class="pln">toBe</span><span class="pun">(</span><span class="lit">3</span><span class="pun">);</span><span class="pln">
    </span><span class="pun">});</span><span class="pln">
  </span><span class="pun">});</span><span class="pln">
</span><span class="pun">});</span></code></pre>

<p>这个测试验证了我们的手机数组里面有三条记录（暂时无需弄明白这个测试脚本）。这个例子显示出为AngularJS的代码创建一个单元测试是多么的容易。正因为测试在软件开发中是必不可少的环节，所以我们使得在AngularJS可以轻易地构建测试，来鼓励开发者多写它们。</p>

<p>在写测试的时候，AngularJS的开发者倾向于使用Jasmine行为驱动开发（BBD）框架中的语法。尽管AngularJS没有强迫你使用Jasmine，但是我们在教程里面所有的测试都使用Jasmine编写。你可以在Jasmine的<a href="http://pivotal.github.com/jasmine/" target="_blank">官方主页</a>或者<a href="https://github.com/pivotal/jasmine/wiki">Jasmine Wiki</a>上获得相关知识。</p>

<p>基于AngularJS的项目被预先配置为使用<a href="http://code.google.com/p/js-test-driver/" target="_blank">JsTestDriver</a>来运行单元测试。你可以像下面这样运行测试：</p>

<ol>
<li>在一个单独的终端上，进入到<code>angular-phonecat</code>目录并且运行<code>./scripts/test-server.sh</code>来启动测试（Windows命令行下请输入<code>.\scripts\test-server.bat</code>来运行脚本，后面脚本命令运行方式类似）；</li>
<li>打开一个新的浏览器窗口，并且转到<a href="http://localhost:9876" target="_blank">http://localhost:9876</a> ；</li>
<li><p>选择“Capture this browser in strict mode”。</p>

<p>这个时候，你可以抛开你的窗口不管然后把这事忘了。JsTestDriver会自己把测试跑完并且把结果输出在你的终端里。</p></li>
<li><p>运行<code>./scripts/test.sh</code>进行测试 。</p>

<p>你应当看到类似于如下的结果：</p>

<pre class="prettyprint"><code><span class="typ">Chrome</span><span class="pun">:</span><span class="pln"> </span><span class="typ">Runner</span><span class="pln"> reset</span><span class="pun">.</span><span class="pln">
</span><span class="pun">.</span><span class="pln">
</span><span class="typ">Total</span><span class="pln"> </span><span class="lit">1</span><span class="pln"> tests </span><span class="pun">(</span><span class="typ">Passed</span><span class="pun">:</span><span class="pln"> </span><span class="lit">1</span><span class="pun">;</span><span class="pln"> </span><span class="typ">Fails</span><span class="pun">:</span><span class="pln"> </span><span class="lit">0</span><span class="pun">;</span><span class="pln"> </span><span class="typ">Errors</span><span class="pun">:</span><span class="pln"> </span><span class="lit">0</span><span class="pun">)</span><span class="pln"> </span><span class="pun">(</span><span class="lit">2.00</span><span class="pln"> ms</span><span class="pun">)</span><span class="pln">
  </span><span class="typ">Chrome</span><span class="pln"> </span><span class="lit">19.0</span><span class="pun">.</span><span class="lit">1084.36</span><span class="pln"> </span><span class="typ">Mac</span><span class="pln"> OS</span><span class="pun">:</span><span class="pln"> </span><span class="typ">Run</span><span class="pln"> </span><span class="lit">1</span><span class="pln"> tests </span><span class="pun">(</span><span class="typ">Passed</span><span class="pun">:</span><span class="pln"> </span><span class="lit">1</span><span class="pun">;</span><span class="pln"> </span><span class="typ">Fails</span><span class="pun">:</span><span class="pln"> </span><span class="lit">0</span><span class="pun">;</span><span class="pln"> </span><span class="typ">Errors</span><span class="pln"> </span><span class="lit">0</span><span class="pun">)</span><span class="pln"> </span><span class="pun">(</span><span class="lit">2.00</span><span class="pln"> ms</span><span class="pun">)</span></code></pre>

<p>耶！测试通过了！或者没有...
注意：如果在你运行测试之后发生了错误，关闭浏览器然后回到终端关了脚本，然后在重新来一边上面的步骤。</p></li>
</ol>

<h2>练习</h2>

<ul>
<li><p>为<code>index.html</code>添加另一个数据绑定。例如：</p>

<pre class="prettyprint"><code><span class="tag">&lt;p&gt;</span><span class="pln">Total number of phones: {{phones.length}}</span><span class="tag">&lt;/p&gt;</span></code></pre></li>
<li><p>创建一个新的数据模型属性，并且把它绑定到模板上。例如：</p>

<pre class="prettyprint"><code><span class="pln">$scope</span><span class="pun">.</span><span class="pln">hello </span><span class="pun">=</span><span class="pln"> </span><span class="str">"Hello, World!"</span></code></pre>

<p>更新你的浏览器，确保显示出来“Hello, World!”</p></li>
<li><p>用一个迭代器创建一个简单的表：</p>

<pre class="prettyprint"><code><span class="tag">&lt;table&gt;</span><span class="pln">
    </span><span class="tag">&lt;tr&gt;&lt;th&gt;</span><span class="pln">row number</span><span class="tag">&lt;/th&gt;&lt;/tr&gt;</span><span class="pln">
    </span><span class="tag">&lt;tr</span><span class="pln"> </span><span class="atn">ng-repeat</span><span class="pun">=</span><span class="atv">"i in [0, 1, 2, 3, 4, 5, 6, 7]"</span><span class="tag">&gt;&lt;td&gt;</span><span class="pln">{{i}}</span><span class="tag">&lt;/td&gt;&lt;/tr&gt;</span><span class="pln">
</span><span class="tag">&lt;/table&gt;</span></code></pre>

<p>现在让数据模型表达式的<code>i</code>增加1：</p>

<pre class="prettyprint"><code><span class="tag">&lt;table&gt;</span><span class="pln">
    </span><span class="tag">&lt;tr&gt;&lt;th&gt;</span><span class="pln">row number</span><span class="tag">&lt;/th&gt;&lt;/tr&gt;</span><span class="pln">
    </span><span class="tag">&lt;tr</span><span class="pln"> </span><span class="atn">ng-repeat</span><span class="pun">=</span><span class="atv">"i in [0, 1, 2, 3, 4, 5, 6, 7]"</span><span class="tag">&gt;&lt;td&gt;</span><span class="pln">{{i+1}}</span><span class="tag">&lt;/td&gt;&lt;/tr&gt;</span><span class="pln">
</span><span class="tag">&lt;/table&gt;</span></code></pre></li>
<li><p>确定把<code>toBe(3)</code>改成<code>toBe(4)</code>之后单元测试失败，然后重新跑一遍<code>./scripts/test.sh</code>脚本</p></li>
</ul>

<h2>总结</h2>

<p>你现在拥有一个模型，视图，控制器分离的动态应用了，并且你随时进行了测试。现在，你可以进入到<a href="http://www.ituring.com.cn/article/15763">步骤3</a>来为应用加入全文检索功能了。</p>
                </div>


**来源：** [http://www.ituring.com.cn/article/15762](http://www.ituring.com.cn/article/15762)