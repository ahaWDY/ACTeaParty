## 1. 用户线程和内核线程的区别

1. 内核级线程:切换由内核控制，当线程进行切换的时候，由用户态转化为内核态。切换完毕要从内核态返回用户态；可以很好的利用smp，即利用多核cpu。windows线程就是这样的。

2. 用户级线程内核的切换由用户态程序自己控制内核切换,不需要内核干涉，少了进出内核态的消耗，但不能很好的利用多核Cpu,目前Linux pthread大体是这么做的。

以下是用户级线程和内核级线程的区别：

（**1**）内核支持线程是OS内核可感知的，而用户级线程是OS内核不可感知的。

（**2**）用户级线程的创建、撤消和调度不需要OS内核的支持，是在语言（如Java)这一级处理的；而内核支持线程的创建、撤消和调度都需OS内核提供支持，而且与进程的创建、撤消和调度大体是相同的。

（**3**）用户级线程执行系统调用指令时将导致其所属进程被中断，而内核支持线程执行系统调用指令时，只导致该线程被中断。

（**4**）在只有用户级线程的系统内，CPU调度还是以进程为单位，处于运行状态的进程中的多个线程，由用户程序控制线程的轮换运行；在有内核支持线程的系统内，CPU调度则以线程为单位，由OS的线程调度程序负责线程的调度。

（**5**）用户级线程的程序实体是运行在用户态下的程序，而内核支持线程的程序实体则是可以运行在任何状态下的程序。

Reference: https://www.jianshu.com/p/47d949d0898a



## 2. onload、DOMReady 的区别？

### Dom文档加载的步骤：

1. 解析html结构；
2. 加载外部脚本和样式表文件；
3. 解析并执行脚本；
4. dom树构建完成（DOMContentLoaded）；
5. 加载图片等外部文件；
6. 页面加载完毕。

### DOM ready、DOM Load、图片Load

DOM ready：（也叫DOMContentLoaded ），在第4步完成后触发；
 图片onload：是在第5步完成后触发；
 页面onload：是第6步完成后触发。

由此可见三者执行顺序为：**domready→图片load→页面load。**

DOMContentLoaded与Load事件触发的时机不一样，先触发DOMContentLoaded事件，后触发load事件。
 如果外部脚本中，出现defer延迟脚本或async异步脚本，这两种脚本有可能在DOMContentLoaded事件之前或之后执行。

### domready和onload事件区别：

前者：在DOM文档结构准备完毕后就可以对DOM进行操作；
 后者：当页面完全加载后（整个document文档包括图片、javascript文件、CSS文件等外部资源)，就会触发window上面的load事件。

Reference: https://www.jianshu.com/p/6b0a95cdbc7a



## 3. 双飞翼布局具体如何实现？
双飞翼布局，就是两端固定宽高，中间自适应的三栏布局

### 方式一：通过flex弹性布局来实现

```html
//HTML结构，div2是中间的自适应区域
...
<body>
    <div class="wrap">
        <div class="div1"></div>  
        <div class="div2"></div>
        <div class="div3"></div>
    </div>
</body>
...
```

```css
*{  //先简单粗暴的解决一下浏览器的默认样式  
    margin: 0;
    padding: 0;
    border: 0;
    box-sizing:border-box;   //使用border-box，盒模型好计算，妈妈再也不用担心我算不清块宽高了
}
.wrap{
    width: 100%;
    height: 100%;
    display: flex;     //使用弹性布局
    flex-flow:row nowrap;  //以沿主轴方向行显示，不换行，从而来显示3个块
    justify-content:space-around;  //这一个加和不叫其实也没事，加上去的意思就是两端对齐
}

[class^='div']{  // 给所有的div都加上高和边框样式，方便观看，不然都缩成一条线了
    height: 400px;
    border: 1px solid #f00;
}

.div1,.div3{  //给两端的div固定的宽
    width: 200px;
    background-color: #ccc;
    flex-shrink: 1; //默认是1，所以不用写也没事，写出来自是表达这个意思
}
.div2{
    background-color: #0f0;
    flex-grow:1;  //这个比较重要，作用是让第二个块的宽度撑满剩余的空间
}
```

### 方式二：通过定位来实现

HTML结构不变，样式改变

```css
.wrap{
    width: 100%;  //同样实现宽高100%铺开
    height: 100%;
    position: relative;  //父层添加相对定位，让子元素相对父层来定位
}
[class^='div']{
    height: 400px;
    border: 1px solid #f00;
}
.div1,.div3{
    position: absolute;
    width: 200px;
    background-color: #ccc;
}
.div1{
    left: 0;  //固定在父层的左侧
    top: 0;
}
.div3{
    right: 0;  //固定在父层的右侧
    top: 0;
}
.div2{
    background-color: #0f0;
    /*这个是关键，我们没有给中间的div2添加过宽属性，所以默认占用父层宽的100%，
     由于两侧块宽是固定的，所以中间的自适应块左右分别200px的外边距中间的content区域就会实现自适应*/
    margin: 0 200px;  
}
```



## 4. Vuex整个触发过程（actions，state，view）

## 5. 微信扫一扫二维码网页上登陆前后端过程？

