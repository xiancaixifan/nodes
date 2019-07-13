# Jquery入门学习

## 一.简介
### 1.Jquery是基于JavaScript的一种框架,兼容主流浏览器,提供了dom,animate(JQ+CSS),ajav;

### 2.Jquery2.0后版本不支持IE6/7/8浏览器
```
    特点
--- write less, do more
```


## 二.引入对象获取
### 1.引入
```JavaScript
<script src="jquery-1.11.0" type=""text/javascript>
</script>
```
### 2. 对象获取
```JavaScript
    //获取文本框内容
    <input type="text" name="username" id="username" value="java" />

    //获取文本框对象
    var ot = $("#username");
    或者
    var ot = JQuery("#usernmme");
```


### 3. Jquery对象操作
- **获取Jquery对象的两种方式:**
    - JQuery("选择器");
    - $("选择器");
        - 当然是各种各样的选择器啊

 - *演示:获取对象中的value属性值*
```JavaScript
<!DOCTYPE html>

<html>
    <head>
        <meta charset="utf-8" />
        <title>JQuery测试啊</title>
    </head>
    <body>
        <input type="text" id="username" value="觥筹啊觥筹" class="usr">
    </body>
</html>
<script src="js/jquery-1.11.0.min.js"></script>
<script type="text/javascript">
    var ol = $("#username");
    console.log(ol.val());  //觥筹啊觥筹
    var ol1 = document.getElementById("username");
    console.log(ol1.value);	//觥筹啊觥筹
</script>
```

## 三.JQuery对象和Dom对象的关系
### 1. Dom对象封装在Jquery对象中,他们可以相互转换,但是**无法直接使用Jqyery对象调用dom的方法**
### 2. 转换语法
    - Dom-&gt;JQ:<font color="red">$(dom对象)</font>
    - JQ-&gt;DOM:<font color="red">JQ[0] 或 JQ.get()</font> 

```JavaScript
觥筹啊觥筹Jquery_ol
index.html:18 觥筹啊觥筹Dom_ol1
ol.get(0).value
"觥筹啊觥筹"
ol1.value
"觥筹啊觥筹"
```


### 3. 基本语法
- 页面加载函数
    - 最后执行,最根本的: window.onload=function(){};
    - 在其他非加载完毕代码执行后执行,最先执行: $(document).ready();
    - 回调当前函数对象,所有执行顺序仅比window.onload高一点: $(document).ready(fnuction(){});
```java
<script type="text/javascript">

$(document).ready(function(){console.log("Jq2_页面加载完成")});//No.2

window.onload=function (){
    console.log("Dom_页面加载完成");
}//No.3

$(document).ready(console.log("Jq1_页面加载完成"));//No.1

</script>
```





## 四.Jquery选择器
**用来获取元素对象的**
### 1. 基本选择器
|名称|说明|语法|
|:-----------|:-----------|:-----------|
|id选择器|根据id来选择元素|$("#config")
|元素选择器|根据标签来选择元素|$("div")
|类选择器|更具类名来获取元素|$(".username")
```
备注:
```
### 2. 层级选择器
|名称|说明|语法|
|:-----------|:-----------|:-----------|
|后代选择器|A B ,表示获取A元素内部的所有B元素|$("#ul li")|
|父子选择器|A>B ,获取A元素内部子级标签中所有的B元素,**不包括孙子**|$("#ul > li")|
|下级选择器|A+B ,获取和A元素平级的下一个B元素|$("#ul+li")|
|下下级选择器|A~B ,获取和A元素平级的下面**所有**B元素|$("#ul~li")|
```
    备注:
```
### 3. 基本过滤器
|名称|说明|语法|
|:-----------|:-----------|:-----------|
|:first|获得当前元素中的**第一个**元素| $("#spnMove:animated")|
|:last|获得当前元素中的**最后**一个元素| $("#spnMove:animated")|
|:even|获得当前元素中索引号为**偶数**的元素,索引从0开始|
|:odd|获得当前元素中索引号为**奇数**的元素,索引从0开始|
|:eq(inedx)|获取**指定**索引的元素,索引从0开始|
|:gt(index)|获取**大于**给定索引的元素,索引从0开始|
|:lt(index)|获取**小于**给定索引的元素,索引从0开始|
|:header|获取标题类型的元素|
|:animated|获取正在执行的d动画效果元素|
```
    备注:
```

### 4. 属性选择器(不需要**":"**)
|名称|说明|语法|
|:-----------|:-----------|:-----------|
|[属性名]|获得有该属性名的元素|$("#abs[type]"),获得由type属性的元素|
|[属性名=值]|获得属性名等于值的元素|
|[属性名!=值]|获取属性名不等于值的元素|
|[属性名^=值]|获取属性名以值开头的元素|
|[属性名$=值]|获取属性名以值结尾的元素|
|[属性名*=值]|获取属性名含有值的元素|
|[以上选择器][xx][xx]|复合属性选择器,多个属性同时过滤返回满足所有的结果|
```
备注:
```

