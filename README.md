# velocity:1.2.3

---


> Velocity.js是一款动画切换的jQuery插件，它重新实现了jQuery的$.animate()方法从而加快动画切换的速度。Velocity.js只有7k的大小，它不仅包含了$.animate()的所有功能，并且还包含了颜色切换、转换(transform)、循环、缓动、CSS切换、Scroll功能，它是jQuery、 jQuery UI、CSS变换 在动画方面的最佳组合。Velocity.js支持IE8+、Chrome、Firefox等浏览器，并支持Andriod以及IOS。Velocity.js在内部实现中使用了jQuery的$.queue()方法，因此它比 jQuery的$.animate()、$.fade()、$.delay()方法更加流畅，其性能也高于CSS的animation属性
> 



## 安装与使用
	
	fis install huya-fed/velocity

	//基于jq和zepto的话
	require('velocity');
	$('#a').velocity( { width: "500px" },1000);

	//假如没有jq和zepto的话
	var Velocity = require('velocity');
	Velocity(document.querySelector('#a'),{ width: "500px" },1000)
	


## 基本概况

	$element.velocity({ 
	   width: "500px",
	   property2: value2
	}, {
	 /* 以下如果没有做特别设置的话都是默认属性 */
	 duration: 400, // 一系列动作在多少时间内完成
	 easing: "swing", //动画加速度
	 queue: "", //动画队列，要必须等某个动作执行完后才能执行某个动作。
	 begin: undefined, //动画执行前，执行此函数
	 progress: undefined,//动画执行进度
	 complete: undefined,//动画执行后，执行此函数
	 display: none, //动画执行完后，隐藏。配合opacity: 0使用就是淡入淡出效果
	 visibility: hidden,//动画执行完后，隐藏。配合opacity: 0使用就是淡入淡出效果
	 loop: true / 2, //循环次数，完成动作之后在按原来动作后退，算一次。
	 delay: 2000,//动作多少秒后开始执行
	 mobileHA: true
	});

### velocity语法也可以像jquery语法一样

	$element.velocity({ top: 50 }, 1000);
	$element.velocity({ top: 50 }, 1000, "swing");
	$element.velocity({ top: 50 }, "swing");
	$element.velocity({ top: 50 }, 1000, function() { alert("Hi"); });


### 属性
	 
同时你只能为每一个CSS属性设置一个值，所以 “padding:’10px 15px’”这样的设置是不合法的。替代做法是单独设置每一个分属性：{paddingLeft:”10px”,paddingTop:”15px”,….}。这样不仅表达清晰，而且意味着你可以特别指定每一个css分属性的值，而不是只设置一个整体的，给予了你足够的空间定制动画。

Note: 有多个单词描述的CSS属性（比如font-size和padding-left）必须使用骆驼拼写法表示（fontSize && paddingLeft）.


如果CSS属性值没有给定确切的单位，那么时间默认是：ms，长度默认是 px，角度默认是 deg。为了表达清晰最好还是显示注明单位，那样以后再看代码时也容易理解。如果有一个值不仅仅由数字表示，那么必须加引号。比如：duration:500（默认是500ms）是合法的，但是duration:500ms就不合法，必须加上引号：duration:”500ms”.

JS动画运许传入简单的表达式作为CSS的属性值，但这些表达式只限于：+=,-=,*=,/=，表示目标值在其本来的值的基础上加多少，减多少，乘多少，除多少，任何其他的表达式是不允许的。实例：

	$element.velocity({
    	width: "+= 50px", // Adds 50px to the current width value
    	eight: "/= 2" // divides the current height value by two
    });

表示动画完成后width比原值大50px,height变为原来的二分之一。


### 链式操作

Velocity的链式调用即在同一个元素上一个动画完成之后马上进入另一个动画，这样一个一个的执行下去：

	$element
	   .velocity({ width: "500px", height: "300px"})
	   .velocity({ opacity: 0 });

等价于：

	$element.velocity({ width: "500px", height: "300px"});
	$element.velocity({ opacity: 0 });

就如之前提到的一样，这允许你不需要任何手动的计算,就能使那些复杂的、有时间限制的多级动画如当初计划的一样,一个接一个的执行。

### 速度参数

