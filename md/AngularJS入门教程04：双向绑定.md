# AngularJS入门教程04：双向绑定 #

<div class="post-text">
<p>在这一步你会增加一个让用户控制手机列表显示顺序的特性。动态排序可以这样实现，添加一个新的模型属性，把它和迭代器集成起来，然后让数据绑定完成剩下的事情。</p>

<p>请重置工作目录：</p>

<pre class="prettyprint"><code><span class="pln">git checkout </span><span class="pun">-</span><span class="pln">f step</span><span class="pun">-</span><span class="lit">4</span></code></pre>

<p>你应该发现除了搜索框之外，你的应用多了一个下来菜单，它可以允许控制电话排列的顺序。</p>

<p>步骤3和步骤4之间最重要的不同在下面列出。你可以在<a href="https://github.com/angular/angular-phonecat/compare/step-3...step-4">GitHub</a>里看到完整的差别。</p>

<h3>模板</h3>

<p><strong>app/index.html</strong></p>

<pre class="prettyprint"><code><span class="typ">Search</span><span class="pun">:</span><span class="pln"> </span><span class="pun">&lt;</span><span class="pln">input ng</span><span class="pun">-</span><span class="pln">model</span><span class="pun">=</span><span class="str">"query"</span><span class="pun">&gt;</span><span class="pln">
</span><span class="typ">Sort</span><span class="pln"> </span><span class="kwd">by</span><span class="pun">:</span><span class="pln">
</span><span class="pun">&lt;</span><span class="kwd">select</span><span class="pln"> ng</span><span class="pun">-</span><span class="pln">model</span><span class="pun">=</span><span class="str">"orderProp"</span><span class="pun">&gt;</span><span class="pln">
  </span><span class="pun">&lt;</span><span class="pln">option value</span><span class="pun">=</span><span class="str">"name"</span><span class="pun">&gt;</span><span class="typ">Alphabetical</span><span class="pun">&lt;</span><span class="str">/option&gt;
  &lt;option value="age"&gt;Newest&lt;/</span><span class="pln">option</span><span class="pun">&gt;</span><span class="pln">
</span><span class="pun">&lt;</span><span class="str">/select&gt;


&lt;ul class="phones"&gt;
  &lt;li ng-repeat="phone in phones | filter:query | orderBy:orderProp"&gt;
    {{phone.name}}
    &lt;p&gt;{{phone.snippet}}&lt;/</span><span class="pln">p</span><span class="pun">&gt;</span><span class="pln">
  </span><span class="pun">&lt;</span><span class="str">/li&gt;
&lt;/</span><span class="pln">ul</span><span class="pun">&gt;</span></code></pre>

<p>我们在<strong>index.html</strong>中做了如下更改：</p>

<ul>
<li>首先，我们增加了一个叫做<code>orderProp</code>的<code>&lt;select&gt;</code>标签，这样我们的用户就可以选择我们提供的两种排序方法。</li>
</ul>

<p><img src="http://docs.angularjs.org/img/tutorial/tutorial_04.png" alt="img_tutorial_04"></p>

<ul>
<li>然后，在<code>filter</code>过滤器后面添加一个<a href="http://docs.angularjs.org/api/ng.filter%3aorderBy" target="_blank">orderBy</a>过滤器用其来处理进入迭代器的数据。<code>orderBy</code>过滤器以一个数组作为输入，复制一份副本，然后把副本重排序再输出到迭代器。</li>
</ul>

<p>AngularJS在<code>select</code>元素和<code>orderProp</code>模型之间创建了一个双向绑定。而后，<code>orderProp</code>会被用作<code>orderBy</code>过滤器的输入。</p>

<p>正如我们在步骤3中讨论数据绑定和迭代器的时候所说的一样，无论什么时候数据模型发生了改变（比如用户在下拉菜单中选了不同的顺序），AngularJS的数据绑定会让视图自动更新。没有任何笨拙的DOM操作！</p>

<h3>控制器</h3>

<p><strong>app/js/controllers.js：</strong></p>

