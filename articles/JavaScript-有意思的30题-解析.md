## JavaScript-有意思的30题-解析

茶余饭后，来杯咖啡。

### 1.D

map对数组的每个元素调用定义的回调函数并返回包含结果的数组。["1","2","3"].map(parseInt)对于数组中每个元素调用paresInt。但是该题目不同于：

    function testFuc(a){
        return parseInt(a);
    }
    console.info(["1","2","3"].map(testFuc));

题目等同于：

    function testFuc(a,x){
        return parseInt(a,x);
    }
    console.info(["1","2","3"].map(testFuc));

map中回调函数的语法如下所示：function callbackfn(value, index, array1)，可使用最多三个参数来声明回调函数。第一参数value，数组元素的值；第二个参数index，数组元素的数组所以；array1，包含该元素的数组对象。
因此，题目等同于[parseInt(1,0),parseInt(2,1),parseInt(3,2)] 最终返回[1, NaN, NaN]。

### 2.A

typeof用以获取一个变量或者表达式的类型，typeof一般只能返回如下几个结果：

    number,boolean,string,function（函数）,object（NULL,数组，对象）,undefined

instanceof 表示某个变量是否是某个对象的实例，null是个特殊的Object类型的值 ，表示空引用的意思 。但null返回object这个其实是最初JavaScript的实现的一个错误， 
然后被ECMAScript沿用了，成为了现在的标准，不过我们把null可以理解为尚未存在的对象的占位符，这样就不矛盾了 ，虽然这是一种“辩解”。
对于我们开发人员 还是要警惕这种“语言特性”。最终返回：["object", false]

### 3.A

pow() 方法可返回 x 的 y 次幂的值。[3,2,1].reduce(Math.pow);等同于：

    function testFuc(x,y){
        console.info(x +" : "+y);
        return Math.pow(x,y);
    }
    console.info([3,2,1].reduce(testFuc));

执行Math.pow(3,2)和Math.pow(2,1)，最终返回9和9。
但是要注意pow的参数都是必须的，[].reduce(Math.pow)，等同于执行Math.pow();会导致错误。

### 4.A

先执行字符串拼接，再执行校验

    var val = 'value';
    console.info('Value id '+(val === 'value123')?'Something':'Nothing');

同样会返回something

### 5.A

判断语句被包裹在立即调用函数里，函数内部无法访问外部值为'World'的name，因此typeof name === 'undefined'为真，执行下一步操作，最终输出Goodbye Jack

### 6.D

END = 9007199254740992 ,START = 9007199254740892目的是计算的END和START之间的差。但是2的53次方计算出的结果由于精度问题使得i++失效。

### 7.C

filter会接触到没有被赋值的元素，即在arr中，长度为10但实际数值元素列表为[0, 1, 2, 10]，因此，最终返回一个空的数组[]。

### 8.C

两个浮点数相加或者相减，将会导致一定的正常的数据转换造成的精度丢失问题eight-six = 0.20000000000000007。
JavaScript中的小数采用的是双精度(64位)表示的，由三部分组成：　符 + 阶码 + 尾数，在十进制中的 1/10，在十进制中可以简单写为 0.1 ，但在二进制中，他得写成：0.0001100110011001100110011001100110011001100110011001…..（后面全是 1001 循环）。因为浮点数只有52位有效数字，从第53位开始，就舍入了。这样就造成了“浮点数精度损失”问题。 
更严谨的做法是(eight-six ).totoFiexd(1)或者用用Math.round方法回归整数运算。判断两个浮点数是否相等，还是建议用逼近的比较，比如if((a-b) < 1E-10)

### 9.C

使用new String()使用构造函数调用讲一个全新的对象作为this变量的值，并且隐式返回这个新对象作为调用的结果，因此showCase()接收的参数为String {0: "A"}为不是我们所认为的“A”

### 10.A

直接调用String("A")创建的变量和"A"无异。

    var a="123"    '只是设置变量
    b=new String('123') '设置一个成员
    
    var a="123";
    a.sex=1;
    alert(a.sex);//输出未定义，因为不是成员，没有这属性
    
    b=new String('123');
    b.sex=1;
    alert(b.sex);//输出1，成员的属性

