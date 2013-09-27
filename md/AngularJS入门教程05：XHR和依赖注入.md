# AngularJS入门教程05：XHR和依赖注入 #


<div class="post-text">
<p>到现在为止，我们使用是硬编码的三条手机记录数据集。现在我们使用AngularJS一个内置服务<a href="http://code.angularjs.org/1.1.0/docs/api/ng.%24http" target="_blank">$http</a>来获取一个更大的手机记录数据集。我们将使用AngularJS的<a href="http://angularjs.cn/A00z" target="_blank">依赖注入（dependency injection (DI)）</a>功能来为<code>PhoneListCtrl</code>控制器提供这个AngularJS服务。</p>

<p>请重置工作目录：</p>

<pre class="prettyprint"><code><span class="pln">git checkout </span><span class="pun">-</span><span class="pln">f step</span><span class="pun">-</span><span class="lit">5</span></code></pre>

<p>刷新浏览器，你现在应该能看到一个20部手机的列表。</p>

<p>步骤4和步骤5之间最重要的不同在下面列出。你可以在<a href="https://github.com/angular/angular-phonecat/compare/step-4...step-5">GitHub</a>里看到完整的差别。</p>

<h3>数据</h3>

<p>你项目当中的<code>app/phones/phones.json</code>文件是一个数据集，它以JSON格式存储了一张更大的手机列表。</p>

<p>下面是这个文件的一个样例：</p>

<pre class="prettyprint"><code><span class="pun">[</span><span class="pln">
 </span><span class="pun">{</span><span class="pln">
  </span><span class="str">"age"</span><span class="pun">:</span><span class="pln"> </span><span class="lit">13</span><span class="pun">,</span><span class="pln">
  </span><span class="str">"id"</span><span class="pun">:</span><span class="pln"> </span><span class="str">"motorola-defy-with-motoblur"</span><span class="pun">,</span><span class="pln">
  </span><span class="str">"name"</span><span class="pun">:</span><span class="pln"> </span><span class="str">"Motorola DEFY\u2122 with MOTOBLUR\u2122"</span><span class="pun">,</span><span class="pln">
  </span><span class="str">"snippet"</span><span class="pun">:</span><span class="pln"> </span><span class="str">"Are you ready for everything life throws your way?"</span><span class="pln">
  </span><span class="pun">...</span><span class="pln">
 </span><span class="pun">},</span><span class="pln">
</span><span class="pun">...</span><span class="pln">
</span><span class="pun">]</span></code></pre>

<h3>控制器</h3>

<p>我们在控制器中使用AngularJS服务<a href="http://code.angularjs.org/1.1.0/docs/api/ng.%24http" target="_blank">$http</a>向你的Web服务器发起一个HTTP请求，以此从<code>app/phones/phones.json</code>文件中获取数据。<code>$http</code>仅仅是AngularJS众多内建服务中之一，这些服务可以处理一些Web应用的通用操作。AngularJS能将这些服务注入到任何你需要它们的地方。</p>

<p>服务是通过AngularJS的<a href="http://angularjs.cn/A00z" target="_blank">依赖注入DI子系统</a>来管理的。依赖注入服务可以使你的Web应用良好构建（比如分离表现层、数据和控制三者的部件）并且松耦合（一个部件自己不需要解决部件之间的依赖问题，它们都被DI子系统所处理）。</p>

<p><strong>app/js/controllers.js</strong></p>

<pre class="prettyprint"><code><span class="kwd">function</span><span class="pln"> </span><span class="typ">PhoneListCtrl</span><span class="pun">(</span><span class="pln">$scope</span><span class="pun">,</span><span class="pln"> $http</span><span class="pun">)</span><span class="pln"> </span><span class="pun">{</span><span class="pln">
  $http</span><span class="pun">.</span><span class="kwd">get</span><span class="pun">(</span><span class="str">'phones/phones.json'</span><span class="pun">).</span><span class="pln">success</span><span class="pun">(</span><span class="kwd">function</span><span class="pun">(</span><span class="pln">data</span><span class="pun">)</span><span class="pln"> </span><span class="pun">{</span><span class="pln">
    $scope</span><span class="pun">.</span><span class="pln">phones </span><span class="pun">=</span><span class="pln"> data</span><span class="pun">;</span><span class="pln">
  </span><span class="pun">});</span><span class="pln">

  $scope</span><span class="pun">.</span><span class="pln">orderProp </span><span class="pun">=</span><span class="pln"> </span><span class="str">'age'</span><span class="pun">;</span><span class="pln">
</span><span class="pun">}</span><span class="pln">