你可以像jq那样使用速度参数 slow、normal、fast

	$element.velocity({ opacity: 1 }, { duration: 1000 });
	or
	$element.velocity({ opacity: 1 }, { duration: "slow" });

### 加速度设置
	
	//CSS3's named easings: "ease", "ease-in", "ease-out", and "ease-in-out".
	$element.velocity({ width: 50 }, "easeInSine");
    //贝塞尔曲线速度
	$element.velocity({ width: 50 }, [ 0.17, 0.67, 0.83, 0.67 ]);
	//一个张弛度（默认500）和一个摩擦系数（默认值20）作为他的参数，较高的张弛度将提高整体速度和动画的反弹力度，较小的摩擦系数将提高动画结束时的速度。（较大的摩擦系数将使动画快速减速）。
	$element.velocity({ width: 50 }, [ 250, 15 ]);
	//分成8块进行
	$element.velocity({ width: 50 }, [ 8 ]);


	$("div").velocity({ translateY: 125 }, {
	  duration: 1150,
	  easing: [ 300, 8 ]
	});


也可以单独的为某一个属性设置变化加速的效果。

	$element.velocity({
	 	borderBottomWidth: [ "2px", "spring" ], // Uses "spring"
	 	width: [ "100px", [ 250, 15 ] ], // Uses custom spring physics
	 	height: "100px" // Defaults to easeInSine, the call's default easing
	}, { 
	 	easing: "easeInSine" // Default easing
	});


也可以自己自定义加速效果

- p:百分比（十进制值）调用的完成百分比
- opts：Velocity初始时传递的参数
- tweenDelta：动画开始到结束的差值（可选）

	$.Velocity.Easings.myCustomEasing = function (p, opts, tweenDelta) {
	 	return 0.5 - Math.cos( p * Math.PI ) / 2;
	};


### queue队列

队列可以设置为false来迫使一个新的动画立即调用并行运行与任何currenty-active动画


	$('#a').velocity({ width: "500px" }, { duration: 3000 });

	setTimeout(function() {
	    $('#a').velocity({
	      height: 500
	    }, {
		  //可以尝试注释掉这个看看效果
	      queue: false,
	      duration: 1500
	    });
	}, 1500);


queue动画队列写法，例如：
如果动画中的queue属性一旦设置了值，动画就不会执行。

	$("div")
		 .velocity({ translateX: 75 }, { queue: "a" }) //给动画queue命名 'a'
		 .velocity({ translateY: 75 }, { queue: "a" })
		 .velocity({ width: 50 }, { queue: "b" })
		 .velocity({ height: 0 }, { queue: "b" })

	$("div").dequeue("a"); //需要dequeue调用才能执行a的动画，而且是按先后顺序执行的
	//没有jq的话
	Velocity.Utilities.dequeue(element(s), "queueName") 



### 动画前执行

	$element.velocity({
	 	opacity: 0
	}, { 
	 	/* 执行动画前 记录所有动画 节点 数量 */
	 	begin: function(elements) { console.log(elements); }
	});

### 动画结束后执行

	$element.velocity({
	 	opacity: 0
	}, { 
	 	/*执行动画后 记录所有动画 节点 数量 */
	 	complete: function(elements) { console.log(elements); }
	});



### 设置进度


	$element.velocity({
	 opacity: 0,
	 tween: [ 10, 20 ] // 设置tweenValue数值10到20
	}, {
	 duration: 100,
	 progress: function(elements, complete, remaining, start, tweenValue) {
	 elements //每一个关键帧的这个对象
	 console.log((complete * 100) + "%"); //这动画完成的进度过程0%-100%
	 console.log(remaining + "ms remaining!"); //这动画剩余多少时间完成100ms到0ms
	 console.log("The current tween value is " + tweenValue)//自定义动画进度数值由20开始逐渐    到10。
	 }
	});


### 推荐在移动设备开启true

	$element.velocity(propertiesMap, { mobileHA: false })

### 循环次数

	$element.velocity({ height: "10em" }, { loop: 2 });
	$element.velocity({ height: "10em" }, { loop: true }); //无限循环

### 每个动画间隔时间delay

	$element.velocity({ 
	 height: "+=10em"
	}, { 
	 loop: 4,
	 delay: 100
	});
	

