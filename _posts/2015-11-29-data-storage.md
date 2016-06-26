---
layout: archive
title: iOS 数据存取方法 对象归档
---

一个NSArchiver对象可以把归档数据或者一个由编码人员提供的可变数据对象(NSMutableData类的实例)转化为流并写入到文件中。
键控档案(Keyed archives)由NSKeyedArchiver创建，NSKeyedUnarchiver译解。键控档案中的每个值在被编码时都给定了一个名字，或者键。当解码时，值按照其名字来解码。
NSKeyedArchiver和NSKeyedUnarchiver为NSCoder的子类，前者把对象或者标量编码成具有独立架构、能被存入文件的格式。当一组对象被归档，每个对象类的数据和实例变量都被i 写入到档案中。后者从档案中解码数据并创建一组与原来对象等价的对象。

存储数据的步骤：
1. 获取文件路径，在对应的数据模型文件中加入存储数据的方法。

[plain] view plain copy

	1	funcdocumentsDirectory() ->String{  
	2	   let paths =NSSearchPathForDirectoriesInDomains(.DocumentDirectory, .UserDomainMask, true  
	3	   return paths[0]  
	4	}  
	5	func dataFilePath()->String {  
	6	   return (documentsDirectory()asNSString).stringByAppendingPathComponent("FileName.plist")  
	7	}  
	8	func saveMyData(){  
	9	   let data =NSMutableData  
	10	   let archiver =NSKeyedArchiver(forWritingWithMutableData: data)  
	11	   archiver.encodeObject(objectNeedToBeSave, forKey:"keyForObject")  
	12	   archiver.finishEncoding()  
	13	   data.writeToFile(dataFilePath(), atomically: true)  
	14	}  

2. 在数据被更新／更改的地方调用saveMyData()方法。
3. 使数据类遵循NSCoding协议，并在数据类中增加协议中的方法。
NSCoding协议中包含两个方法：encodeWithCoder(aCoder)和init?(coder)。

[plain] view plain copy

	1	funcencodeWithCoder(aCoder:NSCoder){  
	2	    aCoder.encodeObject(varietyName1, forKey:"keyOfVariable1")  
	3	    aCoder.encodeBool(varietyName2, forKey:"keyOfVariable2")  
	4	}  
	5	requiredinit?(coder aDecoder:NSCoder){  
	6	    super.init()  
	7	}  
	8	overrideinit() {  
	9	    super.init()  
	10	}  

读取数据的步骤：
1. 完善数据对象中init?(coder)方法。
该方法可用于提取文件中的对象。


[plain] view plain copy

	1	requiredinit?(coder aDecoder:NSCoder){  
	2	   nameOfVaribale1 = aDecoder.decodeObjectForKey("keyOfVariable1")as! String  
	3	   nameOfVariable2 = aDecoder.decodeObjectForKey("keyOfVariable2")as! String  
	4	   super.init()  
	5	}  

2. 在对应的数据模型文件中加入读取数据的方法。


[plain] view plain copy

	1	func loadMyData() {  
	2	   let path = dataFilePath()  
	3	   ifNSFileManager.defaultManager().fileExistsAtPath(path) {  
	4	      if let data = NSData(contentsOfFile: path) {  
	5	          let unarchiver =NSKeyedUnarchiver(forReadingWithData:data)  
	6	           items = unarchiver.decodeObjectForKey("nameOfObjects")as! [ObjectName]  
	7	           unarchiver.finishDecoding()  
	8	       }  
	9	    }  
	10	}  

NSData属性可以用来描述任何类型的数据。
用NSData类型描述的自定义类对象数据，需要将自定义类对象序列化成NSData对象，所以类必须符合NSCoding协议。

将类对象obj 序列化为 NSData 数据的方法:

[plain] view plain copy

	1	<span style="font-family:SimSun;"></span>  
[plain] view plain copy

	1	var data:NSData = NSKeyedArchiver.archivedDataWithRootObject(obj)  

将 NSData 数据反序列化为类对象的方法:

[plain] view plain copy

	1	</pre><pre name="code" class="plain">      var obj:MyClass = NSKeyedUnarchiver.unarchiveObjectWithData(data)  


3. 重写主文件中的init?(coder)方法


[plain] view plain copy

	1	requiredinit?(coder aDecoder:NSCoder) {  
	2	    Objects= [ObjectType]()  
	3	   super.init(coder: aDecoder)  
	4	   loadData()  
	5	}  

该方法在调用时遵循了如下的模式：
1. 确保实例变量Objects有合适的初值（一个新的数组）。
2. 调用父类的init()方法，调用super.init(coder)方法确保视图控制器剩余的部分都被恰当地读取了。
3. 可以调用该方法中的其他方法了，这里调用的是loadData()方法来加载plist文件中的数据。