</span><span class="com">//PhoneListCtrl.$inject = ['$scope', '$http'];</span></code></pre>

<p><code>$http</code>向Web服务器发起一个<code>HTTP GET</code>请求，索取<code>phone/phones.json</code>（注意，url是相对于我们的<code>index.html</code>文件的）。服务器用json文件中的数据作为响应。（这个响应或许是实时从后端服务器动态产生的。但是对于浏览器来说，它们看起来都是一样的。为了简单起见，我们在教程里面简单地使用了一个json文件。）</p>

<p><code>$http</code>服务用<code>success</code>返回[对象应答][ng.$q]。当异步响应到达时，用这个对象应答函数来处理服务器响应的数据，并且把数据赋值给作用域的<code>phones</code>数据模型。注意到AngularJS会自动检测到这个json应答，并且已经为我们解析出来了！</p>

<p>为了使用AngularJS的服务，你只需要在控制器的构造函数里面作为参数声明出所需服务的名字，就像这样：</p>

<pre class="prettyprint"><code><span class="kwd">function</span><span class="pln"> </span><span class="typ">PhoneListCtrl</span><span class="pun">(</span><span class="pln">$scope</span><span class="pun">,</span><span class="pln"> $http</span><span class="pun">)</span><span class="pln"> </span><span class="pun">{...}</span></code></pre>

<p>当控制器构造的时候，AngularJS的依赖注入器会将这些服务注入到你的控制器中。当然，依赖注入器也会处理所需服务可能存在的任何传递性依赖（一个服务通常会依赖于其他的服务）。</p>

<p>注意到参数名字非常重要，因为注入器会用他们去寻找相应的依赖。</p>

<p><img src="http://code.angularjs.org/1.1.0/docs/img/tutorial/xhr_service_final.png" alt="xhr_service_final.png"></p>

<h4>'$'前缀命名习惯</h4>

<p>你可以创建自己的服务，实际上我们在<a href="http://www.ituring.com.cn/article/15771">步骤11</a>就会学习到它。作为一个命名习惯，AngularJS内建服务，作用域方法，以及一些其他的AngularJS API都在名字前面使用一个‘$’前缀。不要使用‘$’前缀来命名你自己的服务和模型，否则可能会产生名字冲突。</p>

<h4>关于JS压缩</h4>

<p>由于AngularJS是通过控制器构造函数的参数名字来推断依赖服务名称的。所以如果你要<a href="http://en.wikipedia.org/wiki/Minification_%28programming%29" target="_blank">压缩</a><code>PhoneListCtrl</code>控制器的JS代码，它所有的参数也同时会被压缩，这时候依赖注入系统就不能正确的识别出服务了。</p>

<p>为了克服压缩引起的问题，只要在控制器函数里面给<code>$inject</code>属性赋值一个依赖服务标识符的数组，就像被注释掉那段最后一行那样：</p>

<pre class="prettyprint"><code><span class="typ">PhoneListCtrl</span><span class="pun">.</span><span class="pln">$inject </span><span class="pun">=</span><span class="pln"> </span><span class="pun">[</span><span class="str">'$scope'</span><span class="pun">,</span><span class="pln"> </span><span class="str">'$http'</span><span class="pun">];</span></code></pre>

<p>另一种方法也可以用来指定依赖列表并且避免压缩问题——使用Javascript数组方式构造控制器：把要注入的服务放到一个字符串数组（代表依赖的名字）里，数组最后一个元素是控制器的方法函数：</p>

