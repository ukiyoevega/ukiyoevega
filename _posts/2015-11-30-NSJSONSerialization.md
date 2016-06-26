---
layout: archive
title: iOS JSON解析 NSJSONSerialization
---

本文介绍如何将json格式的数据转换为自定义类型或Swift中原生类。

示例代码：
[plain] view plain copy
func loadData(){  
    let url:NSURL = NSURL(string:"http://course.gdou.com/JSONDemo/json/titles.json")!  
    let data:NSData = NSData(contentsOfURL:url)!  
    do{  
        let arr:[Dictionary<String,String>]? = try NSJSONSerialization.JSONObjectWithData(data, options: NSJSONReadingOptions.MutableContainers) as? [Dictionary<String,String>]  
        //将对象中的数据存放到newsItem类类型的items数组中  
        for i in arr!{  
        let item = newsItem(title: i["title"] as String!, pubDate: i["pubDate"] as String!, link: i["link"] as String!, imageURL: i["image"] as String!)  
        items.append(item)  
        }  
    }  
    //当抛出NSError类型的错误时将该错误值输出  
    catch let error as NSError?{print("error=\(error?.description)")}  
}  


1. 初始化NSData
convenience
init?(string URLString:
String)
该方法返回NSURL类型的对象，在此用来初始化存放JSON数据的url地址


init?(contentsOfURL url: NSURL)
该方法返回返回由url地址定位的数据初始化成的NSData类型对象。

2. 创建JSON对象
class func JSONObjectWithData(_ data: NSData,options opt: NSJSONReadingOptions)throws -> AnyObject
该方法返回json数据类型的基础库对象(Foundation Objects)，发生错误时抛出NSError类型的错误并返回nil值

struct NSJSONReadingOptions : OptionSetType {
    init(rawValue rawValue: UInt)
    static var MutableContainers: NSJSONReadingOptions { get }
    static var MutableLeaves: NSJSONReadingOptions { get }
    static var AllowFragments: NSJSONReadingOptions { get }
}
创建JSON对象时方法JSONObjectWithData用到的参数选项，其中MutableContainers指定数组和字典被初始化为可变对象(mutable objects)。

3. 将JSON对象中键对应的值数据存放到自定义类型或Swift原生类型的实体中。

参考资料：官方文档
https://developer.apple.com/library/ios/documentation/Foundation/Reference/NSJSONSerialization_Class/index.html#//apple_ref/swift/struct/c:@E@NSJSONReadingOptions


