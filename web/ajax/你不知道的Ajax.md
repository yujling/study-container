

# 前言

作为一名前端开发人员，我们经常使用$.ajax（前几年用的比较多）、axios或者fetch来和后台进行数据交互，大家也都知道它们使用的都是ajax技术，那么我们真的了解ajax的历史和工作原理吗？





作为一名前端开发人员，我们常常都会使用ajax技术与后台进行数据交互，比如前几年被使用坏掉的Jquery中的$.ajax,Vue官网被废弃的vue-resource,现在主流使用的axios或者fetch。在工作开发的时候，为了提高工作效率，我们大部分人都是直接使用这些封装后的方法或者插件工作。那么ajax到底是什么呢？似乎完美无缺的它有什么优点和缺点呢？它的底层原理又是什么呢？我们一起来探索吧

# 一.AJAX是什么

### 1.ajax为什么会出现？

**任何事物的存在，必然有它存在的理由**

以前的技术在使用Servlet+JSP+JavaBean开发模式（MVC）的过程中（我还记得大学时期，老师教我们学习Servlet+JSP+JavaBean，那个时候学的懵懵的，反正啥也没会，到现在就记得有这么个东西，但是不会这些技术，因为从来没学会，感兴趣的可以看看这个https://blog.csdn.net/u014010512/article/details/80746206，跟我上大学时，老师教的差不多，反正是各种创建包），我们的Web应用允许用户在网页端填写并提交表单（Form），以向网页服务器发送请求（Request）。服务器接收请求并处理传来的表单，返回响应(Response)，浏览器取得响应后，通过解析将页面展示在浏览器上，这样就完成了一次用户与服务器的交互。

![image-20200507115759400](C:\Users\18846\Desktop\ajax\image-20200507115759400.png)

​              （在网上找了这么一张图，大家可以感受一下） 

这种模式的原理是这样的：我们看上图，很形象的说明了servlet的MVC模式的运行原理。浏览器发送请求到jsp，所有的请求都会给servlet来处理，servlet通过对javaBean，即核心的model处理，得到处理结果，在返回给view层的jsp页面，jsp页面返回给浏览器最后的html网页。

这种servlet的MVC模式给我的感受是浏览器发送请求成功后，后面需要处理的程序太多了，程序多，说明需要的时间就长，出现故障或者错误的几率就会提高（每一个环节都不能出现问题），那么在实际应用中用户体验就不会那么好。

然而，这种模式存在一些问题。现在看这样一个例子：浏览器端展示了用户登录界面，当用户输入用户名、密码及验证码后，数据被发送到了服务器，假定我们在Servlet中处理请求后发现用户名及密码不匹配，我们接下来会做什么？
  我们会重新将页面连同错误信息的响应返回给浏览器端，浏览器在解析响应后对信息进行展示，使用这样的开发流程，无论业务实现的多么完美都会有一些固有的问题：
  首先，**浪费带宽。**在前后两个页面中除了展示错误信息的部分外其它元素全都是相同的。
  其次，**用户体验差。**假设用户在表单中不小心输入了错误的用户名，在提交表单后就会出现等待一段时间后页面被刷新、并提示用户名错误的情况，这样用户又得重新输入一遍用户名及密码，体验极不友好。在用户的网速比较慢的情况下，用户体验还会更差。
  那么有没有什么方法可以解决这种问题呢？也就是，能不能在用户刚输入用户名时就可以得到反馈呢？

##  2.**AJAX的大致思路**

  在js的XMLHttpRequest对象出现之前是没有办法的，但是在它出现之后，前辈们想到了一种比较好的解决方法，即：使用XMLHttpRequest对象作为Agent来将请求发送给服务器，并用它来接收服务器返回的数据，这样就可以不跳转页面完成数据的交互，而且只需要传输少量必要数据，因此对网速的要求也变低了。
  但是，还有两个问题没有解决：
  1.如何根据服务器端返回的数据动态更改页面，以达到与用户交互的作用？
  2.如何规定服务器发送回来的数据格式，使它更通用，并且传输量尽可能的少呢？
  对于1：前辈们选择了使用JavaScript，个人理解这样做的原因有两个，第一，JavaScript足够流行，几乎所有的主流浏览器都对JavaScript提供了支持；第二，JavaScript可以通过DOM编程的方式来动态的改变网页的内容。
  对于2：前辈们选择了XML，我想可能是因为它语法足够严格、语义明确而且更加通用吧。但是我认为传输的数据格式对使用AJAX并没有影响。例如，我们可以选择传输Json来使传输的数据更加少，甚至可以选择传输一段有意义的字符串，只要服务器端与浏览器端开发者对格式进行了约定即可。当然现在更多的是传输JSON格式的数据