### 11.C

    function isSane(num){
        return isEven(num)||isOdd(num);
    }
该函数判断num是否为正整数，'13'被强制转换为数值13，-9%2结果为-1，Infinity %2为NaN

### 12.D

最终结果为[3, NaN, 3]；
parseInt() 函数可解析一个字符串，并返回一个整数。当参数 radix 的值为 0，或没有设置该参数时，parseInt() 会根据 string 来判断数字的基数。

### 13.A

Array.prototype为[]，Array.isArray(a)是一个判断a是否为数组的方法。
判断对象是否为数组的方法：
1）ES5函数isArray(),该函数测试对象的内部[[Class]]属性是否为Array:Arrray.isArray(a);
2）判断对象的构造函数是否为Array:a.constructor === Array
3）使用对象内部[[Class]]属性创建结果字符串：Object.prototype.toString.call(a)
4）使用instanceof操作符测试对象是否继承自Array：（但由于，一个页面的iframe不会继承自另外一个页面的iframe，该方法不可靠）

    a instanceof Array

### 14.B

在if条件判断语句相对于==比较宽松中，只要if(n)，n不为null，0，undefined数值，都会转换为true。进入console.info(a == true);最终返回false。

### 15.B

数组，在 Javascript 中是对象，对象使用 == 比较都是比较的引用。简单的说，就是，如果是同一个对象，就相等，如果不是同一个对象，就不等。每次使用 [] 都是新建一个数组对象，所以 [] == [] 这个语句里建了两个数据对象，它们不等。

### 16.A

执行'5'+3，加号具备拼接字符串功能，会将3强制转换为字符串'3'和'5'拼接。
执行'5'-3，减号只具备数值运算的功能，因此会将'5'转化为数值，进行5-3的数值运算

### 17.C

### 18.D

区分赋值和声明。虽然var arr = Array(3);创建一个长度为3的数组，但是实际只有一个元素被赋值，因此arr的实际长度为1，即最终参与map的只有一个元素，返回[1]

### 19.D

按照执行步骤，无需多疑，最终结果为10+1+10

### 20.B

js的精确整数最大为:Math.pow(2,53)-1 =9007199254740991.
var a = 111111111111111110000,
 max = 9007199254740992;
a的值大于javascript所能表示的最大整数精度，因此和任何数值相加将会导致失真。

### 21.C

 正确调用方式为x.call([])

### 22.A

Number.MIN_VALUE表示的最小值为5e-324，MIN_VALUE代表的并不是负最小，而是最接近0的一个数，因此Number.MIN_VALUE>0。
负最小值可以使用-Number.MAX_VALUE表示。

### 23.A

1<2，返回true，执行true<3，会强制将true转换为1，1<3，最终返回true。
3<2，返回false，执行false<1,会强制将false转换为0，0<1，最终返回true。

### 24.A

使用a==b判断a和b对象是否相等，可以会将b对象强制转换为a对象的类型，即执行2 == [[[2]]]，会隐式调用parseInt([[[2]]])将[[[2]]]强制转化为数字基本量，即2，2==2，最终返回true。

### 25.C

Number中的toString(a)，能够将数值转化成为a进制的值。但a缺省时，默认转化为十进制。
一般使用方法为：var n = 3;3.toString();
执行3.toString()，因为3只是为数值型变量，为非Number实例，因此对于3不能直接调用Number方法。而执行3..toString()，会强制将3转化为数字实例，因此能够被解释，输出3，同样可以使用(3).toString()。

### 26.C

在立即调用函数内执行，var x1 =y1 =1;创建局部变量x1和全局变量y1。函数外部试图访问函数内部的变量，将会导致错误。

### 27.

列举IE和FF脚本兼容性的问题
（1）window.event
表示当前的事件对象，IE有这个对象，FF没有
（2）获取事件源
IE用srcElement获取事件源，而FF用target获取事件源
（3）添加、移除事件
IE：element.attachEvent("onclick",function)
element.detachEvent("onclick",function)
FF:element.addEventListener("click",function,true)
element.removeEventListener("click",function,true)

### 28.

