---
layout: archive
title: Swift C++ 对比
---

在2014年苹果WWDC上，副总裁Craig宣称要把swift升级成为一款扔掉C语言包袱的Objective-C语言。从OC的名字可以看出，OC是一款基于C构建的语言，那么Swift作为OC的升级，更是摒弃了很多C语言的繁杂特性。
    
文件的合并
学过C语言的都知道，一个用C++的项目必然含有header(.h)头文件和cpp源文件，而Swift在这方面做了改良，把沿袭自C中的.h和.m合并成了两个文件，使项目变得清爽并减少了维护人员的工作量。
    
语言的简洁性
来看下面一段代码

	1	let individualScores = [75, 43, 103, 87, 12]   
	2	var teamScore = 0  
	3	for score in individualScores {  
	4	if score > 50 { teamScore += 3 }   
	5	else { teamScore += 1}  
	6	    }  
	7	print("team score: \(teamScore)")  




不管是否学习过编程语言，swif中t约定俗成的命名方式，元素的命名尽量详细并以完整的英文单词或短语为元素名，使得swift代码更像是一种伪代码，便于理解。


注意到三点
一、swift不像传统的C语言一样需要在所有语句结束后强制加上分号。
二、 if语句后条件不再需要用括弧包裹。
三、输出语句交替输出元素和字符串时，只需要在”“中将非字符元素用\()包裹就能方便的将元素插入到要在屏幕上显示的语句中。


以上三个例子都是swift语言简洁，易于阅读的表现，更多swift语言的优点读者可以在实践中体会到。


nil值的出现和改良
C语言最令学习者头疼的知识点之一是指针，指针的使用不当会给程序带来很大的潜在危害。Objective-C语言对指针做出了改进，添加了nil这一定义。nil为指向一个对象的空指针，它的出现避免了当一个未被初始化的指针被调用时程序崩溃。到swift之后，随着可选值的出现，nil有了更直观的定义，它表示一个值是没有被分配内存，不存在的。现在，当nil可选值被调用时会出现编译错误，让用户知道调用的值并不存在。swift限制指针的使用体现了其类型安全性。


集合类型的改变
C++中，字符串，数组以引用类型存在，而Swift中的Array，String，Dictionary皆为值类型，是通过Struct实现的。这样可以使得赋值或者参数的传递更为安全，不用担心被赋值的变量被篡改。

	1	var someInts = [Int]()  
	2	print(“someInts is of type Int with \(someInts.count) item”)  


此时someInts.count = 0, 说明Swift中数组未被赋值的时候为空，系统不为数组分配存储空间。这在C++中是不允许的。


在C++语言中，如下定义一个一维数组不赋值，直接输出，得到数组的长度和定义的相同，说明系统为数组分配了存储空间。
但数组元素在没被赋值的情况下没有被系统默认赋值为0或者定义为空数组，输出的并不是我们想要的结果。


	1	int a[10];  
	2	int length=sizeof(a)/sizeof(a[0]);  
	3	cout<<length<<endl;  
	4	for(int i=0;i<10;i++)  
	5	cout<<a[i]<<" ";  



输出结果：
10
134519368 134514292 134514096 134519565 134519416 134513973 -1216595168 -1216616832 134519368 134514626 


	1	var a1 = [1,2,3], a2 = [4,5,6]  
	2	var a3 = a1 + a2  
Swift中的数组可以通过两个数组相加创建一个数组。
可以使用加法操作符 "+" 来组合两种已存在的相同类型数组。//原理？


控制流的优化

	1	//C++  
	2	if(a>b) cout<<"a is bigger than b";  
	3	//Swift  
	4	if a>b {print("a is bigger than b");}  
Swift中if语句里的条件不需要用( )括起，c++则强制使用。
Swift中执行代码必须用{}，C++中如果后面执行的语句只有一条，则可以省略{}。


c++if语句后的条件可以隐式的与0比较，swift不能必须为bool类型的条件。


	1	//C++  
	2	switch(i/10){  
	3	  case 1:cout<<"i>=10";break;  
	4	  case 2:cout<<"i>=20";break;  
	5	  case 3:cout<<"i>=30";break;  
	6	  case 4:  
	7	  case 5:  
	8	  case 6:cout<<"i>=40";break;  
	9	}  
	10	//Swift  
	11	switch i/10 {  
	12	  case 1:print("i>=10");  
	13	  case 2:print("i>=20");  
	14	  case 3:print("i>=30");  
	15	  default:print("i>=40");  
	16	  }  


C++中的switch语句需要在每个case后写break，使程序跳出switch执行switch以后的语句。


Swift语言在符合条件后的case后的语句执行完毕后会自动跳出switch，使得代码更简洁人性化。
Swift中，当不同的值进行统一处理的时候，使⽤用逗号将值隔开即可，不需要多写case。C++则必须在每个值前写case。


	1	//C++  
	2	for(i=0;i<=10;i++){  
	3	  cout<<i<<" ";}  
	4	//Swift  
	5	for i=1...5{  
	6	  print("\(i) ");}  
Swift中的for循环语句增加了范围操作，看以上的对比，Swift语句的for循环表达流畅，更易于理解。


再看如下代码段

	1	//Swift  
	2	var ShoppingList = ["Milk","RedDate","Cigarettes","PaperTowel"]  
	3	for Item in ShoppingList {  
	4	  print(Item)  
	5	  }  
在此段代码中没有出现var Item:String语句，但是经过编译代码仍然能够执行，这是因为for-in语句将自动把ShoppingList中的元素一个个赋值给item ，然后执行print输出语句，当ShoppingList中的元素被遍历并皆赋值给Item变量以后，for-in语句执行结束。



函数定义和解读的不同
和C++不同，Swift在定义函数参数时，可以为其分配两个参数名，分别为外部参数名和内部参数名，外部参数名用来标记传递给函数调用的参数，本地参数名在实现函数的时候使用。

给函数的外部参数名命名时，在外部参数名前加上适当的介词如”at”, “with”, “for”能让函数名变得灵活不死板。例如：

	1	configureCheckmarkForCell(someCell, atIndexPath: someIndexPath)  
	2	override func tableView(tableView: UITableView, numberOfRowInSection section:Int) -> Int {  
	3	…}  
	4	override func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {  
	5	…}  
	6	override func tableView(tableView: UITableView, didSelectRowAtIndexPath ) {  
	7	…}  



上面三个函数乍看有着相同的函数名字tableView()，因为在一般的编程语言中函数名为函数定义时括号前面的字符串，但在swift中，函数的名字还包括了参数。也就是说，上面三个函数的正式函数名分别应该为：

	1	override func tableView(numberOfRowInSection)  
	2	override func tableView(cellForRowAtIndexPath）  
	3	override func tableView(didSelectRowAtIndexPath)  
再如上文的configureCheckmarkForCell(someCell, atIndexPath: someIndexPath)函数，此时它的完整的函数名变为configureCheckmarkForCell(atIndexPath)。
总结：当函数有多个参数时，这种方法可以使得函数能用一句话表达清楚出它的明确意图，便于理解。

参考书目：
1. The Swift Programming Language 中文版
2. iOS Apprentice(Fourth Edition) 