<pre class="prettyprint"><code><span class="kwd">var</span><span class="pln"> </span><span class="typ">PhoneListCtrl</span><span class="pln"> </span><span class="pun">=</span><span class="pln"> </span><span class="pun">[</span><span class="str">'$scope'</span><span class="pun">,</span><span class="pln"> </span><span class="str">'$http'</span><span class="pun">,</span><span class="pln"> </span><span class="kwd">function</span><span class="pun">(</span><span class="pln">$scope</span><span class="pun">,</span><span class="pln"> $http</span><span class="pun">)</span><span class="pln"> </span><span class="pun">{</span><span class="pln"> </span><span class="com">/* constructor body */</span><span class="pln"> </span><span class="pun">}];</span></code></pre>

<p>上面提到的两种方法都能和AngularJS可注入的任何函数完美协作，要选哪一种方式完全取决于你们项目的编程风格，建议使用数组方式。</p>

<h3>测试</h3>

<p><strong>test/unit/controllerSpec.js：</strong></p>

<p>由于我们现在开始使用依赖注入，并且我们的控制器也含有了许多依赖服务，所以为我们的控制器构造测试就有一点小小的复杂了。我们需要使用<code>new</code>操作并且提供给构造器包括<code>$http</code>的一些伪实现。然而，我们推荐的方法（而且更加简单噢）是在测试环境下创建一个控制器，使用的方法和AngularJS在产品代码于下面的场景下做的一样：</p>

