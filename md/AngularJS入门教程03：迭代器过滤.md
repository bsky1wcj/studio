# AngularJS入门教程03：迭代器过滤 #


<div class="post-text">
<p>我们在上一步做了很多基础性的训练，所以现在我们可以来做一些简单的事情喽。我们要加入全文检索功能（没错，这个真的非常简单！）。同时，我们也会写一个端到端测试，因为一个好的端到端测试可以帮上很大忙。它监视着你的应用，并且在发生回归的时候迅速报告。</p>

<p>请重置工作目录：</p>

<pre class="prettyprint"><code><span class="pln">git checkout </span><span class="pun">-</span><span class="pln">f step</span><span class="pun">-</span><span class="lit">3</span></code></pre>

<p>我们的应用现在有了一个搜索框。注意到页面上的手机列表随着用户在搜索框中的输入而变化。</p>

<p>步骤2和步骤3之间最重要的不同在下面列出。你可以在<a href="https://github.com/angular/angular-phonecat/compare/step-2...step-3">GitHub</a>里看到完整的差别。</p>

<h3>控制器</h3>

<p>我们对控制器不做任何修改。</p>

<h3>模板</h3>

<p><strong>app/index.html</strong></p>

<pre class="prettyprint"><code><span class="tag">&lt;div</span><span class="pln"> </span><span class="atn">class</span><span class="pun">=</span><span class="atv">"container-fluid"</span><span class="tag">&gt;</span><span class="pln">
  </span><span class="tag">&lt;div</span><span class="pln"> </span><span class="atn">class</span><span class="pun">=</span><span class="atv">"row-fluid"</span><span class="tag">&gt;</span><span class="pln">
    </span><span class="tag">&lt;div</span><span class="pln"> </span><span class="atn">class</span><span class="pun">=</span><span class="atv">"span2"</span><span class="tag">&gt;</span><span class="pln">
      </span><span class="com">&lt;!--Sidebar content--&gt;</span><span class="pln">

      Search: </span><span class="tag">&lt;input</span><span class="pln"> </span><span class="atn">ng-model</span><span class="pun">=</span><span class="atv">"query"</span><span class="tag">&gt;</span><span class="pln">

    </span><span class="tag">&lt;/div&gt;</span><span class="pln">
    </span><span class="tag">&lt;div</span><span class="pln"> </span><span class="atn">class</span><span class="pun">=</span><span class="atv">"span10"</span><span class="tag">&gt;</span><span class="pln">
      </span><span class="com">&lt;!--Body content--&gt;</span><span class="pln">

      </span><span class="tag">&lt;ul</span><span class="pln"> </span><span class="atn">class</span><span class="pun">=</span><span class="atv">"phones"</span><span class="tag">&gt;</span><span class="pln">
        </span><span class="tag">&lt;li</span><span class="pln"> </span><span class="atn">ng-repeat</span><span class="pun">=</span><span class="atv">"phone in phones | filter:query"</span><span class="tag">&gt;</span><span class="pln">
          {{phone.name}}
        </span><span class="tag">&lt;p&gt;</span><span class="pln">{{phone.snippet}}</span><span class="tag">&lt;/p&gt;</span><span class="pln">
        </span><span class="tag">&lt;/li&gt;</span><span class="pln">
      </span><span class="tag">&lt;/ul&gt;</span><span class="pln">

    </span><span class="tag">&lt;/div&gt;</span><span class="pln">
  </span><span class="tag">&lt;/div&gt;</span><span class="pln">
</span><span class="tag">&lt;/div&gt;</span></code></pre>

<p>我们现在添加了一个<code>&lt;input&gt;</code>标签，并且使用AngularJS的<a href="http://docs.angularjs.org/api/ng.filter%3afilter" target="_blank">$filter</a>函数来处理<a href="http://docs.angularjs.org/api/ng.directive%3angRepeat" target="_blank">ngRepeat</a>指令的输入。</p>