### 5. 表单对象选择器
|名称|说明|语法|
|:-----------|:-----------|:-----------|
|:enable|匹配所有元素|
|:disable|匹配所有不可用元素|
|:checked|选取匹配所有被选中的元素(单选框,多选框等)|$('input[type=checkbox]:checked')
|:selected|选取所有被选中项元素(下拉框)|
```
    备注:
```


## 五. Jquery对象的属性和方法
### 1. attr和prop,设置对象属性
|名称|说明|语法|
|:-----------|:-----------|:-----------|
|attr(属性名)|用来获取一个属性的值|
|attr(属性名,属性值)|用来设置一个属性对应的值|
|attr({属性名1:属性值1,xx})|设置多个属性,json格式|
|removeAttr(属性名)|删除某个格式|
|prop格式同上!|

- 一个标签中的checked属性有返回值并且返回boolean类型
    - 只要checked里面有值,那么它就返回为true
    - prop可以设置属性的返回值(只要checked属性纯在,就返回true,设置成false就相当强删除该元素),也可以设置属性的value值;
```
    备注:**prop用来操作dom对象所包含的属性**,
            **attr用来操作自定义的属性**
```



### 2.常用的方法
|名称|说明|语法|
|:---|:---|:---|
|val()|通常用来操作标准的表单对象|用来获取value和设置value的值|$('#xxx').val('[xxx]');
|val|打印一个构造,没有意义|没意义哦|
|text()|获取或改变指定元素的文本|$("xx").text()|
|html()|获取或改变指定元素的html元素|$("xx").html|


### 3.Jquery的绑定事件和解绑时间
#### 3.1.1 one 绑定一次

**`语法格式:one(eventTypes,[data],fn)`**
```
String,Object,Function 
enecTypes:事件类型,可一可多;
data:要传递的数据,可省略;
fn:事件触发时执行的参数;
```

#### 3.1.2  delegate 方式(个孩子绑定)
**`语法格式:delegate( selector, eventTypes, fn )`**
```
selector:子选择器
evenTypes:事件类型,可一可多;
fn:事件触发时执行的参数;
```

#### 3.1.3 on 方式(1.7后,就只有这种方式了)
**`语法格式:on(eventTypes,[selector],[data],fn)`**
```
evenTypes:事件类型,可一可多;
selecotr:子选择器,可以省略;
data:要传递的数据,可以省略;
fn:时间触发时要执行的函数;
```
#### 3.1.4 bind方式
**`bind(evenTypes,[data],fn);`**
```
evenType:事件类型,可一可多
data:要传递的数据参数,可省略
fn:触发事件时要执行的参数

Jq对象.bind(事件名,回调函数)
jq对象.bind({事件名:回调函数,事件名:回调函数});
```

#### 3.2.1  unbind 解绑 bind事件
**`unbind(); -- 解除绑定所有事件`**
**`ubbind(eventType) -- 解除绑定所有事件`**
```
Jq对象.unbind("" "" ""); 中间以空格隔开
```

#### 3.2.2 undelegate 解绑 delegate事件
**`undelegate(); -- 解绑所有的delegate事件`**
**`undelegate(evenType); -- 解绑指定的事件`**

#### 3.2.3   off 解绑 on 事件,1.7之后就只有这个了
**`off(); -- 解绑所有事件`**
**`off(evenTypes,count); -- 解绑所有指定事件,count代表数量,但是不是Number类型的参数,"**"代表全部`**


### 六.Jqyery动画
太多,未完待续吧....

### 七.遍历
#### 1. JQuery.each(data,fn)
```
//遍历函数
        //index表示索引名,element表示元素名
        $("ul li").each(function(index,element){
            //this表示当前的
            // console.log($(this).html())
        });
```
#### 2. 链式方法
|函数	|描述|
|:-----|:-----|
|.add()	|将元素添加到匹配元素的集合中。|
|.andSelf()	|把堆栈中之前的元素集添加到当前集合中。|
|.children()|	获得匹配元素集合中每个元素的所有子元素。|
|.closest()|	从元素本身开始，逐级向上级元素匹配，并返回最先匹配的祖先元素。|
|.contents()|	获得匹配元素集合中每个元素的子元素，包括文本和注释节点。|
|.each()|	对 jQuery 对象进行迭代，为每个匹配元素执行函数。|
|.end()	|结束当前链中最近的一次筛选操作，并将匹配元素集合返回到前一次的状态。|
|.eq()|	将匹配元素集合缩减为位于指定索引的新元素。|
|.filter()|	将匹配元素集合缩减为匹配选择器或匹配函数返回值的新元素。|
|.find()	|获得当前匹配元素集合中每个元素的后代，由选择器进行筛选。|
|.first()	|将匹配元素集合缩减为集合中的第一个元素。|
|.has()	|将匹配元素集合缩减为包含特定元素的后代的集合。|
|.is()|	根据选择器检查当前匹配元素集合，如果存在至少一个匹配元素，则返回 true。|
|.last()|	将匹配元素集合缩减为集合中的最后一个元素。|
|.map()|	把当前匹配集合中的每个元素传递给函数，产生包含返回值的新 jQuery 对象。|
|.next()|	获得匹配元素集合中每个元素紧邻的同辈元素。|
|.nextAll()	|获得匹配元素集合中每个元素之后的所有同辈元素，由选择器进行筛选（可选）。|
|.nextUntil()|	获得每个元素之后所有的同辈元素，直到遇到匹配选择器的元素为止。|
|.not()	|从匹配元素集合中删除元素。|
|.offsetParent()|	获得用于定位的第一个父元素。|
|.parent()|	获得当前匹配元素集合中每个元素的父元素，由选择器筛选（可选）。|
|.parents()	|获得当前匹配元素集合中每个元素的祖先元素，由选择器筛选（可选）。|
|.parentsUntil()|	获得当前匹配元素集合中每个元素的祖先元素，直到遇到匹配选择器的元素为止。|
|.prev()	|获得匹配元素集合中每个元素紧邻的前一个同辈元素，由选择器筛选（可选）。|
|.prevAll()|	获得匹配元素集合中每个元素之前的所有同辈元素，由选择器进行筛选（可选）。|
|.prevUntil()	|获得每个元素之前所有的同辈元素，直到遇到匹配选择器的元素为止。|
|.siblings()|	获得匹配元素集合中所有元素的同辈元素，由选择器筛选（可选）。|
|.slice()	|将匹配元素集合缩减为指定范围的子集。|


