# ES6 全称ECMA           
> ECMA 是一种标准,
> ES6 是JS实现的ECMA标准


> tx39标准制定委员会




## ★ 1. 关于定义变量
- 之前定义定义变量的方式 
    - var:局部和全局变量不明确,初始化时赋值不方便,应为var申明的变量,声名过程和赋值过程是不同步的

- ES6支持的 let const
    - let是定义变量
    - const是定义常量(不可修改)
    - 都是块级作用域
        ```JavaScript
        {
            块级作用域
        }
        ```





### ★1.1 let

- 暂时性死区 TDZ
```javascript
    let a = 12;
    function fn(){
        alert(a); //TDZ:暂时性死区 会报错的哦
        let a = 12;//TDZ:死区结束
    }
```
**在代码块内,只要let定义变量,在定义之前使用,无论全局是否有相同名称变量们都会报错**

> TDZ死区内的所有变量啊啥的,都会报错,不能再死区写东西的


- 不能重复定义变量

``` javascript
    let a =9;

    let a =1;

    alert(a);
```
> 会报错的
> **在同一个代码块中,不能使用let定义重复变量**


- 作用域定级

    ```javascript
            let a = 19;
            {
                let a = 11;
                alert(a);
                {
                    let a = 10;
                    alert(a);
                }
            }
            
            alert(a);
    ```
    > 不同级别作用域可以存在相同变量

    ```javascript
        for (let i = 0; i < 10; i++) {
            let i = 11;
            alert(i);
        }
    ```
    > for括号中的i和大括号中的i 他们不属于一个作用域级别
    >> 不仅仅是for 还有其他的
    >> while(){}
    >> if(){}
    >> ... 等



### ★1.2 const 
> const 是作用域和let一样,但是其拥有不可修改的属性
> 修改会报错
- const变量定义完必须有值,不能后赋值



- const定义对象
    ```javascript
    const arr = ['apple','blanana'];
    arr.push(' orange');
    console.log(arr);

    output:
    (3) ["apple", "blanana", " orange"]
    ```

    > const定义对象,对象内是可以修改的

- 定义一个不可修改的对象
    ```javascript
    const arr2 = Object.freeze(['apple','blanana']);
    arr2.push(' orange');
    console.log(arr2);

    output:
    报错
    ```

    > Object.freeze定义的对象会被冻结,无法修改



## ★ 2. 结构赋值
- 解析数组

    ```javascript

        let arr = ['1','2','3'];
        console.log(arr[0],arr[1],arr[2]);

        let [a,b,c] = ['1','2','3'];
        console.log(a,b,c);

    ```

> 两种打印结果相同 ,第二种就是结构赋值 **别名对应数据**
>> **注意** : 左右两边的结构和格式要保持一致


### ★2.1 解析JSON
    ```javascript
        //1
        let json={
            name:"Tom",
            age:18,
            job:"码畜"
        }

        //2
        let {name,age:a,job}=json;

        console.log(name,a,job)

        output:
        Tom 18 码畜
    ```
    > 其中,步骤2是解析器,解析器的变量名和格式要和源数据中的相同,**:** 可以起到声名别名的作用




### ★2.2 结构不同时给默认值
    ```javascript
       let [a,b="暂无数据",c="暂无数据"] = ['1','2'];
        console.log(a,b,c);


        output:
        1 2 暂无数据


         let [a,b="暂无数据",c="暂无数据"] = ['1','2',];
    ```

    > 如果对应的位置没有值或者值为undefined ,就会启用默认值
    > 也可以加入判断,并且赋值

### ★2.3 解构函数的返回值
    ```javascript
    function good(){
        return {left:10,right:20}
    }

    let {left,right} = good();

    console.log(left,right);    
    ```



### ★2.4 解构函数参数

```javascript
  function good({a,b}){
        console.log(a,b)
    }
    
    good({
        a:555,
        b:666
        })

    output:
    555 666

```

> 函数中的参数也可以给默认值 


## ★3.字符串模板

> `` : 字符串模板(可以随便换行哦)

### ★3.1 字符串拼接变量


```javascript
let a = '好好学习';

    let b ='天天向上';

    let str = `${a},${b}`;

    console.log(str);
```
> 在拼接是使用``号包围该字符串,然后采用${}的形式写入变量


### ★3.2 拼接DOM元素
```javascript
let data = [
        {title:"煞笔卵子1",read:1001},
        {title:"煞笔卵子2",read:1002},
        {title:"煞笔卵子3",read:1003},
        {title:"煞笔卵子4",read:1004},
        {title:"煞笔卵子5",read:1005},
        {title:"煞笔卵子6",read:1006}
    ];
   
        window.onload=function(){
            let aa = document.querySelector("#aa");
          
            console.log(aa);

            for (let index = 0; index < data.length; index++) {
                let span = document.createElement("span");
                span.innerHTML=`<span>${data[index].title}</span>
                                 <span>${data[index].read}</span><br/>`;
                aa.appendChild(span);
              }
        }
```

> 拼接更方便



### 3.3 字符串查找

- String.indexOf("str");
    - 返回的是str在该字符串中的启示位置
```javascript
    let a = 'apple iphone xiaomi huawei';
    console.log(a.indexOf('xiaomi'));

    output:
         13   
```

- String.includes("str") 
    - 判断是否包含,返回boolean类型
```javascript
    let a = 'apple iphone xiaomi huawei';
    console.log(a.includes('huawei'));

    output:
            true
```


- String.startWith("str") $$ String.endWith("str")
    - 返回boolean 判断是否以str开头或结尾


- String.repeat(int)
    - 返回一个String类型, 将String值重复int遍并返回

```javascript
let a = 'apple iphone xiaomi huawei';
    console.log(a.repeat(3));


    output:
        apple iphone xiaomi huaweiapple iphone xiaomi huaweiapple iphone xiaomi huawei
```

> 只能为正整数





## 4. 函数

### 4.1 非解构函数默认参数
```javascript
function show(a="默认a",b="默认b"){
        console.log(a,b)
    }

    show("欢迎");

    output:
            欢迎 默认b
```

### 4.2 解构函数默认参数

```javascript
function show2({x="默认x",y="默认y"}={}) {
        console.log(x,y)
    }

    show2({x:"欢迎",y:"光临"})
    show2({y:"光临"});
    show2({});
    show2();
    
```

> 如果未传值,在末尾加上={}


### 4.3 函数变化
- 函数的参数是变量,作用域是整个函数的范围,所以不能定义和参数相同的便令


## 数组,对象