### fadeIn and fadeOut

	//代替fadeOut()
	$element.velocity({ opacity: 0 }, { display: “none” }); //淡出 文档流不存在
	$element.velocity({ opacity: 0 }, { visibility: “hidden” }); //淡出 文档流存在
	//fadeIn()
	$element.velocity({ opacity: 1 }, { display: “block” });

	或者
	$element
	.velocity(“fadeIn”, { duration: 1500 })
	.velocity(“fadeOut”, { delay: 500, duration: 1500 });

### slideUp and slideDown

	$element
	 .velocity("slideDown", { duration: 1500 })
	 .velocity("slideUp", { delay: 500, duration: 1500 });


### scroll参数（效果只兼容IE10以上）


	$element
	 	.velocity("scroll", { duration: 1500, easing: "spring" });
	 	//滚动条滚动到元素顶部

	例如：
	$(“#element3″).velocity(“scroll”, {
		container: $(“#container”),
		duration: 800,
		delay: 500
	});
	//#container是可滚动的区域css要有position: relative;
	//#element3在#container内容中


	/* 默认是Y轴的，可以通过设置成X轴 */
	$element.velocity("scroll", { axis: "x" });

	$element
    	/* Then scroll to a position 250 pixels BELOW the div. */
    	.velocity("scroll", { duration: 750, offset: 250 })
    	/* Scroll to a position 50 pixels ABOVE the div. */
    	.velocity("scroll", { duration: 750, offset: -50 });

	/* 滚动整一个页面的值，mobileHA需要设置成false，已避免在ios的bug */
	$("html").velocity("scroll", { offset: "750px", mobileHA: false });


### 取代stop()

	stop() == .velocity(“stop”); //当前动作停止如果之前有动作过的仍然保留，后续有动作继续执行
	stop(true) ==.velocity(“stop”, true);  //所有动作停止如果之前有动作过的仍然保留，后续也不执行
	stop(true,true)==velocity(‘finish’,true) //当前瞬间完成，后续不执行
	.velocity(“finish”);  //当前瞬间完成，后续动作继续  (还比jquery多了一个效果)
	//动画后退
	.velocity(“reverse”);


### translate位移

	$element.velocity({ 
		 translateX: "200px", //X轴 位移
	 	translateY: "200px" //Y轴 
	});

### rotate旋转

	$element.velocity({ 
	 	rotateX: "200px", //X轴 位移
	 	rotateY: "200px" //Y轴 
	 	rotateZ: "200px"
	});


	textShadowX, textShadowY, and textShadowBlur
	$element.velocity({ textShadowBlur: "10px" });


### scale拉伸

	$element.velocity({ 
	 	scaleX:'2', //宽为原来2倍
	 	scaleY:'2' //高为原来2倍
	});

### hook设置、获取2D,3D属性值 类似css()


	$.Velocity.hook($('div'),"translateX","200px"); //设置
	$.Velocity.hook($('div'),"translateY","200px"); //设置

	alert( $.Velocity.hook($('div'), "translateX") ) //获取


### 颜色渐变


	$('#ni').velocity({
		  color:'#eff716', //字体颜色渐变
		  backgroundColor:'#000', //背景颜色渐变
		  backgroundColorAlpha: 0.2,//背景颜色透明度渐变
		  colorRed: "50%", //红绿蓝三色中的红渐变，支持百分比渐变
		  colorGreen: "+=150",
		  colorBlue: "+=150",
		  colorAlpha: 0.3 //字体颜色透明度渐变
	 },4000)


### 回调

	$.Velocity.animate($('#ni'), { opacity: 0.5 },3000)
	 	.then(function(elements) { alert($(elements).attr('id')) }) //成功后执行
	 	.catch(function(error) { console.log("Rejected."); }); //失败后执行


### 高级应用

	$('.c').velocity({
	  translateX: function(index, num) { //除了直接设置值，还能添加方法，(索引,总数)类似$.each
	     console.log(total);
	     return (i * 20);
	  }
	 });
	
	效果是批量对class为.c的元素设置translateX值。
	其他属性也可以这么写，要举一反三。





## 参考

http://easings.net/zh-cn

http://julian.com/research/velocity/

http://www.xgllseo.com/?p=3453

http://www.w3ctech.com/topic/1403