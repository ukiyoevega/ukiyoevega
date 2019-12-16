---
layout: post
title: Initializer品类鉴赏
tags:
- iOS
---

#几个基础知识
- 在未给成员变量赋初始值的情况下，类需要显示定义构造器，而结构体则不需要，程序会为其自动生成。



```Swift
class SizeClass {
	var width: CGFloat
	var height: CGFloat

	init(width: CGFloat, height: CGFloat) {
	self.width = width
	self.height = height
	}
}

struct SizeStruct {
	var width: CGFloat
	var height: CGFloat
}
let size = SizeStruct(width: 0, height: 0)
```

- 如果要为结构体成员变量指定初值，则应显示定义构造器。

```swift
struct Size {
	var width: CGFloat
	var height: CGFloat

	init(width: CGFloat = 1, height: CGFloat) {
		self.width = width
		self.height = height
	}
}
let size = Size(width: 2, height: 0)
```

- 要想在实例化时不指定参数名，要用下划线(_)来代替外部参数名。

```swift
struct Person {
	var name: String
	init(_ name: String) {
		self.name = name
	}
}
let John = Person("John")
```

---

#类的继承和构造

<table>
<thead>
<tr>
<th>两类构造器</th>
<th>重要性</th>
<th>作用</th>
</tr>
</thead>
<tbody>
<tr>
<td>Designated initializer</td>
<td>必须</td>
<th>调用对应的超类构造器完成超类的构造</th>
</tr>
<tr>
<td>Convenience initializer</td>
<td>非必须</td>
<th>创建某种特殊用途、或者需要键盘输入初始化的类，也可用于给Designated initializer设置默认值。</th>
</tr>
</tbody>
</table>

```swift
class Food {
	var name:  String
	init(name:  String) {
		self.name = name
	}
	//便利构造器用于给Designated initializer设置默认值
	convenience init() {
		self.init(name: "[Unnamed]")
	}
}
```

---

##构造器使用规则

1. Designated initializer必须调用其直属类的构造器。
2. Convenience initializer必须嵌套其它构造器。
3. Convenience initializer最终必须调用到Designated initializer。

<div align=center>
<img src="/images/init1.png" width=“250” height=“122”/>
</div>

---

#二阶初始化

Swift中的类初始化有两个阶段。第一阶段给类中的每个存储属性分配初始值。一旦确定了每个存储属性的初始值，则开始第二阶段：在实例被引用之前再次为每个存储属性进行定制。

好处：保证了初始化的安全性，第一阶段防止属性值在初始化之前被访问，第二阶段防止属性值被另一个构造器意外地设置为其它的值。

##二阶初始化的安检程序：
1. 构造器必须确保其所有成员属性在被发送到超类构造器之前得到初始化。

```Swift
class ViewController: UIViewController {
	var button: UIButton
	
	init() {
		button = UIButton(type: .system)
		super.init(nibName: nil, bundle: nil)
	}
……
```

2. 构造器必须在继承属性得到赋值之前调用超类构造器。 否则子类的构造器指定的值会被超类构造器覆盖重写。

```swift
class view: UIView {
	init() {
	super.init(frame: CGRect.zero)
	self.backgroundColor = UIColor.red
	}

	required init?(coder aDecoder: NSCoder) {
	fatalError("init(coder:) has not been implemented")
	}
}
```

3. 在将值赋给任何属性（包括由同一类定义的属性）之前，便利构造器须调用另一个构造器。否则便利构造器分配的新值将被同类中的其他构造器覆盖重写。

```swift
init() {
	super.init(frame: CGRect.zero)
	self.backgroundColor = UIColor.red
}

convenience init() {
	self.init()
	self.backgroundColor = UIColor.yellow
}
```

4. 构造器在初始化的第一阶段完成之前不能调用任何实例方法读取任何实例属性的值，或将self作为值引用。

---

##构造器的继承和重写规则：

1. 如果没有在子类中定义构造器，子类会自动继承其父类的构造器。
2. 如果子类定义了构造器并且调用了所有父类的构造器，子类会自动继承父类的便利构造器。

```swift
class Food {
	var name:  String
	init(name:  String) {
		self.name = name
	}

	convenience init() {
		self.init(name: "[Unnamed]")
	}
}

class RecipeIngredient: Food {
	var quantity: Int
	init(name: String, quantity: Int) {
		self.quantity = quantity
		super.init(name: name)
	}

	override convenience init(name: String) {
		self.init(name: name, quantity: 1)
	}
}
//子类定义了构造器并且调用了所有父类的构造器
//则子类会自动继承父类的便利构造器
let re = RecipeIngredient() //re.name: [Unnamed]

class ShoppingListItem: RecipeIngredient {
	var purchased = false
	var description: String {
		var output = "\(quantity) x \(name)"
		output += purchased ? " ✔" : " ✘"
		return output
	}
}
//没有在子类中定义构造器
//子类会自动继承其父类的构造器
let sh = ShoppingListItem()
sh.quantity: 1
```

#可失效构造器
类，结构体或枚举都可以定义可无效构造器。
和普通的构造器一样，可失效构造器可以向上传递给超类或横向传递给同类。
有一个比较绕的点，如果某个构造器失效，那么调用了这个构造器的构造器也会失效。
还有一个比较绕的点，如果想要构造器永不失效，就在普通的构造器里调用可失效构造器。

```swift
class Product {
	let name: String
	init?(name: String) {
		if name.isEmpty { return nil }
	self.name = name
	}
}

class CartItem: Product {
	let quantity: Int
	init?(name: String, quantity: Int) {
		if quantity < 1 { return nil }
		self.quantity = quantity
		//init?向上传递给超类
		super.init(name: name)
	}
}
```

在重写可无效构造器时不需要声明其为可选的init?类型。

```swift
class Document {
	var name: String?
    // this initializer creates a document with a nil name value
	init() {}
	// this initializer creates a document with a nonempty name value
	init?(name: String) {
		if name.isEmpty { return nil }
	self.name = name
	}
}

let docu = Document(name: "")
//docu: nil

class AutomaticallyNamedDocument: Document {
	override init() {
	super.init()
	self.name = "[Untitled]"
	}
	//对父类init?的重写
	override init(name: String) {
		super.init()
		if name.isEmpty {
		self.name = "[Untitled]"
		} else {
		self.name = name
		}
	}
}

let auto = AutomaticallyNamedDocument()
//auto.name: [Untitled]
```

#必须构造器
每个子类都必须实现的构造器，要是希望某个构造器被所有子类实现，可以将其定义为required。子类在继承必须构造器时也要定义为required，但无需书写override标识。