<pre class="prettyprint"><code><span class="kwd">function</span><span class="pln"> </span><span class="typ">PhoneListCtrl</span><span class="pun">(</span><span class="pln">$scope</span><span class="pun">)</span><span class="pln"> </span><span class="pun">{</span><span class="pln">
  $scope</span><span class="pun">.</span><span class="pln">phones </span><span class="pun">=</span><span class="pln"> </span><span class="pun">[</span><span class="pln">
    </span><span class="pun">{</span><span class="str">"name"</span><span class="pun">:</span><span class="pln"> </span><span class="str">"Nexus S"</span><span class="pun">,</span><span class="pln">
     </span><span class="str">"snippet"</span><span class="pun">:</span><span class="pln"> </span><span class="str">"Fast just got faster with Nexus S."</span><span class="pun">,</span><span class="pln">
     </span><span class="str">"age"</span><span class="pun">:</span><span class="pln"> </span><span class="lit">0</span><span class="pun">},</span><span class="pln">
    </span><span class="pun">{</span><span class="str">"name"</span><span class="pun">:</span><span class="pln"> </span><span class="str">"Motorola XOOM™ with Wi-Fi"</span><span class="pun">,</span><span class="pln">
     </span><span class="str">"snippet"</span><span class="pun">:</span><span class="pln"> </span><span class="str">"The Next, Next Generation tablet."</span><span class="pun">,</span><span class="pln">
     </span><span class="str">"age"</span><span class="pun">:</span><span class="pln"> </span><span class="lit">1</span><span class="pun">},</span><span class="pln">
    </span><span class="pun">{</span><span class="str">"name"</span><span class="pun">:</span><span class="pln"> </span><span class="str">"MOTOROLA XOOM™"</span><span class="pun">,</span><span class="pln">
     </span><span class="str">"snippet"</span><span class="pun">:</span><span class="pln"> </span><span class="str">"The Next, Next Generation tablet."</span><span class="pun">,</span><span class="pln">
     </span><span class="str">"age"</span><span class="pun">:</span><span class="pln"> </span><span class="lit">2</span><span class="pun">}</span><span class="pln">
  </span><span class="pun">];</span><span class="pln">

  $scope</span><span class="pun">.</span><span class="pln">orderProp </span><span class="pun">=</span><span class="pln"> </span><span class="str">'age'</span><span class="pun">;</span><span class="pln">
</span><span class="pun">}</span></code></pre>

<ul>
<li>我们修改了<code>phones</code>模型—— 手机的数组 ——为每一个手机记录其增加了一个<code>age</code>属性。我们会根据<code>age</code>属性来对手机进行排序。</li>
<li><p>我们在控制器代码里加了一行让<code>orderProp</code>的默认值为<code>age</code>。如果我们不设置默认值，这个模型会在我们的用户在下拉菜单选择一个顺序之前一直处于未初始化状态。</p>

<p>现在我们该好好谈谈双向数据绑定了。注意到当应用在浏览器中加载时，“Newest”在下拉菜单中被选中。这是因为我们在控制器中把<code>orderProp</code>设置成了‘age’。所以绑定在从我们模型到用户界面的方向上起作用——即数据从模型到视图的绑定。现在当你在下拉菜单中选择“Alphabetically”，数据模型会被同时更新，并且手机列表数组会被重新排序。这个时候数据绑定从另一个方向产生了作用——即数据从视图到模型的绑定。</p></li>
</ul>

<h3>测试</h3>

<p>我们所做的更改可以通过一个单元测试或者一个端到端测试来验证正确性。我们首先来看看单元测试：</p>

<p><strong>test/unit/controllersSpec.js：</strong></p>

