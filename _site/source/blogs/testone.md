# iOS(OC)面试题目整理 -- HonBoom

## 一,自动释放池的内部原理
	答: autorelease实际上只是把对release的调用延迟了,对于每一个autorelease,系统只是把该 Object放入了当前的autorelease pool中,当该pool被释放时,该pool中的所有Object会被调用release。

## 二,int a = 5,int b = 10,(要求在不不设定第三变量的情况下交换两个变量的值)
	答: a = b - a
	    b = b - a
	    a = b + a
        
## 三,Xcode中的目录结构是怎么分组的?
	答:一级目录按模块分,二级目录按模块内的功能分为Model,View,Controller;工具类可单独分为一个category目录
	
## 四,什么时候用delegate,什么时候用Notification?
	答:delegate针对one-to-one的关系,并且receiver可以返回值给sender;
		notification针对one-to-one/many/none,receiver无法返回值给sender.所以,delegate用于sender希望接收到receiver的某个功能反馈值,notification用于通知多个object某个事件.
        
## 五,什么是KVC和KVO?
	答:KVC(key-Value-Coding:键值编码)内部的实现:一个对象在调用setValue的时候,(1)首先根据方法名找到运行方法的时候所需要的环境参数.(2)他会从自己isa指针结合环境参数,找到具体的方法实现的接口.(3)再直接查找得来的具体是方法实现.
	KVO(key-Value_Observing:键值观察者):当观察者为一个对象的属性进行了注册,被观察对象的isa指针被修改的时候,isa指针就会指向一个中间类,而不是真实的类.所以isa指针其实不需要指向实例对象真实的类.所以我们的程序最好不要依赖于isa指针.在调用类的方法的时候,最好明确对象实例的类名.

## 六,请简述堆和栈的区别.
	答:<1>从管理方式来讲:对于栈来说,是由编译器自动管理,无需手动控制;对于堆来说,释放工作由程序员来控制,容易产生内存泄露(memory leak).<2>从申请大小方面讲:栈空间比较小;堆空间比较大.<3>从数据存储方面讲:栈空间一般存放基本类型,对象的地址;堆一般存放对象本身,block的copy
	
## 七,简述ARC的原理
    答:通过编译器在编译的时候,插入类似内存管理的代码来帮助开发者管理内存.
	
## 八,深复制和浅复制的区别.
	答:深复制是对内容进行拷贝;浅复制是对指针进行拷贝.
	
## 九,分类(category)和类扩展(extension)的区别.
	答:分类有名字,类扩展没有名字,是一种特殊的分类;
	分类只能扩展方法(属性只能是声明,并没有真正实现),类扩展可以扩展属性,成员变量和方法.

## 十,OC有私有方法吗?私有变量呢?
	答:1>OC没有类似@private 的修饰词来修饰方法,只要写在.h中,就是公共方法
	2>可以直接写在.m文件中(比如类扩展)声明和实现方法,对编译器来说是私有的.
	
## 十一,NSArray  和  NSSet有何区别
	答:NSArray : 内部数据结构是数组(有顺序的);NSSet : 内部数据结构是链表(无顺序的)

## 十二,#import 和 #include的区别是\? 
	答:* #import与#include的类似，都是把其后面的文件拷贝到该指令所在的地方
	* #import可以自动防止重复导入
	* #import <> 用于包含系统文件
	* #import用于包含本项目中的文件
	* #import , 告诉编译器找到并处理名为Foundation.h文件,这是一个系统文件,#import表示将该文件的信息导入到程序中
	

## 十三,AFN 和 SDWebImage 底层实现
	答:
	AFNetWorking原理: ?
    SDWebimage底层:SDWebImage中为UIImageView提供了一个分类UIImageView+WebCache.h,这个分类中有一个最常用的接口sd_setImageWithURL:placeholderImage:,会在真实图片出现前先显示占位图片,当真实图片加载出来后会替换占位图片
        ※加载图片的过程大致如下:
        <1>首先会在SDWebImageCache中寻找图片是否有对应的缓存,它会以url作为数据的索引先在内存中寻找是否有对应的缓存
        <2>如果缓存未找到就会利用通过MD5处理过的key来继续在磁盘中查询对应的数据,如果找到了,磁盘中的数据加载到内存中,并将图片显示出来.
        <3>如果内存和磁盘中都没有找到,就会向远程服务器发送请求,开始下载图片.
        <4>下载后的图片会加入缓存中,并写入磁盘中.
        <5>整个获取图片的过程都是在子线程中执行,获取到图片后回到主线程讲图片显示出来.
        

## 十四,UIView和CALayer的关系
	答;
	※UIView显示在屏幕上归功于CALayer,通过调用drawRect方法来渲染自身的内容,调节CALayer属性可以调整UIView的外观,UIView继承自UIResponder,比起CALayer可以响应用户数事件,Xcode6之后可以方便的通过视图调试功能查看图层之间的关系.※UIView是iOS系统中界面元素的基础,所有的界面元素都继承自它.它内部是由Core Animation来实现的,它真正的绘图的部分,是由一个叫CALayer(Core Animation Layer)的类来管理.UIView本身,更像是一个CALayer的管理器,访问它的跟绘图和坐标有关的属性,如frame,bounds等,实际上内部都是访问它所在的CALayer属性.
	※UIVIew有个Layer属性,可以返回它的主CALayer实例,UIView有一个LayerClass方法,返回主layer所使用的类,UIVIew的子类,可以通过重载这个方法,来让UIView使用不同的CALayer来显示,如
    ※UIVIew的CALayer类似UIVIew的子View树形结构,也可以向它的Layer上添加子layer,来完成某些特殊的显示.例如下面的代码会在目标View上敷上一层黑色的透明薄膜.
    ※补充部分，这部分有深度了，大致了解一下吧，UIView的layer树形在系统内部被系统维护着三份copy
        * 逻辑树，就是代码里可以操纵的，例如更改layer的属性等等就在这一份
        * 动画树，这是一个中间层，系统正是在这一层上更改属性，进行各种渲染操作
        * 显示树，这棵树的内容是当前正被显示在屏幕上的内容
        * 这三棵树的逻辑结构都是一样的，区别只有各自的属性
    
## 十五,UIViewController整个生命周期自动调用的五个方法是什么?先后顺序?
	答:其实问的就是View的生命周期:
    
## 十六,const 的使用?
	答:如果const写在指针类型的左边, 那么意味着指向的内存空间中的值不能改变, 但是指针的指向可以改变
    如果const写在指针的数据类型和*号之间, 那么意味着指向的内存空间中的值不能改变, 但是指针的指向可以改变
    如果const写在指针的右边(数据类型 * const), 那么意味着指针的指向不可以改变, 但是指针指向的存储空间中的值可以改变
    
    规律:
    如果const写在指针变量名的旁边, 那么指针的指向不能变, 而指向的内存空间的值可以变
    如果const写在数据类型的左边或者右边, 那么指针的指向可以改变, 但是指向的内存空间的值不能改变
    
## 十七,什么是block,请简述.
	答:Block是iOS中一种比较特殊的数据类型,用来保存某一段代码

[Back](..)