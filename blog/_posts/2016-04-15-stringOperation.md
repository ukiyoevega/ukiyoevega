---
layout: post
title: 利用Swift对字符串数据进行格式化
tags:
  - iOS
---

最近从网上爬了一些比较非格式化的文本数据，试了下用Swift实现整理数据的格式。
 
获取到的数据格式第一类是这样的......每行开头和结尾有若干占位符或者空格，并且行与行之间有数目不等的空行。
于是想到了这样的方法: 

- 用NSString里的components(separatedBy:)方法将文本按行遍历一遍
- 再通过NSString的trimmingCharacters(in:)方法对每行首尾的空格删除
- 判断进行空格去除后的行长度是否为0，为0代表这行本来就是空的，跳过，非0则输出去空格后的各行

输入
![](/images/screenshot1.png)

```
func wipeOffWhiteSpace() {
	let htmlPath = "/Users/vega/Desktop/rawText.txt"
	do {
		let htmlData = try NSString(contentsOfFile: htmlPath, encoding: String.Encoding.utf8.rawValue)
		let allLines = htmlData.components(separatedBy: NSCharacterSet.newlines)
		for eachLine in allLines {
			if ((eachLine as NSString).trimmingCharacters(in: NSCharacterSet.whitespaces) as NSString).length > 0 {
				print(eachLine.trimmingCharacters(in: NSCharacterSet.whitespaces))
			}
		}
	}
	catch {
		let error = error
		print(error.localizedDescription)
	}
}
```
输出
![](/images/screenshot2.png)

第二类数据就是一堆乱七八糟的html源码，要从一堆标签里提取某个类型的字符串。由于html有一定的格式，只要找到所需数据在哪个标签里就好了。
解决方法如下：

- 和上面的方法类似，先用components(separatedBy:)把一大堆数据分行，以便于进行单独处理
- 观察所需要的数据在什么标签里，它们左右有什么特点格式的字符串，记左边的为leftString，右边的为rightString
- 比如我想从下面的数据中获取常见症状，那我要的就是`<li><a href="/25_gaishu.html" target="_blank" title="拔牙后出血不止">拔牙后出血不...</a></li>`里面title的内容，所以leftString是`title="`，rightString是`">`。
- 对每行数据进行扫描输出
- 

输入
![](/images/screenshot3.png)

```
    func getString(from line: NSString, between leftString: NSString, and rightString: NSString) -> NSString? {
	var left:NSInteger, right:NSInteger
	var foundData: NSString
	let length = leftString.length
	let scanner = Scanner(string: line as String)
	scanner.scanUpTo(leftString as String, into: nil)
	left = scanner.scanLocation
	if !scanner.isAtEnd {
                scanner.scanLocation = left + length
	scanner.scanUpTo(rightString as String, into: nil)
	right = scanner.scanLocation + 1
	left += length
	foundData = line.substring(with: NSMakeRange(left, right - left - 1)) as NSString
	return foundData
	}
	return nil
}
func getNeeded() {
	let htmlPath = "/Users/vega/Desktop/rawText.txt"
	do {
		let htmlData = try NSString(contentsOfFile: htmlPath, encoding: String.Encoding.utf8.rawValue)
		let allLines = htmlData.components(separatedBy: NSCharacterSet.newlines)
		for eachLine in allLines {
		let data = getString(from: eachLine as NSString, between: "title=\"", and: "\">")
		if let string = data {
			print(string)
			}
		}
	}
	catch {
	let error = error
	print(error.localizedDescription)
	}
}
```
输出
![](/images/screenshot4.png)