题目所给出的代码，除了有addEventListener不兼容IE浏览器的问题之外，最突出的一个问题是：
虽然在页面上会显示值为button+i的按钮，但是点击任意一个按钮，最终都会显示5。
要想点击相关按钮，弹出相应的1，2，3，4，5的值，需要理解闭包原理实现和使用立即回调函数。修改后的代码如下：
    

    function initButtons(){ 
        var body = document.body,button,i;       
        for(i =0;i<5;i++){ 
            (function(i){ 
                 button = document.createElement("button"); 
                 button.innerHTML = "Button" + i; 
                 button.addEventListener("click",function(e){ 
                      alert(i); 
                 },false); 
                 body.appendChild(button); 
              })(i);                     
        }        
    } 
    initButtons(); 

涉及绑定和赋值得到区别。在运行时，进入一个作用域，javascript会为每一个绑定到该作用域的变量在内存中分配一个“槽”（slot）。函数中，创建变量document.body,button,i，因此当函数体（创建按钮，并为按钮绑定事件）被调用时，函数会为每个变量分配一个“槽”。在循环的每次迭代中，循环体都会为嵌套函数分配一个闭包。我们可能理解为，该函数存储的是嵌套函数创建时变量i的值。但事实上，他存储的是i的引用。由于每次函数创建后变量i的值都发生变化，因此函数内部最终看到的是变量i的引用。闭包存储的是外部变量的引用而非值。
立即调用的函数表达式，是一种不可或缺的解决javascript缺少块级作用域的方法。
需要深入理解，可以查看《Effective JavaScript》第13条：使用立即调用的函数表达式创建局部作用域

### 29.使用常规方法和正则表达式匹配两种算法  

    /*写一段代码。判断一个字符串中出现次数最多的字符串，并统计出现的次数*/      
            function toGetTheMostCharsByArray(s){ 
                var r={}; 
                for(var i=0;i<s.length;i++){ 
        
                    if(!r[s[i]]){ 
                        r[s[i]] = 1; 
                    }else{ 
                        r[s[i]]++; 
                    } 
                } 
                var max = { 
                    "value "　:s[0], 
                    "num" :  r[s[0]] 
                }; 
                 
                for(var n in r){//对象使用in关键字，因为没有length 
         
                    if(r[n]>max.num){ 
                        max.num = r[n]; 
                        max.value = n;  
                    } 
                } 
                return max; 
            } 
            function toGetTheMostCharsByRegex(s){ 
                var a = s.split(''); 
                a.sort(); 
                s = a.join(''); 
         
                var regex = /(\w)\1+/g ;//\1代表重复的 
         
                var max = { 
                    "value "　:s[0], 
                    "num" :  0 
                }; 
                 
                s.replace(regex,function(a,b){ 
                    if(max.num < a.length){ 
                        max.num = a.length; 
                        max.value= b; 
                    } 
                }); 
         
                return max; 
         
            } 
            var test = "efdfssssfrhth"; 
            console.info("使用常规方法　，出现最多的字符串为："+toGetTheMostCharsByArray(test).value+" ，出现次数："+toGetTheMostCharsByArray(test).num); 
            console.info("使用字符串匹配，出现最多的字符串为："+toGetTheMostCharsByRegex(test).value+" ，出现次数："+toGetTheMostCharsByRegex(test).num); 

### 30.

javascript的引擎是单线程的
javascript的引擎是基于事件驱动的
setTimeout和setInterval都是往事件队列中增加一个待处理时间而已，setTimeout只触发一次，而setInterval是循环触发

    setTimeout(function(){
        //代码块
        setTimeout(arguments.callee,10);
    },10);

上段代码可使得setTimeout循环触发。但是，执行完这段代码块才挂起时间，所以两次执行时间会大于10毫秒

    setInterval(function(){
        /*代码块*/
    },10);

而上段代码，是自动在10的时候挂上这个事件，所以两次事件的相隔会小于等于10毫秒。
当线程阻塞在一个事件的时候，不管是使用setInterval还是setTimeout都需要等待当前事件处理完才能执行。

    完

声明：本文源于学习的时候保存的一份word文档。word复制自一篇博客“JavaScript30个你不可能全会的题目”，具体出处已无。