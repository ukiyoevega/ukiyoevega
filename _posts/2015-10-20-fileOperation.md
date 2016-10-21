---
layout: post
title: iOS的文件系统和文件读写、压缩操作
tags:
  - iOS
---
###文件系统目录
iOS应用中文件系统的目录长这样:
![](/images/file1.png)
通过NSSearchPathForDirectoriesInDomains(_:_:_:)方法可以获取以上所有文件的目录。

```
//Document
let userPaths = NSSearchPathForDirectoriesInDomains(.documentDirectory, .userDomainMask, true) 
let userPath = userPaths[0]
//Library
let libraryPaths = NSSearchPathForDirectoriesInDomains(.libraryDirectory, .userDomainMask, true)
let libraryPath = libraryPaths[0]
//Caches
let cachepaths = NSSearchPathForDirectoriesInDomains(.cachesDirectory, .userDomainMask, true)
let cachepath = cachepaths[0]
```
也可以通过NSHomeDirectory()直接到达上述目录所在的总目录，再通过手动添加Library子目录。

```
let homeDirectory = URL(string: NSHomeDirectory())
if let homeDirectoryURL = homeDirectory {
	let userPathURL = homeDirectoryURL.appendingPathComponent("Library")
	let userPathString = userPathURL.absoluteString
print(userPathString)
}
```
###Bundle目录
如果要获取应用目录里的文件，也就是和源程序放在一起的文件，应该用path(forResource:ofType:)方法。
![](/images/file2.png)

```
let txtPath = Bundle.main.path(forResource: "rawText", ofType: "txt")
```
###文件的创建、删除、读写操作
##### - 创建文件夹:

```
let desktopDirectory = URL(string: "/Users/vega/Desktop/")
var folderDirectory: String
let fileManager = FileManager.default
if let desktopDirectoryURL = desktopDirectory {
	let folderDirectoryURL = desktopDirectoryURL.appendingPathComponent("Folder")
	folderDirectory = folderDirectoryURL.absoluteString
	do {
		try fileManager.createDirectory(atPath: folderDirectory, withIntermediateDirectories: true, attributes: nil)
	} catch {
		print(error.localizedDescription)
	}
}
```
##### - 创建文件到指定文件夹

```
let txtPathURL = URL(string: folderDirectory)!.appendingPathComponent("test.txt")
let txtPath = txtPathURL.absoluteString
let succeed = fileManager.createFile(atPath: txtPath as String, contents: nil, attributes: nil)
if !succeed { 
	print("fail creating file") 
}
```
##### - 删除文件

```
do {
	try fileManager.removeItem(atPath: txtPath )
} catch {
	print(error.localizedDescription)
}
```

##### - 写入数据到文件

```
1. 非覆盖式写入
let fileHandler = FileHandle(forWritingAtPath: "/Users/vega/Desktop/rawText.txt")!
for i in 1...10 {
	let data = "\(i)非覆盖式写入内容！\n".data(using: String.Encoding.utf8)!
	fileHandler.write(data)
}

```
结果
![](/images/file3.png)

```
//覆盖式写入
let content = "覆盖式写入内容！"
do {
	for _ in 1...10 {
		try content.write(toFile: "/Users/vega/Desktop/text.txt" as String, atomically: true, encoding: String.Encoding.utf8)
	}
} catch {
print(error.localizedDescription)
}
```
结果
![](/images/file4.png)
覆盖式写入用的是字符串方法write(toFile:atomically:encoding:)，即使指定目录不存在可供写入的txt文件，该方法会自动创建一个txt文件供写入。

##### - 读取文件数据
内容如下
![](/images/file5.png)

```
let testPath = "/Users/vega/Desktop/text.txt"
do {
	let content = try NSString(contentsOfFile: testPath as String, encoding: String.Encoding.utf8.rawValue)
	print("succeed reading file content: ")
	print(content)
} catch {
	print(error.localizedDescription)
}
```
读取结果如下
![](/images/file6.png)

##### - 压缩文件
用SSZipArchive对文件进行压缩或者解压的时候，遇到了一个坑...就是只能压缩文件夹，不能对单独的文件进行压缩。如果要批量压缩大量文件，就得把文件一个一个放入文件夹内，再批量压缩文件夹...
![](/images/zip1.png)
先把要压缩的三个txt文件分别放入三个文件夹中
![](/images/zip2.png)
再对三个文件夹进行压缩
![](/images/zip3.png)

```
func moveFile() {
	let fileManager = FileManager.default
	do {
		let txts = try fileManager.contentsOfDirectory(atPath: "/Users/vega/Desktop/txts")
		for i in txts {
			var fullName = i as NSString
			fullName = fullName.substring(to: fullName.length - 4) as NSString
			let txtFileContainer: NSString = "/Users/vega/Desktop/folders/\(fullName)" as NSString
			do {
				_ = try fileManager.createDirectory(atPath: txtFileContainer as String, withIntermediateDirectories: true, attributes: nil)
			} catch { print("fail") }
			let txtFilePath: String = "/Users/vega/Desktop/txts/\(i)"
			do {
				try fileManager.moveItem(atPath: txtFilePath, toPath:txtFileContainer.appendingPathComponent("/\(i)"))
			} catch {print("fail")}
		}
	} catch { print("fail") }
}
func zipTxt() {
	let fileManager = FileManager.default
	do {
		let subFolder = try fileManager.contentsOfDirectory(atPath: "/Users/vega/Desktop/folders")
		for i in subFolder {
			let desPath:NSString = "/Users/vega/Desktop/zipfiles/\(i).zip" as NSString
			let srcPath: String = "/Users/vega/Desktop/folders/\(i)"
			SSZipArchive.createZipFile(atPath: desPath as String, withContentsOfDirectory: srcPath)
		}
	} catch { }
}
```