<pre class="prettyprint"><code><span class="pln">describe</span><span class="pun">(</span><span class="str">'PhoneCat controllers'</span><span class="pun">,</span><span class="pln"> </span><span class="kwd">function</span><span class="pun">()</span><span class="pln"> </span><span class="pun">{</span><span class="pln">

  describe</span><span class="pun">(</span><span class="str">'PhoneListCtrl'</span><span class="pun">,</span><span class="pln"> </span><span class="kwd">function</span><span class="pun">(){</span><span class="pln">
    </span><span class="kwd">var</span><span class="pln"> scope</span><span class="pun">,</span><span class="pln"> ctrl</span><span class="pun">;</span><span class="pln">

    beforeEach</span><span class="pun">(</span><span class="kwd">function</span><span class="pun">()</span><span class="pln"> </span><span class="pun">{</span><span class="pln">
      scope </span><span class="pun">=</span><span class="pln"> </span><span class="pun">{},</span><span class="pln">
      ctrl </span><span class="pun">=</span><span class="pln"> </span><span class="kwd">new</span><span class="pln"> </span><span class="typ">PhoneListCtrl</span><span class="pun">(</span><span class="pln">scope</span><span class="pun">);</span><span class="pln">
    </span><span class="pun">});</span><span class="pln">

    it</span><span class="pun">(</span><span class="str">'should create "phones" model with 3 phones'</span><span class="pun">,</span><span class="pln"> </span><span class="kwd">function</span><span class="pun">()</span><span class="pln"> </span><span class="pun">{</span><span class="pln">
      expect</span><span class="pun">(</span><span class="pln">scope</span><span class="pun">.</span><span class="pln">phones</span><span class="pun">.</span><span class="pln">length</span><span class="pun">).</span><span class="pln">toBe</span><span class="pun">(</span><span class="lit">3</span><span class="pun">);</span><span class="pln">
    </span><span class="pun">});</span><span class="pln">

    it</span><span class="pun">(</span><span class="str">'should set the default value of orderProp model'</span><span class="pun">,</span><span class="pln"> </span><span class="kwd">function</span><span class="pun">()</span><span class="pln"> </span><span class="pun">{</span><span class="pln">
      expect</span><span class="pun">(</span><span class="pln">scope</span><span class="pun">.</span><span class="pln">orderProp</span><span class="pun">).</span><span class="pln">toBe</span><span class="pun">(</span><span class="str">'age'</span><span class="pun">);</span><span class="pln">
    </span><span class="pun">});</span><span class="pln">
  </span><span class="pun">});</span><span class="pln">
</span><span class="pun">});</span></code></pre>

<p>单元测试现在验证了默认值被正确设置。</p>

<p>我们使用Jasmine的接口把<code>PhoneListCtrl</code>控制器提取到一个<code>beforeEach</code>块中，这个块会被所有的父块<code>describe</code>中的所有测试所共享。</p>

<p>运行这些单元测试，跟以前一样，执行<code>./scripts/test.sh</code>脚本，你应该会看到如下输出（<strong>注意：</strong>要在浏览器打开<a href="http://localhost:9876" target="_blank">http://localhost:9876</a>并进入严格模式，测试才会运行！）：</p>

<pre class="prettyprint"><code><span class="typ">Chrome</span><span class="pun">:</span><span class="pln"> </span><span class="typ">Runner</span><span class="pln"> reset</span><span class="pun">.</span><span class="pln">
</span><span class="pun">..</span><span class="pln">
</span><span class="typ">Total</span><span class="pln"> </span><span class="lit">2</span><span class="pln"> tests </span><span class="pun">(</span><span class="typ">Passed</span><span class="pun">:</span><span class="pln"> </span><span class="lit">2</span><span class="pun">;</span><span class="pln"> </span><span class="typ">Fails</span><span class="pun">:</span><span class="pln"> </span><span class="lit">0</span><span class="pun">;</span><span class="pln"> </span><span class="typ">Errors</span><span class="pun">:</span><span class="pln"> </span><span class="lit">0</span><span class="pun">)</span><span class="pln"> </span><span class="pun">(</span><span class="lit">3.00</span><span class="pln"> ms</span><span class="pun">)</span><span class="pln">
  </span><span class="typ">Chrome</span><span class="pln"> </span><span class="lit">19.0</span><span class="pun">.</span><span class="lit">1084.36</span><span class="pln"> </span><span class="typ">Mac</span><span class="pln"> OS</span><span class="pun">:</span><span class="pln"> </span><span class="typ">Run</span><span class="pln"> </span><span class="lit">2</span><span class="pln"> tests </span><span class="pun">(</span><span class="typ">Passed</span><span class="pun">:</span><span class="pln"> </span><span class="lit">2</span><span class="pun">;</span><span class="pln"> </span><span class="typ">Fails</span><span class="pun">:</span><span class="pln"> </span><span class="lit">0</span><span class="pun">;</span><span class="pln"> </span><span class="typ">Errors</span><span class="pln"> </span><span class="lit">0</span><span class="pun">)</span><span class="pln"> </span><span class="pun">(</span><span class="lit">3.00</span><span class="pln"> ms</span><span class="pun">)</span></code></pre>