<pre class="prettyprint"><code><span class="pln">describe</span><span class="pun">(</span><span class="str">'PhoneCat controllers'</span><span class="pun">,</span><span class="pln"> </span><span class="kwd">function</span><span class="pun">()</span><span class="pln"> </span><span class="pun">{</span><span class="pln">

  describe</span><span class="pun">(</span><span class="str">'PhoneListCtrl'</span><span class="pun">,</span><span class="pln"> </span><span class="kwd">function</span><span class="pun">(){</span><span class="pln">
    </span><span class="kwd">var</span><span class="pln"> scope</span><span class="pun">,</span><span class="pln"> ctrl</span><span class="pun">,</span><span class="pln"> $httpBackend</span><span class="pun">;</span><span class="pln">

    beforeEach</span><span class="pun">(</span><span class="pln">inject</span><span class="pun">(</span><span class="kwd">function</span><span class="pun">(</span><span class="pln">_$httpBackend_</span><span class="pun">,</span><span class="pln"> $rootScope</span><span class="pun">,</span><span class="pln"> $controller</span><span class="pun">)</span><span class="pln"> </span><span class="pun">{</span><span class="pln">
      $httpBackend </span><span class="pun">=</span><span class="pln"> _$httpBackend_</span><span class="pun">;</span><span class="pln">
      $httpBackend</span><span class="pun">.</span><span class="pln">expectGET</span><span class="pun">(</span><span class="str">'phones/phones.json'</span><span class="pun">).</span><span class="pln">
          respond</span><span class="pun">([{</span><span class="pln">name</span><span class="pun">:</span><span class="pln"> </span><span class="str">'Nexus S'</span><span class="pun">},</span><span class="pln"> </span><span class="pun">{</span><span class="pln">name</span><span class="pun">:</span><span class="pln"> </span><span class="str">'Motorola DROID'</span><span class="pun">}]);</span><span class="pln">

      scope </span><span class="pun">=</span><span class="pln"> $rootScope</span><span class="pun">.</span><span class="pln">$new</span><span class="pun">();</span><span class="pln">
      ctrl </span><span class="pun">=</span><span class="pln"> $controller</span><span class="pun">(</span><span class="typ">PhoneListCtrl</span><span class="pun">,</span><span class="pln"> </span><span class="pun">{</span><span class="pln">$scope</span><span class="pun">:</span><span class="pln"> scope</span><span class="pun">});</span><span class="pln">
    </span><span class="pun">}));</span></code></pre>

<p>注意：因为我们在测试环境中加载了<strong>Jasmine</strong>和<strong>angular-mock.js</strong>，我们有了两个辅助方法，<a href="http://code.angularjs.org/1.1.0/docs/api/angular.mock.module" target="_blank">module</a>和<a href="http://code.angularjs.org/1.1.0/docs/api/angular.mock.inject" target="_blank">inject</a>，来帮助我们获得和配置注入器。</p>

<p>用如下方法，我们在测试环境中创建一个控制器：</p>

<ul>
<li><p>我们使用<code>inject</code>方法将<a href="http://code.angularjs.org/1.1.0/docs/api/ng.%24rootScope" target="_blank">$rootScope</a>，<a href="http://code.angularjs.org/1.1.0/docs/api/ng.%24controller" target="_blank">$controller</a>和<a href="http://code.angularjs.org/1.1.0/docs/api/ng.%24httpBackend" target="_blank">$httpBackend</a>服务实例注入到Jasmine的<code>beforeEach</code>函数里。这些实例都来自一个注入器，但是这个注入器在每一个测试内部都会被重新创建。这样保证了每一个测试都从一个周知的起始点开始，并且每一个测试都和其他测试相互独立。</p></li>
<li><p>调用<code>$rootScope.$new()</code>来为我们的控制器创建一个新的作用域。</p></li>
<li><p><code>PhoneListCtrl</code>函数和刚创建的作用域作为参数，传递给已注入的<code>$controller</code>函数。</p></li>
</ul>

<p>由于我们现在的代码在创建<code>PhoneListCtrl</code>子作用域之前，于控制器中使用<code>$http</code>服务获取了手机列表数据，我们需要告诉测试套件等待一个从控制器来的请求。我们可以这样做：</p>

<ul>
<li><p>将请求服务<code>$httpBackend</code>注入到我们的<code>beforeEach</code>函数中。这是这个服务的一个伪版本，这样做在产品环境中有助于处理所有的XHR和JSONP请求。服务的伪版本允许你不用考虑原生API和全局状态——随便一个都能构成测试的噩梦——就可以写测试。</p></li>
<li><p>使用<code>$httpBackend.expectGET</code>方法来告诉<code>$httpBackend</code>服务来等待一个HTTP请求，并且告诉它如何对其进行响应。注意到，当我们调用<code>$httpBackend.flush</code>方法之前，响应是不会被发出的。</p></li>
</ul>

<p>现在，</p>

<pre class="prettyprint"><code><span class="pln">it</span><span class="pun">(</span><span class="str">'should create "phones" model with 2 phones fetched from xhr'</span><span class="pun">,</span><span class="pln"> </span><span class="kwd">function</span><span class="pun">()</span><span class="pln"> </span><span class="pun">{</span><span class="pln">
  expect</span><span class="pun">(</span><span class="pln">scope</span><span class="pun">.</span><span class="pln">phones</span><span class="pun">).</span><span class="pln">toBeUndefined</span><span class="pun">();</span><span class="pln">
  $httpBackend</span><span class="pun">.</span><span class="pln">flush</span><span class="pun">();</span><span class="pln">

  expect</span><span class="pun">(</span><span class="pln">scope</span><span class="pun">.</span><span class="pln">phones</span><span class="pun">).</span><span class="pln">toEqual</span><span class="pun">([{</span><span class="pln">name</span><span class="pun">:</span><span class="pln"> </span><span class="str">'Nexus S'</span><span class="pun">},</span><span class="pln">
                           </span><span class="pun">{</span><span class="pln">name</span><span class="pun">:</span><span class="pln"> </span><span class="str">'Motorola DROID'</span><span class="pun">}]);</span><span class="pln">
</span><span class="pun">});</span></code></pre>

<ul>
<li><p>在浏览器里，我们调用<code>$httpBackend.flush()</code>来清空（flush）请求队列。这样会使得<code>$http</code>服务返回的promise（什么是promise请参见<a href="http://stackoverflow.com/questions/11004273/what-is-stdpromise" target="_blank">这里</a>）能够被解释成规范的应答。</p></li>
<li><p>我们设置一些断言，来验证手机数据模型已经在作用域里了。</p></li>
</ul>

<p>最终，我们验证<code>orderProp</code>的默认值被正确设置：</p>

<pre class="prettyprint"><code><span class="pln">it</span><span class="pun">(</span><span class="str">'should set the default value of orderProp model'</span><span class="pun">,</span><span class="pln"> </span><span class="kwd">function</span><span class="pun">()</span><span class="pln"> </span><span class="pun">{</span><span class="pln">
  expect</span><span class="pun">(</span><span class="pln">scope</span><span class="pun">.</span><span class="pln">orderProp</span><span class="pun">).</span><span class="pln">toBe</span><span class="pun">(</span><span class="str">'age'</span><span class="pun">);</span><span class="pln">
</span><span class="pun">});</span><span class="pln">
</span><span class="pun">;</span></code></pre>

<p>执行<code>./scripts/test.sh</code>脚本来运行测试，你应该会看到如下输出：</p>

<pre class="prettyprint"><code><span class="typ">Chrome</span><span class="pun">:</span><span class="pln"> </span><span class="typ">Runner</span><span class="pln"> reset</span><span class="pun">.</span><span class="pln">
</span><span class="pun">..</span><span class="pln">
</span><span class="typ">Total</span><span class="pln"> </span><span class="lit">2</span><span class="pln"> tests </span><span class="pun">(</span><span class="typ">Passed</span><span class="pun">:</span><span class="pln"> </span><span class="lit">2</span><span class="pun">;</span><span class="pln"> </span><span class="typ">Fails</span><span class="pun">:</span><span class="pln"> </span><span class="lit">0</span><span class="pun">;</span><span class="pln"> </span><span class="typ">Errors</span><span class="pun">:</span><span class="pln"> </span><span class="lit">0</span><span class="pun">)</span><span class="pln"> </span><span class="pun">(</span><span class="lit">3.00</span><span class="pln"> ms</span><span class="pun">)</span><span class="pln">
  </span><span class="typ">Chrome</span><span class="pln"> </span><span class="lit">19.0</span><span class="pun">.</span><span class="lit">1084.36</span><span class="pln"> </span><span class="typ">Mac</span><span class="pln"> OS</span><span class="pun">:</span><span class="pln"> </span><span class="typ">Run</span><span class="pln"> </span><span class="lit">2</span><span class="pln"> tests </span><span class="pun">(</span><span class="typ">Passed</span><span class="pun">:</span><span class="pln"> </span><span class="lit">2</span><span class="pun">;</span><span class="pln"> </span><span class="typ">Fails</span><span class="pun">:</span><span class="pln"> </span><span class="lit">0</span><span class="pun">;</span><span class="pln"> </span><span class="typ">Errors</span><span class="pln"> </span><span class="lit">0</span><span class="pun">)</span><span class="pln"> </span><span class="pun">(</span><span class="lit">3.00</span><span class="pln"> ms</span><span class="pun">)</span></code></pre>

<h2>练习</h2>

<ul>
<li><p>在<strong>index.html</strong>末尾添加一个<code>{{phones | json}}</code>绑定，观察json格式的手机列表。</p></li>
<li><p>在<code>PhoneListCtrl</code>控制器中，把HTTP应答预处理一下，使得只显示手机列表的前五个。在<code>$http</code>回调函数里面使用如下代码：</p>

<pre class="prettyprint"><code><span class="pln"> $scope</span><span class="pun">.</span><span class="pln">phones </span><span class="pun">=</span><span class="pln"> data</span><span class="pun">.</span><span class="pln">splice</span><span class="pun">(</span><span class="lit">0</span><span class="pun">,</span><span class="pln"> </span><span class="lit">5</span><span class="pun">);</span></code></pre></li>
</ul>

<h2>总结</h2>

<p>现在你应该感觉得到使用AngularJS的服务是多么的容易（这都要归功于AngularJS服务的依赖注入机制），转到<a href="http://www.ituring.com.cn/article/15766">步骤6</a>，你会为手机添加缩略图和链接。</p>
                </div>



**来源：** [http://www.ituring.com.cn/article/15765](http://www.ituring.com.cn/article/15765)