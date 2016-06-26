---
layout: archive
title: iOS delegate实现页面间的交互
---

有两种方法可以实现视图控制器中的数据传输：
1. 从A到B：当视图A打开视图B，A可以给B传送数据。只需要在B的视图控制器中定义一个实例变量， 然后A就可以在B可视化之前给这个属性一个具体值，通常是在prepareForSegue中实现。
2. 从B到A：建立委托关系。

为什么要建立委托(delegate)关系？
当一个场景A(view)衍生出了另一个场景B，通常B都要返回信息到A中或者对A做出某种回应。理想的情况是B在被派生出之后对A不再有依托关系，A中的信息封装在A本身中，B对其一无所知是最好的。要在这种前提下使两者还能进行交互，最好的办法是建立委托关系，使得A成为B的委托。这样一来，A对B就是独立的，只能从B中接受信息。

委托模式常常用于处理以下情况：界面A打开了界面B，界面B需要在其关闭时对A返回数据。
 
在AB两个界面之间建立委托关系需要5步：
1.定义一个委托协议,位置可以是在B界面对应源文件的空白处。

	1	protocol nameOfDelegate:class{  
	2	   func function1(parameters...)  
	3	   func function2(parameters...)  
	4	    ...  
	5	}  



2.给B一个可选类型委托，此变量必为弱类型weak，定义放在B中界面对应的ViewController类里。


	1	weak var delegate:nameOfDelegate?  

为什么委托为可选类型呢？当界面从故事板里加载并且实例化，它还不能立刻得知其委托是谁。在视图控制器加载完成与委托被指派期间，变量值为nil，变量虽在短时间内为nil，但也必须为可选值。 

 3.让B在给定事件响应的时候给其委托A发送讯息。


	1	@IBActionfunc responseFunction1(parameters..){  
	2	    delegate?.function1(parameters..)  
	3	}    
	4	@IBAction func responseFunction2(parameters..){  
	5	    delegate?.function2(parameters..)  
	6	}  
	7	...  


4.让A遵循委托协议，也即把协议名放入其ViewController界面类中并且给出此协议中方法的具体定义。


	1	func responseFunction1(parameters..){  
	2	   puts the definition here..  
	3	}  
	4	func responseFunction2(parameters..){  
	5	   puts the definition here..  
	6	}  


5.让B知道A已经成为其委托了，在A类中的prepareForSegue方法中给出。


	1	overridefunc prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?) {  
	2	   if segue.identifier =="identifierName" {  
	3	      let navigationController = segue.destinationViewControlleras! UINavigationController  
	4	     let controller = navigationController.topViewControlleras! ItemDetailViewController  
	5	      controller.delegate =self  
	6	    ...    
	7	}  