2.AJAX的定义

Ajax 即“**A**synchronous  **J**avascript **A**nd  **X**ML”（异步 JavaScript 和 XML），是指一种创建交互式、快速动态[网页](https://baike.baidu.com/item/网页/99347)应用的网页开发技术，无需重新加载整个网页的情况下，能够更新部分网页的技术。                   

​                                                                                                                   ——百度百科

简而言之，AJAX 是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。

需要注意的是，Ajax 不是一种新的编程语言，而是一种用于创建更好更快以及交互性更强的Web应用程序的技术。

# 二.AJAX的优缺点

### 1.优点

  能在不更新整个页面的前提下维护数据。这使得Web应用程序更为迅捷地回应用户动作，并避免了在网络上发送那些没有改变的信息。

​     节省带宽

### 2.缺点

-  它可能破坏浏览器的后退功能。在动态更新页面的情况下，用户无法回到前一个页面状态，这是因为浏览器仅能记下历史记录中的静态页面（现代浏览器一般都可以解决这个问题）。
- 使用动态页面更新使得用户难于将某个特定的状态保存到收藏夹收藏或书签中（HTML5以后这个问题已经解决了）。

- 进行Ajax开发时，网络延迟——即用户发出请求到服务器发出响应之间的间隔——需要慎重考虑。这个问题在实际开发中一般都会用Loading解决

# 三.AJAX的原理

### 1.Ajax的原理

Ajax的原理，网上一搜一大堆文章，感兴趣的可以去看一下这篇文章[ 一个完整的ajax请求](https://blog.csdn.net/fortunegrant/article/details/79534732)    这里我就简单说一下,先放一张图

![image-20200507133912210](C:\Users\18846\Desktop\ajax\image-20200507133912210.png)

 **Ajax的原理简单来说通过XmlHttpRequest对象来向服务器发送异步请求，从服务器获得数据，然后用javascript来操作DOM而更新页面。**

这其中最关键的一步就是从服务器获得请求数据。要清楚这个过程和原理，我们必须对 XMLHttpRequest有所了解。

### 2.XMLHttpRequest

XMLHttpRequest是ajax的核心机制，它是在IE5中首先引入的，是一种支持异步请求的技术。简单的说，也就是javascript可以及时向服务器提出请求和处理响应，而不阻塞用户。达到无刷新的效果。

 　XMLHttpRequest这个对象的属性有：

- onreadystatechange 每次状态改变所触发事件的事件处理程序。
- responseText   从服务器进程返回数据的字符串形式。
-  responseXML   从服务器进程返回的DOM兼容的文档数据对象。

-  status      从服务器返回的数字代码，比如常见的404（未找到）和200（已就绪）等

- status Text    伴随状态码的字符串信息

- readyState    对象状态值

　　　　0 (未初始化) 对象已建立，但是尚未初始化（尚未调用open方法）

　　　　1 (初始化) 对象已建立，尚未调用send方法

　　　　2 (发送数据) send方法已调用，但是当前的状态及http头未知

　　　　3 (数据传送中) 已接收部分数据，因为响应及http头不全，这时通过responseBody和responseText获取部分数据会出现错误，

　　　　4 (完成) 数据接收完毕,此时可以通过通过responseXml和responseText获取完整的回应数据

 

　　但是，由于各浏览器之间存在差异，所以创建一个XMLHttpRequest对象可能需要不同的方法。这个差异主要体现在IE和其它浏览器之间。下面是一个比较标准的创建XMLHttpRequest对象的方法。

```
function CreateXmlHttp() {

    //非IE浏览器创建XmlHttpRequest对象
    if (window.XmlHttpRequest) {
        xmlhttp = new XmlHttpRequest();
    }

    //IE浏览器创建XmlHttpRequest对象
    //主要针对老版本的 Internet Explorer （IE5 和 IE6）使用 ActiveX 对象
    if (window.ActiveXObject) {
        try {
            xmlhttp = new       ActiveXObject("Microsoft.XMLHTTP");
        }
        catch (e) {
            try {
                xmlhttp = new ActiveXObject("msxml2.XMLHTTP");
            }
            catch (ex) { }
        }
    }
}

var xhr = CreateXmlHttp(); //创建一个XMLHttpRequest对象
```

# 四.ajax的使用及实现步骤

### 1.实现步骤

ajax的具体使用，具体一点的就分为以下6个步骤:

​    (1) 创建XMLHttpRequest对象,也就是创建一个异步调用对象.
  (2) 创建一个新的HTTP请求,并指定该HTTP请求的方法、URL及验证信息.
  (3)设置响应HTTP请求状态变化的函数.
  (4)发送HTTP请求.
  (5)获取异步调用返回的数据.
  (6)使用JavaScript和DOM实现局部刷新.

如果你要是觉得上面6个步骤记不住，或者不好理解，可以看看下面典型的xhr建立ajax的过程。

典型的xhr建立ajax的过程分为以下4个步骤(包含大部分情况)：

(1)new一个xhr对象。
(2)调用xhr对象的open方法。
(3)send一些数据。
(4)对服务器的响应过程进行监听，来知道服务器是否正确得做出了响应，接着就可以做一些事情。比如获取服务器响应的内容，在页面上进行呈现。

上面不管是具体一点的6步骤还是典型一点的4个步骤，他们本质和原理都是一样的。

### 2.具体使用

(1)创建Ajax核心对象XMLHttpRequest

```
var xmlHttp;
if(window.XMLHttpRequest){  //针对除IE6以外的浏览器
    xmlHttp=new XMLHttpRequest(); //实例化一个XMLHttpRequest
}else{
   xmlHttp=new ActiveXObject("Microsoft.XMLHTTP");   //针对IE5,IE6
}
```

也可以像上文一样先封装一个CreateXmlHttp()方法

(2)向服务器发送请求

```
xmlhttp.open(method,url,async);//打开连接
xmlhttp.send(); //发送请求
```

具体实例：

```
  var xmlHttp = new XMLHttpRequest();  //①创建异步对象（不考虑IE5,6）  
   xmlHttp.open('get','demo_get.html','true');//②调用open()方法并采用异步方式
   xmlHttp.send(); //③使用open()方法将请求发送出去
   xmlHttp.onreadystatechange()=>{
        if(xmlHttp.readyState === 4 && xmlHttp.status === 200){ 
           var data = xmlHttp.responseText;//获取异步返回的数据
           ... //前端对数据处理
        }
}
```



附一段完整的代码：以get请求为例

```
 //封装异步对象
function CreateXmlHttp() {

    //非IE浏览器创建XmlHttpRequest对象
    if (window.XmlHttpRequest) {
        return new XMLHttpRequest();
    }

    //IE浏览器创建XmlHttpRequest对象
    //主要针对老版本的 Internet Explorer （IE5 和 IE6）使用 ActiveX 对象
    if (window.ActiveXObject) {
        try {
            return new  ActiveXObject("Microsoft.XMLHTTP");
        }
        catch (e) {
            try {
            return new ActiveXObject("msxml2.XMLHTTP");
            }
            catch (ex) { }
        }
    }
}

 var xmlHttp = new XMLHttpRequest();  //①创建异步对象  
    xmlHttp.onreadystatechange = function(){  
        //④对服务器的响应过程进行监听，响应成功则对响应数据进行处理
        if(xmlHttp.readyState === 4 && xmlHttp.status === 200){ 
        var data = xmlHttp.responseText;//获取异步返回的数据
            //前端对数据处理
        }
    }
    xmlHttp.open('GET','xiaoling_Demo.html',false);//②调用open()方法并采用异步方式   xmlhttp.open("GET","test1.txt",true);
    xmlHttp.send(); //③使用open()方法将请求发送出去
```