<p>现在我们把注意力转移到端到端测试上来。</p>

<p><strong>test/e2e/scenarios.js：</strong></p>

<pre class="prettyprint"><code><span class="pun">...</span><span class="pln">
    it</span><span class="pun">(</span><span class="str">'should be possible to control phone order via the drop down select box'</span><span class="pun">,</span><span class="pln">
        </span><span class="kwd">function</span><span class="pun">()</span><span class="pln"> </span><span class="pun">{</span><span class="pln">
      </span><span class="com">//let's narrow the dataset to make the test assertions shorter</span><span class="pln">
      input</span><span class="pun">(</span><span class="str">'query'</span><span class="pun">).</span><span class="pln">enter</span><span class="pun">(</span><span class="str">'tablet'</span><span class="pun">);</span><span class="pln">

      expect</span><span class="pun">(</span><span class="pln">repeater</span><span class="pun">(</span><span class="str">'.phones li'</span><span class="pun">,</span><span class="pln"> </span><span class="str">'Phone List'</span><span class="pun">).</span><span class="pln">column</span><span class="pun">(</span><span class="str">'phone.name'</span><span class="pun">)).</span><span class="pln">
          toEqual</span><span class="pun">([</span><span class="str">"Motorola XOOM\u2122 with Wi-Fi"</span><span class="pun">,</span><span class="pln">
                   </span><span class="str">"MOTOROLA XOOM\u2122"</span><span class="pun">]);</span><span class="pln">

      </span><span class="kwd">select</span><span class="pun">(</span><span class="str">'orderProp'</span><span class="pun">).</span><span class="pln">option</span><span class="pun">(</span><span class="str">'Alphabetical'</span><span class="pun">);</span><span class="pln">

      expect</span><span class="pun">(</span><span class="pln">repeater</span><span class="pun">(</span><span class="str">'.phones li'</span><span class="pun">,</span><span class="pln"> </span><span class="str">'Phone List'</span><span class="pun">).</span><span class="pln">column</span><span class="pun">(</span><span class="str">'phone.name'</span><span class="pun">)).</span><span class="pln">
          toEqual</span><span class="pun">([</span><span class="str">"MOTOROLA XOOM\u2122"</span><span class="pun">,</span><span class="pln">
                   </span><span class="str">"Motorola XOOM\u2122 with Wi-Fi"</span><span class="pun">]);</span><span class="pln">
    </span><span class="pun">});</span><span class="pln">
</span><span class="pun">...</span></code></pre>

<p>端到端测试验证了选项框的排序机制是正确的。</p>

<p>你现在可以刷新你的浏览器，然后重新跑一遍端到端测试，或者你可以在<a href="http://angular.github.com/angular-phonecat/step-4/test/e2e/runner.html" target="_blank">AngularJS的服务器</a>上运行一下。</p>

<h2>练习</h2>

<ul>
<li>在<code>PhoneListCtrl</code>控制器中，把设置<code>orderProp</code>那条语句删掉，你会看到AngularJS会在下拉菜单中临时添加一个<strong>空白</strong>的选项，并且排序顺序是默认排序（即未排序）。</li>
<li>在<code>index.html</code>模板里面添加一个`{{orderProp}}绑定来实时显示它的值。</li>
</ul>

<h2>总结</h2>

<p>现在你已经为你的应用提供了搜索功能，并且完整的进行了测试。<a href="http://www.ituring.com.cn/article/15765">步骤5</a>我们将学习AngularJS的服务以及AngularJS如何使用依赖注入。</p>
                </div>

**来源：** [http://www.ituring.com.cn/article/15764](http://www.ituring.com.cn/article/15764)