### 八.表单校验
#### 1. 格式
```JavaScript
 $("#form1").validate({
            rules:{             
            },
            messages:{              
            }
        });
```
<a href="http://www.runoob.com/jquery/jquery-plugin-validate.html">菜鸟详细教程</a>
#### 2. 自定义校验
```JavaScript
$.validator.addMethod("自定义校验规则名字",function(value,element,params){
		value 校验输入框的值
		element 校验框
		params 表示校验规则时传递的实际值
	
	},错误信息);



    /*
        自定义校验规则，用来验证密码。长度6-12字符或数字,不能包含中文字符
        addMethod()方法具有三个参数：
            name:表示校验规则的名字，例如这里是 pwdFormat
            method:表示处理自定义校验规则代码，这里需要给回调函数
                function(value,element,params){
                        回调函数三个参数：
                        value:输入框输入的值，即要验证的值
                        element:表示要验证的输入框标签对象
                        params:使用校验规则时传递的实际参数 。 [6,12] 如果使用时传递的是多个值，
                                那么params就是数组，如果是单个值params就是变量
                }
                关于回调函数补充：如果满足校验规则，返回true，可以提交表单。不满足返回false，不可以提交表单
            message:提示的错误信息，可以不书写，如果书写，那么在使用错误信息的时候就可以不书写了
        */
    $.validator.addMethod("pwdFormat",function (value,element,params) {
        //定义正则
        // var reg=/^[0-9a-zA-Z]{6,12}$/;
        var reg=new RegExp("^[0-9a-zA-Z]{"+params[0]+","+params[1]+"}$");
        //判断
        if(reg.test(value))
        {
            //说明合法
            return true;//可以提交表单
        }else
        {
            //说明不合法
            return false;//不能提交表单
        }

    },"长度{0}-{1}字符或数字,不能包含中文字符");


```

### 九.元素操作
#### 1. 元素追加
##### 1.1 append() 
- 追加到元素内部的最后面位置
#####    1.2 prepend()
- 追加到元素内部最前面位置
#####    1.3 appenTo() , prependTo() 
-和上面两种相反
```JavaScript
      // 需求：创建一个li标签并且使用append追加到ul的最后面
        /*
            在jq中创建标签：$("创建的标签");
            $("<li>222</li>") 创建li标签
         */
        $("ul").append($("<li>222</li>"));
        // 需求：创建一个li标签并且使用prepend添加到ul的最前面
        $("ul").prepend($("<li>000</li>"));

        // 需求： 创建一个li 并且使用appendTo添加到ul的最后面
        $("<li>333</li>").appendTo($("ul"));
        // 需求： 创建一个li 并且使用prependTo添加到ul的最前面
        $("<li>-1-1-1</li>").prependTo($("ul"));
		
		  // 需求一：创建一个li使用after插入到bbb的后面  <li>bbb</li>
        // $("#bbb").after($("<li>ccc</li>"));
        // // 需求二：创建一个li使用before插入到bbb的前面
        // $("#bbb").before($("<li>aaa</li>"));
        // 需求三：创建一个li使用insertAfter插入到bbb的后面
        $("<li>ccc</li>").insertAfter($("#bbb"));
        // 需求四：创建一个li使用insertBefore插入到bbb的前面
        $("<li>aaa</li>").insertBefore($("#bbb"));
```
#### 2. 元素清空和删除案例
##### 2.1 remove()
- 删除该元素
##### 2.2 empty()
- 清空元素内部
```
 //需求1：删除div
        $("div").remove();
        //需求2：清空ul
        $("ul").empty();
```