# RESTful Web API 读书笔记
- - -
### HTTP OPTIONS
客户端用options方法查看可以对某个资源作哪些操作(http方法); 服务器的OPTIONS请求响应里有http allow报头(header), 类似：Allow : GET, HEAD 进行响应。<br>
服务端可以根据客户端OPTIONS请求里给出的报头，决定什么样的allow报头。比如：如果OPTIONS请求有Authorization报头, 请求响应的Allow：GET, HEAD, PUT, DELETE; 否则就是Allow: GET, HEAD; **通过这样达到简单的访问控制检查**
- - -
### HTTP POST
> RFC 2616对POST的定义:POST的目的是允许采用一个统一的方法来作以下操作
> - 对现有资源进行注释;
> - 向公告板、新闻组、邮件列表等发布一则消息;
> - 向一个数据加工程序提供一个数据块(比如提交表单所产生的);
> - 通过一个添加(append)操作来扩充数据库;

PS:  _POST方法实际执行的操作由服务器决定，一般根据所请求的URI(Request-URI)_，而被post的实体(posted entity)从属于那个URI,如同一个文件从属于包含它的目录，一篇文章从属于发布它的新闻组，一条记录从属于它所在的数据库
- - -
### PUT与POST的区别
假如客户端决定新资源用什么URI，就用PUT;<br>
假如服务器决定新资源采用什么URI，就用POST;<br>
也就是客户端用POST创建资源时，客户端不必知道新资源的确切URI，大多数情况下只需要知道父资源的URI就行。<br>
服务器对这种添加资源的POST请求的响应，通常具有HTTP状态码:201(Created),而且在Location报头包含新建从属资源的URI。客户端就能拿到这个URI, 以后就能用PUT修改，DELETE删除，GET获取表示。<br>
示例对比POST和PUT:

:)                            |向新资源发PUT请求      |向已有资源发PUT请求    | POST
:----------------------------:|:----------------------|:----------------------|:-----------------------
/webblogs                     |N/A(资源已存在)        |无效果                 |创建1个新博客
/webblogs/myweblog            |创建该博客             |修改该博客的设置       |往该博客添加一个文章    
/webblogs/myweblog/entries/1  |N/A(你无法知道这个URI) |编辑该博客文章         |为该博客文章添加一个评论
_PS: (N/A Not applicable 不适用)_
- - -
### 安全性(Safety)和幂等性(itempotence)
用来描述HTTP方法的特性: GET和HEAD请求都是安全的; GET, HEAD, PUT和DELETE都是幂等的.
1. 安全性(Safety)<br>
GET和HEAD请求都是读取服务器数据的请求，不改变服务器的状态。无论请求发送几次，服务器的状态都不会改变。
2. 幂等性(Itempotence)<br>
在数学里，一个幂等性的操作无论被运用多少次，结果都一样，比如乘以0。假如对一个资源的某个操作是幂等的，则无论对资源发多少次该资源，结果总是一样的。<br>

