## JS 实现瀑布流的实现

#### 实现自定义的样式
```
<body>
<div id="main">
    <div class="pin">
        <div class="box">
            <img src="./images/1.jpg"/>
        </div>
    </div>
    <div class="pin">
        <div class="box">
            <img src="./images/2.jpg"/>
        </div>
    </div>
    <div class="pin">
        <div class="box">
            <img src="./images/3.jpg"/>
        </div>
    </div>
    <div class="pin">
        <div class="box">
            <img src="./images/4.jpg"/>
        </div>
    </div>
    <div class="pin">
        <div class="box">
            <img src="./images/5.jpg"/>
        </div>
    </div>
    <div class="pin">
        <div class="box">
            <img src="./images/6.jpg"/>
        </div>
    </div>
    <div class="pin">
        <div class="box">
            <img src="./images/7.jpg"/>
        </div>
    </div>
    <div class="pin">
        <div class="box">
            <img src="./images/8.jpg"/>
        </div>
    </div>
    <div class="pin">
        <div class="box">
            <img src="./images/9.jpg"/>
        </div>
    </div>
    <div class="pin">
        <div class="box">
            <img src="./images/10.jpg"/>
        </div>
    </div>
    <div class="pin">
        <div class="box">
            <img src="./images/11.jpg"/>
        </div>
    </div>
    <div class="pin">
        <div class="box">
            <img src="./images/12.jpg"/>
        </div>
    </div>
    <div class="pin">
        <div class="box">
            <img src="./images/13.jpg"/>
        </div>
    </div>
    <div class="pin">
        <div class="box">
            <img src="./images/14.jpg"/>
        </div>
    </div>
    <div class="pin">
        <div class="box">
            <img src="./images/15.jpg"/>
        </div>
    </div>
    <div class="pin">
        <div class="box">
            <img src="./images/16.jpg"/>
        </div>
    </div>
    <div class="pin">
        <div class="box">
            <img src="./images/17.jpg"/>
        </div>
    </div>
    <div class="pin">
        <div class="box">
            <img src="./images/18.jpg"/>
        </div>
    </div>
    <div class="pin">
        <div class="box">
            <img src="./images/19.jpg"/>
        </div>
    </div>
    <div class="pin">
        <div class="box">
            <img src="./images/20.jpg"/>
        </div>
    </div>
    <div class="pin">
        <div class="box">
            <img src="./images/21.jpg"/>
        </div>
    </div>
</div>
```

#### 终点自定义JS逻辑
通过父级对象变量图片子级div

```
function waterfall(parent,pin){
    var oParent=document.getElementById(parent);// 父级对象
    var aPin=getClassObj(oParent,pin);// 获取存储块框pin的数组aPin
    var iPinW=aPin[0].offsetWidth;// 一个块框pin的宽
    var num=Math.floor(document.documentElement.clientWidth/iPinW);//每行中能容纳的pin个数【窗口宽度除以一个块框宽度】
    oParent.style.cssText='width:'+iPinW*num+'px;margin:0 auto;';//设置父级居中样式：定宽+自动水平外边距

    var pinHArr=[];//用于存储 每列中的所有块框相加的高度。a
    for(var i=0;i<aPin.length;i++){//遍历数组aPin的每个块框元素
        var pinH=aPin[i].offsetHeight;
        if(i<num){
            pinHArr[i]=pinH; //第一行中的num个块框pin 先添加进数组pinHArr
        }else{
            var minH=Math.min.apply(null,pinHArr);//数组pinHArr中的最小值minH
            var minHIndex=getminHIndex(pinHArr,minH);
            aPin[i].style.position='absolute';//设置绝对位移
            aPin[i].style.top=minH+'px';
            aPin[i].style.left=aPin[minHIndex].offsetLeft+'px';
            //数组 最小高元素的高 + 添加上的aPin[i]块框高
            pinHArr[minHIndex]+=aPin[i].offsetHeight;//更新添加了块框后的列高
        }
    }
}
```

通过父级和子元素的class类 获取该同类子元素的数组

```
function getClassObj(parent,className){
    var obj=parent.getElementsByTagName('*');//获取 父级的所有子集
    var pinS=[];//创建一个数组 用于收集子元素
    for (var i=0;i<obj.length;i++) {//遍历子元素、判断类别、压入数组
        if (obj[i].className==className){
            pinS.push(obj[i]);
        }
    };
    return pinS;
}
```

在window.onload方法中调用以上主要的逻辑方法

```
window.onload=function(){

    waterfall('main','pin');

    var dataInt={'data':[{'src':'1.jpg'},{'src':'2.jpg'},{'src':'3.jpg'},{'src':'4.jpg'}]};

    window.onscroll=function(){
        if(checkscrollside()){
            var oParent = document.getElementById('main');// 父级对象
            for(var i=0;i<dataInt.data.length;i++){
                var oPin=document.createElement('div'); //添加 元素节点
                oPin.className='pin';                   //添加 类名 name属性
                oParent.appendChild(oPin);              //添加 子节点
                var oBox=document.createElement('div');
                oBox.className='box';
                oPin.appendChild(oBox);
                var oImg=document.createElement('img');
                oImg.src='./images/'+dataInt.data[i].src;
                oBox.appendChild(oImg);
            }
            waterfall('main','pin');
        };
    }
}

```