<p>这样允许用户输入一个搜索条件，立刻就能看到对电话列表的搜索结果。我们来解释一下新的代码：</p>

<ul>
<li><p>数据绑定： 这是AngularJS的一个核心特性。当页面加载的时候，AngularJS会根据输入框的属性值名字，将其与数据模型中相同名字的变量绑定在一起，以确保两者的同步性。</p>

<p>在这段代码中，用户在输入框中输入的数据名字称作<code>query</code>，会立刻作为列表迭代器（<code>phone in phones | filter:</code>query`）其过滤器的输入。当数据模型引起迭代器输入变化的时候，迭代器可以高效得更新DOM将数据模型最新的状态反映出来。</p></li>
</ul>

<p><img src="http://docs.angularjs.org/img/tutorial/tutorial_03.png" alt="img_tutorial_03"></p>

<ul>
<li><p>使用<code>filter</code>过滤器：<a href="http://docs.angularjs.org/api/ng.filter%3afilter" target="_blank">filter</a>函数使用<code>query</code>的值来创建一个只包含匹配<code>query</code>记录的新数组。</p>

<p><code>ngRepeat</code>会根据<code>filter</code>过滤器生成的手机记录数据数组来自动更新视图。整个过程对于开发者来说都是透明的。</p></li>
</ul>

<h3>测试</h3>

<p>在步骤2，我们学习了编写和运行一个测试的方法。单元测试用来测试我们用js编写的控制器和其他组件都非常方便，但是不能方便的对DOM操作和应用集成进行测试。对于这些来说，端到端测试是一个更好的选择。</p>

<p>搜索特性是完全通过模板和数据绑定实现的，所以我们的第一个端到端测试就来验证这些特性是否符合我们的预期。</p>

<p><strong>test/e2e/scenarios.js：</strong></p>

<pre class="prettyprint"><code><span class="pln">describe</span><span class="pun">(</span><span class="str">'PhoneCat App'</span><span class="pun">,</span><span class="pln"> </span><span class="kwd">function</span><span class="pun">()</span><span class="pln"> </span><span class="pun">{</span><span class="pln">

  describe</span><span class="pun">(</span><span class="str">'Phone list view'</span><span class="pun">,</span><span class="pln"> </span><span class="kwd">function</span><span class="pun">()</span><span class="pln"> </span><span class="pun">{</span><span class="pln">

    beforeEach</span><span class="pun">(</span><span class="kwd">function</span><span class="pun">()</span><span class="pln"> </span><span class="pun">{</span><span class="pln">
      browser</span><span class="pun">().</span><span class="pln">navigateTo</span><span class="pun">(</span><span class="str">'../../app/index.html'</span><span class="pun">);</span><span class="pln">
    </span><span class="pun">});</span><span class="pln">


    it</span><span class="pun">(</span><span class="str">'should filter the phone list as user types into the search box'</span><span class="pun">,</span><span class="pln"> </span><span class="kwd">function</span><span class="pun">()</span><span class="pln"> </span><span class="pun">{</span><span class="pln">
      expect</span><span class="pun">(</span><span class="pln">repeater</span><span class="pun">(</span><span class="str">'.phones li'</span><span class="pun">).</span><span class="pln">count</span><span class="pun">()).</span><span class="pln">toBe</span><span class="pun">(</span><span class="lit">3</span><span class="pun">);</span><span class="pln">

      input</span><span class="pun">(</span><span class="str">'query'</span><span class="pun">).</span><span class="pln">enter</span><span class="pun">(</span><span class="str">'nexus'</span><span class="pun">);</span><span class="pln">
      expect</span><span class="pun">(</span><span class="pln">repeater</span><span class="pun">(</span><span class="str">'.phones li'</span><span class="pun">).</span><span class="pln">count</span><span class="pun">()).</span><span class="pln">toBe</span><span class="pun">(</span><span class="lit">1</span><span class="pun">);</span><span class="pln">

      input</span><span class="pun">(</span><span class="str">'query'</span><span class="pun">).</span><span class="pln">enter</span><span class="pun">(</span><span class="str">'motorola'</span><span class="pun">);</span><span class="pln">
      expect</span><span class="pun">(</span><span class="pln">repeater</span><span class="pun">(</span><span class="str">'.phones li'</span><span class="pun">).</span><span class="pln">count</span><span class="pun">()).</span><span class="pln">toBe</span><span class="pun">(</span><span class="lit">2</span><span class="pun">);</span><span class="pln">
    </span><span class="pun">});</span><span class="pln">
  </span><span class="pun">});</span><span class="pln">
</span><span class="pun">});</span></code></pre>

<p>尽管这段测试代码的语法看起来和我们之前用Jasmine写的单元测试非常像，但是端到端测试使用的是<a href="http://code.angularjs.org/1.1.0/docs/guide/dev_guide.e2e-testing" target="_blank">AngularJS端到端测试器</a>提供的接口。</p>

<p>运行一个端到端测试，在浏览器新标签页中打开下面任意一个：</p>

<ul>
<li>node.js用户：<a href="http://localhost:8000/test/e2e/runner.html" target="_blank">http://localhost:8000/test/e2e/runner.html</a></li>
<li>使用其他http服务器的用户：<code>http://localhost:[port-number]/[context-path]/test/e2e/runner.html</code></li>
<li>访客：<a href="http://angular.github.com/angular-phonecat/step-3/test/e2e/runner.html" target="_blank">http://angular.github.com/angular-phonecat/step-3/test/e2e/runner.html</a></li>
</ul>

<p>这个测试验证了搜素框和迭代器被正确地集成起来。你可以发现，在AngularJS里写一个端到端测试多么的简单。尽管这个例子仅仅是一个简单的测试，但是用它来构建任何一个复杂、可读的端到端测试都很容易。</p>

<h2>练习</h2>

<ul>
<li>在<code>index.html</code>模板中添加一个<code>{{query}}</code>绑定来实时显示<code>query</code>模型的当前值，然后观察他们是如何根据输入框中的值而变化。</li>
<li><p>现在我们来看一下我们怎么让<code>query</code>模型的值出现在HTML的页面标题上。</p>

<p>你或许认为像下面这样在<code>title</code>标签上加上一个绑定就行了：</p>

<pre class="prettyprint"><code><span class="tag">&lt;title&gt;</span><span class="pln">Google Phone Gallery: {{query}}</span><span class="tag">&lt;/title&gt;</span></code></pre>

<p>但是，当你重载页面的时候，你根本没办法得到期望的结果。这是因为<code>query</code>模型仅仅在<code>body</code>元素定义的作用域内才有效。</p>

<pre class="prettyprint"><code><span class="tag">&lt;body</span><span class="pln"> </span><span class="atn">ng-controller</span><span class="pun">=</span><span class="atv">"PhoneListCtrl"</span><span class="tag">&gt;</span></code></pre>

<p>如果你想让<code>&lt;title&gt;</code>元素绑定上<code>query</code>模型，你必须把<code>ngController</code>声明<strong>移动</strong>到<code>HTML</code>元素上，因为它是<code>title</code>和<code>body</code>元素的共同祖先。</p>

<pre class="prettyprint"><code><span class="tag">&lt;html</span><span class="pln"> </span><span class="atn">ng-app</span><span class="pln"> </span><span class="atn">ng-controller</span><span class="pun">=</span><span class="atv">"PhoneListCtrl"</span><span class="tag">&gt;</span></code></pre>

<p>一定要注意把<code>body</code>元素上的<code>ng-controller</code>声明给删了。</p>

<p>当绑定两个花括号在<code>title</code>元素上可以实现我们的目标，但是你或许发现了，页面正加载的时候它们已经显示给用户看了。一个更好的解决方案是使用<a href="http://docs.angularjs.org/api/ng.directive%3angBind" target="_blank">ngBind</a>或者<a href="http://docs.angularjs.org/api/ng.directive%3angBindTemplate" target="_blank">ngBindTemplate</a>指令，它们在页面加载时对用户是不可见的：</p>

<pre class="prettyprint"><code><span class="tag">&lt;title</span><span class="pln"> </span><span class="atn">ng-bind-template</span><span class="pun">=</span><span class="atv">"Google Phone Gallery: {{query}}"</span><span class="tag">&gt;</span><span class="pln">Google Phone Gallery</span><span class="tag">&lt;/title&gt;</span></code></pre></li>
<li><p>在<code>test/e2e/scenarios.js</code>的<code>describe</code>块中加入下面这些端到端测试代码：</p>

<pre class="prettyprint"><code><span class="pln">it</span><span class="pun">(</span><span class="str">'should display the current filter value within an element with id "status"'</span><span class="pun">,</span><span class="pln">
    </span><span class="kwd">function</span><span class="pun">()</span><span class="pln"> </span><span class="pun">{</span><span class="pln">
        expect</span><span class="pun">(</span><span class="pln">element</span><span class="pun">(</span><span class="str">'#status'</span><span class="pun">).</span><span class="pln">text</span><span class="pun">()).</span><span class="pln">toMatch</span><span class="pun">(</span><span class="str">/Current filter: \s*$/</span><span class="pun">);</span><span class="pln">
        input</span><span class="pun">(</span><span class="str">'query'</span><span class="pun">).</span><span class="pln">enter</span><span class="pun">(</span><span class="str">'nexus'</span><span class="pun">);</span><span class="pln">
        expect</span><span class="pun">(</span><span class="pln">element</span><span class="pun">(</span><span class="str">'#status'</span><span class="pun">).</span><span class="pln">text</span><span class="pun">()).</span><span class="pln">toMatch</span><span class="pun">(</span><span class="str">/Current filter: nexus\s*$/</span><span class="pun">);</span><span class="pln">
        </span><span class="com">//alternative version of the last assertion that tests just the value of the binding</span><span class="pln">
        </span><span class="kwd">using</span><span class="pun">(</span><span class="str">'#status'</span><span class="pun">).</span><span class="pln">expect</span><span class="pun">(</span><span class="pln">binding</span><span class="pun">(</span><span class="str">'query'</span><span class="pun">)).</span><span class="pln">toBe</span><span class="pun">(</span><span class="str">'nexus'</span><span class="pun">);</span><span class="pln">
</span><span class="pun">});</span></code></pre>

<p>刷新浏览器，端到端测试器会报告测试失败。为了让测试通过，编辑<code>index.html</code>，添加一个<code>id为“status”</code>的<code>div</code>或者<code>p</code>元素，内容是一个<code>query</code>绑定，再加上<code>Current filter:</code>前缀。例如：</p>

<pre class="prettyprint"><code><span class="tag">&lt;div</span><span class="pln"> </span><span class="atn">id</span><span class="pun">=</span><span class="atv">"status"</span><span class="tag">&gt;</span><span class="pln">Current filter: {{query}}</span><span class="tag">&lt;/div&gt;</span></code></pre></li>
<li><p>在端到端测试里面加一条<code>pause();</code>语句，重新跑一遍。你将发现测试器暂停了！这样允许你有机会在测试运行过程中查看你应用的状态。测试应用是实时的！你可以更换搜索内容来证明。稍有经验你就会知道，这对于在端到端测试中迅速找到问题是多么的关键。</p></li>
</ul>

<h2>总结</h2>

<p>我们现在添加了全文搜索功能，并且完成一个测试证明了搜索是对的！现在让我们继续到<a href="http://www.ituring.com.cn/article/15764">步骤4</a>来看看给我们的手机应用增加排序功能。</p>
                </div>


**来源：** [http://www.ituring.com.cn/article/15763](http://www.ituring.com.cn/article/15763)