安全性和幂等性的重要性：允许客户端在非可靠的网络实现可靠的HTTP请求, 无非是请求发多几次的问题。<br>
_POST请求既不是安全的也不是幂等的，向一个资源发两次POST请求，可能会创建两个相同的从属资源。_
- - -
### POST Once Exactly(POE)
POE用于解决POST为不可靠的HTTP方法，POST请求不具有幂等性，即发送两次POST请求跟发送一次POST请求可能有不同的结果。<br>
POST Once Exactly(POE)是一种赋予HTTP POST幂等性的方法。如果一个资源支持POE，那么该资源生命周期中只对POST请求做一次成功的响应，后续的所有POST请求，將返回405("Method Not Allowed")。POE资源是一种为处理单个POST请求而暴露的一次性资源(one-off resource)。<br>
```
工作原理:
假设一个博客资源 /webblogs/myweblog, POST请求发布文章
客户端先向服务器发送GET或HEAD请求:
    HEAD /webblogs/myweblog HTTP/1.1
    Host: www.example.com
    POE:1
服务器对该请求的响应报行一个指向"当前还没有被POST过的POE资源"
    200 OK
    POE-Links: /webblogs/myweblog/entry-factory-104a4ed
```
POE和POE-Links就是为POE起草的报头。<br>
- - -
### URI设计三条原则
- 用路径变量(path variables)来表示层次结构(hierarchy): /parent/child
- 在路径变量中加入标点符号，以消除误解: /parent/child1;child2 (PS: 建议如果child1与child2的次序有关紧要时用,号，否则用;号
- 用查询变量(query variables)来表示算法的输入, 例如：/search?p=jellyfish&start=20
- - -
### 条件GET(conditional HTTP GET)** 用于节省客户端和服务器的带宽和时间
通过以下的报头(header)实现:
- 响应报头: Last-Modified; ETag
- 请求报头: IfModified-Since; If-None-Match

#### Last-Modified与IfModified-Since: 
服务器发送响应请求的时候用Last-Modified报头来标志资源的最后修改时间，客户端保存下这个Last-Modified的时间戳，下次再发同-个GET请求的时候，用这个时间戳作为IfModified-Since报头的值。这样服务器接受到这个GET请求的时候，查看IfModified-Since请求报头的时间戳，如果请求的资源最后修改时间比这个IfModified-Since时间迟(说明资源相对更新了), 服务器就返回响应代码为200,主体为新资源，Last-Modified为资源新的最后修改时间; 否则返回响应代码为304(Not Modified),并省去响应请求主体，客户端也知道上一次请求的值依然最新的资源。<br>
#### Etag与If-None—Match: 
双重保证，如果资源修改的时间差小于1秒，则依靠Last-Modified与IfModified-Since判断是有错，为资源生成一个ETag(可以只根据变动频繁的数据生成哈希等...), 客户端记录下上一次请求的ETag，作为下一次If-None-Match的值，服务器处理第二次请求时如果If-None-Match的值与当前的ETag相同，则认为资源没有发生变化。
- - -
### 认证与授权
#### 基本认证
简单的质询/回应(challenge/response). <br>
访问受到基本认证保护的资源如果没有提供正确的证书时。服务器就会发出质询, 响应码:401 Unauthorized, 以及报头WWW-Authenticate: Basic realm="My Private Data", WWW-Authenticate报头表示了什么样的证书(Basic), 指定realm(用于标志网站上的一组资源)。客户端接到质询后，需要以{用户名:密码}的Base64编码的字符串作为Authorization报头的值重新发送请求。<br>
_PS: 基本认证基本就是用明文传递用户名密码，可以用https解决，或者 TLS/SSL。<br>_
#### 摘要认证
HTTP摘要认证(digest authentication) 可以防止证书被窃听。<br>
认证过程:<br>
1. 客户端发出请求;
2. 服务器发出质询; WWW-Authenticate报头除了Basic换成Digest,还多了3则信息(nonce是一个随机字符串, qop指定连同请求消息主体计算哈希)
```
401 Unauthorized
WWW-Authenticate: Digest realm="My Private Data",qop="auth",nonce="0cc175b9c0f1b6a831c399e269772661",opaque="92eb5ffee6ae2fec3ad71c777531578f"
```
3. 客户端根据以下规则生成Authorization报头值;<br>
根据:请求里面的HTTP方法和URI路径、质询响应里的四则信息、用户名、密码、客户端nonce、序号 构造摘要字符串<br>
```ruby
算法过程(ruby代码):
#质询响应里的信息
REALM="My Private Data"
NONCE="0cc175b9c0f1b6a831c399e269772661"
OPAQUE="92eb5ffee6ae2fec3ad71c777531578f"
QOP="auth"
#客户端已知或计算出的值
NC="00000001"
CNONCE="4a8a08f09d37b7379038409b5f33"
USER="Alibaba"
PASSWORD="open sesame"
#ha1也是保存在服务器的"密码", ha3就是最后Authenticate报头的值
ha1 = MD5::hexdigest("#{user}:#{REALM}:#{PASSWORD}") #注意，这个值也是首次设置用户名密码发给服务器的
ha2 = MD5::hexdigest("#{METHOD}:#{PATH}")
ha3 = MD5::hexdigest("#{ha1}:#{NONCE}:#{NC}:#{CNONCE}:#{QOP};#{ha2}")
```
摘要认证跟基本认证的关键不同在于: 服务器也无法根据摘要获知用户的密码, 当用户为某个realm首次设置密码时，服务器需要为user:realm:password计算哈希值(即上面的ha1),然后记录下来，作为以后身份验证使用。<br>
另一个区别在于，客户端每次都需要发两次请求，因为需要质询里面的WWW-Authenticate报头信息生成ha3。
- - -
### Look-Before-You-Leap 请求
防止客户端**徒劳**地向服务器发送巨大的表示。除了主体内容，其他报头都与普通请求一致，外加Expect报头，值为"100-continue"。
```html
PUT /filestore/myfile.txt HTTP/1.1
Host: example.com
Content-length: 524288000
Expect: 100-continue
```
可以认为这是一个为将要发送的PUT请求(带实体)的询问：允许我在/filestore/myfile.txt 新建(put)一个表示么？大小为Content-size<br>
服务器根据客户端报头及该资源的当前状态作出决定. 如果愿意，就返回响应代码100("continue"),客户端就可以接着发送; 否则，就返回响应代码417("Expection Failed")，客户端就可以不用白白发500多M的主体内容。
- - -
### XHTML
媒体类型: application/xhtml+xml<br>
XHTML 在HTML的基础上施加了一些约束，这些约束是XHTML文档同时也是有效的XML文档(HTML不能被XML解析器解析).
- - -
### 应用表单(application form)
HTML里面的`<form>`元素。用于处理那些"名称符合一定模式"的资源，通过表单输入框的值来替换模式，所以可以生成不同的资源，然后向该URL发送请求(一般是GET/POST)。<br>
```
<form method="GET" action="http://search.example.com/search">
    <input name="q" type="text"/> 
    <input type="submit"/> 
</form>
//上面的表单可以生成类似 http://search.example.com/search?q=chocolate 的资源URI
```
- - -
### 资源表单(resource form)
资源表单用来告诉客户端如何构造一个表示(represention)来更新资源状态，然后发送POST或PUT请求(GET和DELETE请求是无需发送表示的)。
```
<form method="POST" action="http://files.example.com/dir/subdir/" enctype="multipart/form-data">
    <input type="text" name="description"/>
    <input type="file" name="newfile"/>
</form>
enctype属性用于指示资源类型
上面的表单用于发送一个POST请求 URI:http://files.example.com/dir/subdir/, 表示是一种"multipart/form-data"文档，其中包含文本描述和一个文档。
```
- - -
### URI 模板
```
<form id="createUser" method="put" template="user/{username}">
 <p>Username: <input type="text" name="username" /><br/>
 <p>Password: <input type="password" name="password" /><br/>
 <input type="submit" />
</form>
```
_PS:上面的表单在提交的时候username的input输入的值就会替换 "/user/{username}"之中，然后发送put请求。_
- - -
### Ajax
Ajax应用就是在Web浏览器里运行的Web服务客户端
#### Ajax架构
Ajax架构按照以下方式工作:
1. 用户(user)使用浏览器向一个应用的主URI(main URI)发出请求。
2. 服务器(server)返回一个嵌有脚本的Web页面。
3. 浏览器(browser)把Web页面呈现给User, 并运行脚本/或者等待用户触发脚本。
4. 脚本(script)向服务器的某个URI发送异步(asynchronous)HTTP请求，处理请求时用户可以继续其他操作。
5. 脚本解析HTTP响应,并根据响应提供的数据来更新用户试图。

Ajax 应用充当着原始数据(Web服务返回的)与HTML GUI(浏览器)之间的粘合剂的角色。
- - -
### Ajax库
Prototype和Dojo是两种两种用来发送简单的HTTP请求的Ajax库。
#### Prototype
Prototype(http://protype.conio.net/) 引入了以下三个用于发送HTTP请求的类:
- Ajax.Request:  XMLHttpRequest的封装类，负责跨浏览器问题。通过Request.transport可得实际的XMLHttpRequest对象
- Ajax.Updater: Request子类。用于发送HTTP请求，并把响应文档嵌入DOM的一个指定元素里
- Ajax.PeriodcalUpdater: 用于定期发送HTTP请求，并刷新DOM元素

#### Dojo
Dojo(http://dojotoolkit.org/)提供了一个统一的API。
- 隐藏了不同浏览器在XMLHttpRequest方面的差异 PS: XMLHttpRequest的所有变种都放在dojo.io.XMLHttp这个传输类中
- 隐藏了XMLHttpRequest跟“其他通过浏览器发送HTTP请求的方式”之间的差异
- - -
### Ajax发送跨域HTTP请求
#### "正规"方法
在JS代码里面通过以下代码申请"向外部WEB服务发送请求"的权限<br>
```
Netscape.security.PrivilegeManger.enablePrivilege("UniversalBrowserRead")
```
假如脚本是经过数字签名的，客户端的浏览器会向客户展示有关证书，客户决定是否信任，信任的话就会发送Web服务请求的权限。

存在问题: Netscape只能在Mozilla, Firefox等类似Netscape的浏览器上使用。而且，配置数字签名的脚本只有，HTTP文档是存放在一个签过名的Java档案文件中(类似:jar:http://www.example.com/ajax-app.jar!/index.html)，搜索引擎抓不到html文件。

#### 请求代理和JoD
#### 请求代理(request proxy)技术
假设A网站上的Ajax应用试图向B网站发送XMLHttpRequest请求。浏览器会阻止这个行为。所以不直接向B网站发请求，改成向A网站发送请求，在A网站的服务端"偷偷"向B网站发送请求，在返回给Ajax应用。<br>
以上过程就是请求代理技术, 也就是反向代理技术。
#### JoD技术 (JavaScript on Demand)
JoD通过从Web服务获取一个定制的程序(通过`<script src="...">` `<img>` `<frame>`标签，浏览器对src不设防！)，从而绕过了浏览器的安全策略，因为**任何JSON数据结构都是有效的JavaScript**程序，所以JoD特别适合于放回JSON表示的Web服务。<br>
库支持:Jason levit写的JSONscriptRequest 与 Dojo

- - -
### 正向代理与反向代理
都是代理**服务器**(Proxy Server), 允许网络客户端向另一个网络服务器进行非直接的连接。<br>
#### 正向代理
客户端特地向**正向代理服务器**发送请求, 由**正向代理服务器**代为转发请求。比如客户端浏览器设置的代理Proxy就是正向代理了，浏览器设置了代理(里面就有代理服务器的地址IP)之后，接下来访问网址A，浏览器不会直接向A网站的服务器发送请求，而是发送请求到**正向代理服务器**，**正向代理服务器**再向A网站的服务器发送请求，再把请求转发给客户端浏览器。 所以这里代理服务器代理的对象是“客户端”。
#### 反向代理
**反向代理器**一般没有客户端什么事，客户端是不知道有反向代理这回事的。**反向代理服务器**把自己的某些URI隐射到其他的服务器，就是把客户端发过来的请求转发到映射的服务器那里去，Nginx就是一种反向代理器。比如example.com网站把 example.com/baidu映射到www.baidu.com，则客户端访问example.com/baidu时候，example会转发这个请求到www.baidu.com (与正向代理不同的是：客户端并不是要“主动”地访问www.baidu.com)。所以这里代理服务器代理的对象是“服务器”。

